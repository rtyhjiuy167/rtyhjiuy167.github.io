```html
<!DOCTYPE html>
<html lang="en">

<head>
    <style>
        table {
            /* 空单元格隐藏 */
            empty-cells: hide;
            border: none;
            /* 边线合并 */
            border-collapse: collapse;

        }

        table td {
            border: none;
            text-align: center;
            padding: 10px;
            border-bottom: solid 1px #ddd;
            border-right: solid 1px #ddd;
        }

        table td:last-child {
            border-right: none;
        }
    </style>
</head>

<body>
    <table class="table">
        <caption>Hello World</caption>
        <thead>
            <tr>
                <td>编号</td>
                <td>标题</td>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>1</td>
                <td>Hello World?</td>
            </tr>
        </tbody>
        <tfoot>
            <tr>
                <td>2</td>
                <td>Hello World!</td>
            </tr>
        </tfoot>
    </table>
</body>

</html>
```

