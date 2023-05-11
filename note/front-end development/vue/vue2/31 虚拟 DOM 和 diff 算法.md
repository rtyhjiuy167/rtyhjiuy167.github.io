---
title: 30 虚拟 DOM 和 diff 算法
top: 30
tags:
  - Vue
categories:
  - Vue
---

## 虚拟 DOM 和 diff 算法

### 虚拟 DOM

虚拟 DOM 只是 js 模拟的 DOM 结构。 虚拟DOM是 HTML DOM 的抽象。

#### h 函数

```javascript
// h('div',{},'文字')
// h('div',{},[h(),h()])
// h('div',{},h())
function h(sel, data, c) {
    if (arguments.length != 3) {
        throw new Errow("h函数必须传入3个参数");
    }
    if (typeof c == "string" || typeof c == "number") {
        return vnode(sel, data, undefined, c, undefined);
    } else if (Array.isArray(c)) {
        let children = [];
        for (let i = 0; i < c.length; i++) {
            // 注意 c[i] 是 h 函数的执行结果,返回的 vnode
            if (!(typeof c[i] == "object" && c[i].hasOwnProperty("sel"))) {
                throw new Errow("传入的数组参数中必须为h函数");
            }
            children.push(c[i]);
        }
        // 弱化版，规定 有子节点的节点没有 text
        return vnode(sel, data, children, undefined, undefined);
    } else if (typeof c == "object" && c.hasOwnProperty("sel")) {
        let children = [c];
        return vnode(sel, data, children, undefined, undefined);
    } else {
        throw new Errow("h函数传入不对");
    }
}
```

```javascript
function vnode(sel, data, children, text, elm) {
    // 将 data 中 { key:undefined } key 拿出
    const key = data.key;
    return {
        sel, data, children, text, elm, key
    }
}
```

#### 尝试使用

```javascript
let myVode = h("ul", {}, [
  h("li", { key: "1" }, "A"),
  h("li", { key: "2" }, "B"),
  h("li", { key: "3" }, "C"),
]);
console.log(myVode);
```

```
{
  "sel": "ul",
  "data": {},
  "children": [
    {
      "sel": "li",
      "data": { "key": "1" },
      "children": null,
      "text": "A",
      "elm": 对应的真实 DOM 元素 li,
      "key": "1"
    },
    {
      "sel": "li",
      "data": { "key": "2" },
      "children": null,
      "text": "B",
      "elm": 对应的真实 DOM 元素 li,
      "key": "2"
    },
    {
      "sel": "li",
      "data": { "key": "3" },
      "children": null,
      "text": "C",
      "elm": 对应的真实 DOM 元素 li,
      "key": "3"
    }
  ],
  "text": null,
  "elm": 对应的真实 DOM 元素 ul,
  "key": null
}
```

### diff算法

diff算法，可实现最小量更新，只有是同一个虚拟节点，才进行精细化比较，否则暴力删除旧的，添加新的。

同一个虚拟节点：h函数的sel参数须相同（即选择器相同）且key相同。

#### updateChildren()

以`diff`算法实现最小量更新。

