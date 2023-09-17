```bash
npm i vue-router@4
```

## Vue3+Vite 配置

`src/router/index.js`文件内容：

```js
// src/router/index.js
import { createRouter, createWebHistory } from "vue-router";
import Home from '../views/Home.vue'

const routes = [
    {
        path: "/",
        name: "Home",
        component: Home,
    },
    {
        path: "/about",
        name: "About",
        component: () => import("../views/About.vue")
    }
];

const router = createRouter({
    history: createWebHistory('/'),
    routes
});

router.beforeEach((to, from, next) => {
    next()
})


export default router;
```

在 main.js 中引入：

```js
// src/main.js
import { createApp } from 'vue'
import App from './App.vue'
import router from './router/index.js'
createApp(App).use(router).mount('#app')
```

```vue
<!-- src/App.vue -->

<template>
     <router-view />
</template>
```

```vue
<!-- src/views/Home.vue -->

<template>
  <router-link to="/about">关于我们</router-link>
  <button @click="go">跳转</button>
</template>

<script setup>

// useRoute 是当前路由 
// useRouter 是大对象
let uses = useRouter();
let go = () => {
  uses.push("/about");
};
</script>

<style>
</style>
```

```vue
<!-- src/views/About.vue -->
<template>
  about
</template>
```

## Vue3+Vite+TS 配置

`src/router/index.ts`文件内容：

```js
// src/router/index.ts
import { createRouter, createWebHashHistory, RouteRecordRaw } from "vue-router";

const routes: Array<RouteRecordRaw> = [
    {
        name: "A",//命名路由跳转的 name
        path: '/',
        component: () => import('../views/A/index.vue')
    }, {
        name: "B",
        path: '/B',
        component: () => import('../views/B/index.vue')
    },

]

const router = createRouter({
    history: createWebHashHistory(),
    routes
})

export default router
```

在 main.ts 中引入：

```js
// src/main.ts
import { createApp } from 'vue'
import App from './App.vue'
import router from './router/index'
createApp(App).use(router).mount('#app')
```

```vue
<!-- src/App.vue -->

<template>
     <router-view />
</template>
```

```vue
<!-- src/views/A/index.vue -->
<template>
    <div>
        A
    </div>
    <router-link to="/B">跳转到B</router-link>
</template>
```

```vue
<!-- src/views/B/index.vue -->
<template>
    <div>
        B
    </div>
    <router-link to="/">跳转到A</router-link>
</template>
```

## Vue3+TS 路由的详细使用方法

#### 每次切换路由时，相应的组件会被销毁或挂载

`src/main.ts`、`src/router/index.ts`、`src/App.vue`文件内容如`Vue3+Vite+TS 配置`小节。

```vue
<!-- src/views/A/index.vue -->
<template>
    <div>
        A
    </div>
    <router-link to="/B">跳转到B</router-link>
</template>

<script setup lang="ts">
import { onMounted, onUnmounted } from "vue";
//在实例挂载完成后被调用
onMounted(() => {
    console.log("A挂载")
})

//卸载组件实例后调用
onUnmounted(() => {
    console.log("B卸载")
})
</script>
```

```vue
<!-- src/views/B/index.vue -->
<template>
    <div>
        B
    </div>
    <router-link to="/">跳转到A</router-link>


</template>

<script setup lang="ts">
import { onMounted, onUnmounted } from "vue";
//在实例挂载完成后被调用
onMounted(() => {
    console.log("B挂载")
})
//卸载组件实例后调用
onUnmounted(() => {
    console.log("B卸载")
})
</script>
```

#### 传递 query 参数

`src/main.ts`、`src/router/index.ts`、`src/App.vue`文件内容如`Vue3+Vite+TS 配置`小节。

传递的属性值均为 string 类型。

```vue
<!-- src/views/A/index.vue -->
<template>
    <div>
        A
    </div>
    <router-link to="/B">跳转到B</router-link>
</template>

<script setup lang="ts">
import { onMounted } from "vue";
import { useRoute } from "vue-router";

// 获取当前路由规则信息
const route = useRoute();
onMounted(() => {
    console.log(route.query)//Object { uname: "123", pwd: "qwe" }
})
</script>
```

