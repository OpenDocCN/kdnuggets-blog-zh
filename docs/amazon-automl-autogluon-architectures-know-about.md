# 亚马逊通过 AutoGluon 进入 AutoML 竞赛：一些你应该了解的 AutoML 架构

> 原文：[https://www.kdnuggets.com/2020/01/amazon-automl-autogluon-architectures-know-about.html](https://www.kdnuggets.com/2020/01/amazon-automl-autogluon-architectures-know-about.html)

[评论](#comments)

![](../Images/466796caf75fa6aedf36c2f6388cdf4c.png)

几天前，[亚马逊宣布发布 AutoGloun](https://www.amazon.science/amazons-autogluon-helps-developers-get-up-and-running-with-state-of-the-art-deep-learning-models-with-just-a-few-lines-of-code)，一个新工具包，通过仅仅几行代码简化了深度学习模型的创建。这次发布标志着亚马逊进入了竞争激烈的自动化机器学习（AutoML）领域，而这一领域正成为企业机器学习平台的热门趋势。在 AutoML 生态系统的新闻如此之多的情况下，有时很难从中分辨出真正的重要信息。今天，我想探索一些市场上最具创新性的 AutoML 堆栈，这些堆栈并未获得太多的宣传。

AutoML 正成为现代数据科学应用中最热门的话题之一。人们常常将 AutoML 视为一种无需复杂数据科学知识即可使用现成机器学习模型的机制。虽然从理论上讲，这种说法是有道理的，但现实情况却有所不同。在当前的人工智能（AI）阶段，大多数现实世界的应用需要一定程度的机器学习知识。使用如 Watson Developer Cloud 或 Microsoft Cognitive Services 这样的基础 API 可以解决的场景非常基本，只占机器学习场景广泛范围的一小部分。如果是这样的话，那么我们应该考虑 AutoML 的真正价值是什么。

### **挑战**

模型选择是构建机器学习解决方案中最困难的方面之一。有些讽刺的是，尽管机器学习应用中融入了大量科学和数学，模型选择仍然是一个高度主观的任务，依赖于专家的意见。在任何给定的场景中，能够解决该场景的机器学习模型数量都非常庞大，那么我们如何真正知道我们是否在使用最优模型呢？更糟糕的是，即使我们选择了正确的机器学习技术，我们如何确定我们拥有正确的神经网络架构？而一旦我们确定了具体的架构，我们又如何知道正确的超参数配置？这些问题在整个机器学习应用生命周期中困扰着数据科学家。此外，对机器学习问题所需的准确度越高，模型选择过程花费的时间就越多。

毫不奇怪，选择和构建机器学习模型的过程是一个极其耗时的工作，且从未提供过确切的答案。矛盾的是，这正是机器学习擅长的领域，那么我们能否发挥创意，将选择机器学习架构的过程建模为一个机器学习问题呢？

![](../Images/30250374f9d2e5369e35395cd2ea3f7d.png)

### **AutoML 来拯救**

模型搜索是一个看起来非常适合 AutoML 的应用场景。给定一个数据集、一系列优化指标以及一些时间或资源的限制，AutoML 方法应该能够评估成千上万的神经网络架构并产生最佳结果。虽然有效的数据科学团队可能只能评估几个模型来解决特定问题，但 AutoML 方法可以在相对可控的时间内快速搜索成千上万的架构。

使用机器学习来构建更好的机器学习模型似乎像是铁人电影中的情节 ???? 这在现实世界中真的发生了吗？绝对发生了！以下是一些我最喜欢的 AutoML 在关键任务应用中的高调案例研究。

### AutoGluon

让我们从最新的成员开始：亚马逊的 AutoGluon。功能上，AutoGluon 是一个开源库，供开发者在处理涉及图像、文本或表格数据集的机器学习应用时使用。AutoGluon 提供易于使用和扩展的 AutoML，专注于深度学习及涵盖图像、文本或表格数据的实际应用。该框架旨在服务于机器学习初学者和高级专家。AutoML 的第一个版本包括以下一些功能：

+   用少量代码快速原型化深度学习解决方案。

+   利用自动超参数调整、模型选择/架构搜索和数据处理。

+   自动使用最先进的深度学习技术，无需专家知识。

+   轻松改进现有的定制模型和数据管道，或根据你的使用场景定制 AutoGluon。

![](../Images/3d6858c64ef74d69fcc7f711b6364bf4.png)

### Salesforce TransmogrifAI：Einstein 背后的大脑

Salesforce.com 的 Einstein 是全球应用最广泛的机器学习应用之一。最终，Einstein 解决了一系列机器学习场景，如销售预测或潜在客户优先级排序，这些场景在销售和营销应用中无处不在。然而，Einstein 的独特之处在于其机器学习模型能够在完全不同的 Salesforce.com 配置下以自助服务的方式运行。每个客户可能有完全不同的销售和营销模式，但 Einstein 仍然可以完成工作。

Salesforce 的 Einstein 背后的魔力由一个名为 [TransmogrifAI](https://transmogrif.ai/) 的开源框架提供支持。从概念上讲，[TransmogrifAI](https://transmogrif.ai/) 是一个基于 AutoML 的框架，用于针对结构化数据（行和列）创建机器学习模型。更具体地说，[TransmogrifAI](https://transmogrif.ai/) 在机器学习工作流程的五个基本领域中利用了 AutoML：

+   **特征推断：** 从给定数据集中提取特征。

+   **变形：** 将特征转换为数值。

+   **特征验证：** 减少维度，识别潜在偏差等。

+   **模型选择：** 在数千个潜在模型中进行搜索。

+   **超参数优化：** 调整超参数配置。

![](../Images/2cac4189f392b5b628a35fdb36b031c1.png)

鉴于 Salesforce.com 的影响，[TransmogrifAI](https://transmogrif.ai/) 可能被认为是全球最大的 AutoML 应用之一。

### Azure ML：帮助开发者选择合适的机器学习模型

去年，微软研究院进行了一项实验，利用 AutoML 和概率编程来自动化模型选择。结果 [被记录在一篇非常受欢迎的研究论文中](https://arxiv.org/abs/1705.05355)，并在性能上代表了一项突破。在短短几个月内，微软研究团队开创的 AutoML 方法被应用于微软的旗舰机器学习产品：Azure ML。

Azure ML 的最新版本利用 AutoML 简化模型选择。该平台包括一个 AutoML 服务，定期推荐新的机器学习管道以解决特定问题。管道的执行在客户的 Azure ML 实例中进行，而 AutoML 服务仅查看结果并使用这些结果做出更好的推荐。

![](../Images/9bca60b0bae5f46b1f60131690961040.png)

Azure ML 堆栈中的 AutoML 实现是我见过的最完整的之一。当前版本支持对数值和文本数据进行分类和回归 ML 模型推荐，支持自动特征生成（包括缺失值填补、编码、归一化和基于启发式的方法的特征）、特征转换和选择。开发者可以通过 Python SDK 或 Jupyter Notebooks 使用 AutoML。

### Waymo：自动化模型选择用于自动驾驶汽车

自动驾驶汽车就像是一大群安装在四个轮子上的机器学习模型 ????。机器学习使自动驾驶汽车具备所有智能功能，如帮助车辆识别周围环境、理解世界、预测他人行为并决定最佳行动方案。字母表公司的子公司 Waymo 处于自动驾驶汽车技术的最前沿，因此在机器学习领域不断创新。

最近，Waymo 工程团队 [发布了详细的博客文章，介绍了他们如何利用 AutoML 自动化不同机器学习应用中的模型选择](https://medium.com/waymo/automl-automating-the-design-of-machine-learning-models-for-autonomous-driving-141a5583ec2a)。具体而言，Waymo 团队利用了一种名为 [NAS 单元](https://arxiv.org/pdf/1707.07012.pdf) 的 AutoML 技术，这在图像分析算法中已被证明非常有效。

在 Waymo，AutoML 被用于探索卷积网络架构（CNN）中的数百种不同 NAS 单元组合，训练和评估 Waymo 的 LiDAR 分割任务模型。实验已产生出具有 20%-30% 更低延迟和 8%-10% 更低错误率的 CNN 架构，相较于手工设计的模型。

![](../Images/68c5466a545be6017694592546a929c6.png)

从这些例子中可以看出，AutoML 正在成为高度可扩展机器学习架构中的重要元素之一。亚马逊的进入无疑是推动 AutoML 成为机器学习架构关键组件的又一推动力。AutoGluon 是另一个例子，说明用于模型搜索的 AutoML 工具和框架正在不断改进，并逐渐向主流开发者开放。虽然 AutoML 有其他很好的使用案例，但在现实世界机器学习应用中，模型选择仍然是带来最大好处的驱动力。

[原文](https://towardsdatascience.com/amazon-gets-into-the-automl-race-with-autogluon-some-automl-architectures-you-should-know-about-aceaaccaeb8b)。经授权转载。

**相关：**

+   [AutoML 调查结果：如果你尝试它，你会更喜欢它](/2020/01/poll-automl-results.html)

+   [自动化机器学习：团队如何在 AutoML 项目中协作？](/2020/01/teams-work-together-automl-project.html)

+   [AutoML 对时序关系数据：一个新前沿](/2019/10/automl-temporal-relational-data.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

### 更多相关主题

+   [宣布一个博客写作比赛，获胜者将获得 NVIDIA GPU！](https://www.kdnuggets.com/2022/11/blog-writing-contest-nvidia-gpu.html)

+   [在深入了解变压器之前你应该知道的概念](https://www.kdnuggets.com/2023/01/concepts-know-getting-transformer.html)

+   [KDnuggets™新闻22:n03，1月19日：深入了解13种数据…](https://www.kdnuggets.com/2022/n03.html)

+   [2023年你应该考虑的顶级AutoML框架](https://www.kdnuggets.com/2023/05/best-automl-frameworks-2023.html)

+   [KDnuggets新闻，4月13日：数据科学家应该了解的Python库…](https://www.kdnuggets.com/2022/n15.html)

+   [它活了！使用Python和一些便宜的组件构建你的第一个机器人](https://www.kdnuggets.com/2023/06/manning-build-first-robots-python-cheap-basic-components.html)
