# 初学者和 Udacity 深度学习纳米学位的 PyTorch 备忘单

> 原文：[https://www.kdnuggets.com/2019/08/pytorch-cheat-sheet-beginners.html](https://www.kdnuggets.com/2019/08/pytorch-cheat-sheet-beginners.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由 [Uniqtech](https://theoptips.github.io/uniqtech_1/) 提供**

使用一致的自上而下的方法入门 Pytorch 的备忘单。这份备忘单应该比官方文档更容易消化，并且应作为一个过渡工具，帮助学生和初学者尽快开始阅读文档。本文正在不断改进，频繁更新，并将保持在建设中，直到显著改善。您的反馈非常感谢 hi@uniqtech.co，错误和拼写错误将会被及时纠正。

大新闻：我们在 Medium 机器学习和数据科学主页上发表了文章。请点击 ← 并评论以表示支持。下面的备忘单主要是叙述性的。详细备忘单的 PDF JPEG 版本将很快发布，并将发布在本文中。更新于 2019 年 6 月 18 日，为了使本备忘单/教程更具连贯性，我们将插入来自获奖 Kaggle 内核的代码片段，以说明重要的 PyTorch 概念 — [使用 PyTorch 进行的疟疾检测，一个图像分类计算机视觉 Kaggle 内核](https://www.kaggle.com/devilsknight/malaria-detection-with-pytorch) [见 Source 3 下] 作者 [devilsknight](https://www.kaggle.com/devilsknight) 和 vishnu aka [qwertypsv](https://www.kaggle.com/qwertypsv)。

![图](../Images/b6bf45cdb45371fd00627cbdd0ae2c1f.png)

uniqtech 提供的初学者 PyTorch 备忘单

### Pytorch 用自己的话定义

**Pytorch 是“一个开源深度学习平台，提供从研究原型到生产部署的无缝路径。”**

> 根据 Facebook 研究 [Source 1]，PyTorch 是一个提供两个高级特性的 Python 包：
> 
> 具有强大 GPU 加速的张量计算（如 NumPy）
> 
> 基于带状自动微分系统构建的深度神经网络
> 
> 你可以在需要时重用你最喜欢的 Python 包，如 NumPy、SciPy 和 Cython，以扩展 PyTorch。
> 
> Soumith Chintala，Facebook 研究工程师和 PyTorch 的创造者，提供了一些关于 PyTorch 的有趣事实：自动微分曾经是用 Python 编写的，但大多数（代码）已改为 C++（以适应生产）。他认为有趣的 PyTorch 1.0 特性包括混合前端、生产模型解析、使用 Jit 编译器使模型适合生产等。来源：Chintala 在 Udacity 学习中的访谈。

### 主要特点

**组件 | 描述** [Source 2]

[**torch**](https://pytorch.org/docs/stable/torch.html)：一个类似于 NumPy 的张量库，具有强大的 GPU 支持

[**torch.autograd**](https://pytorch.org/docs/stable/autograd.html)**：** 一个基于带状的自动微分库，支持 torch 中所有可微分的张量操作

[**torch.jit**](https://pytorch.org/docs/stable/jit.html)：一个编译堆栈（TorchScript），用于从 PyTorch 代码创建可序列化和可优化的模型。

[**torch.nn**](https://pytorch.org/docs/stable/nn.html)：一个深度集成自动求导的神经网络库，设计为最大灵活性。

[**torch.multiprocessing**](https://pytorch.org/docs/stable/multiprocessing.html)：Python 多进程，但具有跨进程共享 torch 张量的神奇内存功能。适用于数据加载和 Hogwild 训练。

[**torch.utils**](https://pytorch.org/docs/stable/data.html)：DataLoader 和其他实用函数，方便使用。

主要特性：混合前端、分布式训练、Python 优先、工具和库。

这些功能通过 [功能页面](https://pytorch.org/features) 上的并排代码示例得到了优雅的展示！

![图像](../Images/da94f943b753a23621a94585147571a6.png)

Pytorch 文档中的功能页面展示了优雅的代码示例以说明每个功能。还需注意 Python 3 的点积简写，如“@”。

混合前端允许在即时模式和（计算）图模式之间切换。Tensorflow 曾经仅支持图模式，这被认为是快速和高效的，但很难修改、原型设计和研究。由于 Tensorflow 现在也提供即时模式（不再需要`session run`），这个差距正在缩小。

分布式训练：支持 GPU、CPU 和两者之间的轻松切换。（Tensorflow 还支持 TPU，即 Tensor 处理单元。）

Python 优先：为 Python 开发者打造。轻松创建神经网络，在 Pytorch 中运行深度学习。工具和库包括强大的计算机视觉库（卷积神经网络和预训练模型）、自然语言处理等。

Pytorch 还包括像 **torch.tensor** 实例化和计算、模型、验证、评分、自动计算梯度的 Pytorch 特性（使用 autograd，它也会为你执行所有的反向传播）、迁移学习准备好的预加载模型和数据集（阅读我们关于迁移学习的超短有效文章），还有使用 CUDA 的 GPU。

### 何时使用 Pytorch

> AWS、Google 和 GCP 的 GPU 支持 Pytorch 作为一流公民。

Pytorch 为 1.0 版本新增了生产和云合作伙伴支持，包括 AWS、Google Cloud Platform 和 Microsoft Azure。现在你可以在任何深度学习任务中使用 Pytorch，包括计算机视觉和自然语言处理，甚至是在生产环境中。

> 因为它易于使用且具有 Python 风格，高级数据科学家 [Stefan Otte](https://sotte.github.io/) 说：“如果你想要有趣的体验，使用 pytorch”。

Pytorch 也得到了 Facebook AI 研究的支持，因此如果你想在 Facebook 从事数据和机器学习工作，你应该了解 Pytorch。如果你擅长 Python 并且想成为开源贡献者，Pytorch 也是一个不错的选择。

**迁移学习**

迁移学习使用模型来预测未曾训练的数据集的类型。它可以显著提高训练时间和准确性。它还可以帮助处理可用训练数据有限的情况。Pytorch 有一个专门页面，介绍了预训练模型及其在行业标准基准数据集上的性能。请在我们的 Pytorch 迁移学习文章中阅读更多内容。

![](../Images/998ce78c4b9cc073011acd533b33cd36.png)

```py
# pretrained models are at torchvision > models
model = torchvision.models.resnet152(pretrained=False)
```

[阅读 Pytorch 文档中所有可用模型](https://pytorch.org/docs/stable/torchvision/models.html)。请注意，模型的 top-1-error 和 top-5-error，即模型的性能也可以查看。

**数据科学，学术研究 | 使用 Jupyter Notebook 进行演示**

由于 Python 拥有庞大的开发者社区，因此在学生和研究人员中更容易找到转向数据科学和学术研究的 Python 人才，甚至使用 Pytorch 编写生产代码。这消除了学习另一种语言的需求。许多数据分析师和科学家已经熟悉 Jupyter Notebook，Pytorch 在其上运行得非常完美。请在我们的部署 Pytorch 模型到 Amazon Web Service SageMaker 中阅读更多内容。

**Pytorch 是一个深度学习框架**

Pytorch 是一个深度学习框架，就像 Tensorflow 一样，这意味着：对于传统的机器学习模型，目前请使用其他工具。Scikit-learn 是一个 Python 风格的深度学习框架，具有极易使用的 API。文档非常好，每页底部都有代码片段的示例。去看看。你知道吗，许多 Kaggle 用户包括大师们仍然使用 sklearn 的 train_test_split() 来拆分和 scaler 来预处理数据，sklearn 的 Gradient Boosting Tree 或支持向量机来基准测试性能，而顶级高性能的 XGBoost 却显著缺失。

**Tensorflow.js** 和 **Tensorflow.lite** 让 Tensorflow 在浏览器和移动设备上展翅飞翔。苹果刚刚在 2019 年 6 月宣布了 Swift 的 CreateML。移动支持在 Pytorch 中尚未原生实现。不要气馁。向下滚动以阅读关于 [ONNX 的内容，它几乎被所有流行框架支持的交换格式](http://onnx.ai/supported-tools)。[Pytorch 也有一个关于将模型移动到移动设备的教程，尽管这条路与 Tensorflow 相比仍有些绕行](https://pytorch.org/tutorials/advanced/super_resolution_with_caffe2.html)。

### 安装 Pytorch

![](../Images/b4233f6a8a1afcd239dbe879eb37cfd2.png)

对于安装技巧，首先使用官方的 Pytorch 文档。[https://pytorch.org/get-started/locally/](https://pytorch.org/get-started/locally/) 以上截图是可用安装的示例。

使用Anaconda安装Pytorch是一个在各种系统中包括Windows上的良好起点。我们能够在游戏电脑上使用Anaconda安装Pytorch并立即开始使用它的CUDA GPU功能。[阅读我们的Anaconda速查表](https://medium.com/data-science-bootcamp/anaconda-miniconda-cheatsheet-for-data-scientists-2c1be12f56db)。

```py
conda install numpy jupyter notebook
conda install pytorch torchvision -c pytorch
```

你好，Pytorch的世界就像在启动Google Colab一样简单（没错，在Google 的领地上），然后`import torch`，[看看这个只读共享笔记本](https://colab.research.google.com/drive/1uK7BnOWj_-MXI4a5h4I9SLMrqhPnG9Qu)。现代托管数据科学笔记本如Kaggle Kernel和Google Colab都预先安装了Pytorch。

> 看呐：无需服务器的深度学习！

更喜欢基于Jupyter Notebook的教程吗？从这篇文章底部找到Udacity Intro to Pytorch仓库开始入门。

更喜欢其他的安装方法？二进制文件，从源代码编译，和Docker镜像，请见来源2。

**来自来源3的代码片段**

```py
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import torch
from torch import nn, optim
from torchvision import transforms, datasets, models
from torch.utils.data.sampler import SubsetRandomSampler

import os
print(os.listdir("../input/cell_images/cell_images/"))
```

导入`torch`是执行核心Pytorch任务所必需的。你还会看到导入torch神经网络模块`nn`、优化器`optim`和计算机视觉模块`torchvision`，数据转换管道`transforms`、`datasets`和现有的`models`。在这种情况下，Kaggle团队还使用了一个`SubsetRandomSampler`，你将在后续片段中看到它如何被输送到数据转换和加载管道中。

### 数据转换

**来自来源3的代码片段**

注意，训练、测试和验证转换器是相似但不同的。为了增广数据，训练数据会被随机旋转、调整大小和裁剪，甚至垂直翻转（在这种情况下，一个翻转的疟疾细胞不会对分类结果产生负面影响）。因为测试和验证数据应该模拟真实世界数据，不引入任何随机噪音或翻转。就像它们一样，带有中心裁剪。注意大小必须保持一致。深度学习涉及大量的矩阵乘法。维度大小总是很重要的。在图像分类任务中，我们通常希望根据将要使用的预训练模型或现有数据集对图像进行归一化。

```py
# Define your transforms for the training, validation, and testing sets
train_transforms = transforms.Compose([transforms.RandomRotation(30),
 transforms.RandomResizedCrop(224),
 transforms.RandomVerticalFlip(),
 transforms.ToTensor(),
 transforms.Normalize([0.485, 0.456, 0.406], 
 [0.229, 0.224, 0.225])])test_transforms = transforms.Compose([transforms.Resize(256),
 transforms.CenterCrop(224),
 transforms.ToTensor(),
 transforms.Normalize([0.485, 0.456, 0.406], 
 [0.229, 0.224, 0.225])])validation_transforms = transforms.Compose([transforms.Resize(256),
 transforms.CenterCrop(224),
 transforms.ToTensor(),
 transforms.Normalize([0.485, 0.456, 0.406], 
 [0.229, 0.224, 0.225])])
```

为什么要进行归一化？那些奇怪数字是什么？均值和标准差。

```py
normalize = transforms.Normalize(mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225]) # Source 4
```

千万别忘记使用ToTensor将所有内容转换为Pytorch张量。

为什么？因为Pytorch需要它。阅读Pytorch的创作者Chimtala关于此的讨论串 [Source 5]。

> 输入图像首先被加载到范围[0, 1]中，然后这种归一化被应用到RGB图像中，如此描述.. torch vision — 专门用于计算机视觉的数据集、转换和模型
> 
> *所有预训练模型都希望输入图像以相同方式进行归一化，即形状为(3 x H x W)的3通道RGB图像的小批量，其中H和W至少应为224。*
> 
> *图像必须被加载到范围[0, 1]中，并且使用均值=[0.485, 0.456, 0.406]和标准差=[0.229, 0.224, 0.225]进行归一化*
> 
> *这种归一化的示例可在此Imagenet示例中找到[来源4]*

### 使用训练和测试加载器加载数据

```py
train_loader = torch.utils.data.DataLoader(train_data, batch_size=batch_size, num_workers=num_workers)
test_loader = torch.utils.data.DataLoader(test_data, batch_size=batch_size, num_workers=num_workers)
```

**源 3 的代码片段**

`datasets.ImageFolder` 和 `torch.utils.data.DataLoader` 一起工作，以根据 batch_size 和之前部分中的数据变换来分别加载训练、验证和测试数据。每个数据集都有自己的加载器。

```py
img_dir='../input/cell_images/cell_images/'
train_data = datasets.ImageFolder(img_dir,transform=train_transforms)... # omitted
*# convert data to a normalized torch.FloatTensor*
... # omitted

*# obtain training indices that will be used for validation*
... # omitted
print(len(valid_idx), len(test_idx), len(train_idx))

*# define samplers for obtaining training and validation batches*
train_sampler = SubsetRandomSampler(train_idx)
valid_sampler = SubsetRandomSampler(valid_idx)
test_sampler = SubsetRandomSampler(test_idx)

*# prepare data loaders (combine dataset and sampler)*
train_loader = torch.utils.data.DataLoader(train_data, batch_size=64,sampler=train_sampler, num_workers=num_workers)
valid_loader = torch.utils.data.DataLoader(train_data, batch_size=32, sampler=valid_sampler, num_workers=num_workers)
test_loader = torch.utils.data.DataLoader(train_data, batch_size=20, sampler=test_sampler, num_workers=num_workers)
```

### Pytorch 模型概述

使用 `Sequential` 是快速定义模型的一种简单方法。一个命名的有序字典包含了所有被封装在 `nn.Sequential` 中的层，然后将其存储在 `model.classifier` 变量中。这是一种快速定义模型基本结构的方法，但不一定是最 Pythonic 的。它帮助我们说明一个 Python 模型由全连接的线性层组成，形状由 (row, col) 元组指定。ReLU 激活层、20% 概率的 Dropout 和一个输出的 Softmax 或 LogSoftmax 函数。现在不用担心这些。你只需要知道 Softmax 通常是**多分类任务**深度学习模型的最后一层。著名的 ImageNet 数据集有 1000 多个类别，因此 Softmax 的输出至少有 1000 个以上的组件。

一个由全连接层组成的集合，间隔着 ReLU 激活层和一些 Dropout，最后是另一个全连接线性层，这个层连接到一个 Softmax 激活层，是一个典型的普通深度学习神经网络结构。

```py
from collections import OrderedDictclassifier = nn.Sequential(OrderedDict([
                          ('fc1', nn.Linear(2048, 1024)),
                          ('relu', nn.ReLU()),
                          ('dropout',nn.Dropout(0.2)),
                          ('fc2', nn.Linear(1024, 512)),
                          ('relu', nn.ReLU()),
                          ('dropout',nn.Dropout(0.2)),
                          ('fc3', nn.Linear(512, 256)),
                          ('relu', nn.ReLU()),
                          ('dropout',nn.Dropout(0.2)),
                          ('fc4', nn.Linear(256, 102)),
                          ('output', nn.LogSoftmax(dim=1))
                          ]))model.classifier = classifier
```

在 Pytorch 中，查看模型结构非常简单，只需使用 `print(model_name)`。稍后会详细介绍。

**源 3 的代码片段**

如前所述，在 pytorch 部分的迁移学习中，我们可以使用如 resnet50 的预训练模型。将所有层的梯度关闭，除了最后新增的全连接层。

```py
model = models.resnet50(pretrained=True)

for param in model.parameters():
    param.requires_grad = False # turn all gradient off

model.fc = nn.Linear(2048, 2, bias=True) 
#add new fully connected layer

fc_parameters = model.fc.parameters()

for param in fc_parameters:
    param.requires_grad = True #turning last layer gradient to true

model
```

注意最后一层是 2048 x 2，因为我们只是分类两个类别：真或假，疟疾或非疟疾。

模型变量返回一个大型的 ResNet 模型结构，并包含我们自定义的最后一层。

```py
Downloading: "https://download.pytorch.org/models/resnet50-19c8e357.pth" to /tmp/.torch/models/resnet50-19c8e357.pth
100%|██████████| 102502400/102502400 [00:01<00:00, 89692748.86it/s]
```

在下面的代码片段中，我们省略了很多细节，以便将模型架构适配到屏幕上。如果你访问源 3 的 kernel 链接，你会看到相当多的瓶颈和顺序层。

```py
(layer1): Sequential(
    (0): Bottleneck(
      (conv1): Conv2d(64, 64, kernel_size=(1, 1), stride=(1, 1), bias=False)
      (bn1): BatchNorm2d(64, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (conv2): Conv2d(64, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn2): BatchNorm2d(64, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (conv3): Conv2d(64, 256, kernel_size=(1, 1), stride=(1, 1), bias=False)
      (bn3): BatchNorm2d(256, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (relu): ReLU(inplace)
      (downsample): Sequential(
        (0): Conv2d(64, 256, kernel_size=(1, 1), stride=(1, 1), bias=False)
        (1): BatchNorm2d(256, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      )
    )
```

在这里，我们展示了最后的自定义层确实是 2048 x 2。注意，在平均池化和 relu 之前的层是 2048，因此是 2048 x 2。

```py
ResNet(
  (conv1): Conv2d(3, 64, kernel_size=(7, 7), stride=(2, 2), padding=(3, 3), bias=False)
... #omitted
    ... #omitted
      (bn3): BatchNorm2d(2048, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (relu): ReLU(inplace)
    )
  )
  (avgpool): AvgPool2d(kernel_size=7, stride=1, padding=0)
  (fc): Linear(in_features=2048, out_features=2, bias=True)
)
```

### Pytorch 训练循环

训练循环可能是 Pytorch 作为深度学习框架的最具特点的部分。在 Sklearn 中可以用 `fit` 来处理，在 Tensorflow 中可以用 `transform`。在 Pytorch 中，这部分要复杂得多。

`model.train()` 告诉你的模型你正在训练它。因此，像 dropout、batchnorm 等层在训练和测试过程中表现不同的情况下会知道发生了什么，并能相应地调整行为。

更多细节：它将模式设置为训练模式（见 [源代码](https://pytorch.org/docs/stable/_modules/torch/nn/modules/module.html#Module.train)）。你可以调用 `model.eval()` 或 `model.train(mode=False)` 来表明你正在进行测试。期望 `train` 函数来训练模型是有些直观的，但实际上它并不会这么做。它只是设置模式。

`model.eval()` 会通知所有层你处于评估模式，这样，batchnorm 或 dropout 层将在评估模式而不是训练模式下工作。

`torch.no_grad()` 影响自动求导引擎并将其停用。它将减少内存使用并加快计算速度，但你将无法进行反向传播（在评估脚本中你不希望进行反向传播）。— albanD

**代码片段训练循环源 3**

```py
use_cuda = torch.cuda.is_available()
if use_cuda:
    model = model.cuda()
criterion = nn.CrossEntropyLoss()
optimizer = optim.SGD(model.fc.parameters(), lr=0.001 , momentum=0.9)
```

在开始训练之前，检查 CUDA 可用性，将准则设置为 CrossEntropyLoss()，将优化器设置为 Stochastic Gradient Descent。例如，注意学习率从非常小的 `lr=0.001` 开始。

注意，训练循环通常包括周期数、模型架构优化器和准则。损失值必须在每个循环开始时先归零！大循环遍历周期数。在每个周期中，我们将模型设置为训练模式 `model.train()`，如有必要，将模型和数据移到 CUDA，`optimizer.zero_grad()` 在开始之前将梯度归零，预测输出，使用准则计算损失，`loss.backward` 基于优化器计算权重变化，`optimizer.step()` 进行一次反向传播步骤以更新权重。在验证时，将模型切换到评估模式 `model.eval()`。如果验证损失有所减少，则保存模型，跟踪最低验证损失。

```py
def train(n_epochs, model, optimizer, criterion, use_cuda, save_path):
    *"""returns trained model"""*
    *# initialize tracker for minimum validation loss*
    valid_loss_min = np.Inf

    for epoch in range(1, n_epochs+1):
        *# initialize variables to monitor training and validation loss*
        train_loss = 0.0
        valid_loss = 0.0

        *###################*
        *# train the model #*
        *###################*
        model.train()
        for batch_idx, (data, target) in enumerate(train_loader):
            *# move to GPU*
            if use_cuda:
                data, target = data.cuda(), target.cuda()

            *# initialize weights to zero*
            optimizer.zero_grad()

            output = model(data)

            *# calculate loss*
            loss = criterion(output, target)

            *# back prop*
            loss.backward()

            *# grad*
            optimizer.step()

            train_loss = train_loss + ((1 / (batch_idx + 1)) * (loss.data - train_loss))

            if batch_idx % 100 == 0:
                print('Epoch %d, Batch %d loss: %.6f' %
                  (epoch, batch_idx + 1, train_loss))

        *######################* 
        *# validate the model #*
        *######################*
        model.eval()
        for batch_idx, (data, target) in enumerate(valid_loader):
            *# move to GPU*
            if use_cuda:
                data, target = data.cuda(), target.cuda()
            *## update the average validation loss*
            output = model(data)
            loss = criterion(output, target)
            valid_loss = valid_loss + ((1 / (batch_idx + 1)) * (loss.data - valid_loss))

        *# print training/validation statistics* 
        print('Epoch: {} \tTraining Loss: {:.6f} \tValidation Loss: {:.6f}'.format(
            epoch, 
            train_loss,
            valid_loss
            ))

        *## TODO: save the model if validation loss has decreased*
        if valid_loss < valid_loss_min:
            torch.save(model.state_dict(), save_path)
            print('Validation loss decreased ({:.6f} --> {:.6f}).  Saving model ...'.format(
            valid_loss_min,
            valid_loss))
            valid_loss_min = valid_loss

    *# return trained model*
    return model
```

开发工具

+   [在多个 GPU 上训练](https://pytorch.org/tutorials/beginner/former_torchies/parallelism_tutorial.html)

训练循环：

训练和前向传播

**云端免费 GPU**

感谢像 Google Colab 这样的新云技术。一旦你设置好笔记本，你可以在移动设备上继续训练和监控！ 云端选择包括 Google Colab、Kaggle 和 AWS。本地选择包括你的个人笔记本电脑和游戏电脑。

我们之前将一台 MSI NVIDIA GTX 1060 重新用于《刺客信条：起源》 :D 如果你想了解更多，请告诉我们。

能够在本地 GPU 上进行训练是一个重大优势。我们能够快速进行参数调优组合的迭代而不会中断。Google Colab 有 12 小时的超时限制和 12GB 的配额限制。如果你是高级用户，务必避免不断下载数据集，而是将其存储在 Google Drive 中。删除 Google Drive 中的模型后，请务必清空回收站以实际删除它。

无论模型在哪里训练，如果训练损失接近零但验证损失没有减少（没有测试数据集），你可能要注意你的模型是否过拟合，可能在记忆训练数据。暂停并调整你的参数。即使你达到了 99% 的准确率，你的模型也可能不具备泛化能力，因此可能无法在其他地方使用。特别是对于 Udacity 的奖学金挑战和纳米学位的学生，这段信息尤其相关。对 99% 的准确率要保持高度怀疑，但可以先庆祝一下。

### 向前传播

Pytorch 向前传播实际上会计算 `y = wx + b`，在此之前我们只是写了占位符。

### 有用的 Pytorch 库和模块及其安装。

```py
import torch
import torchvision
from torchvision import datasets, transforms, models
import torch.nn.functional as F
from collections import OrderedDict
from torch import nn
from torch import optim
```

Torchvision 模块非常强大。它包含图像处理和数据处理代码、卷积神经网络（CNN）以及其他预训练模型，如 ResNet 和 VGG。

### 转换

+   `transforms.ToTensor()` 将数据数组转换为 Pytorch 张量。优点包括在 CUDA 和 GPU 训练中易于使用。

+   开发人员可以在 Pytorch 中 [构建转换和转换管道](https://pytorch.org/docs/stable/torchvision/transforms.html)。管道是指将多个转换串联在一起并顺序执行的一种方式。

### Pytorch 卷积神经网络（CNN）

本节正在建设中… 请稍后再查看。

使用卷积神经网络完成计算机视觉、图像识别任务。转换和数据增强 API 非常重要，特别是在训练数据有限时。或者，也可以使用许多预加载的模型架构 —— 阅读我们的转移学习部分。

```py
import torch
import numpy as np
from torchvision import datasets
import torchvision.transforms as transforms
```

最大池化层会丢弃原始图像中包含的详细空间信息。

### 检查 Pytorch 模型

首先初始化模型。

```py
vgg16 = models.vgg16(pretrained=True)
print(vgg16)
```

在 Pytorch 中，使用 `print(model_name)` 来打印模型及其架构。你可以轻松地了解模型的详细信息。

```py
VGG(
  (features): Sequential(
    (0): Conv2d(3, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
    (1): ReLU(inplace)
    (2): Conv2d(64, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
...
    (24): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
...
    (29): ReLU(inplace)
    (30): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
  )
  (classifier): Sequential(
    (0): Linear(in_features=25088, out_features=4096, bias=True)
    (1): ReLU(inplace)
    (2): Dropout(p=0.5)
    (3): Linear(in_features=4096, out_features=4096, bias=True)
    (4): ReLU(inplace)
    (5): Dropout(p=0.5)
    (6): Linear(in_features=4096, out_features=1000, bias=True)
  )
)
```

注意，每一层都有名称（编号，并且可以通过这个索引进行查询）。我们使用省略号来省略模型细节，以便 VGG 模型能够适应屏幕。

专业提示：检查模型架构是必须的。转移学习，简而言之，就是修改最后几个分类器层。[在我们的转移学习文章中阅读更多](https://medium.com/data-science-bootcamp/transfer-learning-with-pytorch-code-snippet-load-a-pretrained-model-900374950004)。

专业提示：对于 Tensorflow，使用 `keras model.summary()` 来查看整个模型架构。它甚至会输出参数数量和维度。

### 高级特性

Pytorch 版本

对于Udacity项目，并非所有的纳米学位都已经迁移到Pytorch 1.0。使用正确版本的Pytorch对相应的项目至关重要。你可能还需要在Jupyter Notebook中更改内核，以使用相应版本的Python。Udacity项目已经迁移到Python3。并非所有现实世界中的项目都已迁移到Python 3，但现在是时候从Python 2迁移过来了。

**使用CUDA**

虽然我们决定将CUDA放在高级部分，但现实是CUDA非常易于使用。今天就使用它吧！通过Anaconda、Pytorch和CUDA，我们将一台配备NVIDIA显卡的游戏电脑变成了家庭深度学习的强大机器。无需配置！它直接工作。这个框架在Windows机器上就能运行！（这是之前用于《刺客信条：起源》的MSI NVIDIA GTX 1060 :D 如果你想了解更多，请告诉我们。）

```py
gpu_is_avail = torch.cuda.is_available()if not gpu_is_avail:
     print('CUDA is NOT available.')
else:
     print('CUDA is available. ')device = torch.device(“cuda” if torch.cuda.is_available() else “cpu”)
```

使用CUDA与Pytorch的一个常见错误是不将模型和数据同时移动到CUDA上。当需要时将它们都移回CPU。一般来说，你的模型和数据应该始终位于同一个空间。

**在生产环境中部署Pytorch**：将现有的Pytorch模型转换为生产就绪的部署有两种方法——*追踪和脚本*。追踪不支持具有代码控制流的复杂模型。脚本支持具有控制流的Pytorch代码，但仅支持有限数量的Python模块。

**选择最佳的Softmax结果**：在多类别分类中，通常使用激活函数Softmax。Pytorch有一个专门的函数来提取最顶级的结果——从Softmax输出中最可能的类别。`torch.topk(input, k, dim)`返回顶级概率。

```py
torch.topk(input, k, dim=None, largest=True, sorted=True, out=None) -> (Tensor, LongTensor)
Returns the k largest elements of the given input tensor along a given dimension.
If dim is not given, the last dimension of the input is chosen.
If largest is False then the k smallest elements are returned.
```

**在其他平台上使用Pytorch模型**

```py
import torch.onnx
  import torchvision

  dummy_input = torch.randn(1, 3, 224, 224)
  model = torchvision.models.alexnet(pretrained=True)
  torch.onnx.export(model, dummy_input, "alexnet.onnx")
```

> “以标准ONNX（开放神经网络交换）格式导出模型，以便直接访问ONNX兼容的平台、运行时、可视化工具等。” — Pytorch 1.0文档

**更多关于Pytorch迁移学习的内容**

使用现有模型相当于冻结其某些层和参数，并且不对这些层和参数进行训练。通过将`require_grad`设置为False来关闭训练自动求导。

```py
for param in model.parameters():
 param.requires_grad = False
```

**超参数调整**

除了使用正确的优化器和调整学习率之外，你还可以使用学习率调度器动态调整学习率。

```py
#define scheduler
scheduler = lr_scheduler.StepLR(optimizer, step_size=3, gamma=0.1)
```

+   [在这里阅读更多关于调度器的信息。](https://discuss.pytorch.org/t/how-to-use-torch-optim-lr-scheduler-exponentiallr/12444)

**模型和检查点保存**

### 保存和加载模型检查点

专业提示：你知道可以在本地和Google Drive中保存和加载模型吗？这样你不必每次都从头开始。例如，如果你已经训练了5个周期，你可以保存权重，然后再训练另外5个周期。现在你总共训练了10个周期！非常方便。免费的GPU资源经常超时并被擦除。记住，增量训练是可能的。

你还可以保存一个检查点并在本地加载它。你可能会看到`.pt`和`.pth`扩展名。

```py
# write and then use your custom load_checkpoint function
model = load_checkpoint('checkpoint_resnet50.pth')
print(model)# use pytorch torch.load and load_state_dict(state_dict)
checkpt = torch.load(‘checkpoint_resnet50.pth’)
model.load_state_dict(checkpt)#save locally, map the new class_to_idx, move to cpu
#note down model architecturecheckpoint['class_to_idx']
model.class_to_idx = image_datasets['train'].class_to_idx
model.cpu()
torch.save({'arch': 'resnet18',
           'state_dict': model.state_dict(),
           'class_to_idx': model.class_to_idx},
           'classifier.pth')
```

**来自源3的代码片段**

```py
model.load_state_dict(torch.load('malaria_detection.pt'))
```

请注意，你必须将任何检查点保存到 Google Colab 的 Google Drive 中，否则你的数据可能每 12 小时或更早被删除。虽然 GPU 访问是免费的，但 Google Colab 上的存储是临时的。

### 使用 Pytorch 进行预测

这里是我们不喜欢 Pytorch 的地方。使用 Pytorch 进行预测似乎有点拼凑。尽管编写自己的训练循环看似容易自定义，但比 Tensorflow 更难编写，权衡一下舒适度和自定义性是值得的。然而，预测过程很奇怪，有些函数看起来像是被拼凑在一起的。看看下面的代码片段以了解我们的意思：

**来源 3 的代码片段：**

```py
def test(model, criterion, use_cuda):

   ... #omitted
        *# convert output probabilities to predicted class*
        pred = output.data.max(1, keepdim=True)[1]
        *# compare predictions to true label*
        correct += np.sum(np.squeeze(pred.eq(target.data.view_as(pred))).cpu().numpy())
        total += data.size(0)
    .... #omitteddef load_input_image(img_path):    
    image = Image.open(img_path)
    prediction_transform = transforms.Compose([transforms.Resize(size=(224, 224)),
                                     transforms.ToTensor(), 
                                     transforms.Normalize([0.485, 0.456, 0.406], 
                                                          [0.229, 0.224, 0.225])])

    *# discard the transparent, alpha channel (that's the :3) and add the batch dimension*
    image = prediction_transform(image)[:3,:,:].unsqueeze(0)
    return imagedef predict_malaria(model, class_names, img_path):
    *# load the image and return the predicted breed*
    img = load_input_image(img_path)
    model = model.cpu()
    model.eval()
    idx = torch.argmax(model(img))
    return class_names[idx]
```

`output.data.max`，`np.sum(np.squeeze())`，`cpu().numpy()`，`.unsqueeze(0)`，`torch.argmax` …… WTF？！这真的很让人沮丧。

重要的要点是：我们在这里处理的是转换为概率的 logits，还有需要转化为矩阵的张量和去除多余的括号。我们需要使用 `max` 或 `argmax` 来确定最可能的类别。我们需要将张量移回 CPU，因此使用 `cpu()`，并将张量转化为 ndarray 以便计算，使用 `numpy()`。

Pytorch 是一个深度学习框架，而深度学习常涉及矩阵运算，主要是调整维度，因此需要使用 squeeze 和 unsqueeze 来使维度匹配。

```py
>>> import torch
>>> import numpy as np
>>> test = torch.tensor([[1,2,3]])
>>> np.squeeze(test)
tensor([1, 2, 3])
>>> torch.tensor([1]).unsqueeze(0)
tensor([[1]])
```

这部分备忘单将为你节省很多麻烦，不客气！`np.squeeze()` 去除了 `[[1,2,3]]` 中额外的 `[]` 和多余的维度，原本的形状是 `(1, 3)`，现在变成了 `(3,)`。

而 `unsqueeze` 将 `[1]` 的形状 `(1,)` 变为 `[[1]]` 的形状 `(1,1)`。

### 深入阅读

+   [Pytorch 数据科学纳米学位深度学习简介及 Udacity 的 Pytorch 笔记和教程](https://github.com/udacity/DSND_Term1/tree/master/lessons/DeepLearning/new-intro-to-pytorch)。

+   [使用 Pytorch 进行迁移学习 — 我们超短而有效的文章](https://medium.com/data-science-bootcamp/transfer-learning-with-pytorch-code-snippet-load-a-pretrained-model-900374950004)

+   来源 1: [https://research.fb.com/downloads/pytorch/](https://research.fb.com/downloads/pytorch/)

+   来源 2: Pytorch Github 主页面 [https://github.com/pytorch/pytorch#getting-started](https://github.com/pytorch/pytorch#getting-started) 也提供了很好的备忘单。

+   极其棒的论坛，包括许多活跃的核心贡献者，补充了文档 [https://discuss.pytorch.org/](https://discuss.pytorch.org/)

+   来源 3: 使用 Pytorch 的疟疾检测 kaggle 内核 [https://www.kaggle.com/devilsknight/malaria-detection-with-pytorch](https://www.kaggle.com/devilsknight/malaria-detection-with-pytorch)

+   来源 4: [https://github.com/pytorch/examples/blob/97304e232807082c2e7b54c597615dc0ad8f6173/imagenet/main.py#L197-L198](https://github.com/pytorch/examples/blob/97304e232807082c2e7b54c597615dc0ad8f6173/imagenet/main.py#L197-L198)

+   来源 5: Pytorch 社区论坛 Pytorch 图像变换标准化 [https://discuss.pytorch.org/t/whats-the-range-of-the-input-value-desired-to-use-pretrained-resnet152-and-vgg19/1683](https://discuss.pytorch.org/t/whats-the-range-of-the-input-value-desired-to-use-pretrained-resnet152-and-vgg19/1683)

**[Uniqtech](https://theoptips.github.io/uniqtech_1/)**: 我们经常编写像这样的适合初学者的 Cheat Sheets。关注我们的个人资料以及我们最受欢迎的数据科学训练营出版物。查看我们关于 Pytorch 中迁移学习、Amazon SageMaker 上的 Pytorch 和数据科学的 Anaconda Cheat Sheet 的单页文章。我们主要在 Medium 上活跃，这是一个我们喜爱并有强烈认同感的社区。Medium 对其作者很好，并拥有一个极好的读者社区。如果您想了解我们即将推出的数据科学训练营课程（2024 年秋季发布）、高质量文章的奖学金，或者想为我们写作、提供反馈，请通过 [hi@uniqtech.co](mailto:hi@uniqtech.co) 发送电子邮件给我们。感谢 Medium 社区！

主要作者和贡献者 [Sun](https://medium.com/u/16c164f26caa?source=post_page---------------------------).

[原文](https://medium.com/@uniqtech/pytorch-cheat-sheet-for-beginners-and-udacity-deep-learning-nanodegree-5aadc827de82). 经许可转载。

**相关：**

+   [深度学习 Cheat Sheets](/2018/11/deep-learning-cheat-sheets.html)

+   [使用 PyTorch 框架进行 NLP 入门](/2019/04/nlp-pytorch.html)

+   [3 个更多的 Google Colab 环境管理技巧](/2019/01/more-google-colab-environment-management-tips.html)

### 更多主题

+   [Streamlit 机器学习 Cheat Sheet](https://www.kdnuggets.com/2023/01/streamlit-machine-learning-cheat-sheet.html)

+   [与 ChatGPT 结合的机器学习 Cheat Sheet](https://www.kdnuggets.com/2023/05/machine-learning-chatgpt-cheat-sheet.html)

+   [Scikit-learn 机器学习 Cheat Sheet](https://www.kdnuggets.com/2022/12/scikit-learn-machine-learning-cheatsheet.html)

+   [KDnuggets 2023 Cheat Sheet 集合](https://www.kdnuggets.com/the-kdnuggets-2023-cheat-sheet-collection)

+   [Python 数据清洗 Cheat Sheet](https://www.kdnuggets.com/2023/02/data-cleaning-python-cheat-sheet.html)

+   [ChatGPT Cheat Sheet](https://www.kdnuggets.com/2023/01/chatgpt-cheat-sheet.html)
