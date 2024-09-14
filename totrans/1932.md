# 在Kaggle数据科学竞赛中使用集成方法——第2部分

> 原文：[https://www.kdnuggets.com/2015/06/ensembles-kaggle-data-science-competition-p2.html](https://www.kdnuggets.com/2015/06/ensembles-kaggle-data-science-competition-p2.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](/2015/06/ensembles-kaggle-data-science-competition-p1.html#comments)**由 Henk van Veen 提供**

**堆叠泛化与融合**

平均预测文件是简单易行的，但这不是顶级Kagglers所使用的唯一方法。[重复码]( https://www.kaggle.com/users)的真正收益始于堆叠和融合。抓紧你的高帽和衬裙：这里有龙。七头的。站在其他30条龙的上面。

**Netflix**[![此图展示了Netflix排行榜结果，融合了数百个预测模型](../Images/5430890728ebc281f0fbace948acb835.png)](/wp-content/uploads/compiling-blending-predictive-models-by-netflix-engineers.jpg)

融合数百个预测模型，最终跨越终点线。

Netflix组织并推广了第一个数据科学竞赛。电影推荐挑战中的参赛者真正推动了集成方法的前沿，甚至可能使Netflix决定不在生产中实施获胜的解决方案，因为它实在太复杂了。

尽管如此，这一挑战仍产生了大量论文和新方法：

+   [特征加权线性堆叠](http://arxiv.org/pdf/0911.0460.pdf)

+   [为准确的推荐系统结合预测](http://elf-project.sourceforge.net/CombiningPredictionsForAccurateRecommenderSystems.pdf )

+   [BigChaos解决Netflix奖的方案](http://www.netflixprize.com/assets/GrandPrize2009_BPC_BigChaos.pdf )

当你想提高你的Kaggle表现时，这些都是有趣、易读且相关的读物。

**堆叠泛化**

堆叠泛化由Wolpert在1992年的一篇论文中引入，早于Breiman在1994年发表的开创性论文“Bagging Predictors”。Wolpert还因另一条非常流行的机器学习定理而闻名：

*“搜索和优化中没有免费的午餐”。*

堆叠泛化的基本思想是使用一组基础分类器，然后使用另一个分类器来组合它们的预测，旨在减少泛化误差。

假设你想进行2折堆叠：

+   将训练集拆分为两部分：`train_a`和`train_b`。

+   在`train_a`上拟合第一个阶段的模型，并为`train_b`创建预测。

+   在`train_b`上拟合相同的模型，并为`train_a`创建预测。

+   最后，在整个训练集上拟合模型，并为测试集创建预测。

+   现在在第一个阶段模型的概率上训练第二阶段的堆叠模型。

堆叠模型通过使用第一阶段的预测作为特征，比在孤立训练时获得更多的问题空间信息。

**融合**

混合是Netflix获胜者引入的一个词。它与堆叠泛化非常接近，但更简单，且信息泄漏的风险较小。一些研究者将“堆叠集成”和“混合”互换使用。

使用混合时，你不是为训练集创建外折预测，而是创建一个例如10%的训练集的持出集。然后堆叠模型仅在这个持出集上进行训练。

混合有一些好处：

+   它比堆叠更简单。

+   它防止信息泄漏：泛化器和堆叠器使用不同的数据。

+   你不需要与队友分享分层折叠的种子。任何人都可以将模型放入“混合器”，然后混合器决定是否保留该模型。

然而，缺点是：

+   你总体上使用的数据更少。

+   最终模型可能会对持出集过拟合。

+   使用堆叠（通过更多折计算）的简历比使用单个小的持出集要更扎实。

就性能而言，两种技术能提供相似的结果，似乎是个人偏好和技能的差异。作者更喜欢堆叠。

如果你无法选择，你可以同时进行两个。创建具有堆叠泛化和外折预测的堆叠集成。然后使用持出集进一步在第三阶段组合这些模型，我们将会在下一部分探讨。

[**在Kaggle数据科学比赛中使用集成方法 - 第1部分**](/2015/06/ensembles-kaggle-data-science-competition-p1.html)

[**在Kaggle数据科学比赛中使用集成方法 - 第3部分**](/2015/06/ensembles-kaggle-data-science-competition-p3.html)

你计划如何实现你学到的东西？分享你的想法吧！

原文：[**Kaggle集成指南**](http://mlwave.com/kaggle-ensembling-guide/) 作者：Henk van Veen。

**相关内容：**

+   [如何在不读取数据的情况下领导数据科学比赛](/2015/05/data-science-contest-leaderboard-without-reading-data.html)

+   [前20名R机器学习和数据科学包](/2015/06/top-20-r-machine-learning-packages.html)

+   [Netflix: 主管 - 产品分析、数据科学与工程](/jobs/14/08-11-netflix-director-product-analytics-data-science-engineering.html)

### 更多相关主题

+   [Kaggle比赛对实际问题有用吗？](https://www.kdnuggets.com/are-kaggle-competitions-useful-for-real-world-problems)

+   [5个免费比赛供有志的数据科学家](https://www.kdnuggets.com/5-free-competitions-for-aspiring-data-scientists)

+   [通过参加比赛学习机器学习快4倍](https://www.kdnuggets.com/2022/01/learn-machine-learning-4x-faster-participating-competitions.html)

+   [7个免费Kaggle微课程，适合数据科学初学者](https://www.kdnuggets.com/7-free-kaggle-micro-courses-for-data-science-beginners)

+   [前10名Kaggle机器学习项目，助你在2024年成为数据科学家](https://www.kdnuggets.com/top-10-kaggle-machine-learning-projects-to-become-data-scientist-in-2024)

+   [在Kaggle上竞争的4个技巧及为何你应该开始](https://www.kdnuggets.com/2022/05/packt-top-4-tricks-competing-kaggle-start.html)
