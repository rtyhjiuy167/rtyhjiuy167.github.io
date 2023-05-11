---
title: 8-4 其它常用类-Arrays
top: 8.4
tags:
  - Java
categories:
  - Java
---

<h4>Arrays</h4>

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;

public class Test {
    public static void main(String[] args) {
        //Arrays.toString() 显示数组  底层还是for 循环 连接成 字符串
        Integer[] arr1 = {1, 2, 3};
        System.out.println(Arrays.toString(arr1));

        //sort 采用的是 binarySort排序 因为数组是引用类型，sort 后会直接影响 arr2
        Integer[] arr2 = {5, 3, 1, 8, 6};
        Arrays.sort(arr2);
        System.out.println(Arrays.toString(arr2)); // [1, 3, 5, 6, 8]
        /*
        sort 定制排序
            第二个参数为实现了 Comparator 接口的匿名内部类
         */
        Arrays.sort(arr2, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2 - o1; // 降序
            }
        });
        System.out.println(Arrays.toString(arr2));

        // binarySearch 对升序数组进行二分查找 查找不到 返回负数
        System.out.println(Arrays.binarySearch(arr1, 2));
        System.out.println(Arrays.binarySearch(arr2, 6)); // -6
        //copyOf(original,newLength) 拷贝长度大于数组长度，则添加null
        System.out.println(Arrays.toString(Arrays.copyOf(arr2, arr2.length + 1)));// [8, 6, 5, 3, 1, null]

        // fill 用指定的value 替换所有元素
        Arrays.fill(arr2, 66);
        System.out.println(Arrays.toString(arr2)); // [66, 66, 66, 66, 66]

        // equals 比较两个数组内容是否相同
        System.out.println(Arrays.equals(arr1, arr2)); // false

        //asList 返回 List
        List asList= Arrays.asList(arr1);
        System.out.println("Arrays.asList(arr1)：" + asList + "\t其类型为：" + asList.getClass());
        // Arrays.asList(arr1)：[1, 2, 3]	其类型为：class java.util.Arrays$ArrayList    $ 内部类  ArrayList 为内部类
    }
}
```

<h4>自定义排序</h4>

```java
import java.util.Arrays;
import java.util.Comparator;

public class Test {
    public static void main(String[] args) {
        Book[] books = new Book[4];
        books[0] = new Book("book123", 10000);
        books[1] = new Book("book1", 1000);
        books[2] = new Book("book12", 100000);
        books[3] = new Book("book1234", 100);

        //1. price 从大到小
        Arrays.sort(books, new Comparator<Book>() {
            @Override
            public int compare(Book o1, Book o2) {
                return (int) (o2.getPrice() - o1.getPrice());
            }
        });
        System.out.println(Arrays.toString(books)); // 会自动对每个对象调用 重写的toString() 方法

        // 按书名长度排序 从小到大
        Arrays.sort(books, new Comparator<Book>() {
            @Override
            public int compare(Book o1, Book o2) {
                return o1.getName().length() - o2.getName().length();
            }
        });
        System.out.println(Arrays.toString(books));

        // 按书名大小排序 从大到小
        Arrays.sort(books, new Comparator<Book>() {
            @Override
            public int compare(Book o1, Book o2) {
                return o2.getName().compareTo(o1.getName());
            }
        });
        System.out.println(Arrays.toString(books));
    }
}

class Book {
    private String name;
    private double price;

    public Book(String name, double price) {
        this.name = name;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public double getPrice() {
        return price;
    }

    @Override
    public String toString() {
        return "Book{" +
                "name='" + name + '\'' +
                ", price=" + price +
                '}';
    }
}
```

