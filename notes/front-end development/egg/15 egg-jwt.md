https://www.npmjs.com/package/egg-jwt

```bash
npm i egg-jwt
```

`/config/plugin.js`：

```js
module.exports = {
  jwt: {
    enable: true,
    package: "egg-jwt",
  },
};
```

```js
module.exports = (appInfo) => {
    const config = {};
    config.jwt = {
        secret: "123456",
    };
	//...
};
```

