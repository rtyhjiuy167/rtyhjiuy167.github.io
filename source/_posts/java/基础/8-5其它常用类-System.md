---
title: 8-5 其它常用类-System
top: 8.5
tags:
  - Java
categories:
  - Java
---

```java
import java.util.Arrays;

public class Test {
    public static void main(String[] args) {
        /*
        arraycopy 复制数组，会覆盖原原元素 比较适合底层调用
        src 源数组
        srcPos 从源数组的的起始位置开始拷贝
        dest 目标数组
        destPos 拷贝到目标数组的起始位置
        length 从源数组拷贝多少个数据到目标数组 不要越界
         */
        // 一般都不用，都用 Arrays.copyOf 完成复制数组
        int[] src = {1, 2, 3};
        int[] dest = new int[3];
        System.out.println(Arrays.toString(dest)); // [0, 0, 0]
        System.arraycopy(src, 0, dest, 0, 3);
        System.out.println(Arrays.toString(dest)); // [1, 2, 3]
        System.arraycopy(src, 1, dest, 0, 1);
        System.out.println(Arrays.toString(dest)); // [2, 2, 3]

        //currentTimeMillis() 返回当前时间距离 1970-1-1的毫秒数
        System.out.println(System.currentTimeMillis());

        //gc() 通知系统进行垃圾回收
        System.gc();
        // exit 退出当前程序
        System.exit(0);
    }
}

```

