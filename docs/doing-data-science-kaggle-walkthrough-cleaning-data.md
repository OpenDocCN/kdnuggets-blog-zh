# 数据科学实战：Kaggle 数据清理教程

> 原文：[https://www.kdnuggets.com/2016/03/doing-data-science-kaggle-walkthrough-cleaning-data.html](https://www.kdnuggets.com/2016/03/doing-data-science-kaggle-walkthrough-cleaning-data.html)

**作者：Brett Romero，Open Data Kosovo**。

*这篇关于数据清理的文章是一个系列中的第三部分，系列通过Kaggle竞赛探讨数据科学和机器学习。如果你还没有阅读，建议你回去阅读[第一部分](http://brettromero.com/wordpress/data-science-a-kaggle-walkthrough-introduction/)和[第二部分](http://brettromero.com/wordpress/data-science-a-kaggle-walkthrough-understanding-the-data/)*。

本部分将重点关注清理Airbnb Kaggle竞赛中提供的数据。

![脏数据](../Images/9cccd97cf0197adc18e3a221a9c4dbde.png)

### 数据清理

当我们谈论数据清理时，我们究竟在谈论什么？通常，当人们谈论数据清理时，他们指的是一些具体的内容：

1.  **修复格式** – 数据在从一种格式保存或转换到另一种格式时（例如，从CSV转换到Python），某些数据可能无法正确转换。在上一篇文章中，我们看到一个很好的例子。在csv中，timestamp_first_active列包含像20090609231247这样的数字，而不是预期格式的时间戳：2009-06-09 23:12:47。清理数据的一个典型工作就是修正这些类型的问题。

1.  **填补缺失值** – 正如我们在第二部分中看到的那样，数据集中某些值缺失是相当常见的。这通常意味着某些信息没有被收集。处理缺失数据有几种选项，下面会详细介绍。

1.  **修正错误值** – 对于某些列，有些值明显不正确。例如，“性别”列中可能输入了一个数字，或“年龄”列中可能输入了一个远超100的值。这些值需要被修正（如果可以确定正确值的话）或假设为缺失值。

1.  **标准化类别** – 这是“修正错误值”的一个子类别，这种数据清理非常常见，值得特别提及。在许多（或所有？）情况下，当数据直接从用户收集时，尤其是使用自由文本字段时，拼写错误、语言差异或其他因素会导致相同答案以多种方式提供。例如，在收集出生国家的数据时，如果用户没有提供标准化的国家列表，数据中不可避免地会包含同一国家的多种拼写（例如：USA、United States、U.S. 等）。主要的清理任务之一通常是将这些值标准化，确保每个值只有一个版本。

### 处理缺失数据的选项

缺失数据通常是数据清理过程中较棘手的问题之一。大致上，有两种解决方案：

**1\. 删除/忽略缺失值的行**

面对缺失值时，最简单的解决方案是训练模型时不使用包含缺失值的记录。然而，在开始删除大量行之前，有一些问题需要注意。

首先，这种方法只有在缺失数据的行数相对于数据集来说比较少时才有意义。如果你发现由于缺失值导致你需要删除的数据量超过了数据集的10%左右，你可能需要重新考虑。

第二个问题是，为了删除包含缺失数据的行，你必须确认你删除的行不包含其他行中没有的信息。例如，在当前的Airbnb数据集中，我们发现许多用户没有提供他们的年龄。我们能否假设选择不提供年龄的用户与提供年龄的用户是相同的？还是他们可能代表了一种不同类型的用户，可能是更年长且更注重隐私的用户，因此可能在选择访问国家时会做出不同的决策？如果答案是后者，我们可能不希望仅仅删除这些记录。

**2\. 填补缺失值**

处理缺失数据的第二种广泛选项是用一个值填补缺失值。但应该使用什么值呢？这取决于一系列因素，包括你尝试填补的数据类型。

如果数据是分类数据（即国家、设备类型等），可能有意义的是创建一个新类别来表示‘未知’。另一种选择可能是用该列中最常见的值（众数）填补缺失值。然而，由于这些是填补缺失值的广泛方法，这可能会简化你的数据和/或使最终模型的准确性降低。

对于数值类型的数据（例如年龄列），还有其他选项。鉴于在这种情况下使用众数填补值意义不大，我们可以改为使用均值或中位数。我们甚至可以基于其他标准计算平均值——例如，根据选择了相同`country_destination`的用户的平均年龄来填补缺失的年龄值。

对于两种类型的数据（分类数据和数值数据），我们也可以使用更复杂的方法来填补缺失值。实际上，我们可以使用类似于预测`country_destination`的方法来预测其他列中的值，这些方法基于已经有数据的列。就像在建模过程中一样，这种方法几乎有无尽的方式实现，这里不会详细描述。有关此主题的更多信息，[orange Python库](http://docs.orange.biolab.si/reference/rst/Orange.feature.imputation.html)提供了一些出色的文档。

* * *

## 我们的前三课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 加速你的网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

### 更多相关主题

+   [分类指标入门：逻辑回归与…](https://www.kdnuggets.com/2022/10/classification-metrics-walkthrough-logistic-regression-accuracy-precision-recall-roc.html)

+   [集成学习技术：Python中的随机森林入门](https://www.kdnuggets.com/ensemble-learning-techniques-a-walkthrough-with-random-forests-in-python)

+   [7个免费的Kaggle微课程，适合数据科学初学者](https://www.kdnuggets.com/7-free-kaggle-micro-courses-for-data-science-beginners)

+   [2024年成为数据科学家的前10个Kaggle机器学习项目](https://www.kdnuggets.com/top-10-kaggle-machine-learning-projects-to-become-data-scientist-in-2024)

+   [在Kaggle竞争的前4个技巧以及你为何应当开始](https://www.kdnuggets.com/2022/05/packt-top-4-tricks-competing-kaggle-start.html)

+   [最全面的Kaggle解决方案和创意列表](https://www.kdnuggets.com/2022/11/comprehensive-list-kaggle-solutions-ideas.html)
�复杂的插补方法。然而，我测试的这些方法都没有比上述方法产生更好的最终结果。

**识别并填充其他缺失值列**

从对数据的更详细分析中，你可能还会发现有一列缺失值——`first_affiliate_tracked` 列。按照我们之前在其他列中填充缺失值的方法，现在我们也填充这一列的值。

```py
# Fill first_affiliate_tracked column
print("Filling first_affiliate_tracked column...")
df_all['first_affiliate_tracked'].fillna(-1, inplace=True)

```

**这就全部了吗？**

对数据处理更有经验的人可能会觉得我们对这些数据的清理做得不够——你是对的。Kaggle 比赛的一个好处是提供的数据不需要过多的清理，因为数据提供者并不希望参与者将重点放在此上。许多在现实世界数据中可能会发现的问题（如前所述）在这个数据集中并不存在，节省了我们大量时间。

然而，这种相对简单的清理过程也告诉我们，即使数据集提供的初衷是无需或仅需最小清理，仍然总会有一些事情需要处理。

### 下一次

在下一部分中，我们将重点关注数据转换和特征提取，从而创建一个训练数据集，期望能使模型做出更好的预测。为了确保你不会错过，使用下面的订阅功能。

[1] 对于那些有更多数据挖掘经验的人，你可能会意识到此阶段将测试数据和训练数据合并并不是最佳实践。最佳实践是避免在任何数据预处理或模型调整/验证步骤中使用测试数据集，以避免过拟合。然而，在这次比赛的背景下，由于我们只是试图创建一个分类单一不变数据集的模型，因此最大化该数据集模型的准确性是主要关注点。

**简介： [布雷特·罗梅罗](http://brettromero.com/)** 是一位数据分析师，拥有在多个国家和行业（包括政府、管理咨询和金融）的工作经验。目前，他在科索沃的普里什蒂纳担任数据顾问，与联合国开发计划署（UNDP）以及[开放数据科索沃](http://opendatakosovo.org/)等发展机构合作。

[原文](http://brettromero.com/wordpress/data-science-kaggle-walkthrough-cleaning-data/)。经许可转载。

**相关：**

+   [进行数据科学：Kaggle 演练第 1 部分 – 介绍](/2016/05/doing-data-science-kaggle-walkthrough-intro.html)

+   [在 Twitter 上进行数据科学](/2015/09/data-science-at-twitter.html)

+   [掌握 Python 机器学习的 7 个步骤](/2015/11/seven-steps-machine-learning-python.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 为你的组织提供IT支持

* * *

### 相关文章

+   [分类指标演示：逻辑回归与…](https://www.kdnuggets.com/2022/10/classification-metrics-walkthrough-logistic-regression-accuracy-precision-recall-roc.html)

+   [集成学习技术：使用Python中的随机森林进行演示](https://www.kdnuggets.com/ensemble-learning-techniques-a-walkthrough-with-random-forests-in-python)

+   [7个免费的Kaggle微课程，适合数据科学初学者](https://www.kdnuggets.com/7-free-kaggle-micro-courses-for-data-science-beginners)

+   [2024年成为数据科学家的前10名Kaggle机器学习项目](https://www.kdnuggets.com/top-10-kaggle-machine-learning-projects-to-become-data-scientist-in-2024)

+   [在Kaggle竞赛中的前4个技巧及其重要性](https://www.kdnuggets.com/2022/05/packt-top-4-tricks-competing-kaggle-start.html)

+   [最全面的Kaggle解决方案和创意列表](https://www.kdnuggets.com/2022/11/comprehensive-list-kaggle-solutions-ideas.html)
