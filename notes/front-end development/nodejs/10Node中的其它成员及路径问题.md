---
title: 10 Node中的其它成员及路径问题
top: 10
tags:
  - nodejs
categories:
  - nodejs
---



在每个模块中，除了`require`、`exports`等模块相关的API之外，还有两个特殊的成员：

`__dirname`可以用来动态获取当前文件模块所属目录的绝对路径

`__filename`可以用来动态获取当前文件的绝对路径

```js
console.log(__dirname);
console.log(__filename);
```

在执行`node`命名时，模块中的`./`只是相对于执行`node`命名所处的终端路径<br>`./`不是相对与文件，而是相对于执行`node`命名所处的终端路径

所以这会造成在文件操作时，适用相对路径是不可靠的，应改为绝对路径，而在拼接路径中最好使用`path.join()`方法，其可实现智能拼接

```js
const fs = require('fs')

fs.readFile(path.join(__dirname,'./a.txt'), 'utf8', (err, data) => {
    if (err) {
        throw err;
    } else {
        console.log(data);
    }
})
```

