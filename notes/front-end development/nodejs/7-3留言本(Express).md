---
title: 7-3 留言本(Express) 简
top: 7.3
tags:
  - nodejs
categories:
  - nodejs
---

<h5>app.js</h5>

```javascript
const express = require('express');

const app = express();
app.use(express.urlencoded({ extended: false }))

let comments = [
    {
        name: '张1',
        message: '今天天气不错！',
        dataTime: '2021/6/22'
    },
    {
        name: '张2',
        message: '今天天气不错！',
        dataTime: '2021/6/24'
    },
    {
        name: '张3',
        message: '今天天气不错！',
        dataTime: '2021/6/25'
    },
    {
        name: '张4',
        message: '今天天气不错！',
        dataTime: '2021/6/26'
    },
];

app.engine('html', require('express-art-template'))


app.use('/public/', express.static('./public/'));



app.get('/', (req, res) => {
    res.render('index.html', {
        comment: comments
    });
})

app.get('/post', (req, res) => {
    res.render('post.html');
})//没有模板数据就不传第二个参数





app.get('/pinglun', (req, res) => {
    const url = new URL('http://127.0.0.1:3000' + req.url);
    const params = url.searchParams;
    let obj = {};
    for (const [name, value] of params) {
        obj[name] = value;
    }
    obj.dataTime = '2021/6/28';
    comments.unshift(obj);
    /**
     * res.statusCode = 302;
     * res.setHeader = ('Location','/');
     */
    //重定向 Express以封装过了，可简写
    res.redirect('/')
})

app.post('/pinglun', (req, res) => {
    let obj = req.body;
    obj.dataTime = '2021/6/28';
    comments.unshift(obj);
    res.redirect('/')
})


app.listen(3000, () => {
    console.log("3000端口监听中");
})
```

<h5>./views/404.html</h5>

```javascript
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8"><!--服务端就可以不用写content-type-->
    </head>
    <body>
        失联了
    </body>
</html>
```

<h5>./views/index.html</h5>

```javascript
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <title>Document</title>
    <link rel="stylesheet" href="/public/index.css">
</head>
<body>
    <div class="header container">
        <div class="page-header">
            <h1>Example page header <small>Subtext for header</small></h1>
            <a class='btn btn-success' href="/post">发表留言</a>
        </div>
    </div>
    <hr>
    <div class="comments container">
        <ul class="list-group">
            {{each comment}}
            <li class="list-group-item">{{$value.name}}说：{{$value.message}}<span>{{$value.dataTime}}</span></li>
            {{/each}}


        </ul>
    </div>
</body>

</html>
```

<h5>./views/post.html</h5>

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Document</title>
<link rel="stylesheet" href="/public/post.css">
</head>

<body>
    <div class="header container">
        <div class="page-header">
            <h1><a href="/">首页</a> <small>发表评论</small></h1>
        </div>
        <hr>
    </div>
    <div class="comments container">
     
        <form action="/pinglun" method="post">
            <div class="form-group">
                <label for="input_name">Name</label><br>
                <input type="text" class="form-control" required minlength="2" maxlength="10" id="input_name"
                    name="name" placeholder="请写入你的姓名">
            </div>
            <div class="form-group">
                <label for="textarea_message">Comment</label><br>
                <textarea class="form-control" name="message" id="textarea_message" cols="30" rows="10" required
                    minlength="5" maxlength="20" ></textarea>
            </div>
            <button type="submit" class="btn btn-default">发表</button>
        </form>
    </div>
</body>

</html>
```

<h5>./public/css/index.css</h5>

```css
* {
    list-style-type: none;
}
div{
    width:100%;
}
a {
    font-size: 15px;
    text-decoration: none;
    color: aqua;
}
.list-group .list-group-item span{
    float: right;
    font-size: 10px;
}
```

<h5>./public/css/post.css</h5>

```css
* {
    list-style-type: none;
}
div{
    width:100%;
}
a {
    font-size: 40px;
    text-decoration: none;
    color: aqua;
}
small{
    font-size: 24px;
}
#textarea_message{
    width:100%;
}
#input_name{
    width:100%
}
```

