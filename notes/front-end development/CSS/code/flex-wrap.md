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
            margin: 50px;
        }

        article {
            display: flex;
            border: solid 5px blueviolet;
            flex-direction: column;
            flex-wrap: wrap;
            height: 250px;
        }

        article * {
            width: 100px;
            height: 100px;
            background-color: red;
            margin: 10px;
            font-size: 25px;
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
    </article>

</body>

</html>
```

