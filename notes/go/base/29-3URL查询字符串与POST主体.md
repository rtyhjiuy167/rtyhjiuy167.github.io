---
title: 29-3 URL查询字符串与POST主体
top: 29.3
tags:
  - go
categories:
  - go
---

```go
package main

import (
	"fmt"
	"net/http"
)

//创建处理器函数 用于处理请求
func handler(w http.ResponseWriter, r *http.Request) {
	//如果form表单的action属性的URL地址中也有与form表单参数名相同的请求参数，
	//那么参数值都可以得到，并且form 表单中的参数值在URL的参数值的前面
	r.ParseForm()
	//获取请求参数
	fmt.Fprintln(w, "请求参数有:", r.Form)
	//r.PostForm 拿到不是action的，是Form表单的
	//PostForm只支持 application/x-www-form-urlencoded 若不为者，则需要使用MultipartForm
	fmt.Fprintln(w, "POST请求的form表单中的请求参数有:", r.PostForm)
}

func main() {
	http.HandleFunc("/hello", handler)
	http.ListenAndServe(":8080", nil)
}

```

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
    </head>
    <body>
        <form action="http://localhost:8080/hello?username=actionname&password=666" method="POST" enctype="application/x-www-form-urlencoded">
            <!--"application/x-www-form-urlencoded" 默认 传文件是则为 "multipart/form-data"  编码用的-->
        用户名：<input type="text" name="username" id=""><br>
        密码：<input type="text" name="password" id="">
        <input type="submit">
        </form>
    </body>
</html>
```

