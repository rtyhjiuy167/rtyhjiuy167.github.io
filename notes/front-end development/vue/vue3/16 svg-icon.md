#### svg-icon

针对 vue.js 的配置：

```bash
 ### 安装 vite-plugin-svg-icons
 npm i vite-plugin-svg-icons -D

 ### 安装 fast-glob
 npm i fast-glob@3 -D
```

#### 修改main文件

`src/main.js`或`src/main.ts`：

```js
import { createApp } from 'vue'
import App from './App.vue'
import SvgIcon from './components/svgIcon/index.vue'
import 'virtual:svg-icons-register' 
const app = createApp(App)
app.component('SvgIcon', SvgIcon)
app.mount('#app')
```

#### 修改配置文件

`vite-config.js`或`vite.config.ts`：

```js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import { createSvgIconsPlugin } from 'vite-plugin-svg-icons'
import { resolve } from 'path'

export default defineConfig({
  plugins: [
    vue(),
    createSvgIconsPlugin({
      // 指定需要缓存的图标文件夹
      iconDirs: [resolve(process.cwd(), 'src/assets/icons/svg')],
      // 指定symbolId格式
      symbolId: 'icon-[dir]-[name]',
    }),
  ]
})
```

#### 放入 svg 文件

将`.svg`文件放入`src/assets/icons/svg`文件夹中。

#### 创建组件

`src/components/SvgIcon/index.vue`

```vue
// 注册自定义图标
<template>
  <svg :class="svgClass" aria-hidden="true">
    <use :xlink:href="iconName" :fill="color" />
  </svg>
</template>

<script setup lang="ts">
import { computed } from 'vue';
const props = defineProps({
  iconClass: { // 图标名称与assets/icon/svg下使用的文件名一致
    type: String,
    required: true
  },
  className: { // 给图标添加class
    type: String,
    default: ''
  },
  color: { // 设置图标颜色
    type: String,
    default: '#333'
  }
});
const iconName = computed(() => {
  return `#icon-${props.iconClass}`;
});
const svgClass = computed(() => {
  if (props.className) {
    return `svg-icon ${props.className}`;
  }
  return 'svg-icon';
});
</script>

<style scope>
.sub-el-icon,
.nav-icon {
  display: inline-block;
  font-size: 15px;
  margin-right: 12px;
  position: relative;
}

.svg-icon {
  width: 1em;
  height: 1em;
  position: relative;
  fill: currentColor;
  vertical-align: -2px;
}
</style>
```

#### 使用

`src/App.vue`:

```vue
<template>
  <svg-icon icon-class="phone" ></svg-icon>
</template>
```

#### svg 下载

iconfont-阿里巴巴：https://www.iconfont.cn/

xicons：https://xicons.org/#/