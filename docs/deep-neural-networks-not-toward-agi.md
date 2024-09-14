# 深度神经网络不会引导我们走向 AGI

> 原文：[`www.kdnuggets.com/2021/12/deep-neural-networks-not-toward-agi.html`](https://www.kdnuggets.com/2021/12/deep-neural-networks-not-toward-agi.html)

评论

**由 [Thuwarakesh Murallie](https://www.linkedin.com/in/thuwarakesh/)，Stax Inc. 的数据科学家**

![](img/a445ad39209d0406a32a1079ca03f10c.png)

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全领域。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

*照片由 [Kindel Media](https://www.pexels.com/@kindelmedia?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 提供，来自 [Pexels](https://www.pexels.com/photo/person-pointing-on-a-miniature-toy-robot-8566471/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)。*

我对如 GitHub Copilot、Deepfake 和 AI 机器人以复杂策略玩 Alpha Go 这样的项目感到震惊，连专业玩家也无法理解这些策略。

我曾认为我们快要达到人工通用智能（AGI）。我认为这是 AGI 的伟大飞跃。

但我错了。

目前的神经网络在许多方面都不及人脑。机器学习（包括深度学习）可以解决特定的识别问题。然而，智能更多的是一个生成性问题。

我们需要了解 AI 的实际问题，然后再尝试用生物启发的算法（如深度神经网络）来解决这些问题。

### 为什么机器学习不是人工智能？

许多文献声称 [机器学习是人工智能的一个子集。](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6381354/)

从总体上说，我对此并不反对。但我相信机器学习只是 AI 的一种方法，而不是 AI 本身。

为了更好地理解这一点，我们必须思考孩子们是如何学习的。

人类和动物的学习方式是通过创建现实世界的表征。进一步的学习是通过融合其他类似的虚拟世界来改进这种表征。

它有助于想象尚不存在的事物。实际上，解决问题是从这种内部表征中生成想法，并将它们与现实世界进行匹配。

这就是[孩子们如何学习](https://www.simplypsychology.org/constructivism.html)的原因。因此，他们不一定需要看到猫和狗才能开始识别它们。他们的父母和环境在培养他们的世界内部表征方面发挥了作用。因此，他们生成关于狗和猫的想法，然后将其与自然世界匹配，以便在看到时进行分类。

机器学习（ML）算法不会学会做它们未被编程做的事情，也不会以任何方式具有创造力。它们遵循我们通过数据集示例教给它们的步骤。

正因如此，每当机器学习模型接触到新类型的数据时，都必须从头开始。

尽管深度神经网络已经在一些模式识别任务中占据了主导地位，如物体检测、语音识别或翻译——**它们仍然缺乏生成能力。**

在生成任务中，模型必须从无或随机中创建某物。

*相关： [如何评估深度学习是否适合你？](https://www.the-analytics.club/is-deep-learning-right-for-you)*

### 如果机器学习不是人工智能，那么它是什么？

机器学习算法是特定问题的复杂近似。它们通过优化而非生成想法来解决问题。

他们需要看到足够多的示例才能识别问题。

这就是深度神经网络在许多方面受限的原因。例如，它们需要大量的数据和标记的训练样本。在复杂任务或具有大量类别/类别的问题上，它们的扩展性并不很好。

这些限制阻碍了它们解决需要推理、创造力和抽象思维的问题——例如人工通用智能所需的问题。

实际上，如果你知道如何骑自行车，你应该以某种程度的熟悉度学习驾驶汽车。但将知识从一个机器学习模型转移到另一个模型并不是那么简单。

### 迁移学习难道不是一种知识转移的方法吗？

研究人员使用的一种有用技术叫做迁移学习。迁移学习允许我们使用在一个任务上训练的深度学习模型作为类似工作的起点。

*相关： [迁移学习：你可以学习的最具杠杆作用的深度学习技能](https://towardsdatascience.com/transfer-learning-in-deep-learning-641089950f5d)*

但即使是迁移学习也远未实现普遍的知识转移。在迁移学习中，你需要遵守许多要求，如保持类似的输入结构。

此外，迁移学习并不是解决机器学习模型非生成性质的方案。它仅仅帮助神经网络更快地收敛以解决调整问题。

迁移学习的好处包括减少训练时间和成本，但它仍然是一个识别问题。

### 谷歌知道这一点。

谷歌知道深度神经网络或机器学习一般还无法实现人工通用智能。他们理解，经过训练的模型是对现实世界知识的一种薄弱表示。

> *“机器学习系统常常在单一任务上过度专门化，而它们本可以在许多任务上表现出色。”——Jeff Dean，谷歌高级研究员及 SVP*

谷歌的目标是通过他们称之为“Pathway”的方法来解决这个问题。

关于 Pathway 如何工作的公开信息不多。但根据 Jeff Dean 的[博客文章](https://blog.google/technology/ai/introducing-pathways-next-generation-ai-architecture/)，谷歌 Pathway 即将解决通向 AGI 的三大常见挑战。

Pathway 使我们能够使用一个模型来解决数千个不同的问题。这与传统的机器学习模型不同，因为后者被训练来解决特定问题。

此外，Pathway 使用了多种感官。传统模型使用来自单一感官的输入。例如，计算机视觉问题处理图像，语音识别处理音频信号。Pathway 预计同时处理这些以及其他感官输入。

此外，Pathway 承诺将模型执行从密集模式转移到稀疏模式。这意味着，为了解决一个问题，你不必激活整个神经网络，而是孤立地激活特定路径。

### 总结

这并不是说机器学习在人工智能领域没有其作用。

这仅仅意味着我们不应该将 ML 误认为 AGI，也不要假设它能引导我们实现类似人类智能和通用问题解决能力的目标。

深度神经网络（以及机器学习模型一般）适用于识别问题。但它们没有创建解决更一般问题所需的世界表示。

目前尚不清楚谷歌如何通过 Pathway 解决这些朝向 AGI 的挑战。我们需要等到相关信息发布后才能进一步评论。

你有什么想法？你认为机器学习会引导我们走向 AGI 吗？

你对 Pathway 模型及其在解决通用 AI 问题方面的承诺感到兴奋吗？分享你的想法、意见和观点。我很想听到你们的看法！

**简介：** Thuwarakesh Murallie（[@Thuwarakesh](https://twitter.com/Thuwarakesh)）是 Stax Inc.的数据科学家和 Medium 上的人工智能顶级作者。

**相关：**

+   [对 AI 系统难题进行人类监督的扩展——OpenAI 方法](https://www.kdnuggets.com/2021/10/openai-scaling-human-oversight-ai-systems.html)

+   [超越万亿参数和 GPT-3 的 Switch Transformers——通向 AGI 的路径？](https://www.kdnuggets.com/2021/10/trillion-parameters-gpt-3-switch-transformers-path-agi.html)

+   [AGI 与人类的未来](https://www.kdnuggets.com/2021/07/agi-future-humanity.html)

### 更多相关内容

+   [规划你通向 SAS 认证的旅程](https://www.kdnuggets.com/2022/11/sas-map-journey-towards-sas-certification.html)

+   [婴儿 AGI：完全自主 AI 的诞生](https://www.kdnuggets.com/2023/04/baby-agi-birth-fully-autonomous-ai.html)

+   [我们离 AGI 有多近？](https://www.kdnuggets.com/how-close-are-we-to-agi)

+   [使用最先进的深度学习进行可解释的预测与现在预测](https://www.kdnuggets.com/2021/12/sota-explainable-forecasting-and-nowcasting.html)

+   [神经网络与深度学习：教科书（第二版）](https://www.kdnuggets.com/2023/07/aggarwal-neural-networks-deep-learning-textbook-2nd-edition.html)

+   [使用 PyTorch 解释性神经网络](https://www.kdnuggets.com/2022/01/interpretable-neural-networks-pytorch.html)
