```bash
npm uninstall egg-view-nunjucks
npm install egg-view-ejs
```

`config/plugin.js`：

```js
module.exports = {
  static: {
    enable: true
  },
  // 删掉 nunjucks
  // nunjucks: {
  //   enable: true,
  //   package: 'egg-view-nunjucks'
  // },
  ejs:{
    enable: true,
    package: 'egg-view-ejs'
  }
}
```

`config/config.default.js`：

```js
const userConfig = {
    view: {
        mapping: {
            '.html': 'ejs'
        }
    }
}
```