```javascript
function updateChildren(parentElm, oldCh, newCh) {
    // 旧前
    let oldStartIdx = 0;
    // 新前
    let newStartIdx = 0;
    // 旧后
    let oldEndIdx = oldCh.length - 1;
    // 新后
    let newEndIdx = newCh.length - 1;
    // 旧前节点
    let oldStartVnode = oldCh[0];
    // 旧后节点
    let oldEndVnode = oldCh[oldEndIdx];
    // 新前节点
    let newStartVnode = newCh[0];
    // 新后节点
    let newEndVnode = newCh[newEndIdx];

    let keyMap = null;

    while (oldStartIdx <= oldEndIdx && newStartIdx <= newEndIdx) {

        if (oldStartVnode == null || oldCh[oldStartIdx] == undefined) {
            oldStartVnode = oldCh[++oldStartIdx];
        } else if (oldEndVnode == null || oldCh[oldEndIdx] == undefined) {
            oldEndVnode = oldCh[--oldEndIdx];
        } else if (newStartVnode == null || oldCh[newStartIdx] == undefined) {
            newStartVnode = newCh[++newStartIdx];
        } else if (newEndVnode == null || oldCh[newEndIdx] == undefined) {
            newEndVnode = newCh[--newEndIdx];
        } else
            if (checkSameVnode(oldStartVnode, newStartVnode)) {
                // 新前与旧前
                patchVnode(oldStartVnode, newStartVnode);
                oldStartVnode = oldCh[++oldStartIdx];
                newStartVnode = newCh[++newStartIdx];
            } else if (checkSameVnode(oldEndVnode, newEndVnode)) {
                // 新后与旧后
                patchVnode(oldStartVnode, newStartVnode);
                oldEndVnode = oldCh[--oldEndIdx];
                newEndVnode = newCh[--newEndIdx];
            } else if (checkSameVnode(oldStartVnode, newEndVnode)) {
                // 新后与旧前
                patchVnode(oldStartVnode, newEndVnode);
                // 当新前与旧后命中的时候要移动节点
                parentElm.insertBefore(oldStartVnode.elm, oldEndVnode.elm.nextSibling);
                oldStartVnode = oldCh[++oldStartIdx];
                newEndVnode = newCh[--newEndIdx];
            } else if (checkSameVnode(oldEndVnode, newStartVnode)) {
                // 新前与旧后
                patchVnode(oldEndVnode, newStartVnode);
                // 当新前与旧后命中的时候要移动节点
                parentElm.insertBefore(oldEndVnode.elm, oldStartVnode.elm);
                oldEndVnode = oldCh[--oldEndIdx];
                newStartVnode = newCh[++newStartIdx];
            } else {

                // 做一个 map,用于旧的记录， key 为 key，value 为 index
                // 用来加速搜索
                if (!keyMap) {
                    keyMap = {}
                    for (let i = oldStartIdx; i <= oldEndIdx; i++) {
                        const key = oldCh[i].key;
                        if (key != undefined) {
                            keyMap[key] = i;
                        }
                    }
                }

                // 寻找 newStartIdx 这项在旧的中是否存在
                const idxInOld = keyMap[newStartVnode.key];
                if (idxInOld == undefined) {
                    // newStartIdx 这项在旧的中不存在，说明其是新的元素，需插入

                    parentElm.insertBefore(createElement(newStartVnode), oldStartVnode.elm);
                } else {
                    // newStartIdx 这项在旧的中存在，只需移动
                    const elmToMove = oldCh[idxInOld];

                    patchVnode(elmToMove, newStartVnode);
                    // 将旧项设置为 undefined
                    oldCh[idxInOld] = undefined;
                    parentElm.insertBefore(elmToMove.elm, oldStartVnode.elm)

                }
                // 移动指针，只移动头
                newStartVnode = newCh[++newStartIdx];
            }
    }

    if (newStartIdx <= newEndIdx) {
        // 循环结束时，当新前仍小于等于新后，说明新前与新后之间的节点须添加

        for (let i = newStartIdx; i <= newEndIdx; i++) {
            parentElm.insertBefore(createElement(newCh[i]), oldCh[oldStartIdx].elm);
        }
    } else if (oldStartIdx <= oldEndIdx) {
        // 循环结束时，当旧前仍小于等于旧后，说明旧前与旧后之间的节点须删除
        for (let i = oldStartIdx; i <= oldEndIdx; i++) {
            if (oldCh[i]) {
                parentElm.removeChild(oldCh[i].elm);
            }
        }
    }
}
```

```javascript
function checkSameVnode(a, b) {
    return a.sel == b.sel && a.key == b.key;
}
```

#### createElement()

`createElement()`方法创建虚拟DOM元素对应的真实DOM元素。

