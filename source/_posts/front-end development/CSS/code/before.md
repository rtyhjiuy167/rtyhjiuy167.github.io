```html
<!DOCTYPE html>
<html lang="en">

<head>
    <style>
        h2 {
            position: relative;
            cursor: pointer;
        }

        h2:hover::before {
            content: attr(data-title);
            position: absolute;
            background-color: #555;
            color: #fff;
            left: 10px;
            bottom: -35px;
            padding: 5px;
            border: solid 1px #333;
        }
    </style>
</head>

<body>
    <h2 data-title="Hello World!">Hello World?</h2>
</body>

</html>
```

