# 使用Python + NumPy进行的图像分类深度残差网络

> 原文：[https://www.kdnuggets.com/2016/07/deep-residual-neworks-image-classification-python-numpy.html](https://www.kdnuggets.com/2016/07/deep-residual-neworks-image-classification-python-numpy.html)

**作者：Daniele Ciriello，独立机器学习研究员**

### 概述

我希望用Python从头实现 [“深度残差学习用于图像识别”](https://arxiv.org/abs/1512.03385) 以用于 [我的计算机工程硕士论文](http://www.slideshare.net/DanieleCiriello1/reti-neurali-di-convoluzione-per-la-visione-artificiale)，最后我实现了一个 [简单（仅限CPU）的深度学习框架](https://github.com/dnlcrl/PyFunt/) 以及 [残差模型](https://github.com/dnlcrl/deep-residual-networks-pyfunt)，并在 [CIFAR-10](https://www.cs.toronto.edu/~kriz/cifar.html)、 [MNIST](http://yann.lecun.com/exdb/mnist/) 和 [SFDDD](https://www.kaggle.com/c/state-farm-distracted-driver-detection)上进行了训练。 [结果](https://github.com/dnlcrl/deep-residual-networks-pyfunt/tree/master/docs)不言自明。

### 计算机视觉中的卷积神经网络

在6月13日星期一，我获得了计算机工程硕士学位，并展示了一篇关于计算机视觉中深度卷积神经网络的论文。目前只有意大利语版，我正在进行英文翻译，但不知道是否以及何时能完成，所以我尝试简要描述每一章。

文档组成如下：

+   **介绍**

    主题介绍、论文结构描述以及从感知机到NeoCognitron的神经网络历史的快速描述。

+   **神经网络基础**

    深度学习背后的基本数学概念描述。

+   **前沿技术**

    描述了过去十年中实现目标的主要概念，引入了图像分类和对象定位问题，ILSVRC以及从2012到2015年在这两项任务中获得最佳结果的模型。

+   **实现深度学习框架**

    本章包含了如何实现残差模型中每一层的前向和后向步骤的解释、残差模型的实现以及在训练前测试网络的一些方法。

+   **实验结果**

    在开发了模型和训练求解器之后，我在CIFAR-10上进行了几个实验。在本章中，我展示了如何测试模型以及当去除残差路径、应用数据增强函数以减少过拟合或增加层数时，网络的行为如何变化，然后我展示了如何使用随机生成的图像或数据集中的图像来欺骗训练好的网络。

+   **结论**

    在这里，我描述了在 MNIST 和 SFDDD 上训练相同模型所获得的其他结果（更多信息请查看下面），项目概述以及可能的未来工作。

论文链接：

+   [意大利语](http://www.slideshare.net/DanieleCiriello1/reti-neurali-di-convoluzione-per-la-visione-artificiale)

+   英语（进行中）

演示链接：

+   [幻灯片 + 记录，意大利语](https://www.slideshare.net/DanieleCiriello1/pres-tesi-lm2016transcriptita)

+   [幻灯片 + 记录，英语](https://www.slideshare.net/DanieleCiriello1/pres-tesi-lm2016transcripteng)

以下是我简要描述了如何获得所有这些内容，我使用的来源，训练的残差模型结构以及获得的结果。请记住，我的首要目标是开发和训练模型，因此我没有花太多时间在框架的设计方面，但我正在继续努力（欢迎提交拉取请求）！

### PyFunt、PyDatSet 和 深度残差网络

[Pyfunt](https://github.com/dnlcrl/PyFunt) 是一个简单的 Python 风格的命令式深度学习框架：它主要提供了大多数知名神经层的前向和反向步骤的实现，一些有用的初始化函数和一个求解器，它本质上是一个类，你实例化它，并将要训练的模型和使用 [pydatset](https://github.com/dnlcrl/PyDatSet) 加载的数据传递给它，该数据集包含导入一些数据集的函数和一组人工增强训练集的函数。为了澄清，PyFunt 和 PyDatSet 是这些库的名称，pyfunt 和 pydatset 是包的名称（因此你可以用`from pydatset import ...`导入它们）。

残差模型的实现位于 [deep-residual-networks-pyfunt](https://github.com/dnlcrl/deep-residual-networks-pyfunt)，其中也包含了 `train.py` 文件。

参考论文中提出的残差模型源于 VGG 模型，其中 3x3 的卷积滤波器应用于通道数保持不变的情况下步长为 1，如果特征数量翻倍则步长为 2（这样做是为了在每个卷积层中保持计算复杂度）。因此，残差模型由多个残差块（或残差层）级联组成，这些块是串联的卷积层组，其中最后一层的输出加到块的原始输入上，作者建议每个残差块中使用几层卷积层会效果很好。

每个残差块的组成是，如果应用了降维（使用 2 的卷积步骤而不是 1），则必须对输入应用下采样和零填充，然后才能进行加法，以允许两个 ndarray（skip_path + conv_out）的求和。

一个参数化的残差网络总共有（6*n）+2 层，组成如下（右边的值表示类似 CIFAR 图像的 [3,32,32] 样本的尺寸）。

你可以在下面看到一个包图，展示了`train.py`如何使用其他组件来训练残差模型。

![包图示](../Images/730871585cbfb796dd62009c17a98089.png)

在拥有所有数据后，我开始实验当你移除残差路径时会发生什么，当你应用或不应用数据增强函数时会有什么变化，增加层数或每层滤波器数量时会有什么变化。下面你可以看到一些结果的图像，但我建议你查看[各个JuPyter笔记本](https://github.com/dnlcrl/deep-residual-networks-pyfunt/tree/master/docs)（以及上述链接的论文和演示文稿），以获得更深入的理解，因为你可以在下面显示的所有数据集上找到更详尽的结果描述。

### 结果

我在[CIFAR-10](https://www.cs.toronto.edu/~kriz/cifar.html)、[MNIST](http://yann.lecun.com/exdb/mnist/)和[SFDDD](https://www.kaggle.com/c/state-farm-distracted-driver-detection)上训练了残差模型，结果非常令人兴奋，至少对我来说如此。网络在几乎所有测试中表现良好，显然我的限制在于桌面PC的容量。

#### CIFAR-10

![CIFAR-10](../Images/e6598865c0a7ddb5bc8fa5f36fba7a8c.png)

在CIFAR-10上的一个实验中，训练了一个简单的20层resnet，通过应用数据增强的正则化函数，我得到了与参考论文中显示的类似的结果，如下所示。

![CIFAR-10的结果](../Images/a4e47363095822dd08d07d3b91ea6ef4.png)

![来自MSRA的CIFAR-10结果](../Images/ef984d86fec0245bee9dc4ce4340095a.png)

这个模型的训练大约花费了10小时。更多信息可以在[这个jupyter ipython notebook](https://github.com/dnlcrl/deep-residual-networks-pyfunt/blob/master/docs/CIFAR-10%20Experiments.ipynb)中找到，位于代码库的文档文件夹中。

#### MNIST

![MNIST](../Images/8d32dd9236d6f830da9ad628386182de.png)

与CIFAR-10相比，MNIST是一个更简单的数据集，因此训练时间相对较短，我还尝试使用每个卷积层的一半滤波器数量。

![MNIST结果](../Images/e0a37e9d58924e90a8a706d73cbe92fc.png)

关于MNIST上残差网络的更多实验信息可以在[这里](https://github.com/dnlcrl/deep-residual-networks-pyfunt/blob/master/docs/MNIST%20Experiments.ipynb)找到。

![MNIST最佳模型的错误分类](../Images/e02bb43ee49a269ebe34cf06dde3a77e.png)

在上面的图像中，你可以看到32层网络错误分类的所有验证样本，训练仅进行了30个epoch(!)。左上角是实际类别，左下角是网络的错误分类，右下角是第二次分类的置信度。

#### SFDDD

![SFDDD](../Images/62b2f902e28116382a959b85f2e9582c.png)

State Farm Distracted Driver Detection 是来自State Farm的数据集，托管在 [kaggle.com](https://kaggle.com/)，包含了640x480的驾驶员图片，分为10个分心类别。对于这个数据集，我决定将所有图片调整为64x48，并使用32x32的随机裁剪进行训练，使用中心32x32裁剪进行测试。我也尝试了将所有图片直接缩放到32x32，但结果更差（确认了缩放图片并不大有助于卷积网络学习更通用的特征）。

下面你可以看到两个分别具有32层和44层的模型的学习曲线，看来两个模型在80个epoch后都产生了较低的误差，但问题在于我用于验证集的是从训练集中随机提取的2k张图片，因此我的验证集的相关因子高于State Farm提出的原始训练集和验证集之间的相关性（我在其上获得的误差约为3%）。

![SFDDD](../Images/a00598f164027ed60aec7c1f638da7c1.png)

下面你可以看到六张图片的显著性图，类别为“用右手打电话”，其中较亮的区域表示对网络正确分类贡献最大的图像部分。

![SFDDD](../Images/511f026c6dbc0fc5e84f78812dc9caa3.png)

其他信息将在比赛结束后 [这里](https://github.com/dnlcrl/deep-residual-networks-pyfunt/tree/master/docs) 提供。

### 结束语

我希望我的项目能够帮助你学到新东西。如果没有，也许 *你* 可以教我一些新东西，评论和拉取请求始终欢迎！

### 来源

当我开始考虑实现 [“深度残差网络用于图像识别”](https://arxiv.org/abs/1512.03385)时，GitHub上只有 [this project](https://github.com/gcr/torch-residual-networks) 来自 [gcr](http://sneakygcr.net/)，基于Lua + Torch，这段代码在我实现残差模型时确实帮助了我很多。

[神经网络与深度学习](http://neuralnetworksanddeeplearning.com/) 由 [Michael Nielsen](http://michaelnielsen.org/) 编写，包含了对该主题非常全面且组织良好的介绍，以及大量代码以帮助用户理解过程中的各个部分。

[colah.github.io](http://colah.github.io/archive.html) 由 [Christopher Olah](http://colah.github.io/about.html) 编写，包含了大量关于深度学习和神经网络的优质文章，例如我发现 [this post about convolution layers](http://colah.github.io/posts/2014-07-Conv-Nets-Modular/) 非常有启发性。

[斯坦福大学的CS231N](http://cs231n.github.io/) 由 [Andrej Karpathy](https://twitter.com/karpathy) 等人编写，是一门关于视觉识别CNN的非常有趣的课程，我主要使用了课程材料和作业解决方案来构建[PyFunt](https://github.com/dnlcrl/PyFunt)。

[Arxiv](https://arxiv.org/)，一个提供数学、物理、天文学、计算机科学、定量生物学、统计学和定量金融领域科学论文的电子版库，可以在线访问。也可以查看[Arxiv Sanity Preserver](http://www.arxiv-sanity.com/)由[Karpathy](https://twitter.com/karpathy)提供。

许多其他精彩的资源列在这里：[https://github.com/ChristosChristofidis/awesome-deep-learning](https://dnlcrl.github.io/projects/2016/06/22/awesome-deep-learning)。

当我开始学习深度学习时，我跟踪了最佳论文，并在[this google sheet](https://docs.google.com/spreadsheets/d/1DBFylWzALpMpZLLrukHGt29L1tDUsKq4A11sxIbB-Js/edit?usp=sharing)中收集了标题、作者、年份和链接，我会定期更新。

**简历： [Daniele Ciriello](https://twitter.com/dnlcrl)** 拥有计算机工程硕士学位，是深度学习爱好者，并热爱Python和开源。

[原文](https://dnlcrl.github.io/projects/2016/06/22/Deep-Residual-Networks-for-Image-Classification-with-Python+NumPy.html)。已获许可转载。

**相关：**

+   [科学Python简介（以及一些相关数学） – NumPy](/2016/06/intro-scientific-python-numpy.html)

+   [深入了解卷积神经网络](/2016/06/peeking-inside-convolutional-neural-networks.html)

+   [学习编写神经网络代码](/2016/01/learning-to-code-neural-networks.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织IT

* * *

### 更多相关内容

+   [使用卷积神经网络（CNNs）进行图像分类](https://www.kdnuggets.com/2022/05/image-classification-convolutional-neural-networks-cnns.html)

+   [使用Tensorflow训练图像分类模型指南](https://www.kdnuggets.com/2022/12/guide-train-image-classification-model-tensorflow.html)

+   [多标签分类：使用Python的Scikit-Learn简介](https://www.kdnuggets.com/2023/08/multilabel-classification-introduction-python-scikitlearn.html)

+   [用于图像处理的NumPy](https://www.kdnuggets.com/numpy-for-image-processing)

+   [分类问题的更多性能评估指标](https://www.kdnuggets.com/2020/04/performance-evaluation-metrics-classification.html)

+   [使用HuggingFace对BERT进行微调以进行推文分类](https://www.kdnuggets.com/2022/01/finetuning-bert-tweets-classification-ft-hugging-face.html)
