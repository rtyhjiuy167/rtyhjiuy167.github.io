---
title: 15 numpy
top: 15
tags:
  - python
categories:
  - python
---

<h4>数组的创建</h4>

```python
import numpy as np
import random

# np.array() 返回一个数组 接收一个可可迭代对象
t1 = np.array([1, 2, 3])
print(t1, type(t1))  # [1 2 3] <class 'numpy.ndarray'>

t2 = np.array(range(5))
print(t2, type(t2))  # [0 1 2 3 4] <class 'numpy.ndarray'>

# np.array(range()) 等价于 np.arange()
t3 = np.arange(5)
print(t3, type(t3))  # [0 1 2 3 4] <class 'numpy.ndarray'>

# dtype 可查看数组存储的类型
print(t3.dtype)  # int32

# 指定数组的存储类型
t4 = np.arange(4, dtype=float)
print(t4.dtype)  # float64

t5 = np.array([1, 1, 0, 0, 1], dtype=bool)
print(t5.dtype)  # bool

# 修改数组的存储类型 返回修改后的一个新数组
t6 = t5.astype("int8")
print(t6.dtype)  # int8

t7 = np.array([random.random() for i in range(4)])
print(t7)  # [0.01410859 0.5743651  0.28246758 0.56659701]
print(t7.dtype)  # float64
# 将可迭代对象里的每个元素保留两位小数 四舍五入 返回一个新数组
t8 = np.round(t7, 2)
print(t8)  # [0.01 0.57 0.28 0.57]
```

<h4>数组的形状（维数）</h4>

```python
import numpy as np

# 一维数组
t1 = np.arange(12)
print(t1)  # [ 0  1  2  3  4  5  6  7  8  9 10 11]
print(t1.shape)  # (12,)
# 二维数组 (行数 每行的列数)
t2 = np.array([[1, 2, 3], [4, 5, 6]])
print(t2.shape)  # (2, 3)
# 三维数组 (块 每块中的行数 每行的列数)
t3 = np.array([[[1, 2, 3], [4, 5, 6]], [[7, 8, 9], [10, 11, 12]]])
print(t3.shape)  # (2, 2, 3)
print(t3)
# [[[ 1  2  3]
#   [ 4  5  6]]
#
#  [[ 7  8  9]
#   [10 11 12]]]

# 将三维数组修改成二维数组
t4 = t3.reshape((3, 4))
print(t4)
# [[ 1  2  3  4]
#  [ 5  6  7  8]
#  [ 9 10 11 12]]

# 将二维数组修改成一维数组
t5 = t4.reshape((12,))
print(t5)  # [ 1  2  3  4  5  6  7  8  9 10 11 12]

# 当不知到二维数组的行数与列数时
t6 = t4.reshape((t4.shape[0] * t4.shape[1],))
print(t6)  # [ 1  2  3  4  5  6  7  8  9 10 11 12]

# 将多维数组降为一维
# flatten() 分配了新的内存 修改时，不会影响原数组
# ravel() 返回的是一个数组的视图 修改时，会影响原数组 浅
print(t3.flatten())  # [ 1  2  3  4  5  6  7  8  9 10 11 12]
print(t2.flatten())  # [1 2 3 4 5 6]
print(t3.ravel())  # [ 1  2  3  4  5  6  7  8  9 10 11 12]

# 直接生成多维数组
t7 = np.arange(0, 6, 1).reshape(6, 1)
print(t7)
# [[0]
#  [1]
#  [2]
#  [3]
#  [4]
#  [5]]

t8 = np.arange(0, 6, 1)[:, np.newaxis] # 与上一个生成多维数组的方法等价
print(t8)
# [[0]
#  [1]
#  [2]
#  [3]
#  [4]
#  [5]]

t9 = np.arange(0, 10, 1)[np.newaxis, :]
print(t9)  # [[0 1 2 3 4 5 6 7 8 9]]
```

<h4>数组的计算</h4>

