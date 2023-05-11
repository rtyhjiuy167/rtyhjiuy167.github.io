---
title: 2 setup
top: 2
tags:
  - Vue3
categories:
  - Vue3
---

### setup

Vue2的`data`、`methods`、`computed`等在Vue3中可全部写在`setup`中，其为组合式 API。

```html
<!DOCTYPE html>
<html>
  <head>
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
  </head>

  <body>
    <div id="APP">{{ name }}</div>
    <script>
      Vue.createApp({
        setup() {
          const name = "张三"
          return {
            name
          };
        },
      }).mount("#APP");
    </script>
  </body>
</html>
```

- setup执行的时机
  - 在beforeCreate之前执行一次，this是 undefined 。

- setup的参数
  - props：值为对象，包含：组件外部传递过来，且组件内部声明接收了的属性。
  - context：上下文对象
    - attrs: 值为对象，包含：组件外部传递过来，但没有在props配置中声明的属性, 相当于 ```this.$attrs```。
    - slots: 收到的插槽内容, 相当于 ```this.$slots```。即使子组件没有插槽，也会收到这虚拟结点。
    - emit: 分发自定义事件的函数, 相当于 ```this.$emit```。

setup 不能是一个async 函数，因为返回值不再是return的对象, 而是 promise,  模板看不到return对象中的属性。（后期也可以返回一个Promise实例，但需要Suspense和异步组件的配合）









