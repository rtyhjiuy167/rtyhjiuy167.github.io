```html
<!DOCTYPE html>
<html lang="en">

<head>
    <link rel="stylesheet" href="less.css">
</head>

<body>
    <article class="table">
        <nav>Hello World</nav>
        <section>
            <ul>
                <li>编号</li>
                <li>标题</li>
            </ul>
        </section>
        <section>
            <ul>
                <li>1</li>
                <li>Hello World?</li>
            </ul>
        </section>
        <section>
            <ul>
                <li>2</li>
                <li>Hello World!</li>
            </ul>
        </section>
    </article>
</body>

</html>
```

`less.less`内容如下：

```less
.table {
    display: table;

    nav {
        display: table-caption;
        text-align: center;
        caption-side: top;
    }

    section {
        &:nth-of-type(1) {
            display: table-header-group;
            background-color: #555;
            color: white;
        }

        &:nth-of-type(2) {
            display: table-row-group;
        }

        &:nth-of-type(3) {
            display: table-footer-group;
            background-color: #f3f3f3;
        }

        ul {
            display: table-row;

            li {
                display: table-cell;
                border: solid 1px #ddd;
                padding: 10px;
                text-align: center;
            }
        }

    }
}
```

