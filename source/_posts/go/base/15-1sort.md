---
title: 15-1 sort
top: 15.1
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

/*
可以理解为：
	sort.IntSlice() 方法 把接收的 数组或分片 转成接口
	而 sort.Reverse() 方法 对 实现了 sort.Inferface 接口的对象 调整了将要排序的顺序 实际上是改变了 Interface.Less 方法
	最后以sort.Sort() 方法 开始排序

	相对的 sort.Ints() sort.Float64s() sort.Strings()
		 可以理解为特殊的排序方法
*/

func main() {
	intList := []int{2, 1, 3, 5, 7, 6, 9, 8, 4, 0}
	float64List := []float64{4.2, 5.9, 6.3, 10.1, 52.4, 1.9, 31.4, 27.8, 3.14}
	stringList := []string{"a", "y", "q", "c", "b", "r", "z", "x", "s", "y"}
	//升序
	sort.Ints(intList)         //int类型的升序
	sort.Float64s(float64List) //float64类型的升序
	sort.Strings(stringList)   //string类型的升序
	fmt.Printf("%v\n%v\n%v\n", intList, float64List, stringList)
	//降序
	sort.Sort(sort.Reverse(sort.IntSlice(intList)))
	sort.Sort(sort.Reverse(sort.Float64Slice(float64List)))
	sort.Sort(sort.Reverse(sort.StringSlice(stringList)))

	fmt.Printf("%v\n%v\n%v\n", intList, float64List, stringList)

}

```

```
[0 1 2 3 4 5 6 7 8 9]
[1.9 3.14 4.2 5.9 6.3 10.1 27.8 31.4 52.4]
[a b c q r s x y y z]
[9 8 7 6 5 4 3 2 1 0]
[52.4 31.4 27.8 10.1 6.3 5.9 4.2 3.14 1.9]
[z y y x s r q c b a]
```

