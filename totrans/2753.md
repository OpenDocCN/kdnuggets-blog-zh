# 如何在 COVID-19 危机期间使 AI/机器学习模型保持韧性

> 原文：[https://www.kdnuggets.com/2020/06/ai-ml-models-resilient-covid-19-crisis.html](https://www.kdnuggets.com/2020/06/ai-ml-models-resilient-covid-19-crisis.html)

[评论](#comments)

**[Mayank Kumar](https://in.linkedin.com/in/mayank-kumar-3814aa35)，数据科学顾问和领导者，UnitedHealth Group**。

![](../Images/4e4980a22bbb5c6737f29744a089de83.png)

最近，我遇到了一些文章和对话，数据科学和分析领域的领导者们对 AI/ML 的性能和可靠性表示担忧。概念漂移或数据漂移的问题并不新鲜，但 COVID-19 使其影响显得非常显著。在本文中，我讨论了一些最佳实践，以确保创建有韧性的 AI/ML 解决方案来应对这种数据变化。

### COVID 影响的广度

AI/ML 在所有行业和领域中有着极其广泛的应用。然而，并非所有行业或用例都受到 COVID 触发的数据变化的影响。根据我的评估和观察，只有 AI/ML 被用于捕捉消费者/终端用户行为的领域受到了影响，因为这正是 COVID 改变的唯一内容。例如，你的 Alexa 并没有突然停止理解英语，或者自动驾驶汽车也没有忘记如何驾驶，因为这里没有用户行为的涉及。改变的是我们的生活方式、日常需求、在线存在等。在 B2B 市场中，改变的是公司如何开展业务，他们消费哪些产品，以及他们放弃哪些产品。在这些变化之外，另一个问题是变化具有不稳定性。我们每周都看到由许多外部因素驱动的变化。

这个问题主要存在于 AI/ML 使用历史数据来分析行为和预测未来事件。如果最近的行为发生了剧烈变化并且持续不稳定，那么静态的 AI/ML 解决方案显然没有机会，最多只能是不准确，最差则可能具有误导性。

### **如何应对 COVID****’****的影响？**

![](../Images/1d29feb6f2b76cc8f53200057297c72a.png)

尽管没有明确且详尽的指南来解决这个问题，但根据具体问题的不同，可能需要采用一种或多种策略的不同排列组合。以下是一些我认为可以朝这个方向努力的方法。

**监控你的数据和特征**

作为一名数据科学领导者，我学到的关键之一是成功的数据科学团队应该始终具备监控底层数据和分析特征的能力。这是掌控高级分析策略的关键步骤，你可以直接看到数据的表现以及模型的表现如何。

我不是在谈论临时监控，我感觉我们大多数人已经在需求下进行，当我们发现分析解决方案没有达到预期目标时。我在谈论创建主动措施，能够在数据变化、结果恶化等方面提醒解决方案拥有者。

**评估你的AI/ML解决方案的灵活性**

敏捷性是关键，同样AI/ML也是如此。问问自己，是否你的模型需要针对每次特征分布的变化进行重新训练，或者在你设计特征和模型的方式中是否有一定程度的动态性。为了使解决方案对最近的数据更加敏感，追踪最近的用户行为，需要付出怎样的努力？对不同决策参数赋予不同权重的人工修正有多容易？是否可以在你的AI/ML工作流中应用基于规则的业务过滤？

动态特性、自学习模型、提高解决方案的敏感性能力，以及在AI/ML决策流程中引入人工修正的便捷性是决定数据科学和分析解决方案在快速调整以适应数据模式变化方面的灵活性的关键因素。为此创造了一个术语，称为MLOps，类似于技术中的DevOps，旨在为数据科学领域带来类似的敏捷性。

**重新审视结果变量的定义**

由于我们所称为正常的情况已经发生变化，这确实需要数据科学家重新审视其结果变量的定义，以确定他们之前称为异常的情况是否已经成为新的正常情况，反之亦然。我们的数据是否可能落入一个之前不存在的新类别中，或者某些类别是否已经消失？还应询问模型如何处理训练期间未见过的新数据点？它是否将这些数据点视为正例或负例离群点，还是将其视为正常数据的一部分，这更多的是一个基于用例的决策，可能需要进行。

**从基于配置文件的方法转向非配置文件的方法**

虽然这一部分可能并不适用于所有情况，但在可能的情况下，应探索那些不需要正常配置文件进行训练，而是具有根据给定样本发现离群点的内在能力的方法。该声明主要适用于无监督和混合设置中的离群点检测系统。

**更加信任实时验证你的解决方案**

鉴于我们当前的数据可能已与过去所见发生了显著变化，信任我们在训练、测试和验证过程中建立的过去模型表现将不再提供真实的图景。数据科学团队应尽可能转向实时验证设置，使用A/B测试以观察模型在没有AI/ML解决方案的当前设置下的表现。

### 结论

正如我在某处读到的，AI/ML 是有生命的，需要不断的维护和干预。因此，像其他有生命的物体一样，它需要持续的关注。数据科学团队应将约 20-30% 的精力投入到监控、管理和潜在的改进中，以取得成功。

您是否已经利用上述一些方法来评估您的 AI/ML 解决方案的性能？您的团队还采用了哪些其他技术来应对数据频繁变化的潜在影响？

### 参考资料

[大流行病严重困扰了机器学习系统](https://futurism.com/the-byte/pandemic-confused-machine-learning-systems)

[为什么机器学习项目如此难以管理？](https://www.kdnuggets.com/2020/02/machine-learning-projects-manage.html)

[确保 AI/ML 系统在 COVID-19 中生存的 4 个步骤](https://www.kdnuggets.com/2020/04/ai-machine-learning-system-survives-covid-19.html)

[概念漂移及 COVID-19 对数据科学的影响](https://www.iguazio.com/blog/concept-drift-and-the-impact-of-covid-19-on-data-science/)

[COVID-19 如何影响机器学习](https://www.crayon.com/en/news-and-resources/covid-machine-learning/)

**简介：**[Mayank Kumar](https://in.linkedin.com/in/mayank-kumar-3814aa35) 在医疗和制药行业拥有超过10年的数据科学经验，主要面向美国市场。目前，Mayank 管理多个项目，开发针对性的分析解决方案，利用先进的机器学习、深度学习和大数据处理工具与技术。

**相关：**

+   [数据科学家如何训练和更新模型以为 COVID-19 恢复做准备](https://www.kdnuggets.com/2020/04/data-scientists-train-models-covid-19-recovery.html)

+   [为什么 AI 系统需要人工干预才能正常运作？](https://www.kdnuggets.com/2020/06/ai-systems-need-human-intervention.html)

+   [终极模型再训练指南](https://www.kdnuggets.com/2019/12/ultimate-guide-model-retraining.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT

* * *

### 更多相关主题

+   [弹性 ML 堆栈是模块化的](https://www.kdnuggets.com/2022/06/comet-resilient-ml-stack-modular.html)

+   [如何让大型语言模型与您的软件和谐共处……](https://www.kdnuggets.com/how-to-make-large-language-models-play-nice-with-your-software-using-langchain)

+   [强化学习：教计算机做出最佳决策](https://www.kdnuggets.com/2023/07/reinforcement-learning-teaching-computers-make-optimal-decisions.html)

+   [这是我用的 AI 工具以及我的技能来赚取 $10,000 的方法…](https://www.kdnuggets.com/2023/07/ai-tools-along-skills-make-10000-monthly-bs.html)

+   [使用 Pandas 制作美丽互动可视化的最简单方法](https://www.kdnuggets.com/2021/12/easiest-way-make-beautiful-interactive-visualizations-pandas.html)

+   [假装直到成功：生成真实的合成客户数据集](https://www.kdnuggets.com/2022/01/fake-realistic-synthetic-customer-datasets-projects.html)
