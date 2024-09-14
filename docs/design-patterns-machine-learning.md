# 机器学习中的设计模式

> 原文：[`www.kdnuggets.com/2021/07/design-patterns-machine-learning.html`](https://www.kdnuggets.com/2021/07/design-patterns-machine-learning.html)

评论

**由 [Ágoston Török](https://www.linkedin.com/in/agoston-torok/)，AGT International 数据科学总监**

根据其 [定义](https://en.wikipedia.org/wiki/Software_design_pattern)，设计模式是针对常见问题的可重用解决方案。在软件工程中，这一概念可以追溯到 [1987](http://c2.com/doc/oopsla87.html)年，当时贝克和坎宁安开始将其应用于编程。到了 2000 年代，设计模式——特别是面向对象编程的 SOLID 设计原则——被认为是程序员的常识。快进 15 年，我们来到了 [Software 2.0](https://karpathy.medium.com/software-2-0-a64152b37c35)时代：机器学习模型开始在越来越多的代码位置取代传统功能。今天，我们将软件视为传统代码、机器学习模型和基础数据的融合。这种融合要求这些组件的无缝集成，鉴于这些领域通常各自独特的历史和发展，这往往远非简单。

> 今天，我们将软件视为传统代码、机器学习模型和基础数据的融合。

然而，设计模式尚未扩展到应对这一新时代的挑战。在 Software 2.0 中，常见的挑战不仅仅出现在代码层面，还包括问题定义、数据表示、训练方法、扩展以及 AI 系统设计的伦理方面。这为机器学习的*反模式*实践创造了肥沃的土壤。不幸的是，如今甚至博客文章和会议有时也会出现反模式：那些被认为能改善情况的做法，实际上却使情况更糟。由于反模式也需要技能，因此其从业者往往无法识别它们。因此，接下来我将给出两个常见的机器学习挑战的例子，但不是从设计模式开始，而是首先介绍它们的解决方案反模式。

### 该模型在评估指标上的表现不佳。

在常见场景中，工程师在收集、清理和准备数据后，会训练第一个模型并发现其在测试数据上的表现不佳。一种常见的反模式是用更复杂的模型（例如，通常是梯度提升树）替换第一个模型，并通过这种方式提升性能。这种反模式的变体可能在此步骤之后，通过例如模型平均的方式结合多个模型。

![](https://i.ibb.co/60JgGn1/1-C2-Xo0-Cyun-Fmcf-C6k-BGOmw.png)

唐纳德·克努斯的名言“过早优化是万恶之源”已经快 50 年了，依然是真理。图片由 [tddcomics](https://www.instagram.com/tddcomics/)授权使用。

这些方法的问题在于，它们只关注问题的一部分，即模型，并选择通过增加模型复杂性来解决它。这种步骤迫使我们接受过拟合的高风险，并在额外的预测能力和可解释性之间做出权衡。虽然有高效的实践可以缓解这种选择的副作用（例如 LIME），但我们不能完全消除它们。

设计模式是错误分析。这在实践中意味着查看我们的模型在哪里出错，无论是通过评估模型在不同测试集上的拟合情况，还是通过查看模型错误的个别案例。尽管我们都听说过“垃圾进，垃圾出”的说法，但仍然很少有人意识到，即使是数据中的小不一致性也会对结果产生如此大的影响。也许标签来自不同的评分者，每个人对标注指南有自己稍有不同的解释。也许数据收集方式随着时间发生了变化。错误分析对小数据问题尤其有效。然而，我们还应牢记，在大量数据情况下，我们也会遇到长尾事件（例如，从入学考试中识别稀有人才）。

错误分析的真正力量在于，通过应用它，我们既不会在可解释性和过拟合风险之间做出权衡，实际上，单独应用它就能获得关于数据分布的关键知识。此外，错误分析使我们能够选择以模型为中心（例如，更复杂的模型）和以数据为中心（例如，进一步清理步骤）解决方案。

### 部署模型的性能随时间下降

模型经过广泛的验证并投入生产。用户感到满意并给予积极反馈。然后，一个月/季度/年后，收到报告指出预测中的缺陷。这通常是概念漂移的表现，即模型学习到的输入和输出之间的联系随着时间发生了变化。有些地方概念漂移是众所周知的（如词义、垃圾邮件检测），但‘概念’漂移可以发生在任何领域。例如，口罩和社交距离规定也挑战了许多以前部署的计算机视觉模型。

![](https://i.ibb.co/xzZfH6B/1-L-k-Qqpm-VBd5-Qe-Fuv-En-Tg-Jg.png)

ML 系统在不进行再训练的情况下，假设输入和输出之间学习到的关系不会发生变化。图片由[tddcomics](https://www.facebook.com/tddcomics)提供许可。

一个常见的反模式是将这些例子归因于噪声，并期待情况随着时间的推移而稳定。这不仅意味着缺乏行动，还意味着错误归因，这在数据驱动的业务中通常应被避免。稍微好一点的反模式是对报告做出快速重新训练和新模型部署的反应。即使团队认为他们遵循敏捷软件开发原则，从而选择快速响应变化，这仍然是一个反模式。问题在于，这个解决方案解决的是症状而不是系统设计中的缺陷。

设计模式是对性能的持续评估，这意味着你预计漂移会发生，因此，设计系统以尽快注意到它。这是一种完全不同的方法，因为重点不在于反应速度，而在于*检测*速度。这使得整个系统处于一个更加受控的状态，提供了更多的优先级反应空间。持续评估意味着建立流程和工具，以持续生成新数据的一部分的真实标签。在大多数情况下，这涉及手动标记，通常使用众包服务。然而，在某些情况下，我们可以使用其他更复杂但在部署环境中不可行的模型和设备来生成真实标签。例如，在自动驾驶汽车的开发中，可以使用一个传感器（如 LiDAR）的输入来生成另一个传感器（如相机）的真实标签。

### 机器学习的 SOLID 设计原则

我撰写关于设计模式的原因是，这个领域已经成熟到我们不仅要分享最佳实践，而且应该能够将其抽象为真正的设计模式。幸运的是，这项工作已经由多个团队开始了。实际上，最近有两本书已经出版了 [[1](https://www.oreilly.com/library/view/machine-learning-design/9781098115777/)], [[2](https://www.manning.com/books/deep-learning-design-patterns)]。我喜欢阅读这些书，但我仍然感到尽管我们正在朝着正确的方向前进，但我们仍然离为机器学习从业者制定 SOLID 设计原则还有几步之遥。我相信，虽然基础知识已经存在并用于构建今天的 AI 驱动产品，但对设计模式和反模式的研究是迈向 Software 2.0 时代的重要一步。

![](https://i.ibb.co/mFZj5Jk/1-l3d-Ype-C9z-S5tkkls-ZMOaq-Q.png)

设计模式是机器学习工艺的基础。图片由 [tddcomics](https://twitter.com/tddcomics) 提供。

**简历： [Ágoston Török](https://www.linkedin.com/in/agoston-torok/)** 是 AGT International 的数据科学总监。

[原文](https://towardsdatascience.com/design-patterns-in-machine-learning-b73eea4882cd)。已获授权转载。

**相关：**

+   何时重新训练机器学习模型？运行这 5 个检查来决定时间表

+   2021 年顶级 6 个数据科学在线课程

+   你的机器学习代码消耗了多少内存？

* * *

## 我们的前三课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

### 相关主题

+   [停止学习数据科学来寻找目标，找到目标去…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [一个 90 亿美元的 AI 失败案例分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么让 Python 成为初创企业理想的编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每个数据科学家都应该了解的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
