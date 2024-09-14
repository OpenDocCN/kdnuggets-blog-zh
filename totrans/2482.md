# 每位初学者数据科学家应掌握的6种预测模型

> 原文：[https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

**作者 [Ivo Bernardo](https://www.linkedin.com/in/ivobernardo/)，数据科学家**

![](../Images/f2fc2bdcb0837b8563fd6157e54b6565.png)

图片由 [@barnimages](https://unsplash.com/@barnimages) 提供 — unsplash.com

* * *

## 我们的前3名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行IT工作

* * *

当你沉浸于机器学习和人工智能的炒作漩涡中时，似乎只有先进的技术才能解决你在构建预测模型时遇到的所有问题。但当你真正动手编写代码时，你会发现事实是非常不同的。作为数据科学家，你面临的许多问题是通过组合几种模型来解决的，而其中大多数模型已经存在了很久。

即使你使用更高级的模型解决问题，学习基础知识也会让你在大多数讨论中占得先机。特别是，了解更简单模型的优缺点将帮助你引导数据科学项目取得成功。事实是：高级模型能够做两件事——**放大或修正它们所基于的简单模型的一些缺陷**。

话虽如此，**让我们跳入数据科学的世界，了解你在成为数据科学家时应该学习和掌握的6种模型**。

## 线性回归

这是最古老的模型之一（例如，[弗朗西斯·高尔顿在19世纪使用了“回归”一词](https://en.wikipedia.org/wiki/Regression_analysis%C2%B4)），至今仍然是用数据表示线性关系的最有效方法之一。

学习线性回归是全球经济计量学课程的基本内容——学习这个线性模型将使你对解决回归问题（机器学习中最常见的问题之一）有一个良好的直觉，并且了解如何利用数学构建简单的线来预测现象。

学习线性回归还有其他好处——特别是当你学习了两种可以实现最佳性能的方法时：

+   [封闭形式解](https://towardsdatascience.com/normal-equation-in-python-the-closed-form-solution-for-linear-regression-13df33f9ad71)，一个几乎神奇的公式，通过简单的代数方程给出变量的权重。

+   [梯度下降](https://en.wikipedia.org/wiki/Gradient_descent)，一种朝着最佳权重前进的优化方法，用于优化其他类型的算法。

此外，我们可以通过简单的二维图来实际可视化线性回归，这使得该模型成为理解算法的一个非常好的起点。

学习相关资源：

+   [DataCamp 的线性回归解释](https://www.datacamp.com/community/tutorials/essentials-linear-regression-python)

+   [Sklearn 的回归实现](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html)

+   [R 数据科学 Udemy 课程线性回归部分](https://www.udemy.com/course/r-for-data-science-first-step-data-scientist/?couponCode=MEDIUMREADERS)

## 逻辑回归

尽管名为回归，逻辑回归仍然是你开始掌握分类问题的最佳模型。

学习逻辑回归有几个好处，即：

+   初步了解分类和多分类问题（机器学习任务中的一个重要部分）。

+   理解函数变换，例如 sigmoid 函数所做的变换。

+   理解梯度下降的其他函数的使用及其对优化函数的中立性。

+   初步了解对数损失函数。

学习逻辑回归后你应期待了解什么？你将能够理解分类问题背后的机制，以及如何利用机器学习来区分类别。此类问题的一些示例：

+   了解交易是否欺诈。

+   了解客户是否会流失。

+   根据贷款违约的概率对其进行分类。

就像线性回归一样，逻辑回归也是一种线性算法 —— 学习了这两者后，你将了解到线性算法的主要限制，以及它们如何无法表示许多现实世界的复杂性。

学习相关资源：

+   [DataCamp 的逻辑回归 R 解释](https://www.datacamp.com/community/tutorials/logistic-regression-R)

+   [Sklearn 的逻辑回归实现](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html)

+   [R 数据科学 Udemy 课程 — 分类问题部分](https://www.udemy.com/course/r-for-data-science-first-step-data-scientist/?couponCode=MEDIUMREADERS)

## 决策树

第一个非线性算法应该学习的是决策树。决策树是一个基于 if-else 规则的相当简单且可解释的算法，它将帮助你深入了解非线性算法及其优缺点。

决策树是所有树模型的基石——通过学习它们，你也将准备好学习其他技术，如XGBoost或LightGBM（更多信息见下文）。

有趣的是，决策树既适用于回归问题，也适用于分类问题，两者之间的差异很小——选择影响结果的最佳变量的原理大致相同，你只是切换用于评估的标准——在这种情况下是误差度量。

**尽管你有回归的超参数概念（如正则化参数），但在决策树中它们至关重要**，能够区分一个好的模型和一个绝对无用的模型。超参数在你的机器学习之旅中将是至关重要的，决策树是测试它们的绝佳机会。

关于决策树的一些资源：

+   [LucidChart决策树解释](https://www.lucidchart.com/pages/decision-tree)

+   [Sklearn的决策树解释](https://scikit-learn.org/stable/modules/tree.html)

+   [我关于分类决策树的博客文章](https://towardsdatascience.com/6-things-you-should-learn-to-kickstart-your-natural-language-processing-skills-4e10a1d3d2a?sk=a4231696321577dfcf563f532a69d542)

+   [R 数据科学 Udemy 课程 — 树模型部分](https://www.udemy.com/course/r-for-data-science-first-step-data-scientist/?couponCode=MEDIUMREADERS)

## 随机森林

由于对超参数的敏感性和相对简单的假设，决策树在结果上比较有限。当你学习它们时，你会明白它们非常容易过拟合，创建出不能对未来进行泛化的模型。

**随机森林的概念非常简单——如果决策树是独裁制，随机森林就是民主制。**它们帮助在不同的决策树之间实现多样性，从而增强你的算法的鲁棒性——就像决策树一样，你可以配置大量的超参数来提升这个袋装模型的性能。**什么是袋装？** 这是机器学习中一个非常重要的概念，它为不同的模型带来稳定性——你只需使用平均值或投票机制，将不同模型的结果转化为一种单一的方法。

在实践中，随机森林训练固定数量的决策树，并（通常）对所有这些模型的结果进行平均——就像决策树一样，我们有分类和回归随机森林。如果你听说过“集体智慧”这一概念，那么袋装模型就是将这一概念应用于机器学习模型训练中。

学习随机森林算法的一些资源：

+   [Tony Yiu关于随机森林的Medium文章](https://towardsdatascience.com/understanding-random-forest-58381e0602d2)

+   [Sklearn的随机森林分类器实现](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)

+   [R For Data Science Udemy课程——基于树的模型部分](https://www.udemy.com/course/r-for-data-science-first-step-data-scientist/?couponCode=MEDIUMREADERS)

## XGBoost/LightGBM

基于决策树的其他算法，如XGBoost或LightGBM，使其更加稳定。这些模型是提升算法，它们通过之前弱学习者所犯的错误来找到更稳健且更具泛化能力的模式。

关于机器学习模型的这一思路在[Michael Kearns关于弱学习者和假设检验的论文](https://www.cis.upenn.edu/~mkearns/papers/boostnote.pdf)后获得了关注，展示了提升模型可能是解决模型偏差/方差权衡的一个优秀方案。此外，这些模型也是[Kaggle竞赛中的一些热门选择。](https://towardsdatascience.com/xgboost-lightgbm-and-other-kaggle-competition-favorites-6212e8b0e835)

XGBoost和LightGBM是两种著名的Boosting算法实现。学习它们的一些资源：

+   [微软的Lightgbm GitHub页面](https://github.com/microsoft/LightGBM)

+   [Pranjal Khandelwal](https://www.analyticsvidhya.com/blog/author/pranjalk7/)的关于XGBoost与LightGBM的文章

+   [Vishal Morde的关于XGBoost的Medium帖子](https://towardsdatascience.com/https-medium-com-vishalmorde-xgboost-algorithm-long-she-may-rein-edd9f99be63d)

## 人工神经网络

最终，当前预测模型的圣杯——人工神经网络（ANNs）。

人工神经网络目前是发现数据中非线性模式和建立独立变量与因变量之间复杂关系的最佳模型之一。通过学习它们，你将接触到激活函数、反向传播和神经网络层的概念——这些概念将为你学习深度学习模型提供良好的基础。

此外，神经网络在其架构上有许多不同的变体——学习最基本的神经网络将为跳到其他类型的模型（如循环神经网络（主要用于自然语言处理）和卷积神经网络（主要用于计算机视觉））打下基础。

学习它们的一些额外资源：

+   [IBM关于“神经网络是什么”的文章](https://www.ibm.com/cloud/learn/neural-networks)

+   [Keras（神经网络实现和抽象）文档](https://keras.io/)

+   [Sanchit Tanwar关于构建第一个神经网络的文章](https://towardsdatascience.com/building-our-first-neural-network-in-keras-bdc8abbc17f5)

就这样！这些模型应该能为你在数据科学和机器学习方面提供一个良好的开端。通过学习它们，你将为学习更高级的模型做好准备，并且轻松掌握这些模型背后的数学原理。

好的一点是，更高级的内容通常基于我在这里展示的 6 个模型，因此了解它们的基本数学和机制永远不会有害，即使是在你需要用到“重型武器”的项目中。

**你觉得有什么遗漏的吗？在下面的评论中写下来，我很想听听你的意见。**

***我已经在[***Udemy 课程***](https://www.udemy.com/course/r-for-data-science-first-step-data-scientist/?referralCode=6D1757B5E619B89FA064)*** 上设置了一个学习大多数这些模型的课程——这个课程适合初学者，我希望你能来参加。***

**个人简介：[Ivo Bernardo](https://www.linkedin.com/in/ivobernardo/)** 是 DareData Engineering 的合伙人兼数据科学家，以及 Udemy 的畅销课程讲师和教师。

[原文](https://towardsdatascience.com/6-predictive-models-models-every-beginner-data-scientist-should-master-7a37ec8da76d)。经许可转载。

### 更多相关话题

+   [每个数据科学家都应该知道的三种 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [KDnuggets™ 新闻 22:n01，1月5日：3 个跟踪和可视化工具…](https://www.kdnuggets.com/2022/n01.html)

+   [成为优秀数据科学家所需的 5 个关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [2021 年最佳 ETL 工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [停止学习数据科学以寻找目标，并通过寻找目标来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)
