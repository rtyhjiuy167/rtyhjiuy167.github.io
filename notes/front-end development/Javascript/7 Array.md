---
title: 7 Array
top: 7
tags:
  - JavaScript
categories:
  - JavaScript
---

## Array

 * 数组
   * 数组也是一个对象。
   * 它和我们普通对象功能类似，也是用来存储一些值的。
   * 不同的是普通对象是使用字符串作为属性名的。
   * 而数组是使用数字作为索引来操作元素的。

### 创建数组

通过构造函数创建数组：

```javascript
let arr = new Array(); // 有没有 new 都是一样的
console.log(typeof arr); //object
```

如果知道数组中元素的数量，那么可以给构造函数传入一个数值，然后 length 属性就会被自动创建并设置为这个值，例如：

```javascript
let colors = Array(20); // 创建长度为 20 的数组
console.log(colors.length); // 20
```

也可以给 Array 构造函数传入其他类型的值，此时则会创建只包含该特定值的数组。

另一种创建数组的方式是使用数组字面量表示法，此时不会调用 Array 构造函数：

```javascript
let colors = ["blue","green"];
```

Array构造函数还有两个 ES6 新增的用于创建数组的静态方法：from() 和 of() 。from() 用于将类数组结构转换为数组实例，而 of() 用于将一组参数转换为数组实例。例如：

```javascript
console.log(Array.from("Matt")); // ["M", "a", "t", "t"]

// 使用 Array.from() 可以对现有数组进行浅复制
const a1 = [1, 2, 3, 4];
const a2 = Array.from(a1);

console.log(a1 == a2); // false

a2[1] = 5;
console.log(a1); // [1, 2, 3, 4]
console.log(a2); // [1, 5, 3, 4]

// 可以使用任何可迭代对象
const iter = {
    *[Symbol.iterator]() {
        yield 1;
        yield 2;
        yield 3;
        yield 4;
    }
}
console.log(Array.from(iter));//[1, 2, 3, 4]
```

Array.from()  可以接受第二个可选的映射函数参数。这个函数可以直接增强新数组的值，而无须像调用 Array.from().map() 那样先创建一个中间数组。其还可以接受第三个可选参数，用于指定映射函数中 this 的值，但这个重写 this 值在箭头函数不适用。

```javascript
const a1 = [1, 2, 3, 4];
const a2 = Array.from(a1, x => x ** 2);
const a3 = Array.from(a1, function (x) { return x ** this.exponent }, { exponent: 2 });
console.log(a2); // [1, 4, 9, 16]
console.log(a3); // [1, 4, 9, 16]
```

Array.of() 可以把一组参数转换为数组：

```javascript
console.log(Array.of(1, 2, 3, 4)); // [1, 2, 3, 4]
```

### 数组空位

由于行为不一致和存在性能隐患，因此实践中要避免使用数组空位。如果确实需要空位，则显示地用 undefined 值代替。

### 数组索引

* 索引
  * 从0开始的整数就是索引。
  * 数组的存储性能比普通对象要好，在开发中会经常使用数组来存储一些数据。

### 数组长度

当修改数组长度时，如果修改的值小于元素总数，则会自动删除后面的元素。

### 检测数组

### 迭代器方法

在 ES6 中，Array 地原型上暴露了 3个用于检索数组内容地方法：keys()、values()和entries()。keys() 返回数组索引的迭代器，values() 返回数组元素地迭代器，而 entries() 返回索引值/对的迭代器：

```javascript
const a = ["foo", "bar", "baz", "qux"];

// 注意：keys()、values()和entries() 返回的是迭代器
console.log(Array.from(a.keys())); // [0, 1, 2, 3]
console.log(Array.from(a.values())); // ["foo", "bar", "baz", "qux"]
console.log(Array.from(a.entries())); // [0, "foo"],[1, "bar"], [2, "baz"],[3, "qux"]
```

可使用 ES6 的解构在循环中拆分键/值对：

```javascript
const a = ["foo", "bar", "baz", "qux"];
for (const [idx, element] of a.entries()) {
    console.log(idx);
    console.log(element);
}
```

