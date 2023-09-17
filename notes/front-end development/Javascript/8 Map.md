---
title: 8 Map
top: 8
tags:
  - JavaScript
categories:
  - JavaScript
---

## Map

它类似于对象，也是键值对的集合。但是“键”的范围不限与字符串，各种类型的值（包括对象）都可以当作键。Map 也实现了 iterator 接口，所有可以使用扩展运算符和 for of 进行遍历

其属性如下：

 * size 
   * 返回 Map 的元素个数
 * set
   * 增加一个新元素，返回当前 Map。set() 方法返回映射实例，可以把多个操作连缀起来。
 * get
   * 返回键名对象的键值
 * has
   * 检测 Map 中是否包含某个元素，返回 boolean 值
 * delete
   * 删除指定键名的键值对
 * clear
   * 清空集合，返回 undefined
     

```javascript
let m = new Map();
m.set('name', '张三');
let key = {
    abc: "aaa"
};
m.set(key, ['1', '2']);
console.log(m);
//Map(2) { 'name' => '张三', { abc: 'aaa' } => [ '1', '2' ] }

for (let v of m) {
    console.log(v);
}
/*
    [ 'name', '张三' ]
    [ { abc: 'aaa' }, [ '1', '2' ] ]
*/
m.delete('name');
console.log(m);
//Map(1) { { abc: 'aaa' } => [ '1', '2' ] }

console.log(m.get(key)); //[ '1', '2' ]
```

```javascript
const key1 = { id: 1 }, key2 = { id: 2 }, key3 = { id: 3 };
const m = new Map().set(key1, "val1").set(key2, "val2").set(key3, "val3");
alert(m.get(key1));
```

## WeakMap

ES6 新增的“弱映射”是一种新的集合类型，为这门语言带来了增强的键/值对存储机制。WeakMap 是 Map 的“兄弟”类型，其 API 也是 Map 的子集。WeakMap 中的“weak”描述的是 JavaScript 垃圾回收程序对待“弱映射”中键的方式。

### 弱键

弱映射中的键只能是 Object 或者继承自 Object 的类型，尝试使用非对象设置键会抛出 TypeError。值的类型没有限制。

详见 p170

```javascript
// 该段代码中， set() 方法初始化了一个新对象并将它用作一个字符串的键。但因为没有指向这个对象的其他引用，所以当这行代码指向完成后，这个对象键就会被当作垃圾回收
const wm = new WeakMap();
wm.set({},"val");
```

