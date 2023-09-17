```html
<!DOCTYPE html>
<html lang="en">

<head>
    <style>
        * {
            padding: 0;
            margin: 0;
            box-sizing: border-box;
        }

        body {
            margin: 20px;
        }

        main {
            position: absolute;
            left: 50%;
            top: 50%;
            margin-left: -200px;
            margin-top: -200px;
            width: 400px;
            height: 400px;
            border: solid 5px silver
        }

        div {
            left: 50%;
            top: 50%;
            margin-left: -100px;
            margin-top: -100px;
            position: absolute;
            width: 200px;
            height: 200px;
        }

        div:nth-child(1) {
            background-color: #2ecc71;
        }

        div:nth-child(2) {
            background-color: antiquewhite;
            transition: 1s;
        }

        main:hover div:nth-child(2) {
            background-color: antiquewhite;
            transform: translateX(100px);
        }
    </style>
</head>

<body>
    <main>
        <div></div>
        <div></div>
    </main>
</body>

</html>
```

