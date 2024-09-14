# 新手机器学习工程师常犯的6个错误

> 原文：[https://www.kdnuggets.com/2017/10/top-errors-novice-machine-learning-engineers.html](https://www.kdnuggets.com/2017/10/top-errors-novice-machine-learning-engineers.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png)[评论](#comments)

**作者 [Christopher Dossman](https://twitter.com/cdossman?lang=en)。**

![](../Images/b72bec5ce6dddc1087da37d651697dc2.png)

在机器学习中，有很多种方法来构建产品或解决方案，每种方法都有不同的假设。很多时候，如何导航和识别哪些假设是合理的并不明显。新手在机器学习中会犯错，事后往往会觉得很傻。我列出了新手机器学习工程师常犯的顶级错误。希望你能从这些常见错误中学习，并创建出更有价值的解决方案。

### 认为默认损失函数理所当然

均方误差非常好！它确实是一个很棒的起始默认值，但在实际应用中，这种现成的损失函数很少是你试图解决的业务问题的最佳选择。

以欺诈检测为例。为了与业务目标一致，你真正想要的是根据因欺诈而损失的金额来惩罚假阴性。使用均方误差可能会得到不错的结果，但永远无法获得最先进的结果。

[**成为机器学习工程师 | 第3步：选择你的工具**](https://medium.com/towards-data-science/becoming-a-machine-learning-engineer-step-3-pick-your-tool-da1903a2958f) 查看这篇文章，了解你可以使用的不同机器学习工具。

**总结：** 始终构建一个与解决方案目标紧密匹配的自定义损失函数。

### 使用一种算法/方法解决所有问题

许多人完成了第一个教程后，会立即开始在他们能想到的每一个用例中使用他们学到的相同算法。它很熟悉，他们认为它会像其他算法一样有效。这是一个错误的假设，会导致较差的结果。

让你的数据为你选择模型。完成数据预处理后，将其输入到多种不同的模型中，并查看结果。你将对哪些模型效果最佳以及哪些模型效果不佳有一个很好的了解。

[**成为机器学习工程师 | 第2步：选择一个过程**](https://medium.com/towards-data-science/becoming-a-machine-learning-engineer-step-2-pick-a-process-942eef6ba8dd) 查看这篇文章，掌握你的过程。

**总结：** 如果你发现自己一次又一次地使用相同的算法，这可能意味着你没有获得最佳结果。

### 忽略异常值

异常值可以是重要的，也可以完全被忽略，这取决于上下文。例如，在污染预测中，空气污染的大幅波动可能会发生，查看这些波动并理解其发生原因是个好主意。如果异常值是由某种传感器错误引起的，忽略它们并将其从数据中删除是安全的。

从模型的角度来看，有些模型对异常值的敏感度高于其他模型。例如，Adaboost 将这些异常值视为“困难”案例，并对异常值施加巨大权重，而决策树可能仅简单地将每个异常值计为一个错误分类。

[**成为机器学习工程师 | 第 2 步：选择一个过程**](https://medium.com/towards-data-science/becoming-a-machine-learning-engineer-step-2-pick-a-process-942eef6ba8dd) 介绍了可以用来避免这种错误的最佳实践。

**要点：** 在开始工作之前，务必仔细查看你的数据，并确定异常值是否应该被忽略或更仔细地审视。

### 未正确处理周期性特征

一天的小时、星期中的天、年份中的月份以及风向都是周期性特征的例子。许多新的机器学习工程师没有想到将这些特征转换为可以保留信息的表示方式，比如小时23和小时0彼此接近而不是远离。

以小时为例，处理这种情况的最佳方法是计算正弦和余弦分量，从而将你的周期性特征表示为圆的 (x, y) 坐标。在这种表示中，小时23和小时0在数值上是紧挨在一起的，就像它们应该的那样。

**许多人要求提供代码示例：** [**在这里**](https://gist.github.com/anonymous/7ce6274c630dabd70960c6d7fdd6c580)

**要点：** 如果你有周期性特征而没有对其进行转换，你就会给你的模型提供无用的数据。

### L1/L2 正则化没有标准化

L1 和 L2 正则化会惩罚大系数，这是正则化线性回归或逻辑回归的常用方法；然而，许多机器学习工程师未意识到在应用正则化之前标准化特征的重要性。

想象一下你有一个线性回归模型，其中交易额是一个特征。对所有特征进行标准化，使它们处于相同的水平，这样正则化对所有特征的影响是相同的。不要让某些特征以分为单位，而其他特征以美元为单位。

**要点：** 正则化非常好，但如果你没有标准化特征，可能会引发麻烦。

### 将线性回归或逻辑回归的系数解释为特征重要性

线性回归模型通常会返回每个系数的 p 值。这些系数经常使新手机器学习工程师认为，对于线性模型，系数值越大，特征越重要。实际上这几乎从未成立，因为变量的规模会改变系数的绝对值。如果特征之间具有共线性，系数可能会从一个特征转移到另一个特征。数据集的特征越多，特征之间的共线性可能性越大，简单的特征重要性解释就越不可靠。

**收获：** 理解哪些特征对结果至关重要很重要，但不要认为你可以仅通过系数来判断。它们通常不能讲述完整的故事。

完成一些项目并获得良好结果可能会让你感觉像赢得了一百万美元。你努力工作，并且有结果证明你做得很好，但就像任何其他行业一样，细节决定成败，甚至华丽的图表也可能隐藏偏差和错误。这份清单并不是详尽无遗的，而只是为了让读者思考可能隐藏在解决方案中的所有小问题。为了取得良好结果，遵循你的流程并始终仔细检查是否有常见错误是非常重要的。

如果你发现这篇文章有用，你将从我的 [**成为机器学习工程师 | 第2步：选择流程**](https://medium.com/towards-data-science/becoming-a-machine-learning-engineer-step-2-pick-a-process-942eef6ba8dd) 文章中受益匪浅。它帮助你制定一个流程，使你能更好地发现简单错误，取得更好的结果。

[原文](https://medium.com/towards-data-science/top-6-errors-novice-machine-learning-engineers-make-e82273d394db)。已获得许可转载。

**简介：[Chris Dossman](https://twitter.com/cdossman?lang=en)** 是一位机器学习研究员和未来的太空矿工。

**相关内容：**

+   [导致糟糕数据可视化的5个常见错误](/2017/10/5-common-mistakes-bad-data-visualization.html)

+   [数据科学家在与业务人员打交道时常犯的错误](/2017/04/top-mistakes-data-scientists-make-business.html)

+   [使用 Python 通过标准差移除异常值](/2017/02/removing-outliers-standard-deviation-python.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

### 更多相关主题

+   [I型和II型错误：有什么区别？](https://www.kdnuggets.com/2022/08/type-type-ii-errors-difference.html)

+   [从新手到高手：为什么你的Python技能在数据科学中至关重要](https://www.kdnuggets.com/novice-to-ninja-why-your-python-skills-matter-in-data-science)

+   [避免这些每个AI新手都会犯的5个常见错误](https://www.kdnuggets.com/avoid-these-5-common-mistakes-every-novice-in-ai-makes)

+   [我们不需要数据科学家，我们需要数据工程师](https://www.kdnuggets.com/2021/02/dont-need-data-scientists-need-data-engineers.html)

+   [如何获得ML工作：来自Meta、Google Brain和SAP工程师的建议](https://www.kdnuggets.com/2022/08/corise-land-ml-job-advice-engineers-meta-google-brain-sap.html)

+   [关于数据工程师的11个问题：这个职业是什么，等等…](https://www.kdnuggets.com/2022/10/11-questions-data-engineers-profession-heading.html)
