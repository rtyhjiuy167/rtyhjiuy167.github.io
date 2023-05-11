---
title: 14 matplotlib
top: 14
tags:
  - python
categories:
  - python
---

<h4>matplotlib</h4>

文档：https://www.matplotlib.org.cn/intro/

<h5>简单使用</h5>

```python
"""
matplotlib
    python底层绘图库
    能将数据进行可视化，更直观的呈现
"""
from matplotlib import pyplot as plt  # matplotlib里的 pyplot 是专门用来画图的

# 每个点的x轴坐标与y坐标 都是可迭代的对象即可
x = range(2, 26, 2)
y = [15, 13, 14, 5, 17, 20, 25, 26, 26, 24, 22, 18]

# 绘图
plt.plot(x, y)
# 展示图形
plt.show()
```

```python
import matplotlib.pyplot as plt

# 设置图片大小
plt.figure(figsize=(20, 8), dpi=80)
"""
figure图形图标，指我们画的图
figsize图片大小参数：(宽，高) 
dpi 每英寸上像素点的个数
"""

x = range(2, 26, 2)
y = [15, 13, 14, 5, 17, 20, 25, 26, 26, 24, 22, 18]

# 设置x轴的刻度 传入一个可迭代对象即可
plt.xticks(x)
# 设置y轴的刻度 传入一个可迭代对象即可
plt.yticks(range(min(y), max(y) + 1), 2)

# 绘图 折线
plt.plot(x, y)

# 显示图形
plt.show()

# 保存图片 可设置成svg矢量图格式，放大不会有锯齿
plt.savefig('./p1.svg')
```

<h4>绘制10点至12点的气温</h4>

```python
import matplotlib.pyplot as plt
import random
from matplotlib import font_manager  # 可用来设置中文字体

# 设置图片大小
plt.figure(figsize=(20, 8), dpi=80)

x = range(0, 120)
y = [random.randint(20, 35) for i in range(120)]

_xtick_labels = ["10点{}分".format(i) for i in range(60)]
_xtick_labels += ["11点{}分".format(i) for i in range(60)]

# matplotlib默认不支持中文字符 需要进行配置
# 先实例化，后面用来配置
my_font = font_manager.FontProperties(fname="C:\\Windows\\Fonts\\simsun.ttc",size='x-large')

# 第二个参数：将对应的x轴上的刻度换成相应的字符串  rotation 转换的角度  fontproperties 配置字体
plt.xticks(list(x)[::3], _xtick_labels[::3], rotation=45, fontproperties=my_font)

# 添加x轴的描述信息
plt.xlabel("时间", fontproperties=my_font)
# 添加y轴的描述信息
plt.ylabel("温度 单位(`C)", fontproperties=my_font)
# 添加标题
plt.title("10点至12点每分钟的气温变化情况", fontproperties=my_font)

plt.plot(x, y)

plt.show()

```

<h4>双折线</h4>

```python
import matplotlib.pyplot as plt
from matplotlib import font_manager

# 设置图片大小
plt.figure(figsize=(20, 8), dpi=80)

x = range(11, 31)
y1 = [1, 0, 1, 1, 2, 4, 3, 2, 3, 4, 4, 5, 6, 5, 4, 3, 3, 1, 1, 1]
y2 = [2, 2, 3, 2, 1, 3, 2, 1, 2, 5, 5, 1, 5, 2, 6, 4, 6, 3, 4, 2]
_xtick_labels = ["{}岁".format(i) for i in x]

my_font = font_manager.FontProperties(fname="C:\\Windows\\Fonts\\simsun.ttc", size='x-large')

plt.xticks(x, _xtick_labels, fontproperties=my_font)

# 绘制网格
plt.grid(alpha=0.2)

# label用来标记曲线，需要配合 legend()使用
plt.plot(x, y1, label="1", color='cyan', linestyle=':', linewidth=5)
plt.plot(x, y2, label="2", color='orange', linestyle='-.')

# 绘制图例
# 在legend 里 配置字体的参数是 prop
# loc 图例的位置 默认 best
plt.legend(prop=my_font, loc="upper left")

plt.show()
```

<h4>散点图</h4>

```python
import matplotlib.pyplot as plt
from matplotlib import font_manager

plt.figure(figsize=(20, 8), dpi=80)

x1 = range(1, 32)
x2 = range(51, 82)
y1 = [11, 17, 16, 11, 12, 11, 12, 6, 6, 7, 8, 9, 12, 15, 14, 17, 18, 21, 16, 17, 20, 14, 15, 15, 15, 19, 21, 22, 22, 19,
      21]
