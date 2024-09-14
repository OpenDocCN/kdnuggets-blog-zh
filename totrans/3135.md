# Comet.ml —— 机器学习实验管理

> 原文：[https://www.kdnuggets.com/2018/04/comet-ml-machine-learning-experiment-management.html](https://www.kdnuggets.com/2018/04/comet-ml-machine-learning-experiment-management.html)

赞助帖子。

**由 Gideon Mendels，数据科学家兼 CEO，[Comet.ml](https://www.comet.com/)**

在过去十年里，机器学习已经从学术界显著转向了工业界。大量数据集、计算资源和重大投资的结合使研究人员能够在大多数机器学习基准测试中推动最先进的结果。

然而，我们缺乏管理机器学习团队和流程的基本工具。

在过去的一年里，我们与来自各种公司和研究机构的200多位数据科学家进行了简短的访谈。我们询问了他们的流程和团队动态。虽然我们正在努力发布一篇全面的分析帖子，但有一个特别的数据点引起了我们的注意：

![Comet F1 Pie](../Images/d18877ba0320608364d5d52840b6e62e.png)

大多数数据科学家不会跟踪他们的前期工作。机构和个人数据科学家都面临着失去实验结果的风险。

如果公司没有跟踪其结果，它如何保留其机构知识？

根本问题不是最佳实践或缺乏纪律，而是当前工具的状态。GIT 和其他代码版本控制系统是为软件设计的，而非机器学习。它们不旨在跟踪数据集、结果和其他工件。

当涉及到分享实验、维护知识以及发布可重复工作的时，无论是在公共领域还是公司内部，我们还未能找到最佳实践。

这篇文章介绍了 comet.ml —— 一个强调协作和知识共享的机器学习实验跟踪平台。它允许你**保持当前工作流程**，并从任何机器上报告你的结果 —— 无论是你的笔记本电脑还是 GPU 集群。

你的宝贵训练数据也将安全保存，永远不会离开你的机器。

**它是如何工作的？**

要使用 [Comet.ml](https://www.comet.com/)，只需在你的 Python 脚本中添加几行代码（R 支持即将推出！）。

例如，考虑下面这段简短的 Keras 代码：

![Comet F3 code](../Images/09b885b2d3a80ef358fe0e1a5bd6fa11.png)

每次执行此脚本时，以下内容将**自动**（但以可配置的方式）报告到你的私人仪表板：

1.  **超参数**：命令行参数和模型定义中的所有内容将自动报告。可以轻松添加额外的超参数。

1.  **指标**：来自 ML 库代码的实时图表，展示了你模型的收敛情况。额外的指标可以轻松自定义。

1.  **代码**（可选）: 你的源代码的快照（单个脚本或 Github 仓库差异），将结果和代码关联在一起。

1.  **标准输出/错误**: 无需通过 SSH 远程访问机器即可显示的异常或输出。

1.  **模型结构**:  你模型的序列化版本。

1.  **依赖项**: 所有 Python 依赖项及其版本。

1.  **数据集哈希**: 与数据集关联的哈希值，本地计算以避免基础数据更改的情况。

![Comet F2 概述](../Images/9a7e396fb4f2960a9fac4954c05cef7b.png)

**协作**

Comet.ml 上的项目可以与你的整个团队、单个同事或全世界公开分享。通过提供上面收集的信息的可见性，我们希望使可重复性更容易。合作者还可以对你的工作发表评论和提问。

**实验文档**

你可以为每个实验或单个项目添加丰富的文档（Markup/Latex）。

**比较和分析**

你可以比较实验，并将实验信息提取到本地 Pandas 数据框中。这对于查看超参数的重要性和相关性可能很有用。

![Comet F4 比较](../Images/a42d11bdf16c1e80774cae25a788a2cc.png)

**Github 集成**

我们的大多数用户在大型软件导向的组织中工作。通过 Comet.ml，你可以轻松创建一个 GitHub 拉取请求，将表现最佳的模型提交到任何 git 分支。

**接下来是什么？**

我们正在开发的新功能将于未来几周内发布：

+   **超参数分析** -  提供不同超参数之间的重要性和相关性的测量，包括高级可视化。

+   **绘图生成器** -  创建自定义图表和图形以可视化度量、结果、混淆矩阵，甚至神经网络激活图。

+   **生产监控** - 提供模型漂移、训练数据和推理数据分布之间的差异等测量。

Comet.ml 对于公共项目和学术用途完全免费。如果你想在提交之前先试用私有项目，我们还提供 30 天的试用期。我们希望 Comet.ml 能对你的团队有所帮助，并期待你的反馈。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 需求

* * *

### 更多相关主题

+   [7 种最佳机器学习实验追踪工具](https://www.kdnuggets.com/2023/02/7-best-tools-machine-learning-experiment-tracking.html)

+   [数据科学中实验设计的重要性](https://www.kdnuggets.com/2022/08/importance-experiment-design-data-science.html)

+   [KDnuggets 新闻，5月18日：5 个免费机器学习托管平台](https://www.kdnuggets.com/2022/n20.html)

+   [机器学习模型管理](https://www.kdnuggets.com/2022/07/machine-learning-model-management.html)

+   [通过数据科学学习实现免费的数据管理：CS639](https://www.kdnuggets.com/2023/01/free-data-management-data-science-learning-cs639.html)

+   [使用 AI 进行供应链管理的 5 种方法](https://www.kdnuggets.com/2022/02/5-ways-ai-supply-chain-management.html)
