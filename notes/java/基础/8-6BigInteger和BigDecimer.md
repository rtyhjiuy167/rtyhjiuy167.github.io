---
title: 8-6 BigInteger和BigDecimer
top: 8.6
tags:
  - Java
categories:
  - Java
---

```java
import java.math.BigDecimal;
import java.math.BigInteger;
import java.math.RoundingMode;

public class Test {
    public static void main(String[] args) {

        //当我们编程中，需要处理很大的整数，且 long 不够用
        // 可以使用 BigInteger 传值时 先使用双引号引起来
        BigInteger bigInteger1 = new BigInteger("12345678900987654321");

        //在对 BigInteger 进行加减乘除时，需要使用对应的方法
        //add 加法
        BigInteger bigInteger2 = bigInteger1.add(bigInteger1);
        System.out.println(bigInteger2); // 24691357801975308642
        //subtract 减法
        BigInteger bigInteger3 = bigInteger2.subtract(bigInteger1);
        System.out.println(bigInteger3); // 12345678900987654321
        //multiply 乘法
        BigInteger bigInteger4 = bigInteger1.multiply(bigInteger1);
        System.out.println(bigInteger4); // 152415787526291736223502514857789971041
        //multiply 除法
        BigInteger bigInteger5 = bigInteger1.divide(bigInteger1);
        System.out.println(bigInteger5); // 1

        // BigDecimal 可保留一个精度很高的数
        BigDecimal bigDecimal1 = new BigDecimal("1.00000000000000000000000000000000000000");
        //加减乘 和 BigInteger 相同
        // 除法 可能会遇到无限小数 可以指定精度即可
        BigDecimal bigDecimal2 = new BigDecimal("3");
        BigDecimal bigDecimal3 = bigDecimal1.divide(bigDecimal2, RoundingMode.CEILING); // 保留 分子的精度 向上取
        System.out.println(bigDecimal3); // 0.33333333333333333333333333333333333334
         bigDecimal3 = bigDecimal1.divide(bigDecimal2, RoundingMode.FLOOR); // 保留 分子的精度 向下取
        System.out.println(bigDecimal3); // 0.33333333333333333333333333333333333333
    }
}

```

