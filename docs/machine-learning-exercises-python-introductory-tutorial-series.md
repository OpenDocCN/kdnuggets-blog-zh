# 《Python 中的机器学习练习：入门教程系列》

> 原文：[`www.kdnuggets.com/2017/07/machine-learning-exercises-python-introductory-tutorial-series.html`](https://www.kdnuggets.com/2017/07/machine-learning-exercises-python-introductory-tutorial-series.html)

**作者：John Wittenauer，数据科学家。**

> **编辑说明：** 本教程系列于 2014 年 9 月开始，8 期教程在 2 年内发布。我提及这一点是为了将约翰的第一段话置于背景中，并向读者保证，这个包括所有代码的系列教程在今天仍然与写作时一样相关和最新。这些资料非常棒，无论是对于参加 Andrew Ng 的 MOOC 课程的人，还是作为独立资源。

今年我职业发展中的一个关键时刻是发现了 Coursera。我听说过“MOOC”现象，但一直没时间深入学习课程。今年早些时候，我终于下定决心报名参加 Andrew Ng 的[机器学习课程](https://www.coursera.org/course/ml)。我完成了整个课程，包括所有编程练习。这段经历让我认识到了这种教育平台的力量，从此我就迷上了它。

![Python 练习](http://www.johnwittenauer.net/machine-learning-exercises-in-python-part-1/)

**[第一部分 - 简单线性回归](http://www.johnwittenauer.net/machine-learning-exercises-in-python-part-1/)**

本博客文章将是系列中的第一篇，涵盖 Andrew 课程中的编程练习。我不太喜欢课程中使用 Octave 做作业的一个方面。虽然 Octave/Matlab 是一个很好的平台，但大多数现实世界的“数据科学”都是用 R 或 Python 完成的（当然还有其他语言和工具在使用，但这两种毫无疑问位列前茅）。由于我正在努力提高我的 Python 技能，所以我决定从头开始用 Python 做这些练习。完整的源代码可以在[我的 IPython Github 仓库](https://github.com/jdwittenauer/ipython-notebooks)找到。如果你感兴趣，你还会在根目录的子文件夹中找到这些练习所用的数据和原始练习 PDF。

**[第二部分 - 多变量线性回归](http://www.johnwittenauer.net/machine-learning-exercises-in-python-part-2/)**

在我关于 Python 机器学习系列的[第一部分](http://www.johnwittenauer.net/machine-learning-exercises-in-python-part-1/)中，我们介绍了安德鲁·恩的[机器学习](https://www.coursera.org/course/ml)课程第一部分的内容。在这篇文章中，我们将通过完成第二部分的练习来总结第一部分。如果你还记得，在第一部分中，我们实现了线性回归来预测新食品卡车的利润，这些卡车将被放置在某个城市中。第二部分的任务是预测房屋的销售价格。这次的不同之处在于我们有多个因变量。我们得到的输入包括房屋的平方英尺数以及卧室的数量。我们能否轻松扩展之前的代码来处理多元线性回归？让我们来看看吧！

![Python 练习](http://www.johnwittenauer.net/machine-learning-exercises-in-python-part-3/)

**[第三部分 - 逻辑回归](http://www.johnwittenauer.net/machine-learning-exercises-in-python-part-3/)**

在系列的第二部分中，我们总结了使用梯度下降法实现多元线性回归，并将其应用于一个简单的房价数据集。在这篇文章中，我们将目标从预测一个连续值（回归）转变为将结果分类为两个或更多离散类别（分类），并将其应用于学生录取问题。假设你是一个大学部门的管理员，你想根据申请人在两个考试中的成绩来确定每个申请人的录取机会。你有来自先前申请人的历史数据，可以用作训练集。对于每个训练样本，你都有申请人在两个考试中的得分以及录取决策。为此，我们将构建一个分类模型，该模型使用一种名为逻辑回归的稍微让人困惑的技术，根据考试成绩估计录取概率。

**[第四部分 - 多元逻辑回归](http://www.johnwittenauer.net/machine-learning-exercises-in-python-part-4/)**

在本系列的第三部分中，我们实现了简单和正则化逻辑回归，完成了安德鲁·恩机器学习课程第二个练习的 Python 实现。然而，我们的解决方案有一个限制——它仅适用于二分类问题。在这篇文章中，我们将扩展之前练习的解决方案，以处理多类分类问题。在此过程中，我们将涵盖第三部分的前半部分，并为下一个重要话题——神经网络做好准备。

**[第五部分 - 神经网络](http://www.johnwittenauer.net/machine-learning-exercises-in-python-part-5/)**

在第四部分中，我们通过扩展解决方案来处理多类分类，并在手写数字数据集上进行测试，从而完成了逻辑回归的实现。仅使用逻辑回归，我们就能够达到约 97.5% 的分类准确率，这相当不错，但几乎达到了线性模型的极限。在这篇博客文章中，我们将再次处理手写数字数据集，但这次使用带有反向传播的前馈神经网络。我们将实现未正则化和正则化的神经网络成本函数版本，并通过反向传播算法计算梯度。最后，我们将通过优化器运行算法，并评估网络在手写数字数据集上的表现。

**[第六部分 - 支持向量机](http://www.johnwittenauer.net/machine-learning-exercises-in-python-part-6/)**

我们现在正接近课程内容和这系列博客文章的最后阶段。在这个练习中，我们将使用支持向量机（SVM）来构建一个垃圾邮件分类器。我们将从一些简单的二维数据集开始使用 SVM 以了解其工作原理。然后我们将查看一组电子邮件数据，并利用 SVM 在处理过的邮件上构建分类器，以确定它们是否是垃圾邮件。

![Python 练习](http://www.johnwittenauer.net/machine-learning-exercises-in-python-part-7/)

**[第七部分 - K 均值聚类与 PCA](http://www.johnwittenauer.net/machine-learning-exercises-in-python-part-7/)**

我们现在只剩下这系列中的最后两篇文章了！在这一部分中，我们将讨论两个引人入胜的主题：K 均值聚类和主成分分析（PCA）。K 均值和 PCA 都是无监督学习技术的例子。无监督学习问题没有任何标签或目标供我们学习以进行预测，因此无监督算法试图从数据本身中学习一些有趣的结构。我们将首先实现 K 均值，并看看它如何用于压缩图像。我们还将实验 PCA，以找到人脸图像的低维表示。

**[第八部分 - 异常检测与推荐](http://www.johnwittenauer.net/machine-learning-exercises-in-python-part-8/)**

我们现在已经到达这一系列的最后一篇文章！这是一次有趣的旅程。安德鲁的课程做得非常出色，将其全部翻译成 Python 是一次有趣的经历。在这最后一部分中，我们将覆盖课程中的最后两个主题——异常检测和推荐系统。我们将使用高斯模型实现异常检测算法，并将其应用于检测网络上的故障服务器。我们还将看到如何使用协同过滤构建推荐系统，并将其应用于电影推荐数据集。

**简介: [约翰·维滕瑙尔](http://www.johnwittenauer.net/)** ([@jdwittenauer](https://twitter.com/jdwittenauer)) 是一名数据科学家、工程师、企业家和技术爱好者。

**相关内容：**

+   掌握 Python 机器学习的 7 个步骤

+   数据科学新手: 软件工程师入门教程系列

+   机器学习: 完整详细概述

* * *

## 我们的前 3 名课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 管理

* * *

### 更多相关主题

+   [入门 Pandas 教程](https://www.kdnuggets.com/2022/03/introductory-pandas-tutorial.html)

+   [使用管道编写干净的 Python 代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [是什么让 Python 成为初创公司理想的编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每个数据科学家都应该了解的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [停止学习数据科学以寻找目标，并通过寻找目标来...](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)
