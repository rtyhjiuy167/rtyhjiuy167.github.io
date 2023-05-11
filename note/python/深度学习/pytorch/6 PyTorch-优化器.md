---
title: 6 PyTorch-优化器
top: 6
tags:
  - PyTorch
categories:
  - PyTorch
---

<h4>pytorch内置优化器算法</h4>

|           类           |      算法名称      |
| :--------------------: | :----------------: |
| torch.optim.Adadelta() |    Adadelta算法    |
| torch.optim.Adagrad()  |    Adagrad算法     |
|   torch.optim.Adam()   |      Adam算法      |
|  torch.optim.Adamax()  |     Adamax算法     |
|   torch.optim.ASGD()   | 平均随机梯度下降法 |
|  torch.optim.LBFGS()   |     L-BFGS算法     |
| torch.optim.RMSprop()  |    RMSprop算法     |
|  torch.optim.Rprop()   |  弹性反向传播算法  |
|   torch.optim.SGD()    |   随机梯度下降法   |

```python
import torch
import torchvision.datasets
from torch import nn
from torch.nn import Conv2d, MaxPool2d, Flatten, Linear, Sequential
from torch.utils.data import DataLoader

dataset = torchvision.datasets.CIFAR10("./CIFAR10", train=False, transform=torchvision.transforms.ToTensor(),
                                       download=True)
dataloader = DataLoader(dataset, batch_size=1)


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
loss = nn.CrossEntropyLoss()  # 损失函数
optim = torch.optim.SGD(network.parameters(), lr=0.01)  # 优化器 为优化器配置要优化的参数


for data in dataloader:
    imgs, targets = data
    outputs = network(imgs)
    result_loss = loss(outputs, targets)  # 损失计算
    optim.zero_grad()  # pytorch里计算的梯度是累加的，需要清零
    result_loss.backward()  # 反向传播，计算梯度
    optim.step()  # 执行优化

```