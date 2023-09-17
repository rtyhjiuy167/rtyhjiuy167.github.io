#### csrf

Egg 内置的 [egg-security](https://github.com/eggjs/egg-security/) 插件默认对所有『非安全』的方法，例如 `POST`，`PUT`，`DELETE` 都进行 CSRF 校验。

```js
const { Controller } = require('egg');

class HomeController extends Controller {
  async index() {
    const { ctx } = this;
    ctx.body = ctx.csrf;
  }
  async add() {
    const { ctx } = this;
    ctx.body = 'hi, egg';
  }
}

module.exports = HomeController;
```

使用`apifox`先以`GET`方式请求`http://localhost:7001/`得到csrf token，再以`POST`方式，`Params`为`_csrf=之前get得到的csrf token`请求`http://localhost:7001/add`

#### 关闭 csrf 安全验证

```js
config.security = {
    csrf: {
        // enable: false, 
        // 判断是否需要 ignore 的方法，请求上下文 context 作为第一个参数
        ignore: (ctx) => {
            if (ctx.request.url == "/admin/goods/uploadImg") {
                return true;
            }
            return false;
        },
    },
};
```

