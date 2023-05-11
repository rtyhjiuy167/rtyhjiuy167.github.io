---
title: 29-4 URL查询字符串与POST主体的快速获取
top: 29.4
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
	//通过直接调用FormValue方法和PostFormValue方法直接获取请求参数的值 不需要r.ParseForm()
    //如果必要，本函数会隐式调用ParseMultipartForm和ParseForm
	fmt.Fprintln(w, "URL中的user请求参数的值是：", r.FormValue("username"))           //优先表单
	fmt.Fprintln(w, "Form表单中的username请求参数的值：", r.PostFormValue("username")) //仅表单
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

