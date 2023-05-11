---
title: 2 express
top: 2
tags:
  - Ajax
categories:
  - Ajax
---

# HTTP

HTTP （hypertext transport protocol）协议[超文本传输协议]，协议详细规定了浏览器和万维网服务器之间互相通信的规则

## 请求报文

```
行   
头
空行
体
```

## 响应报文

```
行
头
空行
体
```

```
npm init --yes//在最外层文件夹中打开终端输入

npm i express//之后安装express框架  
```

```js
//1.引入express
//express是第三方web开发框架
// 其高度封装了http模块 更加专注于业务，而非底层细节
const express = require('express');
//先从该文件所在目录找node_modules文件夹，之后在该文件夹中找express文件夹，之后在该文件夹中找package.json文件，package.json中的main之后的文件名即为真正加载的代码，如果main的是错的，则找index.js
// 找不到node_modules,则往上一级查找，直至根目录

//2.创建应用对象
const app = express();

//3.创建路由规则
//request 是对请求报文的封装
//response 是对响应报文的封装
app.get('/', function (request, response) {
    //设置响应
    response.send('HELLO EXPRESS');
});

//4.监听端口启动服务
app.listen(8000, function () {
    console.log("服务器已经启动，8000端口监听中...")
})

```

