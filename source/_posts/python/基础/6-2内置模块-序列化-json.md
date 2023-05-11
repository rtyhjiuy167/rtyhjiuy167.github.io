---
title: 6-2 内置模块-序列化-json
top: 6.2
tags:
  - python
categories:
  - python
---

```python
# 序列化 json
"""
JSON JavaScript Object Notation
JSON 是一个受 JavaScript 的对象字面量语法启发的轻量级的数据交换格式
    dumps()
    loads()
    dump()
    load()
"""
import json

vardict = {'name': 'admin', 'age': 20}
varlist = [1, 2, 3]
print(json.dumps(vardict), type(json.dumps(vardict)))  # {"name": "admin", "age": 20} <class 'str'>
print(json.dumps(varlist), type(json.dumps(varlist)))  # [1, 2, 3] <class 'str'>

# json不是二进制，不需要 b模式
# 写进文件
vardict = {'name': 'admin', 'age': 20}
with open('./data.json', 'w') as fp:
    json.dump(vardict, fp)
# 读json文件
with open('./data.json', 'r') as fp:
    print(json.load(fp))
```