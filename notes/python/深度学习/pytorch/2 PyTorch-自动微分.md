---
title: 2 PyTorch-自动微分
top: 2
tags:
  - PyTorch
categories:
  - PyTorch
---

<h4>自动微分</h4>

```python
import torch

"""
自动微分

    x = torch.tensor([[1.0, 2.0], [3.0, 4.0]], requires_grad=True)
    y = torch.sum(x ** 2 + 2 * x + 1)
    
    可以理解为 y = (xi ** 2 + 2 * xi + 1 ) , i = 1, 2, 3, 4
    y.backward() 就是对 xi 求导 其结果存储在 x.grad 中
"""

x = torch.tensor([[1.0, 2.0], [3.0, 4.0]], requires_grad=True)
y = torch.sum(x ** 2 + 2 * x + 1)

print(y)  # tensor(54., grad_fn=<SumBackward0>) 
# y是计算的结果，所以它有grad_fn属性

y.backward()
print(x.grad)
# tensor([[ 4.,  6.],
#         [ 8., 10.]])  # 2Xi+2
```

`.requires_grad`设置为`True`会追踪对于该张量的所有操作，当完成计算后可以通过调用`.backward()`，来自动计算所有的梯度

要阻止一个张量被跟踪历史，可以调用`.detach()`方法将其与计算历史分离，并阻止它未来的计算记录被跟踪或`with torch.no_grad():`

