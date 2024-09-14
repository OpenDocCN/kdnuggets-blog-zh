# 什么是支持向量机，为什么我要使用它？

> 原文：[`www.kdnuggets.com/2017/02/yhat-support-vector-machine.html`](https://www.kdnuggets.com/2017/02/yhat-support-vector-machine.html)

本文最初发表于[Yhat 博客](http://blog.yhat.com/)。[**Yhat**](https://www.yhat.com/)是一家总部位于布鲁克林的公司，旨在使数据科学适用于开发者、数据科学家和企业。Yhat 提供一个软件平台，用于以 REST API 的形式部署和管理预测算法，同时消除了与生产环境相关的测试、版本控制、扩展和安全等痛苦的工程障碍。

* * *

### 什么是 SVM？

SVM 是一种监督式机器学习算法，可用于分类或回归问题。它使用一种称为核技巧的技术来转换数据，然后基于这些转换找到可能输出之间的最优边界。简单来说，它进行一些极其复杂的数据转换，然后根据你定义的标签或输出来找出如何分隔数据。

### 那么，它的优点是什么？

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 领域

* * *

SVM 能够进行分类和回归。在这篇文章中，我将重点讨论使用 SVM 进行分类。特别是，我将专注于非线性 SVM，即使用非线性核的 SVM。非线性 SVM 意味着算法计算出的边界不一定是一条直线。其优点是可以捕捉到数据点之间更复杂的关系，而不必自己进行困难的转换。缺点是训练时间更长，因为计算量更大。

### 牛和狼

那么，什么是核技巧？

核技巧将你提供的数据进行变换。输入一些你认为会成为优秀分类器的特征，输出的数据会变得难以识别。这就像解开一段 DNA 链。你从一个看似无害的数据向量开始，经过核技巧处理后，它被解开并复合，直到变成一个更大的数据集，通过查看电子表格无法理解。但这里的魔力在于，通过扩展数据集，你的类别之间的边界变得更加明显，SVM 算法能够计算出一个更为优化的超平面。

设想一下，你是一个农民，你有一个问题——你需要建立一个围栏来保护你的奶牛免受狼群的袭击。但是你该在哪里建围栏呢？如果你是一个**非常**数据驱动的农民，一种方法是基于奶牛和狼在牧场中的位置来建立一个分类器。通过测试几种不同类型的分类器，我们发现 SVM 在将奶牛与狼群分开方面表现出色。我觉得这些图表也很好地说明了使用非线性分类器的好处。你可以看到逻辑回归和决策树模型都仅使用了直线。

![](http://blog.yhat.com/static/img/cows_and_wolves.png)

### 想要重新创建分析吗？

想自己创建这些图表？你可以在终端或你选择的 IDE 中运行代码，但，惊喜的是，我推荐[Rodeo](https://www.yhat.com/products/rodeo)。它有一个很棒的弹出图表功能，适合这种分析类型。它还带有 Python，适用于 Windows 机器。此外，多亏了[TakenPilot](https://github.com/TakenPilot)的辛勤工作，它现在运行得非常迅速。

一旦你[下载了 Rodeo](https://www.yhat.com/products/rodeo)，你需要从我的 GitHub 上保存原始的[cows_and_wolves.txt](https://gist.githubusercontent.com/glamp/4365660/raw/474658c2c5a8bb763c50278c810d45a27bd21c7e/cows_and_wolves.txt)文件。确保你已经将工作目录设置为保存文件的位置。

![Rodeo](img/7e3d6ff60c708c9efbbce4383671e3d8.png)

好了，现在只需将下面的代码复制粘贴到 Rodeo 中，并运行，无论是逐行还是整个脚本。别忘了，你可以弹出你的绘图标签，移动窗口或调整大小。

### 让 SVM 来完成艰巨的工作

如果依赖变量和独立变量之间的关系是非线性的，它的准确性不会像 SVM 那样高。变量之间的转换（log(x)，(x²)）变得不那么重要，因为这些会在算法中被考虑。如果你仍然难以理解，可以尝试跟随这个例子。

假设我们有一个包含绿色和红色点的数据集。当用其坐标绘制时，这些点形成一个红色圆圈，绿色轮廓（看起来非常像孟加拉国的国旗）。

如果我们丢失了三分之一的数据，会发生什么呢？如果我们无法恢复这些数据，并且想要找到一种方法来近似缺失的那三分之一的数据会是什么样子呢？

那么我们如何确定缺失的三分之一是什么样的呢？一种方法可能是使用我们拥有的 80% 数据作为训练集来构建模型。但我们应该使用什么类型的模型呢？让我们尝试一下以下几种：

+   逻辑回归模型

+   决策树

+   SVM

我训练了每个模型，然后使用它们对我们缺失的三分之一数据进行预测。让我们来看看我们预测的形状是什么样的……

![](http://blog.yhat.com/static/img/flag.png)

### 跟随

这是用来比较你的逻辑回归模型、决策树和 SVM 的代码。

![Rodeo](img/85682926351dd57b4d477d4bd6a497de.png)

通过复制并运行上面的代码，跟随 [Rodeo](https://github.com/yhat/rodeo)！

### 结果

从图中可以看出，SVM 显然是赢家。但为什么呢？如果你查看决策树和 GLM 模型的预测形状，你会注意到什么？直线边界。我们的输入模型没有包含任何变换来考虑 x、y 和颜色之间的非线性关系。给定一组特定的变换，我们肯定可以让 GLM 和 DT 表现得更好，但为什么要浪费时间呢？没有复杂的变换或缩放，SVM 仅错误分类了 117/5000 个点（98% 的准确率，而 DT 为 51% 和 GLM 为 12%！所有这些错误分类的点都是红色的——因此略微膨胀。

### 何时不使用

那么为什么不把 SVM 用于所有情况呢？不幸的是，SVM 的魔力也是最大的缺点。复杂的数据变换和结果边界平面非常难以解释。这就是为什么它通常被称为黑箱。相反，GLM 和决策树正好相反。它们的操作非常容易理解，尽管性能有所牺牲。

### 更多资源

想了解更多关于 SVM 的信息吗？这里有一些我遇到的很好的资源：

+   初学者 [SVM 教程](http://web.mit.edu/zoya/www/SVM.pdf)：仅仅是基础知识，还有一点点 MIT 的 Zoya Gavrilov 提供的辅助。

+   初学者 SVM 算法的工作原理：[视频](https://youtu.be/1NxnPkZM9bc) 由 Thales Sehn Körting 提供

+   中级 [生物医学中支持向量机的温和介绍](https://www.med.nyu.edu/chibi/sites/default/files/chibi/Final.pdf) 由 NYU 和 Vanderbilt 提供的幻灯片

+   高级 [模式识别中的支持向量机教程](http://research.microsoft.com/en-us/um/people/cburges/papers/SVMTutorial.pdf) 由 Bell Labs 的 Christopher Burges 提供

[原文](http://blog.yhat.com/posts/why-support-vector-machine.html)。经许可转载。

**相关内容：**

+   支持向量机：简明技术概述

+   支持向量机：简单解释

+   Python 中的随机森林

### 更多相关主题

+   [支持向量机：直观方法](https://www.kdnuggets.com/2022/08/support-vector-machines-intuitive-approach.html)

+   [支持向量机的温和介绍](https://www.kdnuggets.com/2023/07/gentle-introduction-support-vector-machines.html)

+   [语义向量搜索如何改变客户支持互动](https://www.kdnuggets.com/how-semantic-vector-search-transforms-customer-support-interactions)

+   [Python 向量数据库和向量索引：构建 LLM 应用程序](https://www.kdnuggets.com/2023/08/python-vector-databases-vector-indexes-architecting-llm-apps.html)

+   [如果我要重新开始学习数据科学，我会怎么做？](https://www.kdnuggets.com/2020/08/start-learning-data-science-again.html)

+   [何时使用集成技术是一个好的选择？](https://www.kdnuggets.com/2022/07/would-ensemble-techniques-good-choice.html)
