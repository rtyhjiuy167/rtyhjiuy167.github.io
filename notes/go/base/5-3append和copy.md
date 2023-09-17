---
title: 5-3 append和copy
top: 5.3
tags:
  - go
categories:
  - go
---

```go
package main

import "fmt"

func main() {
	a1 := []int{1, 3, 5}
	//var a2 []int 这样声明是没有内存的，copy不进去
	//len与cap相同时，cap可不写
	var a2 = make([]int, 4)
	copy(a2, a1)
	fmt.Println(a1, a2)
	// [1 3 5] [1 3 5 0]

	//删掉数组中的某元素 其实就是利用左闭右开来实现
	a1 = append(a1[:1], a1[2:]...)
	fmt.Printf("a1:%v cap(a1):%v\n", a1, cap(a1)) //a1:[1 5] cap(a1):3

	x1 := [...]int{1, 3, 5, 7, 9}
	s1 := x1[:]
	fmt.Printf("x1:%v cap(x1):%v len(x1):%d\n", x1, cap(x1), len(x1))
	fmt.Printf("s1:%v cap(s1):%v len(s1):%d\n", s1, cap(s1), len(s1))
	s1 = append(s1[:1], s1[2:]...) //修改了底层数组 后面的整体往前移一格，覆盖原有的
	fmt.Printf("s1:%v\n", s1)      //s1:[1 5 7 9]
	fmt.Printf("x1:%v\n", x1)      //x1:[1 5 7 9 9]

	var arr1 = make([]int, 5, 10) //创建的数组已有5个零 因为长度为5
	fmt.Printf("arr1:%v\n", arr1) //arr1:[0 0 0 0 0]
}

```

