# 使用 PyTorch 构建卷积神经网络

> 原文：[`www.kdnuggets.com/building-a-convolutional-neural-network-with-pytorch`](https://www.kdnuggets.com/building-a-convolutional-neural-network-with-pytorch)

![使用 PyTorch 构建卷积神经网络](img/ef57307f5583535b4a1ad4932adafb8b.png)

作者提供的图像

# 介绍

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

卷积神经网络（CNN 或 ConvNet）是一种深度学习算法，专门用于对象识别至关重要的任务——如图像分类、检测和分割。CNN 能够在复杂的视觉任务上达到最先进的准确率，推动许多实际应用，如监控系统、仓库管理等。

作为人类，我们可以通过分析模式、形状和颜色轻松识别图像中的对象。CNN 也可以通过学习哪些模式对于区分很重要来进行这种识别。例如，当尝试区分猫和狗的照片时，我们的大脑会关注独特的形状、纹理和面部特征。CNN 学会捕捉这些相同类型的区分特征。即使是非常细致的分类任务，CNN 也能直接从像素中学习复杂的特征表示。

在这篇博客文章中，我们将学习卷积神经网络以及如何使用它们与 PyTorch 构建图像分类器。

# 卷积神经网络是如何工作的？

卷积神经网络（CNNs）通常用于图像分类任务。从高层次看，CNN 包含三种主要类型的层：

1.  **卷积层。** 对输入应用卷积滤波器以提取特征。这些层中的神经元称为滤波器，捕捉输入中的空间模式。

1.  **池化层。** 对卷积层的特征图进行降采样，以整合信息。最大池化和平均池化是常用的策略。

1.  **全连接层。** 将来自卷积层和池化层的高级特征作为分类的输入。可以堆叠多个全连接层。

卷积滤波器作为特征检测器，当它们看到输入图像中的特定类型模式或形状时会激活。随着这些滤波器在图像上的应用，它们生成的特征图会突出显示某些特征的存在位置。

例如，当一个过滤器看到垂直线时，它可能会激活，产生一个特征图，显示图像中的垂直线。应用于相同输入的多个过滤器会产生一系列特征图，捕捉图像的不同方面。

![使用 PyTorch 构建卷积神经网络](img/d6733d2f3d9da90f08c34a309f0339f6.png)

Gif 来源于 [IceCream Labs](https://icecreamlabs.medium.com/3x3-convolution-filters-a-popular-choice-75ab1c8b4da8)

通过堆叠多个卷积层，CNN 可以学习特征的层次结构——从简单的边缘和模式构建到更复杂的形状和物体。池化层有助于巩固特征表示并提供平移不变性。

最终的全连接层将这些学到的特征表示用于分类。对于图像分类任务，输出层通常使用 softmax 激活函数来生成类别的概率分布。

在 PyTorch 中，我们可以定义卷积层、池化层和全连接层来构建 CNN 架构。以下是一些示例代码：

```py
# Conv layers 
self.conv1 = nn.Conv2d(in_channels, out_channels, kernel_size)
self.conv2 = nn.Conv2d(in_channels, out_channels, kernel_size)

# Pooling layer
self.pool = nn.MaxPool2d(kernel_size)

# Fully-connected layers 
self.fc1 = nn.Linear(in_features, out_features)
self.fc2 = nn.Linear(in_features, out_features)
```

然后我们可以在图像数据上训练 CNN，使用反向传播和优化。卷积层和池化层将自动学习有效的特征表示，使网络在视觉任务上表现出色。

# 入门 CNN

在这一部分，我们将加载 CIFAR10 数据集，并使用 PyTorch 构建和训练一个基于 CNN 的分类模型。CIFAR10 数据集提供了十个类别的 32x32 RGB 图像，非常适合测试图像分类模型。共有十个类别，标记为整数 0 到 9。

**注意：** 示例代码是来自 [MachineLearningMastery.com](https://machinelearningmastery.com/building-a-convolutional-neural-network-in-pytorch/) 博客的修改版本。

首先，我们将使用 torchvision 下载和加载 CIFAR10 数据集。我们还将使用 torchvision 将测试集和训练集转换为张量。

```py
import torch
import torch.nn as nn
import torch.optim as optim
import torchvision

transform = torchvision.transforms.Compose(
    [torchvision.transforms.ToTensor()]
)

train = torchvision.datasets.CIFAR10(
    root="data", train=True, download=True, transform=transform
)

test = torchvision.datasets.CIFAR10(
    root="data", train=False, download=True, transform=transform
)
```

```py
Downloading https://www.cs.toronto.edu/~kriz/cifar-10-python.tar.gz to data/cifar-10-python.tar.gz

100%|██████████| 170498071/170498071 [00:10<00:00, 15853600.54it/s]

Extracting data/cifar-10-python.tar.gz to data
Files already downloaded and verified
```

之后，我们将使用数据加载器将图像分成批次。

```py
batch_size = 32
trainloader = torch.utils.data.DataLoader(
    train, batch_size=batch_size, shuffle=True
)
testloader = torch.utils.data.DataLoader(
    test, batch_size=batch_size, shuffle=True
)
```

为了在图像的一批中可视化图像，我们将使用 matplotlib 和 torchvision 实用函数。

```py
from torchvision.utils import make_grid
import matplotlib.pyplot as plt

def show_batch(dl):
    for images, labels in dl:
        fig, ax = plt.subplots(figsize=(12, 12))
        ax.set_xticks([]); ax.set_yticks([])
        ax.imshow(make_grid(images[:64], nrow=8).permute(1, 2, 0))
        break
show_batch(trainloader)
```

正如我们所见，我们有汽车、动物、飞机和船只的图像。

![使用 PyTorch 构建卷积神经网络](img/b79babc9d3e180de665c0c58332fc19d.png)

接下来，我们将构建我们的 CNN 模型。为此，我们需要创建一个 Python 类并初始化卷积层、最大池化层和全连接层。我们的架构包含 2 个卷积层以及池化和线性层。

初始化后，我们不会在前向函数中按顺序连接所有层。如果你是 PyTorch 新手，你应该阅读 可解释的 PyTorch 神经网络 来详细了解每个组件。

```py
class CNNModel(nn.Module):
    def __init__(self):
        super().__init__()
        self.conv1 = nn.Conv2d(3, 32, kernel_size=(3,3), stride=1, padding=1)
        self.act1 = nn.ReLU()
        self.drop1 = nn.Dropout(0.3)

        self.conv2 = nn.Conv2d(32, 32, kernel_size=(3,3), stride=1, padding=1)
        self.act2 = nn.ReLU()
        self.pool2 = nn.MaxPool2d(kernel_size=(2, 2))

        self.flat = nn.Flatten()

        self.fc3 = nn.Linear(8192, 512)
        self.act3 = nn.ReLU()
        self.drop3 = nn.Dropout(0.5)

        self.fc4 = nn.Linear(512, 10)

    def forward(self, x):
        # input 3x32x32, output 32x32x32
        x = self.act1(self.conv1(x))
        x = self.drop1(x)
        # input 32x32x32, output 32x32x32
        x = self.act2(self.conv2(x))
        # input 32x32x32, output 32x16x16
        x = self.pool2(x)
        # input 32x16x16, output 8192
        x = self.flat(x)
        # input 8192, output 512
        x = self.act3(self.fc3(x))
        x = self.drop3(x)
        # input 512, output 10
        x = self.fc4(x)
        return x
```

我们现在将初始化我们的模型，设置损失函数和优化器。

```py
model = CNNModel()
loss_fn = nn.CrossEntropyLoss()
optimizer = optim.SGD(model.parameters(), lr=0.001, momentum=0.9)
```

在训练阶段，我们将训练我们的模型 10 个 epoch。

1.  我们使用模型的前向函数进行前向传播，然后使用损失函数进行反向传播，最后更新权重。这一步骤在所有类型的神经网络模型中几乎是相似的。

1.  之后，我们使用测试数据加载器在每个训练轮次结束时评估模型性能。

1.  计算模型的准确性并打印结果。

```py
n_epochs = 10
for epoch in range(n_epochs):
    for i, (images, labels) in enumerate(trainloader):
        # Forward pass 
        outputs = model(images)
        loss = loss_fn(outputs, labels)

        # Backward pass and optimize
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()
    correct = 0
    total = 0
    with torch.no_grad():
        for images, labels in testloader:
            outputs = model(images)
            _, predicted = torch.max(outputs.data, 1)
            total += labels.size(0)
            correct += (predicted == labels).sum().item()

    print('Epoch %d: Accuracy: %d %%' % (epoch,(100 * correct / total))) 
```

我们的简单模型达到了 57% 的准确率，这个结果并不好。但是，你可以通过添加更多层、增加训练轮次以及优化超参数来提高模型性能。

```py
Epoch 0: Accuracy: 41 %
Epoch 1: Accuracy: 46 %
Epoch 2: Accuracy: 48 %
Epoch 3: Accuracy: 50 %
Epoch 4: Accuracy: 52 %
Epoch 5: Accuracy: 53 %
Epoch 6: Accuracy: 53 %
Epoch 7: Accuracy: 56 %
Epoch 8: Accuracy: 56 %
Epoch 9: Accuracy: 57 % 
```

使用 PyTorch，你不必从头创建卷积神经网络的所有组件，因为这些组件已经存在。如果使用 `torch.nn.Sequential`，事情会变得更加简单。PyTorch 被设计为模块化，并在构建、训练和评估神经网络方面提供了更大的灵活性。

# 结论

在这篇文章中，我们探讨了如何使用 PyTorch 构建和训练一个卷积神经网络进行图像分类。我们涵盖了 CNN 架构的核心组件——用于特征提取的卷积层、用于降采样的池化层和用于预测的全连接层。

我希望这篇文章提供了一个有用的概述，介绍了如何使用 PyTorch 实现卷积神经网络。卷积神经网络（CNN）是深度学习中计算机视觉的基础架构，而 PyTorch 使我们能够灵活地快速构建、训练和评估这些模型。

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://www.linkedin.com/in/1abidaliawan/)) 是一位认证的数据科学专业人士，热爱构建机器学习模型。目前，他专注于内容创作和撰写有关机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是利用图神经网络为那些面临心理健康问题的学生开发一个 AI 产品。

### 更多相关内容

+   [通过构建 15 个神经网络项目学习深度学习](https://www.kdnuggets.com/2022/01/15-neural-network-projects-build-2022.html)

+   [使用 TensorFlow 和 Keras 构建和训练您的第一个神经网络](https://www.kdnuggets.com/2023/05/building-training-first-neural-network-tensorflow-keras.html)

+   [卷积神经网络（CNNs）图像分类](https://www.kdnuggets.com/2022/05/image-classification-convolutional-neural-networks-cnns.html)

+   [卷积神经网络全面指南](https://www.kdnuggets.com/2023/06/comprehensive-guide-convolutional-neural-networks.html)

+   [使用 AIMET 优化神经网络](https://www.kdnuggets.com/2022/04/qualcomm-neural-network-optimization-aimet.html)

+   [神经网络预测中排列的重要性](https://www.kdnuggets.com/2022/12/importance-permutation-neural-network-predictions.html)
