---
title: 15 BOM
top: 15
tags:
  - JavaScript
categories:
  - JavaScript
---

### window 对象

Bom 的核心是 window 对象，表示浏览器的实例。window 对象在浏览器中有两重身份，一个是 ECMAScript 中的 Global 对象，另一个就是浏览器窗口的 JavaScript 接口。这意味着网页中定义的所有对象、变量和函数都以 window 作为其 Global 对象，都可以访问其上定义的全局方法。

#### Global 作用域

因为 window 对象被复用为 ECMAScript 的 Global 对象，所以通过 var 声明的所有全局变量和函数都会变成 window 对象的属性和方法，如果使用 let、const 则不会把变量添加给全局对象。

#### 窗口关系

top 对象始终指向最上层（最外层）窗口，即浏览器本身。而 parent 对象则始终指向当前窗口的父窗口。如果窗口是最上层窗口，则 parent 等于 top （都等于 window）。最上层的 window 如果不是通过 window.open() 打开的，那么其 name 属性就不会包含值。

window.slef 指向 window 本身。

#### 窗口大小

布局视口是相对于可见视口的概念。可视视口只能显示整个页面的一小部分，而布局视口为渲染页面的实际大小。

`window.innerHeight`、`window.innerWidth`、`document.body.clientHeight`、`document.body.clientWidth`返回浏览器窗口中页面可视视口的宽高（不包含浏览器边框和工具栏，且会受浏览器的侧边栏、底部弹窗、放缩等影响）：

```javascript
// 不包含滚动条的宽高
console.log("window.innerHeight:", window.innerHeight);
console.log("window.innerWidth:", window.innerWidth);

// 包含滚动条的宽高
// 不同浏览器可能有区别，记录的是 布局视口 大小
console.log("document.documentElement.clientHeight:", document.documentElement.clientHeight);
console.log("document.documentElement.clientWidth:", document.documentElement.clientWidth);
```

`document.body.clientHeight`和`document.body.clientWidth`返回的是布局视口大小：

```javascript
// 布局视口
console.log("document.body.clientHeight:", document.body.clientHeight);
console.log("document.body.clientWidth:", document.body.clientWidth);
```

可使用下面的函数获取大小：

```javascript
function getPageSize() {
    let pageWidth = window.innerWidth,
        pageHeight = window.innerHeight;

    if (typeof pageWidth != "number") {
        if (document.compatMode == "CSS1Compat") {
            pageWidth = document.documentElement.clientWidth;
            pageHeight = document.documentElement.clientHeight;
        } else {
            pageWidth = document.body.clientWidth;
            pageHeight = document.body.clientHeight;
        }
    }

    return {
        pageWidth,
        pageHeight
    }
}
```

`window.outerHeight`、`window.outerWidth`返回浏览器窗口中页面可视视口的宽高(包含浏览器边框和工具栏，且会受浏览器的侧边栏、底部弹窗、放缩等影响)：

```javascript
console.log("window.outerHeight:", window.outerHeight);
console.log("window.outerWidth:", window.outerWidth);
```

获取元素的布局视口大小：

```javascript
console.log(document.getElementsByTagName('div')[0].clientHeight);
console.log(document.getElementsByTagName('div')[0].clientWidth);
```

#### 视口位置

```javascript
// window.scrollBy()  滚动页面
window.scrollBy(50, 0); // 向右滚动 50px
window.scrollBy(0, 50); // 向下滚动 50px

// window.scrollTo(x,y) 滚动 到 距离屏幕左边 x 像素及顶边 y 像素位置 
window.scrollTo(0, 50);

window.scrollTo({
    left: 100,
    top: 200,
    behavior: 'smooth' // 平滑滚动
})
```

#### 导航与打开新窗口

window.open() 方法接收四个参数：要加载的URL、目标窗口（target）、特性字符串和表示新窗口在浏览器历史记录中是否替代当前加载页面的布尔值。通常调用这个方法时只传前3个参数，最后一个参数只有在不代开新窗口时才会使用。

