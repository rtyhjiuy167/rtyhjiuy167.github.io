---
title: 1 预处理
top: 1
tags:
  - paddlepaddle
categories:
  - paddlepaddle
---

除了ToTensor与Normalize会改变图像的格式，其他的转换不会

##### Resize

```python
"""
paddle.vision.transforms. Resize ( size, ... )

img (PIL.Image|np.ndarray|Paddle.Tensor) - 输入的图像数据，数据格式为'HWC'
"""
import paddle
import numpy as np
import matplotlib.pyplot as plt
from PIL import Image
import paddle.vision.transforms as T
img_path='1.jpg'
image = Image.open(img_path)
image = image.convert("RGB")
transform = T.Resize(size=(224,224))
image =transform(image)
plt.imshow(image)
plt.show()
```

##### ToTensor

```python
"""
data_format (str, optional): 返回张量的格式，必须为 'HWC' 或 'CHW'。 默认值: 'CHW'

img (PIL.Image|numpy.ndarray) - 输入的图像数据，数据格式为'HWC'
会将输入数据从（0-255）的范围缩放到 （0-1）的范围
"""
import paddle
import numpy as np
import matplotlib.pyplot as plt
from PIL import Image
import paddle.vision.transforms as T
img_path='1.jpg'
image = Image.open(img_path)
image = image.convert("RGB")
transform = T.ToTensor()
image =transform(image)
transpose = T.Transpose(order=(1,2,0)) 
image =transpose(image)
print(image[:,:,2][0])  # 打印第3通道的像素值
plt.imshow(image)
plt.show()
```

##### Normalize

```python
"""
Normalize ( mean=0.0, std=1.0, data_format='CHW', to_rgb=False, ...)

img (PIL.Image|np.ndarray|Paddle.Tensor) - 输入的图像数据，数据格式为'HWC'
"""
```