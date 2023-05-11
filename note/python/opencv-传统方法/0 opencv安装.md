---
title: 0 opencv安装
top: 0
tags:
  - opencv
categories:
  - opencv
---

```
# 临时指定下载路径，以防下载安装超时
pip install opencv-python -i https://pypi.tuna.tsinghua.edu.cn/simple
```

工具类，创建`utlis.py文件`，内容如下：

```python
def draw_pictures(figsize, images, figshape=None, titles=None, no_axis=True, my_font=None, cmaps=None):

    if my_font is None:
        my_font = font_manager.FontProperties(fname="C:\\Windows\\Fonts\\simsun.ttc", size='large')
    if figshape is None:
        col = int(len(images) ** 0.5) + 1
        row = int(len(images) ** 0.5) + 1
    else:
        row = figshape[0]
        col = figshape[1]
    plt.figure(figsize=figsize)
    for idx in range(len(images)):
        images[idx] = np.array(images[idx], dtype='uint8')
        plt.subplot(row, col, idx + 1)
        if no_axis:
            plt.axis('off')
        if cmaps[idx] == "BGR":
            plt.imshow(images[idx][:, :, [2, 1, 0]])
        else:
            plt.imshow(images[idx], cmap=cmaps[idx])
        if titles is None:
            pass
        else:
            plt.title(titles[idx], fontproperties=my_font)
    plt.show()
```