---
title: 3 GORM 配置
top: 3
tags:
  - gorm
categories:
  - gorm
---

#### GORM 配置

```go
package main

import (
	"strings"

	"gorm.io/driver/mysql"
	"gorm.io/gorm"
	"gorm.io/gorm/schema"
)

type UserInfo struct {
	Name string
	Age  string
}

func main() {
	dsn := "root:123456@tcp(127.0.0.1:3306)/test?charset=utf8mb4&parseTime=True&loc=Local"
	db, err := gorm.Open(mysql.New(mysql.Config{
		DSN: dsn,
	}), &gorm.Config{
		SkipDefaultTransaction: true,
		NamingStrategy: schema.NamingStrategy{
			TablePrefix:   "t_",                              // 用于添加表名前缀，`UserInfo`表为`t_user_infos`
			SingularTable: true,                              // 使用单数表名，启用该选项后，`user_infos` 表将是`user_info`
		},
		DisableForeignKeyConstraintWhenMigrating: true, // 禁止物理外键约束，一般使用逻辑外键来进行约束，可提高运行速度
	})
	if err != nil {
		panic(err)
	}

	// 创建表 自动迁移 （把结构体和数据表对应起来）
	db.AutoMigrate(&UserInfo{})

	u1 := UserInfo{"小明", "18"}
	//创建数据行
	db.Create(&u1)
}
```