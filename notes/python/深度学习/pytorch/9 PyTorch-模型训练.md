---
title: 9 PyTorch-模型训练
top: 9
tags:
  - PyTorch
categories:
  - PyTorch
---

<h4>训练的步骤</h4>

```python
import torch
import torchvision.datasets
from torch import nn
from torch.nn import Conv2d, MaxPool2d, Flatten, Linear, Sequential
from torch.utils.data import DataLoader

# 1.1准备数据集
train_data = torchvision.datasets.CIFAR10("./CIFAR10", train=True, transform=torchvision.transforms.ToTensor(),
                                          download=True)
test_data = torchvision.datasets.CIFAR10("./CIFAR10", train=False, transform=torchvision.transforms.ToTensor(),
                                         download=True)
# 1.2 用 DataLoader 来加载数据集
train_dataloader = DataLoader(train_data, batch_size=64)
test_dataloader = DataLoader(test_data, batch_size=64)


# 2.1 搭建神经网络
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


# 2.2 网络模型实例化
network = Network()

# 2.3 训练需要的损失函数
loss_fn = nn.CrossEntropyLoss()

# 2.4 训练需要的优化器
optimizer = torch.optim.SGD(network.parameters(), lr=1e-2)

# 记录
total_train_step = 0
total_test_step = 0
train_data_size = len(train_data)
test_data_size = len(test_data)

# 网络模型训练次数
epoch = 20

# 4 模型训练
for i in range(epoch):
    print("-----第{}轮训练开始-----".format(i + 1))
    # 4.1 让网络进入训练状态，只对某些层有必要
    network.train()
    for data in train_dataloader:
        # 4.2 分离特征与标签
        imgs, targets = data
        # 4.3 放入特征进行训练得到结果
        outputs = network(imgs)
        # 4.4 用损失函数比较训练结果与实际的误差
        loss = loss_fn(outputs, targets)
        # 4.5 计算的梯度结果清零
        optimizer.zero_grad()
        # 4.6 反向传播
        loss.backward()
        # 4.7 参数优化
        optimizer.step()

        total_train_step += 1

        if total_train_step % 100 == 0:
            print("训练次数：{},Loss:{}".format(total_train_step, loss.item()))

    total_test_loss = 0
    total_accuracy = 0

    # 4.8 每训练完一次进行测试模型效果
    network.eval()
    with torch.no_grad():  # 测试时不对梯度进行干涉
        for data in test_dataloader:
            imgs, targets = data
            outputs = network(imgs)
            loss = loss_fn(outputs, targets)
            total_test_loss += loss.item()
            accuracy = (outputs.argmax(1) == targets).sum()
            total_accuracy += accuracy

    print("整体测试集上的Loss:{}".format(total_test_loss))
    print("整体测试集上的正确率:{}".format(total_accuracy / test_data_size))

# 模型保存
# 如果想保存某次训练好的模型，加个判断即可

```

<h4>gpu训练</h4>

方式一：

```python
import torch
import torchvision.datasets
from torch import nn
from torch.nn import Conv2d, MaxPool2d, Flatten, Linear, Sequential
from torch.utils.data import DataLoader

if torch.cuda.is_available():
    print("可以用gpu进行训练")
# 1.1准备数据集
train_data = torchvision.datasets.CIFAR10("./CIFAR10", train=True, transform=torchvision.transforms.ToTensor(),
                                          download=True)
test_data = torchvision.datasets.CIFAR10("./CIFAR10", train=False, transform=torchvision.transforms.ToTensor(),
                                         download=True)
# 1.2 用 DataLoader 来加载数据集
train_dataloader = DataLoader(train_data, batch_size=64)
test_dataloader = DataLoader(test_data, batch_size=64)


# 2.1 搭建神经网络
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


# 2.2 网络模型实例化
network = Network()
if torch.cuda.is_available():
    network = network.cuda()
# 2.3 训练需要的损失函数
loss_fn = nn.CrossEntropyLoss()
if torch.cuda.is_available():
    loss_fn = loss_fn.cuda()
# 2.4 训练需要的优化器
optimizer = torch.optim.SGD(network.parameters(), lr=1e-2)

# 记录
total_train_step = 0
total_test_step = 0
train_data_size = len(train_data)
test_data_size = len(test_data)

# 网络模型训练次数
epoch = 20

# 4 模型训练
for i in range(epoch):
    print("-----第{}轮训练开始-----".format(i + 1))
    # 4.1 让网络进入训练状态，只对某些层有必要
    network.train()
    for data in train_dataloader:
        # 4.2 分离特征与标签
        imgs, targets = data
        if torch.cuda.is_available():
            imgs = imgs.cuda()
            targets = targets.cuda()
        # 4.3 放入特征进行训练得到结果
        outputs = network(imgs)
        # 4.4 用损失函数比较训练结果与实际的误差
        loss = loss_fn(outputs, targets)
        # 4.5 计算的梯度结果清零
        optimizer.zero_grad()
        # 4.6 反向传播
        loss.backward()
        # 4.7 参数优化
        optimizer.step()

        total_train_step += 1

        if total_train_step % 100 == 0:
            print("训练次数：{},Loss:{}".format(total_train_step, loss.item()))

    total_test_loss = 0
    total_accuracy = 0

    # 4.8 每训练完一次进行测试模型效果
    network.eval()
    with torch.no_grad():  # 测试时不对梯度进行干涉
        for data in test_dataloader:
            imgs, targets = data
            if torch.cuda.is_available():
                imgs = imgs.cuda()
                targets = targets.cuda()
            outputs = network(imgs)
            loss = loss_fn(outputs, targets)
            total_test_loss += loss.item()
            accuracy = (outputs.argmax(1) == targets).sum()
            total_accuracy += accuracy

    print("整体测试集上的Loss:{}".format(total_test_loss))
    print("整体测试集上的正确率:{}".format(total_accuracy / test_data_size))

# 模型保存
# 如果想保存某次训练好的模型，加个判断即可

```

