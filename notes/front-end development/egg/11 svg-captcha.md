```bash
 npm install svg-captcha
```

```js
const svgCaptcha = require('svg-captcha');
 
var captcha = svgCaptcha.create();
console.log(captcha);
```

```js
const { Controller } = require("egg");
const svgCaptcha = require("svg-captcha");
class HomeController extends Controller {
  async index() {
    const { ctx } = this;
    await ctx.render("default/index");
  }
  async captcha() {
    const { ctx } = this;
    const captcha = svgCaptcha.create({
      size: 4,
      fontSize: 50,
      width: 100,
      height: 40,
      background: "#cc9966",
    });
    // 将验证码保存到session
    ctx.session.code = captcha.text;
    // 指定返回的类型
    ctx.response.type = "image/svg+xml";
    // 返回一张图片
    ctx.body = captcha.data;
  }
}
module.exports = HomeController;
```

