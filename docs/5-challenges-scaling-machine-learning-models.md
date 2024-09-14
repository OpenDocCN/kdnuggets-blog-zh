# 扩展机器学习模型的 5 个挑战

> 原文：[`www.kdnuggets.com/2020/10/5-challenges-scaling-machine-learning-models.html`](https://www.kdnuggets.com/2020/10/5-challenges-scaling-machine-learning-models.html)

评论

**由 [Sigmoid Analyitcs](https://www.sigmoid.com/) 提供**

![](img/5e7210238b61fc50dad0c9ef0d13f02b.png)

机器学习（ML）模型是为了特定的业务目标而设计的。ML 模型的生产化是指在相关数据集上托管、扩展和运行 ML 模型。生产中的 ML 模型还需要具备对未来变化和反馈的弹性和灵活性。最近的一项研究[Forrester](https://www.rtinsights.com/forrester-ml-development/) 表示，提高客户体验、提升盈利能力和收入增长是组织计划通过 ML 计划实现的关键目标。

尽管获得了全球认可，ML 模型很难转化为实际的商业收益。处理实时数据并将 ML 模型投入生产时，众多工程、数据和业务问题成为瓶颈。根据我们的调查，43% 的人表示在 ML 模型生产和集成过程中遇到瓶颈。确保 ML 模型按照企业预期实现最终目标非常重要，因为全球范围内的组织采纳速度正在以前所未有的速度增长，这得益于强大且廉价的开源基础设施。[Gartner](https://www.gartner.com/smarterwithgartner/gartner-predicts-the-future-of-ai-technologies/) 预测，到 2020 年底，超过 40% 的全球领先组织计划实际部署 AI 解决方案。为了了解生产化 ML 模型中的常见陷阱，让我们深入探讨组织面临的 5 个主要挑战。

### **1\. 数据复杂性**

训练一个 ML 模型需要大约一百万条相关记录，并且这些数据不能是随便的。数据的可行性和预测风险成为问题。评估我们是否拥有相关数据集以及是否能足够快地获取它们来进行预测并不简单。获取上下文数据也是一个问题。在 Sigmoid 与 Yum Brands 进行的 ML 扩展中，一些公司的产品如 KFC（具有新的会员计划）没有足够的客户数据。仅有数据也不够。大多数 ML 团队以非数据湖方法开始，并在传统的数据仓库上训练 ML 模型。在传统的数据系统中，数据科学家通常将 80% 的时间用于清理和管理数据，而不是训练模型。强大的治理系统和数据目录也是必需的，以确保数据透明共享并得到良好的目录，以便再次利用。由于数据复杂性，维护和运行 ML 模型的成本相对于回报随时间递减。

### **2\. 工程与部署**

一旦数据可用，基础设施和技术栈必须根据使用案例和未来的弹性进行最终确定。机器学习系统的工程设计可能非常复杂。机器学习领域提供了广泛的技术选择。在选择每种技术栈时，将其标准化并确保不会使生产化变得更困难对模型的成功至关重要。例如，数据科学家可能会使用像 Pandas 这样的工具并用 Python 编码。但这些工具在生产环境中可能不太适用，Spark 或 Pyspark 更为理想。不当的技术解决方案可能会导致高昂的成本。而且生命周期挑战和管理、稳定多个生产模型也可能变得非常棘手。

### **3\. 集成风险**

一个与不同数据集和建模技术良好集成的可扩展生产环境对于机器学习模型的成功至关重要。集成不同的团队和操作系统总是充满挑战。复杂的代码库必须转化为结构良好的系统，准备推向生产。在缺乏标准化流程将模型推向生产的情况下，团队可能会在任何阶段陷入困境。工作流自动化对于不同团队集成到工作流系统和进行测试是必要的。如果模型在正确的阶段未经过测试，整个生态系统可能需要在最后修复。技术栈必须标准化，否则集成可能会成为真正的噩梦。集成也是一个关键时刻，确保机器学习实验框架不是一次性的奇迹。否则，如果商业环境发生变化或发生灾难性事件，模型将无法提供价值。

### **4\. 测试与模型维护**

测试机器学习模型虽然困难，但与生产过程的其他步骤同样重要，甚至更重要。理解结果、运行健康检查、监控模型性能、留意数据异常以及重新训练模型，这些步骤共同完成整个生产化周期。即使在测试后，可能还需要适当的机器学习生命周期管理工具来发现测试中看不见的问题。

### **5\. 角色分配与沟通**

在数据科学、数据工程、DevOps 和其他相关团队之间保持透明的沟通对机器学习模型的成功至关重要。但角色分配、详细的权限授予和每个团队的监控非常复杂。强有力的协作和过量的沟通对于在早期阶段识别不同领域的风险是必不可少的。让数据科学家深度参与也可以决定机器学习模型的未来。

除了上述挑战外，还需要关注诸如 COVID-19 之类的不可预见事件。当客户的购买行为突然变化时，过去的解决方案不再适用，且缺乏足够的新数据来训练模型成为障碍。扩展机器学习模型并不容易。请关注我们下一篇关于在大规模生产化机器学习模型的最佳实践。

[原文](https://www.sigmoid.com/blogs/5-challenges-to-be-prepared-for-before-scaling-machine-learning-models/)。经许可转载。

**相关：**

+   机器学习模型部署

+   使用 Python 和 Heroku 创建并部署您的第一个 Flask 应用

+   停止训练更多模型，开始部署它们

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

### 更多相关内容

+   [通过 Apache Gobblin 扩展数据管理](https://www.kdnuggets.com/2023/01/scaling-data-management-apache-gobblin.html)

+   [使用 Python 进行数据扩展](https://www.kdnuggets.com/2023/07/data-scaling-python.html)

+   [扩展您的 Web 数据驱动产品时需要知道的事项](https://www.kdnuggets.com/2023/08/things-know-scaling-web-datadriven-product.html)

+   [创建机器学习特征的挑战](https://www.kdnuggets.com/2022/02/challenges-creating-features-machine-learning.html)

+   [企业中的机器学习：用例和挑战](https://www.kdnuggets.com/2022/08/dss-machine-learning-enterprise-cases-challenges.html)

+   [如何应对 3 个常见的机器学习挑战](https://www.kdnuggets.com/2022/09/comet-tackle-3-common-machine-learning-challenges.html)
