# 超越独热编码：类别变量的探索

> 原文：[https://www.kdnuggets.com/2015/12/beyond-one-hot-exploration-categorical-variables.html](https://www.kdnuggets.com/2015/12/beyond-one-hot-exploration-categorical-variables.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者：威尔·麦金尼斯**。

在机器学习中，数据至关重要。用于根据数据进行预测的算法和模型很重要，也非常有趣，但机器学习仍然受到“垃圾进，垃圾出”这一理念的制约。考虑到这一点，让我们来看一下这些输入数据的一个小子集：类别变量。

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯的捷径

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

![mushrooms-cars](../Images/56c0fdcb94cc15a810d93943d1ca52f4.png)

类别变量 ([wiki](https://en.wikipedia.org/wiki/Categorical_variable)) 是那些表示固定数量可能值的变量，而不是连续数量的变量。每个值将测量分配到这些有限组或类别中的一个。这些变量与有序变量不同，有序变量之间的距离应相等，无论类别数量如何，而有序变量则具有某种固有的排序。例如：

1.  有序变量：低、中、高

1.  类别变量：乔治亚州、阿拉巴马州、南卡罗来纳州……，纽约

我们稍后使用的机器学习算法倾向于需要数字而不是字符串作为输入，因此我们需要某种编码方法来进行转换。

插一句话：在这篇文章中还有一个概念会经常出现，那就是维度的概念。简单来说，就是数据集中列的数量，但它对最终模型有着重要的影响。在极端情况下，“[维度灾难](https://en.wikipedia.org/wiki/Curse_of_dimensionality)”的概念讨论了在高维空间中某些事物可能会停止正常工作。即使在相对低维的问题中，具有更多维度的数据集也需要更多的参数来使模型理解，这意味着需要更多的行来可靠地学习这些参数。如果数据集中的行数是固定的，而添加额外的维度却没有为模型提供更多的信息，这可能会对最终模型的准确性产生不利影响。

回到眼前的问题：我们想将分类变量编码为数字，但我们担心这个维度问题。显而易见的答案是直接为每个类别分配一个整数（假设我们事先知道所有可能的类别）。这称为序数编码。它不会给问题增加任何维度，但暗示了一个可能并不存在的变量顺序。

#### 方法论

为了了解效果如何，我编写了一个简单的Python脚本来测试常见数据集上的不同编码方法。首先是过程概述：

+   我们收集一个包含分类变量的分类问题的数据集

+   我们使用某种编码方法将X数据集转换为数值

+   我们使用scikit-learn的交叉验证分数和BernoulliNB()分类器来生成数据集的分数。这在每个数据集上重复10次，使用所有分数的均值。

+   我们存储数据集的维度、均值分数以及编码数据和生成分数的时间。

这在来自UCI数据集库的几个不同数据集上重复进行：

+   [汽车评估](https://archive.ics.uci.edu/ml/datasets/Car+Evaluation)

+   [蘑菇](https://archive.ics.uci.edu/ml/datasets/Mushroom)

+   [剪接位点](http://archive.ics.uci.edu/ml/machine-learning-databases/molecular-biology/splice-junction-gene-sequences/)

我尝试了7种不同的编码方法（4-7的描述摘自[statsmodel的文档](http://statsmodels.sourceforge.net/devel/contrasts.html)）：

1.  序数：如上所述

1.  独热编码：每个类别一个列，每个单元格中填入1或0，表示该行是否包含该列的类别

1.  二进制：首先将类别编码为序数，然后将这些整数转换为二进制代码，然后将该二进制字符串的数字分成单独的列。这种编码以比独热编码更少的维度编码数据，但会有一些距离扭曲。

1.  总和：比较给定水平的因变量均值与所有水平因变量的总体均值。也就是说，它使用每一个前k-1水平和水平k之间的对比。在这个例子中，水平1与所有其他水平进行比较，水平2与所有其他水平进行比较，水平3与所有其他水平进行比较。

1.  多项式：对于k=4水平的多项式编码，系数包括线性、二次和三次趋势。这里的分类变量被假设为由一个基础的、等间距的数值变量表示。因此，这种编码仅用于具有相等间距的有序分类变量。

1.  后向差分：将一个水平的因变量均值与前一个水平的因变量均值进行比较。这种编码可能对名义变量或序数变量有用。

1.  Helmert：对一个水平的因变量均值与所有前一个水平的因变量均值进行比较。因此，‘逆’这个名字有时用来与前向 Helmert 编码区分。

### 结果

#### 蘑菇

|  | 编码 | 维度 | 平均得分 | 耗时 |
| --- | --- | --- | --- | --- |
| 0 | 序数 | 22 | 0.81 | 3.65 |
| 1 | 独热编码 | 117 | 0.81 | 8.19 |
| 6 | Helmert 编码 | 117 | 0.84 | 5.43 |
| 5 | 后向差分编码 | 117 | 0.85 | 7.83 |
| 3 | 求和编码 | 117 | 0.85 | 4.93 |
| 4 | 多项式编码 | 117 | 0.86 | 6.14 |
| 2 | 二进制编码 | 43 | 0.87 | 3.95 |

#### 汽车

|  | 编码 | 维度 | 平均得分 | 耗时 |
| --- | --- | --- | --- | --- |
| 10 | 求和编码 | 21 | 0.55 | 1.46 |
| 13 | Helmert 编码 | 21 | 0.58 | 1.46 |
| 7 | 序数 | 6 | 0.64 | 1.47 |
| 8 | 独热编码 | 21 | 0.65 | 1.39 |
| 11 | 多项式编码 | 21 | 0.67 | 1.49 |
| 12 | 后向差分编码 | 21 | 0.70 | 1.50 |
| 9 | 二进制编码 | 9 | 0.70 | 1.44 |

#### 拼接

|  | 编码 | 维度 | 平均得分 | 耗时 |
| --- | --- | --- | --- | --- |
| 14 | 序数 | 61 | 0.68 | 5.11 |
| 17 | 求和编码 | 3465 | 0.92 | 25.90 |
| 16 | 二进制编码 | 134 | 0.94 | 3.35 |
| 15 | 独热编码 | 3465 | 0.95 | 2.56 |

### 结论

这绝不是一项详尽的研究，但似乎二进制编码在保持合理一致性的情况下表现良好，没有显著的维度增加。正如预期，序数编码表现一直很差。

如果你想查看源代码、添加或建议新的数据集或编码方法，我已经将所有内容（包括数据集）上传到 GitHub：[https://github.com/wdm0006/categorical_encoding](https://github.com/wdm0006/categorical_encoding)。

可以直接在那儿贡献，或在这里评论提出建议。

[原始](http://willmcginnis.com/2015/11/29/beyond-one-hot-an-exploration-of-categorical-variables/).

**相关：**

+   [数据挖掘 Medicare 数据 – 我们能发现什么？](/2014/04/data-mining-medicare-data.html)

+   [机器学习的 5 个部落 – 问题与答案](/2015/11/domingos-5-tribes-machine-learning-questions-answers.html)

+   [欺诈检测解决方案](/solutions/fraud-detection.html)

### 更多相关话题

+   [如何处理机器学习中的分类数据](https://www.kdnuggets.com/2021/05/deal-with-categorical-data-machine-learning.html)

+   [使用 MultiLabelBinarizer 编码分类特征](https://www.kdnuggets.com/2023/01/encoding-categorical-features-multilabelbinarizer.html)

+   [构建视觉搜索引擎 - 第 1 部分：数据探索](https://www.kdnuggets.com/2022/02/building-visual-search-engine-part-1.html)

+   [ChatGPT 驱动的数据探索：解锁数据集中的隐藏洞察](https://www.kdnuggets.com/2023/07/chatgptpowered-data-exploration-unlock-hidden-insights-dataset.html)

+   [超越编码：为何人性化触感很重要](https://www.kdnuggets.com/beyond-coding-why-the-human-touch-matters)

+   [超越管道：图作为 Scikit-Learn 元估计器](https://www.kdnuggets.com/2022/09/graphs-scikitlearn-metaestimators.html)
