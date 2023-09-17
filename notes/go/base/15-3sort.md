---
title: 15-3 sort
top: 15.3
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

	var que hp
	que.Push(1)
	que.Push(2)
	len := que.Len()

	fmt.Println(len)                                      // 2
	fmt.Printf("que %T:%v\n", que, que)                   //que main.hp:{[1 2]}
	fmt.Printf("que %T:%v\n", que.IntSlice, que.IntSlice) //que sort.IntSlice:[1 2]
	que.IntSlice = que.IntSlice[1:]                       //用que.IntSlice更改其内容
	fmt.Printf("que %T:%v\n", que, que)                   //que main.hp:{[2]}
}

type hp struct{ sort.IntSlice } //实现了sort.IntSlice接口 可以使用默认的Len Less Swap

func (h *hp) Push(v interface{}) {
	h.IntSlice = append(h.IntSlice, v.(int)) // v.(int) 表示只接收 int 类型
}

```

<h2>Sort包</h2>

```go
type Interface interface {
	Len() int

	Less(i, j int) bool

	Swap(i, j int)
}
```

```go
// int 类型的
type IntSlice []int

func (x IntSlice) Len() int           { return len(x) }
func (x IntSlice) Less(i, j int) bool { return x[i] < x[j] }
func (x IntSlice) Swap(i, j int)      { x[i], x[j] = x[j], x[i] }
```

```go
// float64 类型的
type Float64Slice []float64

func (x Float64Slice) Len() int { return len(x) }

func (x Float64Slice) Less(i, j int) bool { return x[i] < x[j] || (isNaN(x[i]) && !isNaN(x[j])) }
func (x Float64Slice) Swap(i, j int)      { x[i], x[j] = x[j], x[i] }
```

