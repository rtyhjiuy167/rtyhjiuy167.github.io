```
npm i jsonwebtoken
```

颁发：

```javascript
const jwt = require("jsonwebtoken");
// 第一个参数 res 加密的信息  第二个参数为加密的私钥  expiresIn 为有效期
jwt.sign(res, "secret", { expiresIn: "1h" })// s 秒 h 小时
```

验证：

```javascript
const jwt = require("jsonwebtoken");

const auth = async (ctx, next) => {
  const { token } = ctx.request.header;
  try {
    // 验证失败，会抛出错误
    const user = jwt.verify(token, "私钥");
    ctx.state.user = user;
  } catch (err) {
    switch (err.name) {
      case "TokenExpiredError":
        console.error("token已过期", err);
        return;
      case "JsonWebTokenError":
        console.error("无效的token", err);
        return;
    }
  }
  await next();
};
module.exports = {
  auth
};
```

