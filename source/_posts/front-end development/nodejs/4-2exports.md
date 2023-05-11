---
title: 4-2 exports
top: 4.2
tags:
  - nodejs
categories:
  - nodejs
---

a.js：

```js
/**
 * require方法有两种作用
 *  1.加载文件模块并执行里面的代码
 *  2.拿到被加载文件模块导出的接口对象
 * 
 *  在每个文件模块中都提供了一个对象：exports
 *  exports 默认是空对象 其也是require方法的返回值
 *  在每个模块中需要把所有可被外部访问的成员挂载到exports对象中
 */
var bExports = require('./b');
console.log(bExports.b1);//undefined
console.log(bExports.b2);//b2
console.log(bExports.b3);//b3
```

b.js：

```js
var b1 = 'b1';
var b3 = 'b3';
exports.b1;
exports.b2 = 'b2';
exports.b3 = b3;
/**
 * 上面的导出默认是一个对象
 * 如果一个模块需要直接导出一个方法或者数组而非像上面那样挂载，应：
 * module.exports =  xxx
 * 这样导出就直接得到 xxx
 */
/**
 * 导出多个可以这样
 * module.exports = {//不能写成 exports =
 * //exports只是引用module.exports
 * 	a:123，
 *  b:234,
 *  }
 */
//在Node中。每个模块内部都有一个module对象
// moudule对象中有一个成员exports，其也是一个对象
// exports === module.exports //true
// 模块里有这样的代码 var exports = module.exports
//exports是对module.exports的引用
// 默认exports与module.exports指向一致
// require是 return的是module.exports
```

