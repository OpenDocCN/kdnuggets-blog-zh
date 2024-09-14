# 增强科学家系列 第一部分：机器学习在 SEM 图像分类中的实际应用

> 原文：[`www.kdnuggets.com/2020/03/the-augmented-scientist-practical-application-machine-learning-classification-images.html`](https://www.kdnuggets.com/2020/03/the-augmented-scientist-practical-application-machine-learning-classification-images.html)

评论

**作者 [Iain Keaney](https://www.linkedin.com/in/iain-keaney-9a668b47), [Skellig.ai](https://www.skellig.ai/)**

![增强科学图示](img/475e10bf83a0678190e37f8e537318e8.png)

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您组织中的 IT 工作

* * *

欢迎来到我们增强科学家系列的第一篇博客，在这里我们将探讨机器学习如何改善科学家日常工作的方式。随着这一系列的展开，我们将深入挖掘更多机器学习的应用，以作为科学研究的辅助工具，去除大量重复的分析工作，使科学家能更深入地探索他们的领域。我和我的妻子都来自科学背景，她是化学专业的，我是物理专业的。当我开始从事机器学习时，我被它加速处理繁琐分析工作的巨大潜力所打动，这些工作在研究实验室中消耗了大量时间。

作为我们对增强科学家概念的第一次探索，我们的目标是看看我们能否建立一个分类器，识别[扫描电子显微镜](https://en.wikipedia.org/wiki/Scanning_electron_microscope)（SEM）图像中的模式，并将我们分类器的性能与当前的最先进技术进行比较。后续文章将探讨 SEM 图像分类的更集中应用。

### 什么是 SEM？

我们将首先关注 SEM 图像分析，这是我妻子作为电化学家在职业生涯中广泛使用的技术。SEM 广泛用于化学和生物学中，以纳米尺度创建表面图像。它通过用聚焦的电子束扫描表面来工作。电子从表面反射，形成表面的形貌和成分图像。

![图](img/e60e3debf9b711715a308547a15e58aa.png)

来源：[`en.wikipedia.org/wiki/Scanning_electron_microscope`](https://en.wikipedia.org/wiki/Scanning_electron_microscope)

这种详细成像有广泛的应用，每种应用有不同的分析需求，比如优化生物传感器中的电极表面，或在燃料电池中产生电力。表面的结构可以告诉我们很多关于物体的属性和特征。

### 之前的工作：

在这篇博客文章中，我们将查看是否能在[Modarres et al 2017](https://www.nature.com/articles/s41598-017-13565-z#Sec2)的工作基础上取得任何进展，他们之前研究了分类 18,577 张 SEM 图像的模式（你可以在[这里](https://b2share.eudat.eu/records/19cc2afd23e34b92b36a1dfd0113a89f)找到数据集）。这些示例展示了 Modarres et al 2017 数据集中分类的模式类型，包括 Tips、Particles、Patterned Surfaces、MEMS devices 和 electrodes、Nanowires、Porous Sponge、Biological、Powder、Films Coated Surfaces 和 Fibres。

![图示](img/285f799e82ab7ee3482f1e3cfab59984.png)

来源：[`www.nature.com/articles/s41598-017-13565-z#Sec2`](https://www.nature.com/articles/s41598-017-13565-z#Sec2)

下表显示了各类别中图像的分布。

![图示](img/fba5b29ddf02cdb4cd1a3450f3698937.png)

来源：[`www.nature.com/articles/s41598-017-13565-z#Sec2`](https://www.nature.com/articles/s41598-017-13565-z#Sec2)

使用预训练的 Inception-v3 模型，该模型在 Tensorflow 中实现，Modarres 等人达到了约 90%的准确率，约 80%的精确率和约 90%的召回率@1。Modarres 等人还报告称，数据集中的不平衡对分类器在准确预测欠代表类别方面没有影响。他们的混淆矩阵（见下图）显示，人口最少的类别，如多孔海绵、涂层表面和纤维，都表现得相当好。Modarres 等人将此归因于这些类别的独特模式。（注意：Modarres 等人混淆矩阵中展示的值是每个实际/真实标签的验证样本的百分比）

![图示](img/ada5a524eed11450706d76ea8152fb76.png)

来源：[`www.nature.com/articles/s41598-017-13565-z#Sec2`](https://www.nature.com/articles/s41598-017-13565-z#Sec2)

在这一点上需要记住两件事：Modarres 等人没有使用任何数据增强技术，而 Inception-v3 实现的训练过程在 2 个 GPU 上花费了大约 7 分钟。

### 我们的方法：

想要看看是否能比 Modarres 等人取得更高的准确率，我们在进行了一些自定义修改后重新进行了他们的研究。首先，我们使用了 Fastai v1 框架上的 Resnet 50 实现，并在 Colab GPU 上执行。为了避免过拟合，我们采用了 Fast.ai 介绍的两种方法：数据增强和渐进式调整大小。

### 数据增强：

数据增强本质上是改变/扭曲每张图像，有效地创建一张新图像。Fastai v1 有一个很好的工具叫做 get_transforms，处理这个过程。函数[get_transforms](https://docs.fast.ai/vision.transform.html)以多种不同的方式转换图像，例如翻转、旋转、改变对比度和扭曲，这意味着小数据集实际上变得更大。

![图像](img/2d2d112b9a2c478b16cd2b7efcbba602.png)

资料来源：[`docs.fast.ai/vision.transform.html`](https://docs.fast.ai/vision.transform.html)

### 进阶调整大小：

进阶调整大小是[Jeremy Howard](https://twitter.com/jeremyphoward)在[Fast.ai](https://www.fast.ai/)提出的一种方法。这个概念是将数据集中的图像裁剪成更小的尺寸，然后在裁剪图像上训练模型，接着增大图像尺寸并再次训练模型。我们重复这个过程多次，每次增加数据集中图像的尺寸。在这种情况下，我们从 64x64 像素的图像开始。我们在这个数据集上训练了 Resnet 50，总共 15 个 epoch，然后将其逐步增大到 128，最后到 224。进阶调整大小有两个优点。首先，很多早期训练是在较小的图像上进行的，这意味着计算成本较低，训练时间更短。其次，反复更改图像的大小并重新训练模型可以使模型过拟合变得非常困难。这意味着你的最终模型在以前从未见过的新数据上会更稳定。

### 结果：

我们的实现可以在我们的 GitHub 仓库中找到，[这里](https://github.com/skellig-ai/ikeaney.github.io/blob/master/SEM_Classification_ResNet50.ipynb)。我们实现了 94.5%的总体准确率，比 Modarres 等人之前的最新技术提高了超过 4.5%，尽管我们的训练过程确实花费了近 400 分钟。考虑到数据集的不平衡，考虑其他指标如[精确度和召回率](https://developers.google.com/machine-learning/crash-course/classification/precision-and-recall)是有用的。精确度本质上是正识别的比例实际上是正确的，而召回率是实际正样本中被正确识别的比例。尽管我们的模型稍有过拟合，但实现了 94.2%的精确度和 91.8%的召回率。与 Modarres 等人报告的约 80%和 90%相比，这表明我们的模型在数据集中表现出更好的表现。

### 结论：

希望你喜欢我们增强科学家系列的第一篇博客文章。这个系列将探讨更多机器学习可以用于协助科学家在各个学科中的工作的方法。如果你有兴趣参与，或有使用机器学习增强自己工作的例子，请告诉我们，我们很乐意听到你的意见。

**简介：[Iain Keaney](https://www.linkedin.com/in/iain-keaney-9a668b47)** （[**@Iain_Keaney**](https://twitter.com/Iain_Keaney)）是 [Skellig.ai](https://www.skellig.ai/) 的机器学习工程师和项目协调员。你可以在这里找到[Skellig.ai 博客](https://skellig-ai.github.io/)。

[原文](https://skellig-ai.github.io/2020/02/03/The-Augmented-Scientist-Part-1.html)。经许可转载。

**相关：**

+   使用更少数据进行图像分类的深度学习

+   部署机器学习模型是什么意思？

+   使用 Keras 简化图像分类的单一函数

### 更多相关内容

+   [认识 Gorilla：加州大学伯克利分校和微软的 API 增强型 LLM…](https://www.kdnuggets.com/2023/06/meet-gorilla-uc-berkeley-microsoft-apiaugmented-llm-outperforms-gpt4-chatgpt-claude.html)

+   [检索增强生成：信息检索与文本生成的交汇点](https://www.kdnuggets.com/retrieval-augmented-generation-where-information-retrieval-meets-text-generation)

+   [数据科学的基本数学：特征向量及其在 PCA 中的应用](https://www.kdnuggets.com/2022/06/essential-math-data-science-eigenvectors-application-pca.html)

+   [创建一个从音频中提取主题的 Python 网络应用](https://www.kdnuggets.com/2023/01/creating-web-application-extract-topics-audio-python.html)

+   [使用 Google Earth 引擎在 Python 中构建地理空间应用](https://www.kdnuggets.com/2022/03/building-geospatial-application-python-google-earth-engine-greppo.html)

+   [本周提升搜索应用的 8 种方法](https://www.kdnuggets.com/2022/09/corise-8-ways-improve-search-application-week.html)
