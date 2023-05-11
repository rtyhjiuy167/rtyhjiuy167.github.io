文档地址：https://www.eggjs.org/zh-CN/core/cookie-and-session#cookie-%E7%A7%98%E9%92%A5

## cookie

```js
ctx.cookies.set(key, value, options)
ctx.cookies.get(key, options)
```

#### 过期时间

```
ctx.cookies.set(key,value,options)
```

`{Number} maxAge`: 设置这个键值对在浏览器的最长保存时间。是一个从服务器当前时刻开始的毫秒数。

`{Date} expires`: 设置这个键值对的失效时间，如果设置了 maxAge，expires 将会被覆盖。如果 maxAge 和 expires 都没设置，Cookie 将会在浏览器的会话失效（一般是关闭浏览器时）的时候失效

#### encrypt

`{Boolean} encrypt`：设置是否对 Cookie 进行加密，如果设置为 true，则在发送 Cookie 前会对这个键值对的值进行加密，客户端无法读取到 Cookie 的明文值。默认为 false。

如果要获取已加密的`cookies`，需要在`options`中添加`encrypt:true`

#### httpOnly

`{Boolean} httpOnly`: 设置键值对是否可以被 js 访问，默认为 true，不允许被 js 访问。只允许后端访问，不允许在前端访问

#### signed

`{Boolean} signed`：设置是否对 Cookie 进行签名，如果设置为 true，则设置键值对的时候会同时对这个键值对的值进行签名，后面取的时候做校验，可以防止前端对这个值进行篡改。默认为 true。

#### 中文

方法一：加密，加密后可以设置中文。

方法二：转成`base64`字符串。

```js
// 转成 base64
new Buffer.from('中文').toString('base64')
// 还原
new Buffer.from('5Lit5paH', 'base64').toString()
```

#### 清除 cookie

```js
ctx.cookies.set(key, null);
```

## session

session是另一种记录客户状态的机制，不同的是Cookie 保存在客户端浏览器中，而session保存在服务器上（session 的键返回给客户端，session 的值保存在服务端）。

服务器端会创建一个session对象，生成一个类似于key、value的键值对，然后将 key 返回给客户端（一般还会给`key`起个名字）。

`Response Headers`中的`set-cookie`会自动执行保存，并在之后发送请求时附上。

```
ctx.session.key = value;
```

在`config.default.js`中进行`session`配置：

```js
config.session = {
    key: "SESSION_ID", // 设置客户端保存的 session key 的key
    maxAge: 30 * 60 * 1000, // 有效时间
    renew: true, // 每次刷新页面重置有效时间
};
```

注意：Serverless 中使用 session 需要注意 Serverless 容器的冷启动和热启动。冷启动相比热启动稍微要慢一些，因为冷启动的时候需要创建容器。当我们的程序长时间没有人访问的时 候运行我们程序的 Docker 容器会被销毁。Docker 容器销毁后重新访问我们的程序的时候又 会执行冷启动。Docker 容器的重启会导致 session 丢失。所以我们在 Serverless 中使用 session 的时候要注意云厂商 Docker 容器默认销毁的时间。腾讯云的 Serverless 云函数如果 30 分钟 内没有访问的话就会销毁容器。所以如果你的 session 要保存 30 分钟以上的话就需要把 session 存储到 redis 或者数据库中