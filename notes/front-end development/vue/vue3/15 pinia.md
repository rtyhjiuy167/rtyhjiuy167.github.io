### pinia

pinia没有 mutations，只有 state、getter、actions
pinia分模块不需要modules
pinia体积更小、性能更好
pinia支持直接修改数据

```bash
npm i pinia
# 使用镜像
npm i pinia --registry=https://registry.npm.taobao.org
```

`src/store/index.js`或`src/store/index.ts`

```js
// src/store/index.js

import { defineStore } from "pinia";

export const useStore = defineStore("storeId", {
    state: () => {
        return {
            counter: 0
        }
    },
    getters: {},
    actions: {}
})
```

`src/main.js`或`src/main.ts`

```js
// src/main.js
import { createApp } from 'vue'
import { createPinia } from 'pinia'
import App from './App.vue'
createApp(App).use(createPinia()).mount('#app')
```

```vue
<!-- src/App.vue -->

<template>
  {{ counter }}
  <br />
  <button @click="addNum">counter++</button>
</template>

<script setup>
import { useStore } from "./store/index.js";
import { storeToRefs } from "pinia";
const store = useStore();
let { counter } = storeToRefs(store);
const addNum = () => {
  counter.value++;
};
</script>
```

### 分模块

直接分文件

### 持久化存储

```bash
npm i pinia-plugin-persist
```

```js
// src/store/index.js
import { createPinia } from 'pinia'

import piniaPluginPersist from 'pinia-plugin-persist'

const store = createPinia()
store.use(piniaPluginPersist)

export default store
```

```js
// src/store/use.js

import { defineStore } from "pinia";

export const useStore = defineStore("storeId", {
    state: () => {
        return {
            counter: 0
        }
    },
    getters: {},
    actions: {},
    //开启数据缓存
    persist: {
        enabled: true,
        strategies: [
            {
                key: 'counter',
                storage: localStorage,
                // 按需配置需要持久化的数据
                paths: ["counter"]
            }
        ]
    }
})
```

```js
// src/main.js

import { createApp } from 'vue'
import App from './App.vue'
import store from './store'
createApp(App).use(store).mount('#app')
```

```vue
<!-- src/App.vue -->

<template>
  {{ counter }}
  <br />
  <button @click="addNum">counter++</button>
</template>

<script setup>
import { useStore } from "./store/use.ts";
import { storeToRefs } from "pinia";
const store = useStore();
let { counter } = storeToRefs(store);
const addNum = () => {
  counter.value++;
};
</script>
```