```vue
<!-- src/views/B/index.vue -->
<template>
    <div>
        B
    </div>
    <!-- 路由跳转并携带 query 参数 -->
    <!-- to 的字符串写法 -->
    <router-link to="/?uname=123&pwd=qwe">跳转到A</router-link>
    <br>
    <!-- to 的对象写法 -->
    <router-link :to="{
        path: '/',
        query: {
            uname: 123, // 写的是 number ，但传递时会自动转成 string
            pwd: 'qwe'
        }
    }">跳转到A</router-link>

</template>
```

#### 命名路由跳转

`src/main.ts`、`src/router/index.ts`、`src/App.vue`文件内容如`Vue3+Vite+TS 配置`小节。

使用命名跳转路由必须使用 to 的对象写法。

```vue
<!-- src/views/A/index.vue -->
<template>
    <div>
        A
    </div>
    <router-link to="/B">跳转到B</router-link>
</template>

<script setup lang="ts">
import { onMounted } from "vue";
import { useRoute } from "vue-router";

// 获取当前路由规则信息
const route = useRoute();
onMounted(() => {
    console.log(route.query)//Object { uname: "123", pwd: "qwe" }
})
</script>
```

```vue
<!-- src/views/B/index.vue -->
<template>
    <div>
        B
    </div>
	<!-- name 的属性值，必须与 src/router.indext.ts 中自己配置的 name 相同  -->
    <router-link :to="{
        name: 'A',
        query: {
            uname: 123, // 写的是 number ，但传递时会自动转成 string
            pwd: 'qwe'
        }
    }">跳转到A</router-link>

</template>

```

#### 传递 params 参数

`src/main.ts`、`src/App.vue`文件内容如`Vue3+Vite+TS 配置`小节。

传递的属性值均为 string 类型。

```tsx
// src/router/index.ts
import { createRouter, createWebHashHistory, RouteRecordRaw } from "vue-router";

const routes: Array<RouteRecordRaw> = [
    {
        
        name: "A",
        path: '/',
        component: () => import('../views/A/index.vue')
    }, {
        name: "B",
        path: '/B/:uname/:pwd', //  :开头表示占位符，传递 params 参数
        component: () => import('../views/B/index.vue')
    },

]

const router = createRouter({
    history: createWebHashHistory(),
    routes
})

export default router
```

```vue
<!-- src/views/A/index.vue -->
<template>
    <div>
        A
    </div>
    <router-link to="/B/123/qwe">跳转到B并携带 params 参数 字符串写法</router-link>
    <br>
    <router-link :to="{
        name: 'B',//命名路由跳转
        params: {
            uname: 123, // 写的是 number ，但传递时会自动转成 string
            pwd: 'qwe'
        }
    }">跳转到B并携带 params 参数 对象式写法</router-link>
</template>
```

```vue
<!-- src/views/B/index.vue -->
<template>
    <div>
        B
    </div>
    <router-link to="/">跳转到A</router-link>
</template>

<script setup lang="ts">
import { onMounted } from "vue";
import { useRoute } from "vue-router";
// 获取当前路由信息
const route = useRoute();
    
onMounted(() => {
    console.log(route.params) // Object { uname: "123", pwd: "qwe" }
})
</script>
```

#### 路由的 props 配置

`src/main.ts`、`src/App.vue`文件内容如`Vue3+Vite+TS 配置`小节。

传递的属性值均为 string 类型。

1.props的对象式写法

```tsx
// src/router/index.ts
import { createRouter, createWebHashHistory, RouteRecordRaw } from "vue-router";

const routes: Array<RouteRecordRaw> = [
    {

        name: "A",
        path: '/',
        component: () => import('../views/A/index.vue')
    }, {
        name: "B",
        path: '/B',
        component: () => import('../views/B/index.vue'),
        props: { uname: '123', pwd: 'qwe' }
    },

]

const router = createRouter({
    history: createWebHashHistory(),
    routes
})

export default router
```

```vue
<!-- src/views/A/index.vue -->
<template>
    <div>
        A
    </div>
    <router-link to="/B">跳转到B</router-link>
</template>

```

```vue
<!-- src/views/B/index.vue -->
<template>
    <div>
        B
    </div>
    <router-link to="/">跳转到A</router-link>
</template>

<script setup lang="ts">
import { onMounted } from "vue";
// 获取当前路由信息
const props = defineProps({
    uname: {
        type: String,
    },
    pwd: {
        type: String
    }
})
onMounted(() => {
    console.log(props.uname, props.pwd)
})
</script>
```

