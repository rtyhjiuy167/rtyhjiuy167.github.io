---
title: 4 PyTorch-损失函数
top: 4
tags:
  - PyTorch
categories:
  - PyTorch
---

|               类                |        算法名称        | 使用问题类型 |
| :-----------------------------: | :--------------------: | :----------: |
|        torch.nn.L1Loss()        |   平方绝对值误差损失   |     回归     |
|       torch.nn.MSELoss()        |      均方误差损失      |     回归     |
|   torch.nn.CrossEntropyLoss()   |       交叉熵损失       |    多分类    |
|       torch.nn.NLLLoss()        |   负对数似然函数损失   |    多分类    |
|      torch.nn.NLLLoss2d()       | 图片负对数似然函数损失 |   图像分割   |
|      torch.nn.KLDivLoss()       |       KL散度损失       |     回归     |
|       torch.nn.BCELoss()        |    二分类交叉熵损失    |    二分类    |
|  torch.nn.MarginRankingLoss()   |    评价相似度的损失    |              |
| torch.nn.MultiLabelMarginLoss() |    多标签分类的损失    |  多标签分类  |
|     torch.nn.SmoothL1Loss()     |      平滑的L1损失      |     回归     |
|    torch.nn.SoftMarginLoss()    |  多标签二分类问题损失  |  多标签分类  |

```python
import torch
import matplotlib.pyplot as plt
import torch.nn as nn

"""
损失函数
torch.nn.L1Loss(reduction="mean")
    reduction 用来判断损失的方式
        mean 表示计算每个batch的均值
        sum 计算的损失为每个batch的和
        none 表示不使用该参数
    公式 |Xi-Yi| 结果再根据reduction进行处理
    
torch.nn.MSELoss(reduction="mean")
    公式 (Xi-Yi)^2
    结果再根据reduction进行处理
    
torch.nn.CrossEntropyLoss 用于多分类

"""

inputs = torch.tensor([1, 2, 3], dtype=torch.float32)
targets = torch.tensor([1, 2, 5], dtype=torch.float32)

loss = nn.L1Loss(reduction="mean")

result = loss(inputs, targets)
print(result)  # tensor(0.6667) # [(1-1)+(2-2)+(5-3)]/3=0.6667

loss_mse = nn.MSELoss(reduction="mean")
result = loss_mse(inputs, targets)
print(result)  # tensor(1.3333) # [(1-1)^2+(2-2)^2+(5-3)^2]/3=1.3333

x = torch.tensor([0.1, 0.2, 0.3])
x = torch.reshape(x, (1, 3))  # nn.CrossEntropyLossy要求输入为2维
y = torch.tensor([1])  # 正确的索引为1 对应的概率为0.2
loss_cross = nn.CrossEntropyLoss()
result = loss_cross(x, y)
print(result)  # tensor(1.1019) #  -0.2+ln(exp(0.1)+exp(0.2)+exp(0.3))
```

