---
title: 32 flutter html post go 
top: 32
tags:
  - go
categories:
  - go
---

```dart
//1.前端 flutter post 数据
await Dio().post(Config.domain + '/addUser', data: {
        'username': username,
        'password': password,
        'email': email,
      });
```

```go
//1.后端 beego 接收数据
len := c.Ctx.Request.ContentLength
body := make([]byte, len)
c.Ctx.Request.Body.Read(body)
u := &models.User{} // 事先定义了一结构体 一定要加 & 不然之后获取不到数据
json.Unmarshal(body, u) // 将相应的数据解析到结构体中
```

```html
<!--2.前端 html post 数据-->
<form action="/user/doAdd" method="post">
        ID <input type="text" name="id"><br><br>
        用户名 <input type="text" name="username"><br><br>
        密 码   <input type="text" name="password"><br><br>
        爱 好 <input type="checkbox" value=1 id="lable1" name="hobby"> <label for="lable1">吃饭</label> <!--把value上传-->>
        <input type="checkbox" value=2 id="lable2" name="hobby"> <label for="lable2">睡觉</label>
        <input type="checkbox" value=3 id="lable3" name="hobby"> <label for="lable3">运动</label>
        <input type="submit" value="提交">
    </form>
```

```go
//2.后端 beego 接收数据
u := User{} //事先定义了一结构体
err := c.ParseForm(&u) //直接将相应的数据解析到结构体中
```

