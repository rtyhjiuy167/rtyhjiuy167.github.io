---
title: 0 MongoDB数据库的基本概念 
top: 0
tags:
  - mongodb
categories:
  - mongodb
---

## MongoDB数据库的基本概念

```js
{
    qq:{
        users:[
            {name:"张三",age:15},
            {name:"李四",age:15}
        ],
        products:[
            {},
            {}
        ]
    },
    taobao:{
        users:[
            {},
            {}
        ]
    }
}
//MongoDB数据库中有qq与taobao数据库，其中qq数据库中有users与products的集合（表），users集合中又有多个文档（表记录）
```

```js
const mongoose = require('mongoose');

//连接MongoDB数据库，使用test数据库
mongoose.connect('mongodb://localhost/test', { useNewUrlParser: true, useUnifiedTopology: true });

//创建一个模型
//就是在设计数据库
//Mongo是动态的，非常灵活，只需要在代码中设计数据库
//表名为 cats,转换成了小写并加了s  存储的为key为name,value要求为字符串型
const Cat = mongoose.model('Cat', { name: String });

//持久化，保存kitty实例
const kitty = new Cat({ name: 'Zildjian' });
kitty.save().then(() => console.log('meow'));
```

