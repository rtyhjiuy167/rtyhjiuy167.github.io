---
title: 3 运算符 string
top: 3
tags:
  - go
categories:
  - go
---

```go
package main

import "fmt"

var (
	a = 10
	b = 20
)

func main() {
	fmt.Println("a", a)
	//单独的语句
	a++ //不能放在 = 的右边赋值 b = a++ 会报错

	//关系运算符
	fmt.Println(a == b) //Go语言是强类型的，相同类型的变量才能比较 32位的只能与32位比较

	num1 := 123
	fmt.Println(num1 / 10) //12
	fmt.Println(num1 % 10) //3
}

```

```go
package main

import "fmt"

func main() {
	//字符串
	str := "1234"
	fmt.Printf("%v %T\n", str, str)       // 1234 string
	fmt.Printf("%v %T\n", str[1], str[1]) // 50 uint8
	for i, v := range str {
		fmt.Printf("%v %v\n", i, v)
	}
}
```