2.props的布尔值写法

```tsx
// src/router/index.ts
import { createRouter, createWebHashHistory, RouteRecordRaw } from "vue-router";

const routes: Array<RouteRecordRaw> = [
    {

        name: "A",
        path: '/',
        component: () => import('../views/A/index.vue')
    }, {
        name: "B",
        path: '/B',
        component: () => import('../views/B/index.vue'),
        props:true
    },

]

const router = createRouter({
    history: createWebHashHistory(),
    routes
})

export default router
```

```vue
<!-- src/views/A/index.vue -->
<template>
    <div>
        A
    </div>
    <br>
    <router-link :to="{
        name: 'B',//命名路由跳转
        params: {
            uname: '123',
            pwd: 'qwe'
        }
    }">跳转到B</router-link>
</template>
```

```vue
<!-- src/views/B/index.vue -->
<template>
    <div>
        B
    </div>
    <router-link to="/">跳转到A</router-link>
</template>

<script setup lang="ts">
import { onMounted } from "vue";
import { useRoute } from "vue-router";
// 获取当前路由信息
const route = useRoute();
const props = defineProps({
    uname: {
        type: String,
    },
    pwd: {
        type: String
    }
})
onMounted(() => {
    console.log(props.uname, props.pwd) // 123 qwe
    console.log(route.params) // Object { uname: "123", pwd: "qwe" }
})
</script>
```

3.props的函数式写法

```tsx
// src/router/index.ts
import { createRouter, createWebHashHistory, RouteRecordRaw } from "vue-router";

const routes: Array<RouteRecordRaw> = [
    {

        name: "A",
        path: '/',
        component: () => import('../views/A/index.vue')
    }, {
        name: "B",
        path: '/B',
        component: () => import('../views/B/index.vue'),
        props(route) {
            return { route: route }
        }
    },

]

const router = createRouter({
    history: createWebHashHistory(),
    routes
})

export default router
```

```vue
<!-- src/views/A/index.vue -->
<template>
    <div>
        A
    </div>
    <router-link :to="{ name: 'B' }">跳转到B</router-link>
</template>
```

```vue
<!-- src/views/B/index.vue -->
<template>
    <div>
        B
    </div>
    <router-link to="/">跳转到A</router-link>
</template>

<script setup lang="ts">
import { onMounted } from "vue";
const props = defineProps({
    route: {},
})
onMounted(() => {
    console.log(props.route)
})
</script>
```

#### router-link 的 replace

在 router-link 标签中 添加属性 replace ， 跳转路由时会替换当前路由

#### 缓存路由组件

`src/main.ts`、`src/router/index.ts`文件内容如`Vue3+Vite+TS 配置`小节。

```vue
<!-- src/views/A/index.vue -->
<template>
    <div>
        <div>
            A
        </div>
        <input type="text">
        <router-link replace :to="{ name: 'B' }">跳转到B</router-link>
    </div>
</template>
<script setup lang="ts">
import { defineComponent } from 'vue';
defineComponent({
    name: "A"
})
</script>
```

```vue
<!-- src/views/B/index.vue -->
<template>
	<!-- transition 要求每个包裹的组件必须有根标签 div -->
    <div>
        <div>
            B
        </div>
        <router-link to="/">跳转到A</router-link>
    </div>
</template>
```

```vue
<!-- src/App.vue -->
<template>
     <router-view v-slot="{ Component }">
         <!-- 组件切换时动画，可根据需要使用 -->
          <transition>
              <!-- 使用keep-alive 必须配合 v-slot 使用 -->
               <keep-alive>
                    <component :is="Component" />
               </keep-alive>
          </transition>
     </router-view>
</template>
```

### route、router的一些属性

```tsx
import { onMounted } from 'vue';
import { useRoute, useRouter } from "vue-router";

// 获取当前路由规则信息
const route = useRoute();

const router = useRouter();

onMounted(() => {
    //返回当前路径
    console.log(route.fullPath)
    //获取路由信息，返回元素的个数为当前所处路由级数
    console.log(route.matched)

    // 获取路由对象的配置的路由数组
    console.log(router.options.routes)
})
```

