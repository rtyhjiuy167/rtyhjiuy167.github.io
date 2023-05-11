### 表单

##### 验证

1）通过`a-form`下的`a-button`的`html-type="submit"`属性触发

```vue
<template>
  <a-form :model="formState" :rules="rules">
    <a-form-item name="username">
      <a-input v-model:value="formState.username" />
    </a-form-item>
    <a-button type="primary" html-type="submit">Submit</a-button>
  </a-form>
</template>
<script setup>
import { reactive } from "vue";
const formState = reactive({
  username: "",
});

const rules = {
  username: [{ required: true, message: "Please input your username!" }],
};
</script>
```

2）通过`a-form`的`ref`的`validate()`触发

```vue
<template>
  <a-form :model="formState" :rules="rules" ref="formRef">
    <a-form-item name="username">
      <a-input v-model:value="formState.username" />
    </a-form-item>
    <a @click="handleClick">Submit</a>
  </a-form>
</template>
<script setup>
import { reactive } from "vue";
const formState = reactive({
  username: "",
});

const rules = {
  username: [{ required: true, message: "Please input your username!" }],
};
const formRef = ref();
const handleClick = () => {
  console.log(formRef.value.validate());
};
```

