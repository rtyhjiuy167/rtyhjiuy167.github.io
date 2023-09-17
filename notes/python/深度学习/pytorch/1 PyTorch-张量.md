---
title: 1 PyTorch-张量
top: 1
tags:
  - PyTorch
categories:
  - PyTorch
---

<h4>张量的数据类型</h4>

|    数据类型    |             dytpe             |     CPU tensor     |       GPU tensor        |
| :------------: | :---------------------------: | :----------------: | :---------------------: |
|   32位浮点型   | torch.float32 或 torch.float  | torch.FloatTensor  | torch.cuda.FloatTensor  |
|   64位浮点型   | torch.float64 或 torch.double | torch.DoubleTensor | torch.cuda.DoubleTensor |
|   16位浮点型   |  torch.float16 或 torch.half  |  torch.HalfTensor  |  torch.cuda.HalfTensor  |
| 8位无符号整型  |          torch.uint8          |  torch.ByteTensor  |  torch.cuda.ByteTensor  |
| 8位有符号整型  |          torch.int8           |  torch.CharTensor  |  torch.cuda.CharTensor  |
| 16位有符号整型 |          torch.int16          | torch.ShortTensor  | torch.cuda.ShortTensor  |
| 32位有符号整型 |          torch.int32          |  torch.IntTensor   |  torch.cuda.IntTensor   |
| 64位有符号整型 |          torch.int64          |  torch.LongTensor  |  torch.cuda.LongTensor  |

```python
import torch

"""
在torch中默认的浮点数据类型是32位浮点型(torch.FloatTensor)
可通过 set_default_tensor_type() 函数查看
可通过 set_default_tensor_type() 函数设置默认的数据类型，但是该函数只支持设置浮点型数据类型

.dtype 获取张量的数据类型

.long() 将张量的数据类型转换成 torch.int64
.int() 将张量的数据类型转换成 torch.int32
.float() 将张量的数据类型转换成 torch.torch.float32
......转换时要考虑是否有溢出

.item() 将张量转化为真实值
"""
print(torch.get_default_dtype())  # torch.float32

tensor1 = torch.tensor([1.2, 3.4])
print(tensor1.dtype)  # torch.float32

tensor2 = torch.tensor([1, 3])
print(tensor2.dtype)  # torch.int64

torch.set_default_tensor_type(torch.DoubleTensor)
tensor3 = torch.tensor([1., 3.])
print(tensor3.dtype)  # torch.float64

tensor3 = torch.tensor([1.2, 3.4])
print("a.long()方法：", tensor3.long().dtype)  # a.long()方法： torch.int64
print("a.int()方法:", tensor3.int().dtype)  # a.int()方法: torch.int32
print("a.float()方法:", tensor3.float().dtype)  # a.float()方法: torch.float32

a = torch.tensor(5)
print(a)  # tensor(5)
print(a.item())  # 5

```

<h4>张量的生成</h4>

```python
import torch

"""
在pytorch中可以使用torch.tensor()函数来生成张量，而且可以根据指定的形状生成张量

.shape 可查看张量的维度
.size() 方法 返回张量的形状大小
.numel() 方法 返回张量中的元素的总数量

torch.tensor() 根据传入的列表或元组生成对应的张量

torch.Tensor() 
    传入列表或元组 生成对应的张量
    传入有限个整数 生成维数与参数个数相同、特定尺寸的张量

torch.ones_like() 生成与传入的张量形状大小相同的全1张量
torch.zeros_like() 生成与传入的张量形状大小相同的全0张量
torch.rand_like() 生成与传入的张量形状大小相同的服从标准正态分布的随机数张量

torch.tensor( , dtype= , requires_grad= )
    参数一：传入的列表或元组 
    dtype 数据类型 可选 有默认值
    requires_grad 是否需要计算梯度 默认为 False


.new_tensor() 生成与调用该方法的张量的类型相同、形状可不同的张量

.new_full(,fill_value=)
    参数一：元组 表示生成的张量的形状大小
    fill_value 张量中的元素值 （所有元素都为该值）

.new_ones() 生成与调用该方法的张量的类型相同、形状可不同的全1张量 相当于 .new_full(,fill_value=1)
.new_zeros() 生成与调用该方法的张量的类型相同、形状可不同的全0张量 相当于 .new_full(,fill_value=0)
.new_empty() 生成与调用该方法的张量的类型相同、形状可不同的空张量

torch.zeros()
torch.ones()
torch.empty()
torch.eye(n) 生成 n×n 的单位矩阵
torch.full( ,fill_value= )
"""

tensor1 = torch.tensor([1, 2, 3, 4])
print("tensor1：", tensor1)  # tensor1： tensor([1., 2., 3., 4.])
print("tensor1.shape：", tensor1.shape)  # tensor1.shape： torch.Size([4])
print("tensor1.size()：", tensor1.size())  # tensor1.size()： torch.Size([4])
print("tensor1.numel()：", tensor1.numel())  # tensor1.numel()： 4

tensor2 = torch.Tensor(2, 4)  # 生成的是三维张量，内容随机生成
print(tensor2.shape)  # torch.Size([2, 4])

tensor3 = torch.ones_like(tensor2)
print(tensor3)
# tensor([[1., 1., 1., 1.],
#         [1., 1., 1., 1.]])

tensor4 = torch.zeros_like(tensor2)
print(tensor4)
# tensor([[0., 0., 0., 0.],
#         [0., 0., 0., 0.]])

tensor5 = torch.rand_like(tensor2)
print(tensor5)
# tensor([[0.9163, 0.9765, 0.9319, 0.2779],
#         [0.9233, 0.5556, 0.6346, 0.2957]])

tensor6 = torch.tensor([1, 2, 3, 4, ], dtype=torch.float64, requires_grad=True)
print(tensor6)  # tensor([1., 2., 3., 4.], requires_grad=True)

tensor7 = tensor6.new_tensor([1., 2.])
print(tensor7.dtype)  # torch.float64

tensor8 = tensor7.new_full((1, 3), fill_value=1)
print(tensor8)  # tensor([[1., 1., 1.]], dtype=torch.float64)

tensor9 = tensor7.new_zeros((1, 3))
print(tensor9)  # tensor([[0., 0., 0.]], dtype=torch.float64)

tensor10 = tensor7.new_empty((1, 3))

```

