# 采访：文斯·达利，King.com谈休闲游戏背后的严肃分析

> 原文：[https://www.kdnuggets.com/2015/03/interview-vince-darley-king-analytics-gaming.html](https://www.kdnuggets.com/2015/03/interview-vince-darley-king-analytics-gaming.html)

![文斯·达利](../Images/50953b1575aa75b8dc3141f83d29e70c.png)[**文斯·达利**](http://uk.linkedin.com/in/vincedarley/en)是[**King**](https://king.com/)的副总裁数据分析与商业智能部门，King是一家领先的互动娱乐公司，专注于移动世界，创造了如《糖果传奇》等游戏。King于2014年3月在纽约证券交易所上市（NYSE: KING）。

在数据科学领域拥有超过17年的经验，文斯在King公司监督一个团队，处理所有与分析和商业智能相关的方面，包括下游和上游数据管道、数据仓库、实时和批量报告、细分和实时分析——这些信息在整个业务中用于改善玩家体验。

文斯拥有哈佛大学复杂系统的硕士学位和博士学位，以及剑桥大学数学学士学位。他还是一位出版作者，曾共同撰写《NASDAQ市场模拟：从复杂适应系统科学中获得的主要市场洞见》。

这是我与他的采访：

**[安莫尔·拉杰普罗希特](https://twitter.com/hey_anmol): Q1\. 您处理的游戏和玩家数据的主要特征是什么？从这些数据中获取可操作见解的主要挑战是什么？**

![游戏数据特征](../Images/5ac7723ea422f3412f4a85e1af661e33.png)[**文斯·达利**](http://uk.linkedin.com/in/vincedarley/en)：我们的大多数数据关注玩家在游戏关卡中的尝试——例如分数、星级、时间戳、玩家是否成功或失败，以及导致玩家失败的因素。我们还跟踪玩家对朋友的帮助——例如发送生命或移动——最后，还包括任何财务交易。

我们游戏的一个重要特点是你可以随时随地在任何设备上玩。从数据角度来看，这可能会带来挑战，因为我们需要仔细整合非常多样的数据和标识符，以创建一个清晰的个体玩家画像。这可能很困难，因为数据量（每天10-20亿事件）很大，而且数据（正如所有大数据一样！）是嘈杂的。

但正确处理这些数据非常重要——购买新设备来玩游戏（因此旧设备上玩得不多）和对游戏失去兴趣且几乎不在唯一设备上玩的玩家之间有很大区别。其次，我们当然只是观察行为，但为了使见解具有实际操作性，我们需要推测关于玩家的动机/心理状态，以便采取正确的行动。这是很困难的！

**AR: Q2\. 在King公司，机器学习和预测分析的最常见用例是什么？**

[![king-com-logo](../Images/d8e5ad97b1e6768ff66fb03031fdae5b.png)](https://king.com/)

**VD:** King 真的很关心长期视角，因此最常见的情况是预测或建模游戏/网络功能变更对特定玩家群体的长期客户生命周期价值影响。

另一个常见但非常困难的情况是预测玩家将会停止游戏。这很困难，因为只有在我们能够非常早期地预测到这一点时才有用，例如当玩家仍在玩游戏时。

**AR: Q3\. 在基础设施方面，你们是如何调整数据架构以应对数据的快速增长的？从中有什么重要的经验教训？**

**VD:** 在过去 3-4 年里，我们不得不进行多次调整。![hadoop](../Images/fe23fbc9fa3ac2af492d6a6733d8790d.png)第一次重大变化发生在我们的游戏在 Facebook 平台上取得成功时，我们启用了 Hadoop 以便能够扩展到数百万玩家。Hadoop 在我们经历了 Saga 游戏的爆炸性增长，以及在移动端成功地服务于数亿玩家的过程中表现良好，作为一种相对便宜、有效的“超级油轮”处理今天 2 PB 的压缩数据。我称之为“超级油轮”，因为 Hadoop 是庞大的、坚固的，但不是非常灵活。

因此，一年多前我们在 Hadoop 旁边添加了 Exasol 系统——这是一种非常快速的内存分析数据库——并且使我们的核心处理（例如 ETL）可以更快地完成——将夜间处理时间从 8-12 小时减少到 1-2 小时。这些经验教训很重要。

![exasol](../Images/b6f1ecd6bc32de2006f8a9c659a6aae8.png)首先，在我们的规模下（截至 2014 年第 4 季度的 1.49 亿日活跃用户），没有一种解决方案适合所有情况——需要像我们 Hadoop/Exasol 组合这样的混合解决方案。其次，我们最近让我们的 Hadoop 集群填满得有些过头了，这让它变得意外脆弱。最好留出足够的余量。

在 BI 方面，我们已经使用 QlikView 进行标准化报告多年，这通常效果良好。在我们拥有数百万的历史玩家（其中约有 3.56 亿月活跃用户）的大规模环境下，构建报告以便我们能够快速按 cohort、国家、游戏、细分、获取渠道、平台进行切片和切块可能会遇到问题。QlikView 有时会感觉有点迟缓，并且它的自我感知能力不强（即，它令人惊讶地不提供很多关于监控业务用户报告延迟、加载时间、响应时间等的支持）。但总体而言，我们对它的优缺点与市场上其他东西的比较感到满意。

我们所有的数据基础设施都是内部建设的，尽管我们保持对云服务动态的关注。

**AR: Q4\. A-B 测试常见的问题是什么？你们有什么建议来避免/减轻这些问题？**

**VD:** 好吧，我可以讲上几个小时关于 A-B 测试的内容，所以我们来关注几个核心难点。首先，在我们这个行业（免费游戏），几乎所有的指标都是非常偏斜/长尾的——这意味着少量的玩家很容易扭曲分析结果。其次，我们希望玩家在多个设备上有一致的体验，这使得测试变得复杂。第三，有时效果/影响是微妙的，因此我们可能需要运行实验相当长的时间才能得到确凿的结果。第四，我们希望从一个游戏中学到的东西能够足够有洞察力，以便对其他游戏的决策提供信息，但设计能够告诉你有关特定游戏的 AB 测试比设计能够告诉你关于玩家的测试要容易得多。

![ab-testing](../Images/c471703f2f63dd551d6035bfd4ac5293.png)

建议？雇佣一些优秀的数据科学家来处理统计学的微妙之处！决定你是否需要了解并准确量化“答案”，还是只需足够自信地做出合理决策——这将帮助你决定何时进行短期或长期实验。最后，在实验设计阶段专注于玩家及其体验，而不是仅仅考虑“特性 A 或特性 B 是否能给我更好的指标？”。

**[访谈第二部分](/2015/03/interview-vince-darley-king-game-top-mobile-game.html)**

**相关：**

+   [文斯·达利谈如何成为顶级收入游戏](/2015/03/interview-vince-darley-king-game-top-mobile-game.html)

+   [机器学习元素表解码](/2015/03/machine-learning-table-elements.html)

+   [做机器学习时的 7 个常见错误](/2015/03/machine-learning-data-science-common-mistakes.html)

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速通道进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

### 更多相关话题

+   [猎鹰 LLM：开源 LLM 的新王者](https://www.kdnuggets.com/2023/06/falcon-llm-new-king-llms.html)

+   [ChatGPT 如何工作：聊天机器人背后的模型](https://www.kdnuggets.com/2023/04/chatgpt-works-model-behind-bot.html)

+   [稳定扩散：生成 AI 背后的基本直觉](https://www.kdnuggets.com/2023/06/stable-diffusion-basic-intuition-behind-generative-ai.html)

+   [入门 LLMOps：无缝交互的秘密调料](https://www.kdnuggets.com/getting-started-with-llmops-the-secret-sauce-behind-seamless-interactions)

+   [基于LLM的自主代理的增长](https://www.kdnuggets.com/the-growth-behind-llmbased-autonomous-agents)

+   [数据科学面试指南 - 第2部分：面试资源](https://www.kdnuggets.com/2022/04/data-science-interview-guide-part-2-interview-resources.html)
