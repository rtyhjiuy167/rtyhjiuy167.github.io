地址：https://www.npmjs.com/package/ejs

```bash
npm install ejs koa-views
```

```js
app.use(
  views(
    path.join(__dirname, '..', 'views'), // 路径要对上
    {
      map: {
        html: 'ejs'// 使用 ejs 模板解析 
      }
    }
  )
)
```

在`src`目录下创建`views`文件夹，在`views`下创建`index.html`，内容如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <h3>
    <%=title%>
        
  </h3>
</body>

</html>
```

```js
router.get('/', async (ctx) => {
	let title = "haha"
  await ctx.render('index.html',{
    title
  }) // render 中的第一个参数会去配置好的路径上去寻找静态资源
})
```