```python
import numpy as np

# 一维数组
t1 = np.arange(6)

# 数组与数的计算 算术 比较 
# 对每个元素都作用（广播原则） 返回一个新数组
print(t1 + 666)  # [666 667 668 669 670 671 672 673 674 675 676 677]
print(t1 / 0)  # 在numpy里 非0数除以0 会得到无穷   0/0 会得  not a number
# [666 667 668 669 670 671]
#  [nan inf inf inf inf inf]

"""
如果两个数组的后缘维度（即从末尾开始算起的维度）的轴长度相符或其中一方的长度为1，则认为它们时广播兼容的。
广播会在缺失和（或）长度为1的维度上进行
例如 
    shape 为(3,3,3)不能和(3,2)的 计算
    shape 为(3,3,2)能和(3,2)的 计算
"""
# 数组与数组的计算 二维数组
t2 = np.array([[0], [1], [2], [3]])
t3 = np.array([[0, 1, 2, 3], [4, 5, 6, 7], [8, 9, 10, 11], [12, 13, 14, 15]])
print(t3 - t2)  # t3的每列都与t2的相应列去做运算
# [[ 0  1  2  3]
#  [ 3  4  5  6]
#  [ 6  7  8  9]
#  [ 9 10 11 12]]

```

<h4>读取本地数据</h4>

```python
"""
np.loadtxt(frame,dtype=np.float,delimiter=None,skiprows=0,usecols=None,unpack=False)
frame 文件、字符串或产生器，可以时.gz或bz2压缩文件
dtype 数据类型，可选 CSV的字符串以什么数据类型读入
delimiter 分隔字符串 默认为空格
skiprows 跳过前x行，一般跳过第一行表头
usecols 读取指定的列，索引，元组类型
unpack 为False 则按源数据从行开始读取，为 True 则将矩阵转置 默认为False
converters 数据转换
"""
import numpy as np


def iris_type(s):
    it = {b'Iris-setosa': 0, b'Iris-versicolor': 1, b'Iris-virginica': 2}
    return it[s]


us_file_path = "./data/iris.data"
#  converters={4: iris_type} 将第5列使用函数iris_type进行转换
t1 = np.loadtxt(us_file_path, delimiter=",", converters={4: iris_type}, unpack=True)

# 一般用 pandas来读文件csv，这样才能使用 info() head() 等重要接口，以快速了解信息
# 了解信息后，再用 numpy的方法快速导入，实现分类变量数值化
```

<h4>取数组的行、列、元素</h4>

```python
"""
轴 axis
在numpy中可以理解为方向 使用0、1、2...数字表示
    对于一维数组，只有一个0轴
    对于二维数组，有0轴和1轴 0轴指行 1轴指列
    对于三维数组，有0、1、2轴 0轴指块 1轴指行 2轴指列
"""
import numpy as np

t1 = np.arange(16).reshape(4, 4)
# 等价于：t1 = np.array([[0, 1, 2, 3], [4, 5, 6, 7], [8, 9, 10, 11], [12, 13, 14, 15]])

# 对于二维数组
# 取行 中括号内以第一个数字代表行索引
# 取单行
print(t1[1])  # [4 5 6 7]
print(t1[1, :])  # [4 5 6 7]

# 取连续的多行
print(t1[2:])
print(t1[2:, :])  # 两者等价 逗号左边的表示行 逗号右边表示列
# [[ 8  9 10 11]
#  [12 13 14 15]]


# 取不连续的多行
print(t1[[0, 2, 3]])
print(t1[[0, 2, 3], :])  # 两者等价
# [[ 0  1  2  3]
#  [ 8  9 10 11]
#  [12 13 14 15]]

# 取列 中括号内以第二个数字代表列索引
# 取单列
print(t1[:, 0])  # [ 0  4  8 12]

# 取多列
print(t1[:, 2:])
# [[ 2  3]
#  [ 6  7]
#  [10 11]
#  [14 15]]

# 取不连续的多列
print(t1[:, [0, 2]])
# [[ 0  2]
#  [ 4  6]
#  [ 8 10]
#  [12 14]]

# 取多行和多列
print(t1[2:5, 1:4])  # 可理解为取交叉点
# [[ 9 10 11]
#  [13 14 15]]

# 取多个不相邻的多个点
# [[ 行 ],[ 列 ]] 两个中括号 第一个中括号表示行 第二个表示列
print(t1[[0, 0, 0], [0, 2, 3]])  # [0 2 3]

# 其它索引方式
# 返回满足一定条件的元素 数组标识名[条件]
print(t1[t1 < 4])  # 返回由数组中小于4的元素组成的数组 # [0 1 2 3]
print(t1[np.isnan(t1)])  # 返回由数组中的所有nan元素组成的数组

# numpy的三元运算符 where() 返回一个数组
t2 = np.where(t1 < 4, 100, 200)  # 把t1数组里小于4的元素替换为100，其它替换为200
print(t2)
# [[100 100 100 100]
#  [200 200 200 200]
#  [200 200 200 200]
#  [200 200 200 200]]

```