### 复制和填充

```
array.fill(item,start,end)
```

```javascript
// copy.Within()
let ints,
    reset = () => ints = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
reset();

// 从 ints 中复制索引 0 开始的内容，插入到索引 3 开始的位置
// 在源索引或目标索引到达数组边界时停止
ints.copyWithin(3);
console.log(ints);// [0, 1, 2, 0, 1, 2, 3, 4, 5, 6]

reset();
// 从 ints 中复制索引 2 开始的内容，插入到索引 0 开始的位置
ints.copyWithin(0, 2);
console.log(ints);// [2, 3, 4, 5, 6, 7, 8, 9, 8, 9]

reset();
// 从 ints 中复制索引 0 开始到索引 3 结束的内容，插入到索引 4 开始的位置
ints.copyWithin(4, 0, 3);
console.log(ints);// [0, 1, 2, 3, 0, 1, 2, 7, 8, 9]
```

### 转换方法

valueOf() 返回的是数组本身，而 toString() 返回由数组中每个值的等效字符串拼接而成的一个逗号分隔的字符串。

```javascript
const colors = ["red", "green", "blue"];
console.log(colors.toString()); // red,green,blue
console.log(colors.valueOf()); // ["red", "green", "blue"]
console.log(colors); // ["red", "green", "blue"]

// 如果想使用不同的分隔符，则可以使用 join() 方法
console.log(colors.join("||")); // red||green||blue
```

### 栈方法

```javascript
let colors = Array();
// push() 方法接受任意数量的参数，并将它们添加到数组末尾，返回数组的最新长度
let count = colors.push("red", "green", "blue");
console.log(count);// 3

// pop() 方法用于删除数组的最后一项，同时减少数组的 length 值，返回被删除的项
let item = colors.pop();
console.log(item);// blue
```

### 队列方法

```javascript
let colors = Array();
// unshift() 在数组开头添加任意多个值
colors.unshift("red", "green", "blue");

// 取得第一项
let item = colors.shift();
console.log(item);// red
```

### 排序方法

sort() 方法会在每一项上调用 String() 转型函数，然后比较字符串来决定顺序。即使数组的元素都是数值，也会把数组转换为字符串再比较、排序。

```javascript
let values1 = ["ab", "bve", "c", "es", "ac"];
values1.sort();
console.log(values1); // ["ab", "ac", "bve", "c", "es"]

let values2 = [10, 5, 21, 66];
values2.sort();
console.log(values2); // [10, 21, 5, 66]
```

sort() 方法可以接受一个比较函数，用于判断哪个值应该排在前面。

```javascript
let values = [10, 5, 21, 66];
// 比较函数接受两个参数，如果第一个参数应该排在第二个参数前面，就返回负值；
// 如果两个参数相等，就返回 0；
// 如果第一个参数应该排在第二个参数后面，就返回正值
function compare(value1, value2) {
    if (value1 < value2) {
        return -1;
    } else if (value1 > value2) {
        return 1;
    } else {
        return 0;
    }
}
values.sort(compare);
console.log(values); // [5, 10, 21, 66]
```

```javascript
let values = [10, 5, 21, 66];
// 降序 简写形式1
values.sort((a, b) => a < b ? 1 : a > b ? -1 : 0);
console.log(values); // [66, 21, 10, 5]

// 降序 简写形式2
values.sort((a, b) => b - a);
console.log(values); // [66, 21, 10, 5]
```

### 操作方法

concat()

splice(开始位置，要删除元素的数量，要插入的元素)



flat() 将多维数组转化为低位数组 

```javascript
const arr1 = [1, 2, 3, 4, [2, 3]];
const arr2 = [1, 2, 3, 4, [2, 3, 4, [3, 5, 6]]];
console.log(arr1.flat());
//[ 1, 2, 3, 4, 2, 3 ]
console.log(arr2.flat());
//[ 1, 2, 3, 4, 2, 3, 4, [ 3, 5, 6 ] ]

//参数为深度 是一个数字 默认为一
console.log(arr2.flat(2));
/*
    [
        1, 2, 3, 4, 2,
        3, 4, 3, 5, 6
    ]
*/
```

