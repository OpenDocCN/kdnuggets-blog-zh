# 为什么自动化特征工程将改变你的机器学习方式

> 原文：[https://www.kdnuggets.com/2018/08/automated-feature-engineering-will-change-machine-learning.html](https://www.kdnuggets.com/2018/08/automated-feature-engineering-will-change-machine-learning.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者 [William Koehrsen](https://twitter.com/koehrsen_will)，Feature Labs**

![图片](../Images/1aaf5ba4e7a7004049c3debb7a5c0f6c.png)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT

* * *

数据科学中少有确定性——库、工具和算法随着更好方法的发展不断变化。然而，有一个趋势不会消失，那就是向*更高水平的自动化*的转变。

近年来，[模型选择的自动化](https://epistasislab.github.io/tpot/api/)和[超参数调整](https://github.com/automl)取得了进展，但机器学习流程中[最重要的方面](https://homes.cs.washington.edu/~pedrod/papers/cacm12.pdf)，[特征工程](https://www.kdnuggets.com/2018/12/feature-engineering-explained.html)，却在很大程度上被忽视了。这个关键领域中最有能力的条目是[Featuretools](https://docs.featuretools.com/#minute-quick-start)，一个开源Python库。本文将使用这个库，探讨自动化特征工程如何改善你的机器学习方式。

![](../Images/10c12c4dcee741fb89cad8717eb726dc.png)

Featuretools是一个用于自动化特征工程的开源Python库。自动化特征工程是一种相对较新的技术，但在用它解决了多个数据科学问题后，我相信它应该成为任何机器学习工作流的*标准*部分。这里我们将查看这些项目中的两个结果和结论，完整的[代码可在GitHub上作为Jupyter笔记本获取](https://github.com/Featuretools/Automated-Manual-Comparison)。

每个项目都突显了自动化特征工程的一些好处：

+   **贷款偿还预测：** 自动化特征工程相比于手动特征工程可以将机器学习开发时间缩短10倍，同时提供更好的建模性能。 ([笔记本](https://github.com/Featuretools/Automated-Manual-Comparison/tree/master/Loan%20Repayment))

+   **零售支出预测：** 自动化特征工程创建有意义的特征，并通过内部处理时间序列过滤器来防止数据泄露，从而实现成功的模型部署。 ([笔记本](https://github.com/Featuretools/Automated-Manual-Comparison/tree/master/Retail%20Spending))

随意深入代码并尝试Featuretools！ (完全透明：我为[Feature Labs](https://www.featurelabs.com/)工作，该公司开发了这个库。这些项目是使用Featuretools的免费开源版本完成的。)

### **特征工程：手动与自动化**

[特征工程](https://en.wikipedia.org/wiki/Feature_engineering)是将数据集转换为可用于训练机器学习模型的解释性变量——特征——的过程。通常，数据分布在多个表格中，需要将这些数据汇集到一个包含观察结果的行和特征的列的单一表格中。

传统的特征工程方法是使用领域知识逐一构建特征，这是一种繁琐、耗时且容易出错的过程，称为手动特征工程。手动特征工程的代码是*问题依赖性的*，*必须为每个新的数据集重新编写。*

自动化特征工程通过从一组相关数据表中自动提取*有用且有意义*的特征，并使用一个可以*应用于任何问题*的框架来改进这一标准工作流程。它不仅减少了特征工程所花费的时间，而且创建了可解释的特征，并通过过滤时间相关的数据来防止数据泄露。

> 自动化特征工程比手动特征工程更高效且可重复，使你能够更快地构建更好的预测模型。

### 贷款偿还：更快地构建更好的模型

面对家庭信贷贷款问题（一个正在[Kaggle上进行的机器学习竞赛](https://www.kaggle.com/c/home-credit-default-risk)，目标是预测贷款是否会被客户偿还），数据科学家主要面临的难题是数据的规模和分布。查看完整数据集，你会发现*5800万*行数据分布在七个表格中。机器学习需要一个单一的表格用于训练，因此特征工程意味着将每个客户的所有信息整合到一个表格中。

![](../Images/5b75cf347c83b5be9c654ee9726f78c8.png)

特征工程需要将一组相关表中的所有信息捕捉到一个表中。我第一次尝试这个问题时使用了传统的手动特征工程：我花了总共**10小时**手动创建了一组特征。首先，我阅读了[其他数据科学家的工作](https://www.kaggle.com/c/home-credit-default-risk/kernels)，探索数据，并研究了问题领域以获得必要的领域知识。然后我将知识转化为代码，一次构建一个特征。作为一个手动特征的示例，我找到了客户在以前贷款上的逾期总数，这一操作需要使用3个不同的表。

最终手动工程的特征表现相当不错，相对于基准特征（相对于最高排行榜分数），实现了65%的改进，表明了[适当特征工程的重要性](https://homes.cs.washington.edu/~pedrod/papers/cacm12.pdf)。

然而，低效甚至无法描述这个过程。对于手动特征工程，我最终每个特征花费了超过15分钟，因为我使用了逐个特征制作的传统方法。

![](../Images/b5a4fa499842a9d508e2bbf236732acf.png)

手动特征工程过程。除了繁琐和耗时，手动特征工程还包括：

+   **问题特定：**我花了很多小时编写的所有代码都不能应用于*任何其他问题*

+   **易出错：**每行代码都是犯错的机会

此外，最终的手动工程特征受限于**人类创造力**和**耐心**：我们能构思的特征数量有限，我们有的时间也有限。

> 自动特征工程的承诺在于通过利用一组相关表格并自动构建数百个有用的特征，来超越这些限制，这些代码可以应用于所有问题。

### **从手动到自动特征工程**

如Featuretools中所实现的，自动特征工程使得像我这样的领域新手也能从一组相关的数据表中创建数千个相关特征。我们需要知道的只是表的基本结构和它们之间的关系，我们在一个名为[实体集](https://docs.featuretools.com/generated/featuretools.EntitySet.html)的数据结构中跟踪这些关系。一旦我们有了实体集，使用一种名为[深度特征合成](https://www.featurelabs.com/blog/deep-feature-synthesis/)（DFS）的方法，我们能够在*一次函数调用*中构建数千个特征。

![](../Images/8233ab01f8616439d3a2fc6b064fc404.png)

使用 Featuretools.DFS 的自动化特征工程过程通过称为 [“primitives”](https://docs.featuretools.com/automated_feature_engineering/primitives.html)的函数来聚合和转换我们的数据。这些原语可以简单到对列取均值或最大值，也可以复杂到基于主题专业知识，因为 [Featuretools 允许我们定义自己的自定义原语](https://docs.featuretools.com/guides/advanced_custom_primitives.html)。

特征原语包括我们通常会手动执行的许多操作，但使用 Featuretools 时，我们可以在任何关系数据库中使用*完全相同*的语法，而不是重新编写代码以应用这些操作。此外，当我们将原语堆叠在一起以创建深层特征时，DFS 的强大功能才会显现。（有关 DFS 的更多信息，请查看 [this blog post](https://www.featurelabs.com/blog/deep-feature-synthesis/) ，由该技术的发明者之一撰写。）

> 深度特征合成是灵活的——允许它应用于任何数据科学问题——也是强大的——通过创建深层特征揭示我们数据中的洞察。

我将省略设置所需的几行代码，但 DFS 的操作在一行代码中完成。在这里，我们使用数据集中所有 7 个表为每个客户生成*数千个特征*（`ft` 是导入的 featuretools 库）：

```py
# Deep feature synthesis
feature_matrix, features = ft.dfs(entityset=es, 
                                  target_entity='clients',
                                  agg_primitives = agg_primitives,
                                trans_primitives = trans_primitives)
```

以下是我们从 Featuretools 自动获得的一些**1820 个特征**：

+   客户在以往贷款上的最大总支付额。这是使用 `MAX` 和 `SUM` 原语跨越 3 个表创建的。

+   客户在以往信用卡债务的百分位排名。这使用了一个 `PERCENTILE` 和一个 `MEAN` 原语跨越 2 个表。

+   客户在申请过程中是否提交了两份文件。这使用了一个 `AND` 转换原语和 1 个表。

这些特征中的每一个都是通过简单的聚合构建的，因此是可以被人理解的。Featuretools 创建了许多我手动实现的相同特征，但也创造了数千个我从未想到过的特征——或没有时间实现的特征。并非每个特征对问题都相关，有些特征高度相关，不过，*特征过多总比特征过少是一个更好的问题*！

在进行了一些特征选择和模型优化后，这些特征在预测模型中的表现略优于手动特征，整体开发时间为**1小时**，相比手动过程减少了 10 倍。Featuretools 更快，因为它需要的领域知识更少，并且需要编写的代码行数也大大减少。

我承认，学习 Featuretools 有一点时间成本，但这是一个值得的投资。学习 Featuretools 大约需要一个小时，你可以将其应用于*任何机器学习*问题。

以下图表总结了我在贷款还款问题上的经验：

![Image](../Images/d1ad6837257f31146ded86b94dda4e60.png)

自动化与手动特征工程在时间、特征数量和性能方面的比较。

+   开发时间：完成最终特征工程代码所需的一切：**10 小时手动 vs 1 小时自动**

+   方法生成的特征数量：**30 个特征手动 vs 1820 个自动**

+   相对基线的改进是相对于基线的百分比增益，与使用特征训练的模型在公开排行榜上的最佳分数相比：**65% 手动 vs 66% 自动**

> 我的收获是，自动化特征工程不会取代数据科学家，而是通过显著提高效率，使她能够将更多时间用于机器学习流程的其他方面。

此外，我为第一个项目编写的 Featuretools 代码可以应用于任何数据集，而手动工程代码则必须丢弃并为下一个数据集完全重写！

### 零售支出：构建有意义的特征并防止数据泄漏

对于第二个数据集，[在线时间戳客户交易记录](https://archive.ics.uci.edu/ml/datasets/online+retail#)，预测问题是将客户分类为两个群体：那些在下个月花费超过 $500 的客户和那些不会花费的客户。然而，与其为所有标签使用单个月份，每个客户的标签*会出现多次*。我们可以使用他们在五月的支出作为标签，然后是六月，以此类推。

![](../Images/40c4b456b5b3b1fc2729f740921fb539.png)

每个客户作为训练样本被多次使用。使用每个客户作为观察对象多次会带来创建训练数据的困难：在为某个月的客户制作特征时，我们不能使用未来月份的信息，*即使我们可以访问这些数据*。在部署中，我们*永远无法获得未来数据*，因此不能用于训练模型。公司通常会面临这个问题，并经常部署在现实世界中表现远逊于开发阶段的模型，因为它是使用无效数据训练的。

幸运的是，确保我们在时间序列问题中的数据有效性是[在 Featuretools 中很简单的](https://docs.featuretools.com/automated_feature_engineering/handling_time.html)。在 Deep Feature Synthesis 函数中，我们传入如上所示的数据框，其中截止时间表示我们不能用于标签的数据点，Featuretools 在构建特征时会*自动*考虑时间。

某个月的客户特征是使用过滤到该月之前的数据构建的。请注意，创建我们特征集合的调用与贷款还款问题中的调用相同，只是多了`cutoff_time`。

```py
# Deep feature synthesis
feature_matrix, features = ft.dfs(entityset=es, 
                                  target_entity='customers',
                                  agg_primitives = agg_primitives,
                                  trans_primitives = trans_primitives,
                                  cutoff_time = cutoff_times)
```

运行深度特征合成的结果是一个特征表，每个客户*每个月*都有一个。我们可以利用这些特征训练一个带有标签的模型，然后对任何月份进行预测。此外，我们可以放心，模型中的特征不会使用未来的信息，这会导致不公平的优势和误导性的训练得分。

使用自动特征，我能够构建一个机器学习模型，其ROC AUC达到了0.90，相比之下，基线模型（预测与前一个月相同的花费水平）的ROC AUC为0.69，用于预测一个月的客户花费类别。

除了提供令人印象深刻的预测性能外，Featuretools的实现还给了我同样宝贵的东西：可解释的特征。请查看随机森林模型中最重要的15个特征：

![](../Images/ff083e61483b98ff52a2818ffa34a613.png)

从随机森林模型中提取的15个最重要的Featuretools特征。特征重要性告诉我们，预测客户下个月花费多少的最重要指标是他们之前的花费`SUM(purchases.total)`以及购买次数`SUM(purchases.quantity)`。这些特征本可以手动构建，但那样我们就得担心数据泄漏，并且可能在开发时表现良好，但在实际应用中效果却差。

如果已经存在工具来创建有意义的特征，而无需担心特征的有效性，那么为什么还要手动实现呢？此外，自动特征在问题背景中完全清晰，可以指导我们的现实世界推理。

> 自动特征工程识别了最重要的信号，实现了数据科学的主要目标：揭示隐藏在大量数据中的洞察。

即便在手动特征工程上花费了比使用Featuretools更多的时间，我也未能开发出接近同等性能的特征集。下面的图表显示了在使用两个数据集训练的模型上对未来一个月客户销售进行分类的ROC曲线。曲线越向左上方表示预测越好：

![](../Images/b4281a47906f49d0918ad72243c61fc4.png)

比较自动特征工程和手动特征工程结果的ROC曲线。曲线越向左上方表示性能越好。我甚至不完全确定手动特征是否使用了有效的数据，但使用Featuretools的实现，我不必担心时间相关问题中的数据泄漏。也许无法手动工程出有用的有效特征反映了我作为数据科学家的不足，但如果工具可以安全地为我们完成这项工作，为什么不使用呢？

> 我们在日常生活中使用自动安全系统，而 Featuretools 中的自动特征工程是构建有意义的时间序列机器学习特征的安全方法，同时提供优越的预测性能。

### 结论

我从这些项目中深信，自动特征工程应该成为*机器学习工作流程的一个不可或缺的部分*。尽管技术尚不完美，但仍能显著提升效率。

主要结论是自动特征工程：

+   **将实施时间减少**多达 10 倍

+   实现了与**同等水平**或**更佳**的建模**性能**

+   提供了**具有现实世界意义的可解释特征**

+   **防止不当的数据使用**，以免使模型失效

+   **融入现有工作流程**和机器学习模型

> “聪明地工作，而不是更努力”可能是个陈词滥调，但有时候这些空话中确实包含真理：如果有一种方法可以用更少的时间投入获得相同的结果，那么显然这是值得学习的方法。

[Featuretools](https://www.featuretools.com/) 将始终免费使用并且开源（欢迎贡献），还有几个例子 — [这是我写的一篇文章](https://towardsdatascience.com/automated-feature-engineering-in-python-99baf11cc219) — 可以在 10 分钟内让你入门。你的数据科学工作是安全的，但通过自动特征工程可以显著简化工作。

如果你关心构建有意义的高性能预测模型，那么可以通过 [Feature Labs](https://www.featurelabs.com/contact/) 联系我们。尽管这个项目是使用开源的 Featuretools 完成的，但[商业产品](https://www.featurelabs.com/product)提供了额外的工具和支持来创建机器学习解决方案。

**简介：[Will Koehrsen](https://twitter.com/koehrsen_will)** 是 Feature Labs 的数据科学家，同时也是数据科学的传播者和倡导者。

[原文](https://towardsdatascience.com/why-automated-feature-engineering-will-change-the-way-you-do-machine-learning-5c15bf188b96)。已获许可转载。

**相关：**

+   [深度特征合成：自动特征工程如何运作](/2018/02/deep-feature-synthesis-automated-feature-engineering.html)

+   [使用 fast.ai 快速特征工程](/2018/03/feature-engineering-dates-fastai.html)

+   [特征选择的基本概念](/2017/11/rapidminer-basic-concepts-feature-selection.html)

### 更多相关话题

+   [忘记 ChatGPT，这款新的 AI 助手遥遥领先，将…](https://www.kdnuggets.com/2023/08/forget-chatgpt-new-ai-assistant-leagues-ahead-change-way-work-forever.html)

+   [Feature Store Summit 2022：一个关于特征工程的免费会议](https://www.kdnuggets.com/2022/10/hopsworks-feature-store-summit-2022-free-conference-feature-engineering.html)

+   [机器学习中的特征工程实用方法](https://www.kdnuggets.com/2023/07/practical-approach-feature-engineering-machine-learning.html)

+   [构建可处理的特征工程管道用于多变量…](https://www.kdnuggets.com/2022/03/building-tractable-feature-engineering-pipeline-multivariate-time-series.html)

+   [使用 RAPIDS cuDF 利用 GPU 进行特征工程](https://www.kdnuggets.com/2023/06/rapids-cudf-leverage-gpu-feature-engineering.html)

+   [特征工程入门](https://www.kdnuggets.com/feature-engineering-for-beginners)
