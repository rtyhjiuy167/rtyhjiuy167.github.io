### bean加载控制

为避免spring加载到springmvc的bean，需要对bean进行加载控制。

1）精准扫描

2）过滤

@ComponentScan的 excludeFilters属性可用于过滤，而includeFilters属性可用于加载。

```java
@Configuration
@ComponentScan(
        value = "com.rtyhjiuy15",
        excludeFilters = @ComponentScan.Filter(
                type = FilterType.ANNOTATION, // 按注解过滤
                classes = Controller.class // 过滤带 Controller 的类
        ))
public class SpringConfig {
}
```

3）不区分spring和springMVC环境



