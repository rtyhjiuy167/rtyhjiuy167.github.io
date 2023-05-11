```html
<!DOCTYPE html>
<html lang="en">

<head>
    <style>
        body {
            margin: 0 auto;
            width: max-content;
        }

        article {
            width: 150px;
            position: relative;
        }

        article div {
            height: 50px;
            border: solid 1px #ddd;
            text-align: center;

            line-height: 50px;
        }

        article div:nth-of-type(1) {
            background-color: #fff;
            position: relative;

            z-index: 2;
        }

        article:hover div:nth-of-type(1) {
            border-bottom: none;
        }

        article:hover div:nth-of-type(2) {
            display: block;
        }

        article div:nth-of-type(2) {
            width: 300px;
            position: absolute;
            right: 0;
            top: 50px;
            display: none;
        }
    </style>
</head>

<body>
    <article>
        <div>我的购物车</div>
        <div>购物车中暂无商品</div>
    </article>
</body>

</html>
```