```javascript
// 当前页面跳转
window.open("http://baidu.com","_self");

// 若窗口为存在，则打开新窗口；若存在，该窗口进行页面跳转
window.open("http://baidu.com","窗口名");
```

打开一个可缩放窗口：

```javascript
window.open("http://baidu.com", "baiduWindow", "height=100,width=100,left=100,top=50")
```

调整通过`window.open()`创建的窗口：

```javascript
let baiduWindow = window.open("http://baidu.com", "baiduWindow", "height=100,width=100,left=100,top=50");
// 缩放
baiduWindow.resizeTo(1500, 500);
// 移动
baiduWindow.moveTo(0,0);
// 关闭
baiduWindow.close();
```

有些浏览器程序可能会屏蔽弹窗，此时`window.open()`可能会返回`null`或抛出错误，可使用下面的函数来进行弹窗设置：

```javascript
function popUpWindow(url, target, features) {
    let blocked = false;
    try {
        let newWindow = window.open(url, target, features);
        if (newWindow == null) {
            blocked = true;
        }
    } catch (e) {
        blocked = true;
    }
    if (blocked) {
        alert("弹窗被禁止");
    }
    return blocked;
}
```

### location 对象

location 是最有用的 BOM 对象之一，提供了当前窗口中加载文档的信息，以及通常的导航功能。这个对象独特的地方在于，它既是 widow 的属性，也是 document 的属性。

```javascript
// URL散列值
console.log(location.hash);
// 端口号
console.log(location.port);
// 服务器名（域名或IP）
console.log(location.hostname);
// 当前加载页面的网站 URL 
console.log(location.href);
// URL 中的路径或文件名 例如 http://127.0.0.1:5500/1.html 中的 1.html
console.log(location.pathname);
// 页面使用的协议 通常为 http: 或 https:
console.log(location.protocol);
// URL 的查询字符串 例如 http://127.0.0.1:5500/1.html?a=1&b=2 中的 ?a=1&b=2
console.log(location.search);
// URL原地址 例如 http://127.0.0.1:5500/1.html 中的 http://127.0.0.1:5500
console.log(location.origin); 
```

#### 查询字符串

使用 URLSearchParams 提供的一组标准 API 方法，可以检查和修改查询字符串。

```javascript
const searchParams = new URLSearchParams(location.search);
for (let param of searchParams) {
    console.log(param)
}
```

`get()`、`has()`、`delete()`方法演示：

```javascript
let qs = "?a=1&b=2";
const searchParams = new URLSearchParams(qs);
console.log(searchParams.toString()); // a=1&b=2
console.log(searchParams.get("a")); // 1
searchParams.delete("a"); 
console.log(searchParams.has("a")); // false
console.log(searchParams.toString()); // b=2
```

#### 操作地址

```
location = "http://www.baidu.com"
```

### navigator 对象

```javascript
// 返回浏览器全名
console.log("appName:", navigator.appName);
// 返回浏览器版本，通常与实际的浏览器版本不一致
console.log("appVersion:", navigator.appVersion);
// 返回浏览器的主语言
console.log("language:", navigator.language);
// 返回可用媒体设备
console.log("mediaDevices:", navigator.mediaDevices);
// 返回浏览器运行的系统平台
console.log("platform:", navigator.platform);
// 返回布尔值，表示浏览器是否联网
console.log("onLine:", navigator.onLine);
// 返回浏览器的用户代理字符串
console.log("userAgent:", navigator.userAgent);
```

查询是否安装了对应的插件：

```javascript
function hasPlugin(name) {
    name = name.toLowerCase();
    for (let plugin of window.navigator.plugins) {
        if (plugin.anme.toLowerCase().indexOf(name) > -1) {
            return true;
        }
    }
}
```

### screen 对象

少很用。

### history 对象

