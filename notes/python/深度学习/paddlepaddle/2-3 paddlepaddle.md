---
title: 2-3 paddlepaddle
top: 2.3
tags:
  - paddlepaddle
categories:
  - paddlepaddle
---

<h3>人脸关键点检测</h3>

<h4>数据集</h4>

实验所采用的数据集来源为github的[开源项目](https://github.com/udacity/P1_Facial_Keypoints)

`training` 和 `test` 文件夹分别存放训练集和测试集

`training_frames_keypoints.csv` 和 `test_frames_keypoints.csv` 存放着训练集和测试集的标签（图片名及人脸关键点的坐标）

```python
import os
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib.image as mpimg

import warnings

# paddle.set_device('gpu') # 设置为GPU

warnings.filterwarnings('ignore')  # 忽略 warning

key_pts_frame = pd.read_csv('data/training_frames_keypoints.csv')  # 读取数据集
print('Number of images: ', key_pts_frame.shape[0])  # 输出数据集大小

print(key_pts_frame.head(5))  # 看前五条数据
#                    Unnamed: 0     0     1     2  ...   132    133   134    135
# 0           Luis_Fonsi_21.jpg  45.0  98.0  47.0  ...  81.0  122.0  77.0  122.0
# 1       Lincoln_Chafee_52.jpg  41.0  83.0  43.0  ...  83.0  122.0  79.0  122.0
# 2       Valerie_Harper_30.jpg  56.0  69.0  56.0  ...  75.0  105.0  73.0  105.0
# 3         Angelo_Reyes_22.jpg  61.0  80.0  58.0  ...  91.0  139.0  85.0  136.0
# 4  Kristen_Breitweiser_11.jpg  58.0  94.0  58.0  ...  88.0  122.0  84.0  122.0
#
# [5 rows x 137 columns]
# 136 包含x,y,所以是64个点

def show_keypoints(image, key_pts):
    """
    Args:
        image: 图像信息
        key_pts: 关键点信息，
    展示图片和关键点信息
    """
    plt.imshow(image)  # 展示图片信息
    for i in range(len(key_pts) // 2, ):
        plt.scatter(key_pts[i * 2], key_pts[i * 2 + 1], s=20, marker='.', c='b')  # 展示关键点信息


# 先展示单条数据
n = 14  # n为数据在表格中的索引 选第14条数据
image_name = key_pts_frame.iloc[n, 0]  # 获取图像名称
key_pts = key_pts_frame.iloc[n, 1:].to_numpy()  # 将图像label格式转为numpy.array的格式
key_pts = key_pts.astype('float').reshape(-1)  # 获取图像关键点信息
print(key_pts.shape)
plt.figure(figsize=(5, 5))  # 展示的图像大小
show_keypoints(mpimg.imread(os.path.join('data/training/', image_name)), key_pts)  # 展示图像与关键点信息
plt.show()  # 展示图像

```

<h4>构建训练集</h4>

由于所给的数据只是原图片，需进行预处理，所以需要自定义数据集

```python
import os
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib.image as mpimg
from paddle.io import Dataset
import warnings
from PIL import Image
# paddle.set_device('gpu') # 设置为GPU

warnings.filterwarnings('ignore')  # 忽略 warning

key_pts_frame = pd.read_csv('data/training_frames_keypoints.csv')  # 读取数据集

def show_keypoints(image, key_pts):
    """
    Args:
        image: 图像信息
        key_pts: 关键点信息，
    展示图片和关键点信息
    """
    plt.imshow(image)  # 展示图片信息
    for i in range(len(key_pts) // 2, ):
        plt.scatter(key_pts[i * 2], key_pts[i * 2 + 1], s=20, marker='.', c='b')  # 展示关键点信息

# 按照Dataset的使用规范，构建人脸关键点数据集
class FacialKeypointsDataset(Dataset):
    # 人脸关键点数据集
    """
    步骤一：继承paddle.io.Dataset类
    """

    def __init__(self, csv_file, root_dir, transform=None):
        """
        步骤二：实现构造函数，定义数据集大小
        Args:
            csv_file (string): 带标注的csv文件路径
            root_dir (string): 图片存储的文件夹路径
            transform (callable, optional): 应用于图像上的数据处理方法
        """
        self.key_pts_frame = pd.read_csv(csv_file)  # 读取csv文件
        self.root_dir = root_dir  # 获取图片文件夹路径
        self.transform = transform  # 获取 transform 方法

    def __getitem__(self, idx):
        """
        步骤三：实现__getitem__方法，定义指定index时如何获取数据，并返回单条数据（训练数据，对应的标签）
        """

        image_name = os.path.join(self.root_dir,
                                  self.key_pts_frame.iloc[idx, 0])
        # 获取图像
        image = Image.open(image_name)
        image = np.array(image.convert("RGB"), dtype='float32')
        # 图像格式处理，如果包含 alpha 通道，那么忽略它
        if (image.shape[2] == 4):
            image = image[:, :, 0:3]

        # 获取关键点信息
        key_pts = self.key_pts_frame.iloc[idx, 1:].to_numpy()
        key_pts = key_pts.astype('float').reshape(-1)  # [136, 1]

        # 如果定义了 transform 方法，使用 transform方法
        if self.transform:
            image, key_pts = self.transform([image, key_pts])

        # 转为 numpy 的数据格式
        image = np.array(image, dtype='float32')
        key_pts = np.array(key_pts, dtype='float32')

        return image, key_pts

    def __len__(self):
        """
        步骤四：实现__len__方法，返回数据集总数目
        """
        return len(self.key_pts_frame)  # 返回数据集大小，即图片的数量


# 构建一个数据集类
face_dataset = FacialKeypointsDataset(csv_file='data/training_frames_keypoints.csv',
                                      root_dir='data/training/')

# 输出数据集大小
print('数据集大小为: ', len(face_dataset))
# 根据 face_dataset 可视化数据集
num_to_display = 3

for i in range(num_to_display):
    # 定义图片大小
    plt.figure(figsize=(10, 10))
    # 随机选择图片
    rand_i = np.random.randint(0, len(face_dataset))
    sample = face_dataset[rand_i]
    # 输出图片大小和关键点的数量
    print(i, sample[0].shape, sample[1].shape)
    # 设置图片打印信息
    plt.title("simple{}".format(i))
    # 输出图片
    show_keypoints(sample[0], sample[1])
    plt.show()
```

<h4>数据预处理</h4>

对图像进行预处理，包括灰度化、归一化、重新设置尺寸、随机裁剪，修改通道格式等等，以满足数据要求；每一类的功能如下：

- 灰度化：丢弃颜色信息，保留图像边缘信息；识别算法对于颜色的依赖性不强，加上颜色后鲁棒性会下降，而且灰度化图像维度下降（3->1），保留梯度的同时会加快计算。
- 归一化：加快收敛
- 重新设置尺寸：数据增强
- 随机裁剪：数据增强
- 修改通道格式：改为模型需要的结构

```python
import os
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib.image as mpimg
import paddle.vision.transforms.functional as F
import paddle.vision.transforms as T
import paddle
from paddle.io import Dataset
from paddle.vision.transforms import Compose
import warnings
from PIL import Image
# paddle.set_device('gpu') # 设置为GPU

warnings.filterwarnings('ignore')  # 忽略 warning

key_pts_frame = pd.read_csv('data/training_frames_keypoints.csv')  # 读取数据集
print('Number of images: ', key_pts_frame.shape[0])  # 输出数据集大小


# 计算标签的均值和标准差，用于标签的归一化
key_pts_values = key_pts_frame.values[:, 1:]  # 取出标签信息
data_mean = key_pts_values.mean()  # 计算均值
data_std = key_pts_values.std()  # 计算标准差



def show_keypoints(image, key_pts):
    """
    Args:
        image: 图像信息
        key_pts: 关键点信息，
    展示图片和关键点信息
    """
    plt.imshow(image)  # 展示图片信息
    for i in range(len(key_pts) // 2, ):
        plt.scatter(key_pts[i * 2], key_pts[i * 2 + 1], s=20, marker='.', c='b')  # 展示关键点信息

# 按照Dataset的使用规范，构建人脸关键点数据集
class FacialKeypointsDataset(Dataset):
    # 人脸关键点数据集
    """
    步骤一：继承paddle.io.Dataset类
    """

    def __init__(self, csv_file, root_dir, transform=None):
        """
        步骤二：实现构造函数，定义数据集大小
        Args:
            csv_file (string): 带标注的csv文件路径
            root_dir (string): 图片存储的文件夹路径
            transform (callable, optional): 应用于图像上的数据处理方法
        """
        self.key_pts_frame = pd.read_csv(csv_file)  # 读取csv文件
        self.root_dir = root_dir  # 获取图片文件夹路径
        self.transform = transform  # 获取 transform 方法

    def __getitem__(self, idx):
        """
        步骤三：实现__getitem__方法，定义指定index时如何获取数据，并返回单条数据（训练数据，对应的标签）
        """

        image_name = os.path.join(self.root_dir,
                                  self.key_pts_frame.iloc[idx, 0])
       # 获取图像
        image = Image.open(image_name)
        image = np.array(image.convert("RGB"), dtype='float32')
        # 图像格式处理，如果包含 alpha 通道，那么忽略它
        if (image.shape[2] == 4):
            image = image[:, :, 0:3]

        # 获取关键点信息
        key_pts = self.key_pts_frame.iloc[idx, 1:].to_numpy()
        key_pts = key_pts.astype('float').reshape(-1)  # [136, 1]

        # 如果定义了 transform 方法，使用 transform方法
        if self.transform:
            image, key_pts = self.transform([image, key_pts])

        # 转为 numpy 的数据格式
        image = np.array(image, dtype='float32')
        key_pts = np.array(key_pts, dtype='float32')

        return image, key_pts

    def __len__(self):
        """
        步骤四：实现__len__方法，返回数据集总数目
        """
        return len(self.key_pts_frame)  # 返回数据集大小，即图片的数量


# 构建一个数据集类
face_dataset = FacialKeypointsDataset(csv_file='data/training_frames_keypoints.csv',
                                      root_dir='data/training/')

# 输出数据集大小
print('数据集大小为: ', len(face_dataset))

class GrayNormalize(object):
    # 将图片变为灰度图，并将其值放缩到[0, 1]
    # 将 label 放缩到 [-1, 1] 之间

    def __call__(self, data):
        image = data[0]  # 获取图片
        key_pts = data[1]  # 获取标签

        image_copy = np.copy(image)
        key_pts_copy = np.copy(key_pts)

        # 灰度化图片
        gray_scale = paddle.vision.transforms.Grayscale(num_output_channels=3)
        image_copy = gray_scale(image_copy)

        # 将图片值放缩到 [0, 1]
        image_copy = image_copy / 255.0

        # 将坐标点放缩到 [-1, 1]
        mean = data_mean  # 获取标签均值
        std = data_std  # 获取标签标准差
        key_pts_copy = (key_pts_copy - mean) / std

        return image_copy, key_pts_copy


class Resize(object):
    # 将输入图像调整为指定大小

    def __init__(self, output_size):
        assert isinstance(output_size, (int, tuple))
        self.output_size = output_size

    def __call__(self, data):

        image = data[0]  # 获取图片
        key_pts = data[1]  # 获取标签

        image_copy = np.copy(image)
        key_pts_copy = np.copy(key_pts)

        h, w = image_copy.shape[:2]
        if isinstance(self.output_size, int):
            if h > w:
                new_h, new_w = self.output_size * h / w, self.output_size
            else:
                new_h, new_w = self.output_size, self.output_size * w / h
        else:
            new_h, new_w = self.output_size

        new_h, new_w = int(new_h), int(new_w)

        img = F.resize(image_copy, (new_h, new_w))

        # scale the pts, too
        key_pts_copy[::2] = key_pts_copy[::2] * new_w / w
        key_pts_copy[1::2] = key_pts_copy[1::2] * new_h / h

        return img, key_pts_copy


class RandomCrop(object):
    # 随机位置裁剪输入的图像

    def __init__(self, output_size):
        assert isinstance(output_size, (int, tuple))
        if isinstance(output_size, int):
            self.output_size = (output_size, output_size)
        else:
            assert len(output_size) == 2
            self.output_size = output_size

    def __call__(self, data):
        image = data[0]
        key_pts = data[1]

        image_copy = np.copy(image)
        key_pts_copy = np.copy(key_pts)

        h, w = image_copy.shape[:2]
        new_h, new_w = self.output_size

        top = np.random.randint(0, h - new_h)
        left = np.random.randint(0, w - new_w)

        image_copy = image_copy[top: top + new_h,
                     left: left + new_w]

        key_pts_copy[::2] = key_pts_copy[::2] - left
        key_pts_copy[1::2] = key_pts_copy[1::2] - top

        return image_copy, key_pts_copy


class ToCHW(object):
    # 将图像的格式由HWC改为CHW
    def __call__(self, data):
        image = data[0]
        key_pts = data[1]

        transpose = T.Transpose((2, 0, 1))  # 改为CHW
        image = transpose(image)

        return image, key_pts


# 测试 Resize
resize = Resize(256)

# 测试 RandomCrop
random_crop = RandomCrop(128)

norm = GrayNormalize()

# 测试 Resize + RandomCrop，图像大小变到250*250， 然后截取出224*224的图像块
composed = paddle.vision.transforms.Compose([Resize(250), RandomCrop(224)])

test_num = 800
data = face_dataset[test_num]

transforms = {'None': None,
              'norm': norm,
              'random_crop': random_crop,
              'resize': resize,
              'composed': composed}
for i, func_name in enumerate(['None', 'norm', 'random_crop', 'resize', 'composed']):

    # 定义图片大小
    fig = plt.figure(figsize=(10, 10))

    # 处理图片
    if transforms[func_name] != None:
        transformed_sample = transforms[func_name](data)
    else:
        transformed_sample = data

    # 设置图片打印信息
    ax = plt.subplot(1, 5, i + 1)
    ax.set_title(' Transform is #{}'.format(func_name))

    # 输出图片
    show_keypoints(transformed_sample[0], transformed_sample[1])
    plt.show()

data_transform = Compose([Resize(256), RandomCrop(224), GrayNormalize(), ToCHW()])

# create the transformed dataset
train_dataset = FacialKeypointsDataset(csv_file='data/training_frames_keypoints.csv',
                                       root_dir='data/training/',
                                       transform=data_transform)

print('Number of train dataset images: ', len(train_dataset))

for i in range(4):
    sample = train_dataset[i]
    print(i, sample[0].shape, sample[1].shape)

test_dataset = FacialKeypointsDataset(csv_file='data/test_frames_keypoints.csv',
                                             root_dir='data/test/',
                                             transform=data_transform)

print('Number of test dataset images: ', len(test_dataset))
```

<h4>模型训练</h4>

```python
import os
import numpy as np
import pandas as pd
from PIL import Image
import paddle.vision.transforms.functional as F
import paddle.vision.transforms as T
import paddle
from paddle.io import Dataset
from paddle.vision.transforms import Compose
import warnings
import paddle.nn as nn
from paddle.metric import Metric

paddle.device.set_device('gpu:0')

warnings.filterwarnings('ignore') 

key_pts_frame = pd.read_csv('data/training_frames_keypoints.csv')  # 读取数据集

# 计算标签的均值和标准差，用于标签的归一化
key_pts_values = key_pts_frame.values[:, 1:]  # 取出标签信息
data_mean = key_pts_values.mean()  # 计算均值
data_std = key_pts_values.std()  # 计算标准差

# 按照Dataset的使用规范，构建人脸关键点数据集
class FacialKeypointsDataset(Dataset):
    # 人脸关键点数据集
    """
    步骤一：继承paddle.io.Dataset类
    """

    def __init__(self, csv_file, root_dir, transform=None):
        """
        步骤二：实现构造函数，定义数据集大小
        Args:
            csv_file (string): 带标注的csv文件路径
            root_dir (string): 图片存储的文件夹路径
            transform (callable, optional): 应用于图像上的数据处理方法
        """
        self.key_pts_frame = pd.read_csv(csv_file)  # 读取csv文件
        self.root_dir = root_dir  # 获取图片文件夹路径
        self.transform = transform  # 获取 transform 方法

    def __getitem__(self, idx):
        """
        步骤三：实现__getitem__方法，定义指定index时如何获取数据，并返回单条数据（训练数据，对应的标签）
        """

        image_name = os.path.join(self.root_dir,
                                  self.key_pts_frame.iloc[idx, 0])
        # 获取图像
        image = Image.open(image_name)
        image = np.array(image.convert("RGB"), dtype='float32')
        # 图像格式处理，如果包含 alpha 通道，那么忽略它
        if (image.shape[2] == 4):
            image = image[:, :, 0:3]

        # 获取关键点信息
        key_pts = self.key_pts_frame.iloc[idx, 1:].to_numpy()
        key_pts = key_pts.astype('float').reshape(-1)  # [136, 1]

        # 如果定义了 transform 方法，使用 transform方法
        if self.transform:
            image, key_pts = self.transform([image, key_pts])

        # 转为 numpy 的数据格式
        image = np.array(image, dtype='float32')
        key_pts = np.array(key_pts, dtype='float32')

        return image, key_pts

    def __len__(self):
        """
        步骤四：实现__len__方法，返回数据集总数目
        """
        return len(self.key_pts_frame)  # 返回数据集大小，即图片的数量

# 构建一个数据集类
face_dataset = FacialKeypointsDataset(csv_file='data/training_frames_keypoints.csv',
                                      root_dir='data/training/')


class GrayNormalize(object):
    # 将图片变为灰度图，并将其值放缩到[0, 1]
    # 将 label 放缩到 [-1, 1] 之间

    def __call__(self, data):
        image = data[0]  # 获取图片
        key_pts = data[1]  # 获取标签

        image_copy = np.copy(image)
        key_pts_copy = np.copy(key_pts)

        # 灰度化图片
        gray_scale = paddle.vision.transforms.Grayscale(num_output_channels=3)
        image_copy = gray_scale(image_copy)

        # 将图片值放缩到 [0, 1]
        image_copy = image_copy / 255.0

        # 将坐标点放缩到 [-1, 1]
        mean = data_mean  # 获取标签均值
        std = data_std  # 获取标签标准差
        key_pts_copy = (key_pts_copy - mean) / std

        return image_copy, key_pts_copy


class Resize(object):
    # 将输入图像调整为指定大小

    def __init__(self, output_size):
        assert isinstance(output_size, (int, tuple))
        self.output_size = output_size

    def __call__(self, data):

        image = data[0]  # 获取图片
        key_pts = data[1]  # 获取标签

        image_copy = np.copy(image)
        key_pts_copy = np.copy(key_pts)

        h, w = image_copy.shape[:2]
        if isinstance(self.output_size, int):
            if h > w:
                new_h, new_w = self.output_size * h / w, self.output_size
            else:
                new_h, new_w = self.output_size, self.output_size * w / h
        else:
            new_h, new_w = self.output_size

        new_h, new_w = int(new_h), int(new_w)

        img = F.resize(image_copy, (new_h, new_w))

        # scale the pts, too
        key_pts_copy[::2] = key_pts_copy[::2] * new_w / w
        key_pts_copy[1::2] = key_pts_copy[1::2] * new_h / h

        return img, key_pts_copy


class RandomCrop(object):
    # 随机位置裁剪输入的图像

    def __init__(self, output_size):
        assert isinstance(output_size, (int, tuple))
        if isinstance(output_size, int):
            self.output_size = (output_size, output_size)
        else:
            assert len(output_size) == 2
            self.output_size = output_size

    def __call__(self, data):
        image = data[0]
        key_pts = data[1]

        image_copy = np.copy(image)
        key_pts_copy = np.copy(key_pts)

        h, w = image_copy.shape[:2]
        new_h, new_w = self.output_size

        top = np.random.randint(0, h - new_h)
        left = np.random.randint(0, w - new_w)

        image_copy = image_copy[top: top + new_h,
                     left: left + new_w]

        key_pts_copy[::2] = key_pts_copy[::2] - left
        key_pts_copy[1::2] = key_pts_copy[1::2] - top

        return image_copy, key_pts_copy


class ToCHW(object):
    # 将图像的格式由HWC改为CHW
    def __call__(self, data):
        image = data[0]
        key_pts = data[1]

        transpose = T.Transpose((2, 0, 1))  # 改为CHW
        image = transpose(image)

        return image, key_pts

data_transform = Compose([Resize(256), RandomCrop(224), GrayNormalize(), ToCHW()])

train_dataset = FacialKeypointsDataset(csv_file='data/training_frames_keypoints.csv',
                                       root_dir='data/training/',
                                       transform=data_transform)

test_dataset = FacialKeypointsDataset(csv_file='data/test_frames_keypoints.csv',
                                      root_dir='data/test/',
                                      transform=data_transform)


class SimpleNet(nn.Layer):

    def __init__(self, key_pts):
        super(SimpleNet, self).__init__()

        # 使用resnet50作为backbone
        self.backbone = paddle.vision.models.resnet50(pretrained=True)

        # 添加第一个线性变换层 resnet50输出为1000，所以这里的输入为1000
        self.linear1 = nn.Linear(in_features=1000, out_features=512)

        # 使用 ReLU 激活函数
        self.act1 = nn.ReLU()

        # 添加第二个线性变换层作为输出，输出元素的个数为 key_pts*2，代表每个关键点的坐标
        self.linear2 = nn.Linear(in_features=512, out_features=key_pts * 2)

    def forward(self, x):
        x = self.backbone(x)
        x = self.linear1(x)
        x = self.act1(x)
        x = self.linear2(x)
        return x


model = paddle.Model(SimpleNet(key_pts=68))


class NME(Metric):
    """
    1. 继承paddle.metric.Metric
    """

    def __init__(self, name='nme', *args, **kwargs):
        """
        2. 构造函数实现，自定义参数即可
        """
        super(NME, self).__init__(*args, **kwargs)
        self._name = name
        self.rmse = 0
        self.sample_num = 0

    def name(self):
        """
        3. 实现name方法，返回定义的评估指标名字
        """
        return self._name

    def update(self, preds, labels):
        """
        4. 实现update方法，用于单个batch训练时进行评估指标计算。
        - 当`compute`类函数未实现时，会将模型的计算输出和标签数据的展平作为`update`的参数传入。
        """
        N = preds.shape[0]

        preds = preds.reshape((N, -1, 2))
        labels = labels.reshape((N, -1, 2))

        self.rmse = 0

        for i in range(N):
            pts_pred, pts_gt = preds[i,], labels[i,]
            interocular = np.linalg.norm(pts_gt[36,] - pts_gt[45,])

            self.rmse += np.sum(np.linalg.norm(pts_pred - pts_gt, axis=1)) / (interocular * preds.shape[1])
            self.sample_num += 1

        return self.rmse / N

    def accumulate(self):
        """
        5. 实现accumulate方法，返回历史batch训练积累后计算得到的评价指标值。
        每次`update`调用时进行数据积累，`accumulate`计算时对积累的所有数据进行计算并返回。
        结算结果会在`fit`接口的训练日志中呈现。
        """
        return self.rmse / self.sample_num

    def reset(self):
        """
        6. 实现reset方法，每个Epoch结束后进行评估指标的重置，这样下个Epoch可以重新进行计算。
        """
        self.rmse = 0
        self.sample_num = 0


# 使用 paddle.Model 封装模型
model = paddle.Model(SimpleNet(key_pts=68))

# 定义Adam优化器
optimizer = paddle.optimizer.Adam(learning_rate=0.001,
                                  weight_decay=5e-4,
                                  parameters=model.parameters())
# 定义SmoothL1Loss
loss = nn.SmoothL1Loss()

# 使用自定义metrics
metric = NME()

model.prepare(optimizer=optimizer, loss=loss, metrics=metric)

model.fit(train_dataset, epochs=50, batch_size=20, verbose=1)
checkpoints_path = './checkpoints/models'
model.save(checkpoints_path)

```

<h4>结果预测</h4>

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import paddle.vision.transforms.functional as F
import paddle.vision.transforms as T
import paddle
from paddle.vision.transforms import Compose
import warnings
import paddle.nn as nn
from PIL import Image

paddle.device.set_device('gpu:0')

warnings.filterwarnings('ignore')

key_pts_frame = pd.read_csv('data/training_frames_keypoints.csv')  # 读取数据集

# 计算标签的均值和标准差，用于标签的归一化
key_pts_values = key_pts_frame.values[:, 1:]  # 取出标签信息
data_mean = key_pts_values.mean()  # 计算均值
data_std = key_pts_values.std()  # 计算标准差

class GrayNormalize(object):
    # 将图片变为灰度图，并将其值放缩到[0, 1]
    # 将 label 放缩到 [-1, 1] 之间

    def __call__(self, data):
        image = data[0]  # 获取图片
        key_pts = data[1]  # 获取标签

        image_copy = np.copy(image)
        key_pts_copy = np.copy(key_pts)

        # 灰度化图片
        gray_scale = paddle.vision.transforms.Grayscale(num_output_channels=3)
        image_copy = gray_scale(image_copy)

        # 将图片值放缩到 [0, 1]
        image_copy = image_copy / 255.0

        # 将坐标点放缩到 [-1, 1]
        mean = data_mean  # 获取标签均值
        std = data_std  # 获取标签标准差
        key_pts_copy = (key_pts_copy - mean) / std

        return image_copy, key_pts_copy


class Resize(object):
    # 将输入图像调整为指定大小

    def __init__(self, output_size):
        assert isinstance(output_size, (int, tuple))
        self.output_size = output_size

    def __call__(self, data):

        image = data[0]  # 获取图片
        key_pts = data[1]  # 获取标签

        image_copy = np.copy(image)
        key_pts_copy = np.copy(key_pts)

        h, w = image_copy.shape[:2]
        if isinstance(self.output_size, int):
            if h > w:
                new_h, new_w = self.output_size * h / w, self.output_size
            else:
                new_h, new_w = self.output_size, self.output_size * w / h
        else:
            new_h, new_w = self.output_size

        new_h, new_w = int(new_h), int(new_w)

        img = F.resize(image_copy, (new_h, new_w))

        key_pts_copy[::2] = key_pts_copy[::2] * new_w / w
        key_pts_copy[1::2] = key_pts_copy[1::2] * new_h / h

        return img, key_pts_copy


class RandomCrop(object):
    # 随机位置裁剪输入的图像

    def __init__(self, output_size):
        assert isinstance(output_size, (int, tuple))
        if isinstance(output_size, int):
            self.output_size = (output_size, output_size)
        else:
            assert len(output_size) == 2
            self.output_size = output_size

    def __call__(self, data):
        image = data[0]
        key_pts = data[1]

        image_copy = np.copy(image)
        key_pts_copy = np.copy(key_pts)

        h, w = image_copy.shape[:2]
        new_h, new_w = self.output_size

        top = np.random.randint(0, h - new_h)
        left = np.random.randint(0, w - new_w)

        image_copy = image_copy[top: top + new_h,
                     left: left + new_w]

        key_pts_copy[::2] = key_pts_copy[::2] - left
        key_pts_copy[1::2] = key_pts_copy[1::2] - top

        return image_copy, key_pts_copy


class ToCHW(object):
    # 将图像的格式由HWC改为CHW
    def __call__(self, data):
        image = data[0]
        key_pts = data[1]

        transpose = T.Transpose((2, 0, 1))  # 改为CHW
        image = transpose(image)

        return image, key_pts

class SimpleNet(nn.Layer):

    def __init__(self, key_pts):
        super(SimpleNet, self).__init__()

        # 使用resnet50作为backbone
        self.backbone = paddle.vision.models.resnet50(pretrained=True)

        # 添加第一个线性变换层 resnet50输出为1000，所以这里的输入为1000
        self.linear1 = nn.Linear(in_features=1000, out_features=512)

        # 使用 ReLU 激活函数
        self.act1 = nn.ReLU()

        # 添加第二个线性变换层作为输出，输出元素的个数为 key_pts*2，代表每个关键点的坐标
        self.linear2 = nn.Linear(in_features=512, out_features=key_pts * 2)

    def forward(self, x):
        x = self.backbone(x)
        x = self.linear1(x)
        x = self.act1(x)
        x = self.linear2(x)
        return x

checkpoints_path = './checkpoints/models'
# 加载保存好的模型进行预测
model = paddle.Model(SimpleNet(key_pts=68))
model.load(checkpoints_path)
model.prepare()


def show_all_keypoints(image, predicted_key_pts):
    """
    展示图像，预测关键点
    Args：
        image：裁剪后的图像 [224, 224, 3]
        predicted_key_pts: 预测关键点的坐标
    """
    # 展示图像
    plt.imshow(image.astype('uint8'))

    # 展示关键点
    for i in range(0, len(predicted_key_pts), 2):
        plt.scatter(predicted_key_pts[i], predicted_key_pts[i + 1], s=20, marker='.', c='m')


def visualize_output(test_images, test_outputs, batch_size=1, h=20, w=10):
    """
    展示图像，预测关键点
    Args：
        test_images：裁剪后的图像 [224, 224, 3]
        test_outputs: 模型的输出
        batch_size: 批大小
        h: 展示的图像高
        w: 展示的图像宽
    """

    if len(test_images.shape) == 3:
        test_images = np.array([test_images])

    for i in range(batch_size):
        plt.figure(figsize=(h, w))
        ax = plt.subplot(1, batch_size, i + 1)

        # 随机裁剪后的图像
        image = test_images[i]

        # 模型的输出，未还原的预测关键点坐标值
        predicted_key_pts = test_outputs[i]

        # 还原后的真实的关键点坐标值
        predicted_key_pts = predicted_key_pts * data_std + data_mean

        # 展示图像和关键点
        show_all_keypoints(np.squeeze(image), predicted_key_pts)

        plt.axis('off')

    plt.show()


img = Image.open("./6.jpg")
img = np.array(img.convert("RGB"), dtype='float32')

# 关键点占位符
kpt = np.ones((136, 1))

transform = Compose([Resize(256), RandomCrop(224)])

# 对图像先重新定义大小，并裁剪到 224*224 的大小
rgb_img, kpt = transform([img, kpt])

norm = GrayNormalize()
to_chw = ToCHW()

# 对图像进行归一化和格式变换
img, kpt = norm([rgb_img, kpt])
img, kpt = to_chw([img, kpt])

img = np.array([img], dtype='float32')

# 加载保存好的模型进行预测
model = paddle.Model(SimpleNet(key_pts=68))
model.load(checkpoints_path)
model.prepare()

# 预测结果
out = model.predict_batch([img])
out = out[0].reshape((out[0].shape[0], 136, -1))


def show_all_keypoints(image, predicted_key_pts):
    """
    展示图像，预测关键点
    Args：
        image：裁剪后的图像 [224, 224, 3]
        predicted_key_pts: 预测关键点的坐标
    """
    # 展示图像
    plt.imshow(image.astype(np.uint8))

    # 展示关键点
    for i in range(0, len(predicted_key_pts), 2):
        plt.scatter(predicted_key_pts[i], predicted_key_pts[i + 1], s=20, marker='.', c='m')


def visualize_output(test_images, test_outputs, batch_size=1, h=20, w=10):
    """
    展示图像，预测关键点
    Args：
        test_images：裁剪后的图像 [224, 224, 3]
        test_outputs: 模型的输出
        batch_size: 批大小
        h: 展示的图像高
        w: 展示的图像宽
    """

    if len(test_images.shape) == 3:
        test_images = np.array([test_images])

    for i in range(batch_size):
        plt.figure(figsize=(h, w))
        ax = plt.subplot(1, batch_size, i + 1)

        # 随机裁剪后的图像
        image = test_images[i]

        # 模型的输出，未还原的预测关键点坐标值
        predicted_key_pts = test_outputs[i]

        # 还原后的真实的关键点坐标值
        predicted_key_pts = predicted_key_pts * data_std + data_mean

        # 展示图像和关键点
        show_all_keypoints(np.squeeze(image), predicted_key_pts)

        plt.axis('off')

    plt.show()


visualize_output(rgb_img, out, batch_size=1)

```
