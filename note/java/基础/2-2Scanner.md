---
title: 2-2 Scanner
top: 2.2
tags:
  - Java
categories:
  - Java
---

```java
//导入扫描仪对象所需的包
import java.util.Scanner;

public class Text {
    public static void main(String[] args) {
        //创建一个扫描仪对象
        Scanner sc = new Scanner(System.in);

        //从键盘录入信息

        System.out.printf("请输入一个整数：" );
        int i = sc.nextInt(); // 从键盘中接收一个整数
        System.out.println("您输入的整数为：" + i);

        System.out.printf("请输入一个小数：" );
        double d = sc.nextDouble(); // 从键盘中接收一个浮点数
        System.out.println("您输入的小数为：" + d);

        System.out.printf("请输入文字：" );
        String s = sc.next(); // 从键盘中接收文字 空格后无法接收
        // String s = sc.nextLine();// 从键盘中接收文字 空格后可接收 但是前面代码有输出时，会出现异常
        
        System.out.println("您输入的文字为：" + s);

        sc.close();//释放资源 关闭扫描仪
    }
}
```

 <h4>循环读取</h4>

```java
package text;

import java.util.Scanner;
/*
scanner对象使用char[]数组保存用户输入的字符串
    以空格为分隔

hasNext()方法 阻塞并等待用户输入，有输入时会直接返回true

hasNext(pattern) 阻塞并等待用户输入，
    有输入时,若下一个将要读取的字符串为 pattern 则返回 true，否则为false

 */

public class Text {

    public static void main(String[] args) {
        String t;
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入：");
        while (!sc.hasNext("#")) {
            t = sc.next();
            System.out.println("读取到的数据为：" + t);

        }
        sc.close();
        System.out.println("读取结束");
    }

}
```

