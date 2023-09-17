```html
<!DOCTYPE html>
<html lang="en">

<head>
    <style>
        * {
            padding: 0;
            margin: 0;
        }

        body {
            margin: 20px;
        }

        li {
            list-style: none;
        }

        header {
            height: 50px;
            background-color: #eee;
            display: flex;
            justify-content: space-between;
        }

        header section {
            flex: 1;
            display: flex;
            border-right: solid 1px #ccc;
            flex-direction: column;
        }

        header section:last-child {
            border-right: none;
        }

        header section h4 {
            /* 不进行缩小 */
            flex-shrink: 0;
            height: 50px;
            display: flex;
            text-align: center;
            justify-content: center;
            flex-direction: column;
        }

        header section ul {
            display: flex;
            flex-direction: column;
            border: solid 1px #ccc;
            text-align: center;
            margin-top: 5px;
        }

        header section li {
            display: flex;
            flex-direction: column;
            border-bottom: solid 1px #ccc;
            text-align: center;
            margin-top: 5px;
        }

        header section li:last-child {
            border-bottom: none;
        }
    </style>
</head>

<body>


    <header>
        <section>
            <h4>一</h4>
            <ul>
                <li>HTML</li>
                <li>CSS</li>
                <li>JS</li>
            </ul>
        </section>
        <section>
            <h4>二</h4>
            <ul>
                <li>PHP</li>
                <li>JAVA</li>
                <li>GO</li>
            </ul>
        </section>
        <section>
            <h4>三</h4>
        </section>
    </header>
</body>

</html>
```

