---
title: 2 http
top: 2
tags:
  - nodejs
categories:
  - nodejs
---

```js
//1.加载 http核心模块
const http = require('http');
//2.使用http.createServer()方法创建一个Web服务器
// 其返回一个Server实例
const server = http.createServer();
//3.服务器要干嘛？
// 提供数据的服务

//3.1接收请求
// 注册request请求事件
// 当客户端请求过来，就会自动触发服务器的request请求事件，然后执行回调函数
//   request请求事件处理函数，需要接收两个参数
//     Request 请求对象
//          请求对象可以用来获取客户端的一些请求信息，例如请求路径
//     Response 响应对象
//     响应对象可以用来给客户端发送响应信息
server.on('request', (request, response) => {
    console.log("收到客户端的请求了，其请求路径是：http://127.0.0.1:3000" + request.url);

    /**
     * response 对象有一个方法:write可以用来给客户端发送响应数据
     *  write 可以使用多次，但是最后一定要使用end来结束响应，否则客户端会一直等待
     *      等待时不会呈递给用户
     */
    response.write('hello');
    response.write(' node js');
    response.end();
})

//4.绑定端口号，启动服务器 IP默认本地127.0.0.1
server.listen(3000, () => {
    console.log("启动成功");
});
```

```js
const http = require('http')


const server = http.createServer((req,res)=>{
   res.statusCode = 200;
   res.setHeader("Content-type",'text/plain;charset=utf-8');
   res.end("你好");
})

server.listen(9999,"127.0.0.1",()=>{
   console.log(`server running at http://127.0.0.1:9999`)
})


```

```js
const http = require('http')
const fs = require('fs')
const path = require('path')

const server = http.createServer((req,res)=>{;
   const fileName = path.basename(req.url);//找到 127.0.0.1:8080/后的地址
   const filePath = path.join(__dirname,fileName);
   console.log(`fileName:${fileName},filePath:${filePath}`);
   fs.readFile(filePath, (err, data) => {
      if (err) {
          res.setHeader('Content-Type', 'text/plain;charset=utf-8');
          res.end('文件读取失败，请稍后重试！');
      } else {
          res.setHeader('Content-Type', 'text/html;charset=utf-8');
          res.end(data);
      }
  })

})

server.listen(9999,"127.0.0.1",()=>{
   console.log(`server running at http://127.0.0.1:9999`)
})
```

