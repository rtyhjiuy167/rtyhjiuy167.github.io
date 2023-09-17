---
title: 6-8 内置模块-calendar
top: 6.8
tags:
  - python
categories:
  - python
---

```python
import calendar

# calendar.monthrange(年,月) 返回指定年月的第一天是周几（周1为0），和该月的天数
print(calendar.monthrange(2021, 10))  # (4, 31)
days = calendar.monthrange(2021, 10)[1]
w = calendar.monthrange(2021, 10)[0] + 1
print(f'2021年10月的第一天为周{w}, 该月有{days}天')  # 2021年10月的第一天为周5, 该月有31天
```

<h5>输出日历</h5>

```python
import calendar

def showdate(year, month):
    res = calendar.monthrange(year, month)
    days = res[1]
    w = res[0] + 1  # 周1为0
    # 实现日历信息的输出
    print(f'---{year}年{month}月的日历信息---')
    print('----------------------------')
    print(' 一   二  三  四  五   六  日 ')
    d = 1
    count = 1
    while d <= days:
        for i in range(1, 8):
            if d > days or (d == 1 and i < w):
                print('\t', end='')
            else:
                print(' {:0>2d} '.format(d), end="")  # :0>2 小于两位时用0补齐 >右对齐
                d += 1
        print()
    print('----------------------------')

showdate(2021,10)
```