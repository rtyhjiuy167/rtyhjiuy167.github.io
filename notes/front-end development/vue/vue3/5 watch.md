## watch

```vue
<!-- src/App.vue -->

<template>
  <h2>1.求和为：{{ sum }}</h2>
  <button @click="sum++">1.点我+1</button>
  <h2>姓名：{{ person.name }}</h2>
  <h2>年龄：{{ person.age }}</h2>
  <h2>c:{{ person.a.b.c }}</h2>
  <button @click="person.name += '~'">修改姓名</button>
  <button @click="person.age++">增长年龄</button>
  <button @click="person.a.b.c++">c加1</button>
</template>

<script setup>
let sum = ref(0);
const person = reactive({
  name: "张三",
  age: 18,
  a: { b: { c: 666 } },
});
// 监视 ref 定义的一个基本数据的响应式数据 不需要加 value
watch(
  sum,
  (newValue, oldValue) => {
    console.log("sum变了", newValue, oldValue);
  },
  {
    immediate: true,
  }
);

// 坑1：
// 监视 ref 所定义的多个响应式数据 坑：无法逐个区别
// watch([], () => {});

// 坑2：
// 监视 reactive所定义的响应式对象 坑：现在的 Vue3 不能得到旧数据，
// watch(person, (newValue, oldValue) => {
//   console.log("new:", newValue, "old:", oldValue); //这里的oldValue 与newValue相同
//   console.log(newValue.age === oldValue.age); //true
//   console.log(newValue.name === oldValue.name); //true
// });
// 如果想要监视对象中的某一属性，应直接监视它，且应写成函数的形式:
watch(
  () => person.name,
  (newValue, oldValue) => {
    console.log(newValue, oldValue);
  }
);

// 坑3：
// watch 监视 reactive 强制开启深度监视，且无法关闭，deep属性不可设置；如果监视的是其属性 deep属性可设置
// watch(
//   person,
//   (newValue, oldValue) => {
//     console.log("我明明关闭了深度监视，咋还有呢？？");
//   },
//   { deep: false }

//#region
//#endregion
</script>
```

```vue
<!-- src/App.vue -->

<template>
  <h2>1.求和为：{{ sum }}</h2>
  <button @click="sum++">1.点我+1</button>
  <h2>姓名：{{ person.name }}</h2>
  <h2>年龄：{{ person.age }}</h2>
  <h2>c:{{ person.a.b.c }}</h2>
  <button @click="person.name += '~'">修改姓名</button>
  <button @click="person.age++">增长年龄</button>
  <button @click="person.a.b.c++">c加1</button>
</template>

<script setup>
let sum = ref(0);
const person = ref({
  name: "张三",
  age: 18,
  a: { b: { c: 666 } },
});
watch(
  sum,
  (newValue, oldValue) => {
    console.log("sum变了", newValue, oldValue);
  },
  {
    immediate: true,
  }
);

// watch(
//   person.value,//这样写检测的不再是 ref定义的数据，而是里面求助于 reactive 所定义的数据
//   (newValue, oldValue) => {
//     console.log("new:", newValue, "old:", oldValue);
//   });
watch(
  person,
  (newValue, oldValue) => {
    console.log("new:", newValue, "old:", oldValue);
  },
  { deep: true }
);
//以上两种写法监视value， oldValue 与 newValue 仍相同

//#region
//#endregion
</script>
```

