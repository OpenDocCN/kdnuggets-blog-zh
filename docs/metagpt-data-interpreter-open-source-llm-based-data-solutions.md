# 介绍 MetaGPT 的数据解释器：SOTA 开源 LLM 基于的数据解决方案

> 原文：[https://www.kdnuggets.com/metagpt-data-interpreter-open-source-llm-based-data-solutions](https://www.kdnuggets.com/metagpt-data-interpreter-open-source-llm-based-data-solutions)

![MetaGPT 的数据解释器：开源统计建模](../Images/b884eaf5109028b6181e8414c74affc6.png)

图像由作者使用 Midjourney 创建

[MetaGPT](https://github.com/geekan/MetaGPT) 是一个多智能体框架，用于将角色分配给各种智能体，形成协作实体，这些实体能够协调工作以执行复杂指令。MetaGPT 自称为“作为多智能体系统的软件公司”，为你提供这些协作实体的预期用途的想法。MetaGPT 可作为独立应用程序从命令行使用，也可作为库在你自己的 Python 脚本中使用，提供了所需的灵活性和控制。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 工作

* * *

该项目始于 2023 年 4 月，利用 ChatGPT，目前在撰写时 GitHub 上已有近 40K 星标。其 GitHub 仓库进一步描述如下：

> MetaGPT 接收一行要求作为输入，并输出用户故事/竞争分析/需求/数据结构/API/文档等。
> 
> 在内部，MetaGPT 包括产品经理/架构师/项目经理/工程师。它提供了一个软件公司的整个过程以及精心编排的标准操作程序（SOP）。

![MetaGPT 架构](../Images/ba6c132c70f72e05165594230f6e0f62.png)

MetaGPT 的软件公司多智能体示意图（逐步实施）（来自 [MetaGPT 的 GitHub](https://github.com/geekan/MetaGPT)）

MetaGPT 可用于代码生成、原型设计、项目规划等。它被认可为 [杰出的开源成就](https://www.benchcouncil.org/evaluation/opencs/annual.html)，并且持续成为 GitHub 上的热门仓库。

这就是 MetaGPT。现在让我们深入讨论 [数据解释器](https://arxiv.org/abs/2402.18679)、[Deep Wisdom](https://www.deepwisdom.ai/)'s 最新 MetaGPT 改进和成就。

> 介绍 MetaGPT 数据解释器的完整视频
> 
> 展示如何通过动态规划、工具利用、增强推理和基于经验的验证来应对电力负荷预测挑战。
> 
> 仓库：[https://t.co/xWGS0UF9oW](https://t.co/xWGS0UF9oW)
> 
> 案例：[https://t.co/GhNH54Ahhi](https://t.co/GhNH54Ahhi)… [pic.twitter.com/Xc5aam1TXz](https://t.co/Xc5aam1TXz)
> 
> — MetaGPT (@MetaGPT_) [2024年3月19日](https://twitter.com/MetaGPT_/status/1770131837173870918?ref_src=twsrc%5Etfw)

数据解释器是MetaGPT框架中的另一个成员代理，专注于评估和解决数据相关任务。从论文中：

> 在这项研究中，我们介绍了数据解释器，这是一种通过代码解决问题的解决方案，强调三项关键技术以增强数据科学中的问题解决能力：1) 使用分层图结构进行动态规划，以实现实时数据适应性；2) 动态工具集成，以提高执行期间的代码熟练度，丰富所需的专业知识；3) 反馈中的逻辑不一致性识别，通过经验记录提高效率。[...] 与开源基准相比，它展示了优越的性能，在机器学习任务中表现出显著改进，从0.86提高到0.95。此外，它在MATH数据集上表现出26%的提升，在开放性任务上则取得了112%的显著改善。

这些发现确实令人印象深刻。而且无需盲目相信这些结果，因为他们已经发布了这些结果。Deep Wisdom还提供了[大量示例](https://docs.deepwisdom.ai/main/en/DataInterpreter/)，以展示他们的数据解释器代理如何与现有的MetaGPT框架一起使用。

[这个示例](https://docs.deepwisdom.ai/main/en/DataInterpreter/detail.html?id=NVIDIAStockTrendAnalysis)展示了它如何用于NVIDIA股票趋势分析。要查看MetaGPT数据解释器提示的样子，我将在下面重复一遍：

> 从Yahoo Finance获取NVIDIA Corporation (NVDA)的股票价格数据，重点关注过去5年的历史收盘价。总结统计数据（均值、中位数、标准差等）以了解收盘价的集中趋势和离散程度。分析数据以发现任何明显的趋势、模式或异常情况，可能使用滚动平均或百分比变化。创建一个图表来可视化所有的数据分析。保留20%的数据集用于验证。对训练集训练预测模型。报告模型的验证准确率，并可视化预测结果。

你可以查看上面链接的示例笔记本，以跟随MetaGPT的过程并查看结果。剧透：Deep Wisdom没有分享这些结果，因为它们并不令人印象深刻 :)

阅读[完整论文](https://arxiv.org/abs/2402.18679)以获取所有相关信息。有关安装和使用的更多信息，请查看项目的[GitHub仓库](https://github.com/geekan/MetaGPT)。根据我的经验，我可以证明MetaGPT是一个值得查看的项目，并且随着数据解释器代理的加入，这一点比以前更加真实。

[](https://www.linkedin.com/in/mattmayo13/)****[马修·梅奥](https://www.kdnuggets.com/wp-content/uploads/./profile-pic.jpg)**** ([**@mattmayo13**](https://twitter.com/mattmayo13)) 拥有计算机科学硕士学位和数据挖掘研究生文凭。作为[KDnuggets](https://www.kdnuggets.com/)与[Statology](https://www.statology.org/)的执行编辑，以及[Machine Learning Mastery](https://machinelearningmastery.com/)的特约编辑，马修致力于使复杂的数据科学概念变得易于理解。他的专业兴趣包括自然语言处理、语言模型、机器学习算法以及探索新兴的人工智能。他的使命是让数据科学社区中的知识变得更加民主化。马修从6岁起就开始编程。

### 更多相关主题

+   [介绍Objectiv：开源产品分析基础设施](https://www.kdnuggets.com/2022/06/objectiv-introducing-objectiv-opensource-product-analytics-infrastructure.html)

+   [介绍OpenLLM：开源LLM库](https://www.kdnuggets.com/2023/07/introducing-openllm-open-source-library-llms.html)

+   [介绍MPT-7B：一种新的开源LLM](https://www.kdnuggets.com/2023/05/introducing-mpt7b-new-opensource-llm.html)

+   [封闭源与开源图像注释](https://www.kdnuggets.com/closed-source-vs-open-source-image-annotation)

+   [ChatGPT代码解释器：在几分钟内完成数据科学](https://www.kdnuggets.com/2023/07/chatgpt-code-interpreter-data-science-minutes.html)

+   [你可以用ChatGPT的代码解释器进行数据科学的5种方式](https://www.kdnuggets.com/2023/08/5-ways-chatgpt-code-interpreter-data-science.html)
