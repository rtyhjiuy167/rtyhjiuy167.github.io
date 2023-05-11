---
title: 2-2 paddlepaddle
top: 2.2
tags:
  - paddlepaddle
categories:
  - paddlepaddle
---

<h4>线性回归 手写数值识别</h4>

```python
import paddle
import numpy as np
import matplotlib.pyplot as plt
import paddle.vision.transforms as T
from paddle.static import InputSpec

# 数据的加载和预处理
transform = T.Normalize(mean=[127.5], std=[127.5])
# 训练数据集
train_dataset = paddle.vision.datasets.MNIST(mode='train', transform=transform)
# 评估数据集
eval_dataset = paddle.vision.datasets.MNIST(mode='test', transform=transform)
print('训练集样本量：{}，验证集样本量：{}'.format(len(train_dataset), len(eval_dataset)))
# 训练集样本量：60000，验证集样本量：10000

network = paddle.nn.Sequential(
    paddle.nn.Flatten(),  # 拉平 将 (28,28) => (784)
    paddle.nn.Linear(784, 512),  # 隐层：线性变换
    paddle.nn.ReLU(),  # 激活函数
    paddle.nn.Linear(512, 10)  # 输出层
)

# 模型封装
model = paddle.Model(network)

# 模型可视化
model.summary((1, 28, 28))

# 配置优化器、损失函数、评估指标
model.prepare(paddle.optimizer.Adam(learning_rate=0.001, parameters=network.parameters()),
              paddle.nn.CrossEntropyLoss(),
              paddle.metric.Accuracy()
              )

model.fit(train_dataset,  # 训练数据集
          eval_dataset,  # 评估数据集
          epochs=5,  # 训练的总轮次
          batch_size=64,  # 训练使用的批大小
          verbose=1,  # 日志展示形式
          )
# 模型评估 根据prepare接口配置的loss和metric进行返回
result = model.evaluate(eval_dataset, verbose=1)
print(result)

# 使用model.predict接口对大量数据进行批量预测
result = model.predict(eval_dataset)


# 定义画图方法
def show_img(img, predict):
    plt.figure()
    plt.title('predict:{}'.format(predict))
    plt.imshow(img.reshape([28, 28]), cmap=plt.cm.binary)
    plt.show()


# 抽样展示
indexs = [2, 15, 38, 211]
for idx in indexs:
    show_img(eval_dataset[idx][0], np.argmax(result[0][idx]))

# 使用 model.predict_batch 来进行单张或少量多张图片的预测
# 读取单张图片
image = eval_dataset[501][0]

# 单张图片预测 返回的各个标签的可能性
result = model.predict_batch([image])

# 可视化结果 通过 np.argmax 返回最大值对应的索引 这里刚好索引与数值对应
show_img(image, np.argmax(result))

# 保存模型 可用与后续调优训练
model.save('finetuning/mnist_model')
```

```python
import paddle
import paddle.vision.transforms as T
from paddle.static import InputSpec

# 数据的加载和预处理
transform = T.Normalize(mean=[127.5], std=[127.5])
# 训练数据集
train_dataset = paddle.vision.datasets.MNIST(mode='train', transform=transform)
# 评估数据集
eval_dataset = paddle.vision.datasets.MNIST(mode='test', transform=transform)

network = paddle.nn.Sequential(
    paddle.nn.Flatten(),  # 拉平 将 (28,28) => (784)
    paddle.nn.Linear(784, 512),  # 隐层：线性变换
    paddle.nn.ReLU(),  # 激活函数
    paddle.nn.Linear(512, 10)  # 输出层
)

# 模型封装 为了后面保存预测模型，这里传入了inputs参数
# 告诉预测模型其输入的形状 -1 表示批的维度为灵活的 传多少个样本过来都可 28 28 是图片大小
model_2 = paddle.Model(network, inputs=[InputSpec(shape=[-1, 28, 28], dtype='float32', name='image')])

# 加载之前保存的阶段训练模型 注意这里不用加后辍
model_2.load('finetuning/mnist_model')
# 模型配置
model_2.prepare(paddle.optimizer.Adam(learning_rate=0.001, parameters=network.parameters()),
                paddle.nn.CrossEntropyLoss(),
                paddle.metric.Accuracy()
                )
model_2.fit(train_dataset,  # 训练数据集
            eval_dataset,  # 评估数据集
            epochs=2,  # 训练的总轮次
            batch_size=64,  # 训练使用的批大小
            verbose=1,  # 日志展示形式
            )

# 保存用于后续推理部署的模型
model_2.save('infer/mnist_static_model', training=False)
```

<h4>卷积网络 手写数字识别</h4>

```python
import paddle
import numpy as np
import matplotlib.pyplot as plt
import paddle.vision.transforms as T
import paddle.nn as nn

transform = T.Normalize(mean=[127.5], std=[127.5])

# 训练数据集
train_dataset = paddle.vision.datasets.MNIST(mode='train', transform=transform)

# 验证数据集
eval_dataset = paddle.vision.datasets.MNIST(mode='test', transform=transform)

print('训练样本量：{}，测试样本量：{}'.format(len(train_dataset), len(eval_dataset)))


class LeNet(nn.Layer):
    """
    继承paddle.nn.Layer定义网络结构
    """

    def __init__(self, num_classes=10):
        """
        初始化函数
        """
        super(LeNet, self).__init__()

        self.features = nn.Sequential(
            nn.Conv2D(in_channels=1, out_channels=6, kernel_size=3, stride=1, padding=1),  # 第一层卷积
            nn.ReLU(),  # 激活函数
            nn.MaxPool2D(kernel_size=2, stride=2),  # 最大池化，下采样
            nn.Conv2D(in_channels=6, out_channels=16, kernel_size=5, stride=1, padding=0),  # 第二层卷积
            nn.ReLU(),  # 激活函数
            nn.MaxPool2D(kernel_size=2, stride=2)  # 最大池化，下采样
        )

        self.fc = nn.Sequential(
            nn.Linear(400, 120),  # 全连接
            nn.Linear(120, 84),  # 全连接
            nn.Linear(84, num_classes)  # 输出层
        )

    def forward(self, inputs):
        """
        前向计算
        """
        y = self.features(inputs)
        y = paddle.flatten(y, 1)
        out = self.fc(y)

        return out


network_1 = LeNet()
paddle.summary(network_1, (1, 1, 28, 28))

network_2 = paddle.vision.models.LeNet(num_classes=10)
paddle.summary(network_2, (1, 1, 28, 28))

# 模型封装
model = paddle.Model(network_2)

# 模型配置
model.prepare(paddle.optimizer.Adam(learning_rate=0.001, parameters=model.parameters()),  # 优化器
              paddle.nn.CrossEntropyLoss(),  # 损失函数
              paddle.metric.Accuracy())  # 评估指标

# 启动全流程训练
model.fit(train_dataset,  # 训练数据集
          eval_dataset,  # 评估数据集
          epochs=5,  # 训练轮次
          batch_size=64,  # 单次计算数据样本量
          verbose=1)  # 日志展示形式

result = model.evaluate(eval_dataset, verbose=1)

print(result)
# 进行预测操作
result = model.predict(eval_dataset)


# 定义画图方法
def show_img(img, predict):
    plt.figure()
    plt.title('predict: {}'.format(predict))
    plt.imshow(img.reshape([28, 28]), cmap=plt.cm.binary)
    plt.show()


# 抽样展示
indexs = [2, 15, 38, 211]

for idx in indexs:
    show_img(eval_dataset[idx][0], np.argmax(result[0][idx]))

model.save('finetuning/mnist')
```