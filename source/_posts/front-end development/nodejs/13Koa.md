*Koa* (*koa*js) 是express下一代的新的 web 框架

```
 npm i koa -S
```

```js
const Koa = require('koa');
const app = new Koa();
app.use(async(ctx)=>{
   let url = ctx.url;
   let request = ctx.request;
   let req_query = request.query; // 接收到请求参数 以对象形式返回
   let req_querystring=request.querystring; // 接收到请求参数 以字符串形式返回
   ctx.body={
      url,
      req_query,
      req_querystring
   }
})

app.listen(3000,()=>{
   console.log("[demo] server is starting at port 3000");
})

```

```js
const Koa = require('koa')
const app = new Koa();

// ctx 上下文对象
app.use(async(ctx,next)=>{
   console.log("1");
   await next(); // 跳到下个中间件
   console.log("3");
   await next();
   console.log("cc");
})
 
app.use(ctx=>{
   console.log("2");
   ctx.body="hello";
})

app.listen(9999,"127.0.0.1",()=>{
   console.log(`server running at http://127.0.0.1:9999`)
})


```

```js
// 使用 Koa-static 中间件 ，对静态资源可进行访问设置
const Koa = require('koa');
const serve = require('koa-static');
const path = require('path');
const app = new Koa();
app.use(serve(path.join(__dirname))) // 对指定路径进行开放

app.listen(9999,"127.0.0.1",()=>{
   console.log(`server running at http://127.0.0.1:9999`)
})


```

```js
const Koa = require('koa');
const http = require('http');
const serve = require('koa-static');
const path = require('path');
const SocketIo = require('socket.io');
const port = 3000;
const hostname="127.0.0.1";

//创建 koa 实例对象 
const app = new Koa();
// 获取 server 实例对象
const server = http.createServer(app.callback());
// 获取socket 实例对象
const io = SocketIo(server);
//注册静态服务器 一定要放在 socketIo 后面
app.use(serve(path.join(__dirname)))

app.listen(port,hostname,()=>{
   console.log(`server running at http://127.0.0.1:9999`)
})


```

