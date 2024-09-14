# AI 无限训练与维护循环

> 原文：[`www.kdnuggets.com/2021/11/ai-infinite-training-maintaining-loop.html`](https://www.kdnuggets.com/2021/11/ai-infinite-training-maintaining-loop.html)

评论

**由[Roey Mechrez, Ph.D.](https://roimehrez.github.io/)，BeyondMinds 首席技术官兼联合创始人**。

如果你曾尝试为你的业务推出 AI 解决方案，你可能对以下场景非常熟悉：你花了几周时间收集数据来训练你的 AI 模型，清理数据、增强数据并改善数据。接着，你探索了几种可以适应你 AI 解决方案问题的开源模型和架构。接下来，你可能使用了几种工具来加速开发过程，并创建了一个管道来帮助包装解决方案、调整参数，并比较数百种训练选择。经典的数据科学实验室工作，对吧？寻找完美的模型——开发一种最先进的解决方案来解决 AI 可以帮助解决的问题。

如果一切顺利，训练过程顺利进行，你的模型在测试数据集上的准确率达到 95%。需要一些额外的软件魔法和周末的加班来解决所有未完成的工作，但你终于达到了终点线：将模型投入生产——迎接激进的 Q3 截止日期。

快进三个月。冬天来了。模型效果不佳。与模型互动的业务人员不满意。他们从 AI 中得到的结果没有意义。在一个寒冷的早晨，你收到了来自高层的邮件，标题明确无误：“你的 AI 没有工作。”

这不是一个轶事。实际上，对于许多公司来说，这是一种痛苦的现实，尤其是在涉及到关键任务或操作敏感的应用程序时，例如缺陷检测、索赔自动化、欺诈检测、客户入职、KYC（了解你的客户）、AML（反洗钱）、预测性维护或信用评分。

### 使用真实世界数据训练你的 AI 模型

虽然许多因素和组件可能导致 AI 解决方案在生产中失败，但根本问题深植于这一过程的基本设计中。令人沮丧的现实是，大多数在受控实验室环境中无缝工作的 AI 模型，在面对真实世界环境时却失败了，因为现实世界中的数据往往是嘈杂的、动态的且未标记的。应对这种差异的最有效方式是让你的 AI 模型尽早面对生产数据，即使它仍处于实验室训练阶段。

关键在于在模型已经投入生产后，持续改进和优化它。这种设计将使模型逐步进步，随着用户实时反馈的获得而不断改进。听起来很简单，对吧？与其先构建一个 AI 模型然后再将其商品化，不如保持一个持续的构建-商品化-重新构建循环。不幸的是，创建这种持续的设计周期极其复杂。它涉及研究、协调以及软件与研究的深度结合。

这个循环应从数据标注开始，然后是训练、评估和部署。一旦投入生产，循环将进入监控和收集人类用户反馈的关键步骤。此时，循环完成一个完整的循环。它现在可以重新开始，利用在之前步骤中获得的数据在重新训练步骤中持续改进。正如这个无限形状的循环所示，生产部署不再是终点线，而是为模型增加稳定性和可持续性的起点：

![](img/40b62a2bb1543668f9c5d2b2587d6b43.png)

*训练与维护 AI 无限循环*

### 避免维护陷阱

在产品开发生命周期中，系统维护往往被忽视——这可能对系统的未来产生灾难性的影响。在许多情况下，维护可能成为组织内部扩展系统的严重障碍，阻碍系统的广泛使用和影响力的提升。

对于机器学习和 AI 来说，维护是一个巨大的负担，可能导致 TCO 失控，危及整个用例的盈利性。近年来，AI 技术已经成熟并变得更加普及，诱使各行业的企业尝试从 AI 解决方案中获取价值。然而，严峻的现实是，大约 90%的企业未能实现 ROI 正面的 AI 项目。AI 解决方案的维护，特别是在扩展特定 AI 解决方案时的维护障碍，在这一失败率中扮演了重要角色。这也是为什么那些考虑深入 AI 领域的公司应该将学习和维护循环作为他们启动任何系统的基本构建块的另一个原因。

### 经验教训：常青 AI 是可能的

让我们回到最初的起点：你在实验室中耐心训练的模型并取得了你满意的结果。从生产中崩溃和失败的主要教训是什么？答案是：旨在设计一个能够识别数据变化（偏离最初用于训练的数据集）的系统，收集来自人类用户的持续反馈，并重新训练和部署更新、更好的模型。

**在花费第一个 AI 美元之前，你应该问自己的一些棘手问题：**

1.  我能检测到数据何时发生变化吗？

1.  我能否检测到模型可能出错的情况？

1.  我将如何收集用户反馈？

1.  是否使用了适当的防御机制来保护我的模型免受可能干扰的噪声？

1.  用新数据再培训模型的过程是怎样的，如何替换旧的、过时的模型？

1.  我将如何将数据分为无需自动处理的样本和需要人工检查的样本？

换句话说，将 AI 产品化是一个基础设施协调问题。在规划解决方案设计时，您应该使用持续监控、再培训和反馈来确保稳定性和可持续性。成功做到这一点将帮助您的公司从 AI 中获得期望的价值。

**相关：**

+   [机器学习模型开发与模型运维：原则与实践](https://www.kdnuggets.com/2021/10/machine-learning-model-development-operations-principles-practice.html)

+   [何时再培训机器学习模型？运行这 5 个检查来决定时间表](https://www.kdnuggets.com/2021/07/retrain-machine-learning-model-5-checks-decide-schedule.html)

+   [MLOps 是一个工程学科：初学者概述](https://www.kdnuggets.com/2021/07/mlops-engineering-discipline.html)

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的捷径。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您组织的 IT 工作

* * *

### 更多相关主题

+   [探索 Python 的 itertools 中的无限迭代器](https://www.kdnuggets.com/exploring-infinite-iterators-in-python-itertools)

+   [介绍 Streaming-LLM：适用于无限长度输入的 LLM](https://www.kdnuggets.com/introduction-to-streaming-llm-llms-for-infinite-length-inputs)

+   [如何加速 XGBoost 模型训练](https://www.kdnuggets.com/2021/12/speed-xgboost-model-training.html)

+   [如何使用合成数据克服机器学习模型训练的数据短缺](https://www.kdnuggets.com/2022/03/synthetic-data-overcome-data-shortages-machine-learning-model-training.html)

+   [与 Nvidia 的在线培训和研讨会](https://www.kdnuggets.com/2022/07/online-training-workshops-nvidia.html)

+   [机器学习中的训练数据和测试数据的区别](https://www.kdnuggets.com/2022/08/difference-training-testing-data-machine-learning.html)
