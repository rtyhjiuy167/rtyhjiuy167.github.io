---
title: 17-1 channel
top: 17.1
tags:
  - go
categories:
  - go
---

```go
package main

import "fmt"

//channel是一种引用类型

func main() {
	var ch chan int
	fmt.Println(ch) // <nil>
	//声明的通道后需要使用make函数初始化之后才能使用

	// make(chan 元素类型, [缓冲大小])  缓冲大小指能存几个值
	//通道关闭后仍能取值

	ch1 := make(chan int, 1) //带缓冲区通道
	ch1 <- 10                //发送值
	x := <-ch1
	// len(ch1)  取通道中元素的数量
	// cap(ch1)  拿到通道的容量
	fmt.Println(x)
	close(ch1) //关闭通道不是必须的 通道是可以被垃圾回收机制回收的

	//ch2 := make(chan int) //无缓冲区通道，又称为阻塞的通道

	channal1 := make(chan int, 100)
	channal2 := make(chan int, 200)
	go f1(channal1)
	go f2(channal1, channal2)

	for ret := range channal2 { //for range 会自己判断是否取完值
		fmt.Println(ret)
	}
}

func f1(ch chan<- int) { //通过 chan 右边加 <- 可以确保 ch 在函数里只能 接收值
	for i := 0; i < 100; i++ {
		ch <- i
	}
	close(ch)
}

//从 ch1 中取出数据
//把结果发送到 ch2 中
func f2(ch1 <-chan int, ch2 chan<- int) {
	for {
		temp, ok := <-ch1
		//当能从 ch1 中取值时 ok 为 true，不能取值时为 false
		if !ok {
			break
		}
		ch2 <- temp
	}
	close(ch2)
}

```

