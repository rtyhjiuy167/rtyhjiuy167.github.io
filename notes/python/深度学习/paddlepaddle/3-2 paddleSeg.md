PaddleSeg是基于飞桨PaddlePaddle开发的端到端图像分割开发套件，涵盖了**高精度**和**轻量级**等不同方向的大量高质量分割模型。

只需要提供`train.txt`、`val.txt`、`test.txt`、`.yml`配置文件以及数据图片即可。

### 安装PaddleSeg

通过pip安装的PaddleSeg，模型相关信息的配置需要通过相关API进行配置

下载PaddleSeg代码，模型相关信息的配置可直接通过.yml进行配置，也可通过相关API进行配置

#### pip安装

```python
# PaddlePaddle里则执行下行代码
! pip install paddleseg
```

#### 下载PaddleSeg代码

##### 从github或Gitee上下载

```python
# 下两行二选一

# 从github下载
! git clone https://github.com/PaddlePaddle/PaddleSeg
# 使用Gitee进行代码下载
! git clone https://gitee.com/paddlepaddle/PaddleSeg.git
```

##### 验证下载

通过加载`PaddleSeg/configs/quick_start/`目录下的`bisenet_optic_disc_512x512_1k.yml`配置文件信息来执行`PaddleSeg/`目录下的`train.py`文件进行训练。当正常打印log记录，则表示安装成功。

```python
# 运行验证
! python train.py --config configs/quick_start/bisenet_optic_disc_512x512_1k.yml
```

<img src="https://ai-studio-static-online.cdn.bcebos.com/5d3a93d047ad48afb21ab6e2535626d943081fd59ee94263b0376d24ee00153e" style="zoom:80%;" />

### 相关文件配置

#### train.txt val.txt test.txt

```yaml
# 相对于main的训练/验证/测试图片相对路径 相对于main的标签图片相对路径
# 注意：两路径之间以一个空格隔开
# 例如：
images/image1.jpg labels/label1.png
```

#### 训练、测试

##### 1.使用API加载相关信息进行训练、测试

```python
#
from paddleseg.models import BiSeNetV2
model = BiSeNetV2(num_classes=2,
                 lambd=0.25,
                 align_corners=False,
                 pretrained=None)
```



##### 2.使用.yml配置文件加载相关信息进行训练、测试(推荐)

```python
batch_size: 16 # 一次参数更新运算所需的样本数量
iters: 1000 # iteration/step：表示每运行一个iteration/step，更新一次参数权重，即进行一次学习，每一次更新参数需要batch size个样本进行运算学习，根据运算结果调整更新一次参数。指定该参数后，设置 epoch无效
    
train_dataset:
  type: Dataset
  dataset_root: /home/aistudio
  train_path: train.txt
  num_classes: 2
  transforms:
    - type: ResizeStepScaling
      min_scale_factor: 0.5
      max_scale_factor: 2.0
      scale_step_size: 0.25
    - type: RandomPaddingCrop
      crop_size: [512, 512]
    - type: RandomHorizontalFlip
    - type: RandomDistort
      brightness_range: 0.4
      contrast_range: 0.4
      saturation_range: 0.4
    - type: Normalize
  mode: train

val_dataset:
  type: Dataset
  dataset_root: /home/aistudio
  val_path: eval.txt
  transforms:
    - type: Normalize
  mode: val
  num_classes: 2

test_dataset:
  type: Dataset
  dataset_root: /home/aistudio
  val_path: test.txt
  transforms:
    - type: Normalize
  mode: test
  num_classes: 2

optimizer:
  type: sgd
  momentum: 0.9
  weight_decay: 4.0e-5

learning_rate:
  value: 0.01
  decay:
    type: poly
    power: 0.9
    end_lr: 0.0

loss:
  types:
    - type: CrossEntropyLoss
  coef: [1]

model:
  type: UNet
  num_classes: 2
  use_deconv: False
  pretrained: Null


```

