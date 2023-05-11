### 权限

1）在配置类上设置相应权限

`java\包名\config\security\SecurityConfig.java`：

```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    private UserDetailsService userDetailsService;

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService).passwordEncoder(password());
    }

    @Bean
    PasswordEncoder password() {
        return new BCryptPasswordEncoder();
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.exceptionHandling().accessDeniedPage("/unauth.html");
        http.formLogin()//自定义编写的登录页面
                .and().authorizeRequests()
                .antMatchers("/").permitAll()//设置可所有人访问的路径 即不需要输入账户及密码
                .antMatchers("/hello").hasAuthority("admins")//设置具有某个权限才能访问的路径
                //.antMatchers("/hello").hasRole("sale") // 如果一起使用则会以 或 判断
                .anyRequest().authenticated();//所有请求方式都允许
  
    }
}

```

2）在控制器方法上添加注解

先在配置类或主程序类上添加如下注解：

```java
@EnableGlobalMethodSecurity(securedEnabled = true)
```

* @Secured

  例如 @Secured({"ROLE_sale","ROLE_haha"})，对相应的权限字段进行校验后，再执行控制器方法

* @PreAuthorize()

  例如 @PreAuthorize("hasAnyAuthority('admins')")，对相应的权限字段进行校验后，再执行控制器方法

* @PostAuthorize()

  例如 @PostAuthorize("hasAnyAuthority('admins')")，先执行控制器方法，再对相应的权限字段进行，如果校验通过则返回结果

* @PostFilter

  对返回的数据进行过滤

* @PreFilter

  对传入的参数数据进行过滤