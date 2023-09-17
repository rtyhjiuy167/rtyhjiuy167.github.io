---
title: 7-2 express-art-template
top: 7.2
tags:
  - nodejs
categories:
  - nodejs
---

```shell
npm i art-template --save 
npm i express-art-template --save
```

app.js

```javascript
var express = require('express');

var app = express();

//配置使用art-temple模板引擎
//第一个参数 当渲染以 .html 结尾的文件的时喉，使用art-temple模板引擎
//express-art-template是专门用来在Express中把art-template整合到Express
app.engine('html',require('express-art-template'))

//Express为Response相应的对象提供了一个方法：render
//render方法默认是不可以使用的，但是如果配置了模板引擎就可以使用了
//res.render('html模板名',{模板数据})
//  第一个参数不写路径，其是会去该文件所在目录中找views文件夹，进入该目录中查找该模板文件
app.use('/pubilc/', express.static('./public/'));

//如果想要修改默认的views目录，则可以
// app.set('views',render函数的默认路径)

app.get('/', (req, res) => {
    res.render('404.html',{
        message:'失联了QAQ'
    });
})

app.listen(3000, () => {
    console.log("3000端口监听中");
})
```

./views/404.html

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8"><!--服务端就可以不用写content-type-->
    </head>
    <body>
        {{message}}
    </body>
</html>
```

