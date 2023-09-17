---
title: 11 express art-template 模板引入与继承
top: 11
tags:
  - nodejs
categories:
  - nodejs
---

<h5>app.js</h5>

```js
const express = require('express');
const path = require('path');
const app = express();

app.use('/public', express.static(path.join(__dirname, './public')));
app.use('/node_modules', express.static(path.join(__dirname, './node_modules')));

app.engine('html', require('express-art-template'))

//默认
app.set('views', path.join(__dirname, './views/'))

app.get('/', (req, res) => {
    res.render("index.html",{
        name:'张三'
    })
})

app.listen(1234,()=>{
    console.log("启动中")
})
```

<h5>header.html</h5>

```html
<div>
    <h1>公共的头部</h1>
</div>
```

<h5>footer.html</h5>

```html
<div>
    <h1>公共的底部</h1>
</div>
```

<h4>模板引入</h4>

<h5>index.html</h5>

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
</head>

<body>
    {{include './header.html'}}
    <h1>hello</h1>
    {{include './footer.html'}}
</body>

</html>
```

<h4>模板继承</h4>

<h5>index.html</h5>

```html
{{extend './layout.html'}}
{{ block 'content'}}
<div>
    <h1>填坑内容</h1>
</div>
{{ /block}}
```

<h5>layout.html</h5>

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <link rel="stylesheet" href="/node_modules/bootstrap/dist/css/bootstrap.css">
    {{block 'head'}}{{ /block}}
</head>

<body>
    {{include './header.html'}}
    {{ block 'content'}}
    <h1>默认内容</h1>
    <!--这里写的内容都是无法被继承过去的，在这里写只是为了方便查看-->
    {{ /block}}
    {{include './footer.html'}}
    <script src="/node_modules/bootstrap/dist/js/bootstrap.js"></script>
    <script src="/node_modules/jquery/dist/jquery.js"></script>
    {{block 'script'}}{{ /block}}
</body>

</html>
```

