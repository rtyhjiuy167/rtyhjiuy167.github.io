自己配置路由需要安装插件：

```bash
npm i @nuxtjs/router -S
```

在`nuxt.config.js`里添加相应配置：

```js
export default {
	//...
  modules: [
    '@nuxtjs/router'
  ],
	//...
}

```

在根目录创建`router.js`文件，文件内容如下：

```js
import Vue from "vue"
import VueRouter from "vue-router"
// Nuxt 因数据要全部加载，所以不用懒加载
import Home from '@/pages/Home'
import Login from '@/pages/Login'
Vue.use(VueRouter);

const routes = [
    {
        path: "/",
        name: "Home",
        component: Home
    },
    {
        path: "/login",
        name: "Login",
        component: Login,
        //beforeEnter 会在服务端和客户端都运行
        //注意服务端中没有 localStorage、cookies
        beforeEnter(to, from, next) {
            console.log("路由独享守卫")
            next()
        }
    }
]
const router = new VueRouter({
    mode: 'history',
    routes
})

router.beforeEach((to, from, next) => {
    next()
})

router.afterEach(() => { })

// 注意 vuxt 对外暴露的是一个函数
export function createRouter() {
    return router
}
```

