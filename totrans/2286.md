# 处理推荐和搜索中的位置偏差

> 原文：[https://www.kdnuggets.com/2023/03/dealing-position-bias-recommendations-search.html](https://www.kdnuggets.com/2023/03/dealing-position-bias-recommendations-search.html)

人们更常点击搜索和推荐中的顶部项目，是因为它们在顶部，而不是因为它们的相关性。如果你用机器学习模型来排序搜索结果，它们可能会因为这种正向自我增强的反馈循环而最终质量下降。这个问题如何解决？

# 排名中的偏差

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT

* * *

每次你向人们展示列表，如搜索结果或推荐时，我们很难公正地评估列表中的所有项目。

![处理推荐和搜索中的位置偏差](../Images/e19a71c400cd938cf225370850923471.png)

项目排名无处不在。

一个 [级联点击模型](https://www.semanticscholar.org/paper/An-experimental-comparison-of-click-position-bias-Craswell-Zoeter/1ba36a2a1609cf09e2a08eb4712555848d5d728c) 假设人们在找到相关项之前会顺序评估列表中的所有项。但这意味着底部的项目被评估的机会较小，因此自然点击较少：

![处理推荐和搜索中的位置偏差](../Images/2e7edb8662ba3f6f989e173d03097a11.png)

列表中的位置越高？——点击越多。

顶部项目仅因为其位置获得更多点击？——这种行为称为**位置偏差**。然而，位置偏差并不是列表中唯一的偏差，还有很多其他危险因素需要注意：

+   **展示偏差**：例如，由于3x3网格布局，位于第4位（紧接在第1位下方）的项目可能比第3位角落的项目获得更多点击。

+   **模型偏差**：当你用由同一模型生成的历史数据来训练机器学习模型时。

实际上，位置偏差是最强的偏差之一？——去除它可能会提高模型的可靠性。

# 实验：测量位置偏差

我们进行了一项小规模的众包研究关于位置偏差。使用 [RankLens](https://github.com/metarank/ranklens) 数据集，我们用 [Google 关键词规划师](https://ads.google.com/home/tools/keyword-planner/) 工具生成了一系列查询以查找每部特定电影。

![处理推荐和搜索中的位置偏差](../Images/249f75f099d86dba77d2f6a3ae7801e8.png)

滥用Google关键字规划师获取人们用来寻找电影的真实查询。

通过一组电影和对应的实际查询，我们拥有一个完美的搜索评估数据集？——所有项目都为更广泛的观众所熟知，我们提前知道正确标签。

所有主要的众包平台，如 [Amazon Mechanical Turk](https://www.mturk.com/)、[Scale.com](https://scale.com/) 和 [Toloka.ai](https://toloka.ai/) 都有针对典型搜索评估的现成模板：

![处理推荐和搜索中的位置偏差](../Images/88a42b1a6926e6a3dc460003015e66dc.png)

一个典型的 [搜索排名评估模板](https://toloka.ai/en/docs/template-builder/)。

但这些模板中有一个巧妙的技巧，防止你因位置偏差而自找麻烦：每个项目必须独立检查。即使屏幕上出现多个项目，它们的排序也是随机的！但随机的项目顺序是否能阻止人们点击第一个结果？

实验的原始数据可以在 [github.com/metarank/msrd](https://github.com/metarank/msrd) 上找到，但主要观察结果是**即使在随机排序的项目上，人们仍然更倾向于点击第一个位置**！

![处理推荐和搜索中的位置偏差](../Images/2e1ecb462c90ad95cd534aacbd1055c5.png)

即使在随机排名中，第一个项目的点击次数也更多。

# 逆向倾向加权

但你如何抵消点击的隐性反馈中位置的影响？每次你测量一个项目的点击概率时，你观察到的是两个独立变量的组合：

+   **偏差**：点击列表中特定位置的概率。

+   **相关性**：项目在当前上下文中的重要性（例如来自ElasticSearch的BM25得分和推荐中的余弦相似度）

在前一段提到的MSRD数据集中，很难将位置的影响与BM25相关性独立区分，因为你只能观察到它们的组合：

![处理推荐和搜索中的位置偏差](../Images/c4c4bbd8903d1fcbf0323e14c6ea84b9.png)

按BM25排序时，人们更喜欢相关项目。

例如，18%的点击发生在第1位。这只是因为我们在这里展示了最相关的项目吗？如果将相同的项目放在第20位，是否会获得相同数量的点击？

[逆向倾向加权](https://arxiv.org/abs/1608.04468)方法表明，某个位置的观察点击概率只是两个独立变量的组合：

![处理推荐和搜索中的位置偏差](../Images/e8e6f96d9f6535adcf8fe361113a7de2.png)

真实的相关性是否独立于位置？

然后，如果你估算每个位置的点击概率（倾向性），你可以用它来加权所有相关性标签，并获得实际的无偏相关性：

![处理推荐和搜索中的位置偏差](../Images/830ebf60b98faa14e196324f7735eda8.png)

通过倾向性加权

但在实践中如何估算倾向性？最常见的方法是对排名进行轻微的随机排序，这样在相同上下文中（例如搜索查询）相同的项目会在不同的位置上进行评估。

![处理推荐和搜索中的位置偏差](../Images/08616038c65c5c57ad67831e8031af39.png)

通过随机排序来估算倾向性。

但额外的随机排序肯定会降低你的业务指标，如点击率和转化率。是否有不涉及随机排序的更少侵入性的替代方法？

![处理推荐和搜索中的位置偏差](../Images/b1b1ddd9d13976a75764f545f945a169.png)

来自 MICES’19 演讲的幻灯片[实时个性化搜索结果](https://www.youtube.com/watch?v=AYdOpfY8jQU)：搜索结果随机排序时转化率下降 2.8%！

# 位置感知学习

一个[位置感知的排名方法](https://www.semanticscholar.org/paper/PAL%3A-a-position-bias-aware-learning-framework-for-Guo-Yu/4eb8bb576a5603b82c27d7f40e508b7dc8c1c0cc)建议让你的机器学习模型同时优化排名相关性和位置影响：

+   在训练时，你使用项目位置作为输入特征，

+   在预测阶段，你用一个**常量**值来替代它。

![处理推荐和搜索中的位置偏差](../Images/62291befb6c882510da1651b44375f6e.png)

在推断过程中用常量替代偏差因素

换句话说，你让你的排名机器学习模型在训练时检测位置如何影响相关性，但在预测时将这一特征归零：所有项目同时呈现在相同位置上。

![处理推荐和搜索中的位置偏差](../Images/5a123555f3a5afe1ff2d3a3ea732d1a0.png)

v

但你应该选择哪个常量值？[PAL](https://www.semanticscholar.org/paper/PAL%3A-a-position-bias-aware-learning-framework-for-Guo-Yu/4eb8bb576a5603b82c27d7f40e508b7dc8c1c0cc) 论文的作者做了一些数值实验来选择最佳值——经验法则是不选择过高的位置，因为噪声太多。

![处理推荐和搜索中的位置偏差](../Images/495e2391d7d8b5e3d9a2cebeeb84f108.png)

[PAL](https://www.semanticscholar.org/paper/PAL%3A-a-position-bias-aware-learning-framework-for-Guo-Yu/4eb8bb576a5603b82c27d7f40e508b7dc8c1c0cc) 的作者测试了不同的位置常量值

# 实用的PAL

PAL 方法已经成为多个开源推荐和搜索工具的一部分：

+   [ToRecSys](https://github.com/p768lwy3/torecsys) 实现了 PAL 作为一种偏差消除方法，以在有偏数据上训练推荐系统。

+   [Metarank](https://github.com/metarank/metarank) 可以使用基于 PAL 的特征来训练一个无偏的 LambdaMART Learn-to-Rank 模型。

由于位置感知方法仅仅是特征工程中的一个小窍门，在 [Metarank](https://github.com/metarank/metarank) 中，只需再添加一个特征定义即可：

![处理推荐和搜索中的位置偏差](../Images/9d3c0714d0aa703729e7b5e9541b2c3d.png)

将 [位置](https://docs.metarank.ai/reference/overview/feature-extractors/relevancy#position) 作为 Learn-to-Rank 模型的排名特征

在上述 MSRD 数据集上，这种受 PAL 启发的排名特征相比其他排名特征具有较高的 SHAP 重要性值。

![处理推荐和搜索中的位置偏差](../Images/d3f30503972ffee1dfbc5eb671c512a6.png)

训练 LambdaMART 模型时位置的重要性

# 结束语

位置感知学习方法不仅限于纯排名任务和位置去偏：你可以使用这个技巧来克服任何其他类型的偏差：

+   对于由于网格布局造成的**展示偏差**，你可以在训练过程中引入一对特征，表示项目的行和列位置。但在预测时将它们交换为常数。

+   对于**模型偏差**，当展示更多的项目获得更多的点击时？——你可以引入一个“点击次数”训练特征，并在预测时用一个常数值替换它。

使用 PAL 方法训练的机器学习模型应产生无偏的预测。

**[Roman Grebennikov](https://www.linkedin.com/in/romangrebennikov/)** 是 Delivery Hero SE 的首席工程师，专注于搜索个性化和推荐。他是功能编程、Learn-to-Rank 模型和性能工程的务实爱好者。

### 更多相关内容

+   [处理文本数据中的噪声标签](https://www.kdnuggets.com/2023/04/dealing-noisy-labels-text-data.html)

+   [使用网格搜索和随机搜索进行超参数调优（Python）](https://www.kdnuggets.com/2022/10/hyperparameter-tuning-grid-search-random-search-python.html)

+   [通过 Uplimit 的机器学习搜索课程提升您的搜索引擎技能！](https://www.kdnuggets.com/2023/10/uplimit-elevate-your-search-engine-skills-search-with-ml-course)

+   [构建视觉搜索引擎 - 第 2 部分：搜索引擎](https://www.kdnuggets.com/2022/02/building-visual-search-engine-part-2.html)

+   [在 3 分钟内理解偏差-方差权衡](https://www.kdnuggets.com/2020/09/understanding-bias-variance-trade-off-3-minutes.html)

+   [偏差-方差权衡](https://www.kdnuggets.com/2022/08/biasvariance-tradeoff.html)
