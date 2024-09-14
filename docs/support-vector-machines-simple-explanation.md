# 支持向量机：简单解释

> 原文：[`www.kdnuggets.com/2016/07/support-vector-machines-simple-explanation.html`](https://www.kdnuggets.com/2016/07/support-vector-machines-simple-explanation.html)

**作者：Noel Bambrick，AYLIEN**。

### 介绍

在这篇文章中，我们将向你介绍支持向量机（SVM）机器学习算法。我们将采用类似于我们最近发布的 [朴素贝叶斯入门；简单解释](http://blog.aylien.com/post/120703930533/naive-bayes-for-dummies-a-simple-explanation) 的方式，保持简短且不过于技术化。我们的目标是让那些刚接触机器学习的你对这一算法的关键概念有一个基本的理解。

### 支持向量机 - 它们是什么？

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

支持向量机（SVM）是一种监督式机器学习算法，可用于分类和回归目的。SVM 更常用于分类问题，因此这也是我们在这篇文章中关注的内容。

SVM 基于找到一个最佳划分数据集为两个类别的超平面的想法，如下图所示。

![SVM](img/fa2e2d0a1043695bf64083c0efe4dbcf.png)

#### 支持向量

支持向量是离超平面最近的数据点，是一个数据集中的点，如果这些点被移除，会改变划分超平面的位置。因此，它们可以被视为数据集的关键元素。

### 什么是超平面？

作为一个简单的例子，对于只有两个特征的分类任务（如上图所示），你可以将超平面视为一条线，它线性地分隔并分类一组数据。

从直观上看，数据点离超平面的距离越远，我们对其正确分类的信心就越高。因此，我们希望数据点尽可能远离超平面，同时仍处于其正确的一侧。

所以当新的测试数据被添加时，它落在超平面的哪一侧将决定我们赋予它的类别。

#### 我们如何找到合适的超平面？

换句话说，我们如何最好地将数据中的两个类别隔离开来？

超平面与来自任一数据集的最近数据点之间的距离称为边际。目标是选择一个具有最大边际的超平面，使得训练集中超平面与任何点之间的边际最大，从而增加新数据被正确分类的可能性。

![SVM](img/850fa031b44c27acac9ce455b9e7cc95.png)

#### 但是当没有明确的超平面时会发生什么？

这就是难点所在。数据很少像我们上面的简单示例那样干净。一个数据集往往更像下面这些混乱的球，代表一个线性不可分的数据集。

![SVM](img/8f6030cd7d14f278f090e4bdb0f2f6bd.png)< 要对上面这样的数据集进行分类，需要从 2D 视图转换到 3D 视图。用另一个简化的例子来解释这一点最为直观。假设我们上面两组彩色球放在一个平面上，然后这个平面突然被提起，导致球体飞向空中。当球在空中时，你用平面来分隔它们。这种‘提起’球体的操作代表了将数据映射到更高维度。这被称为核方法。你可以在[这里](http://t.umblr.com/redirect?z=http%3A%2F%2Fwww.eric-kim.net%2Feric-kim-net%2Fposts%2F1%2Fkernel_trick.html&t=YWFkNDNjNzEyOTdmMzc5ZGRlNWU5YjBjZjMwOGI3MTMwMWQ1YjBkNix1cjlJVDBQcA%3D%3D)阅读更多关于核方法的信息。

![SVM](img/086fc32f5b768512fea953826d12bf5d.png)

因为我们现在在三维空间中，所以我们的超平面不再是一个直线。它现在必须是一个平面，如上面的例子所示。其理念是数据将持续映射到更高的维度，直到能够形成一个超平面来进行分隔。

### 支持向量机的优缺点

**优点**

+   准确性

+   在较小且较干净的数据集上表现良好

+   它可能更高效，因为它使用了训练点的子集

**缺点**

+   不适合较大的数据集，因为 SVM 的训练时间可能较长

+   对于噪声较多且类重叠的数据集效果较差

### SVM 的用途

SVM 用于文本分类任务，如类别分配、垃圾邮件检测和情感分析。它还常用于图像识别挑战，特别是在基于特征的识别和基于颜色的分类中表现优异。SVM 在手写数字识别的许多领域中也扮演着重要角色，例如邮政自动化服务。

这就是对支持向量机的一个非常高层次的介绍。如果你想深入了解 SVM，我们建议你查看（需要找到视频或更详细博客的链接）。

**关于**：本博客最初发表于[**AYLIEN 文本分析博客**](http://t.sidekickopen57.com/e1t/c/5/f18dQhb0S7lC8dDMPbW2n0x6l2B9nMJW7t5XZs8q5vngW7fZwJ-3N1L7sVQsyTK56dFYsf8TlV-x02?t=http%3A%2F%2Fblog.aylien.com%2Fpost%2F146410178273%2Fsupport-vector-machines-for-dummies-a-simple&si=5740235936759808&pi=75c5cbd5-7b4f-411f-e28d-d4dccc46da4b)。AYLIEN 提供工具和服务，帮助开发人员和数据科学家在大规模处理非结构化内容。

[原文](http://blog.aylien.com/post/146410178273/support-vector-machines-for-dummies-a-simple)。经许可转载。

**相关：**

+   如何选择支持向量机核

+   深度学习什么时候比 SVM 或随机森林更有效？

+   机器学习关键术语，解释

### 更多相关话题

+   [支持向量机：直观的方法](https://www.kdnuggets.com/2022/08/support-vector-machines-intuitive-approach.html)

+   [支持向量机的温和介绍](https://www.kdnuggets.com/2023/07/gentle-introduction-support-vector-machines.html)

+   [语义向量搜索如何改变客户支持互动](https://www.kdnuggets.com/how-semantic-vector-search-transforms-customer-support-interactions)

+   [Python 向量数据库和向量索引：构建 LLM 应用](https://www.kdnuggets.com/2023/08/python-vector-databases-vector-indexes-architecting-llm-apps.html)

+   [线性回归与逻辑回归：简明解释](https://www.kdnuggets.com/2022/03/linear-logistic-regression-succinct-explanation.html)

+   [KDnuggets 新闻 22:n12，3 月 23 日：最佳数据科学书籍…](https://www.kdnuggets.com/2022/n12.html)
