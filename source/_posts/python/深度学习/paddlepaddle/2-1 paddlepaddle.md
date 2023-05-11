---
title: 2-1 paddlepaddle
top: 2.1
tags:
  - paddlepaddle
categories:
  - paddlepaddle
---

<h4>Tensor</h4>

文档：https://www.paddlepaddle.org.cn/documentation/docs/zh/guides/01_paddle2.0_introduction/basic_concept/tensor_introduction_cn.html

```python
import paddle

"""
Tensor
    paddle使用Tensor来表示数据，在神经网络中传递的数据均为Tensor
    Tensor是类似于 Numpy array 的概念
    可通过dtype来指定Tensor数据类型，否则会创建float32类型的Tensor
"""
tensor1 = paddle.to_tensor([2.0, 3.0, 4.0], dtype='float64')
print(tensor1)  # Tensor(shape=[3], dtype=float64, place=CPUPlace, stop_gradient=True,[2., 3., 4.])

# 显示Tensor的shape
print(tensor1.shape, type(tensor1.shape))  # [3] <class 'list'>

# paddle.reshape(tensor, shape) 返回一个改变shape的新Tensor
tensor2 = paddle.reshape(tensor1, [3, 1])
print(tensor1.shape)  # [3, 1]

# paddle.cast(tensor,dtype) 返回一个改变dtype的新Tensor
tensor3 = paddle.cast(tensor1, dtype='int64')
print(tensor3)  # Tensor(shape=[3], dtype=int64, place=CPUPlace, stop_gradient=True,[2, 3, 4])

# tensor1.numpy() 将数据以numpy.ndarray的方式返回
print(tensor1.numpy(), type(tensor1.numpy()))  # [2. 3. 4.] <class 'numpy.ndarray'>

# Paddle 使用标准的 Python 索引规则与 Numpy 索引规则,可对Tensor进行索引和切片操作
print(tensor1[1:].numpy())  # [3. 4.]
```

<h4>基本使用</h4>

```python
# 加载飞桨和相关类库
import paddle
import numpy as np
import matplotlib.pyplot as plt

"""
MNIST 是一个数据集 只不过paddlepaddle把它包装了一下
    mode
         train 选取数据中的已为我们分好的训练集
         test  选取数据中的已为我们分好的测试集
返回的实例中
    索引为0的 即特征对象 类型 <class 'PIL.Image.Image'>  需要 np.array() 转换为特征矩阵 二维数组
    索引为1的 即标签 类型 numpy.ndarray
"""

train_dataset = paddle.vision.datasets.MNIST(mode='train')

print(type(train_dataset[0]))  # <class 'tuple'>
print(train_dataset[0][0])  # <PIL.Image.Image image mode=L size=28x28 at 0x1B85EA0E0A0>
print(train_dataset[0][1])  # [5]

print(type(train_dataset[0][0]))  # <class 'PIL.Image.Image'>
print(type(train_dataset[0][1]))  # <class 'numpy.ndarray'>

train_data0 = np.array(train_dataset[0][0])  # 得到特征矩阵
train_label_0 = np.array(train_dataset[0][1])  # 得到标签

plt.figure(figsize=(4, 2))
plt.imshow(train_data0, cmap=plt.cm.binary) # 对画布进行填充
plt.title('image')
plt.show()

print("图像数据形状和对应数据为:", train_data0.shape)
print("图像标签形状和对应数据为:", train_label_0.shape, train_label_0)
print("\n打印第一个batch的第一个图像，对应标签数字为{}".format(train_label_0))
```