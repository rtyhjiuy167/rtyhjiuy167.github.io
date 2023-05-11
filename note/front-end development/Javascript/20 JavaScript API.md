## File API

```html
<!DOCTYPE html>
<html>
  <head> </head>
  <body>
    <input type="file" id="filesList" multiple />
    <script>
      const filesList = document.getElementById("filesList");
      filesList.addEventListener("change", (e) => {
        let files = e.target.files;
        for (let i = 0; i < files.length; i++) {
          const f = files[i];
          console.log(`${f.name} ${f.type} ${f.size}bytes`);
        }
      });
    </script>
  </body>
</html>

```

#### FileReader 类型

FileReader 类型表示一种异步文件读取机制。

* readAsText(file, encoding)

  从文件中读取存文本内容并保存在`result`属性中。第二个参数表示编码，可选。

* readAsDataURL(file)

  读取文件并将内容的数据 URI 保存在`result`属性中。

* readAsBinaryString(file)

  读取文件并将每个字符的二进制数据保存在`result`属性中。

* readAsArrayBuffer(file)

  读取文件并将文件内容以`ArrayBuffer`形式保存在`result`属性中。

```html
<!DOCTYPE html>
<html>
  <head> </head>
  <body>
    <input type="file" id="filesList" />
    <div id="output"></div>
    <div id="progress"></div>
    <script>
      const filesList = document.getElementById("filesList");
      filesList.addEventListener("change", (e) => {
        let files = e.target.files,
          progress = document.getElementById("progress"),
          output = document.getElementById("output"),
          reader = new FileReader(),
          type = "default";
        if (/image/.test(files[0].type)) {
          reader.readAsDataURL(files[0]);
          type = "image";
        } else {
          reader.readAsText(files[0]);
          type = "text";
        }
        reader.onerror = function () {
          output.innerHTML = "错误：" + reader.error.code;
        };
        reader.onprogress = function (e) {
          if (e.lengthComputable) {
            progress.innerHTML = `${e.loaded}/${e.total}`;
          }
        };
        reader.onload = function () {
          let html = "";
          switch (type) {
            case "image":
              html = `<img src="${reader.result}">`;
              break;
            case "text":
              html = reader.result;
              break;
          }
          output.innerHTML = html;
        };
      });
    </script>
  </body>
</html>
```

## Blob API

blob 表示二进制大对象（binary large object），是 JavaScript 对不可修改二进制数据的封装类型。

Blob 实例实际上是 File 的超类。

#### Blob 部分读取

```html
<!DOCTYPE html>
<html>
  <head> </head>
  <body>
    <input type="file" id="file" />
    <div id="output"></div>
    <script>
      const filesList = document.getElementById("file");
      filesList.addEventListener("change", (e) => {
        let files = e.target.files,
          progress = document.getElementById("progress"),
          output = document.getElementById("output"),
          blob = new Blob([e.target.files[0]]),
          reader = new FileReader();
        if (blob) {
          reader.readAsText(blob.slice(0, 32));
          reader.onload = function () {
            output.innerHTML = reader.result;
          };
        }
      });
    </script>
  </body>
</html>

```

#### 对象 URL

对象 URL 有时也称为 Blob URL，是指引用存储在 File 或 Bol==lob 中数据的 URL。对象URL的优点是不用把文件内容读取到 JavaScript 也可以使用文件。

```html
<!DOCTYPE html>
<html>
  <head> </head>
  <body>
    <input type="file" id="file" />
    <div id="output"></div>
    <script>
      const filesList = document.getElementById("file");
      filesList.addEventListener("change", (e) => {
        let files = e.target.files,
          progress = document.getElementById("progress"),
          output = document.getElementById("output"),
          url = window.URL.createObjectURL(files[0]);
        if (url && /image/.test(files[0].type)) {
          output.innerHTML = `<img src="${url}">`;
        }
      });
    </script>
  </body>
</html>
```

如果把对象 URL 直接放到`<img>`标签，就不需要把数据先读到 JavaScript 中了。`<img>`标签可以直接从响应的内存位置把数据读取到页面上。

使用完数据之后，最好能释放与之关联的内存。不过，只要对象  URL 在使用，就不能释放。 页面卸载前，可使用`window.URL.revokeObjectURL()`方法，让所有对象 URL 占用的内存都释放掉。

## 媒体元素

HTML5 新增了两个与媒体元素相关的元素，`<audio>`和`<>video`，从而为浏览器提供了嵌入音频和视频的统一解决方案。

由于浏览器支持的媒体格式不同，因此可以指定多个不同的媒体源。为此，可以使用一个或多个`<source>`代替元素的`src`属性。

## Web 组件

这里所说的 Web 组件指的是一套用于增强 DOM 行为的工具，包括影子 DOM、自定义元素和HTML模板。

#### HTML 模板

HTML 模板的思想是在页面中写出特殊标记，让浏览器自动将其解析为 DOM 子树，但跳过渲染。`<template>`标签为此而生。

通过`template`元素的`content`属性可以取得这个`DocumentFragment`的引用：

```html
<!DOCTYPE html>
<html>
  <head> </head>
  <body>
    <template id="app"> 内容 </template>
    <script>
      const app = document.getElementById("app");
      console.log(app.content); // DocumentFragment [ #text ]
    </script>
  </body>
</html>
```

