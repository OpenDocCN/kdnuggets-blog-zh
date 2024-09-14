# 异常检测简介

> 原文：[https://www.kdnuggets.com/2017/04/datascience-introduction-anomaly-detection.html](https://www.kdnuggets.com/2017/04/datascience-introduction-anomaly-detection.html)

**由 [DataScience.com](https://www.datascience.com/blog/intro-to-anomaly-detection-learn-data-science-tutorials) 提供赞助。**

本概述旨在为数据科学和机器学习领域的初学者提供指导。几乎不需要正式的专业经验，但读者应具备一些基本的微积分（特别是积分）、编程语言 Python、函数式编程和机器学习的知识。

### 简介：异常检测

异常检测是一种用于识别不符合预期行为的异常模式的技术，这些模式被称为离群点。它在商业中有许多应用，从入侵检测（识别可能表示黑客攻击的网络流量中的异常模式）到系统健康监测（在 MRI 扫描中发现恶性肿瘤），以及信用卡交易中的欺诈检测到操作环境中的故障检测。

本概述将涵盖几种检测异常的方法，以及如何使用简单移动平均（SMA）或低通滤波器在 Python 中构建检测器。

[![](../Images/1ff5523f9fb1df59f1b2a666d68c0ee2.png)](https://www.datascience.com/blog/intro-to-anomaly-detection-learn-data-science-tutorials)

图 1\. 太阳黑子的异常

### 什么是异常？

在开始之前，重要的是建立异常的定义边界。异常可以广泛地分类为：

1.  **点异常：** 如果数据的单个实例与其他数据偏差过大，则该实例异常。 *业务用例：* 基于“消费金额”检测信用卡欺诈。

1.  **上下文异常：** 异常是特定于上下文的。这种类型的异常在时间序列数据中很常见。 *业务用例：* 在假期期间每天花费 100 美元用于食物是正常的，但其他时候可能会显得异常。

1.  **集体异常：** 一组数据实例集体有助于检测异常。 *业务用例：* 某人意外地尝试从远程机器复制数据到本地主机，这种异常将被标记为潜在的网络攻击。

异常检测类似于 - 但不完全相同于 - 噪声去除和新颖性检测。 **新颖性检测** 关注于识别新观察中未包含在训练数据中的未观察到的模式 - 比如圣诞节期间对 YouTube 新频道的突然兴趣。 **噪声去除** ([NR](http://datamining.rutgers.edu/publication/tkdehcleaner.pdf)) 是保护分析免受不必要观察发生的过程；换句话说，就是从原本有意义的信号中去除噪声。

### 异常检测技术

**简单统计方法**

识别数据异常的最简单方法是标记那些偏离分布的常见统计属性的数据点，包括均值、中位数、众数和分位数。假设异常数据点的定义是偏离均值一定标准差的数据点。遍历时间序列数据的均值并不完全简单，因为它不是静态的。你需要一个滚动窗口来计算数据点的平均值。从技术上讲，这叫做滚动平均或移动平均，旨在平滑短期波动并突出长期波动。从数学上讲，n期简单移动平均也可以定义为“低通滤波器”。（卡尔曼滤波器是这种度量的更复杂版本；你可以在[这里](http://www.bzarg.com/p/how-a-kalman-filter-works-in-pictures/)找到一个非常直观的解释。）

**挑战** 低通滤波器可以帮助你在简单用例中识别异常，但在某些情况下，这种技术可能不起作用。以下是一些例子：

+   数据中包含的噪声可能类似于异常行为，因为正常行为和异常行为之间的边界通常并不精确。

+   异常或正常的定义可能会频繁变化，因为恶意对手会不断适应。因此，基于移动平均的阈值可能并不总是适用。

+   这种模式基于季节性。这涉及到更复杂的方法，例如将数据分解为多个趋势，以识别季节性的变化。

### 基于机器学习的方法

下面是流行的基于机器学习的异常检测技术的简要概述。

**基于密度的异常检测**

基于密度的异常检测基于k最近邻算法。

*假设：* 正常数据点通常位于密集的邻域中，而异常数据点则远离这些邻域。

使用评分评估最近的数据点，这个评分可能是欧氏距离或依赖于数据类型（分类或数值）的类似度量。它们可以大致分为两种算法：

1.  [k最近邻](http://www.scholarpedia.org/article/K-nearest_neighbor)：k-NN是一种简单的非参数懒惰学习技术，用于根据距离度量（如欧氏距离、曼哈顿距离、闵可夫斯基距离或汉明距离）的相似性来分类数据。

1.  数据的相对密度：这通常被称为[局部离群因子](http://link.springer.com/article/10.1007%2Fs10618-012-0300-z)（LOF）。这个概念基于一种叫做可达距离的距离度量。

**基于聚类的异常检测**

聚类是无监督学习领域中最流行的概念之一。

*假设：* 相似的数据点往往属于相似的组或簇，这取决于它们与局部中心的距离。

[K-means](https://www.datascience.com/blog/introduction-to-k-means-clustering-algorithm-learn-data-science-tutorials) 是一种广泛使用的聚类算法。它创建了‘k’个相似的数据点簇。落在这些组之外的数据实例可能会被标记为异常。

**基于支持向量机的异常检测**

[支持向量机](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.675.575&rep=rep1&type=pdf) 是另一种有效的异常检测技术。SVM 通常与监督学习相关，但也有扩展（例如[OneClassCVM,](http://scikit-learn.org/stable/modules/generated/sklearn.svm.OneClassSVM.html)）可用于将异常检测作为无监督问题（即训练数据未标记）。该算法学习一个软边界，以便使用训练集对正常数据实例进行聚类，然后，通过测试实例调整自身，以识别落在学习区域之外的异常。

根据用例，异常检测器的输出可能是用于过滤特定领域阈值的数值标量值或文本标签（例如二进制/多标签）。

### 使用低通滤波器构建简单的检测解决方案

在本节中，我们将重点介绍如何使用移动平均构建一个简单的异常检测包，以识别样本数据集中每月太阳黑子数量的异常，该数据集可以通过以下命令在[这里下载](http://www-personal.umich.edu/~mejn/cp/data/sunspots.txt)：

```py
wget -c -b www-personal.umich.edu/~mejn/cp/data/sunspots.txt

```

该文件包含3,143行数据，包含1749-1984年间收集的太阳黑子信息。太阳黑子定义为太阳表面上的黑点。研究太阳黑子有助于科学家理解太阳在一段时间内的性质，特别是其磁性特性。

阅读本教程的其余部分，包括[**DataScience.com网站**](https://www.datascience.com/blog/intro-to-anomaly-detection-learn-data-science-tutorials)上的Python代码。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT

* * *

### 更多相关话题

+   [数据科学中异常检测技术的初学者指南](https://www.kdnuggets.com/2023/05/beginner-guide-anomaly-detection-techniques-data-science.html)

+   [BigQuery 中的异常检测：揭示隐藏的洞察并推动行动](https://www.kdnuggets.com/anomaly-detection-in-bigquery-uncover-hidden-insights-and-drive-action)

+   [如何使用 Python 执行运动检测](https://www.kdnuggets.com/2022/08/perform-motion-detection-python.html)

+   [使用 Hugging Face Transformers 进行文本情感检测](https://www.kdnuggets.com/using-hugging-face-transformers-for-emotion-detection-in-text)

+   [KDnuggets 新闻，8月17日：如何执行运动检测…](https://www.kdnuggets.com/2022/n33.html)

+   [功能数据中用于异常检测的密度核深度](https://www.kdnuggets.com/density-kernel-depth-for-outlier-detection-in-functional-data)
