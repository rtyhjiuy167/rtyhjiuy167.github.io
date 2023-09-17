---
title: 18 mixin 混合
top: 18
tags:
  - Vue
categories:
  - Vue
---

## 混合（混入）

对于想要把多个组件公用的配置，可提取成一个混入对象，在想使用的地方引入并使用即可。

```javascript
export const myMixin = {
  data() {
    return {}
  },
  computed: {},
  methods: {},
  created() { },
  beforeMount() { },
  mounted() { },
  beforeDestroy() { },
}
```

#### 局部使用

在组件文件中引入，并在`mixins`配置项中进行配置：

```javascript
export default {
  mixins: [ /* 填写混入对象名即可 */],
};
```

#### 全局使用

在入口文件中使用`Vue.mixin()`方法：

```js
import Vue from 'vue'
import App from './App.vue'
import { myMixin } from "./mixin";

Vue.mixin(myMixin);
Vue.config.productionTip = false

new Vue({
  render: h => h(App)
}).$mount('#app')
```

#### 注意事项

组件中的`data`数据如果在混合中有重复定义的变量，组件中的生效。

组件中的生命周期函数如果混合中也有，则混合先执行，组件中的后执行。

