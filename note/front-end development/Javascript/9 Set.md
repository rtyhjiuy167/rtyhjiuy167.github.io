---
title: 9 Set
top: 9
tags:
  - JavaScript
categories:
  - JavaScript
---

### Set

ES6 新增的 Set 是一种新集合类型，为这么语言带来集合数据结构。 Set 在很多方面都像是加强的 Map,这是因为它们的大多数 API 和行为都是共有的。

```javascript
let s = new Set();
console.log(s, typeof s);

//可传入可迭代数据 可实现去重
let s2 = new Set([1, 2, 3, 1, 2, 5, 4, 3, 2, 1,]);
console.log(s2)
/*
 Set(5){1,2,3,5,4}
 [[Entries]]
 0: 1
 1: 2
 2: 3
 3: 5
 4: 4
 size: (...)
 __proto__: Set
*/

// 输出元素个数
console.log(s2.size)
// 添加新元素
s2.add("新增元素");
//检测是否含有某元素
console.log(s2.has("2"));

//集合实现了iterator 接口 可以使用 for of
for (let v of s2) {
    console.log(v)
}

//清空
s2.clear;
```

### set的使用

```javascript
//数组去重
let arr = [1, 2, 3, 4, 5, 4, 3, 2, 1];
let result1 = [...new Set(arr)];
console.log(result1); //[1, 2, 3, 4, 5]

//求交集
let arr2 = [3, 4, 5, 6, 5, 4];
//先去重，这样再比对效率高
let result2 = [...new Set(arr2)].filter(item => new Set(arr2).has(item));
console.log(result2); //[3, 4, 5, 6]

//求并集
let union = [...new Set([...arr, ...arr2])];
console.log(union); //[1, 2, 3, 4, 5, 6]

//求差集
let diff = [...new Set(arr)].filter(item => !(new Set(arr2).has(item)));
console.log(diff); //[1, 2]
```

### 死循环

```javascript
let s = new Set([1]);
s.forEach((item, idx) => {
    s.delete(item);
    s.add(item);
    console.log(item, idx);
});
```

其余详见 P175