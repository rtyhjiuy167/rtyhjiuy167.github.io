---
title: 16 DOM
top: 16
tags:
  - JavaScript
categories:
  - JavaScript
---

## DOM

DOM 中有 12 种 节点类型，这些类型都基础一种基本类型。

### 节点层级

#### Node 类型

DOM Level1 描述了名为 Node 的接口，这个接口是所有 DOM 节点类型都必须实现的。

1）每个节点都有`nodeType`属性，表示该节点的类型。节点类型由定义在 Node 类型上的 12 个数值常量表示，例如`Node.ELEMENT_NODE`：1，`Node.TEXT_NODE`：3。

2）每个节点都有 `childNodes`属性，其返回类型为类数组。`childNodes`属性会获取包括文本节点在内的所有子节点，而IE8及以下则不会将 text 当成文本。

```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="UTF-8">
</head>

<body>
  <ul>
    <li></li>
    <li></li>
  </ul>
  <script>
    let ul = document.getElementsByTagName("ul")[0];
    console.log(ul.childNodes); // NodeList(5) [ #text, li, #text, li, #text ]
	// 类数组转为数组1
    console.log(Array.prototype.slice.call(ul.childNodes, 0)); // Array(5) [ #text, li, #text, li, #text ]
	// 类数组转为数组2
    console.log([].slice.call(ul.childNodes, 0)); // Array(5) [ #text, li, #text, li, #text ]
	// 类数组转为数组3 使用ES6的 Array.from()静态方法
    console.log(Array.from(ul.childNodes)); // Array(5) [ #text, li, #text, li, #text ]
  </script>
</body>

</html>
```

上例中，ul标签与li标签之间存在间隔，被识别为文本。

3）`children`属性可以获取所有子元素，没有文本节点。

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
</head>

<body>
    <ul>
        <li></li>
        <li></li>
    </ul>
    <script>
        let ul = document.getElementsByTagName("ul")[0];
        console.log(ul.children); // HTMLCollection { 0: li, 1: li, length: 2 }
    </script>

</body>

</html>
```

3）firstChildfirst

firstChild使用时其为父节点与子节点间的文本，有空格就有文本。

4）ElementChild

firstElementChild返回第一个时其为子元素，不为文本。

5）previousElementSubling

返回前一个兄弟元素，不为文本。

6）previousSibling

返回前一个兄弟节点，会获取到text。

#### Document 类型

Document 类型是 JavaScript 中表示文档节点的类型。在浏览器中，文档对象 document 是 HTML 的实例（HTMLDocument 继承 Document），表示整个 HTML页面。document是window对象的属性，因此是一个全局对象。

#### Element 类型

1）设置、获取、移除属性

* getAttribute()

```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="UTF-8">
</head>

<body>
  <div id="app" class="container" data-name="自定义属性"></div>
  <script>
    const div = document.getElementById('app');
    console.log(div.getAttribute("id"));
    console.log(div.getAttribute("class"));
    console.log(div.getAttribute("data-name"));
  </script>
</body>

</html>
```

* setAttribute()
* removeAttribute()
* 因为元素也是DOM对象属性，所以直接给DOM对象的属性赋值也可以设置或获取元素属性的值，不过**不适用**于自定义属性。给自定义属性赋值也使用`setAttribute()`

2）创建元素

可以使用 document.createElement() 方法创建新元素，再使用appendChild()、insertBefore()、replaceChild()等方法把元素添加到文档树。

#### Text 类型

Text 节点由 Text 类型标识。

#### Coment 类型

DOM 中的注释通过 Comment 类型标识。

### MutationObserver 接口

#### 基本使用

MutationObserver 接口可以在 DOM 被修改时异步执行回调。MutationObserver可以观察整个文档、DOM树的一部分，或某个元素。此外还可以观察元素属性、子节点、文本或者前三者任意组合。

MutationObserver 的实例要通过调用 MutationObserver 构造函数并传入一个回调函数来创建：

```javascript
let observer = new MutationObserver((mutationRecoards, observer) => {})
```

然后调用实例的 observe() 方法，并传入要观察期变化的 DOM 节点，以及一个 MutationObserverInit 对象。

```javascript
observer.observer(document.body , { attributes: true})
```

对于回调参数，每个回调都会接收到一个 mutationRecoard 实例的数组和一个 MutationObserver 的实例。

因为执行回调之前可能同时发生多个满足观察条件的事件，所以每次执行回调都会传入一个包含按顺序入队的 MutationObserver 实例的数组。

#### MutationObserverInit 

|       属性        |                           说明                           |
| :---------------: | :------------------------------------------------------: |
|      subtree      | 布尔值，表示除了目标节点，是否观察目标节点的子树（后代） |
|    attributes     |              表示是否观察目标节点的属性变化              |
|  attributeFilter  |                 表示要观察哪些属性的变化                 |
| attributeOldValue |                 是否记录变化之前的属性值                 |



```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
</head>

