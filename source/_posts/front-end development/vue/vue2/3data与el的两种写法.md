---
title: 3 data与el的两种写法
top: 3
tags:
  - Vue
categories:
  - Vue
---

## data与el的两种写法

data 与 el 的有两种写法。

#### el 的两种写法

1）配置el属性

```javascript
const vm = new Vue({
    el: '#root', 
});
```

2）vm.$moun() 指定 el 的值

```javascript
vm.$mount('#root');
```

#### data 的两种写法

1）对象式

```javascript
const vm = new Vue({
    el: '#root',
    data: { }
});
```

2）函数式 

在组件定义中只能使用函数式。

当一个**组件**被定义，`data` 必须声明为返回一个初始数据对象的函数，因为组件可能被用来创建多个实例。如果 `data` 仍然是一个纯粹的对象，则所有的实例将**共享引用**同一个数据对象！通过提供 `data` 函数，每次创建一个新实例后，我们能够调用 `data` 函数，从而返回初始数据的一个全新副本数据对象。

```javascript
const vm = new Vue({
    el: '#root',
    data() {
        return {}
    }
});
```

