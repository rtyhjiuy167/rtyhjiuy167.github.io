#### 全局配置

即在`nuxt.config.js`中进行配置

#### 单页配置

例如在`根目录/pages/Home.vue`中进行配置：

```vue
<template>
  <div>home</div>
</template>

<script>
export default {
  name: "IndexPage",
  head() {
    return {
      title: "标题",
      meta: [
        { hid: "description", name: "description", content: "此处是网站描述" },
       { hid: "keywords", name: "keywords", content: "此处是网站关键字" },
      ],
    };
  },
};
</script>

```

