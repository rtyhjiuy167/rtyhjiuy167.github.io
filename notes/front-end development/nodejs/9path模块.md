---
title: 9 path模块
top: 9
tags:
  - nodejs
categories:
  - nodejs
---

```js
const express = require('express');
const path = require('path');
const app = express();
/**
 * path.basename
 *  获取一个路径的文件名（默认包含扩展名）
 * path.dirname
 *  获取一个路径中的目录部分
 * path.extname
 *  获取一个路径中的扩展名部分
 * path.parse
 *  把一个路径转为对象
 *      root根路径
 *      dir目录
 *      base包含后缀名的文件名
 *      ext后缀名
 *      name不包含后缀名的文件名
 */
app.use('/public', express.static(path.join(__dirname, './public')));
/**
 * app.use('/public',express.static(path.join(__dirname,'./public')));
 *      与app.use('/pubilc/', express.static('./public/'));的差别
 * 前者可以把'./public'即相对路径改为绝对路径
 */
/**
 * path.join()可以自动拼接路径，避免手动拼接出错
 */
app.use('/node_modules', express.static(path.join(__dirname, './node_modules')));

app.get('/', (req, res) => {
    res.send("hello")
})

app.listen(1234)
```

