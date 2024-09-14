# 5 种将 AI 应用于小数据集的方法

> 原文：[`www.kdnuggets.com/2022/02/5-ways-apply-ai-small-data-sets.html`](https://www.kdnuggets.com/2022/02/5-ways-apply-ai-small-data-sets.html)

![5 种将 AI 应用于小数据集的方法](img/fed9eb27fdbd33a0cc676a227318abea.png)

[技术照片由 rawpixel.com 创建 - www.freepik.com](https://www.freepik.com/photos/technology)

人工智能和数据科学协同工作，以改善数据收集、分类、分析和解释。然而，我们只听说过使用 AI 理解大数据集。这是因为小数据集通常容易被人理解，应用 AI 来分析和解释它们并不必要。

* * *

## 我们的前 3 名课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT 工作

* * *

如今，许多企业和制造商将 AI 集成到生产线中，逐渐造成数据稀缺。与大型公司不同，许多小型公司由于风险、时间和预算限制，无法收集大量训练数据。这导致了对小数据集的 AI 解决方案的忽视或错误应用。

由于大多数公司不知道如何正确地利用[AI 在小数据集上的应用](https://www.industryweek.com/technology-and-iiot/digital-tools/article/21122846/making-ai-work-with-small-data)，它们盲目地将其应用于基于以往文件的未来预测。不幸的是，这会导致错误和风险决策。

因此，学习正确的方法将 AI 应用于小数据集并避免任何误解是至关重要的。

### AI 在小数据集上的 5 种正确应用方法

将 AI 算法应用于小数据集可以获得无人工错误和虚假结果的结果，但前提是正确应用。你还可以节省时间和资源，通常用于手动解释小数据。

以下是将 AI 应用于小数据集的一些方法：

### 1\. 少样本学习

少样本学习模型向 AI 引入少量训练数据，以作为新数据集解释的参考。这是一种常用的计算机视觉方法，因为它不需要许多示例进行识别。

例如，财务分析系统并不需要大量库存就能有效运作。因此，你可以根据系统的容量输入一个[利润和损失表模板](https://www.freshbooks.com/accounting-templates/profit-and-loss)，而不是让 AI 系统承载大量信息。

与其他 AI 系统不同的是，如果你在这个模板中输入更多信息，它将导致虚假的结果。

当你在 AI 系统中上传样本数据时，它会从训练数据集中学习模式，以便对未来的小数据集进行解释。少量学习模型的吸引力在于，你不需要一个庞大的训练数据集来训练 AI，使其在低成本和低努力下可运行。

### 2\. 知识图谱

知识图谱模型通过从一个大的原始数据集中筛选创建二级数据集。它用于存储事件、对象、实际情况以及理论或抽象概念的互联描述和特征。

除了作为数据存储，该模型还同时对特定数据集中的语义进行编码。

知识图谱模型的主要功能是组织和结构化数据集中的重要点，以整合从各种来源收集的信息。知识图谱被标注以关联特定含义。图谱中有两个主要组件 - 节点和边。节点是两个或更多的项，而边表示它们之间的连接和关系。

你可以使用知识图谱来存储信息，通过多个算法整合数据并操控数据，以突出新信息。此外，它们在组织小数据集方面非常有用，使数据高度可解释和可重用。

### 3\. 迁移学习

公司避免在小数据集上应用 AI，因为他们对结果不确定。对于大数据集产生有效结果的方法在小数据集上往往适得其反，产生虚假的结果。然而，[迁移学习方法](https://builtin.com/data-science/transfer-learning)可以在数据集大小不同的情况下产生类似且可靠的结果。

迁移学习使用一个 AI 模型作为起点，但通过一个新的 AI 模型获得结果。简而言之，它是将知识从一个模型转移到另一个模型的过程。

该模型主要用于计算机视觉领域和自然语言处理。原因在于这些任务需要大量的数据和计算能力。因此，使用迁移学习可以减少额外的时间和精力。

为了在小数据上应用迁移学习模型，新数据集必须与原始训练数据集相似。在应用过程中，移除神经网络的末端，并添加一个类似于新数据集类别的全连接层。在此之后，随机化全连接层的权重，同时冻结先前网络的权重。现在，根据新的全连接和操作层更新并训练 AI 网络。

### 4\. 自监督学习

自监督学习或 SSL 模型从可用或训练数据集中[获取监督信号](https://2020/04/google-open-sources-simclr-self-supervised-semi-supervised-image-training.html)。然后，它利用已有数据来预测未观察到或隐藏的数据。

SSL 模型主要用于执行回归分析和分类任务。然而，它在计算机视觉、视频处理和机器人控制领域标记未标记数据方面也很有帮助。该模型迅速解决了数据标记挑战，因为它独立构建并监督完整的过程。这样，公司节省了创建和应用不同 AI 模型的额外成本和时间。

使用 SSL 模型具有高度的适应性，因为它能够生成可靠的结果，无论数据集的大小如何，证明了模型的可扩展性。SSL 对于长期提高 AI 能力也非常有用，因为它支持升级。此外，它消除了对样本案例的需求，因为 AI 系统会独立演变。

### 5\. 合成数据

这是一种[人工生成的数据](https://blogs.nvidia.com/blog/2021/06/08/what-is-synthetic-data/)，由在真实数据集上训练过的 AI 算法创建。顾名思义，这些数据是人工创建的，不基于实际事件。合成数据的预测能力与原始数据的预测相匹配。它可以替代初始数据预测，因为它不使用伪装和修改。

合成数据在可用数据集中存在空白且无法通过积累数据填补的情况下使用效果最佳。此外，与其他 AI 学习和测试模型相比，它成本低廉，不会侵犯客户隐私。因此，合成数据迅速在多个领域占据主导地位，到 2024 年底，[60% 的 AI 分析项目](https://blogs.gartner.com/andrew_white/2021/07/24/by-2024-60-of-the-data-used-for-the-development-of-ai-and-analytics-projects-will-be-synthetically-generated/)将会是合成生成的。

合成数据越来越受到关注，因为公司可以创建满足现有数据中不存在的特定条件的数据。因此，如果公司由于隐私限制无法访问数据集或产品无法进行测试，它们仍然可以利用 AI 算法创建合成数据以获得可靠的结果。

### 总结

人工智能正在快速发展并接管以简化每一个复杂任务。然而，大多数人不知道他们可以应用人工智能算法。例如，它非常适合组织和分析大数据，同时在较小的数据集上也相当有效。但为了获得正确的结果，你必须使用准确的人工智能方法和模型。使用本文中列出的人工智能模型，因为它们适用于在小数据集上创建正确的结果。

**[Nahla Davies](http://nahlawrites.com/)** 是一名软件开发者和技术作家。在全职从事技术写作之前，她曾担任过许多引人注目的职位，包括在一家《Inc. 5000》体验式品牌公司担任首席程序员，该公司客户包括三星、时代华纳、Netflix 和索尼。

### 更多相关主题

+   [KDnuggets 新闻，11 月 30 日：什么是切比雪夫定理及其应用](https://www.kdnuggets.com/2022/n46.html)

+   [什么是切比雪夫定理，它如何应用于数据科学？](https://www.kdnuggets.com/2022/11/chebychev-theorem-apply-data-science.html)

+   [MLOps：最佳实践及其应用方法](https://www.kdnuggets.com/2022/04/mlops-best-practices-apply.html)

+   [使用 apply() 方法处理 Pandas Dataframes](https://www.kdnuggets.com/2022/07/apply-method-pandas-dataframes.html)

+   [如何使用 NumPy 对数组进行填充](https://www.kdnuggets.com/how-to-apply-padding-to-arrays-with-numpy)

+   [在 Python 中加载数据的 5 种不同方法](https://www.kdnuggets.com/2020/08/5-different-ways-load-data-python.html)
