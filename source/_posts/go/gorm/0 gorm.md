---
title: 0 gorm
top: 0
tags:
  - gorm
categories:
  - gorm
---

gorm中文文档：https://gorm.io/zh_CN/docs/

#### 普通的连接方式

```go
package main

import (
	"gorm.io/driver/mysql"
	"gorm.io/gorm"
)

type UserInfo struct {
	Name string
	Age  string
}

func main() {
	dsn := "root:123456@tcp(127.0.0.1:3306)/test?charset=utf8mb4&parseTime=True&loc=Local"
	db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{})
	if err != nil {
		panic(err)
	}
}
/*
    charset=utf8mb4 编码使用utf8mb4 可用于存表情
    parseTime=True
    loc=Local
*/
```

#### 可配置更多信息的连接方式

```go
package main

import (
	"gorm.io/driver/mysql"
	"gorm.io/gorm"
)

type UserInfo struct {
	Name string
	Age  string
}

func main() {
	dsn := "root:123456@tcp(127.0.0.1:3306)/test?charset=utf8mb4&parseTime=True&loc=Local"
	db, err := gorm.Open(mysql.New(mysql.Config{
		DSN: dsn,
	}), &gorm.Config{})
	if err != nil {
		panic(err)
	}
}
```