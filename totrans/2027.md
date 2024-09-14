# 没有数据工程技能的数据科学家将面临严峻的现实

> 原文：[https://www.kdnuggets.com/2021/09/data-scientists-data-engineering-skills.html](https://www.kdnuggets.com/2021/09/data-scientists-data-engineering-skills.html)

[评论](#comments)

![](../Images/a4621c3a66ffadb010efac3b48eab6a1.png)

*照片由 [Ben White](https://unsplash.com/@benwhitephotography?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/s/photos/surprise?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)。*

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

你可能读过关于数据科学家和数据工程师区别的文章。我一直认为这个区别很明确。数据工程师准备好数据，然后数据科学家在这些数据上工作。

然而，经过我作为数据科学家的工作，我对这一区别的看法发生了剧烈变化。

数据科学的一切都始于数据。你的机器学习模型的质量与输入的数据同样重要。垃圾进，垃圾出！没有合适的数据，数据科学家无法通过某种魔法创造有价值的产品。

合适的数据并不总是立即可用。大多数情况下，将原始数据转换为适当格式的责任将落在数据科学家身上。

除非你在一家大型科技公司工作，该公司有专门的数据工程师和数据科学家团队，否则你应该具备处理一些数据工程任务的能力和技能。这些任务涵盖了广泛的操作，我将在文章的其余部分详细阐述。

### 毕竟有什么区别呢？

我想阐述一下数据工程师的工作和数据科学家之间的关系。

> 数据工程师就是数据工程师。数据科学家应该既是数据科学家，又是数据工程师。

这可能看起来是一个有争议的说法。然而，我想强调的是，在我开始作为数据科学家工作之前，我的观点是不同的。我曾经认为数据工程师和数据科学家是两个独立的实体。

在文章的剩余部分，我将尝试解释为什么数据科学家应该既是数据科学家又是数据工程师。

比如说，数据工程师会进行一系列称为 ETL（提取、转换、加载）的操作。这包括从一个或多个来源收集数据，应用一些转换，然后将其加载到另一个来源中。

如果数据科学家被期望执行 ETL 操作，我绝对不会感到惊讶。数据科学仍在发展中，大多数公司没有明确分开的数据工程师和数据科学家角色。因此，数据科学家应该能够执行一些数据工程任务。

如果你期望仅仅在使用现成数据运行机器学习算法，你会在刚开始工作时就面临严峻的现实。

你可能需要编写一些 SQL 存储过程来预处理客户端数据。也有可能你会从几个不同的来源收到客户端数据。你的工作将是提取和组合这些数据。然后，你需要将它们加载到一个单一的来源中。为了编写高效的存储过程，你需要广泛的 SQL 技能。

ETL 程序的转换部分涉及许多数据清理和操作步骤。如果你处理大规模数据，SQL 可能不是最佳选择。此时，分布式计算是更好的替代方案。因此，数据科学家也应熟悉分布式计算。

在分布式计算中，你的最佳伙伴可能是 Spark。它是一个用于大规模数据处理的分析引擎。我们可以将数据和计算分布在集群上，以实现显著的性能提升。

如果你熟悉 Python 和 SQL，那么适应 Spark 不会很困难。你可以使用 PySpark，这是一种 Spark 的 Python API。

关于集群工作，最佳环境是云。虽然有多种云提供商，但 AWS、Azure 和 Google Cloud Platform（GCP）领先于前。

虽然 PySpark 代码在所有云提供商中都是相同的，但环境设置和集群创建方式有所不同。它们允许通过脚本或用户界面创建集群。

在集群上进行分布式计算是一个完全不同的世界。它与在你的计算机上进行分析完全不同。它具有非常不同的动态。评估集群性能和选择集群的最佳工作节点数量将是你主要关注的问题。

### 结论

长话短说，数据处理将成为你作为数据科学家的重要部分。我所说的重要，是指你80%以上的时间都将用于数据处理。数据处理不仅仅是清理和操作数据。它还包括 ETL 操作，这通常被认为是数据工程师的工作。

我强烈推荐熟悉 ETL 工具和概念。如果你有机会进行实践，那将非常有帮助。

认为作为数据科学家你只会处理机器学习算法是一个天真的想法。这确实是一个重要的任务，但它只会占用你时间的一小部分。

[原文](https://towardsdatascience.com/data-scientists-without-data-engineering-skills-will-face-the-harsh-truth-ff482a223ddc)。转载已获许可。

**相关：**

+   [数据科学在未来10年不会消失，但你的技能可能会](https://www.kdnuggets.com/2021/06/data-science-not-becoming-extinct-10-years.html)

+   [数据工程师最重要的工具](https://www.kdnuggets.com/2021/08/most-important-tool-data-engineers.html)

+   [数据科学家、数据工程师及其他数据职业解析](https://www.kdnuggets.com/2021/05/data-scientist-data-engineer-data-careers-explained.html)

### 更多相关话题

+   [成为出色数据科学家需要的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每位初学者数据科学家应掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [建立一个稳固的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)

+   [使用管道编写干净的Python代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [如何在没有任何工作经验的情况下获得你的第一份数据科学工作](https://www.kdnuggets.com/2021/02/first-job-data-science-without-work-experience.html)
