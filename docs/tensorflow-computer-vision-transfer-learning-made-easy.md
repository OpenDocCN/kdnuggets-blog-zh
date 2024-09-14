# TensorFlow 计算机视觉 – 迁移学习变得简单

> 原文：[https://www.kdnuggets.com/2022/01/tensorflow-computer-vision-transfer-learning-made-easy.html](https://www.kdnuggets.com/2022/01/tensorflow-computer-vision-transfer-learning-made-easy.html)

**90+% 准确率？通过迁移学习变得可能。**

[上周](https://betterdatascience.com/tensorflow-for-computer-vision-how-to-increase-model-accuracy-with-data-augmentation/)，你已经看到数据增强如何使你的 TensorFlow 模型的准确率提高几个百分点。相比于你今天将看到的，我们仅仅触及了表面。我们将最终用一种相当简单的方法在验证集上超过 90% 的准确率。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 在 IT 领域支持你的组织

* * *

你还会看到如果我们将训练数据量缩小 20 倍，验证准确率会发生什么。剧透 - 它将保持不变。

不想阅读？请观看我的视频：

你可以在 [GitHub](https://github.com/better-data-science/TensorFlow) 上下载源代码。

## TensorFlow 中的迁移学习是什么？

从头开始编写神经网络模型架构涉及大量的猜测工作。多少层？每层多少节点？使用什么激活函数？正则化？你不会很快用完问题。

迁移学习采用不同的方法。不是从头开始，而是利用一个已经由非常聪明的人在庞大的数据集上，用比你家里拥有的更高级的硬件训练过的现有神经网络模型。这些网络可能有数百层，这与我们几周前实现的 [2 层 CNN](https://betterdatascience.com/train-image-classifier-with-convolutional-neural-networks/) 大相径庭。

简而言之 - 你深入网络的层数越多，你提取的特征就越复杂。

整个迁移学习过程归结为 3 个步骤：

1.  **使用预训练的网络** - 例如，使用一个已经在数百万张图像上训练过的 VGG、ResNet 或 EfficientNet 架构，用于检测 1000 个类别。

1.  **剪掉模型的头部** - 排除预训练模型的最后几层，并用你自己的层替换它们。例如，我们的 [狗与猫数据集](https://betterdatascience.com/top-3-prerequisites-for-deep-learning-projects/) 有两个类别，最终的分类层需要与之相符。

1.  **微调最终层** - 在你的数据集上训练网络以调整分类器。预训练模型的权重被冻结，这意味着它们在你训练模型时不会更新。

归根结底，迁移学习使你可以用更少的数据获得显著更好的结果。我们的自定义2块架构在验证集上的准确率仅为76%。迁移学习将把它提高到90%以上。

## 入门 - 库和数据集导入

我们将使用来自Kaggle的[狗与猫数据集](https://www.kaggle.com/pybear/cats-vs-dogs?select=PetImages)。它的许可证是创作共用许可证，这意味着你可以免费使用它：

![TensorFlow用于计算机视觉 - 轻松实现迁移学习](../Images/81716bae384a67575544cf36f83e942d.png)*图像1——狗与猫数据集（图像来源：作者）*

数据集相当大——有25,000张图像，按类别均匀分布（12,500张狗图像和12,500张猫图像）。它应该足够大以训练一个不错的图像分类器。唯一的问题是——它的结构并不适合深度学习。你可以参考我之前的文章创建一个适当的目录结构，并将其拆分为训练集、测试集和验证集：

**[TensorFlow用于图像分类——深度学习项目的三个主要前提 | 更好的数据科学](https://betterdatascience.com/top-3-prerequisites-for-deep-learning-projects)**

你想用TensorFlow训练一个神经网络进行图像分类吗？确保首先完成这三步。

你还应该删除*train/cat/666.jpg*和*train/dog/11702.jpg*图像，因为它们已损坏，模型将无法使用它们进行训练。

完成后，你可以继续进行库的导入。我们今天只需要Numpy和TensorFlow。其他导入是为了消除不必要的警告信息：

```py
import os
os.environ['TF_CPP_MIN_LOG_LEVEL'] = '3' 

import warnings
warnings.filterwarnings('ignore')

import numpy as np
import tensorflow as tf
```

在整篇文章中，我们将不得不从不同的目录加载训练和验证数据。最佳实践是声明一个用于加载图像和[数据增强](https://betterdatascience.com/tensorflow-for-computer-vision-how-to-increase-model-accuracy-with-data-augmentation/)的函数：

```py
def init_data(train_dir: str, valid_dir: str) -> tuple:
    train_datagen = tf.keras.preprocessing.image.ImageDataGenerator(
        rescale=1/255.0,
        rotation_range=20,
        width_shift_range=0.2,
        height_shift_range=0.2,
        shear_range=0.2,
        zoom_range=0.2,
        horizontal_flip=True,
        fill_mode='nearest'
    )
    valid_datagen = tf.keras.preprocessing.image.ImageDataGenerator(
        rescale=1/255.0
    )

    train_data = train_datagen.flow_from_directory(
        directory=train_dir,
        target_size=(224, 224),
        class_mode='categorical',
        batch_size=64,
        seed=42
    )
    valid_data = valid_datagen.flow_from_directory(
        directory=valid_dir,
        target_size=(224, 224),
        class_mode='categorical',
        batch_size=64,
        seed=42
    )

    return train_data, valid_data
```

现在让我们加载我们的狗和猫数据集：

```py
train_data, valid_data = init_data(
    train_dir='data/train/', 
    valid_dir='data/validation/'
)
```

这是你应该看到的输出：

![TensorFlow用于计算机视觉 - 轻松实现迁移学习](../Images/605b0afd68cc15a8c20c5ddd23bdf6f5.png)图像2 - 训练和验证图像的数量（图像来源：作者）

20K训练图像对于迁移学习来说是否过多？可能是，但让我们看看能获得多准确的模型。

## TensorFlow中的迁移学习实践

通过迁移学习，我们基本上是加载一个巨大的预训练模型，但没有顶部的分类层。这样，我们可以冻结已学习的权重，只添加输出层以匹配我们的数据集。

例如，大多数预训练模型是基于*ImageNet*数据集进行训练的，该数据集有1000个类别。我们只有两个（猫和狗），所以我们需要指定这点。

这就是 `build_transfer_learning_model()` 函数的作用所在。它有一个参数 - `base_model` - 表示预训练的架构。首先，我们将冻结该模型中的所有层，然后通过添加几个自定义层来构建一个 `Sequential` 模型。最后，我们将使用常用的方法来编译模型：

```py
def build_transfer_learning_model(base_model):
    # `base_model` stands for the pretrained model
    # We want to use the learned weights, and to do so we must freeze them
    for layer in base_model.layers:
        layer.trainable = False

    # Declare a sequential model that combines the base model with custom layers
    model = tf.keras.Sequential([
        base_model,
        tf.keras.layers.GlobalAveragePooling2D(),
        tf.keras.layers.BatchNormalization(),
        tf.keras.layers.Dropout(rate=0.2),
        tf.keras.layers.Dense(units=2, activation='softmax')
    ])

    # Compile the model
    model.compile(
        loss='categorical_crossentropy',
        optimizer=tf.keras.optimizers.Adam(),
        metrics=['accuracy']
    )

    return model
```

现在有趣的部分开始了。从 TensorFlow 导入 `VGG16` 架构，并将其指定为我们 `build_transfer_learning_model()` 函数的基础模型。`include_top=False` 参数意味着我们不需要顶层分类层，因为我们已经声明了自己的分类层。此外，注意 `input_shape` 如何设置以类似于我们的图像形状：

```py
# Let's use a simple and well-known architecture - VGG16
from tensorflow.keras.applications.vgg16 import VGG16

# We'll specify it as a base model
# `include_top=False` means we don't want the top classification layer
# Specify the `input_shape` to match our image size
# Specify the `weights` accordingly
vgg_model = build_transfer_learning_model(
    base_model=VGG16(include_top=False, input_shape=(224, 224, 3), weights='imagenet')
)

# Train the model for 10 epochs
vgg_hist = vgg_model.fit(
    train_data,
    validation_data=valid_data,
    epochs=10
)
```

这是训练模型 10 个周期后的输出：

![TensorFlow for Computer Vision - 轻松实现迁移学习](../Images/39122c33fe19479e0d46ab9b105f2ce5.png)图像 3 - 在 20K 训练图像上经过 10 个周期的 VGG16 模型（图片由作者提供）

这真是值得一提 - 93% 的验证准确率，甚至不用考虑模型架构。迁移学习的真正优势在于训练准确模型所需的数据量，这比自定义架构所需的数据量要少得多。

**减少了多少？** 让我们将数据集缩小 20 倍，看看会发生什么。

## 在一个缩小了 20 倍的子集上进行迁移学习

我们希望看看减少数据集大小是否会对预测能力产生负面影响。为训练和验证图像创建新的目录结构。图像将存储在 `data_small` 文件夹中，但可以随意将其重命名为其他名称：

```py
import random
import pathlib
import shutil

random.seed(42)
dir_data = pathlib.Path.cwd().joinpath('data_small')
dir_train = dir_data.joinpath('train')
dir_valid = dir_data.joinpath('validation')

if not dir_data.exists(): dir_data.mkdir()
if not dir_train.exists(): dir_train.mkdir()
if not dir_valid.exists(): dir_valid.mkdir()

for cls in ['cat', 'dog']:
    if not dir_train.joinpath(cls).exists(): dir_train.joinpath(cls).mkdir()
    if not dir_valid.joinpath(cls).exists(): dir_valid.joinpath(cls).mkdir()
```

这是你可以用来打印目录结构的命令：

```py
!ls -R data_small | grep ":$" | sed -e 's/:$//' -e 's/[^-][^\/]*\//--/g' -e 's/^/   /' -e 's/-/|/'
```

![TensorFlow for Computer Vision - 轻松实现迁移学习](../Images/7ef0bece76f3a8a575292c59ee516fd6.png)图像 4 - 目录结构（图片由作者提供）

将一部分图像复制到新文件夹中。`copy_sample()` 函数从 `src_folder` 中提取 `n` 张图像并将它们复制到 `tgt_folder`。默认情况下，我们将 `n` 设置为 500：

```py
def copy_sample(src_folder: pathlib.PosixPath, tgt_folder: pathlib.PosixPath, n: int = 500):
    imgs = random.sample(list(src_folder.iterdir()), n)

    for img in imgs:
        img_name = str(img).split('/')[-1]

        shutil.copy(
            src=img,
            dst=f'{tgt_folder}/{img_name}'
        )
```

现在让我们复制训练和验证图像。对于验证集，我们将每类仅复制 100 张图像：

```py
# Train - cat
copy_sample(
    src_folder=pathlib.Path.cwd().joinpath('data/train/cat/'), 
    tgt_folder=pathlib.Path.cwd().joinpath('data_small/train/cat/'), 
)

# Train - dog
copy_sample(
    src_folder=pathlib.Path.cwd().joinpath('data/train/dog/'), 
    tgt_folder=pathlib.Path.cwd().joinpath('data_small/train/dog/'), 
)

# Valid - cat
copy_sample(
    src_folder=pathlib.Path.cwd().joinpath('data/validation/cat/'), 
    tgt_folder=pathlib.Path.cwd().joinpath('data_small/validation/cat/'),
    n=100
)

# Valid - dog
copy_sample(
    src_folder=pathlib.Path.cwd().joinpath('data/validation/dog/'), 
    tgt_folder=pathlib.Path.cwd().joinpath('data_small/validation/dog/'),
    n=100
)
```

使用以下命令打印每个文件夹中的图像数量：

![TensorFlow for Computer Vision - 轻松实现迁移学习](../Images/24cb32aab7a80fcb2f3eb8fa75b1d405.png)图像 5 - 每类训练和验证图像的数量（图片由作者提供）

最后，调用 `init_data()` 函数从新源加载图像：

```py
train_data, valid_data = init_data(
    train_dir='data_small/train/', 
    valid_dir='data_small/validation/'
)
```

![TensorFlow for Computer Vision - 轻松实现迁移学习](../Images/8a1379ff83585f15d74157ed096f5e66.png)图像 6 - 缩小子集中的训练和验证图像数量（图片由作者提供）

总共有 1000 张训练图像。看看我们能否从如此小的数据集中获得一个不错的模型会很有趣。我们将保持模型架构不变，但由于数据集较小，将训练更多周期。此外，由于每个周期的训练时间减少，我们可以进行更长时间的训练：

```py
vgg_model = build_transfer_learning_model(
    base_model=VGG16(include_top=False, input_shape=(224, 224, 3), weights='imagenet')
)

vgg_hist = vgg_model.fit(
    train_data,
    validation_data=valid_data,
    epochs=20
)
```

![TensorFlow for Computer Vision - Transfer Learning Made Easy](../Images/3e1a4d6b74130dba5838e26bd6f2b569.png) 图片 7 - 最后10个周期的训练结果（图片由作者提供）

而且看看这个 - 我们获得了与在2万张图片上训练的模型大致相同的验证准确率，真是太棒了。

这就是迁移学习的真正力量所在。你不总是可以获得庞大的数据集，因此看到我们能在如此有限的数据下建立如此精确的模型，真是令人惊叹。

## 结论

总结一下，当构建图像分类模型时，迁移学习应成为你的首选方法。你无需考虑架构，因为有人已经为你做了这个工作。你无需拥有庞大的数据集，因为有人已经在数百万张图片上训练了通用模型。最后，大多数情况下，你也不需要担心性能差，除非你的数据集非常专业。

你需要做的唯一事情是选择一个预训练的架构。我们今天选择了VGG16，但我鼓励你尝试ResNet、MobileNet、EfficientNet等。

这是另一个**作业** - 使用今天训练的两个模型来预测整个测试集。准确率如何比较？请告知我。

**保持联系**

+   注册我的[通讯](https://mailchi.mp/46a3d2989d9b/bdssubscribe)

+   订阅[YouTube](https://www.youtube.com/c/BetterDataScience)

+   在[LinkedIn](https://www.linkedin.com/in/darioradecic/)上连接

**[Dario Radečić](https://www.linkedin.com/in/darioradecic/)** 是Deep Data Digital的首席执行官和创始人，同时也是数据科学家和技术作家。

[原文](https://betterdatascience.com/tensorflow-transfer-learning/)。转载经许可。

### 相关主题

+   [KDnuggets新闻 2022年3月9日：在5分钟内构建一个机器学习网络应用](https://www.kdnuggets.com/2022/n10.html)

+   [你需要了解的数据管理的6件事及其重要性…](https://www.kdnuggets.com/2022/05/6-things-need-know-data-management-matters-computer-vision.html)

+   [计算机视觉的5个应用](https://www.kdnuggets.com/2022/03/5-applications-computer-vision.html)

+   [DINOv2: Meta AI的自监督计算机视觉模型](https://www.kdnuggets.com/2023/05/dinov2-selfsupervised-computer-vision-models-meta-ai.html)

+   [探索计算机视觉的世界：介绍MLM的最新…](https://www.kdnuggets.com/2024/01/mlm-discover-the-world-of-computer-vision-ebook)

+   [大数据分析师用BigQuery ML轻松搞定机器学习](https://www.kdnuggets.com/machine-learning-made-simple-for-data-analysts-with-bigquery-ml)
