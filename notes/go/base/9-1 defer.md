---
title: 9-1 defer
top: 9.1
tags:
  - go
categories:
  - go
---

```go
package main

import "fmt"

func main() {
	a := deferDemo()
	fmt.Println(a)    //2
	fmt.Println(f1()) //6
	fmt.Println(f2()) //5
	fmt.Println(f3()) //5

	q1 := 1
	q2 := 2
	//defer 只会剩下一层来后执行 函数里的参数 赋值的右边 全部都已经确定了
	defer calc("1", q1, calc("10", q1, q2)) //按顺序执行到这一行时，会 执行成 calc("1", 1, 3) 该句则到后面再执行
	q1 = 0
	defer calc("2", q1, calc("20", q1, q2))
	/*
		10 1 2 3
		20 0 2 2
		2 0 2 2
		1 1 3 4
	*/
}

func deferDemo() int {
	fmt.Println("start")
	defer fmt.Println("loading")
	//defer把他后面的语句延迟到函数即将返回的时候再执行（此时返回值已经确定了）
	// return a 分成两步
	//  1. 返回值 = a  2. return 返回值 真正的RET返回
	// defer 是在1、2部之间的
	//多个 defer 按照顺序依次执行
	fmt.Println("end")
	a := 2
	defer func() {
		a++
	}()
	return a
}

func f1() (x int) {
	defer func() {
		x++
	}()
	return 5 //返回值赋值 返回值++ RET返回  最终结果：6
}

func f2() (y int) {
	x := 5
	defer func() {
		x++
	}()
	return x //最终结果：5
}

func f3() (x int) {
	defer func(x int) {
		x++ // 改变的是函数中的副本
	}(x)
	return 5 //最终结果：5
}

func calc(index string, a, b int) int {
	ret := a + b
	fmt.Println(index, a, b, ret)
	return ret
}

```

