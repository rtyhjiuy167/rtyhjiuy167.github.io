### 使用 ref 获取 dom 元素

```vue
<template>
  <input type="text" ref="input" value="1234">
</template>

<script setup lang="ts">
import { onMounted,ref } from "vue";
    
// 声明的变量名必须为 dom 结点中的 ref 的值
const input = ref<HTMLElement | null>(null);

onMounted(() => {
  console.log(input)
})
</script>
```

写法不同，效果相同：

```vue
<template>
  <input type="text" ref="input" value="1234">
</template>

<script setup lang="ts">
import { onMounted, Ref, ref } from "vue";
const input: Ref<HTMLElement | null> = ref(null);

onMounted(() => {
  console.log(input)
})
</script>
```