方式二：

```python
import torch
import torchvision.datasets
from torch import nn
from torch.nn import Conv2d, MaxPool2d, Flatten, Linear, Sequential
from torch.utils.data import DataLoader

if torch.cuda.is_available():
    device = torch.device("cuda:0")
    print("使用gpu进行训练")
else:
    device = torch.device("cpu")
    print("使用cpu进行训练")

# 1.1准备数据集
train_data = torchvision.datasets.CIFAR10("./CIFAR10", train=True, transform=torchvision.transforms.ToTensor(),
                                          download=True)
test_data = torchvision.datasets.CIFAR10("./CIFAR10", train=False, transform=torchvision.transforms.ToTensor(),
                                         download=True)
# 1.2 用 DataLoader 来加载数据集
train_dataloader = DataLoader(train_data, batch_size=64)
test_dataloader = DataLoader(test_data, batch_size=64)


# 2.1 搭建神经网络
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


# 2.2 网络模型实例化
network = Network().to(device)

# 2.3 训练需要的损失函数
loss_fn = nn.CrossEntropyLoss().to(device)

# 2.4 训练需要的优化器
optimizer = torch.optim.SGD(network.parameters(), lr=1e-2)

# 记录
total_train_step = 0
total_test_step = 0
train_data_size = len(train_data)
test_data_size = len(test_data)

# 网络模型训练次数
epoch = 20

# 4 模型训练
for i in range(epoch):
    print("-----第{}轮训练开始-----".format(i + 1))
    # 4.1 让网络进入训练状态，只对某些层有必要
    network.train()
    for data in train_dataloader:
        # 4.2 分离特征与标签
        imgs, targets = data
        imgs = imgs.to(device)
        targets = targets.to(device)

        # 4.3 放入特征进行训练得到结果
        outputs = network(imgs)
        # 4.4 用损失函数比较训练结果与实际的误差
        loss = loss_fn(outputs, targets)
        # 4.5 计算的梯度结果清零
        optimizer.zero_grad()
        # 4.6 反向传播
        loss.backward()
        # 4.7 参数优化
        optimizer.step()

        total_train_step += 1

        if total_train_step % 100 == 0:
            print("训练次数：{},Loss:{}".format(total_train_step, loss.item()))

    total_test_loss = 0
    total_accuracy = 0

    # 4.8 每训练完一次进行测试模型效果
    network.eval()
    with torch.no_grad():  # 测试时不对梯度进行干涉
        for data in test_dataloader:
            imgs, targets = data
            imgs = imgs.to(device)
            targets = targets.to(device)
            outputs = network(imgs)
            loss = loss_fn(outputs, targets)
            total_test_loss += loss.item()
            accuracy = (outputs.argmax(1) == targets).sum()
            total_accuracy += accuracy

    print("整体测试集上的Loss:{}".format(total_test_loss))
    print("整体测试集上的正确率:{}".format(total_accuracy / test_data_size))

# 模型保存
# 如果想保存某次训练好的模型，加个判断即可
```