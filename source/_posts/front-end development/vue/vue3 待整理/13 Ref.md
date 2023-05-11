### toRef

可以用来为源响应式对象上的某个 property 新创建一个 ref 。然后，ref 可以被传递，它会保持对其源 property 的响应式连接。

```vue
<template>
    person:{{ person }}
    <h2>姓名：{{ person.name }}</h2>
    <h2>年龄：{{ person.age }}</h2>
    <h2>c:{{ person.a.b.c }}</h2>
    <button @click="name += '~'">修改姓名</button>
    <button @click="age++">增长年龄</button>
    <button @click="c++">c加1</button>
</template>
<script setup lang="ts" >
import { reactive, toRef } from 'vue';

let person = reactive({
    name: '张三',
    age: 18,
    a: {
        b: { c: 1 }
    }
});

// 使用 toRef 包裹的对象应为被 reactive或ref 包裹，不然没意义
let name = toRef(person, 'name');
let age = toRef(person, 'age');
let c = toRef(person.a.b, 'c');
</script>

```

### Refs

将响应式对象转换为普通对象，其中结果对象的每个 property 都是指向原始对象相应 property 的 ref。

```vue
<template>
    person:{{ person }}
    <h2>姓名：{{ person.name }}</h2>
    <h2>年龄：{{ person.age }}</h2>
    <h2>c:{{ person.a.b.c }}</h2>
    <button @click="x.name.value += '~'">修改姓名</button>
    <button @click="x.age.value++">增长年龄</button>
    <button @click="x.a.value.b.c++">c加1</button>
</template>
<script setup lang="ts" >
import {reactive, toRefs } from 'vue';

let person = reactive({
    name: '张三',
    age: 18,
    a: {
        b: { c: 1 }
    }
});

// 使用 toRefs 包裹的对象应为被 reactive 包裹
let x = toRefs(person)
//  取值时，子一级须加 .value ，后面就不在需要
</script>
```

### ToRaw

只是把数据变回非响应式数据而已

```vue
<template>
    <h2>姓名：{{ person.name }}</h2>
    <h2>年龄：{{ person.age }}</h2>
    <h2>c:{{ person.a.b.c }}</h2>
    <button @click="person.name += '~'">修改姓名</button>
    <button @click="person.age++">增长年龄</button>
    <button @click="person.a.b.c++">c加1</button>
    <button @click="showRawPerson">展示原始数据</button>
</template>
<script setup lang="ts" >
import { reactive, ref, toRaw } from 'vue';
let person = reactive({
    name: '张三',
    age: 18,
    a: {
        b: { c: 1 }
    }
});
const showRawPerson = () => {
    const p = toRaw(person)
    p.age++
    console.log(p)
}

</script>

```

