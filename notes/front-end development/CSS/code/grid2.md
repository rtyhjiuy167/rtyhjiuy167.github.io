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
            grid-template-columns: repeat(2, 1fr);
            grid-template-rows: repeat(2, 1fr);
        }

        div {
            background-color: blueviolet;
            background-clip: content-box;
            padding: 10px;
            box-sizing: border-box;
        }

        div:nth-child(1) {
            grid-row-start: 1;
            grid-column-start: 1;
            grid-row-end: 3;
            grid-column-end: 2;
        }
    </style>
</head>

<body>
    <article>
        <div>1</div>
        <div>2</div>
        <div>3</div>
    </article>
</body>

</html>
```