<body>
    <main style="width:100%">
        <div style="background-color:#ddd;width:100%">1</div>
    </main>
    <button>改变</button>
    <script>
        const div = document.getElementsByTagName('div')[0];
        const btn = document.getElementsByTagName('button')[0];
        let observer = new MutationObserver((mutationRecoards, observer) => {
            console.log(mutationRecoards)
            console.log(observer)
        })
        observer.observe(div, { attributes: true, attributeFilter: ['style'] })
        btn.onclick = () => {
            // 共触发两次
            div.style.display = 'inline-block'
            div.style.width = '300px'
        }
    </script>
</body>

</html>
```

## DOM 扩展

#### scrollIntoView

每个 HTML 元素上都存在`scrollIntoView()`方法，该方法可以滚动浏览器窗口或容器元素以便包含元素进入视口。

```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="UTF-8">
</head>

<body>
  <div style="width: 100px;height:100px;background-color:red"></div>
  <div style="width: 100px;height:100px;background-color:blue"></div>
  <div style="width: 100px;height:100px;background-color:aqua"></div>
  <div style="width: 100px;height:100px;background-color:yellow"></div>
  <script>
    document.getElementsByTagName("div")[2].scrollIntoView({
      behavior: "smooth", // 定义过渡动画
      block: "center", // 定义垂直方向的对齐
      inline: "center" // 定义水平方向的对齐
    })
  </script>
</body>

</html>
```

#### getComputedStyle

`style`对象中包含支持`style`属性的元素为这个属性设置的样式信息，但不包含从其他样式表层叠基础的同样影响该元素的样式信息。DOM2 Style 在`document.defaultView`上增加了`getComputedStyle()`方法，该方法可取得元素的计算样式，包含从其他样式表层叠基础的同样影响该元素的样式信息。

`getComputedStyle()`接收两个参数：要取得计算样式的元素和伪元素字符串。如果不查询伪元素，则第二个参数传`null`。

```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="UTF-8">
</head>

<body>
  <div style="width: 100px;height:100px;background-color:red"></div>
  <script>
    console.log(document.defaultView.getComputedStyle(document.getElementsByTagName('div')[0], null));
  </script>
</body>

</html>
```

#### 元素尺寸

1）偏移尺寸

偏移尺寸仅可读。

* offsetHeight

  元素在垂直方向上占用的像素尺寸，包括它的高度、水平滚动条和上、下边框的高度。

* offsetLeft

  元素左边框距离包含元素（offetParent）左边框内侧的像素值。

* offsetTop

  元素上左边框距离包含元素（offetParent）边框内侧的像素值。

* offsetWidth

  元素在水平方向上占用的像素尺寸，包括它的宽度、垂直滚动条和左、右边框的高度。

因为使用`offsetHeight`或`offsetTop`得到的偏移量是相对于包含元素的（包含元素可以通过`offestParent`访问到），所以得到的值可能不是元素在页面中的偏移量。要得到元素在页面中的偏移量，需要不断计算，直到加至根元素。

下面例子中取得了`td`与`table`的偏移量，注意`td`的`offestParent`为`table`：

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <style>
    * {
      margin: 0;
    }

    table {
      margin: 10px;
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
        <td id="td2">Hello World!</td>
      </tr>
    </tfoot>
  </table>

  <script>

    const table = document.querySelector('.table');
    const td2 = document.querySelector('#td2')

    function getElementOffsetLeft(element) {
      let actualLeft = element.offsetLeft;
      let current = element.offsetParent;
      while (current !== null) {
        actualLeft += current.offsetLeft;
        current = current.offsetParent;
      }
      return actualLeft;
    }

    console.log(table.offsetLeft); // 10
    console.log(getElementOffsetLeft(table)); // 10
    console.log(td2.offsetLeft); // 52
    console.log(getElementOffsetLeft(td2)); // 62
  </script>
</body>

</html>
```

`offsetParent`指向最近的带有`position: absolute`的父元素：

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <style>
    main {
      position: relative;
      width: 250px;
      height: 250px;
      background-color: yellow;
    }

    main div {
      position: absolute;
      width: 100px;
      height: 100px;
    }

    main div {
      background-color: red;
    }
  </style>
</head>

<body>
  <main>
    <div></div>
  </main>
  <script>
    const div = document.getElementsByTagName("div")[0];
    console.log(div.offsetParent); // <main>
  </script>
</body>

</html>
```

2）客户端尺寸

元素客户端尺寸包含元素内容及其内边距所占用的空间，相当于偏移尺寸少了边框、滚动条。

* clientHeight

* clientWidth

3）滚动尺寸

* scrollHeight

  元素内容的总高度。

* scrollLeft

  内容区左侧隐藏的像素数，设置这个属性可以改变元素的滚动位置。

* scrollTop

  内容区顶部隐藏的像素数，设置这个属性可以改变元素的滚动位置。

* scrollWidth

  元素内容的总宽度。

注意：当父元素的`style`中将`display`设置为`none`时，其子元素的滚动尺寸为0。

4）getBoundingClientRect()

每个元素上都暴露了`getBoundingClientRect()`方法，该方法返回一个`DOMRect`对象，包含`left`、`x`、`top`、`y`、`right`、`bottom`、`height`、`width`。这些元素给出了元素在页面中相对于视口的位置。其中`left`、`x`值与之前偏移尺寸例子中`getElementOffsetLeft()`方法返回的数值相同，`top`、`y`同理。