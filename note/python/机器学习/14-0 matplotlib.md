#### 坐标轴

```python
"""
设置x坐标轴：plt.xticks(ticks=None, labels=None)
 ticks: 范围
 labels: 刻度
"""
```

#### 子图

通过plt.subplot(row,col,idx)指定当前图表的子图个数，以及当前正在绘制的子图

通过plt.gca()获取当前子图

```python
import matplotlib.pyplot as plt
currentAxis=plt.gca()
```

#### 绘制矩形

```python
import matplotlib.pyplot as plt
import matplotlib.patches as patches
import numpy as np
rect = patches.Rectangle((0.0, 2.0),  # 矩形左下角坐标
                         width=2,  # 矩形的宽度
                         height=3,  # 矩形的高度
                         alpha=1,  # 矩阵的透明度
                         facecolor='green'  # 是否填充矩阵(设置为绿色)
                         )
x = np.arange(5)
y = np.arange(5)
plt.xticks(x)
plt.yticks(y)
currentAxis = plt.gca()
currentAxis.add_patch(rect)
plt.show()
```

#### 箭头绘制

```python
import matplotlib.pyplot as plt
"""
plt.arrow(x, y, dx, dy, ...)
x, y：箭头尾部的坐标。类型为浮点数。必备参数。
dx, dy：箭头在xy方向的长度。类型为浮点数。必备参数。
width：箭头尾部的宽度。类型为浮点数，默认值为0.001。
head_width：完全箭头头部的宽度。类型为浮点数或None，默认值为3*width。
head_length：完全箭头头部的长度。类型为浮点数或None，默认值为1.5*head_width。
length_includes_head：长度是否包含箭头。类型为布尔值，默认值为False。
shape：箭头的形状，分为全箭头、左半箭头和右半箭头。取值范围为{'full', 'left', 'right'}，默认值为'full'。
overhang：箭头头部尾角的倾斜系数（小数）。类型为浮点数，值可为负值或大于1，默认值为0，即箭头头部为三角形。
head_starts_at_zero：箭头头部的起始位置。类型为布尔值，默认值为False。当值为True时，箭头头部从0坐标开始绘制，当值为False时，箭头尾部从0坐标开始绘制
color: 箭头颜色
"""
plt.arrow(x=300, y=288, dx=30, dy=40, color='red', width=0.001, length_includes_head=True, head_width=5, head_length=10,
          shape='full')
plt.show()

```