<h4>数组拼接和拆分</h4>

```python
import numpy as np

t1 = np.array([[0, 1, 2, 3], [4, 5, 6, 7]])
t2 = np.array([[8, 9, 10, 11], [12, 13, 14, 15]])

# 竖直拼接
t3 = np.vstack((t1, t2))
print(t3)
# [[ 0  1  2  3]
#  [ 4  5  6  7]
#  [ 8  9 10 11]
#  [12 13 14 15]]

# 水平拼接
t4 = np.hstack((t1, t2))
print(t4)
# [[ 0  1  2  3  8  9 10 11]
#  [ 4  5  6  7 12 13 14 15]]

# zeros() 创建元素都为0的数组
t5 = np.zeros((2, 1)).astype(int)
print(t5)
# [[0]
#  [0]]

t6 = np.hstack((t1, t5))
print(t6)
# [[0 1 2 3 0]
#  [4 5 6 7 0]]

# 拆分数组
t7 = np.arange(16).reshape(4, 4)
# 第二个参数：沿轴切分的位置 axis默认为0
print(np.split(t7, (2, 3), axis=0))  # 在第2行后切断，在第三后切断
# [array([[0, 1, 2, 3],
#        [4, 5, 6, 7]]),
#  array([[ 8,  9, 10, 11]]),
#  array([[12, 13, 14, 15]])]


# ones() 创建元素都为1的数组
print(np.ones((2, 2)).astype(int))
# [[1 1]
#  [1 1]]

# 创建一个对角线全为1的正方形的数组
print(np.eye(4))
# [[1. 0. 0. 0.]
#  [0. 1. 0. 0.]
#  [0. 0. 1. 0.]
#  [0. 0. 0. 1.]]

# 获取对应轴最大值的位置
print(np.argmax(np.eye(4), axis=1))  # [0 1 2 3] 第一列的最大值在第一个，第二列在第二个...

# 获取对应轴最小值的位置
print(np.argmin(t1, axis=0))  # [0 0 0 0]

```

<h4>数组的行、列互换</h4>

```python
import numpy as np

t1 = np.arange(16).reshape(4, 4)

# 转置 行列交换
# transpose()
print(t1.transpose())

print(t1.T)

# swapaxes(1, 0)  轴数据交换 相当于转置
print(t1.swapaxes(1, 0))

t1[[1, 2], :] = t1[[2, 1], :]  # t1数组的第二行与第三行互换
print(t1)
# [[ 0  1  2  3]
#  [ 8  9 10 11]
#  [ 4  5  6  7]
#  [12 13 14 15]]

t1[:, [1, 2]] = t1[:, [2, 1]]  # t1数组的第二列与第三列互换
print(t1)
# [[ 0  2  1  3]
#  [ 8 10  9 11]
#  [ 4  6  5  7]
#  [12 14 13 15]]

```

<h4>随机数数组与数组元素排序</h4>

