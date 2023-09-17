---
title: 18 canvas
top: 18
tags:
  - JavaScript
categories:
  - JavaScript
---

## 基本的画布功能

创建 \<canvas\> 元素时至少要设置其 width 和 height 属性。出现在开始和结束标签之间的内容，会在浏览器不支持 \<canvas\> 元素时显示。

```html
<!DOCTYPE html>
<html lang="en">
<body>
    <!-- canvas在低版本浏览器中不兼容时，浏览器会把canvas结点当成普通结点，会显示文本结点内容 -->
    <canvas id="canvas" width="500px" height="500px">您的浏览器不支持canvas</canvas>
</body>
    <script>
        let canvas = document.querySelector('#canvas')
        //通过测试getContext方法是否存在，来确保浏览器支持 <canvas>
        if (canvas.getContext) {
        }
    </script>
</html>
```

#### 绘制基本步骤

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <style>
        canvas {
            margin: 0 auto;
            border: 1px solid #aaa;
            display: block;
        }
    </style>
</head>

<body>
    <canvas id="canvas" width="500px" height="500px"></canvas>
    <script>
        let canvas = document.querySelector('#canvas')
        if (canvas.getContext) {
            let ctx = canvas.getContext('2d')
            ctx.beginPath();
            ctx.moveTo(100, 100);
            ctx.lineTo(100, 200);
            //设置线条颜色
            ctx.strokeStyle = 'blue';
            //设置线条粗细
            ctx.lineWidth = 10;
            ctx.stroke();
            ctx.closePath();
        }
    </script>
</body>

</html>
```

#### 绘制线条

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <style>
        canvas {
            margin: 0 auto;
            border: 1px solid #aaa;
            display: block;
        }
    </style>
</head>

<body>
    <canvas id="canvas" width="500px" height="500px"></canvas>
    <script>
        drawSolidLine(100, 100, 400, 100)
        drawdashedLine(100, 200, 400, 200)
        //绘制实线
        function drawSolidLine(x1, y1, x2, y2, color = 'black', width = 1) {
            let canvas = document.querySelector('#canvas')
            let ctx = canvas.getContext('2d')
            if (canvas.getContext) {
                ctx.beginPath();
                ctx.moveTo(x1, y1);
                ctx.lineTo(x2, y2);
                ctx.strokeStyle = 'blue';
                ctx.lineWidth = width;
                ctx.stroke();
                ctx.closePath();
            }
        }
        //绘制虚线
        function drawdashedLine(x1, y1, x2, y2, itemNum = 10, spacing = 2, color = 'black', width = 1) {
            //每段实线+空白的x大小
            const itemDX = Math.abs(x2 - x1) / itemNum
            //每段实线+空白的y大小
            const itemDY = Math.abs(y2 - y1) / itemNum
            // 每段空白的x大小
            const spaceingX = spacing * itemDX / (itemDY + itemDX)
            // 每段空白的y大小
            const spaceingY = spacing * itemDY / (itemDY + itemDX)
            //每段实线的x大小
            const lineX = itemDX - spaceingX
            //每段实线的y大小
            const lineY = itemDY - spaceingY
            for (let i = 0; i < itemNum; i++) {
                drawSolidLine(x1 + itemDX * i, y1 + itemDY * i, x1 + lineX + itemDX * i, y1 + lineY + itemDY * i, color, width)
            }
        }
    </script>
</body>

</html>
```

#### 导出

可以使用toDataURL()方法导出\<canvas\>元素上的图像。

```javascript
function exportCanvasAsPNG(id, fileName) {
	const canvas = document.getElementById(id);
	const MIME_TYPE = "image/png";
	const imgURL = canvas.toDataURL(MIME_TYPE);
	const a = document.createElement('a');
	a.download = fileName;
	a.href = imgURL;
	a.dataset.downloadurl = [MIME_TYPE, a.download, a.href].join(':');
	a.click();
}
exportCanvasAsPNG("canvas", "图片名字");
```

