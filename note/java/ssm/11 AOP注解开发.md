### 使用

* @Pointcut

  用于标识方法，其为切入点，小括号中使用`"切入点表达式"`

  切入点表达式：动作关键字（execution） 访问修饰符（public 可省略） 返回值（如 void）包名.接口名/类.方法

  可以使用统配符描述切入点：

  * *

    单个独立的任意符号

    `* com.*.*.*.save()`能匹配到 `void com.rtyhjiuy15.dao.BookDao.save())`

    `* com.*.*.*.save(*)`不能匹配到 `void com.rtyhjiuy15.dao.BookDao.save())`

    `void com.rtyhjiuy15.dao.BookDao.*e())`能匹配到 `void com.rtyhjiuy15.dao.BookDao.save())`

  * .. 

    多个连续的符号

    `* com..save(..)`能匹配到 `void com.rtyhjiuy15.dao.BookDao.save())`

    `*..save(..)`能匹配到任意的包下的所有类或接口下的save方法

  * +

  书写技巧：

  * 描述切入点通常描述接口，而不描述实现类
  * 访问控制修饰符针对接口开发均采用public描述（可省略访问控制修饰符描述）

* @Before

  用于将共性功能与切入点建立联系，为前置通知

* @After

  用于将共性功能与切入点建立联系，为后置通知

* @Around

  用于将共性功能与切入点建立联系，为环绕通知

  原始操作的执行需要接收`ProceedingJoinPoint`类型的pjp参数后，使用`pjp.procedd()`方法在指定位置执行原始操作。

  原始操作如果有返回值，必须在最后以`Object`形式返回

* @AfterReturing

  无异常抛出才会运行，运行在@After之前

* @AfterThrowing

  有异常抛出才会运行

* @Aspect

  标识类进行AOP处理

* @EnableAspectJAutoProxy

  标识工程使用了AOP注解

```java
@Component
@Aspect
public class MyAdvice {
    @Pointcut("execution(int com.rtyhjiuy15.dao.BookDao.*e())")
    private void pt1() {
    }

    @Pointcut("execution(int com.rtyhjiuy15.dao.BookDao.*i())")
    private void pt2() {
    }

    @After("pt1()")
    public void method1() {
        System.out.println("123456");
    }

    @Around("pt2()")
    public Object method2(ProceedingJoinPoint pjp) throws Throwable {
        Object[] args = pjp.getArgs(); // 获取参数
        for(int i = 0; i < args.length; i++){
            if(args[i].getClass().equals(String.class)){
                args[i] = args[i].toString().trim();
            }
        }
        Integer num = (Integer) pjp.proceed(args); // 获取原始操作的返回值
        Signature signature =pjp.getSignature(); // 获取一次执行的签名信息，即封装了所有信息的对象
        String classname = signature.getDeclaringTypeName(); // 获取类名
        String methodName =signature.getName(); // 获取方法名
        System.out.println();
        System.out.println("123456");
        return num;
    }
}
```

```java
@Configuration
@ComponentScan("com.rtyhjiuy15")
@EnableAspectJAutoProxy
public class SpringConfig {}
```

### AOP工作流程

1.Spring容器启动

2.读取所有切面配置中的切入点

3.初始化bean，判定bean对应的类中的方法是否匹配到任意切入点

* 匹配失败，创建对象
* 匹配成功，创建原始对象（目标对象）的代理对象

4.获取bean方法

* 获取bean，调用方法并执行，完成操作
* 获取的是bean是代理对象时，根据代理对象的运行模式运行原始方法与增强的内容，完成操作

