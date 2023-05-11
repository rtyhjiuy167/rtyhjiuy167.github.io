---
title: 22 vuex
top: 22
tags:
  - Vue
categories:
  - Vue
---

## Vuex

专门在 UVe 中实现集中式状态（数据）管理的一个 Vue 插件，对 Vue 应用中多个组件共享的状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于任意组件通信。

#### 下载

```bash
# vue2 中只能使用 vuex 的3版本
npm i vuex@3 # 安装3版本
```

#### 创建 vuex 文件

创建`src/store/index.js`文件，内容如下：

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
// 准备actions -- 用于响应组件的动作 例如在其中请求数据
const actions = {}

// 准备mutations -- 用于操作数据
const mutations = {}

// 准备state -- 用于存储数据
const state = {}

// 准备getters -- 用于加工数据
const getters = {}

// 必须先 Vue.use 再 Vuex.Store
export default new Vuex.Store({
    actions,
    mutations,
    state,
    getters
})
```

#### 在入口文件中引入

```js
// src/main.js
import Vue from 'vue'
import App from './App.vue'

import store from './store' //默认引入 index

new Vue({
  render: h => h(App),
  store,
}).$mount('#app')
```

#### 简单使用

对于`state`中的数据，可以在组件文件中使用``this.$store.state["变量名"]`来获取。

对于`getters`中的数据，可以在组件文件中使用``this.$store.getters["方法名"]`来获取。

对于`actions`中的方法，可以在组件文件中使用`this.$store.dispatch("方法名" [,参数...])`来调用。

对于`mutations`中的方法，可以在组件文件中使用`this.$store.commit("方法名" [,参数...])`来调用。

`actions`中的方法默认接收`context`参数，如果想要在其中调用`mutations`中的方法，需要通过`context.commit("方法名" [,参数...])`的方式去调用。

#### 使用mapState, mapGetters, mapMutations, mapActions

对于`state`中的数据，可以在组件文件中的`computed`中使用`...mapState(["变量名1", "变量名2"])`，如果想使用别名，则可写成`...mapState({ "自定义别名1": "变量名1", "自定义别名2": "变量名2"})`。

对于`mapGetters`中的数据，可以在组件文件中的`computed`中使用`...mapGetters(["变量名1", "变量名2"])`，

对于`actions`中的方法，可以在组件文件中的`methods`使用`...mapActions(["方法名1" ,"方法名2"])`进行引入，如果想使用别名，则可写成`...mapActions({"自定义别名1":"方法名1" , "自定义别名2":"方法名2"})`

对于`mutations`中的方法，可以在组件文件中的`methods`使用`...mapMutations(["方法名1" ,"方法名2"])`进行引入，如果想使用别名，则可写成`...mapMutations({"自定义别名1":"方法名1" , "自定义别名2":"方法名2"})`

#### 模块使用

```javascript
const oneOptions = {
    // 开启命名空间，可以使用让 mapState, mapGetters, mapMutations, mapActions 以模块化的形式获取
    namespaced: true,
    actions: {},
    mutations: {},
    state: {},
    getters: {}
}
export default oneOptions
```

```javascript
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)
// import 导入模块
export default new Vuex.Store({
    modules: {
        // oneOptions
    }
})
```

对于`state`中的数据，可以在组件文件中使用``this.$store.模块名.state["变量名"]`来获取或者在`computed`中使用`...mapState("模块名", ["变量名1", "变量名2"])`

对于`getters`中的数据，可以在组件文件中使用``this.$store.getters["模块名/方法名"]`来获取或者在`computed`中使用`...mapGetters("模块名", ["变量名1", "变量名2"])`。

对于`actions`中的方法，可以在组件文件中使用`this.$store.dispatch("模块名/方法名" [,参数...])`来调用或者在`methods`中使用`...mapState("模块名", ["变量名1", "变量名2"])`进行引入。

对于`mutations`中的方法，可以在组件文件中使用`this.$store.commit("模块名/方法名" [,参数...])`来调用或者在`methods`中使用`...mapMutations("模块名", ["变量名1", "变量名2"])`进行引入。