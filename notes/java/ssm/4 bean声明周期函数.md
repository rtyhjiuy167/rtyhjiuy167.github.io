### bean的生命周期

1）通过配置`applicationContext.xml`文件使用声明周期函数

`BookDaoImpl`类中先声明两个函数`init`与`destroy`：

```java
public class BookDaoImpl implements BookDao {
   public void init(){
       System.out.println("init。。。");
   }
    public void destroy(){
        System.out.println("destory。。。");
   }

    @Override
    public void save(){
        System.out.println("book dao save ...");
    }
}
```

在`applicationContext.xml`中，配置bean的`init-method`属性其值为`init`，意为在初始化该bean时，会执行类中的`init`方法，而属性`destroy-method`其值为`destroy`，意为在关闭容器时，会执行类中的`destroy`方法：

```xml
 <bean id="BookDaoImpl" class="com.rtyhjiuy15.dao.impl.BookDaoImpl" init-method="init" destroy-method="destroy"/>
```

强行关闭IoC容器：

```java
ConfigurableApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
BookDao bookDao = (BookDao) ctx.getBean("BookDaoImpl");
ctx.close();
```

使用钩子进行关闭：

```java
ConfigurableApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
BookDao bookDao = (BookDao) ctx.getBean("BookDaoImpl");
ctx.registerShutdownHook();
```

2）通过继承接口文件使用声明周期函数

```java
public class BookDaoImpl implements BookDao, InitializingBean, DisposableBean {
    public BookDaoImpl(){
        System.out.println("constructor...");
    }

    @Override
    public void save(){}

    @Override
    public void destroy() throws Exception {
        System.out.println("destroy...");
    }

    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("after constructor and set...");
    }
}
```

类通过实现`InitializingBean`、 `DisposableBean`接口后，在`applicationContext.xml`文件中就不用配置`init-method`和`destroy-method`属性了：

```xml
<bean id="BookDaoImpl" class="com.rtyhjiuy15.dao.impl.BookDaoImpl"/>
```

