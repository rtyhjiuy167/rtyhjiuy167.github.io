---
title: 27-2 操作与测试mysql
top: 27.2
tags:
  - go
categories:
  - go
---

<h2>web01_db</h2>

<h3>model</h3>

<h4>user_test.go</h4>

```go
// 文件必须以_test.go结尾 该文件包含 `TestXxx` 函数 
// 且将该文件放在与被测试的包相同的包中
// 最后在model 目录下打开cmd 执行 go test 或 go test -v
package model

import (
	"fmt"
	"testing"
)

//TestMain函数可以在测试函数执行前做一些其它的操作
//若有TestMain 函数则仅会执行该函数
func TestMain(m *testing.M) {
	fmt.Println("测试开始！")
	//要想执行测试函数。可通过m.Run()来执行测试函数
	m.Run()
}

//测试函数 在无TestMain 函数时会自动执行
func TestXxx(t *testing.T) {
	t.Run("开始了哦", testAddUser) //t.Run 的第一个参数无作用 仅给程序员看的
}

//若函数不是以Test开头（区分大小写），那么该函数默认不执行
//我们可以将他设置成一个子测试函数
func testAddUser(t *testing.T) {
	fmt.Println("测试添加用户：")
	user := &User{}
	//调用添加用户的方法
	user.AddUser()
	user.AddUser2()
}

```

<h4>user.go</h4>

```go
package model

import (
	"1/customerManage/webapp/web01_db/utils"
	"fmt"
)

type User struct {
	ID       int
	Username string
	Password string
	Email    string
}

//adduser 添加User的方法
func (user *User) AddUser() error {
	//1.写sql语句
	sqlStr := "insert into users(username,password,email) values(?,?,?)"
	//2.预编译
	inStmt, err := utils.Db.Prepare(sqlStr)
	if err != nil {
		fmt.Println("预编译出现异常：", err)
		return err
	}
	//3.执行 预编译后调用的是inStmt 里的Exec 方法
	_, err = inStmt.Exec("admin4", "123456", "1122@qq.com")
	if err != nil {
		fmt.Println("执行出现异常：", err)
		return err
	}
	return nil
}

func (user *User) AddUser2() error {
	//1.写sql语句
	sqlStr := "insert into users(username,password,email) values(?,?,?)"
	//2.执行 写完sql语句后记执行，调用的是utils.Db 里的Exec 方法
	_, err := utils.Db.Exec(sqlStr, "admin5", "123456", "1122@qq.com")
	if err != nil {
		fmt.Println("执行出现异常：", err)
		return err
	}
	return nil
}

```

<h3>utils</h3>

<h4>db.go</h4>

```go
package utils

import (
	"database/sql"
	"fmt"

	_ "github.com/go-sql-driver/mysql"
)

var (
	Db  *sql.DB
	err error
)

func init() {
	Db, err = sql.Open("mysql", "root:@tcp(localhost:3306)/test") //
	if err != nil {
		fmt.Println("连接错误")
		panic(err.Error())
	}
}

```

