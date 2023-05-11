---
title: 10-2 PyTorch
top: 10.2
tags:
  - PyTorch
categories:
  - PyTorch
---

<h4>花分类</h4>

```python
import json

import matplotlib.pyplot as plt
import torch
from torchvision import datasets, transforms
import os
import numpy as np

data_dir = './flower_data/'
train_dir = data_dir + '/train'
valid_dir = data_dir + '/valid'

# 图像数据增强 数据不够，以此来增加数据量，更高效利用数据
data_transforms = {
    'train': transforms.Compose([transforms.RandomRotation(45),  # 随机翻转，-45度到45度之间随机选
                                 transforms.CenterCrop(224),  # 从中心开始裁剪 , 留下 224*224
                                 transforms.RandomHorizontalFlip(p=0.5),  # 随机水平翻转 选择一个概率 这里50%可能会翻转
                                 transforms.RandomVerticalFlip(p=0.5),  # 随机垂直翻转
                                 transforms.ColorJitter(brightness=0.2, contrast=0.1, saturation=0.1, hue=0.1),
                                 # 参数1为亮度，参数2为对比度，参数3为饱和度，参数4为色相
                                 transforms.RandomGrayscale(p=0.025),  # 概率转换成灰度图 即R=G=B
                                 transforms.ToTensor(),
                                 transforms.Normalize([0.485, 0.456, 0.406], [0.229, 0.224, 0.225]),  # 均值，标准差
                                 ]),
    'valid': transforms.Compose(
        [transforms.Resize(256),
         transforms.CenterCrop(224),
         transforms.ToTensor(),
         transforms.Normalize([0.485, 0.456, 0.406], [0.229, 0.224, 0.225]),
         ])
}

batch_size = 8
# datasets.ImageFolder() 参数一为 路径 参数二 为预处里方法
image_datasets = {x: datasets.ImageFolder(os.path.join(data_dir, x), data_transforms[x]) for x in ['train', 'valid']}
dataloaders = {x: torch.utils.data.DataLoader(image_datasets[x], batch_size=batch_size, shuffle=True) for x in
               ['train', 'valid']}
datasets_sizes = {x: len(image_datasets[x]) for x in ['train', 'valid']}
class_names = image_datasets['train'].classes
print(image_datasets)
print(dataloaders)
print(datasets_sizes)
# 将编号转成实际的名字
with open('cat_to_name.json', 'r') as f:
    cat_to_name = json.load(f)
print(cat_to_name)


def im_convert(tensor):
    image = tensor.to("cpu").clone().detach()
    image = image.numpy().squeeze()
    image = image.transpose(1, 2, 0)
    image = image * np.array((0.229, 0.224, 0.225)) + np.array((0.485, 0.456, 0.406))
    image = image.clip(0, 1)
    return image


fig = plt.figure(figsize=(20, 12))
columns = 4
rows = 2
dataiter = iter(dataloaders['valid'])
inputs, classes = dataiter.next()

for idx in range(columns * rows):
    ax = fig.add_subplot(rows, columns, idx + 1, xticks=[], yticks=[])
    ax.set_title(cat_to_name[str(int(class_names[classes[idx]]))])
    plt.imshow(im_convert(inputs[idx]))

plt.show()
```

<h4>cat_to_name.json</h4>

```json
{
  "21": "fire lily",
  "3": "canterbury bells",
  "45": "bolero deep blue",
  "1": "pink primrose",
  "34": "mexican aster",
  "27": "prince of wales feathers",
  "7": "moon orchid",
  "16": "globe-flower",
  "25": "grape hyacinth",
  "26": "corn poppy",
  "79": "toad lily",
  "39": "siam tulip",
  "24": "red ginger",
  "67": "spring crocus",
  "35": "alpine sea holly",
  "32": "garden phlox",
  "10": "globe thistle",
  "6": "tiger lily",
  "93": "ball moss",
  "33": "love in the mist",
  "9": "monkshood",
  "102": "blackberry lily",
  "14": "spear thistle",
  "19": "balloon flower",
  "100": "blanket flower",
  "13": "king protea",
  "49": "oxeye daisy",
  "15": "yellow iris",
  "61": "cautleya spicata",
  "31": "carnation",
  "64": "silverbush",
  "68": "bearded iris",
  "63": "black-eyed susan",
  "69": "windflower",
  "62": "japanese anemone",
  "20": "giant white arum lily",
  "38": "great masterwort",
  "4": "sweet pea",
  "86": "tree mallow",
  "101": "trumpet creeper",
  "42": "daffodil",
  "22": "pincushion flower",
  "2": "hard-leaved pocket orchid",
  "54": "sunflower",
  "66": "osteospermum",
  "70": "tree poppy",
  "85": "desert-rose",
  "99": "bromelia",
  "87": "magnolia",
  "5": "english marigold",
  "92": "bee balm",
  "28": "stemless gentian",
  "97": "mallow",
  "57": "gaura",
  "40": "lenten rose",
  "47": "marigold",
  "59": "orange dahlia",
  "48": "buttercup",
  "55": "pelargonium",
  "36": "ruby-lipped cattleya",
  "91": "hippeastrum",
  "29": "artichoke",
  "71": "gazania",
  "90": "canna lily",
  "18": "peruvian lily",
  "98": "mexican petunia",
  "8": "bird of paradise",
  "30": "sweet william",
  "17": "purple coneflower",
  "52": "wild pansy",
  "84": "columbine",
  "12": "colt's foot",
  "11": "snapdragon",
  "96": "camellia",
  "23": "fritillary",
  "50": "common dandelion",
  "44": "poinsettia",
  "53": "primula",
  "72": "azalea",
  "65": "californian poppy",
  "80": "anthurium",
  "76": "morning glory",
  "37": "cape flower",
  "56": "bishop of llandaff",
  "60": "pink-yellow dahlia",
  "82": "clematis",
  "58": "geranium",
  "75": "thorn apple",
  "41": "barbeton daisy",
  "95": "bougainvillea",
  "43": "sword lily",
  "83": "hibiscus",
  "78": "lotus lotus",
  "88": "cyclamen",
  "94": "foxglove",
  "81": "frangipani",
  "74": "rose",
  "89": "watercress",
  "73": "water lily",
  "46": "wallflower",
  "77": "passion flower",
  "51": "petunia"
}
```