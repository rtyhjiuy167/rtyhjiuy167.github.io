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
            margin: 100px;
            height: 100px;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        main div {
            cursor: pointer;
            color: rgb(200, 127, 127);
            font-size: 2em;
        }

        main>div>strong {
            display: inline-block;
            transition: 1s;
        }

        main div:hover>strong:nth-of-type(1) {
            transform: rotate(360deg);
        }

        main div:hover>strong:nth-of-type(2) {
            transform: rotate(-360deg);
        }
    </style>
</head>

<body>
    <main>
        <div>
            <strong>Hello</strong>
            <strong>World</strong>
        </div>

    </main>
</body>

</html>
```

