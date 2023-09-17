---
title: 3 PyTorch-神经网络层
top: 3
tags:
  - PyTorch
categories:
  - PyTorch
---

<h4>Convolution Layers	卷积层</h4>

<h5>torch.nn.functional.conv2d</h5>

对图像进行特征提取常用2维卷积

```python
import torch
import torch.nn.functional as F

"""
对几个输入平面组成的输入信号应用2D卷积
torch.nn.functional.conv2d(input, weight, bias=None, stride=1, padding=0, dilation=1, groups=1)
    input   输入张量 形状大小要求：(minibatch,in_channels,iH,iW) 
    weight  过滤器张量 形状大小要求：(out_channels, in_channels/groups, kH, kW)
    bias    可选偏置张量 (out_channels)
    stride  卷积核的步长，可以是单个数字或一个元组 (sh x sw)。默认为1
    padding 输入上隐含零填充。可以是单个数字或元组。 默认值：0 
    groups  将输入分成组，in_channels应该被组数除尽

"""
input = torch.tensor([[1, 2, 0, 3, 1],
                      [0, 1, 2, 3, 1],
                      [1, 2, 1, 0, 0],
                      [5, 2, 3, 1, 1],
                      [2, 1, 0, 1, 1]])
kernel = torch.tensor([[1, 2, 1],
                       [0, 1, 0],
                       [2, 1, 0]])
input = torch.reshape(input, (1, 1, 5, 5))
kernel = torch.reshape(kernel, (1, 1, 3, 3))

output = F.conv2d(input, weight=kernel, stride=1)
print(output)
# tensor([[[[10, 12, 12],
#           [18, 16, 16],
#           [13,  9,  3]]]])
```

$$
\begin{align}
&2维卷积后的形状大小：\\
&H_{out}=[\frac{H_{in}+2×padding[0]-dilation[0]×(kernel\_size[0]-1)-1}{stride[0]}+1]\\
&W_{out}=[\frac{W_{in}+2×padding[1]-dilation[1]×(kernel\_size[1]-1)-1}{stride[1]}+1]
\end{align}
$$

<h5>torch.nn.Conv2d</h5>

```python
torch.nn.Conv2d(in_channels, out_channels, kernel_size, stride=1, padding=0, dilation=1, groups=1, bias=True, padding_mode='zeros', device=None, dtype=None)
```

```python
import numpy as np
import torch
from torch import nn
import matplotlib.pyplot as plt
from PIL import Image

myim = Image.open("./1.jpg")
myimgray = np.array(myim.convert("L"), dtype=np.float32)

imh, imw = myimgray.shape
myimgray_t = torch.from_numpy(myimgray.reshape((1, 1, imh, imw)))

# 自定义一个卷积核 用来进行边缘轮廓提取
kersize = 5
ker = torch.ones(kersize, kersize, dtype=torch.float32)
ker[2, 2] = 24
ker = ker.reshape((1, 1, kersize, kersize))

# 卷积的输出为2，其中一个卷积核为自定义，另一个为随机数
conv2d = nn.Conv2d(1, 2, (kersize, kersize), bias=False)
conv2d.weight.data[0] = ker

imcov2dout = conv2d(myimgray_t)
imcov2dout_im = imcov2dout.data.squeeze()
plt.figure(figsize=(18, 6))
plt.subplot(1, 3, 1)
plt.imshow(myimgray, cmap=plt.cm.gray)
plt.axis("off")
plt.subplot(1, 3, 2)
plt.imshow(imcov2dout_im[0], cmap=plt.cm.gray)
plt.axis("off")
plt.subplot(1, 3, 3)
plt.imshow(imcov2dout_im[1], cmap=plt.cm.gray)
plt.axis("off")
plt.show()
```

<h4>Pooling layers	池化层</h4>

<h5>torch.nn.MaxPool2d</h5>

对图像特征提取后，为了保存特征而又要压缩数据时，可使用

2维池化后的形状大小与2维卷积后的形状大小的计算公式相同

```python
import torch
import torch.nn.functional as F

"""
torch.nn.MaxPool2d(kernel_size, stride=None, padding=0, dilation=1, return_indices=False, ceil_mode=False)
    ceil_mode 设置为False时，池化核在移动时，如果不嫩全部覆盖输入的矩阵的时，忽略该步
"""
input = torch.tensor([[1, 2, 0, 3, 1],
                      [0, 1, 2, 3, 1],
                      [1, 2, 1, 0, 0],
                      [5, 2, 3, 1, 1],
                      [2, 1, 0, 1, 1]], dtype=torch.float32)

input = torch.reshape(input, (1, 1, 5, 5))

output1 = F.max_pool2d(input, kernel_size=3, ceil_mode=True)
print(output1)
# tensor([[[[2., 3.],
#           [5., 1.]]]])

output2 = F.max_pool2d(input, kernel_size=3, ceil_mode=False)
print(output2)
# tensor([[[[2.]]]])
```

<h4>Padding Layers	填充层</h4>

<h4>激活函数</h4>

```python
import numpy as np
import torch
from torch import nn
import matplotlib.pyplot as plt
from PIL import Image

x = torch.linspace(-10, 10, 200)
sigmoid = nn.Sigmoid()  # Sigmoid激活函数
ysigmoid = sigmoid(x)

tanh = nn.Tanh()  # Tanh激活函数
ytanh = tanh(x)

relu = nn.ReLU()  # ReLU激活函数
yrelu = relu(x)

softplus = nn.Softplus()  # Softplus激活函数
ysoftplus = softplus(x)

plt.figure(figsize=(15, 3))

plt.subplot(1, 4, 1)
plt.plot(x.data.numpy(), ysigmoid.data.numpy(), "r-")
plt.title("Sigmoid")

plt.subplot(1, 4, 2)
plt.plot(x.data.numpy(), ytanh.data.numpy(), "r-")
plt.title("Tanh")

plt.subplot(1, 4, 3)
plt.plot(x.data.numpy(), yrelu.data.numpy(), "r-")
plt.title("ReLU")

plt.subplot(1, 4, 4)
plt.plot(x.data.numpy(), ysoftplus.data.numpy(), "r-")
plt.title("Softplus")

plt.show()
```

<img src="https://img-blog.csdnimg.cn/f1d43109f015472cbab7270190a1d528.png" style="zoom:80%;" />

<h4>Linear Layers 线性层（全连接层）</h4>



<h4>Dropout Layers	降层</h4>

随机把输入的元素按概率变为0，目的是为了防止过拟合