```python
"""
numpy生成随机数
    .random.rand(d0,d1,...,dn) 创建d0~dn维度的均匀分布的随机数数组，浮点数，范围0~1
    .random.randn(d0,d1,...,dn) 创建d0~dn维度的标准正态分布的随机数数组，浮点数
    .random.randint(low,high,(shape)) 从给定的上下限范围生成指定形状的随机数数组，整形，范围 low~high
    .random.uniform(low,high,(size)) 从给定的上下限范围生成指定形状的均匀分布的随机数数组，浮点数，范围 low~high
    .random.seed(s) 随机数种子 可通过设定相同的随机数种子，使每次生成相同的随机数数组
"""

import numpy as np

print(np.random.rand(2, 2))  # 创建两行两列的二维均匀分布随机数数组
# [[0.92126275 0.20863124]
#  [0.15398411 0.57929354]]
print(np.random.randn(3, 3))
# [[ 0.12808009 -0.34409545  0.16794586]
#  [-2.14140308 -1.19147045 -1.54585045]
#  [ 0.55013945 -0.44577289 -0.43342841]]

np.random.seed(10)  # 该行代码以下的数组将受到影响
print(np.random.randint(20, 30, ((2, 3))))  # 创建两行三列的范围在[20,30)随机数数组

print(np.random.randint(0, 256, 3)) # 创建 一维的、范围在[0,256)的有且只有 3个元素的数组

rng = np.random.RandomState(1)  # 随机数种子
sz = rng.rand(2, 2)
print(sz)  # 按照指定随机数种子生成指定维数的数组 如果写成 np.random.rand() 则受到上面的种子10的影响
# [[4.17022005e-01 7.20324493e-01]
#  [1.14374817e-04 3.02332573e-01]]

# .sort() 排序
res = np.sort(sz, axis=0)  # 这里针对0轴的每个元素来对其排序
print(res)
# [[1.14374817e-04 3.02332573e-01]
#  [4.17022005e-01 7.20324493e-01]
```

<h4>数组复制</h4>

```python
"""
对于数组a和b
    a=b 完全不复制，a和b相互影响
    a=b[:] 视图的操作，一种切片，会创建新的对象a，但是a的数据完全由b保管，它们两个的数据变化使一致的
    a=b.copy() 深拷贝
"""
```

<h4>nan inf</h4>

```python
import numpy as np

print(type(np.inf))  # <class 'float'>
print(type(np.nan))  # <class 'float'>
print(np.nan == np.nan)  # False
print(np.inf == np.inf)  # True

# 统计数组中nan的个数
# 方法1.  用count_nonzero和nan的特性来计算 .count_nonzero(数组标识名 != 数组标识名)
t = np.array([[0, 1, 2], [np.nan, 1, 3], [0, 0, np.nan]])
# .count_nonzero 统计不为0的个数
print(np.count_nonzero(t))  # 6

# 用 t != t 返回只有 False 和 True 元素的数组
print(t != t)
# [[False False False]
#  [ True False False]
#  [False False  True]]

# Ture相当于1 False相当于0
print(np.count_nonzero(t != t))  # 2

# 方法2计算nan的个数  .count_nonzero(.isnan(数组标识名))
print(np.count_nonzero(np.isnan(t)))  # 2

# nan参与算术运算 得到的仍是 nan
print(np.sum(t))  # nan

# 将nan的部分换成当前列的均值
def fill_ndarray(t1):
    for i in range(t.shape[1]):
        temp_col = t1[:, i]  # 得到当前的一列
        nan_num = np.count_nonzero(t1 != t1)  # 得到当前一列的nan的个数
        if nan_num != 0:  # 不为0，说明当前列有nan
            temp_not_nan_col = temp_col[temp_col == temp_col]  # 得到当前一列的不包含nan的数组
            # 得到没有nan的列，就可以正常运算了
            # 这里把是nan的部分换成了均值
            temp_col[np.isnan(temp_col)] = temp_not_nan_col.mean()
    return t1
```

<h4>数组中常用的统计函数</h4>

```python
import numpy as np

"""
    求和：t.sum()
    均值：t.mean()
    中值：t.median()
    最大值：t.max()
    最小值：t.min()
    极值：np.ptp() 即最大和最小值的差
    标准差：t.std()
"""
t = np.array([[0, 1, 2, 3], [4, 5, 6, 7], [8, 9, 10, 11], [12, 13, 14, 15]])
# np.sum() t1.sum() 求和
print(np.sum(t))  # 120
print(t.sum())  # 120

# 对竖轴上的每行求和
print(np.sum(t, axis=1))  # [ 6 22 38 54]
print(t.sum(axis=1))  # [ 6 22 38 54]

# 对横轴上的每列求和
print(np.sum(t, axis=0))  # [24 28 32 36]
print(t.sum(axis=0))  # [24 28 32 36]

# 获取对应轴最大值的位置
print(np.argmax(np.eye(4), axis=1))  # [0 1 2 3] 第一列的最大值在第一个，第二列在第二个...

# 获取对应轴最小值的位置
print(np.argmin(t, axis=0))  # [0 0 0 0]
```



