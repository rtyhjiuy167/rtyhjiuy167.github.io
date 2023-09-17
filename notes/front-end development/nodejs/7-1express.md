---
title: 7-1 express
top: 7.1
tags:
  - nodejs
categories:
  - nodejs
---

```javascript
var express = require('express');

//创建服务器应用程序
// 也就是原来的http.createServer
var app = express();

//当服务器收到get请求 / 的时候，执行回调函数
/**
 * 不同的请求路径在express框架中可分开写
 *  原生的写法不美观
 * 且content-type会自动加上 简洁明了
 *  请求路径不存在时会显示自动显示错误
 */

//公开指定目录
//当以/pubilc/开头时，去./public/目录中找对应的资源
app.use('/pubilc/', express.static('./public/'));
//当省略第一个参数时，直接在./public/目录中找对应的资源，如果仍以/pubilc/开头会报错

app.get('/', (req, res) => {

    //res.write('') res.end('')
    //原生的没有send
    res.send('hello express!');
})


//相当于server.listen
app.listen(3000, () => {
    console.log("3000端口监听中");
})
```

<h4>express模板引入</h4>

<h5>header.html</h5>

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
</head>

<body>
    {{include './header.html'}}
    <h1>hello</h1>
</body>

</html>
```

<h5>header.html</h5>

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
</head>

<body>
    {{include './header.html'}}
    <h1>hello</h1>
</body>

</html>
```

