# 我的秘密武器，帮助我在 Kaggle 比赛中进入前 2%

> 原文：[https://www.kdnuggets.com/2018/11/secret-sauce-top-kaggle-competition.html](https://www.kdnuggets.com/2018/11/secret-sauce-top-kaggle-competition.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由 [Abhay Pawar](https://www.linkedin.com/in/abhayspawar/), Instacart**。

参加 Kaggle 比赛很有趣且上瘾！在过去的几年里，我开发了一些标准方法来探索特征并构建更好的机器学习模型。这些简单但强大的技术帮助我在 [Instacart 市场篮子分析](https://www.kaggle.com/c/instacart-market-basket-analysis) 比赛中获得了前 2% 的排名，我在 Kaggle 之外也使用这些技术。让我们直接进入正题！

* * *

## 我们的前 3 名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

在数值数据上构建任何监督学习模型的最重要方面之一是充分理解特征。查看模型的部分依赖图有助于你了解模型输出如何随特征变化。

![](../Images/07c9b6de56eb09c892fd33c2bfcca060.png)

**[来源](http://scikit-learn.org/stable/auto_examples/ensemble/plot_partial_dependence.html)**

但这些图的问题在于它们是使用训练模型创建的。如果我们能够直接从训练数据创建这些图，它将有助于我们更好地理解基础数据。实际上，它可以帮助你完成以下所有任务：

1.  特征理解

1.  识别噪声特征 (**最有趣的部分！**)

1.  特征工程

1.  特征重要性

1.  特征调试

1.  泄漏检测和理解

1.  模型监控

为了使其易于访问，我决定将这些技术放入 Python 包 [featexp](https://github.com/abhayspawar/featexp)，在本文中，我们将看看如何使用它进行特征探索。我们将使用 Kaggle 上的 [Home Credit Default Risk](https://www.kaggle.com/c/home-credit-default-risk/) 竞赛的应用数据集。该竞赛的任务是使用提供的数据预测违约者。

1.  **特征理解**

![](../Images/ea624a802c8f342df8575ad80e83098b.png)

**特征与目标的散点图无帮助**

如果因变量（目标）是二值的，散点图就不起作用，因为所有点要么在 0，要么在 1。对于连续目标，数据点过多会使理解目标与特征的趋势变得困难。Featexp 创建了更好的图表来帮助解决这个问题。我们来试试吧！

![](../Images/4f370ec9c6877ce1c858a553c8cd5f6a.png)

**DAYS_BIRTH（年龄）的特征与目标图**

Featexp 创建了一个数值特征的等人口区间（X 轴）。然后计算每个区间的目标均值，并将其绘制在上面的左侧图中。在我们的例子中，目标均值就是违约率。图表告诉我们，高负值的 DAYS_BIRTH（较大年龄）的客户违约率较低。这是合理的，因为年轻人通常更容易违约。这些图表帮助我们了解特征对客户的影响以及它如何影响模型。右侧的图表显示了每个区间的客户数量。

1.  **识别噪声特征**

噪声特征会导致过拟合，识别这些特征并不容易。在 featexp 中，你可以传递一个测试集，并比较训练集/测试集中的特征趋势以识别噪声特征。这个测试集不是实际的测试集，而是你的本地测试集/验证集，你对其目标是知道的。

![](../Images/351dde6ae8bc3e0a98dacd8ce5f3f4e2.png)

**训练集和测试集中的特征趋势比较**

Featexp 计算两个指标以显示在这些图表上，这有助于评估噪声。

1.  **趋势相关性**（在测试图中看到）：如果一个特征在训练集和评估集中的趋势不同，则可能导致过拟合。这是因为模型正在学习一些在测试数据中不适用的内容。趋势相关性有助于理解训练/测试趋势的相似程度，计算时使用训练集和测试集中区间的目标均值。上述特征的相关性为 99%。看起来不噪声！

1.  **趋势变化**：趋势方向的突然和重复变化可能暗示噪声。但这种趋势变化也可能发生，因为该区间在其他特征方面有很大不同，因此其默认率无法与其他区间进行比较。

下述特征没有保持相同的趋势，因此其趋势相关性较低，为 85%。这两个指标可以用来去掉噪声特征。

![](../Images/83ad33aa0de9f5870280829884569f80.png)

噪声特征示例

在特征数量较多且它们彼此相关时，去掉低趋势相关性的特征效果很好。这样可以减少过拟合，其他相关特征可以避免信息丢失。同时，也要注意不要去掉过多重要特征，否则可能会导致性能下降。**此外，你不能仅通过特征重要性来识别这些噪声特征，因为它们可能非常重要，但仍然非常嘈杂！**

使用不同时间段的测试数据效果更好，因为这样你可以确保特征趋势随时间保持一致。

***get_trend_stats()***函数在featexp中返回一个包含每个特征趋势相关性和变化的数据框。

![](../Images/d3de62250fd43f434c6f248afa3dda3f.png)

**get_trend_stats()返回的数据框**

让我们实际尝试在数据中丢弃趋势相关性低的特征，看看结果是否有所改善。

![](../Images/2e5ecf19c6de1dc2da562afc954c2ea4.png)

**不同特征选择下的AUC与趋势相关性**

**我们可以看到，丢弃特征的趋势相关性阈值越高，排行榜（LB）AUC越高。** 不丢弃重要特征可以进一步提高LB AUC到0.74。值得注意且令人担忧的是测试AUC没有像LB AUC那样变化。确保验证策略正确，使得本地测试AUC跟随LB AUC也很重要。完整代码可以在[featexp_demo](https://github.com/abhayspawar/featexp/blob/master/featexp_demo.ipynb)笔记本中找到。

1.  **特征工程**

通过查看这些图，你可以获得创建更好特征的洞见。对数据有更好的理解可以导致更好的特征工程。但除此之外，它还可以帮助你改进现有特征。让我们再看一个特征EXT_SOURCE_1：

![](../Images/92d9594de93911d7327e963c608c014d.png)

**EXT_SOURCE_1的特征与目标关系图**

具有高EXT_SOURCE_1值的客户违约率较低。但，第一个区间（~8%的违约率）并没有跟随特征趋势（先上升再下降）。这个区间只有-99.985左右的负值和一个大的人口基数。这可能意味着这些是特殊值，因此不符合特征趋势。幸运的是，非线性模型在学习这种关系时不会有问题。但是，对于像逻辑回归这样的线性模型，这些特殊值和空值（将作为单独的区间显示）应该用具有类似违约率的区间的值进行填补，而不是简单地用特征均值填补。

1.  **特征重要性**

Featexp还可以帮助你评估特征的重要性。DAYS_BIRTH和EXT_SOURCE_1都有较好的趋势。然而，EXT_SOURCE_1的人口集中在特殊值区间，意味着该特征对大多数客户的信息相同，因此不能很好地区分他们。这表明它可能没有DAYS_BIRTH重要。根据XGBoost模型的特征重要性，DAYS_BIRTH实际上比EXT_SOURCE_1更重要。

1.  **特征调试**

查看Featexp的图可以帮助你通过以下两种方式捕捉复杂特征工程代码中的错误：

![](../Images/427300b93d98d42509f93614b0ea11f4.png)

**零变异特征只显示一个区间**

1.  检查特征的总体分布是否正常。我个人因为一些小错误多次遇到类似上图的极端情况。

1.  在查看这些图表之前，始终假设特征趋势会是什么样的。特征趋势看起来与预期不符可能暗示某些问题。**坦率地说，假设趋势的过程使得构建机器学习模型变得更加有趣！**

1.  **泄漏检测**

目标到特征的数据泄漏会导致过拟合。泄漏特征具有较高的特征重要性。然而，理解特征泄漏的原因是困难的。查看 featexp 图表可以帮助你解决这个问题。

下面的特征在‘空值’分箱中的违约率为 0%，在所有其他分箱中的违约率为 100%。显然，这是泄漏的极端案例。该特征仅在客户违约时有值。根据特征的性质，这可能是由于错误，或者该特征仅为违约者填充（在这种情况下应删除）。**了解泄漏特征的问题有助于更快地进行调试。**

![](../Images/253799c3124bc342cdd142d4037a4b0d.png)

**理解特征为何会泄漏**

1.  **模型监控**

由于 featexp 计算了两个数据集之间的趋势相关性，因此可以轻松用于模型监控。每次模型重新训练时，可以将新的训练数据与经过充分测试的训练数据进行比较（通常是第一次构建模型时的训练数据）。趋势相关性可以帮助你监控特征与目标之间的关系是否发生了变化。

这些简单的操作一直帮助我在实际生活中和在 Kaggle 上构建更好的模型。使用 featexp 只需 15 分钟就能查看这些图表，这绝对值得，因为你在此之后不会盲目操作。

**个人简介**： [Abhay Pawar](https://www.linkedin.com/in/abhayspawar/) 目前在 Instacart 担任高级机器学习工程师，隶属于搜索与发现团队。他处理大规模机器学习问题，帮助 Instacart 提升服务质量。

[原文](https://towardsdatascience.com/my-secret-sauce-to-be-in-top-2-of-a-kaggle-competition-57cff0677d3c)。经许可转载。

**资源：**

+   [在线和基于网络的：分析、数据挖掘、数据科学、机器学习教育](https://www.kdnuggets.com/education/online.html)

+   [分析、数据科学、数据挖掘和机器学习的软件](https://www.kdnuggets.com/software/index.html)

**相关内容：**

+   [数据科学家有多少？是否存在短缺？](https://www.kdnuggets.com/2018/09/how-many-data-scientists-are-there.html)

+   [表格数据深度学习简介](https://www.kdnuggets.com/2018/05/introduction-deep-learning-tabular-data.html)

+   [去 Kaggle 还是不去](https://www.kdnuggets.com/2018/05/to-kaggle-or-not.html)

### 更多相关话题

+   [开始使用 LLMOps：无缝交互的秘诀](https://www.kdnuggets.com/getting-started-with-llmops-the-secret-sauce-behind-seamless-interactions)

+   [GPT-4：一体化的8种模型；秘密已揭晓](https://www.kdnuggets.com/2023/08/gpt4-8-models-one-secret.html)

+   [HuggingGPT：解决复杂AI任务的秘密武器](https://www.kdnuggets.com/2023/05/hugginggpt-secret-weapon-solve-complex-ai-tasks.html)

+   [在Kaggle竞争的四大技巧及为何你应立即开始](https://www.kdnuggets.com/2022/05/packt-top-4-tricks-competing-kaggle-start.html)

+   [2024年成为数据科学家的10个Kaggle机器学习项目](https://www.kdnuggets.com/top-10-kaggle-machine-learning-projects-to-become-data-scientist-in-2024)

+   [最全面的Kaggle解决方案和创意列表](https://www.kdnuggets.com/2022/11/comprehensive-list-kaggle-solutions-ideas.html)