### 搜索和位置方法

ECMAScript 提供了3个严格相等的搜索方法：indexOf()、lastIndexOf() 和 includes() 。这些方法都接受两个参数：要查找的元素和一个可选的起始搜索位置。

```javascript
let numbers = [1, 2, 3, 4, 2, 1, 4, 2, 3];
console.log(numbers.indexOf(4)); // 3
console.log(numbers.lastIndexOf(4)); // 6
console.log(numbers.includes(4)); // true
```

ECMAScript 允许按照定义的断言函数搜索数组，每个索引都会调用这个函数。断言函数的返回值决定了相应索引的元素是否被认为匹配。

find() 和 findIndex() 方法使用了断言函数。这两个方法都从数组的最小索引开始。find() 返回第一个匹配的元素，findIndex() 返回第一个匹配元素的索引。这两个方法也都接受第二个可选的参数，用于指定断言函数内部 this 的值。

```javascript
const people = [
    {
        name: "xiaoming",
        age: 27
    },
    {
        name: "xiaowang",
        age: 29
    }
];

console.log(people.find((element,index,array) => element.age < 28)); // {name: "xiaoming", age: 27}
console.log(people.findIndex((element,index,array) => element.age < 28)); // 0


// 找到匹配项后，这两个方法都不再继续搜索
const evens = [2, 4, 6];
evens.find((item, idx, arr) => {
    console.log(`item:${item} idx:${idx} arr:${arr}`);
    return item === 4;
})
// item:2 idx:0 arr:2,4,6
// item:4 idx:1 arr:2,4,6
```

### 迭代方法

* every()：对数组每一项都运行传入的函数，如果对每一项函数都返回 ture ，则这个方法返回 true。
* filter()：对数组每一项都运行传入的函数，函数返回 true 的项会组成数组之后返回。
* forEach()：对数组每一项都运行传入的函数，没有返回值。
* map()：对数组每一项都运行传入的函数，返回由每次函数调用的结果构成的数组。
* some()：对数组每一项都运行传入的函数，如果有一项函数返回 ture，则这个方法返回 true。

```javascript
/** 
*forEach()方法需要一个函数作为参数
* 像这种函数，由我们创建但是不由我们来调用，而是由浏览器来调用，我们称为回调函数
* 
* forEach()方法 IE9及以上
*  数组中有几个元素函数就会执行几次，每次执行时，浏览器会将遍历到的元素
*  以实参的形式传递进来，我们可以定义形参来读取这些内容
*  
* 实际上浏览器会在回调函数中传递三个参数
*   第一个参数：当前正在遍历的元素
*   第二个函数：当前正在遍历的元素的索引
*   第二个函数：当前的数组
*/
var arr = ["孙悟空", "猪八戒", "沙和尚"];
arr.forEach(function (a, b, c) {
    console.log("a = " + a + "; b = " + b + "; c = " + c);
})
/*
a = 孙悟空; b = 0; c = 孙悟空,猪八戒,沙和尚
a = 猪八戒; b = 1; c = 孙悟空,猪八戒,沙和尚
a = 沙和尚; b = 2; c = 孙悟空,猪八戒,沙和尚
*/
```

### 归并方法

reduce() 和 reduceRight() 函数接受4个参数：上一个归并值、当前项、当前项的索引和数组本身。

```javascript
let values = [1, 2, 3, 4, 5];
let sum = values.reduce((prev, cur, index, array) => {
    console.log(`prev:${prev} cur:${cur} index:${index} array:${array}`);
    return prev+cur;
});
// prev:1 cur:2 index:1 array:1,2,3,4,5
// prev:3 cur:3 index:2 array:1,2,3,4,5
// prev:6 cur:4 index:3 array:1,2,3,4,5
// prev:10 cur:5 index:4 array:1,2,3,4,5
console.log(sum); // 15
```

reduceRight() 方法与之类似，只是方向相反而已。
