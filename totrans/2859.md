# 迁移学习变得简单：编码一种强大的技术

> 原文：[https://www.kdnuggets.com/2019/11/transfer-learning-coding.html](https://www.kdnuggets.com/2019/11/transfer-learning-coding.html)

[评论](#comments)

### 面向普通用户的人工智能

人工智能（A.I.）正在成为[*最强大且具有变革性的全球性技术*](https://www.strategyand.pwc.com/uk/en/transformative-power-ai/transformative-power-artificial-intelligence.html)，以前所未有的方式影响全球经济、医疗、金融、工业、社会文化互动等各个方面。这一点在迁移学习和机器学习能力的发展中尤为重要。

我们已经在日常生活中使用人工智能技术，无论我们是否意识到，它都在影响我们的生活和选择。从我们的Google搜索和导航、Netflix电影推荐、Amazon购买建议、日常任务的语音助手如Siri或Alexa、Facebook社区建设、医疗诊断、信用评分计算和按揭决策等，人工智能的应用只会不断增长。

大多数现代人工智能系统目前由一类算法或技术驱动，即[深度学习](https://machinelearningmastery.com/what-is-deep-learning/)，它基本上训练和构建[具有不同架构配置的深层神经网络](https://blog.exxactcorp.com/deep-learning-vs-machine-learning-vs-data-science-how-do-they-differ/)。

![](../Images/208231dc1b5052d184e745923aeea657.png)

*图片来源：Fjodor van Veen – asimovinstitute.org.*

经过50多年的起伏波动，深度学习革命已获得动力，看起来势不可挡——这得益于大数据技术、硬件创新和算法。因此，[深度学习网络有望影响并从根本上改变](https://elitedatascience.com/machine-learning-impact)我们人类在未来几十年的生活、工作和娱乐方式。

*所以，我们终于可以看到人工智能为全球每个人带来的希望！*

**然而……有一个陷阱。**

### 深度学习昂贵且专注范围狭窄

深度学习网络往往需要大量资源和计算成本。与传统的统计学习模型（如回归、决策树或支持向量机）不同，深度学习网络往往包含数百万个参数，因此需要大量的训练数据以避免过拟合。

因此，深度学习模型会使用**大量的高维原始数据**，如图像、非结构化文本或音频信号进行训练。此外，它们还反复进行**数百万次向量化计算**（例如矩阵乘法），以优化庞大的参数集以适应数据。而且，它们的构建涉及到一个[大量的超参数](https://towardsdatascience.com/hyperparameters-in-deep-learning-927f7b2084dd)（例如层数、每层的神经元数量、优化算法设置等），通常需要一个高水平研究团队花费数周或数月时间来创建一个最先进的模型。

*所有这些都对训练和优化特定任务的鲁棒且高性能的深度学习模型所需的计算能力提出了巨大的需求。*

假设我们可以在花费大量计算资源后训练出一个优秀的模型。**难道我们不希望将这个模型用于尽可能多的任务，并多次收获我们投资的回报吗？**

*这就是问题所在。*

到目前为止，深度学习算法**传统上被设计为孤立工作**。这些算法被训练来[解决特定任务](https://bdtechtalks.com/2018/04/23/strong-ai-vs-weak-ai-deep-learning/)。在大多数情况下，一旦特征空间分布发生变化，模型就必须**从头开始重建**。

但这没有意义，尤其是与我们人类目前利用有限计算速度的方式相比。

人类具有[跨任务转移知识的固有能力](https://www.nap.edu/read/9853/chapter/6#52)。我们在学习一个任务时获得的知识，可以以相同的方式来解决相关任务。如果任务或领域之间的相似性很高，我们能够更好地跨任务利用我们的“已学”知识。

*转移学习的理念是克服孤立学习范式，并利用在一个任务中获得的知识来解决相关任务，这在机器学习，特别是在深度学习领域中得到了应用。*

![](../Images/c6555dd2aeee750bd0f47985e3600e0d.png)

*图片来源：[转移学习综合实用指南：在深度学习中的实际应用](https://towardsdatascience.com/a-comprehensive-hands-on-guide-to-transfer-learning-with-real-world-applications-in-deep-learning-212bf3b2f27a)。*

### 深度学习网络的转移学习

在深度学习环境中，转移学习过程有许多策略可供选择，还有多个重要因素需要考虑和工程决策需要做出——数据集和领域的相似性、监督或非监督设置、需要多少重新训练等。

然而，简单来说，我们可以假设对于转移学习：

+   我们需要使用一个预训练的深度学习模型

+   重新使用全部或某些部分

+   将其应用于我们新感兴趣的领域中的特定机器学习问题——分类或回归。

通过这种方式，我们可以避免训练和优化大型深度学习模型所需的大量计算工作。

*最终，一个训练好的深度学习模型只是一组在特定数据结构格式下的数百万个实数，这些可以直接用于预测/推断，这是我们作为模型使用者真正感兴趣的任务。*

但请记住，预训练模型可能是针对特定分类进行训练的，即其输出向量和计算图只适用于预测某一特定任务。

因此，迁移学习中广泛使用的策略是：

+   加载预训练模型的权重矩阵，但不包括离输出层最近的最后几层的权重，

+   保持这些权重固定，即不可训练

+   附加适合当前任务的新层，并使用新数据训练模型

![](../Images/4513886ec76fb18b9b3ca3a77712aeaa.png)

*图：我们将在这里探索的深度学习网络迁移学习策略。*

这样，我们无需训练整个模型，就可以**将模型重新用于我们的特定机器学习任务**，同时利用从预训练、优化模型中加载的固定权重中包含的数据结构和模式。

### 一个你可以在笔记本电脑上运行的实用示例

让我们动手构建一个简单的代码演示，展示迁移学习的力量，好吗？

**为什么不遵循传统方法？**

传统上，这个主题的教程集中在从著名的高性能深度学习网络中学习，例如[VGGNet-16](https://neurohive.io/en/popular-networks/vgg16/)、[ResNet-50](https://medium.com/@14prakash/understanding-and-implementing-architectures-of-resnet-and-resnext-for-state-of-the-art-image-cf51669e1624) 或[Inception-V3/V4](https://towardsdatascience.com/a-simple-guide-to-the-versions-of-the-inception-network-7fc52b863202) 等。这些网络在庞大的[ImageNet数据库](http://www.image-net.org/)上进行了训练，并在[年度ImageNet比赛 - ILSVRC](http://www.image-net.org/challenges/LSVRC/)中获得了前列，从而成为图像分类任务的黄金基准模型。

然而，这些网络的问题在于它们包含大量复杂的层，当你开始学习深度学习概念时，不容易理解。

*因此，如果你想从零开始编写一个迁移学习的示例，从自学和建立信心的角度来看，先尝试一个独立的示例可能会更有益。你可以先训练一个深度学习模型，将其学习转移到另一个种子网络，然后展示在标准分类任务中的性能。*

**我们在这个教程中要做什么？**

在本文中，我们将使用 Python 和 Keras 包（TensorFlow 后端）在一个非常简单的环境中演示迁移学习概念。我们将使用著名的 CIFAR-10 数据集并进行以下操作：

+   通过在一组特征层之上堆叠一组分类层来创建一个 Keras 神经网络

+   在由前 5 类 (0…4) 的示例组成的部分 CIFAR-10 数据集上训练生成的网络。

+   冻结特征层并在其上堆叠一组新的全连接层，从而创建另一个卷积网络

+   在 CIFAR-10 的其余类别 (5..9) 的示例上训练这个新的卷积网络，只调整那些密集连接层的权重

整个代码是开源的，[可以在这里找到](https://github.com/tirthajyoti/Deep-learning-with-Python/blob/master/Notebooks/Transfer_learning_CIFAR.ipynb)。我们在本文中只展示了部分重要的代码。

**代码示例**

我们首先导入必要的库和函数：

1.  from time import time

1.  import keras

1.  from keras.datasets import mnist,cifar10

1.  from keras.models import Sequential

1.  from keras.layers import Dense, Dropout, Activation, Flatten

1.  from keras.layers import Conv2D, MaxPooling2D

1.  from keras.optimizers import Adam

1.  from keras import backend as K

1.  import matplotlib.pyplot as plt

1.  import random

接下来，我们决定一些深度学习模型的架构选择。我们将使用卷积神经网络 (CNN)，因为它最适合图像分类任务：

1.  # 要使用的卷积滤波器数量

1.  filters = 64

1.  # 最大池化的池化区域大小

1.  pool_size = 2

1.  # 卷积核大小

1.  kernel_size = 3

接下来，我们将数据集拆分为训练集和验证集，并创建两个数据集——一个包含标签低于 5 的类别，另一个包含 5 及以上的类别。为什么要这样做？

整个 CIFAR-10 数据集有 10 类图像，尺寸非常小。**我们将有两个神经网络**。一个将被预训练，学习将转移到第二个网络。但是，我们不会使用所有的图像类别来训练这两个网络。**第一个网络将仅使用前 5 类图像进行训练，这一学习将帮助第二个网络更快地学习最后 5 类图像**。

因此，

![](../Images/2d79361e3f786d6866899a439699885b.png)

*CIFAR-10 数据集中的 10 类图像。*

这里是前 5 类的一些随机图像，第一个神经网络将“看到”并进行训练。这些类别是——**飞机、汽车、鸟、猫**或**鹿**。

![](../Images/37af3bf1cfccd33cb5a0ec7f18282f57.png)

*图：前 5 类图像，仅被第一个神经网络看到。*

但我们实际上感兴趣的是为最后 5 类图像构建一个神经网络——**狗、青蛙、马、羊**或**卡车**。

![](../Images/8d2624dfcc6e5e50b7d33aa383dd7dcf.png)

*图：最后 5 类图像，仅被第二个神经网络看到。*

接下来，我们定义两组/类型的层：特征层（卷积）和分类层（密集）。

再次，请不要被这些代码片段的实现细节困扰。你可以从任何标准的Keras教程中了解细节。关键是理解概念。

特征层：

1.  特征层 = [

1.  **Conv2D**(filters, kernel_size,

1.  padding='valid',

1.  input_shape=input_shape),

1.  **Activation**('relu'),

1.  **Conv2D**(filters, kernel_size),

1.  **Activation**('relu'),

1.  **MaxPooling2D**(pool_size=pool_size),

1.  **Dropout**(25),

1.  **Flatten**(),

1.  ]

密集分类层：

1.  分类层 = [

1.  **Dense**(128),

1.  **Activation**('relu'),

1.  **Dropout**(25),

1.  **Dense**(num_classes),

1.  **Activation**('softmax')

1.  ]

接下来，我们通过将**特征层**和**分类层**堆叠在一起创建完整模型。

1.  1.  model_1 = **Sequential**(特征层 + 分类层)

然后我们定义一个用于训练模型的函数（未显示），并训练模型若干轮以达到足够好的性能：

1.  **train_model**(model_1,

1.  (x_train_lt5, y_train_lt5),

1.  (x_test_lt5, y_test_lt5), 类别数)

我们可以展示网络在训练轮次中的准确度演变：

![](../Images/e35416277e8b1316d8d807f8a6b4cb6d.png)

*图：训练第一个网络时验证集准确度随轮次变化。*

接下来，我们**冻结特征层并重建模型**。

冻结特征层是迁移学习的核心。这允许重新使用预训练模型进行分类任务，因为用户可以在预训练特征层上堆叠新的全连接层，从而获得良好的性能。

我们将创建一个名为**model_2**的新模型，具有**不可训练**的**特征层**和**可训练**的**分类层**。我们在下图中展示了摘要：

1.  对于特征层中的每一层：

1.  可训练 = False

1.  model_2 = **Sequential**(特征层 + 分类层)

![](../Images/0e2f59f900013b67c13f41b64fa377c1.png)

*图：第二个网络的模型摘要，显示了固定和可训练的权重。固定权重直接从第一个网络转移过来。*

现在我们训练第二个模型并观察**它总体上所需时间更少且性能相当或更高**。

1.  **train_model**(model_2,

1.  (x_train_gte5, y_train_gte5),(x_test_gte5, y_test_gte5), 类别数)

第二个模型的准确率甚至高于第一个模型，尽管这并不总是如此，且取决于模型架构和数据集。

![](../Images/f74b9408a1d84fe41383f4fb5a738b53.png)

*图：训练第二个网络时验证集准确度随轮次变化。*

两个模型的训练时间如下所示：

![](../Images/5dec718a5302933f8930708ff6d0b97d.png)

*图：两个网络的训练时间。*

### 我们达成了什么？

不仅**model_2**的训练速度比**model_1**更快，而且它还以更高的基准准确度开始，并在相同的训练轮次和相同的超参数（学习率、优化器、批量大小等）下达到了更好的最终准确度。而且，它在**model_1**未见过的图像上完成了训练。

这意味着虽然**model_1**是在**airplane、automobile、bird、cat**或**deer**的图像上训练的，但它学到的权重在转移到**model_2**时，帮助**model_2**在完全不同类别的图像分类中表现出色——**dog、frog、horse、sheep**或**truck**。

难道这不令人惊讶吗？现在，你可以用这么少的代码行来构建这种转移学习。再次强调，所有代码都是开源的，[可以在这里找到](https://github.com/tirthajyoti/Deep-learning-with-Python/blob/master/Notebooks/Transfer_learning_CIFAR.ipynb)。

[原文](https://blog.exxactcorp.com/transfer-learning-made-easy/)。经许可转载。

**相关：**

+   [利用迁移学习和弱监督廉价构建 NLP 分类器](https://www.kdnuggets.com/2019/03/building-nlp-classifiers-cheaply-transfer-learning-weak-supervision.html)

+   [利用迁移学习回收深度学习模型](https://www.kdnuggets.com/2015/08/recycling-deep-learning-representations-transfer-ml.html)

+   [NLP 中的迁移学习现状](https://www.kdnuggets.com/2019/09/state-transfer-learning-nlp.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 工作

* * *

### 更多相关话题

+   [TensorFlow 在计算机视觉中的应用 - 迁移学习简化版](https://www.kdnuggets.com/2022/01/tensorflow-computer-vision-transfer-learning-made-easy.html)

+   [提示工程中的并行处理：思维骨架…](https://www.kdnuggets.com/parallel-processing-in-prompt-engineering-the-skeleton-of-thought-technique)

+   [LLaMA 3：Meta 迄今为止最强大的开源模型](https://www.kdnuggets.com/llama-3-metas-most-powerful-open-source-model-yet)

+   [利用 BigQuery ML 简化数据分析师的机器学习](https://www.kdnuggets.com/machine-learning-made-simple-for-data-analysts-with-bigquery-ml)

+   [每个数据科学家至少犯过的一个错误](https://www.kdnuggets.com/2022/09/mistake-every-data-scientist-made-least.html)

+   [简化 Pandas DataFrames 的合并](https://www.kdnuggets.com/2022/09/combining-pandas-dataframes-made-simple.html)
