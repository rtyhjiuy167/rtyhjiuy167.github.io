### 什么是MVC

MVC是一种软件架构的思想，将软件按照模型、视图、控制器来划分

M：model，模型层，指工程中的JavaBean，作用是处理数据

V：View，视图层，指工程中的html或jsp等页面，作用是与用户进行交互，展示数据

C：Controller，控制层，指工程中的servlet，作用是接收请求和响应浏览器

MVC工作流程：

用户通过视图层发送请求到访问器，在服务器中请求被 Controller 接收，Controller 调用相应 Model 层处理请求，处理完毕将结果返回到 Controller，Controller 再根据请求处理的结果找到相应的 View 视图，渲染数据后最终响应给浏览器。

### 什么是SpringMVC

SpringMVC是Spring的一个后续产品，是Spring的一个子项目

SpringMVC基于原生的 Servlet，通过功能强大的前端控制器DispatcherServlet，对请求和响应进行统一处理

### 使用xml配置

1）新建项目

`File`⟶`New`⟶`Project`⟶`Maven`⟶JDK选择8或11⟶`Next`⟶填写Artifact Coordinates（可默认）⟶`Finish`

2）导入相关依赖

在`pom.xml`文件中的`project`标签里**添加**如下标签：

```xml
<!-- webapp必须使用war打包方式  -->
<packaging>war</packaging>

<dependencies>
    
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.3.19</version>
    </dependency>
    
    <!-- 日志 -->
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-core</artifactId>
        <version>1.2.11</version>
    </dependency>
    
    <!-- servletAPI -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
        <!--依赖范围设置为 provied ， 说明依赖在服务器中已被提供，所以该jar包由服务器提供，不进行打包 -->
        <scope>provided</scope>
    </dependency>
    
    <!-- Spring 和 Thtmeleaf整合包 -->
    <dependency>
        <groupId>org.thymeleaf</groupId>
        <artifactId>thymeleaf-spring5</artifactId>
        <version>3.0.14.RELEASE</version>
    </dependency>
    
</dependencies>
```

3）创建web.xml文件

`src\main`目录下新建`webapp`目录

`File`⟶`Project Structure`⟶`Modules`⟶点击自己的项目⟶选择`Web`⟶点击右侧`Add Application Ser specific descriptor..`上方的加号按钮⟶选择`web.xml`（如果不可选择，说明之前的依赖未成功导入）⟶输入路径`根目录\src\main\webapp\WEB-INF\web.xml`

`web.xml`文件的**所有内容**如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!-- 配置SpringMVC的前端控制，对浏览器发送的请求进行统一处理 -->
    <servlet>
        <servlet-name>SpringMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

        <!-- 配置Spring配置文件的位置和名称 -->
        <init-param>

            <param-name>contextConfigLocation</param-name>
            
            <!-- classpath: 后的字符串与 resources 目录下的一致-->
            <param-value>classpath:springMVC.xml</param-value>

        </init-param>

        <!-- 将前端控制器 DispatcherServlet 的初始化时间提前到服务器启动时，可以提高第一次访问时间 -->
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>SpringMVC</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

</web-app>
```

4）创建SpringMVC的配置文件

根据`web.xml`文件中的SpringMVC配置信息，右键`src\main\sources`目录⟶`New`⟶`XML Configuration File`⟶`Spring Config`⟶输入文件名`springMVC`。

`springMVC.xml`文件的**所有内容**如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 扫描组件 -->
    <context:component-scan base-package="com.rtyhjiuy15.controller"></context:component-scan>
    <!-- 配置 Thymeleaf 视图解析器 -->
    <bean id="viewResolver" class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
        <property name="order" value="1"/>
        <property name="characterEncoding" value="UTF-8"/>
        <property name="templateEngine">
            <bean class="org.thymeleaf.spring5.SpringTemplateEngine">
                <property name="templateResolver">
                    <bean class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
                        <!-- 视图前缀 -->
                        <property name="prefix" value="/WEB-INF/templates/"/>
                        <!-- 视图后缀 -->
                        <property name="suffix" value=".html"/>
                        <!-- 模板模型 -->
                        <property name="templateMode" value="HTML5"/>
                        <property name="characterEncoding" value="UTF-8"/>
                    </bean>
                </property>
            </bean>
        </property>
    </bean>
</beans>
```

5）创建控制器

根据`springMVC.xml`文件中的包扫描的具体位置，在`src\main\java`目录新键包`com.rtyhjiuy15`，并在该包下建立子包`controller`，在该子包下新建一个类，名为`HelloController`。

`HelloController`类里的内容如下：

```java
@Controller
public class HelloController {
    @RequestMapping("/")
    public String index() {
        return "index";
    }
}
```

6）创建html文件

根据`springMVC.xml`文件中视图解析路径，在`src\main\webapp\WEB-INF`目录下新建`templates`目录，再在`templates`目录下建立`index.html`文件，并在`index.html`文件中的`body`标签里填上`Hello World`

7）配置Tomcat

<img src=".\pictures\tomcat配置1.png" alt="tomcat配置1" style="zoom:150%;" />

<img src=".\pictures\tomcat配置2.png" alt="tomcat配置2" style="zoom:80%;" />

<img src=".\pictures\tomcat配置3.png" alt="tomcat配置3" style="zoom:80%;" />

<img src=".\pictures\tomcat配置4.png" alt="tomcat配置4" style="zoom:80%;" />

<img src=".\pictures\tomcat配置5.png" alt="tomcat配置5" style="zoom:80%;" />

### 使用注解简单配置

1）新建项目

`File`⟶`New`⟶`Project`⟶`Maven`⟶JDK选择8或11⟶`Next`⟶填写Artifact Coordinates（可默认）⟶`Finish`

2）导入相关依赖

在`pom.xml`文件中的`project`标签里**添加**如下标签：

```xml
<!-- webapp必须使用war打包方式  -->
<packaging>war</packaging>
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.3.19</version>
    </dependency>
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
    </dependency>
</dependencies>
```

3）创建控制器

在`src\main\java`目录新键包`com.rtyhjiuy15`，并在该包下建立子包`controller`，在该子包下新建一个类，名为`HelloController`。

`HelloController`类里的内容如下：

```java
@Controller
public class HelloController {
    @ResponseBody
    @RequestMapping(value = "/")
    public String index() {
        return "index";
    }
}
```

4）创建springmvc配置类

在`src\main\java\com.rtyhjiuy15`目录下建立包`config`，在该包下新建一个类，名为`SpringMvcConfig`。

```java
@Configuration
@ComponentScan("com.rtyhjiuy15.controller")
public class SpringMvcConfig {
}
```

5）创建servlet配置类

在`src\main\java\com.rtyhjiuy15\config`目录下新建一个类，名为`ServletConfigInitConfig`。

```java
// 实现覆盖AbstractDispatcherServletInitializer的三个方法后即可加载该配置
public class ServletConfigInitConfig  extends AbstractDispatcherServletInitializer
{
    @Override
    protected WebApplicationContext createServletApplicationContext() {
        AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
        ctx.register(SpringMvcConfig.class);
        return ctx;
    }

    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"}; // 拦截所有请求交由springmvc处理，而不是tomcat
    }

    @Override
    protected WebApplicationContext createRootApplicationContext() {
        return null;
    }
}
```

上面代码的简写：

```java
public class ServletConfigInitConfig  extends AbstractAnnotationConfigDispatcherServletInitializer
{

    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{SpringConfig.class};
    }

    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringMvcConfig.class};
    }

    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
}
```

6）配置Tomcat并启动即可
