---
title: 11 发送AJAX请求
top: 11
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
    <button id="btn3">ajax</button>
    <script src="https://cdn.bootcdn.net/ajax/libs/axios/0.21.1/axios.min.js"></script>
    <script>
        const btns = document.querySelectorAll('button');
        //配置 baseURL 可在之后简写url
        axios.defaults.baseURL = 'http://127.0.0.1:8000';
        btns[0].onclick = () => {
            axios.get('/axios-server', {
                //url参数
                params: {
                    id: 100,
                    vip: 7
                },
                //请求头信息
                headers: {
                    name: 'nihao'
                }
            }).then((data) => {
                console.log(data)
            })
        }

        btns[1].onclick = () => {
            axios.post('/axios-server', {
                usename: 'myName'//请求体
            }, {
                //url参数
                params: {
                    id: 200,
                    vip: 9
                },
                //请求头信息
                headers: {
                    height: 180
                }
            })
        }

        btns[2].onclick = () => {
            axios(
                {
                    method: 'POST',
                    url: '/axios-server',

                    //url参数
                    params: {
                        id: 200,
                        level: 1
                    },
                    //请求头信息
                    headers: {
                        a: 180,
                        b: 200,
                    },
                    //请求体
                    data: {
                        usename: 'myName'
                    }
                }).then(response => {
                    //响应状态码
                    console.log(response.status);
                    //响应状态字符串
                    console.log(response.statusText);
                    //响应头信息
                    console.log(response.headers);
                    //响应体
                    console.log(response.data);
                })
        }
    </script>

</body>

</html>
```

```js
const express = require('express');

const app = express();

app.all('/axios-server', function (request, response) {
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

