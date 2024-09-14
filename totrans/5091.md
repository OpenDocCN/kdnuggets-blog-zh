# Algorithmia 测试：人工 vs 自动标签生成

> 原文：[https://www.kdnuggets.com/2015/04/algorithmia-tested-automated-tag-generation.html/2](https://www.kdnuggets.com/2015/04/algorithmia-tested-automated-tag-generation.html/2)

**结论 – 标签**

结果很有趣。对于这些标签来说，写文章时进行的许多分类似乎都是由 Algorithmia 自动提取的。例如，KDnuggets 上的 [新闻](/news/index.html) 类别中的大多数文章都被赋予了 **新闻** 标签。此外，自动生成的标签有时非常具体。例如，标签“treenet”是为这些 2014 年 5 月的文章 [May](/2014/05/video-series-data-mining-for-statisticians.html) 和 2014 年 1 月的文章 [January](/2014/01/data-mining-for-beginners-boot-camp-salford-video-series.html) 生成的，相隔五个月。我预计人工编辑不会做出这些连接，这显示了机器生成标签的一个巨大好处。

表格 1: 按频率排序的标签

| 排名 | 手动生成 | 机器生成 |
| --- | --- | --- |
| 1 | 大数据 (2.0%) | 数据 (5.2%) |
| 2 | 机器学习 (1.8%) | 分析 (2.4%) |
| 3 | Hadoop (1.4%) | 数据分析 (1.9%) |
| 4 | 数据科学 (1.4%) | 大 (1.7%) |
| 5 | 深度学习 (1.2%) | 新闻 (1.6%) |
| 6 | 采访 (1.0%) | 大数据 (1.5%) |
| 7 | R (1.0%) | 数据新闻 (1.3%) |
| 8 | 数据挖掘 (0.9%) | 挖掘 (1.2%) |
| 9 | 数据科学 (0.9%) | 科学 (1.2%) |
| 10 | 分析 (0.8%) | 数据挖掘 (1.2%) |

上表描绘了机器生成和手动标签在一定程度上存在差异。许多顶级标签相似，如 **数据科学**、**数据挖掘** 和 **大数据**。

> 但是，项目集生成的标签往往更倾向于常见标签。

“数据”一项单独占这些标签的 5.2%。这使得一些机器生成的标签稍显不那么有用。但这也与使用常见标签作为帖子的高层次分类有关。注意，“KDnuggets”在机器标签中是一个非常常见的标签，但由于用站点名称标记 KDnuggets 的帖子显得多余，因此被移除了。

自动标签生成的一个弱点是无法捕捉到那些不在大多数停用词列表中的无关标签。最明显的例子是链接缩短器 `buff.ly` 和 `shrd.by`，它们在热门的 Twitter 帖子中很常见。除此之外，**大**、**数据**、**科学**、**挖掘**、**分析** 和 **社交** 的每一种排列组合都有可能出现，这也部分归因于数据清理。为了使自动生成的标签更具吸引力，需要一个更具网站特定性的停用词列表。

考虑到这一点，我认为最佳的工作流程应该是在人工标记的基础上，结合机器标记的方式。也许每个月在整个语料库上离线重新运行标记算法，以便在发布时，能够揭示人类标记者可能不会立即想到的文章之间的联系。

**结论 – Algorithmia**

更有趣的结果是使用 Algorithmia 的体验。整体体验非常愉快。拥有使用 Algorithmia 支持的语言（Python、Java 或 JavaScript）的经验，实际上是使用它的唯一前提。这是一个好事，因为它表明他们将服务设置得与现有生态系统良好兼容，这对于确保你尽快提高生产力至关重要。

在线编辑器的工作方式符合语言的预期，Python 编辑器速度很快且提供了良好的自动完成。访问围绕你的算法创建的 API 非常简单，而且很多示例生成已经为你完成。有一个过程的部分不太清楚——将算法从私有转为公开——但在尝试发布新版本并看到选项后，这一点很容易搞清楚。

观看这个平台的发展将是很有趣的。我可以想象研究论文会发布它们算法的在线版本作为一种演示。这将是很棒的，并且会让我作为读者印象深刻。我还可以看到公司使用 Algorithmia 将一些工作转移到云端。例如，如果一个算法需要间歇性地运行（以避免需要自己的硬件），但可能需要按需调用，Algorithmia 将是一个不错的选择。与 Algorithmia 合作是一种愉快的经历，我会毫不犹豫地向朋友推荐它。

**相关：**

+   [Algorithmia – 市场如何促进创新？](/2015/04/algorithmia-marketplace-innovation.html)

+   [KDnuggets 对分析市场的评估：大数据的下一个大趋势](/2013/11/kdnuggets-review-analytics-marketplaces-next-big-thing-for-big-data.html)

+   [调查：机器学习 API](/2015/04/poll-machine-learning-apis.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 加快您的网络安全职业发展。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT

* * *

### 更多相关主题

+   [检索增强生成：信息检索如何与文本生成相结合](https://www.kdnuggets.com/retrieval-augmented-generation-where-information-retrieval-meets-text-generation)

+   [Bark：终极音频生成模型](https://www.kdnuggets.com/2023/05/bark-ultimate-audio-generation-model.html)

+   [人工智能的未来：探索下一代生成模型](https://www.kdnuggets.com/2023/05/future-ai-exploring-next-generation-generative-models.html)

+   [揭开 Midjourney 5.2 的面纱：AI 图像生成的飞跃](https://www.kdnuggets.com/2023/06/unveiling-midjourney-52-leap-forward.html)

+   [文本到视频生成：一步步指南](https://www.kdnuggets.com/2023/08/text2video-generation-stepbystep-guide.html)

+   [如何通过检索增强生成使大型语言模型更智能](https://www.kdnuggets.com/how-retrieval-augment-generation-makes-llms-smarter)
