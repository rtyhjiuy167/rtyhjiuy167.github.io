---
title: 16 pandas
top: 16
tags:
  - python
categories:
  - python
---

Pandas 官网：https://pandas.pydata.org/

Pandas 菜鸟教程：https://www.runoob.com/pandas/pandas-tutorial.html

<h4>Series</h4>

```python
"""
Series 一维 带标签的数组

"""
import pandas as pd

print(pd.Series([1, 2, 31, 12, 3]))
# 0     1
# 1     2
# 2    31
# 3    12
# 4     3
# dtype: int64

# 指定标签
s1 = pd.Series([1, 2, 31, 12, 3, 5], index=list("abcdef"))
print(s1)
# a     1
# b     2
# c    31
# d    12
# e     3
# f     5
# dtype: int64

# 用字典 键->索引
s2 = pd.Series({"name": "xiaohong", "age": 18, "gender": 1})
print(s2)
# name      xiaohong
# age             18
# gender           1
# dtype: object

# 修改数据类型
s1.astype(float)

# 取单个值
# 通过位置来取 默认索引
print(s2[0])  # xiaohong

# 通过键来取
print(s2["age"])  # 18

# 取多个值
# 取连续的值 通过默认索引
print(s2[:2])
# name    xiaohong
# age           18
# dtype: object

# 取不连续的值 通过默认索引
print(s2[[0, 2]])
# name      xiaohong
# gender           1
# dtype: object


# 取不连续的值 通过键
print(s2[["name", "gender"]])
# name      xiaohong
# gender           1
# dtype: object
```

<h4>DataFrame的创建</h4>

```python
import numpy
import pandas as pd

"""
DataFrame对象既有行索引，又有列索引
行索引，表明不同行，横向索引，为 index , 0轴 axis=0
列索引，表明不同列，纵向索引，为 column , 1轴 axis=1
"""
data1 = pd.DataFrame(numpy.arange(12).reshape(3, 4))
print(data1)
#    0  1   2   3
# 0  0  1   2   3
# 1  4  5   6   7
# 2  8  9  10  11

data2 = pd.DataFrame(numpy.arange(12).reshape(3, 4), index=list("abc"), columns=list("wxyz"))
print(data2)
#    w  x   y   z
# a  0  1   2   3
# b  4  5   6   7
# c  8  9  10  11

d1 = {"name": ["xiaoming", "zhangsan"], "age": [18, 20], "tel": [181, 165]}
data3 = pd.DataFrame(d1)
print(data3)
#        name  age  tel
# 0  xiaoming   18  181
# 1  zhangsan   20  165

d2 = [{"name": "xiaoming", "age": 18, "tel": 181}, {"name": "zhangsan", "age": 21, "tel": 111}]
data4 = pd.DataFrame(d2)
print(data4)
#        name  age  tel
# 0  xiaoming   18  181
# 1  zhangsan   21  111
```