```html
<!DOCTYPE html>
<html lang="en">

<head>
    <style>
        main {
            border: solid 1px black;
            height: 220px;
            width: 700px;
            margin: 0 auto;
            padding: 20px;
        }

        div {
            width: 200px;
            height: 200px;
            box-sizing: content-box;
        }

        div:nth-of-type(1) {
            border: solid 1px blue;
            float: left;
        }

        div:nth-of-type(2) {
            border: solid 1px red;
            float: right;
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

