# 特征工程如何帮助你在Kaggle竞赛中表现出色 - 第3部分

> 原文：[https://www.kdnuggets.com/2017/07/feature-engineering-help-kaggle-competition-3.html](https://www.kdnuggets.com/2017/07/feature-engineering-help-kaggle-competition-3.html)

**作者：Gabriel Moreira，CI&T。**

![Header image](../Images/f1e89e36ceb486d673bddb4fa9cd5de5.png)

在[第一部分](https://medium.com/unstructured/how-feature-engineering-can-help-you-do-well-in-a-kaggle-competition-part-i-9cc9a883514d)和[第二部分](https://medium.com/unstructured/how-feature-engineering-can-help-you-do-well-in-a-kaggle-competition-part-ii-3645d92282b8)中，我介绍了[Outbrain点击预测机器学习竞赛](https://www.kaggle.com/c/outbrain-click-prediction)以及我应对挑战的初步任务。我展示了用于探索性数据分析、特征工程、交叉验证策略和基本统计与机器学习的基线预测模型的主要技术。

在这一系列的最后一篇文章中，我描述了如何使用更强大的机器学习算法解决点击预测问题，以及将我推上排行榜第19位（前2%）的集成技术。

### 正则化跟随者

预测点击率（CTR）的一个流行方法是使用[正则化跟随者（FTRL）](https://www.eecs.tufts.edu/~dsculley/papers/ad-click-prediction.pdf)优化器的逻辑回归，这种方法已被[谷歌](https://www.eecs.tufts.edu/~dsculley/papers/ad-click-prediction.pdf)用于生产环境中，每天预测数十亿次事件，使用相应的大特征空间。

这是一个线性模型，具有懒惰的系数（*权重*）表示方式，并且与L1正则化结合使用时，会导致非常稀疏的系数向量。这种稀疏性特征缩减了内存使用，使得对于具有数十亿维度的特征向量具有良好的可扩展性，因为每个实例通常只有几百个非零值。FTRL通过从磁盘或网络中流式传输示例来提供对大数据集的高效训练——每个训练示例只需处理一次（在线学习）。

我尝试了两种不同的FTRL实现，分别在[Kaggler](https://github.com/jeongyoonlee/Kaggler)和[Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit/wiki)（VW）框架中可用。

对于CTR预测，理解特征之间的交互是非常重要的。例如，来自“Nike”广告商的广告的平均转化率可能会有所不同，这取决于显示广告的出版商（例如 ESPN vs. Vogue）。

我考虑了所有分类特征来训练[Kaggler](https://github.com/jeongyoonlee/Kaggler)的FTRL模型。特征交互选项被启用，这意味着对于所有可能的两个特征组合，特征值被相乘并哈希（阅读关于特征哈希的内容，请参见[第一篇文章](https://medium.com/unstructured/how-feature-engineering-can-help-you-do-well-in-a-kaggle-competition-part-i-9cc9a883514d)），形成一个2²⁸维度的稀疏特征向量。这是我最慢的模型，训练时间超过了12小时。但我的LB分数跃升至0.67659，这是**方法 #6**的结果。

我还在[VW](https://github.com/JohnLangford/vowpal_wabbit/wiki)上尝试了FTRL，这是一个在CPU和内存资源使用方面非常快速和高效的框架。输入数据再次为分类特征，并添加了一些选定的数值分箱特征（阅读关于特征分箱的内容，请参见[第一篇文章](https://medium.com/unstructured/how-feature-engineering-can-help-you-do-well-in-a-kaggle-competition-part-i-9cc9a883514d)）。我的最佳模型在两小时内训练完成，并在**方法 #7**中获得了0.67512的LB分数。虽然准确率稍低，但比之前的模型快得多。

在第二个FTRL模型上使用VW时，我利用VW的超参数配置了仅一部分特征（命名空间）的交互（特征配对）。在这种情况下，交互仅配对了分类特征和一些选定的数值特征（没有分箱转换）。这一变化使得在**方法 #8**中取得了更好的结果：0.67697。

所有这三个FTRL模型都被集成到我的最终提交中，具体内容将在接下来的部分中描述。

### 领域感知因式分解机

一个最近的因式分解机变体，[领域感知因式分解机 (FFM)](http://www.csie.ntu.edu.tw/~cjlin/papers/ffm.pdf) 在2014年的两个CTR预测竞赛中取得了胜利。FFM尝试通过学习每个特征交互对的潜在因子来建模特征交互。该算法以[LibFFM](https://www.csie.ntu.edu.tw/~cjlin/libffm/)框架的形式提供，并被许多竞争者使用。LibFFM 对大数据集的并行处理和内存使用非常高效。

在**方法 #9**中，我使用了分类特征，并将配对交互哈希到700,000维度。我的最佳模型仅用37分钟训练完成，并且成为了我的最佳单一模型，LB分数为**0.67932**。

在**方法 #10**中，输入数据除了分类特征外，还包含了一些选定的数值分箱特征。训练数据增加到214分钟，LB分数为**0.67841**。

对于**方法 #11**，我尝试训练一个FFM模型，仅考虑训练集中最后30%的事件，假设在最后几天的训练可以提高对测试集未来两天（50%）的预测。LB得分为**0.6736**，提高了这两天的准确性，但降低了测试集中之前事件的准确性。

在机器学习项目中，某些有前景的方法最终可能会失败。这发生在一些FFM模型上。我尝试将学习从GBDT模型转移到带有叶子编码的FFM模型，这是一种在Criteo竞赛中获胜的[方法](http://www.csie.ntu.edu.tw/~r01922136/kaggle-2014-criteo.pdf)。然而，FFM模型的准确性下降，可能是因为GBDT模型在这个上下文中不够准确，增加了更多的噪声而非信号。

另一个不成功的方法是训练独立的FFM模型，针对测试集中事件更多的地理区域，比如一些国家（美国 ~ 80%，加拿大 ~ 5%，英国 ~ 5%，澳大利亚 ~ 2%，其他 ~ 8%）和美国州（加州 ~ 10%，德州 ~ 7%，佛罗里达 ~ 5%，纽约 ~ 5%）。为了生成预测，每个测试集事件被发送到专门针对这些地区的模型。专门针对美国的模型在预测该地区的点击时表现良好，但在其他地区准确性较低。因此，使用全球范围的FFM模型表现更好。

我在最终的集成中使用了三种选择的FFM方法，接下来我将展示。

### 集成方法

[集成方法](http://mlwave.com/kaggle-ensembling-guide/)是将不同模型的预测结果结合起来，以提高准确性和泛化能力。模型预测的相关性越小，集成的准确性可能越好。集成的主要思想是，单个模型不仅受信号影响，还受随机噪声的影响。通过结合多个模型的预测，可以取消大量噪声，从而提高模型的泛化能力，如下所示。

![](../Images/2684f72c212750e2759b372eaffb703b.png)

来自竞赛[不要过拟合](https://www.kaggle.com/c/overfitting#description)

一种简单而有效的集成方法是通过[平均](https://en.wikipedia.org/wiki/Average)合并模型预测。对于这次竞赛，我测试了许多加权平均类型，如算术、几何、调和和排序平均等。

我能找到的最佳方法是使用加权算术平均的[logit（sigmoidal logistic 的逆）](https://en.wikipedia.org/wiki/Logit)预测CTR（概率），如公式1所示。它考虑了三种选择的FFM模型（方法 #9、#10 和 #11）以及一个FTRL模型（方法 #6）的预测。这种平均方法使我在**方法 #12**中得到了**0.68418**的LB得分，比我最好的单一模型（方法 #10，得分为0.67932）有了显著提升。

![](../Images/74c390999bb9c9f2d3837d6cc21c02a6.png)

方程 1 — 通过模型预测的logit加权平均进行集成

那时，比赛提交截止日期仅剩几天。我选择在剩余时间里使用学习排序模型进行集成，以合并最佳模型的预测。

该模型是一个[GBDT](https://en.wikipedia.org/wiki/Gradient_boosting)，具有100棵树，并使用[XGBoost](https://xgboost.readthedocs.io/)的排名目标。这个集成模型考虑了最佳的3个FFM模型和3个FTRL模型的预测以及15个精选的工程数值特征（如用户浏览次数、用户偏好相似性和按类别的平均点击率）。

该集成层仅使用验证集数据进行训练（如[第二篇文章](https://medium.com/unstructured/how-feature-engineering-can-help-you-do-well-in-a-kaggle-competition-part-ii-3645d92282b8)中所述），在一种称为[Blending](http://mlwave.com/kaggle-ensembling-guide/)的设置中。这样做的原因是，如果训练集的预测也被集成模型考虑，它会优先考虑过拟合的模型，从而降低其对测试集的泛化能力。在最后一天的比赛中，**方法 #13** 给了我最佳的公共LB得分（**0.68688**）。比赛结束后，我的私人LB得分被揭示（**0.68716**），让我在最终排行榜上保持在第19位，如下图所示。

![](../Images/ad9fac701d8bac781d91c18751023241.png)

Kaggle的Outbrain点击预测最终排行榜

我没有跟踪确切的提交日期，但下图展示了我在比赛期间LB得分的演变情况。可以看出，第一次提交时得分有很大的跃升，但之后的改进变得更加困难，这在机器学习项目中是很常见的。

![](../Images/aca13969e6171642ff0693e191e62df1.png)

我在比赛期间的方法的LB得分

### 结论

我从这次比赛中学到的一些东西包括：

1.  良好的交叉验证策略对于竞争至关重要。

1.  应该花费大量时间进行特征工程。在数据集上添加新特征往往需要更多的努力和时间。

1.  哈希是稀疏数据的必备方法。实际上，在简单性和效率方面，它的表现优于独热编码（OHE）。

1.  OHE类别特征对于决策树集成并不理想。根据竞争对手分享的经验，树集成使用原始类别值（id）并且树木足够多时表现更好，因为这将减少特征向量的维度，提高随机特征集包含更多预测特征的机会。

1.  测试许多不同的框架对学习很有帮助，但通常需要大量时间来将数据转换为所需格式、阅读文档和调整超参数。

1.  阅读关于主要技术（FTRL和FFM）的科学论文对于调整超参数非常重要。

1.  研究论坛帖子、公共Kernel（共享代码）以及竞争者分享的过去解决方案是学习的好方法，对比赛至关重要。每个人都应该有所回馈！

1.  基于平均的集成方法大大提高了准确性，*与机器学习模型融合*提升了标准，*堆叠*是必须的。我没有时间探索堆叠，但根据其他竞争者的说法，使用固定折叠上的外部折叠预测增加了用于集成训练的可用数据（完整训练集），并提高了最终集成的准确性。

1.  不应在比赛最后时刻才生成提交文件！

Kaggle就像是前沿机器学习的大学，对于那些决定接受挑战并从过程中和同伴中学习的人来说。这是一次非常有趣的经历，能够与世界级数据科学家竞争和学习。

谷歌云平台在这个过程中是一个很棒的合作伙伴，提供了所有必要的工具来克服大数据和分布式计算的难关，而我的注意力和努力则集中在登顶山峰上。

**个人简介： [加布里埃尔·莫雷拉](https://about.me/gspmoreira)** 是一位热衷于利用数据解决问题的科学家。他是航空技术学院（Instituto Tecnológico de Aeronáutica - ITA）的博士生，研究推荐系统和深度学习。目前，他是 [CI&T的首席数据科学家](https://medium.com/unstructured)，领导团队利用机器学习解决客户的挑战性问题，处理大规模和非结构化数据。

[原文](https://medium.com/unstructured/how-feature-engineering-can-help-you-do-well-in-a-kaggle-competition-part-iii-f67567aaf57c)。经许可转载。

**相关：**

+   [特征工程如何帮助你在Kaggle比赛中表现出色 – 第1部分](/2017/06/feature-engineering-help-kaggle-competition-1.html)

+   [特征工程如何帮助你在Kaggle比赛中表现出色 – 第2部分](/2017/06/feature-engineering-help-kaggle-competition-2.html)

+   [独家：对杰里米·霍华德关于深度学习、Kaggle、数据科学等的采访](/2017/01/exclusive-interview-jeremy-howard-deep-learning-kaggle-data-science.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的IT需求

* * *

### 相关阅读

+   [特征存储峰会2022：关于特征工程的免费会议](https://www.kdnuggets.com/2022/10/hopsworks-feature-store-summit-2022-free-conference-feature-engineering.html)

+   [数据科学项目，帮助你解决实际问题](https://www.kdnuggets.com/2022/11/data-science-projects-help-solve-real-world-problems.html)

+   [5种稀有的数据科学技能，能帮助你找到工作](https://www.kdnuggets.com/5-rare-data-science-skills-that-can-help-you-get-employed)

+   [生成式人工智能如何帮助你改进数据可视化图表](https://www.kdnuggets.com/how-generative-ai-can-help-you-improve-your-data-visualization-charts)

+   [免费Python资源，帮助你成为专家](https://www.kdnuggets.com/free-python-resources-that-can-help-you-become-a-pro)

+   [等级系统如何帮助预测AI成本](https://www.kdnuggets.com/2022/03/level-system-help-forecast-ai-costs.html)
