#### 重构Vue

```js
// 重构的写法
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)

// nuxt 中必须返回函数
const store = ()=> new Vue.store({
    module:{}
})

export default store;
```

### vuex里的写法

```js
export const state = () => {
    return {
    }
}
export const mutations = {
    
}
export const actions = {
    nuxtServerInit(store, context) {
    }
}
```

