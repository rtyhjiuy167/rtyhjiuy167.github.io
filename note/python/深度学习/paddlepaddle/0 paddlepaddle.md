---
title: 0 paddlepaddle
top: 0
tags:
  - paddlepaddle
categories:
  - paddlepaddle
---

paddlepaddle安装教程：https://www.paddlepaddle.org.cn/install/quick?docurl=/documentation/docs/zh/install/pip/windows-pip.html#old-version-anchor-2-1.1%E7%9B%AE%E5%89%8D%E9%A3%9E%E6%A1%A8%E6%94%AF%E6%8C%81%E7%9A%84%E7%8E%AF%E5%A2%83

<h4>win10 gpu</h4>

1.检查python、pip版本及本机的操作系统和位数信息，需达到指定要求

<img src="https://img-blog.csdnimg.cn/bec30f69889f4e17b03eca7cfb783e95.png" style="zoom:80%;" />

2.查看是否支持gpu，是否有N卡

<img src="https://img-blog.csdnimg.cn/005d0b65a5d44a168ed5b1eacd8c4127.png" style="zoom:50%;" />

3.下载指定的cuda与cudnn

cuda下载地址：https://link.csdn.net/?target=https%3A%2F%2Fdeveloper.nvidia.com%2Fcuda-toolkit-archive

cudnn下载地址：https://developer.nvidia.com/rdp/cudnn-archive

安装cuda，安装完成后在cmd执行`nvcc --version`显示cuda版本号或者执行`nvidia-smi`查看更详细信息

将cudnn压缩包解压，并将文件夹里的文件分别复制到cuda安装的文件目录里的对应目录下

4.执行安装paddlepaddle-gpu

例如：在pycharm的终端里执行`python -m pip install paddlepaddle-gpu==2.2.0.post112 -f https://www.paddlepaddle.org.cn/whl/windows/mkl/avx/stable.html`

安装前一定要卸载cpu版的，`python -m pip uninstall paddlepaddle`

```python
import paddle
paddle.utils.run_check()
# 如有打印出PaddlePaddle is installed successfully!说明安装成功
print(paddle.__version__)  # 打印paddle的版本号
```

5.使用gpu运行

```python
import paddle
print(paddle.device.get_device())  # gpu:0
paddle.device.set_device('gpu:0')  # 直接将上一句代码的打印结果放入即可
```

注意：使用gpu运行时，如果出现gpu out of merrory，可能是batch_size参数设置过大，显存不够用，可将batch_size参数设置小一点

卸载gpu版：`python -m pip uninstall paddlepaddle-gpu`
