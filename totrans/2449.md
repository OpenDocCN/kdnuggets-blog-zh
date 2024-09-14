# 机器学习分类算法

> 原文：[https://www.kdnuggets.com/2022/03/machine-learning-algorithms-classification.html](https://www.kdnuggets.com/2022/03/machine-learning-algorithms-classification.html)

![机器学习分类算法](../Images/841f1967f90042877b182b0acb204859.png)

[Kevin Ku](https://unsplash.com/@ikukevk) 通过 Unsplash

你可以使用许多类型的算法，因此选择哪个算法以及哪个算法最适合你的任务可能会感到非常困惑。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行IT服务

* * *

区分不同类型的算法的一个好方法是通过它们的学习类型和任务。我将会介绍不同类型的分类算法。但首先，让我们理解机器学习中的不同学习类型。

# 机器学习的类型

机器学习有3种不同的类型：

1.  监督学习

1.  无监督学习

1.  强化学习

**监督学习**是指算法在标记数据集上进行学习，并分析训练数据。这些标记数据集包含输入和预期输出。

一个**监督学习**算法的例子是逻辑回归。

**无监督学习**在无标记数据上进行学习，推断隐藏结构以产生准确和可靠的输出。

一个**无监督学习**算法的例子是K-Means。

**强化学习**是训练机器学习模型以做出一系列决策。它关注于智能体如何在环境中采取行动以最大化累计奖励的概念。

# 分类与回归：

监督学习可以进一步分为两个类别：分类和回归。

**分类**是预测标签，通过识别一个对象属于哪个类别，基于不同的参数。

**回归**是预测连续输出，通过寻找因变量和自变量之间的相关性。

许多算法可以用于分类和回归问题。在本文中，我们将讨论可以用于分类任务的算法。

## 逻辑回归

![机器学习分类算法](../Images/554bc3913eb38eb35fa36003bb007ce7.png)

[来源](https://www.statstest.com/simple-logistic-regression/)

逻辑回归是一种用于分类问题的机器学习算法，基于概率的概念。它用于当因变量（目标）是分类变量时。它广泛应用于当分类问题是二元的：真或假，是或否等。逻辑回归使用 sigmoid 函数来返回标签的概率。

例如，它可以用来预测一封电子邮件是否为垃圾邮件（1）或不是（0）。

## 决策树

![分类的机器学习算法](../Images/17c569e347e751740f8188e77b47b232.png)

[来源](https://medium.datadriveninvestor.com/decision-tree-algorithm-with-hands-on-example-e6c2afb40d38)

决策树是一种非参数监督学习方法，用于分类和回归。总体目标是通过从数据特征中推断出简单的决策规则来构建一个模型，以预测目标变量的值。

决策树的概念体现在它的名称中。它通过层次化的方法建立树枝，每个树枝可以被视为一个 if-else 语句。构建分类决策树的过程是通过将数据分割成多个分区，并在每个节点上再次进行分割的迭代过程。数据集的最终分类位于决策树的叶节点。

例如，将追求篮球的青少年不同的属性和特征进行分类。你的身高、体重、族裔群体变量会被划分成多个分区，然后再进行划分。

## 随机森林®

![分类的机器学习算法](../Images/11f341206d9515b13cc4543719a87742.png)

[来源](https://ai-pool.com/a/s/random-forests-understanding)

随机森林® 是一种监督学习算法，由许多决策树组成。一个好的记忆方法是把它当作许多树组成的森林。

随机森林算法基于决策树生成的预测结果来产生其结果。该预测是通过取各个决策树输出的平均值或均值来完成的。树木数量的增加提高了结果的精度。因此，森林中的决策树数量越多，准确性越高，过拟合被防止或至少减少。

与决策树相比，随机森林算法模型难以解释并快速做出决策，同时所需时间较长。

## K-最近邻（KNN）

![分类的机器学习算法](../Images/098b622a1e7ed80a21f3fd495e3b810d.png)

[来源](https://www.javatpoint.com/k-nearest-neighbor-algorithm-for-machine-learning)

K-最近邻（KNN）算法是一种监督机器学习算法，可以用于解决分类和回归问题。KNN 算法假设相似的事物存在于接近的位置。

KNN 使用相似性概念，或其他词汇如距离、邻近或接近。它使用数学方法计算图上点之间的距离。通过这样做，它根据最接近的标记数据点来标记未观察到的数据。

这句话：“物以类聚，人以群分” 与 KNN 相关。

## 支持向量机（SVM）

![机器学习分类算法](../Images/ea155fc0ac7d7f8ade3a85df3d23497d.png)

[来源](https://matlab1.com/support-vector-machine-2/)

支持向量机是一种监督学习模型，使用线性模型，可以用于分类和回归问题。

支持向量机算法的概念是创建一条线或一个超平面，将数据分为不同的类别。它使用那些接近超平面的数据点，这些点影响超平面的定位和方向，从而最大化分类器的边界。

*随机森林* 和 *RANDOMFORESTS* 是 Minitab, LLC 的注册商标。

**[Nisha Arya](https://www.linkedin.com/in/nisha-arya-ahmed/)** 是一名数据科学家和自由职业技术作家。她特别感兴趣于提供数据科学职业建议或教程以及数据科学相关的理论知识。她还希望探索人工智能如何有助于人类寿命的延续。作为一个热衷学习的人，她寻求拓宽技术知识和写作技能，同时帮助指导他人。

### 相关主题

+   [开始使用 Scikit-learn 进行机器学习中的分类](https://www.kdnuggets.com/getting-started-with-scikit-learn-for-classification-in-machine-learning)

+   [更多分类问题的性能评估指标](https://www.kdnuggets.com/2020/04/performance-evaluation-metrics-classification.html)

+   [使用 PyCaret 进行二分类介绍](https://www.kdnuggets.com/2021/12/introduction-binary-classification-pycaret.html)

+   [使用 HuggingFace 对 BERT 进行推特分类的微调](https://www.kdnuggets.com/2022/01/finetuning-bert-tweets-classification-ft-hugging-face.html)

+   [分类的逻辑回归](https://www.kdnuggets.com/2022/04/logistic-regression-classification.html)

+   [分类的最近邻](https://www.kdnuggets.com/2022/04/nearest-neighbors-classification.html)
