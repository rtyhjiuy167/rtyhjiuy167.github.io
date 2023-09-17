---
title: 14-2 matplotlib图片展示
top: 14.2
tags:
  - python
categories:
  - python
---

```python
import matplotlib.pyplot as plt
import matplotlib.image as mpimg
import numpy as np
img_path='1.jpg'
image = mpimg.imread(img_path) # 读取的像素值为0~255
plt.imshow(image) # 显示图片
plt.show()
```

```python
import matplotlib.pyplot as plt
from PIL import Image
img_path='1.jpg'
image = Image.open(img_path) # RGB或RGBA 读取的像素值为0~255
if image.mode == "RGB":
    image=image.convert("RGB")
plt.imshow(image) # 显示图片
plt.show()
```

