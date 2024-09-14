# 思维图谱：大型语言模型中复杂问题解决的新范式

> 原文：[https://www.kdnuggets.com/graph-of-thoughts-a-new-paradigm-for-elaborate-problem-solving-in-large-language-models](https://www.kdnuggets.com/graph-of-thoughts-a-new-paradigm-for-elaborate-problem-solving-in-large-language-models)

![思维图谱：大型语言模型中复杂问题解决的新范式](../Images/64427c89afa9e69d0bb7407300ba5d50.png)

# 关键要点

+   思维图谱（GoT）是一个新颖的框架，旨在增强大型语言模型（LLMs）在复杂问题解决任务中的提示能力。

+   GoT通过将LLM生成的信息表示为图，超越了现有的范式，如思维链（CoT）和思维树（ToT），从而实现了更灵活和高效的推理。

+   该框架在任务性能上显示了显著改进，包括排序质量提高了62%，与思维树相比，成本降低了31%以上。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织IT

* * *

> 这项工作使LLM推理更接近人类思维或脑机制，如递归，这两者都形成了复杂的网络。

# 介绍

人工智能快速发展的背景下，出现了越来越复杂的大型语言模型（LLMs），能够处理各种任务。然而，持续的挑战之一是提高这些模型高效解决复杂问题的能力。进入[思维图谱（GoT）](https://arxiv.org/abs/2308.09687)，这一框架希望在这方面取得巨大进展。GoT通过将生成的信息结构化为图，从而提高了LLMs的提示能力，实现更复杂和灵活的推理。

尽管像链式思维（CoT）和思维树（ToT）等现有范式已对 LLM 的结构化输出和层次推理做出了贡献，但它们往往在线性或树状结构的限制下运行。这种限制有时会阻碍模型处理需要多维推理和整合不同信息片段的复杂问题。思维图通过引入图形结构来管理“LLM 思考”，解决了这一问题。这为信息在模型内的存储、访问和操作提供了前所未有的灵活性。通过 GoT，开发者和研究人员可以细化提示策略，以有效地导航这一图形，从而使 LLM 能够以更类似人类的方式解决复杂问题。

# 理解思维图

思维图基于一个简单却强大的概念：它将 LLM 生成的信息建模为图形，其中每个顶点代表一个信息单元，通常称为“LLM 思考”。这些顶点之间的边表示不同思维单元之间的依赖关系或联系。这种基于图形的方法允许：

+   将任意的 LLM 思考组合成和谐的结果

+   提炼复杂思维网络的本质

+   利用反馈循环强化思维

与现有的如 CoT 和 ToT 等范式相比，GoT 提供了一种更灵活高效的方式来管理和操作 LLM 生成的信息。

![GoT 过程比较图](../Images/ac1f50e79f940d0de84691bc31a6464e.png)

**图 1**：思维图（GoT）与其他提示策略的比较（图片来自论文）

# 实施思维图

要实现 GoT，开发者需要将问题解决过程表示为图形，其中每个节点或顶点代表一个思考或一条信息。然后，这些思考之间的关系或依赖被映射为图中的边。这种映射允许进行各种操作，例如合并节点以创建更复杂的思考，或应用转换以增强现有的思考。

GoT 的一个显著特点是其可扩展性，使其能够适应各种任务和领域。与更僵化的结构不同，GoT 中的图形表示在问题解决过程中可以动态调整。这意味着当 LLM 生成新的思考或获得额外的见解时，这些可以无缝地融入现有图形中，而无需完全重做。

此外，GoT 实现了反馈循环的机制，在这种机制下，模型可以根据新获得的信息重新审视和完善其早期的思考。这种动态的迭代过程显著提升了模型输出的质量，使其成为一个特别强大的工具，适用于需要持续改进和调整的复杂任务。

# 结论

GoT的引入可能标志着LLMs领域及其在复杂问题解决任务中的应用的重要进展。通过采用基于图的方法来表示和操控LLMs生成的信息，GoT提供了一种更灵活和高效的推理形式。它在提高任务表现和降低计算成本方面的成功，使其成为未来研究和应用的有前途的框架。开发人员和研究人员应探索这一新范式，以尝试解锁LLMs的全部问题解决潜力，并改进其提示。

[](https://www.linkedin.com/in/mattmayo13/)****[马修·梅约](https://www.kdnuggets.com/wp-content/uploads/./profile-pic.jpg)**** ([**@mattmayo13**](https://twitter.com/mattmayo13)) 拥有计算机科学硕士学位和数据挖掘研究生文凭。作为 [KDnuggets](https://www.kdnuggets.com/) 和 [Statology](https://www.statology.org/) 的主编，以及 [Machine Learning Mastery](https://machinelearningmastery.com/) 的特约编辑，马修致力于使复杂的数据科学概念变得易于理解。他的专业兴趣包括自然语言处理、语言模型、机器学习算法和探索新兴的人工智能。他的使命是使数据科学社区的知识民主化。马修从6岁开始编程。

### 更多相关内容

+   [顶级开源大型语言模型](https://www.kdnuggets.com/2022/09/john-snow-top-open-source-large-language-models.html)

+   [更多关于大型语言模型的免费课程](https://www.kdnuggets.com/2023/06/free-courses-large-language-models.html)

+   [了解大型语言模型](https://www.kdnuggets.com/2023/03/learn-large-language-models.html)

+   [介绍来自John Snow Labs的医疗专用大型语言模型](https://www.kdnuggets.com/2023/04/john-snow-introducing-healthcare-specific-large-language-models-john-snow-labs.html)

+   [大型语言模型是什么，如何运作？](https://www.kdnuggets.com/2023/05/large-language-models-work.html)

+   [人工智能：大型语言模型与视觉模型](https://www.kdnuggets.com/2023/06/ai-large-language-visual-models.html)
