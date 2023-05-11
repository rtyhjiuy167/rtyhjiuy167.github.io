---
title: 17-2 work pool
top: 17.2
tags:
  - go
categories:
  - go
---

```go
package main

import (
	"fmt"
	"time"
)

//work pool

func worker(id int, jobs <-chan int, results chan<- int) {
	for job := range jobs {
		fmt.Printf("worker:%d start job%d\n", id, job)
		results <- job * 2 //相对于取走了一个任务
		time.Sleep(time.Millisecond * 500)
		fmt.Printf("worker:%d stop job%d\n", id, job)
	}
}

func main() {
	jobs := make(chan int, 100)
	results := make(chan int, 100)

	//开启3个 goroutine
	for j := 0; j < 3; j++ {
		go worker(j, jobs, results)
	}

	//发送5个任务
	for i := 0; i < 5; i++ {
		jobs <- i
	}
	close(jobs)

	//输出结果  用 for range 自动判断必须是在 通道已关闭的情况下
	for i := 0; i < 5; i++ {
		ret := <-results
		fmt.Println(ret)
	}
}

```

