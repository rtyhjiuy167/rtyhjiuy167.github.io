---
title: 8-3 其它常用类-Math
top: 8.3
tags:
  - Java
categories:
  - Java
---

```java
public class Test {
    public static void main(String[] args) {
        //abs 绝对值
        System.out.println(Math.abs(-2));
        //pow 求幂
        System.out.println(Math.pow(3.00, 2)); // 3.0 ^2.0 = 9.0
        //ceil 向上取整
        System.out.println(Math.ceil(-2.1)); // -2.0
        //floor 向下取整
        System.out.println(Math.floor(-2.1)); // -3.0
        //round 四舍五入
        System.out.println(Math.round(-2.1)); // -2
        //sqrt 求开方
        System.out.println(Math.sqrt(3)); // 1.7320508075688772
        //random 生成随机小数 [0,1)
        System.out.println(Math.random());
    }
}
```

