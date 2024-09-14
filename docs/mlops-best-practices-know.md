# 你应该了解的 MLOps 最佳实践

> 原文：[https://www.kdnuggets.com/2023/04/mlops-best-practices-know.html](https://www.kdnuggets.com/2023/04/mlops-best-practices-know.html)

![MLOps 最佳实践](../Images/646befef8a2fe0fbed454c2b72879fa2.png)

照片由 [Arian Darvishi](https://unsplash.com/@arianismmm?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

MLOps，即机器学习运维，是用于模型部署到生产环境的一系列技术和工具。近年来，DevOps 在缩短软件发布之间的时间和减少差距方面的成功，对任何公司的生命周期都至关重要。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

由于成功的历史，开发人员进入了机器学习领域，应用 DevOps 原则，这催生了 MLOps。通过将 CI/CD 原则与机器学习模型结合，数据领域能够及时整合和交付模型。MLOps 引入了新的原则，如持续训练 (CT) 和持续监控，使生产环境更加适合任何机器学习模型。

随着 MLOps 的进步，我们应该遵循一些最佳实践，以实现最佳工作流程。这些实践是什么呢？让我们深入了解一下。

# MLOps 最佳实践

在继续之前，本文假设读者已具备 MLOps、机器学习和编程的基础知识。考虑到这一点，让我们继续探讨最佳实践。

## 1\. 建立清晰的项目结构

通过清晰的结构来评估 MLOps 更加容易。没有适用于每一个点的确切 MLOps 流水线和工具，因此我们需要为我们的项目建立清晰的结构。一个组织良好的项目结构使得我们未来的项目更易于导航、维护和扩展。

项目结构意味着我们需要从头到尾了解，从业务问题到生产和监控，都需要精确。一些**改进我们项目结构的建议**包括：

- 根据环境和功能组织我们的代码和数据。同时，保持代码和数据的命名规范整洁，以避免意外。

- 使用版本控制，如 GIT 或 DVC 来跟踪更改，

- 拥有一致风格的文档，

- 与团队沟通你所做的任何更改。

建立清晰的项目结构是一个麻烦的过程，但从长远来看，它肯定会对我们的项目有帮助。

## 2\. 了解你的工具栈

MLOps不仅仅是一个概念，它也涉及到工具。你的MLOps活动中有很多工具可以选择。然而，选择取决于你的项目和公司要求。

例如，如果公司的合规要求数据分析必须在本公司创建的工具中完成，我们必须遵守。这就是为什么了解你在MLOps管道中想使用的工具栈在开发能力时至关重要。

为了帮助你了解项目所需的工具，这里有**MLOps Stack Template**由[Valohai](https://valohai.com/blog/the-mlops-stack/)提供，你可以参考。

![MLOps最佳实践](../Images/5cf6521078b9571dddad8e58b317ee3f.png)

图片来源：[Valohai](https://valohai.com/blog/the-mlops-stack/)

另外，尽量将工具限制在三到五个之间。使用的工具越多，情况就会变得越复杂。

## 3\. 追踪你的开支

在我们的管道中使用MLOps的目标是最小化技术债务。这是一个很好的目标，因为我们不希望技术债务让项目变得复杂。然而，不要因为我们想要最小化技术债务而让开支变得过高。

许多用于MLOps的工具是基于订阅或按使用收费的，这取决于工具本身。与其从头开发或使用开源工具，许多付费工具提供了更好的集成MLOps体验的服务。

但有时我们需要记住，服务是需要收费的，而我们又稀疏使用这些服务，这也是我在早期采用MLOps时遇到的情况。记得好好追踪开支，我们不希望MLOps带来的价值被金钱削减。

使用像AWS这样的云服务，计算器和警报会提醒你开支。如果没有，可以尝试使用各种工具来跟踪。即使是简单的Excel也能奏效。

## 4\. 对所有事物设立标准

我们不仅需要一个简洁的项目结构，还需要**MLOps管道每个部分的标准**。最小化技术债务意味着我们希望一切正常运作，常常问题出在团队缺乏标准。

想象一下，如果工具、变量、脚本、数据等的命名是随机的，并且团队成员之间没有一致性。开发者需要理解发生了什么，过程会变得更长，并且会产生技术债务。

标准化不仅适用于命名约定，还可以应用于与MLOps管道相关的所有方面。数据分析过程、使用的环境、管道结构、部署过程等等。制定所有标准，MLOps的效果会更好。

## 5\. 定期评估你的MLOps成熟度

我们的MLOps准备情况有多远是一个我们需要经常问的问题。我们希望充分发挥MLOps的好处，这只有在成熟度达到时才会出现。遗憾的是，这不是一天或一个月可以实现的。

这可能需要一些时间，但这就是为什么在实施MLOps时不要等待一个完美的管道。相反，从可以首先处理的事项开始，并持续评估我们的MLOps准备情况。

作为参考，我喜欢使用 [Microsoft Azure](https://learn.microsoft.com/en-us/azure/architecture/example-scenario/mlops/mlops-maturity-model) 提供的MLOps成熟度金字塔来评估准备情况。共有五个级别，每个级别为我们的生态系统提供价值。

![你应该知道的MLOps最佳实践](../Images/c9846b802b764d7b01286f7ed2a14af8.png)

图片来源于 [Microsoft Azure](https://learn.microsoft.com/en-us/azure/architecture/example-scenario/mlops/mlops-maturity-model)

# 结论

MLOps或机器学习操作对公司生命周期至关重要。这就是为什么你可以遵循一些最佳实践：

1.  建立一个清晰的项目结构

1.  了解你的工具栈

1.  跟踪你的开支

1.  为一切设定标准

1.  定期评估你的MLOps成熟度

希望这有所帮助。

**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)** 是一名数据科学助理经理和数据撰稿人。在全职工作于Allianz Indonesia期间，他喜欢通过社交媒体和写作媒体分享Python和数据技巧。

### 更多相关主题

+   [MLOps：最佳实践及如何应用](https://www.kdnuggets.com/2022/04/mlops-best-practices-apply.html)

+   [KDnuggets新闻，4月13日：数据科学家应该了解的Python库…](https://www.kdnuggets.com/2022/n15.html)

+   [你需要了解的关于MLOps的所有内容：KDnuggets技术简报](https://www.kdnuggets.com/tech-brief-everything-you-need-to-know-about-mlops)

+   [分类问题的更多性能评估指标你应该了解…](https://www.kdnuggets.com/2020/04/performance-evaluation-metrics-classification.html)

+   [你应该了解的关于梯度下降和成本函数的5个概念](https://www.kdnuggets.com/2020/05/5-concepts-gradient-descent-cost-function.html)

+   [在了解变换器之前你应该知道的概念](https://www.kdnuggets.com/2023/01/concepts-know-getting-transformer.html)