```javascript
// 传入虚拟节点，并将其 elm 属性指向自己对应的DOM节点
// 弱化版的，没有实现元素属性的添加，这个其实遍历,使用 setAttrbute 添加即可
function createElement(vnode) {
  let domNode = document.createElement(vnode.sel);
  if (
    (vnode.text != "" && vnode.children == undefined) ||
    vnode.children.length == 0
  ) {
    domNode.innerText = vnode.text;
  } else if (Array.isArray(vnode.children) && vnode.children.length > 0) {
    // 内部有子节点，需要递归
    for (let i = 0; i < vnode.children.length; i++) {
      let ch = vnode.children[i];
      // 当有子节点时，创建对应的 DOM 并添加到 父节点中
      let chDOM = createElement(ch);
      domNode.appendChild(chDOM);
    }
  }
  vnode.elm = domNode;
  return vnode.elm;
}
```

#### patch()

`patch()` 方法用于真实 DOM 元素进行挂载上树。

```javascript
function (oldVnode, newVnode) {
    //既然要上树，上树的肯定有 DOM，且之后 调用 patch 也可能在 DOM 节点上
    // 判断传入的第一个参数，是DOM节点还是虚拟节点
    if (oldVnode.sel == '' || oldVnode.sel == undefined) {
        // 传入的第一个参数是 DOM 节点，此时要包装为虚拟节点
        oldVnode = vnode(oldVnode.tagName.toLowerCase(), {}, [], undefined, oldVnode)
    }
    // 判断 oldVnode 和 newVnode 是不是同一个 节点
    if (oldVnode.key == newVnode.key && oldVnode.sel == newVnode.sel) {
        patchVnode(oldVnode, newVnode);

    } else {
        let newVnodeElm = createElement(newVnode);
        if (oldVnode.elm && newVnodeElm) {
            oldVnode.elm.parentNode.insertBefore(newVnodeElm, oldVnode.elm);
        }

        // 删除老节点
        oldVnode.elm.parentNode.removeChild(oldVnode.elm);
    }
}
```

#### patchVnode()

`patchVnode()`方法对虚拟DOM进行更新，调用`updateChildren()`方法。

```javascript
function patchVnode(oldVnode, newVnode) {
  // 判断新旧 vnode 是不是同一个对象
  if (oldVnode === newVnode) return;
  // 判断 vnode 有没有 text 属性
  // 因为这里是弱化版的有 text 默认与 children 不共存
  if (
    newVnode.text != undefined &&
    (newVnode.children == undefined || newVnode.children.length == 0)
  ) {
    if (newVnode.text != oldVnode.text) {
      // innerText 一改，里面的所有内容全变
      oldVnode.elm.innerText = newVnode.text;
    }
  } else {
    if (oldVnode.children != undefined && oldVnode.children.length > 0) {
      // 新创建的节点应应插入到所有未处理的节点之前，这样可保证顺序，而不是所有已处理节点之后
      updateChildren(oldVnode.elm, oldVnode.children, newVnode.children);
    } else {
      // 清空老节点的文本
      oldVnode.elm.innerHTML = "";
      for (let i = 0; i < newVnode.children.length; i++) {
        let dom = createElement(newVnode.children);
        oldVnode.elm.appendChild(dom);
      }
    }
  }
}
```

### 使用

```javascript
let myVode1 = h("ul", {}, [
  h("li", { key: "A" }, "A"),
  h("li", { key: "B" }, "B"),
  h("li", { key: "C" }, "C"),
  h("li", { key: "D" }, "D"),
  h("li", { key: "E" }, "E"),
]);
const container = document.getElementById("container");
patch(container, myVode1);

let myVode2 = h("ul", {}, [
  h("li", { key: "E" }, "F"),
  h("li", { key: "C" }, "C"),
]);
const btn = document.getElementById("btn");
btn.onclick = function () {
  patch(myVode1, myVode2);
};
```