```python
import torch

"""
随机数张量

torch.normal( , , )
    参数一 每个随机数所服从的分布的平均值
    参数二 每个随机数所服从的分布的标准差
    参数三 返回的张量的形状大小 
    
torch.normal(mean=,std=)
    函数中，通过mean参数指定随机数的均值，std参数指定随机数的标准差，如果mean参数和std参数有多个值，则生成多个随机数

torch.randperm() 将[0,n)之间的整数进行随机排序

torch.arange(start=, end=, step=) 生成指定的张量

torch.linspace(start=, end=, steps=) 在指定范围内生成指定数量的等间隔张量

torch.logspace(start=, end=, steps=) 在指定范围内生成指定数量的以对数为间隔的张量
"""

torch.manual_seed(123)  # 选定随机数种子

A = torch.normal(0.0, 1.0, (3,))
print("A：", A)  # A： tensor([0.1204])

B = torch.normal(mean=0.0, std=torch.tensor([1.0]))
print("B：", B)  # B： tensor([-0.2404])

C = torch.normal(mean=0.0, std=torch.tensor([1.0, 2.0]))
print("C：", C)  # C： tensor([-1.1969,  0.4185])

D = torch.rand(1, 3)
print("D：", D)  # D： tensor([[0.0756, 0.1966, 0.3164]])

E = torch.rand_like(D)
print("E：", E)  # E： tensor([[0.4017, 0.1186, 0.8274]])

print(torch.randperm(10))  # tensor([3, 2, 8, 6, 5, 7, 9, 4, 0, 1])

print(torch.arange(start=0, end=10, step=2))
# tensor([0, 2, 4, 6, 8])

print(torch.linspace(start=1, end=10, steps=5))
# tensor([ 1.0000,  3.2500,  5.5000,  7.7500, 10.0000])

print(torch.logspace(start=1, end=10, steps=5))
# tensor([1.0000e+01, 1.7783e+03, 3.1623e+05, 5.6234e+07, 1.0000e+10])
```

<h4>张量与Numpy数据相互转换</h4>

```python
import numpy as np
import torch

"""
Numpy 浮点数的默认类型为 64位
Pytorch张量 浮点数的默认类型为 32位

torch.from_numpy() 或 torch.as_tensor()
    将 Numpy 数组转换成 Pytorch张量

.numpy() 将 Pytorch张量 转换成 Numpy 数组

"""
ndarray1 = np.array([1, 2, 3])
tensor1 = torch.from_numpy(ndarray1)
print(tensor1, tensor1.dtype)  # tensor([1, 2, 3], dtype=torch.int32) torch.int32

ndarray2 = np.array([1., 2., 3.])
tensor2 = torch.from_numpy(ndarray2)
print(tensor2, tensor2.dtype)  # tensor([1., 2., 3.], dtype=torch.float64) torch.float64

ndarray3 = np.array([1., 3., 5.])
tensor3 = torch.as_tensor(ndarray3)
print(tensor3, tensor3.dtype)  # tensor([1., 3., 5.], dtype=torch.float64) torch.float64

ndarray4 = tensor3.numpy()
print(ndarray4, type(ndarray4))  # [1. 3. 5.] <class 'numpy.ndarray'>

```

<h4>随机数</h4>

