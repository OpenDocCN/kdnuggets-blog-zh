# 数据科学实用技巧：如何使用一对多和一对一进行多分类

> 原文：[`www.kdnuggets.com/2020/08/one-vs-rest-one-multi-class-classification.html`](https://www.kdnuggets.com/2020/08/one-vs-rest-one-multi-class-classification.html)

评论

**由机器学习专业人士兼作家托马斯·格雷**。

![](img/eddab375a6fe2bcdb19f51b4eb2cea5d.png)

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

只有少数分类模型支持多分类。特定算法，包括逻辑回归和感知机，最适合于二分类，并不支持多于两个类别的分类任务。解决[多分类问题](https://en.wikipedia.org/wiki/Multiclass_classification)的最佳替代方法是将多分类数据集拆分为多个适合二分类模型的数据集合。

用于[二分类](https://www.coursera.org/lecture/analytics-excel/introduction-to-binary-classification-TUihw)问题的算法无法处理多分类任务。因此，启发式方法，如一对一和一对多，用于将多分类问题拆分为多个二进制数据集，并训练二分类模型。

### 二分类与多分类

分类问题在机器学习中很常见。在大多数情况下，开发者倾向于使用监督机器学习方法来预测给定数据集的类别表。与回归不同，分类涉及设计分类器模型并训练它来输入和分类测试数据集。为此，你可以将数据集分为二分类或多分类模块。

如其名所示，二分类涉及解决只有两个类别标签的问题。这使得过滤数据、应用分类算法和训练模型以预测结果变得简单。另一方面，多分类适用于输入训练数据中有多个类别标签的情况。该技术使开发者能够将测试数据分类为多个二进制类别标签。

话虽如此，虽然二分类只需一个分类模型，但多分类方法中使用的模型则取决于分类技术。以下是多分类算法的两种模型。

### 多类别分类的一对多分类模型

一对多模型，也称为一对所有，是一种定义明确的启发式方法，利用二分类算法进行多类别分类。该技术涉及将多类别数据集拆分成多个二分类问题。之后，训练一个二分类器来处理每个二分类模型，其中最有信心的一个进行预测。

例如，对于一个包含红色、绿色和蓝色数据集的多类别分类问题，二分类可以按如下方式进行分类：

+   问题一：红色 vs. 绿色/蓝色

+   问题二：蓝色 vs. 绿色/红色

+   问题三：绿色 vs. 蓝色/红色

使用此模型唯一的挑战在于你需要为每个类别创建一个模型。上述数据集中的三个类别需要三个模型，这对于具有百万行数据的大数据集、较慢的模型（如神经网络）以及具有大量类别的数据集可能会很具挑战性。

一对多方法需要单独的模型来预测类似概率的得分。然后使用得分最高的类别索引来预测类别。因此，它通常用于能够自然预测得分或数值类别成员资格的分类算法，如感知器和逻辑回归。

### 多类别分类的一对一分类模型

与一对多模型类似，一对一模型是另一种出色的启发式方法，它利用二分类算法来处理多类别数据集。它也将多类别数据集拆分成二分类问题。然而，与一对多模型将数据集拆分为每个类别的单一二分类数据组不同，一对一分类模型将数据集分组为每个类别与其他所有类别的对比数据文件。

例如，[考虑到具有四个类别的多类别数据集问题](https://towardsdatascience.com/machine-learning-multiclass-classification-with-imbalanced-data-set-29f6a177c1a) — 蓝色、红色、绿色和黄色 — 一对一方法将其拆分为以下六个二分类数据集：

问题一：红色 vs. 绿色

问题二：红色 vs. 蓝色

问题三：红色 vs. 黄色

问题四：绿色 vs. 黄色

问题五：蓝色 vs. 绿色

问题六：蓝色 vs. 黄色

上述内容相比之前解释的一对多方法有更多的分类数据集。因此，用于计算二分类数据集总数的公式变为：

类别数 x (类别数 - 1)/2

从上述四个多类别数据集中，该公式给出了预期的六个二分类问题，如下所示：

4 x (4 - 1)/2

(4 x 3) /2

12/2

=6

虽然每个二分类模型只能预测一个类别标签，但“一对一”策略会预测投票最多的模型。如果二分类模型能够准确预测数值类别成员，如概率，那么得分总和最多的类别将被视为类别标签。

从本质上讲，该模型最适合支持支持向量机和相关的基于核的分类算法。这可能是因为核方法的规模与训练数据集的大小不成比例。此外，使用训练数据的子集可以逆转这种效果。

Scikit-learn 中由 [SVC 类](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html) 实现的支持向量学习机器支持多类分类的“一对一”分类方法。要使用此方法，你需要将“决策函数形状”设置更改为“ovo”。

此外，scikit-learn 归档还允许使用单独的“一对一”分类器多类方法，使得“一对一”选项可以与任何分类器一起使用。这使得该多类方法可以与任何其他二分类器（如感知器、逻辑回归、SVM 或本身支持多类分类的不同分类器）一起使用。

### 结论

如前所述，使用“一对多”多类分类选项使处理大型数据集变得困难，因为类别实例数量庞大。然而，“一对一”多类分类选项仅将主要数据集拆分为每对类别的单一二分类任务。

尽管“一对多”方法无法处理多个数据集，但它训练的分类器数量较少，使其成为更快的选择，且经常受到青睐。另一方面，“一对一”方法由于特定类别的主导性，较少产生数据集的不平衡。

话虽如此，你认为哪一种方法更好呢？请在下方评论区分享你的想法。

**简介：** Thomas Glare 是一位机器学习专家，教开发者如何从现代机器学习方法和实践教程中获得最佳效果。你可以在此 [网站](https://futureentech.com/write-business-blog-visitors/) 上找到他的文章。

**相关内容：**

+   [机器学习中的分类项目：循序渐进的指南](https://www.kdnuggets.com/2020/06/classification-project-machine-learning-guide.html)

+   [一种简单且可解释的二分类器性能度量](https://www.kdnuggets.com/2020/03/interpretable-performance-measure-binary-classifier.html)

+   [使用 5 种机器学习算法分类稀有事件](https://www.kdnuggets.com/2020/01/classify-rare-event-machine-learning-algorithms.html)

### 了解更多此主题

+   [分类指标指南：逻辑回归的准确率、精确率、召回率和 ROC](https://www.kdnuggets.com/2022/10/classification-metrics-walkthrough-logistic-regression-accuracy-precision-recall-roc.html)

+   [更多分类问题的性能评估指标…](https://www.kdnuggets.com/2020/04/performance-evaluation-metrics-classification.html)

+   [使用 PyCaret 的二分类介绍](https://www.kdnuggets.com/2021/12/introduction-binary-classification-pycaret.html)

+   [使用 HuggingFace 微调 BERT 进行推文分类](https://www.kdnuggets.com/2022/01/finetuning-bert-tweets-classification-ft-hugging-face.html)

+   [用于分类的机器学习算法](https://www.kdnuggets.com/2022/03/machine-learning-algorithms-classification.html)

+   [分类的逻辑回归](https://www.kdnuggets.com/2022/04/logistic-regression-classification.html)
