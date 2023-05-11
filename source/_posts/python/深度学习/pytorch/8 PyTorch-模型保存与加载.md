---
title: 8 PyTorch-模型保存与加载
top: 8
tags:
  - PyTorch
categories:
  - PyTorch
---

```python
import torchvision.datasets
import torch

vgg16 = torchvision.models.vgg16(pretrained=False)

# 保存方式1 保存结构+保存参数
torch.save(vgg16, "vgg16_method1.pth")

# 保存方式1⟶加载模型
model = torch.load("vgg16_method1.pth")
print(model)

# 保存方式2 保存参数
torch.save(vgg16.state_dict(), "vgg16_method2.pth")

# 保存方式2⟶加载模型
vgg16 = torchvision.models.vgg16(pretrained=False)
vgg16.load_state_dict(torch.load("vgg16_method2.pth"))
print(vgg16)
```