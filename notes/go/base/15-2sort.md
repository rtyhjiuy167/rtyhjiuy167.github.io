---
title: 15-2 sort
top: 15.2
tags:
  - go
categories:
  - go
---

```go
package main

import (
	"fmt"
	"sort"
)

func main() {

	intList := []int{2, 1, 3, 5, 7, 6, 9, 8, 4, 0}
	sort.Sort(Reverse{sort.IntSlice(intList)}) //sort.Sort(sort.Reverse(sort.IntSlice(intList)))
	fmt.Printf("%v\n", intList)
}

//sort 包中有一个 sort.Interface 接口，该接口有三个方法 Len() 、 Less(i,j) 和 Swap(i,j)

// 自定义的 Reverse 类型  自定义的是一结构体
type Reverse struct {
	sort.Interface
	// 这样，Reverse可以接纳任何实现了sort.Interface的对象
	// sort.IntSlice(切片或数组) 即为实现了sort.Interface的对象
}

// Reverse 只是将其中的 Inferface.Less 的顺序对调了一下
// Less是一比较函数 返回的是布尔值
func (r Reverse) Less(i, j int) bool {
	return r.Interface.Less(j, i)
	//在调用sort.Sort() 时 Less() 是会被执行的
}

```