y2 = [26, 26, 28, 19, 21, 17, 16, 19, 18, 20, 20, 19, 22, 23, 17, 20, 21, 20, 22, 15, 11, 15, 5, 13, 17, 10, 11, 11, 10,
      12, 12]

my_font = font_manager.FontProperties(fname="C:\\Windows\\Fonts\\simsun.ttc", size='medium')

_x = list(x1) + list(x2)
_xtick_labels = ["3月{}日".format(i) for i in x1]
_xtick_labels += ["10月{}日".format(i - 50) for i in x2]

plt.xticks(_x[::3], _xtick_labels[::3], fontproperties=my_font, rotation=45)

# 画散点 参数 s 是点的大小
plt.scatter(x1, y1, label="3月份")
plt.scatter(x2, y2, label="4月份")

plt.xlabel("时间", fontproperties=my_font)
plt.ylabel("温度", fontproperties=my_font)
plt.title("标题", fontproperties=my_font)
plt.legend(prop=my_font)

plt.show()
```

<h4>条形图</h4>

```python
import matplotlib.pyplot as plt
from matplotlib import font_manager

plt.figure(figsize=(20, 8), dpi=80)

a = ["战狼2", "速度与激情8", "功夫瑜伽", "西游伏妖篇", "变形金刚5：最后的骑士", "摔跤吧！爸爸", "加勒比海盗5"]
b = [56.01, 26.94, 17.53, 16.49, 15.45, 12.96, 11.8]

my_font = font_manager.FontProperties(fname="C:\\Windows\\Fonts\\simsun.ttc", size='large')


# 绘制竖状条形图
# plt.bar(range(len(a)), b, width=0.3)
# plt.xticks(range(len(a)), a, fontproperties=my_font)

# 绘制横状条形图
plt.barh(range(len(a)), b, height=0.3)
plt.yticks(range(len(a)), a, fontproperties=my_font)

plt.show()
```

<h4>多个条形图</h4>

```python
import matplotlib.pyplot as plt
from matplotlib import font_manager

plt.figure(figsize=(20, 8), dpi=80)

a = ["猩球崛起3:终极之战", "敦刻尔克", "蜘蛛侠：英雄归来", "战狼2"]
b3 = [15746, 312, 4497, 319]
b2 = [12357, 156, 2045, 168]
b1 = [2358, 339, 2358, 362]

bar_width = 0.2

# x1 x2 x3 之间的距离为 1  从range(len(a)) 可看出
# 而 每个 x 要画3个条形 ， 所以应保证 bar_width<=0.3
x1 = list(range(len(a)))
x2 = [i + bar_width for i in x1]
x3 = [i + bar_width * 2 for i in x1]

my_font = font_manager.FontProperties(fname="C:\\Windows\\Fonts\\simsun.ttc", size='large')

plt.bar(range(len(a)), b1, width=bar_width, label="9月14日")
plt.bar(x2, b2, width=bar_width, label="9月15日")
plt.bar(x3, b3, width=bar_width, label="9月16日")

plt.xticks(x2, a, fontproperties=my_font)

plt.legend(prop=my_font)

plt.show()
```

<h4>直方图</h4>

用户年龄分布状态、一段时间内用户点击次数的分布状态、用户活跃时间的分布状态

```python
import matplotlib.pyplot as plt
from matplotlib import font_manager

plt.figure(figsize=(20, 8), dpi=80)

a = [131, 98, 125, 131, 124, 139, 131, 117, 128, 135, 131, 102, 107, 114, 119, 128, 121, 142, 127, 130, 124, 101, 110,
     116, 117, 110, 128, 128, 125, 115, 88, 136, 126, 134, 95, 138, 117, 111, 78, 132, 124, 113, 150, 110, 117, 86, 95,
     144, 105, 126, 130, 126, 130, 126, 116, 123, 106, 112, 138, 123, 86, 101, 99, 136, 123, 117, 119, 105, 137, 123,
     128, 104, 109, 134, 125, 127, 105, 120, 107, 129, 116, 108, 132, 103, 136, 118, 102, 120, 114, 105, 115, 132, 145,
     119, 121, 112, 139, 125, 138, 109, 132, 134, 156, 106, 117, 127, 144, 139, 139, 119, 140, 83, 110, 102, 123, 107,
     143]

d = 3  # 组距
num_bins = (max(a) - min(a)) // d + 1  # 组数

my_font = font_manager.FontProperties(fname="C:\\Windows\\Fonts\\simsun.ttc", size='large')

# 绘制直方图
plt.hist(a, num_bins)
plt.xticks(range(min(a), max(a) + d, d))

plt.grid()
plt.show()
```
