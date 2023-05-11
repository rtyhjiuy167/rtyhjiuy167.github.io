## 一、快速开始

使用SpringBoot进行项目搭建。

在SpringBoot项目的基础上引入如下依赖即可：

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

默认用户名为：user

密码会随机生成，可在控制台出查看

{% post_link java/spring-security/快速开始配置 '详细配置' %}

## 二、功能使用

### （一）自定义密码

#### 1.配置文件

在`application.yml`中配置`spring. security.user.name`与`spring. security.userpassword`属性。

#### 2.自定义配置数据源

通过继承父类`WebSecurityConfigurerAdapter`的spring-security配置类中重写`configure(AuthenticationManagerBuilder builder)`方法的形式来自定义配置数据源。通过`AuthenticationManagerBuilder`类中的`userDetailsService`方法来设置数据源。

##### （1）InMemoryUserDetailsManager

通过创建`InMemoryUserDetailsManager`对象并设置相应的属性值。

{% post_link java/spring-security/自定义密码1配置 'Web详细配置' %}

##### （2）存放数据库

账户信息的实体类需要实现`UserDetails`接口，并重写`getAuthorities()`、`getPassword()`、`etUsername()`、`isAccountNonExpired()`、`isAccountNonLocked()`、`isCredentialsNonExpired()`、`isEnabled()`方法，分别表示获取账户权限/角色字段、获取密码、获取用户名、账户是否过期、账户是否过期、账户是否锁定、账户是否激活。

{% post_link java/spring-security/自定义密码2配置 '前后端分离-自定义密码配置' %}

### （二）验证码

{% post_link java/spring-security/验证码配置 '前后端分离-验证码配置' %}

### （三）会话管理

```java
http.sessionManagement() // 开启会话管理
    .maximumSessions(1) // 设置最大并发数
    .maxSessionsPreventsLogin(true)// 设置登录后不允许在其它设备上登录
    .expiredSessionStrategy(event -> {
        HttpServletResponse response = event.getResponse();
        Map<String, Object> result = new HashMap<>();
        result.put("status", 500);
        result.put("msg", "当前会话已经失效，请重新登录");
        response.setContentType("application/json;charset=UTF-8");
        String s = new ObjectMapper().writeValueAsString(result);
        response.getWriter().println(s);
        response.flushBuffer();
    });
```

### （四）注销

```java
http.logout().logoutUrl("/logout").logoutSuccessHandler((request, response, authentication) -> {
    Map<String, Object> result = new HashMap<>();
    result.put("msg", "注销成功");
    result.put("用户信息", authentication.getPrincipal());
    response.setStatus(HttpStatus.OK.value());
    response.setContentType("application/json;charset=UTF-8");
    String s = new ObjectMapper().writeValueAsString(result);
    response.getWriter().println(s);
});
```

### （五）记住密码

继承`ersistentTokenBasedRememberMeServices`类，并重写`ersistentTokenBasedRememberMeServices`类的父类`AbstractRememberMeServices`的` rememberMeRequested(HttpServletRequest request, String parameter)`方法。

并在继承`WebSecurityConfigurerAdapter`类的子类，即配置类中，重写的`void configure(HttpSecurity http)`方法中，开启自动登录：

```java
http.rememberMe().rememberMeServices(rememberMeServices());// 设置自动登录使用哪个 service
```

此外还需要在登录的过滤器中使用

```java
loginFilter.setRememberMeServices(rememberMeServices());
```

### （六）跨域

```java
CorsConfigurationSource configurationSource() {
    CorsConfiguration corsConfiguration = new CorsConfiguration();
    corsConfiguration.setAllowedHeaders(Arrays.asList("*"));
    corsConfiguration.setAllowedMethods(Arrays.asList("*"));
    corsConfiguration.setAllowedOrigins(Arrays.asList("*"));
    corsConfiguration.setMaxAge(3600L);
    UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
    source.registerCorsConfiguration("/**", corsConfiguration);
    return source;
}
```

```java
http.cors().configurationSource(configurationSource())
```

### （七）密码更新

```java
@Component
public class MyUserDetailService implements UserDetailsService,UserDetailsPasswordService {

    private final UserMapper userMapper;
    private final RoleMapper roleMapper;

    @Autowired
    public MyUserDetailService(UserMapper userMapper,RoleMapper roleMapper){
        this.roleMapper=roleMapper;
        this.userMapper=userMapper;
    }

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        LambdaQueryWrapper<User> lambdaQueryWrapper = new LambdaQueryWrapper<User>().eq(User::getUsername,username);
        User user = userMapper.selectOne(lambdaQueryWrapper);
        if(ObjectUtils.isEmpty(user)) throw new UsernameNotFoundException("用户名不正确");
        LambdaQueryWrapper<Role> lambdaQueryWrapper2 = new LambdaQueryWrapper<Role>().eq(Role::getId,user.getId());
        List<Role> roles = roleMapper.selectList(lambdaQueryWrapper2);
        user.setRoles(roles);
        return user;
    }
    @Override
    public UserDetails updatePassword(UserDetails user, String newPassword) {
        System.out.println("user:"+user+" newPassword:"+newPassword);
        LambdaUpdateWrapper<User> lambdaUpdateWrapper= new LambdaUpdateWrapper<User>().eq(User::getUsername,user.getUsername()).set(User::getPassword,newPassword);
        int result = userMapper.update(null, lambdaUpdateWrapper);
        if(result == 1){
            ((User) user).setPassword(newPassword);
        }
        return user;
    }
}
```

### (八）CSRF

```java
 http.csrf().csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse());// 将令牌保存到 cookie 中，并允许前端获取
```

前端请求时header中需要携带`XSRF-TOKEN`字段。

### （九）OAUTH2



### (九）功能集成

{% post_link java/spring-security/功能集成配置 '前后端分离-功能集成配置' %}
