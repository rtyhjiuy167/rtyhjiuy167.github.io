---
title: 27-1 mysql查询
top: 27.1
tags:
  - go
categories:
  - go
---

<h2>web01_db</h2>

<h3>model</h3>

<h4>user_test.go</h4>

```go
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
	//t.Run("开始了哦", testAddUser) //t.Run 的第一个参数无作用 仅给程序员看的
	//t.Run("测试获取一个用户", testGetUserById)
	t.Run("测试获取所有用户", testGetUsers)
}

//若函数不是以Test开头（区分大小写），那么该函数默认不执行
//我们可以将他设置成一个子测试函数
func testAddUser(t *testing.T) {
	fmt.Println("测试添加用户：")
	//user := &User{}
	//调用添加用户的方法
	//user.AddUser()
	//user.AddUser2()
}

//测试获取一个User
func testGetUserById(t *testing.T) {
	fmt.Println("测试查询一条记录")
	user := User{
		ID: 1,
	}
	//调用获取User的方法
	u, err := user.GetUserById()
	if err != nil {
		fmt.Println("user.GetUserById err：", err)
	}
	fmt.Println("得到了：", u)
}

//测试获取所有User
func testGetUsers(t *testing.T) {
	fmt.Println("测试获取所有User")
	user := &User{}
	us, err := user.GetUsers()
	if err != nil {
		fmt.Println("user.GetUsers err：", err)
	}
	for key, value := range us {
		fmt.Printf("第%v个用户是%v：\n", key+1, value)
	}
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
	//1.写sql语句 ? 相当于占位符 在Exec里补上
	sqlStr := "insert into users(username,password,email) values(?,?,?)"
	//2.执行 写完sql语句后记执行，调用的是utils.Db 里的Exec 方法
	_, err := utils.Db.Exec(sqlStr, "admin5", "123456", "1122@qq.com")
	if err != nil {
		fmt.Println("执行出现异常：", err)
		return err
	}
	return nil
}

//GetUserById 根据用户的id从数据库中查看一条记录
func (user *User) GetUserById() (*User, error) {
	//写sql语句 ? 相当于占位符 在QueryRow里补上
	sqlStr := "select id,username,password,email from users where id = ?"
	//执行
	row := utils.Db.QueryRow(sqlStr, user.ID)
	//声明
	var id int
	var username string
	var password string
	var email string
	err := row.Scan(&id, &username, &password, &email)
	if err != nil {
		return nil, err
	}
	u := &User{
		ID:       id,
		Username: username,
		Email:    email,
	}
	return u, nil
}

//GetUsers 获取数据库中所有的记录
func (user *User) GetUsers() ([]*User, error) {
	//写sql语句
	sqlStr := "select id,username,password,email from users"
	//执行
	rows, err := utils.Db.Query(sqlStr)
	if err != nil {
		return nil, err
	}
	//创建User切片
	var users []*User

	//Next准备用于Scan方法的下一行结果。如果成功会返回真，如果没有下一行或者出现错误会返回假。 Err应该被调用以区分这两种情况。
	//每一次调用Scan方法，甚至包括第一次调用该方法，都必须在前面先调用Next方法。
	for rows.Next() {
		//声明
		var id int
		var username string
		var password string
		var email string
		err := rows.Scan(&id, &username, &password, &email)
		if err != nil {
			return nil, err
		}
		u := &User{
			ID:       id,
			Username: username,
			Email:    email,
		}
		users = append(users, u)
	}
	return users, nil
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

