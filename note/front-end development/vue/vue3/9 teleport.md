```vue
<!-- src/components/list.vue -->

<template>
<!--to后的双引号内填写类名、Id或标签名-->
<teleport to=".main">传送</teleport>
</template>

```

```vue
<!-- src/App.vue -->

<template>
  <div class="header">header</div>
  <div class="main">main</div>
  <div class="footer">footer</div>
  <List> </List>
</template>

<script setup>
import List from "./components/list.vue";
</script>
```

