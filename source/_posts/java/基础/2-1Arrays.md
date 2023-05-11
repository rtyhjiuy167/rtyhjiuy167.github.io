---
title: 2-1 Arrays
top: 2.1
tags:
  - Java
categories:
  - Java
---

<h4>数组创建</h4>

```java
public class Text {
    public static void main(String[] args) {
        //数组 引用类型
        // 在堆内存中开辟出一段连续的空间，用于存储数组中的元素
        // 栈内存 其实际存储的是数组首元素的地址 其存储机制其实就是引用堆内存中的数据

        /*
         * 静态创建
         *  即数组在创建的时候，其内部的数据就已经有初始值
         * 语法：
         * 数据类型[] 数组名={数据}
         * 数据类型[] 数组名=new 数据类型[]{数据}
         *  两种语法的效果相同
         * */
        int[] arr1 = {1,2,3};
        int[] arr2 = new int[]{};

        /**
         *动态创建
         *  即只规定数组的长度
         * 语法：
         * 数据类型[] 数组名=new 数据类型[数组的长度]
         */
        int[] arr3 = new int[1];

        /**
         * 索引取出数组中的元素 第一个元素的下标为0
         * 语法：
         *  数组名[下标]
         */

        //数组的长度属性 数组一但创建，其长度不可改变，但元素值可以改变
        int len=arr3.length;
    }
}

```

<h4>数组遍历</h4>

```java
public class Text {
    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4};
        //1.for 用索引+length 遍历数组
        //2.简化for 用foreach循环遍历数组 没有索引
        for (int a : arr) {
            //第一次把arr[0]赋给a
            //第二次把arr[1]赋给a
            //...
            // 注意：a是中间变量
            System.out.println(a);
        }
    }
}

```

<h4>数组元素追加</h4>

```java
public class Text {
    public static void main(String[] args) {

        int[] arr = {1, 2, 3, 4};
        //追加一个元素 5
        //由于数组长度不会改变，只能创建一个新的数组
        int[] arr1 = new int[arr.length + 1];

        for (int i=0;i<arr.length;i++) {
            arr1[i]=arr[i];
        }
        arr1[arr1.length-1]=5;
        arr=arr1; //原arr指向的地址已无变量指向，被自动回收
        //这样就实现了追加 如果是在中间追加 就用两个并列的for实现
        // 笑
        for (int a:arr){
            System.out.println(a);
        }
    }
}
```

<h4>数组元素删除</h4>

```java
public class Text {
    public static void main(String[] args) {

        //方法1. 不创建新数组
        int[] arr = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
        //准备一个有效元素个数统计的变量
        int size = arr.length;
        //删除数组索引为 4 的元素
        int delIndex = 4;
        //从删除的位置开始，之后往前移动 要删除的元素相当于被覆盖了
        for (int i = delIndex; i < size - 1; i++) {
            arr[i] = arr[i + 1];
        }
        //最后一位闲置
        arr[size - 1] = 0;
        size--;
        for (int a : arr) {
            System.out.println(a);
        }
        System.out.println("-------------我是分割线-------------");
        //方法2. 创建新数组
        int[] arr1 = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
        int[] newArr = new int[arr1.length - 1];
        //删除数组索引为 4 的元素
        // 4 前面的复制过去就行了
        for (int i = 0; i < 4; i++) {
            newArr[i] = arr1[i];
        }
        for (int i = 5; i < arr1.length; i++) {
            newArr[i - 1] = arr1[i];
        }
        arr1 = newArr;
        for (int a : arr1) {
            System.out.println(a);
        }
    }
}

```

<h4>数组工具类</h4>

```java
import java.util.Arrays;
public class Text {
    public static void main(String[] args) {
        int[] arr = {12, 2, 1, 42, 32, 25, 65, 45, 62, 433};
        //对数组元素进行排序
        Arrays.sort(arr);
        for(int v:arr){
            System.out.println(v);
        }

        /**
         * 在数组中用二分法查询元素出现的下标数+1 没找到则返回一个负数
         * Arrays.binarySearch(arr,value) 要求数组必须是升序
         */
        int index =Arrays.binarySearch(arr,1123);
        System.out.println(index);

        //快速的遍历数组 返回给定数组的字符串表达形式
        String arr1 = Arrays.toString(arr);
        System.out.println(arr1);

        /**
         * 数组复制  仅复制元素
         * Arrays.copyOf(arr,newLength) 内部其实是使用了 System.arraycopy
         *  newLength 不能大于 arr.Length
         *  当newLength<arr.Length时，只复制前几个
         */
        int[] arr2 = Arrays.copyOf(arr,arr.length);
        System.out.println(Arrays.toString(arr2));

        /**
         *System.arraycopy(src,srcPos,dest,destPost,length)
         * src：源数组
         * srcPos：起始索引 从源数组中的哪个索引开始复制
         * dest：目标数组
         * destPost：目标索引 复制的元素放目标数组中的起始索引
         * length：要复制多长
         */
        int[] arr3=new int[arr.length+1];
        System.arraycopy(arr,3,arr3,1,5);
        System.out.println(Arrays.toString(arr3));
    }
}

```

