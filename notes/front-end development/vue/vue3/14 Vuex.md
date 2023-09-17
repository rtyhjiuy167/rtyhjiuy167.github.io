### 用法

```bash
npm i vuex@4 # 安装4版本
```

```js
<!-- src/store/index.js -->
import { createStore } from "vuex";

export default createStore({
    state: {
        num1: 10,
        num2: 30,
    },
    getters: {
        total(state) {
            return state.num1 + state.num2
        }
    },
    mutations: {
        ADDNUM1(state) {
            state.num1++
        }
    },
    actions: {},
    modules: {}
})
```

```js
// src/main.js
import { createApp } from 'vue'
import App from './App.vue'
import store from './store/index'
createApp(App).use(store).mount('#app')
```

```vue
<!-- src/App.vue -->

<template>
  {{ num1 }} + {{ num2 }} = {{ total }}
  <hr />
  <button @click="btn">num1++</button>
</template>

<script setup>
import { computed } from "@vue/runtime-core";
import { useStore } from "vuex";
let store = useStore();
//值得修改实现响应式 需要使用 computed
let num1 = computed(() => store.state.num1);
let num2 = computed(() => store.state.num2);
let total = computed(() => store.getters.total);
const btn = () => {
  store.commit("ADDNUM1");
};
</script>
```

### 持久化存储

```bash
npm i vuex-persistedstate
```

```js

export default {
    namespaced: true,
    state: {
        num1: 10,
        num2: 30,
    },
  
    mutations: {
        ADDNUM1(state) {
            state.num1++
        }
    },
    actions: {},
}
```

```js
const getters = {
    total: state => state.num1 + state.num2

}
export default getters
```

```js
<!-- src/store/index.js -->
import { createStore } from "vuex";
import getters from './getters'
import persistedState from 'vuex-persistedstate'
import num from './modules/num.js'
export default createStore({
    modules: {
        num,
    },
    getters,
    plugins: [persistedState({
        key: "hahahahhaha",
        paths: ["num"]
    })]
})
```

```js
// src/main.js
import { createApp } from 'vue'
import App from './App.vue'
import store from './store/index'
createApp(App).use(store).mount('#app')
```

```vue
<!-- src/App.vue -->

<template>
  {{ num1 }} + {{ num2 }} = {{ total }}
  <hr />
  <button @click="btn">num1++</button>
</template>

<script setup>
import { useStore } from "vuex";
let store = useStore();
//值得修改实现响应式 需要使用 computed
let num1 = computed(() => store.state.num.num1);
let num2 = computed(() => store.state.num.num2);
let total = computed(() => store.getters["num/total"]);
const btn = () => {
  store.commit("num/ADDNUM1");
};
</script>
```

