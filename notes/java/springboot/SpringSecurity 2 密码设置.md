### configure(HttpSecurity http)



重写`configure(HttpSecurity http)`方法，可以设置自定义校验资源认证规则。

```java
http.authorizeRequests()
    .mvcMatchers("/login.html").permitAll() // 一定要放行指定登录页面静态资源
    .mvcMatchers("/index").permitAll()
    .anyRequest().authenticated()
    .and() // and用于返回 http ，以实现链式调用
    .formLogin() // 表单登录
    .loginPage("/login.html")   // 使用thymeleaf指定登录页面,前缀与后缀可在application.properties中进行配置
    .loginProcessingUrl("/doLogin") // 指定登录页面后，前端页面的action必须和此配置相同
    .usernameParameter("username") // 指定登录页面后，用于指定用户名输入框的name属性的值
    .passwordParameter("password") //指定登录页面后，用于指定用户名输入框的password属性的值
    //                .successForwardUrl("/users") // 登录成功后跳转,地址栏不变 以post形式请求
    //                .defaultSuccessUrl("/users",false) //当访问的url被拦截时，登录成功后会自动跳转,如果没有url拦截，则自动跳转到指定url 重定向 //第二个参数 false，表示关闭固定跳转到指定url
    //                .successHandler()//
    //                .failureForwardUrl()// 失败跳转 forward
    //                .failureUrl()// 失败跳转 redirect 重定向
    .failureHandler(new MyAuthenticationFailureHandler()) // 认证失败后返回 json
    .and()
    .logout()//拿到注销登录的相关配置
    //                .logoutUrl("/logout")// 设置get的注销登录的url
    .logoutRequestMatcher(
    new OrRequestMatcher(
        new AntPathRequestMatcher("/logout", "POST"),
        new AntPathRequestMatcher("/logout", "GET")
    )
)// 设置退出的请求的url与请求方式
    .invalidateHttpSession(true) // 会话失效
    .clearAuthentication(true) // 认证标记清除
    //                .logoutSuccessUrl("/login.html")// 注销成功后跳转页面
    .logoutSuccessHandler(new MyLogoutSuccessHandler())
    .and()
    .csrf().disable();//跨站请求保护
```



### 自定义数据源

配置类中重写`configure(AuthenticationManagerBuilder builder)`方法：

```java
// 自定以配置数据源信息
@Autowired
private  MyUserDetailService myUserDetailService;

@Override
protected void configure(AuthenticationManagerBuilder builder) throws Exception {
builder.userDetailsService(myUserDetailService);
}
```

 MyUserDetailService类的代码如下：

```java
@Component
public class MyUserDetailService implements UserDetailsService {
    @Autowired
    private UserMapper userMapper;

    @Autowired
    private RoleMapper roleMapper;

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
}
```

实体类User格式如下：

```java
@Data
public class User implements UserDetails {
    private Integer id;
    private String username;
    private String password;
    private Boolean enabled; // 账户是否激活
    @TableField("accountNonExpired")
    private Boolean accountNonExpired; // 账户是否过期
    @TableField("accountNonLocked")
    private Boolean accountNonLocked; // 账户是否锁定
    @TableField("credentialsNonExpired")
    private Boolean credentialsNonExpired;// 密码是否过期

    @TableField(exist = false)
    private List<Role> roles = new ArrayList<>();

    //返回权限信息
    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        Set<SimpleGrantedAuthority> authorities = new HashSet<>();
        roles.forEach(role -> {
            SimpleGrantedAuthority simpleGrantedAuthority = new SimpleGrantedAuthority(role.getName());
            authorities.add(simpleGrantedAuthority);
        });
        return authorities;
    }

    @Override
    public String getPassword() {
        return password;
    }

    @Override
    public String getUsername() {
        return username;
    }

    @Override
    public boolean isAccountNonExpired() {
        return accountNonExpired;
    }

    @Override
    public boolean isAccountNonLocked() {
        return accountNonLocked;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return credentialsNonExpired;
    }

    @Override
    public boolean isEnabled() {
        return enabled;
    }
}

```

实体类Role如下：

```java
@Data
public class Role {
    private Integer id;
    private String name;
    @TableField("name_zh")
    private String nameZn;
}
```

### 前后端分离：

```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Autowired
    private MyUserDetailService myUserDetailService;

    // 自定以配置数据源信息
    @Override
    protected void configure(AuthenticationManagerBuilder builder) throws Exception {
        builder.userDetailsService(myUserDetailService);
    }

    // 作用：用来将自定义AuthenticationManager在工厂中进行暴露，可以在任何位置注入
    @Override
    @Bean
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }

    // 重写 configure(HttpSecurity http) 方法放行指定资源
    // 放行地资源应该写在要认证的前面
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                .anyRequest().authenticated()
                .and()
                .formLogin()
                .and()
                .exceptionHandling()
                .authenticationEntryPoint((request, response, authException) -> {
                    response.setContentType(MediaType.APPLICATION_JSON_UTF8_VALUE);
                    response.setStatus(HttpStatus.UNAUTHORIZED.value());
                    response.getWriter().println("请先认证");
                })
                .and()
                .csrf().disable();

        //  addFilterAt 用自定义的过滤器替换原来的过滤器
        http.addFilterAt(loginFilter(), UsernamePasswordAuthenticationFilter.class);
        http.logout().logoutUrl("/logout").logoutSuccessHandler((request, response, authentication) -> {
            Map<String, Object> result = new HashMap<>();
            result.put("msg", "注销成功");
            result.put("用户信息", authentication.getPrincipal());
            response.setStatus(HttpStatus.OK.value());
            response.setContentType("application/json;charset=UTF-8");
            String s = new ObjectMapper().writeValueAsString(result);
            response.getWriter().println(s);
        });
    }

    @Bean
    public LoginFilter loginFilter() throws Exception {
        LoginFilter loginFilter = new LoginFilter();
        loginFilter.setFilterProcessesUrl("/doLogin");
        loginFilter.setUsernameParameter("username");
        loginFilter.setPasswordParameter("password");
        loginFilter.setAuthenticationManager(authenticationManagerBean());
        loginFilter.setAuthenticationSuccessHandler((request, response, authentication) -> {
                    Map<String, Object> result = new HashMap<>();
                    result.put("msg", "登录成功");
                    result.put("用户信息", authentication.getPrincipal());
                    response.setStatus(HttpStatus.OK.value());
                    response.setContentType("application/json;charset=UTF-8");
                    String s = new ObjectMapper().writeValueAsString(result);
                    response.getWriter().println(s);
                }
        );
        loginFilter.setAuthenticationFailureHandler((request, response, ex) -> {
            Map<String, Object> result = new HashMap<>();
            result.put("msg", "登录失败:" + ex.getMessage());
            response.setStatus(HttpStatus.INTERNAL_SERVER_ERROR.value());
            response.setContentType("application/json;charset=UTF-8");
            String s = new ObjectMapper().writeValueAsString(result);
            response.getWriter().println(s);
        });
        return loginFilter;
    }

}
```

