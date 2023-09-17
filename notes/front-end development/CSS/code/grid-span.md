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
            grid-row-start: 2;
            grid-row-end: span 2;
            grid-column-start: 2;
            grid-column-end: span 2;
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

