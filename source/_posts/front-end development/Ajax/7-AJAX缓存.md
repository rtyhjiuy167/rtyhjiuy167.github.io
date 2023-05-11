---
title: 7 AJAX缓存
top: 7
tags:
  - Ajax
categories:
  - Ajax
---

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <style>
        #result {
            width: 200px;
            height: 100px;
            border: solid 1px #90b;
        }
    </style>
</head>

<body>
    <button id="btn">点击发送请求</button>
    <div id="result"></div>
    <script>
        const btn = document.getElementById("btn");
        const result = document.getElementById("result");
        //绑定事件
        
        btn.onclick = function () {
            const xhr = new XMLHttpRequest();
            //解决因AJAX缓存无法显示最新数据问题
            xhr.open('GET', 'http://127.0.0.1:8000/ie?t='+Date.now());
            xhr.send();
            xhr.onreadystatechange = function () {
                if (xhr.readyState === 4) {
                    if (xhr.status >= 200 && xhr.status < 300) {
                        result.innerHTML = xhr.response;
                    }
                }
            };
        };
    </script>

</body>

</html>
```



```js
//1.引入express
const express = require('express');

//2.创建应用对象
const app = express();

//3.创建路由规则
//request 是对请求报文的封装
//response 是对响应报文的封装

//可以接受任意类型的请求
app.get('/ie', function (request, response) {
    //设置响应头 设置允许跨域
    response.setHeader('Access-Control-Allow-Origin', '*');
    response.send('HELLO IE2' );
});

//4.监听端口启动服务
app.listen(8000, function () {
    console.log("服务器已经启动，8000端口监听中...")
})
```

