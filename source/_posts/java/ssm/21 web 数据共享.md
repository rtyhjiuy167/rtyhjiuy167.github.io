### 域对象共享数据

#### 使用 servletAPI 向 request 域对象共享数据

```java
package com.rtyhjiuy15.mvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import javax.servlet.http.HttpServletRequest;

@Controller
public class HelloController {
    @RequestMapping(value = "/")
    public String index(HttpServletRequest request) {
        request.setAttribute("testRequestScope", "hello,servletAPI");
        return "index";
    }
}
```

index.html：

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org" xmlns="http://www.w3.org/1999/html">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>
    <!-- 如果下行idea出现红线，可以关闭第一个 Thymeleaf 检查 -->
    <p th:text="${testRequestScope}"></p>
</body>
</html>
```

#### 使用 ModelAndView 向 request 域对象共享数据

```java
package com.rtyhjiuy15.mvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;


@Controller
public class HelloController {
    @RequestMapping("/")
    public ModelAndView testModelAndView() {
        ModelAndView mav = new ModelAndView();
        // 处理模型数据，即向请求域 request 共享数据
        mav.addObject("testRequestScope","hello,ModelAndView");
        // 设置视图名称，即页面
        mav.setViewName("index");
        return mav;
    }
}
```

#### 使用 Model 向 request 域对象共享数据

```java
package com.rtyhjiuy15.mvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;


@Controller
public class HelloController {
    @RequestMapping("/")
    public String testModel(Model model) {
        model.addAttribute("testRequestScope","hello,model");
        return "index";
    }
}
```

#### 使用 Map 向 request 域对象共享数据

```java
package com.rtyhjiuy15.mvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import java.util.Map;

@Controller
public class HelloController {
    @RequestMapping("/")
    public String testMap(Map<String,Object> map) {
        map.put("testRequestScope","hello,map");
        return "index";
    }
}
```

#### 使用 ModelMap 向 request 域对象共享数据

```java
package com.rtyhjiuy15.mvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HelloController {
    @RequestMapping( "/")
    public String testModelMap(ModelMap modelMap) {
        modelMap.addAttribute("testRequestScope","hello,ModelMap");
        return "index";
    }
}
```

Model、ModelMap、Map类型的参数本质上都是 BindingAwareModelMap 类型的

#### 向 session 域共享数据

```java
package com.rtyhjiuy15.mvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import javax.servlet.http.HttpSession;

@Controller
public class HelloController {
    @RequestMapping( "/")
    public String testHttpSession(HttpSession session) {
        session.setAttribute("testSessionScope","hello,session");
        return "index";
    }
}
```

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org" xmlns="http://www.w3.org/1999/html">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>
    <!-- 如果下行idea出现红线，可以关闭第一个 Thymeleaf 检查 -->
    <p th:text="${session.testSessionScope}"></p>
</body>
</html>
```

#### 向 application 域共享数据

```java
package com.rtyhjiuy15.mvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import javax.servlet.ServletContext;
import javax.servlet.http.HttpSession;

@Controller
public class HelloController {
    @RequestMapping( "/")
    public String testApplication(HttpSession session) {
        ServletContext application = session.getServletContext();
        application.setAttribute("testApplicationScope","hello,application");
        return "index";
    }
}
```

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org" xmlns="http://www.w3.org/1999/html">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>
    <!-- 如果下行idea出现红线，可以关闭第一个 Thymeleaf 检查 -->
    <p th:text="${application.testApplicationScope}"></p>
</body>
</html>
```

### SpringMVC的视图

SpringMVC中的视图是View接口，视图的作用是渲染数据，将模型Model中的数据展示给用户

SpringMVC中的视图的种类有多种，默认有转发视图（InternalResourceView）和重定向视图（RedirectView）

若使用的视图技术为 Thymeleaf，在SpringMVC配置文件中配置了 Thymeleaf 的视图解析器，由此视图解析之后得到的是 ThymeleafView。

#### ThymeleafView

当控制器方法中所设置的视图名称没有任何前缀时，该视图会被 SpringMVC 配置文件中所配置的视图解析器解析，视图名称拼接视图前缀和视图后缀所得到的最终路径，会通过转发的方式实现跳转。

#### InternalResourceView

```java
package com.rtyhjiuy15.mvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;


@Controller
public class HelloController {

    @RequestMapping( "/haha")
    public String testIndex() {
        return "index";
    }
    @RequestMapping( "/")
    public String testView() {
        return "forward:/haha"; // 相当于跳到 /haha 执行对应的控制器
    }
}
```

#### RedirectView

重定向视图服务器发送两次请求。url会改变。

```java
package com.rtyhjiuy15.mvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;


@Controller
public class HelloController {

    @RequestMapping( "/haha")
    public String testIndex() {
        return "index";
    }
    @RequestMapping( "/")
    public String testView() {
        return "redirect:/haha";
    }
}
```

#### 视图控制器

SpringMVC.xml：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!-- 扫描组件 -->
    <context:component-scan base-package="com.rtyhjiuy15.mvc.controller"></context:component-scan>
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
    <!-- 相当于视图控制器中没有其他处理，直接 return 视图名称 -->
    <mvc:view-controller path="/" view-name="index"></mvc:view-controller>
    <!-- 开启视图控制器后，还需开启mvc注解驱动 -->
    <mvc:annotation-driven/>
</beans>
```

