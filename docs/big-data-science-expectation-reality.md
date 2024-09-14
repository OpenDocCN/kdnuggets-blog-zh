# 大数据科学：期望与现实

> 原文：[https://www.kdnuggets.com/2016/10/big-data-science-expectation-reality.html](https://www.kdnuggets.com/2016/10/big-data-science-expectation-reality.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png)

[comments](#comments)

**由 Stepan Pushkarev, Hydrosphere.io 的 CTO 提供。**

过去几年对于从事分析和大数据工作的人来说就像梦想成真一样。平台工程师有了学习 Hadoop、Scala 和 Spark 的新职业道路。Java 和 Python 程序员有机会进入大数据领域。在那里，他们发现了更高的薪资、新的挑战，并能够扩展到分布式系统。但最近，我开始听到一些工程师对在那里工作感到失望和希望破灭的抱怨。

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 需求

* * *

[![期望与现实](../Images/69d000ea290ac24ff8991b25108a5782.png)](/wp-content/uploads/expectation_vs_reality.png)

他们的期望并不总是实现。大数据工程师将大部分时间花在 ETL 上，为分析师准备数据集，并将数据科学家开发的模型编码成脚本。因此，他们在数据科学家和大数据之间成为了齿轮。

我从数据科学家那里听到相同的反馈。真正的大数据项目与 Kaggle 竞赛不同。即便是小任务也需要 Python 和 Scala 技能，还需与大数据工程师和运维团队多次互动和设计会议。

DevOps 工程师仅能使用基本工具并完成教程。他们对仅仅通过 Cloudera 向导和半自动化的 Ansible 脚本部署下一个 Hadoop 集群感到厌倦。

在这种环境下，大数据工程师永远无法从事可扩展性和存储优化的工作。数据科学家也不会发明新的算法。DevOps 工程师也不会将整个平台置于自动化状态。团队的风险在于，他们的想法可能无法超越概念验证。

我们如何改变这种情况？有 3 个关键点：

1.  **工具演变**——Apache Spark/Hadoop 生态系统很棒。但它不够稳定和用户友好，不能让人完全忽视。工程师和数据科学家应贡献于现有的开源项目，并创建新工具来填补日常操作中的空白。

1.  **教育和跨领域技能**——当数据科学家编写代码时，他们不仅需要考虑抽象问题，还需要考虑实际可行性和合理性。例如，他们需要思考查询运行的时间以及提取的数据是否适合他们使用的存储机制。

1.  **改进流程**——DevOps 可能是解决方案。在这里，DevOps 不仅仅意味着编写 Ansible 脚本和安装 Jenkins。我们需要 DevOps 以最佳方式运作，以减少交接并发明新工具，为每个人提供自助服务，使他们尽可能高效。

### **数据科学家、大数据工程师和业务用户的持续分析环境**

通过持续分析和自助服务，数据科学家可以从最初的构想到生产全过程中掌控数据项目。他们越是自主，就能投入更多时间来产生实际的见解。

数据科学家从最初的业务想法开始，经过数据探索和数据准备，然后进入模型开发。接下来是部署和验证环境，最后推动到生产阶段。拥有合适的工具后，他或她可以在一天内多次完成这个完整的迭代，而无需依赖大数据工程师。

大数据工程师则负责可扩展性和存储优化，开发并贡献于像 Spark 这样的工具，支持流处理架构等。他提供给数据科学家一个 API 和 DSL。

产品工程师应当获得数据科学家开发的整洁打包的分析模型。然后，他们可以为业务用户构建智能应用程序，使他们能够以自助服务的方式使用这些模型。

这样工作，没有人会因为等待下一个人而被困住。每个人都在使用自己的抽象进行工作。

这听起来可能像是一个乌托邦。但我相信这可以激励大数据团队改善他们现有的大数据平台和流程，额外的好处是让每个人都感到满意。

**简介：** [Stepan Pushkarev](https://www.linkedin.com/in/stepanpushkarev) 是 [![Stepan Pushkarev](../Images/87d23f9919ffc864bf4a905a1f6cb836.png)Hydrosphere.io](http://hydrosphere.io) 的首席技术官。Stepan 共同创办并领导了电子商务、物联网和广告技术公司的工程团队。负责从数学模型、基础设施与运营到网络和移动应用程序的完整产品栈，同时负责招聘、建立工程文化和交付流程，他结合了强大的技术、管理和企业家背景。

**相关内容：**

+   [连接数据系统和DevOps](/2016/06/connecting-data-systems-devops.html)

+   [成功的数据科学团队的三个基本组成部分](/2015/08/3-components-successful-data-science-team.html)

+   [健康综合：大数据 DevOps 工程师](/jobs/14/09-18-healthintegrated-big-data-devops-engineer.html)

### 更多相关内容

+   [成为优秀数据科学家所需的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家都应掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [停止学习数据科学以寻找目标，并通过寻找目标来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [建立一个强大的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)
