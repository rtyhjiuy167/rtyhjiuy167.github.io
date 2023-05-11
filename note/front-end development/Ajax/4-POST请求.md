---
title: 4-POST请求
top: 4
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
    <div id="result"></div>
    <script>
        //获取button元素
        const result = document.getElementById("result");
        //绑定事件
        result.onmouseover = function () {
            //1.创建对象
            const xhr = new XMLHttpRequest();
            //2.初始化 设置请求方法和url
            xhr.open('POST', 'http://127.0.0.1:8000/server?a=100&b=200');
            //3.发送
            xhr.send();
            //参数设置 xhr.send('a=100&b=200');
            		//或： xhr.send('a:100&b:200');
            //4.事件绑定 处理服务端返回的结果
            //on When 当...的时候
            //readystate 是xhr对象中的属性，表示状态 其有五个值 
            //  0（未初始化） 1(open已调用完毕) 2(send调用完毕) 
            //  3(服务端返回了部分结果)  4(服务端返回了全部结果)
            //change 改变
            xhr.onreadystatechange = function () {
                //判断（当服务端返回了所有结果时）
                if (xhr.readyState === 4) {
                    //判断响应状态码
                    //状态码已2开头的都是成功的
                    if (xhr.status >= 200 && xhr.status < 300) {
                        //处理结果 行 头 空行 体
                        //1.响应行
                        console.log(xhr.status);//状态码 200
                        console.log(xhr.statusText);//状态字符串 OK
                        console.log(xhr.getAllResponseHeaders);//所有响应头
                        console.log(xhr.response);//响应体
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
app.get('/server', function (request, response) {
    //设置响应头 设置允许跨域
    response.setHeader('Access-Control-Allow-Origin', '*');
    //设置响应体
    response.send('HELLO EXPRESS');
});

app.post('/server', function (request, response) {
    //设置响应头 设置允许跨域
    response.setHeader('Access-Control-Allow-Origin', '*');
    //设置响应体
    response.send('HELLO EXPRESS POST');
});

//4.监听端口启动服务
app.listen(8000, function () {
    console.log("服务器已经启动，8000端口监听中...")
})
```

