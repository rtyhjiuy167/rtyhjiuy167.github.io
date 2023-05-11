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
            grid-template-columns: repeat(auto-fill, 100px);
            grid-template-rows: repeat(auto-fill, 100px);
        }

        div {
            background-color: blueviolet;
            background-clip: content-box;
            padding: 10px;
            box-sizing: border-box;
        }
    </style>
</head>

<body>
    <article>
        <div>1</div>
        <div>2</div>
        <div>3</div>
        <div>4</div>
        <div>5</div>
        <div>6</div>
        <div>7</div>
        <div>8</div>
    </article>
</body>

</html>
```

