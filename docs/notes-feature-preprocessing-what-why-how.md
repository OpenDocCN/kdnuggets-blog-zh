# 特征预处理的笔记：什么，为什么，怎么做

> 原文：[https://www.kdnuggets.com/2018/10/notes-feature-preprocessing-what-why-how.html](https://www.kdnuggets.com/2018/10/notes-feature-preprocessing-what-why-how.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)![图片](../Images/936e98582a437454bc4a6a7480f46cbf.png)

原始数据与移动数据与移动&缩放数据（来源：[人工智能GitBook](https://leonardoaraujosantos.gitbooks.io/artificial-inteligence/content/feature_scaling.html)）

本文涵盖了一些与数值数据预处理相关的重要点，重点讨论特征值的缩放和处理异常值的广泛问题。

### **特征缩放的影响**

[特征缩放](https://en.wikipedia.org/wiki/Feature_scaling)可以定义为“用来标准化独立变量或数据特征范围的方法。” 特征缩放是数据预处理的主要组成部分之一，可以应用于所有类型的数据。

一些模型受特征缩放的影响，而另一些则不受影响。非树基模型容易受特征缩放的影响：

+   线性模型

+   最近邻分类器

+   神经网络

相反，树基模型不受特征缩放的影响。这应该很明显，但树基建模算法的例子包括：

+   普通决策树

+   随机森林

+   梯度提升树

**为什么会这样？**

想象一下，我们将特定特征值的尺度乘以100。如果我们考虑沿着与该特征对应的轴的数据点定位，这将对如最近邻分类器等模型产生明显影响。例如，线性模型、最近邻和神经网络都对数据点值之间的相对位置敏感；实际上，这就是K最近邻（kNN）的明确前提。

由于树基模型在特征数据轴上寻找最佳划分位置，这种相对位置的重要性要小得多。如果确定的划分位置在点a和b之间，那么如果这两个点都被某个常数乘或除，这一点将保持不变。

![图片](../Images/666fca2f624b4f62bddf531313ba86a3.png)

左侧的算法，如kNN，生成的模型易受特征缩放的影响；右侧的树基模型则不受特征缩放的影响

特征缩放的一个潜在用途是测试特征的重要性。例如，对于 kNN 来说，给定特征的值越大，它对模型的影响越大。我们可以通过有意放大一个或多个我们认为更重要的特征的尺度，来利用这一事实，观察这些更大值的轻微变化是否会影响结果模型。相反，将特征的数据点值乘以零会导致沿该轴的数据点堆叠，使该特征在建模过程中无用。

这涵盖了线性模型，但神经网络呢？事实证明，神经网络也受到尺度的重大影响：[正则化](https://en.wikipedia.org/wiki/Regularization_(mathematics))的影响与特征尺度成正比。如果没有适当的缩放，梯度下降方法可能会遇到问题。

最后，提供一个最简单的特征缩放重要性示例：考虑 k-means 聚类。如果你的数据有4列，其中3列的值在0和1之间缩放，而第四列的值在0和1,000之间，那么相对容易确定这些特征中的哪一个将是聚类结果的唯一决定因素。

### **特征缩放方法**

**最小/最大缩放**

![公式](../Images/8ec67a92d352f40501d4e8c926490369.png)

+   这种方法将值缩放在0和1之间 ???? [0, 1]

+   将最小值设置为零，最大值设置为一，所有其他值相应地缩放在两者之间

+   Scikit-learn [`sklearn.preprocessing.MinMaxScaler`](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html) 模块就是这样一个实现

```py

from sklearn.preprocessing import MinMaxScaler

# import some data

sc = MinMaxScaler()
data = sc.fit_transform(data))

```

**标准缩放**

![公式](../Images/20e63d6c5f250e7fd176da67e0a378ba.png)

+   这种方法通过将数据的均值设置为0，将标准差设置为1来进行缩放

+   从特征值中减去均值，并除以标准差

+   这导致了标准化分布

+   Scikit-learn [`sklearn.preprocessing.StandardScaler`](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html) 模块就是这样一个实现

```py

from sklearn.preprocessing import StandardScaler

# import some data

sc = StandardScaler()
sc.fit_transform(data))

```

这些（及其他）缩放方法在实践中的区别是什么？这取决于应用的具体数据集，但实际上结果通常是*大致*相似的。

### 处理异常值

人们经常强烈反对“去除异常值”的讨论。现实情况是，异常值可能会不利地扭曲模型。真正的问题不是去除异常值本身，而是你需要理解异常值与数据的关系，并做出是否应去除它们以及原因的知情判断。去除异常值的替代方法是使用某种形式的缩放来管理它们。

离群值不仅对特征可能是个问题，对目标值也可能如此。例如，如果所有目标值都在0到1之间，而引入了一个目标值为100,000的新数据点，那么生成的回归模型会开始对模型中的其他数据点进行不真实的高预测。

请记住，离群值通常代表NaNs，特别是当它们超出上下界且紧密聚集时（例如，所有NaNs可能已经转换为-999）。这种类型的离群值会人为地扭曲分布，因此绝对需要一些处理方法。领域知识和数据检查是决定如何处理离群值的最有用工具。

**裁剪**

修正线性模型中的离群值的一种方法是将值保持在上下界之间。我们如何选择这些界限？一种合理的方法是使用百分位数——仅保持在第一个和第99个百分位数之间的值。这种裁剪形式被称为[温莎化](https://en.wikipedia.org/wiki/Winsorizing)。

有关温莎化的更多信息，请参考[`scipy.stats.mstats.winsorize`](https://docs.scipy.org/doc/scipy-0.13.0/reference/generated/scipy.stats.mstats.winsorize.html)。

```py

import numpy as np
from scipy.stats.mstats import winsorize

# import some data

data = winsorize(data, limits = 0.01)

```

**排名变换**

排名变换去除特征值之间的相对距离，并用表示特征值排名的一致间隔替代（例如：第一、第二、第三...）。这将离群值移动到其他特征值附近（在一个规律的间隔内），并且可以作为线性模型、kNN和神经网络的有效方法。

有关排名变换的更多信息，请参考[`scipy.stats.rankdata`](https://docs.scipy.org/doc/scipy-0.16.0/reference/generated/scipy.stats.rankdata.html)。

```py

from scipy.stats import rankdata
rankdata([0, 2, 3, 2])

# Output:
# array([ 1\. ,  2.5,  4\. ,  2.5])

```

**日志变换**

对于非树模型，日志变换可能非常有用。它们可以将较大的特征值从极端值驱离，靠近特征均值。它还有使接近零的值更易区分的效果。

有关实施这些变换的更多信息，请参见[`numpy.log`](https://docs.scipy.org/doc/numpy-1.15.0/reference/generated/numpy.log.html)和[`numpy.sqrt`](https://docs.scipy.org/doc/numpy-1.15.0/reference/generated/numpy.sqrt.html)。

将各种预处理方法结合到一个模型中可能会很有用，通过使用不同的方法预处理数据集实例的百分比，然后将结果合并到一个训练集中。或者，通过集成在不同预处理数据上训练的模型（与前述方法描述比较）也可能是有益的。

预处理不是“万能”的情况，你愿意根据可靠的数学原理尝试不同的方法，可能会增加模型的稳健性。

**参考文献：**

1.  [如何赢得数据科学竞赛：向顶尖 Kagglers 学习](https://www.coursera.org/learn/competitive-data-science)，国家研究大学高等经济学院（Coursera）

**相关：**

+   [掌握数据准备的 7 个步骤，使用 Python](/2017/06/7-steps-mastering-data-preparation-python.html)

+   [文本数据预处理：Python 实践指南](/2018/03/text-data-preprocessing-walkthrough-python.html)

+   [数据准备技巧、窍门和工具：与内部人士的访谈](/2016/10/data-preparation-tips-tricks-tools.html)

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 管理

* * *

### 更多相关内容

+   [SQL 专业笔记：免费电子书评审](https://www.kdnuggets.com/2022/05/sql-notes-professionals-free-ebook-review.html)

+   [KDnuggets 新闻，5 月 11 日：SQL 专业笔记；如何...](https://www.kdnuggets.com/2022/n19.html)

+   [特征存储峰会 2022：关于特征工程的免费会议](https://www.kdnuggets.com/2022/10/hopsworks-feature-store-summit-2022-free-conference-feature-engineering.html)

+   [掌握数据清理和预处理技巧的 7 个步骤](https://www.kdnuggets.com/2023/08/7-steps-mastering-data-cleaning-preprocessing-techniques.html)

+   [利用 ChatGPT 实现自动化数据清理和预处理](https://www.kdnuggets.com/2023/08/harnessing-chatgpt-automated-data-cleaning-preprocessing.html)

+   [在 Pandas 中清理和预处理文本数据用于 NLP 任务](https://www.kdnuggets.com/cleaning-and-preprocessing-text-data-in-pandas-for-nlp-tasks)
