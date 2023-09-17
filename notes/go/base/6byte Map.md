---
title: 6 byte Map
top: 6
tags:
  - go
categories:
  - go
---

```go
package main

import (
	"fmt"
)

func main() {

	// ASCII 字符表
	//   0 ~ 9   :  48 ~ 57
	a1 := '0' + byte(1)
	fmt.Println("a1:", a1) //a1: 49
	a2 := '0' + 1
	fmt.Println("a2:", a2) //a1: 49
	a3 := make([]byte, 5, 10)
	a3[0] = 45
	a3[1] = '0'
	a3[2] = '1'
	a3[3] = '0' + 1
	a3[4] = '0' + byte(9)
	fmt.Println("a3:", a3, "string(a3):", string(a3))
	//a3: [45 48 49 49 57] string(a3): -0119

}
/* Go语言的基本类型有：

   bool
   string
   int、int8、int16、int32、int64
   uint、uint8、uint16、uint32、uint64、uintptr
   byte // uint8 的别名 代表了 ASCII 码的一个字符
   rune // int32 的别名 代表一个 Unicode 码
   float32、float64
   complex64、complex128
*/
```

```go
//Map(集合) 
// Map 是一种无序的键值对的集合  Map 是使用 hash 表来实现的

/* 声明变量，默认 map 是 nil */
var map1 map[string]string //键与值都为字符串型 //此时仅创建，还没有初始化
map1 = make(map[string]string) // 初始化  其实该两句可以合并为一句
// map1 := make(map[int]string)  //    map1 := map[int]string{} // 两者等价？
map1 [ "name" ] = "张三" // 在map里插入键值对
delete(map1, "name")  // 在map里删除键值对
val, ok := map1["name"] // 尝试在map1里找键为"name"的值，有则返回值和 true,没有则返回初始值与 false
```

