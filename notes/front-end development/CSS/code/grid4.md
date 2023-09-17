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

        article {
            width: 300px;
            height: 300px;
            border: solid 5px silver;
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            grid-template-rows: repeat(3, 1fr);
        }

        div {
            background-color: blueviolet;
            background-clip: content-box;
            padding: 10px;
            box-sizing: border-box;
        }

        div {
            grid-row: 2/3;
            grid-column: 1/3;

        }
    </style>
</head>

<body>
    <article>
        <div></div>
    </article>
</body>

</html>
```

