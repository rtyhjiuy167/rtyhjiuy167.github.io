---
title: 6-7 内置模块-time
top: 6.7
tags:
  - python
categories:
  - python
---

```python
import time

# 获取当前系统的时间戳 1970年1月1日0时0分0分
print(time.time())

# 获取当前系统时间
print(time.ctime())  # Fri Oct  1 15:51:53 2021

# 获取当前系统时间 时间元组
print(
    time.localtime())  # time.struct_time(tm_year=2021, tm_mon=10, tm_mday=1, tm_hour=15, tm_min=51, tm_sec=53, tm_wday=4, tm_yday=274, tm_isdst=0)

# 获取指定时间
t = 1633074713.2332487
print(time.ctime(t))  # Fri Oct  1 15:51:53 2021
print(time.localtime(
    t))  # time.struct_time(tm_year=2021, tm_mon=10, tm_mday=1, tm_hour=15, tm_min=51, tm_sec=53, tm_wday=4, tm_yday=274, tm_isdst=0)

# 格式化时间 年 月 日 时 分 秒
res = time.localtime(t)
print(f'{res.tm_year}年{res.tm_mon}月{res.tm_mday}日{res.tm_hour}:{res.tm_min}:{res.tm_sec}')  # 2021年10月1日15:51:53

# 使用strftime(格式) 格式看文档
print(time.strftime('%Y年%m月%d日 %H:%M:%S 周%w'))  # 2021年10月01日 16:01:38 周5

# time.sleep(秒) 暂停当前线程的执行

# 计算程序运行时间 秒
start = time.perf_counter()
# ...
end = time.perf_counter()
print(end - start) # 1.9999999999881224e-07
```