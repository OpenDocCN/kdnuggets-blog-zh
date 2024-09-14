# YOLOv5 PyTorch 教程

> 原文：[https://www.kdnuggets.com/2022/12/yolov5-pytorch-tutorial.html](https://www.kdnuggets.com/2022/12/yolov5-pytorch-tutorial.html)

![YOLOv5 PyTorch 教程](../Images/677a6a1db1a597d6d66c9f2dce9867b3.png)

# 使用YOLOv5进行PyTorch开发

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 在IT领域支持你的组织

* * *

YOLO，即“你只看一次”的缩写，是一种开源软件工具，因其高效的实时物体检测能力而被广泛使用。YOLO算法使用卷积神经网络（CNN）模型来检测图像中的物体。

该算法仅需一次前向传播即可检测图像中的所有对象。这使得YOLO算法在速度上优于其他算法，成为迄今为止最知名的检测算法之一。

# 什么是YOLO物体检测？

物体检测算法是一种能够在给定帧中检测特定对象或形状的算法。例如，简单的检测算法可能能够检测和识别图像中的形状，如圆形或方形，而更高级的检测算法可以检测更复杂的对象，如人类、自行车、汽车等。

YOLO算法不仅通过其一次前向传播能力提供了高检测速度和性能，而且还以极高的准确性和精度进行检测。

在本教程中，我们将重点关注[YOLOv5](https://github.com/ultralytics/yolov5)，这是YOLO软件的第五个也是最新的版本。它最初发布于2020年5月18日。YOLO开源代码可以在[GitHub](https://github.com/ultralytics/yolov5)上找到。我们将使用YOLO与著名的PyTorch库。

[PyTorch](https://pytorch.org/)是一个基于著名Torch库的深度学习开源包。它也是一个基于Python的库，通常用于自然语言处理和计算机视觉。

# YOLO算法是如何工作的？

## 第一步：残差块（将图像划分为更小的网格状框）

在这一步骤中，完整（整体）帧被划分为更小的框或网格。

所有网格都绘制在原始图像上，形状和大小完全一致。这些分割的想法在于每个网格框将检测其内部的不同对象。

![YOLOv5 PyTorch 教程](../Images/b697b57190ca3e06f2f0b3b6d35ee00f.png)

## 步骤 2：边界框回归（识别边界框中的对象）

在检测到图像中的给定对象后，会绘制一个边界框将其包围。边界框具有诸如**中心点、高度、宽度和类别（检测到的对象类型）**等参数。

![YOLOv5 PyTorch 教程](../Images/e8b6c4eab9ee049ccef69744a4194440.png)

## 步骤 3：交并比（IOU）

![YOLOv5 PyTorch 教程](../Images/30111de8b4f85a72d7daa1d9005134ca.png)

**IOU**，即**交并比**，用于计算我们模型的准确性。这是通过量化两个框的交集程度来实现的，这两个框分别是实际值框（图中的红框）和从结果中返回的框（图中的蓝框）。

在本文的教程部分，我们将 IOU 值确定为 40%，这意味着如果两个框的交集低于 40%，则不应考虑该预测。这是为了帮助我们计算预测的准确性。

下面是显示 YOLO 检测算法完整过程的图像

![YOLOv5 PyTorch 教程](../Images/1c627f3285b1a1621a7bf28cb7243337.png)

要了解更多关于 YOLO 算法如何工作的详细信息，请查看[YOLO 算法简介](https://www.section.io/engineering-education/introduction-to-yolo-algorithm-for-object-detection/#:~:text=YOLO%20is%20an%20algorithm%20that,%2C%20parking%20meters%2C%20and%20animals.)。

# 我们希望通过模型实现什么目标？

本教程示例的主要目标是使用 YOLO 算法检测给定图像中的一系列胸部疾病。与任何机器学习模型一样，我们将使用成千上万张胸部扫描图像来运行我们的模型。目标是让 YOLO 算法成功检测出给定图像中的所有病变。

## 数据集

本教程中使用的[VinBigData 512 图像数据集](https://www.kaggle.com/datasets/awsaf49/vinbigdata-512-image-dataset)可以在 Kaggle 上找到。数据集分为两部分，训练数据集和测试数据集。训练数据集包含 15,000 张图像，而测试数据集包含 3,000 张。这种数据划分在训练数据集通常是测试数据集的 4 到 5 倍的情况下是比较理想的。

![YOLOv5 PyTorch 教程](../Images/495e553306a0277e683d55dfe65641c1.png)

数据集的另一部分包含所有图像的标签。在这个数据集中，每张图像都标记有一个类别名称（发现的胸部疾病），以及类别 ID、图像的宽度和高度等。查看下面的图像以查看所有可用的列。

![YOLOv5 PyTorch 教程](../Images/ab2b60451cc644a880b98d46a39186da.png)

# YOLOv5 教程

> **注意：** 你可以在 Kaggle 上查看[原始代码](https://www.kaggle.com/code/mostafaibrahim17/yolov5/notebook)。

## 步骤 1：导入必要的库

首先，我们将在代码的最开始导入所需的库和包。首先，让我们解释一下我们刚刚导入的一些更常见的库。NumPy 是一个开源的 Python 数值库，允许用户创建矩阵并对其执行多种数学操作。

```py
import pandas as pd
import os
import numpy as np
import shutil
import ast
from sklearn import model_selection
from tqdm import tqdm
import wandb
from sklearn.model_selection import GroupKFold\
from IPython.display import Image, clear_output  # to display images
from os import listdir
from os.path import isfile
from glob import glob
import yaml
# clear_output()
```

## 步骤 2：定义我们的路径

为了简化我们的工作，我们将首先定义训练和测试数据集标签和图像的直接路径。

```py
TRAIN_LABELS_PATH = './vinbigdata/labels/train'
VAL_LABELS_PATH = './vinbigdata/labels/val'
TRAIN_IMAGES_PATH = './vinbigdata/images/train' #12000
VAL_IMAGES_PATH = './vinbigdata/images/val' #3000
External_DIR = '../input/vinbigdata-512-image-dataset/vinbigdata/train' # 15000
os.makedirs(TRAIN_LABELS_PATH, exist_ok = True)
os.makedirs(VAL_LABELS_PATH, exist_ok = True)
os.makedirs(TRAIN_IMAGES_PATH, exist_ok = True)
os.makedirs(VAL_IMAGES_PATH, exist_ok = True)
size = 51
```

## 步骤 3：导入和读取文本数据集

在这里，我们将导入和读取文本数据集。这些数据以 CSV 文件格式存储为行和列。

```py
df = pd.read_csv('../input/vinbigdata-512-image-dataset/vinbigdata/train.csv')
df.head()
```

> **注意：** df.head() 函数打印给定数据集的前 5 行。

## 步骤 4：过滤和清理数据集

由于没有数据集是完美的，因此大多数时候需要一个过滤过程来优化数据集，从而优化我们模型的性能。在此步骤中，我们将删除任何 class id 等于 14 的行。

这个 class id 代表疾病类别中没有发现的情况。我们删除这个类别的原因是它可能会混淆我们的模型。此外，它还会减慢模型的速度，因为我们的数据集会稍微变大。

```py
df = df[df.class_id!=14].reset_index(drop = True)
```

## 步骤 5：计算 YOLO 的边界框坐标

如前所述在**‘YOLO 算法如何工作部分’**（特别是步骤 1 和 2），YOLO 算法期望数据集以某种格式存在。在这里，我们将遍历数据框并应用一些转换。

以下代码的最终目标是计算每个数据点的新 x-mid、y-mid、宽度和高度维度。

```py
df['x_min'] = df.apply(lambda row: (row.x_min)/row.width, axis = 1)*float(size)
df['y_min'] = df.apply(lambda row: (row.y_min)/row.height, axis = 1)*float(size)
df['x_max'] = df.apply(lambda row: (row.x_max)/row.width, axis =1)*float(size)
df['y_max'] = df.apply(lambda row: (row.y_max)/row.height, axis =1)*float(size)

df['x_mid'] = df.apply(lambda row: (row.x_max+row.x_min)/2, axis =1)
df['y_mid'] = df.apply(lambda row: (row.y_max+row.y_min)/2, axis =1)

df['w'] = df.apply(lambda row: (row.x_max-row.x_min), axis =1)
df['h'] = df.apply(lambda row: (row.y_max-row.y_min), axis =1)

df['x_mid'] /= float(size)
df['y_mid'] /= float(size)

df['w'] /= float(size)
df['h'] /= float(size)
```

## 步骤 6：更改提供的数据格式

在代码的这一部分，我们将把数据集中的所有行的给定数据格式更改为以下列：<class> <x_center> <y_center> <width> <height>。这是必要的，因为 YOLOv5 算法只能以这种特定格式读取数据。

```py
def preproccess_data(df, labels_path, images_path):
    for column, row in tqdm(df.iterrows(), total=len(df)):
        attributes = row[
            ["class_id", "x_mid", "y_mid", "w", "h"]
        ].values
        attributes = np.array(attributes)
        np.savetxt(
            os.path.join(labels_path, f"{row['image_id']}.txt"),
            [attributes],
            fmt=["%d", "%f", "%f", "%f", "%f"],
        )
        shutil.copy(
            os.path.join(
                "/kaggle/input/vinbigdata-512-image-dataset/vinbigdata/train",
                f"{row['image_id']}.png",
            ),
            images_path,
        )
```

然后，我们将运行 preproccess_data 函数两次，一次使用训练数据集及其图像，另一次使用测试数据集及其图像。

```py
preproccess_data(df, TRAIN_LABELS_PATH, TRAIN_IMAGES_PATH)
preproccess_data(val_df, VAL_LABELS_PATH, VAL_IMAGES_PATH)
```

使用以下代码行，我们将把 YOLOv5 算法克隆到我们的模型中。

```py
!git clone https://github.com/ultralytics/yolov5.git
```

## 步骤 7：定义我们模型的类别

在这里，我们将把模型中可用的 14 种胸部疾病定义为类别。这些是可以在数据集的图像中识别的实际疾病。

```py
classes = [ 'Aortic enlargement',
            'Atelectasis',
            'Calcification',
            'Cardiomegaly',
            'Consolidation',
            'ILD',
            'Infiltration',
            'Lung Opacity',
            'Nodule/Mass',
            'Other lesion',
            'Pleural effusion',
            'Pleural thickening',
            'Pneumothorax',
            'Pulmonary fibrosis']

data = dict(
    train =  '../vinbigdata/images/train',
    val   =  '../vinbigdata/images/val',
    nc    = 14,
    names = classes
    )

with open('./yolov5/vinbigdata.yaml', 'w') as outfile:
    yaml.dump(data, outfile, default_flow_style=False)

f = open('./yolov5/vinbigdata.yaml', 'r')
print('\nyaml:')
print(f.read())
```

## 步骤 8：训练模型

首先，我们将打开 YOLOv5 目录。然后，我们将使用 pip 安装 requirements 文件中列出的所有库。

requirements 文件包含代码库运行所需的所有库。我们还将安装其他库，例如 pycocotools、seaborn 和 pandas。

```py
%cd ./yolov5
!pip install -U -r requirements.txt
!pip install pycocotools>=2.0 seaborn>=0.11.0 pandas thop
clear_output()
```

Wandb，缩写自 weights and biases，允许我们监控给定的神经网络模型。

```py
# b39dd18eed49a73a53fccd7b684ea7ecaed75b08
wandb.login()
```

现在我们将对提供的vinbigdata数据集进行100轮的YOLOv5训练。我们还将传递一些其他标志，例如 --img 512，表示我们的图像大小为512像素， --batch 16 允许我们的模型每批处理16张图像。使用 --data ./vinbigdata.yaml 标志，我们将传递我们的数据集，即vinbigdata.yaml数据集。

```py
!python train.py --img 512 --batch 16 --epochs 100 --data ./vinbigdata.yaml --cfg models/yolov5x.yaml --weights yolov5x.pt --cache --name vin 
```

## 第9步：评估模型

首先，我们将识别测试数据集目录以及权重目录。

```py
test_dir = (
    f"/kaggle/input/vinbigdata-{size}-image-dataset/vinbigdata/test"
)
weights_dir = "./runs/train/vin3/weights/best.pt"
os.listdir("./runs/train/vin3/weights")
```

在这一部分，我们将使用detect.py作为推理工具来检查我们预测的准确性。我们还将传递一些标志，例如 --conf 0.15\，这是模型的置信度阈值。如果检测到的对象的置信度低于15%，则将其从输出中删除。 --iou 0.4\ 标志告诉我们的模型，如果两个框的交集与并集的比例低于40%，则应将其移除。

```py
!python detect.py --weights $weights_dir\
--img 512\
--conf 0.15\
--iou 0.4\
--source $test_dir\
--save-txt --save-conf --exist-ok
```

# 最后的思考

在这篇文章中，我们解释了YOLOv5是什么以及基本的YOLO算法是如何工作的。接下来，我们简要说明了PyTorch。然后我们覆盖了几个使用YOLO而不是其他类似检测算法的理由。

最后，我们带您了解了一个能够检测X光图像中胸部疾病的机器学习模型。在这个示例中，我们使用YOLO作为主要的检测算法来查找和定位胸部病变。然后，我们将每个病变分类为特定的类别或疾病。

如果你对机器学习和构建自己的模型感兴趣，特别是那些需要检测给定图像或视频中多个对象的模型，那么YOLOv5绝对值得一试。

**[Kevin Vu](https://www.kdnuggets.com/author/kevin-vu)** 负责管理Exxact Corp博客，并与许多撰写有关深度学习不同方面的才华横溢的作者合作。

[原文](https://exxactcorp.com/blog/Deep-Learning/YOLOv5-PyTorch-Tutorial) 经许可转载。

### 更多相关主题

+   [使用PyTorch解释性神经网络](https://www.kdnuggets.com/2022/01/interpretable-neural-networks-pytorch.html)

+   [深度学习的完整免费PyTorch课程](https://www.kdnuggets.com/2022/10/complete-free-pytorch-course-deep-learning.html)

+   [PyTorch Lightning入门](https://www.kdnuggets.com/2022/12/getting-started-pytorch-lightning.html)

+   [调整PyTorch中的Adam优化器参数](https://www.kdnuggets.com/2022/12/tuning-adam-optimizer-parameters-pytorch.html)

+   [使用PyTorch的迁移学习实用指南](https://www.kdnuggets.com/2023/06/practical-guide-transfer-learning-pytorch.html)

+   [提升PyTorch生产力的技巧](https://www.kdnuggets.com/2023/08/pytorch-tips-boost-productivity.html)
