---
title: 3 操作系统信息os
top: 3
tags:
  - nodejs
categories:
  - nodejs
---

```js
//获取操作系统的信息
var os =require('os');

//用来操作路径的
var path =require('path');

//获取当前操作系统的CPU信息
console.log(os.totalmem());

//memory 内存
console.log(os.cpus());

//extension name 获取扩展名 
console.log(path.extname('2.js'));
```

