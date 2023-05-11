---
title: 6-1 内置模块-序列化-pickle
top: 6.1
tags:
  - python
categories:
  - python
---

```python
# 系统内置模块就是安装python解释器后，系统可以提供的模块
#  在需要使用时，可在导入后使用
"""
序列化是指可以把python中的数据，以文本或二进制的方式进行转换
    文本序列化模块 json 其它的语言也能用
    二进制序列化模块 pickle 仅python专用
        dumps() 序列化 可以把python的任意对象序列化成一个二进制数据
        loads() 反序列化，可以把一个序列化后的二进制数据反序列化为python的对象
        dump() 序列化，把一个数据对象进行序列化并写入到文件中
        load() 反序列化，
"""
import pickle

vars = '1234'
res = pickle.dumps(vars)
print(res)  # b'\x80\x04\x95\x08\x00\x00\x00\x00\x00\x00\x00\x8c\x041234\x94.'
res = pickle.loads(res)
print(res)  # 1234

# 把一个python数据进行序列化后写入文件
# 使用 dumps 完成
vars = '1234'
res = pickle.dumps(vars)
with open('./data.txt', 'wb') as fp:
    fp.write(res)

# 把一个反序列的二进制文件读取出来，并完成反序列化
# 使用 loads 完成
with open('./data.txt', 'rb') as fp:
    print(pickle.loads(fp.read()))

# 把一个python数据进行序列化后写入文件
# 使用 dump 完成
vars = '1234'
with open('./data2.txt', 'wb') as fp:
    pickle.dump(vars, fp)

# 把一个反序列的二进制文件读取出来，并完成反序列化
# 使用 load 完成
with open('./data.txt', 'rb') as fp:
    print(pickle.load(fp))
```