# 使用合成数据训练 AI 进行时间序列模式分类

> 原文：[https://www.kdnuggets.com/2021/10/teaching-ai-classify-time-series-patterns-synthetic-data.html](https://www.kdnuggets.com/2021/10/teaching-ai-classify-time-series-patterns-synthetic-data.html)

[评论](#comments)

## 我们想要实现什么目标？

我们想训练一个可以做类似事情的 AI 代理或模型，

![使用合成数据训练 AI 进行时间序列模式分类](../Images/09a0bbf71a73d15eca77d3c0da527966.png)

**图片来源**：作者使用[Pixabay 图片](https://pixabay.com/illustrations/alien-robot-android-antennae-blue-1905155/)（免费使用）准备的

## 方差、异常、变化

更具体地说，我们想训练一个 AI 代理（或模型）来识别/分类时间序列数据，

+   低/中/高方差

+   异常频率（*异常比例很小或很高*）

+   异常尺度（*异常是否远离正常值或接近正常值*）

+   时间序列数据中的正向或负向变化（在存在一些异常的情况下）

## 但我们不想复杂化问题

但是，我们不想进行大量的特征工程或学习复杂的时间序列算法（例如 ARIMA）和特性（例如季节性、平稳性）。

我们只想将我们的时间序列数据（带有正确标签）输入到某种监督‘学习’机器中，这样它就可以从原始数据中*学习这些类别*（高或低方差、异常太少或太多等）。

## 一个有用的 Python 库

为什么不利用一个 Python 库，它可以自动为我们完成这种分类工作，而我们只需将数据以标准的 Numpy/Pandas 格式输入其中？

如果这个库具有我们最喜欢的 Scikit-learn 包的外观和感觉，那就更好了！

我们在美丽的库中找到了这样的功能 —— [**tslearn**](https://tslearn.readthedocs.io/en/stable/index.html)。简单来说，它是一个提供时间序列分析的机器学习工具的 Python 包。该包构建在（因此依赖于）`scikit-learn`、`numpy` 和 `scipy` 库之上。

![](../Images/b38a62759752c6e95a2e978c6e14b4cb.png)

图片来源：[tslearn 文档](https://tslearn.readthedocs.io/en/stable/index.html)

## 为什么（以及如何）使用合成数据？

正如我在这篇文章中所写的 —— “*合成数据集是通过程序生成的数据仓库。因此，它不是通过任何现实生活中的调查或实验收集的。它的主要目的是灵活且足够丰富，以帮助机器学习从业者进行各种分类、回归和聚类算法的有趣实验*。”

[**合成数据生成——新数据科学家的必备技能**](https://towardsdatascience.com/synthetic-data-generation-a-must-have-skill-for-new-data-scientists-915896c0c1ae)

…然后扩展了本文中的论点——“*合成时间序列也不例外——它帮助数据科学家实验各种算法方法，并以仅用真实数据集无法实现的方式为实际部署做准备。*”

[**用Python创建具有异常签名的合成时间序列**](https://towardsdatascience.com/create-synthetic-time-series-with-anomaly-signatures-in-python-c0b80a6c093c)

基本上，我们想要**合成具有异常和其他模式的时间序列数据**，自动标记它们，并将它们输入到`tslearn`算法中，以便教会我们的AI代理这些模式。

特别是，如果我们想使用基于深度学习的分类器（如`tslearn`提供的），那么我们可能需要大量涵盖所有可能变异的数据，这在现实生活中可能不易获得。这时，合成数据就派上用场了。

那就开始冒险吧！

## 教授AI代理时间序列模式

演示笔记本可以在[**我的Github仓库中找到**](https://github.com/tirthajyoti/Synthetic-data-gen/blob/master/Notebooks/Anomaly-training-tslearn.ipynb)。将时间序列数据转换为适合`tslearn`模型训练的格式是相当简单的，已在笔记本中说明。在这里，我们主要关注不同类型的分类结果作为说明。

## 数据中的高或低方差？

我处理大量工业数据，即一组传感器从机器、工厂、操作员和业务流程中生成源源不断的数字数据。

检测时间序列数据流是否具有高或低方差对许多下游过程决策可能至关重要。因此，我们从这里开始。

流程很简单，

+   使用`SyntheticTS`模块生成合成数据（在我的文章中讨论过，可以在这里找到）

+   生成相应的类别标签以匹配这些Numpy/Pandas数据系列（注意**基于领域知识注入的自动标签生成**）

+   将合成序列数据转换为`tslearn`时间序列对象（数组）

+   将其存储在训练数据集中

+   将训练数据输入到适合的时间序列分类器中，我们选择了`TimeSeriesMLPClassifier`方法，该方法建立在熟悉的Scikit-learn多层感知机方法之上，基本上实现了**全连接深度学习网络**。

![](../Images/554f86dbdfcd97576b41b75895ed3e4d.png)

使用合成数据的训练流程，**来源**：完全由作者准备

`TimeSeriesMLPClassifier`具备标准Scikit-learn MLP分类器的所有功能，

+   隐藏层大小和神经元数量

+   激活函数

+   求解器/优化器（例如‘Adam’）

+   学习率

+   批量大小

+   容忍度

+   动量设置

+   早停准则

![](../Images/e34a112829b154665fdd83338d88fa35.png)

> 基本上，我们希望**合成具有异常和其他模式的时间序列数据**，自动标记它们，并将其输入`tslearn`算法，以便教我们的AI代理这些模式。

为了简洁起见，在笔记本中我没有显示训练/测试拆分，但这应该作为实际应用的数据科学工作流标准步骤。

我们可以在训练后绘制标准损失曲线，并进行各种超参数调整，以使性能达到最佳。

![](../Images/e9b9fd191f5200237f93a4559c46b4ec.png)

然而，展示深度学习分类器调整并不是本文的目标。我们更愿意关注最终的***结果，即它做出了什么样的分类决策***。

以下是一些随机测试结果。

![](../Images/18c8bee51733c8200e54b626cf8c14fc.png)

## 标签生成——手动还是自动？

本文的整个重点是展示可以通过合成数据避免手动标记。

我生成了数百个具有**随机方差或偏移**的合成时间序列来训练分类器。由于它们是程序生成的，因此也可以自动标记。

一旦你看到实际笔记本中的生成代码，这一点将会清楚。以下是方差训练的思路。

![](../Images/db52def0fe9ae4de102b47b85135ac99.png)

## 异常——数据的高比例还是低比例？

识别异常是不够的。在大多数现实情况中，你还需要识别它们的频率和发生模式。

这是因为工业数据分析系统通常负责在检测到数据流中的足够异常后生成警报。因此，为了决定是否发出警报，它们需要对异常计数是否代表正常数据的显著部分有一个清晰的认识。

你不想引发太多虚假警报，对吧？这对AI驱动系统的声誉不好。

所以，我们进行了同样的过程来训练一个AI模型，以处理时间序列数据中的异常比例。以下是随机测试结果，

![](../Images/932b3681b4556146a930cd4681073738.png)

## 异常——它们的幅度有多大？

在许多情况下，我们还希望将传入的数据分类为高/中/低异常幅度。对于工业数据分析，这个特征可能会指示机器或过程的状态异常。

我们遵循了上述相同的训练过程并获得了这些结果，

![](../Images/64dba33cf025f15193e48850717ee135.png)

## 数据漂移或偏移——在哪里，如何？

工业数据分析中的另一个经典操作是检测来自机器的传感器数据中的漂移/偏移。这可能有多种原因，

+   机器可能会老化，

+   过程配方/设置突然改变而没有适当的记录，

+   小的子组件可能会随着时间的推移而退化。

底线是，AI驱动的系统应该能够识别这些类别——至少在正向或负向转移及其发生点方面，即*漂移是否在过程生命周期中早期或晚期开始*。

> 识别异常是不够的。在大多数实际情况中，你还需要识别其频率和出现模式。这是因为工业数据分析系统通常负责在检测到数据流中的足够异常后生成警报。

在这种情况下，我们将转移的位置（在整个时间段中的早期或晚期）也纳入了考虑。因此，我们有以下类别用于训练数据，

+   早期正向转移

+   晚期正向转移

+   早期负向转移

+   晚期负向转移

**由于这种复杂性增加，我们需要生成比之前实验更多的合成数据**。以下是结果，

![](../Images/c877eb1b4d9a62e96d312bf6c24d0cdb.png)

## 摘要

时间序列分类是许多精彩应用场景中一个非常有趣的话题。在本文中，我们展示了如何**使用合成数据来训练AI模型**（具有几个全连接层的深度学习网络）用于模拟工业过程或传感器流的单维**时间序列数据**。

特别是，我们专注于**教会AI模型各种异常特征和数据漂移模式**，因为这些分类是机器降解的重要指标。简而言之，它们构成了所谓的[**预测分析**](https://www.industr.com/en/how-convergence-of-data-analytics-iot-drives-industry-2551064)在**工业4.0**或**智能制造**领域的基础。

我们希望未来AI驱动的预测分析中对合成数据的使用会显著增长。

你可以查看作者的[**GitHub**](https://github.com/tirthajyoti?tab=repositories)** 代码库**，获取有关机器学习和数据科学的代码、想法和资源。如果你像我一样，对AI/机器学习/数据科学充满热情，请随时[在LinkedIn上添加我](https://www.linkedin.com/in/tirthajyoti-sarkar-2127aa7/)或[关注我在Twitter上的动态](https://twitter.com/tirthajyotiS)。

**简介：[Tirthajyoti Sarkar](https://www.linkedin.com/in/tirthajyoti-sarkar-2127aa7/)**是Adapdix Corp.的数据科学/机器学习经理。他定期为KDnuggets和TDS等出版物贡献关于数据科学和机器学习的多样话题。他撰写了数据科学书籍，并参与开源软件的开发。Tirthajyoti拥有电气工程博士学位，并在攻读计算数据分析硕士学位。可以通过tirthajyoti at gmail[dot]com联系他。

[原文](https://towardsdatascience.com/teaching-ai-to-classify-time-series-patterns-with-synthetic-data-555d8e75ee8a)。经许可转载。

**相关：**

+   [GPU驱动的数据科学（非深度学习）与RAPIDS](/2021/08/gpu-powered-data-science-deep-learning-rapids.html)

+   [为什么以及如何学习“高效数据科学”？](/2021/07/learn-productive-data-science.html)

+   [Python中的蒙特卡洛积分](/2020/12/monte-carlo-integration-python.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的捷径

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

### 更多相关话题

+   [停止学习数据科学以寻找目的，并通过寻找目的来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的5个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [每个数据科学家都应该知道的三个R语言库（即使你使用Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [一个90亿美元的人工智能失败，深入分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [是什么让Python成为初创企业理想的编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)