仅`DocumentFragment`上的 DOM 匹配方法可以查询其子树中节点： 

```html
<!DOCTYPE html>
<html>
  <head> </head>
  <body>
    <template id="app">
      <div>内容</div>
    </template>
    <script>
      const fragment = document.getElementById("app").content;
      console.log(fragment.querySelector("div")); // div
      console.log(document.querySelector("div")); // null
    </script>
  </body>
</html>
```

使用`DocumentFragment`可以一次性添加所有子节点，且只会导致一次布局重排：

```html
<!DOCTYPE html>
<html>
  <head> </head>
  <body>
    <div id="app"></div>
    <script>
      const fragment = new DocumentFragment();
      const app = document.getElementById("app");
      fragment.appendChild(document.createElement("p"));
      fragment.appendChild(document.createElement("p"));
      fragment.appendChild(document.createElement("p"));
      console.log(fragment.children.length); // 3
      app.appendChild(fragment); // 仅此处导致布局重排，并且使用 fragment.children 变空
      console.log(fragment.children.length); // 0
    </script>
  </body>
</html>
```

使用`template`标签可以轻松实现一次性添加所有子节点，且只会导致一次布局重排：

```html
<!DOCTYPE html>
<html>
  <head> </head>
  <body>
    <div id="app"></div>
    <template id="fragment">
      <p>1</p>
      <p>2</p>
    </template>
    <script>
      const template = document.getElementById("fragment");
      const fragment = template.content;
      const app = document.getElementById("app");
      // app.appendChild(fragment) 会把 DocumentFragment 的所有子节点转移，可以使用 document.importNode() 方法进行复制
      app.appendChild(fragment); // app.appendChild(document.importNode(fragment, true));
    </script>
  </body>
</html>
```

即使转移了DocumentFragment 的所有子节点但`template`标签仍在 DOM 上，可以进行移除：

```html
<!DOCTYPE html>
<html>
  <head> </head>
  <body>
    <div id="app"></div>
    <template id="fragment">
      <p>1</p>
      <p>2</p>
    </template>
    <script>
      const template = document.getElementById("fragment");
      const fragment = template.content;
      const app = document.getElementById("app");
      app.appendChild(document.importNode(fragment, true));
      template.parentNode.removeChild(template);
    </script>
  </body>
</html>
```

#### 影子 DOM

通过影子 DOM 可以将一个完整的 DOM 树作为节点添加到父 DOM 树。这样可以实现 DOM 封装，意味着 CSS 样式和 CSS 选择符可以限制在影子 DOM 子树而不是整个顶级 DOM 树中。影子 DOM会实际渲染到页面上。

影子 DOM 是通过`attachShadow()`方法创建并添加给有效 HTML 元素的。容纳影子 DOM 的元素被称为影子宿主（shadow host）。影子 DOM 的根节点被称为影子根（shadow root）。

`attachShadow()`方法需要一个`shadowRootInit`对象，返回影子 DOM 的实例。`shadowRootInit`对象必须包含一个`mode`属性，值为`"open"`或`"closed"`。对`"open"`影子DOM 的引用可以通过`shadowRoot`属性在HTML元素上或得，而对`"closed"`影子DOM 的引用无法这样获取。

```javascript
for (let color of ["red", "blue", "green"]) {
    const div = document.createElement("div");
    const shadowDOM = div.attachShadow({ mode: "open" });
    document.body.appendChild(div);
    shadowDOM.innerHTML = `
<p>Make me ${color}</p>
<style>
p { color: ${color}}
</style>
`;
}
```

影子 DOM 是为自定义 Web 组件设计的，为此需要支持嵌套 DOM片段。从概念上讲，位于影子宿主中的 HTML 需要一种机制以渲染到影子 DOM 中去，但这些 HTML 又不必属于影子 DOM 树。

影子 DOM 一添加到元素中，浏览器就会赋予它最高优先级，优先渲染它的内容而不是原来的文本： 

```html
<!DOCTYPE html>
<html>
  <head> </head>
  <body>
    <div id="container">
      <p>内容</p>
    </div>
    <script>
      const div = document.getElementById("container");
      setTimeout(() => {
        div.attachShadow({ mode: "open" });
      }, 1000);
    </script>
  </body>
</html>

```

为了显示内容，需要使用`slot`标签只是浏览器在哪里放置原先的 HTML：

```html
<!DOCTYPE html>
<html>
  <head> </head>
  <body>
    <div id="container">
      <p>内容</p>
    </div>
    <script>
      const div = document.getElementById("container");
      div.attachShadow({ mode: "open" }).innerHTML = `
      <div id="inner">
        <slot></slot>
      </div>`;
    </script>
  </body>
</html>
```

注意：虽然在页面检查窗口中看大内容在影子 DOM 中，但这实际上只是 DOM 内容的投射（projection），实际的元素仍然处于外部 DOM 中。

除了默认插槽（slot），还可以使用命令插槽（named slot）实现多个投射：

```html
<!DOCTYPE html>
<html>
  <head> </head>
  <body>
    <div id="container">
      <p slot="first">first</p>
      <p slot="second">second</p>
    </div>
    <script>
      const div = document.getElementById("container");

      div.attachShadow({ mode: "open" }).innerHTML = `
        <slot name="second"></slot>
        ---   我是分割线   ---
        <slot name="first"></slot>
        `;
    </script>
  </body>
</html>
```

