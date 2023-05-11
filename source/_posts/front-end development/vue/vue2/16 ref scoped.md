---
title: 16 ref scoped
top: 16
tags:
  - Vue
categories:
  - Vue
---

#### html标签属性 ref

ref 被用来给元素或子组件注册引用信息。

应用在html标签上获取的是真实DOM元素，应用在组件标签上获取的是组件实例对象。

```html
<h1 ref="title"></h1>
```

```javascript
this.$refs.title
```

#### style标签属性 scoped

style 标签中使用 scoped，则该 vue 文件里的 style 只作用于该组件 （对子组件也不会生效）。

```html
<style scoped></style>
```

如果没有加上`scoped`的`style`在最后会作用到整个页面，加上`scoped`后就只作用于局部。

对于根组件，`scoped`失效。

如果想在使用`scoped`的同时，又想使部分的样式在该页面生效，可以使用`:deep(选择器)`。