```python
import torch

"""
随机数正常张量

torch.normal( , , )
    参数一 每个随机数所服从的分布的平均值
    参数二 每个随机数所服从的分布的标准差
    参数三 返回的张量的形状大小 
    
torch.normal(mean=,std=)
    函数中，通过mean参数指定随机数的均值，std参数指定随机数的标准差，如果mean参数和std参数有多个值，则生成多个随机数


"""

torch.manual_seed(123)  # 选定随机数种子

A = torch.normal(0.0, 1.0, (3,))
print("A：", A)  # A： tensor([0.1204])

B = torch.normal(mean=0.0, std=torch.tensor([1.0]))
print("B：", B)  # B： tensor([-0.2404])

C = torch.normal(mean=0.0, std=torch.tensor([1.0, 2.0]))
print("C：", C)  # C： tensor([-1.1969,  0.4185])

D = torch.rand(1,3)
print("D：", D)  # D： tensor([[0.0756, 0.1966, 0.3164]])

E = torch.rand_like(D)
print("E：", E)  # E： tensor([[0.4017, 0.1186, 0.8274]])
```

<h4>维度 维度大小</h4>

```python
import torch

"""
.reshape() 针对一个生成的张量，改变其形状

torch.reshape(input= ,shape= ) 改变张量的形状

torch.unsqueeze( , dim= ) 维度提升，在指定的维度进行插入

torch.squeeze( ) 移除所有维度大小为1的维度
torch.squeeze( , dim= ) 若指定维度的维度大小为1，则移除该维度，否则不移除

.expand( ) 对张量的维度进行扩展，从而对形状大小进行修改
    对于 n 维张量  .expand( x,第n维的大小,...,第2维的大小,-1) 相当与由x个该张量里的元素组成了n+1维的张量
    
.repeat() 对每个维度大小进行倍数扩展，重复填充

"""

A = torch.arange(12.0).reshape(3, 4)
print(A)
# tensor([[ 0.,  1.,  2.,  3.],
#         [ 4.,  5.,  6.,  7.],
#         [ 8.,  9., 10., 11.]])

B = torch.reshape(input=A, shape=(1, 3, 4))
print(B)
# tensor([[[ 0.,  1.,  2.,  3.],
#          [ 4.,  5.,  6.,  7.],
#          [ 8.,  9., 10., 11.]]])

C = torch.unsqueeze(B, dim=2)
print("C.shape：", C.shape)  # C.shape：torch.Size([1, 3, 1, 4])

D = torch.squeeze(C)
print("D.shape：", D.shape)  # D.shape：torch.Size([3, 4])

E = torch.squeeze(C, dim=2)
print("E.shape：", E.shape)  # E.shape： torch.Size([1, 3, 4])

F = torch.arange(3)
print(F)  # tensor([0, 1, 2])

print(F.expand(1, -1).shape)  # torch.Size([1, 3]) # 升了1维

G = F.expand(3, -1)
print(G, G.shape)
# tensor([[0, 1, 2],
#         [0, 1, 2],
#         [0, 1, 2]])
# torch.Size([3, 3])

H = G.expand(2, 3, -1)
print(H.shape)  # torch.Size([2, 3, 3])

I = H.expand(4, 2, 3, -1)
print(I.shape)  # torch.Size([4, 2, 3, 3])

J = torch.arange(6.0).reshape(2, 3)
print(J.repeat(2, 3).shape)  # torch.Size([4, 9])
```

<h4>张量计算</h4>

<h5>比较大小</h5>

```python
import torch

"""
torch.allclose( , ,rtol= ,atol= ,equal_nan= )
    逐一比较两个张量(两个张量的形状大小须相同)里的元素，全部相近，返回True，否则返回False
    公式：|A - B| <= atol+rtol×|B| 
    对于缺失值的比较，如果equal_nan设置成True，则认为两个缺失值相同
    
torch.eq( , )
    逐一比较两个张量里的元素，相同处为True，返回一个张量 (传入的两个张量的形状大小须相同)
torch.ge() 逐元素比较大于等于 (两个张量的形状大小须相同)
torch.gt() 逐元素比较大于 (两个张量的形状大小须相同)
torch.ne() 逐元素比较不等于 (两个张量的形状大小须相同)
torch.le() 逐元素比较小于等于 (两个张量的形状大小须相同)
torch.lt() 逐元素比较小于 (两个张量的形状大小须相同)
torch.isnan() 逐元素判读是否为 缺失值 (两个张量的形状大小须相同)

torch.equal( , )
    判断两个张量是否具有相同的形状和元素，是返回True
    


"""

tensor1 = torch.tensor([10.0, 12.0])
tensor2 = torch.tensor([9.9, 12.07])
print(torch.allclose(tensor1, tensor2, rtol=1e-05, atol=1e-08))  # False
print(torch.allclose(tensor1, tensor2, rtol=0.1, atol=0.01))  # True

tensor3 = torch.tensor([1, 2, 3.0])
tensor4 = torch.tensor([3, 2.0, 3.0])
tensor5 = torch.tensor([1, 2.0])
print(torch.eq(tensor3, tensor4))  # tensor([False,  True,  True])
print(torch.equal(tensor4, tensor5))  # False

tensor6 = torch.tensor([10.0, float("nan"), 12.0])
print(torch.isnan(tensor6))  # tensor([False,  True, False])
```

<h5>基本运算</h5>

<h5>统计相关的计算</h5>

