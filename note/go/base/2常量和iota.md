---
title: 2 常量和iota
top: 2
tags:
  - go
categories:
  - go
---

```go
package main //入口文件
import "fmt"

//批量声明常量时，如果某一行声明后没有赋值，默认就和上一行一致
const (
	a1 = 100
	a2 //n2=100
	a3 //n3=100
)

//iota是go语言的常量计数器，只能在常量的表达式中使用。
//iota在const关键字出现时将被重置为0。
//const中每新增一行常量声明将使iota计数一次(iota可理解为const语句块中的行索引)。
//使用iota能简化定义，在定义枚举时很有用
const (
	b1 = iota //0
	b2        //1
	b3        //2
)

const (
	c1 = iota //0
	c2 = 100  //100
	c3 = iota //2
	c4        //3
)
const c5 = iota //0

//多个iota定义在一行
const (
	d1, d2 = iota + 1, iota + 2 //1,2
	d3, d4                      //2,3
)

//使用_跳过某些值
const (
	e1 = iota //0
	e2        //1
	_
	e4 //3
)

/*
定义数量级
这里的<<表示左移操作，1<<10表示将1的二进制表示向左移10位，也就是由1变成了10000000000，也就是十进制的1024。
同理2<<2表示将2的二进制表示向左移2位，也就是由10变成了1000，也就是十进制的8
*/
const (
	_  = iota
	KB = 1 << (10 * iota)
	MB = 1 << (10 * iota)
	GB = 1 << (10 * iota)
	TB = 1 << (10 * iota)
	PB = 1 << (10 * iota)
)

//语句只能放在函数里
func main() {

	fmt.Println("a1:", a1)
	fmt.Println("a2:", a2)
	fmt.Println("a3:", a3)
	fmt.Println("b1:", b1)
	fmt.Println("b2:", b2)
	fmt.Println("b3:", b3)
	fmt.Println("c1:", c1)
	fmt.Println("c2:", c2)
	fmt.Println("c3:", c3)
	fmt.Println("c4:", c4)
	fmt.Println("c5:", c5)
	fmt.Println("d1:", d1)
	fmt.Println("d2:", d2)
	fmt.Println("d4:", d4)
	fmt.Println("e1:", e1)
	fmt.Println("e2:", e2)
	fmt.Println("e4:", e4)
}

```

