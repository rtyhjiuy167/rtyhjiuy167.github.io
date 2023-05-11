---
title: 12 JQuery发送请求
top: 12
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
</head>

<body>
    <h2>JQuery发送AJAX请求</h2>
    <button id="btn1">GET</button>
    <button id="btn2">POST</button>
    <button id="btn3">通用型方法ajax</button>
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
    <script>
        $('#btn1').click(function () {
            $.get('http://127.0.0.1:8000/jquery-server', { a: 100, b: 200 }, function (data) {
                console.log(data);
            });
        })
        $('#btn2').click(function () {
            $.post('http://127.0.0.1:8000/jquery-server', { a: 100, b: 200 }, function (data) {
                console.log(data);
            }, 'json');
        })
        $('#btn3').click(function () {
            $.ajax({
                //url
                url: 'http://127.0.0.1:8000/jquery-server',
                //参数
                data: { a: 100, b: 200 },
                //请求类型
                type: 'GET',
                //响应体结果
                dataType: 'json',
                //成功的回调
                success: function (data) {
                    console.log(data);
                },
                //超时时间
                timeout:1000,
                //失败的回调
                error: function () {
                    console.log("出错了")
                },
                //头信息
                headers:{
                    c:300,
                    d:400
                }
            })
        })
    </script>

</body>

</html>
```

```js
const express = require('express');

const app = express();

app.all('/jquery-server', function (request, response) {
    response.setHeader('Access-Control-Allow-Origin', '*');
    response.setHeader('Access-Control-Allow-Headers', '*');
    const data = {
        name: 'noName'
    };
    response.send(JSON.stringify(data));
});

app.listen(8000, function () {
    console.log("服务器已经启动，8000端口监听中...")
})
```

