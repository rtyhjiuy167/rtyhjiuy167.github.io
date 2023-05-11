---
title: 5 PyTorch-神经网络
top: 5
tags:
  - PyTorch
categories:
  - PyTorch
---



<img src="https://img-blog.csdnimg.cn/d5741429ad0442df8c67f8078b97f075.png" style="zoom:80%;" />

```python
import torch
from torch import nn
from torch.nn import Conv2d, MaxPool2d, Flatten, Linear


class Network(nn.Module):
    def __init__(self):
        super(Network, self).__init__()
        # 在数据预处理后，输入的图片都是三通道的，大小为32×32
        # 第一层：2维卷积 输入的通道数为2，输出为32，创建32个随机数卷积核分别进行卷积并得到32个输出
        # 有输出的形状大小的计算公式知，其输出的形状大小不变,仍为32×32
        self.conv1 = Conv2d(in_channels=3, out_channels=32, kernel_size=5, padding=2)
        # 第二层：2维最大池化 对输入的所有矩阵进行最大池化后得到输出
        # 有输出的形状大小的计算公式知，其输出的形状大小减半，为16×16
        self.maxpool1 = MaxPool2d(kernel_size=2)
        # 第三层：2维卷积 输入的通道数为32，输出为32，创建32个随机数卷积核分别进行卷积并得到32个输出
        # 有输出的形状大小的计算公式知，其输出的形状大小不变,仍为16×16
        self.conv2 = Conv2d(in_channels=32, out_channels=32, kernel_size=5, padding=2)
        # 第四层：2维最大池化 对输入的所有矩阵进行最大池化后得到输出
        # 有输出的形状大小的计算公式知，其输出的形状大小减半，为8×8
        self.maxpool2 = MaxPool2d(kernel_size=2)
        # 第五层：2维卷积 输入的通道数为32，输出为64，创建64个随机数卷积核分别进行卷积并得到64个输出
        # 有输出的形状大小的计算公式知，其输出的形状大小不变,仍为8×8
        self.conv3 = Conv2d(in_channels=32, out_channels=64, kernel_size=5, padding=2)
        # 第六层：2维最大池化 对输入的所有矩阵进行最大池化后得到输出
        # 有输出的形状大小的计算公式知，其输出的形状大小减半，为4×4
        self.maxpool3 = MaxPool2d(kernel_size=2)
        # 第七层：扁平化，一维化 对输入的所有矩阵进行降维，降成一维
        # 上一层的输出为 64×4×4,所以该层的输出为 1024
        self.flatten = Flatten()
        # 第八层：线性层，输入的特征数为 1024，转化为 输出的特征为 64
        self.linear1 = Linear(in_features=1024, out_features=64)
        # 第九层即最后一层：线性层，输入的特征数为 64，转化为 输出的特征为 10
        # 输出的特征数10，即为十分类的各个类别的概率值，它们的和不一定为1
        self.linear2 = Linear(in_features=64, out_features=10)

    def forward(self, x):
        x = self.conv1(x)
        x = self.maxpool1(x)
        x = self.conv2(x)
        x = self.maxpool2(x)
        x = self.conv3(x)
        x = self.maxpool3(x)
        x = self.flatten(x)
        x = self.linear1(x)
        x = self.linear2(x)
        return x


network = Network()
print(network)

# 对网络结构进行检验，查看每步的输入与输出是否能对应上
input = torch.ones((64, 3, 32, 32))
output = network(input)  # 没有报错即可
```

<h4>Sequential</h4>

使用Sequential对各层数进行包裹，可以在前向传播中简写

```python
import torch
from torch import nn
from torch.nn import Conv2d, MaxPool2d, Flatten, Linear, Sequential

class Network(nn.Module):
    def __init__(self):
        super(Network, self).__init__()
        self.model1 = Sequential(
            Conv2d(in_channels=3, out_channels=32, kernel_size=5, padding=2),
            MaxPool2d(kernel_size=2),
            Conv2d(in_channels=32, out_channels=32, kernel_size=5, padding=2),
            MaxPool2d(kernel_size=2),
            Conv2d(in_channels=32, out_channels=64, kernel_size=5, padding=2),
            MaxPool2d(kernel_size=2),
            Flatten(),
            Linear(in_features=1024, out_features=64),
            Linear(in_features=64, out_features=10),
        )

    def forward(self, x):
        x = self.model1(x)
        return x


network = Network()
print(network)

# 对网络结构进行检验，查看每步的输入与输出是否能对应上
input = torch.ones((64, 3, 32, 32))
output = network(input)  # 没有报错即可

```

