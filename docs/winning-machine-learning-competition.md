# 在机器学习竞赛中，是什么使得一个参赛作品获胜？

> 原文：[https://www.kdnuggets.com/2021/05/winning-machine-learning-competition.html](https://www.kdnuggets.com/2021/05/winning-machine-learning-competition.html)

[评论](#comments)

**由 [Harald Carlens](https://harald.co)，独立机器学习研究员**。

![](../Images/79f23d2c7152ec288dca2dca248cec4b.png)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在的组织在IT领域

* * *

如果你曾经尝试过Kaggle风格的竞赛，你可能会看到它们的竞争有多么激烈。每场竞赛可以吸引成千上万的竞争者，包括学术和企业团队。

在运营 [MLContests.com](http://mlcontests.com) 的过程中，自2019年底以来，我一直跟踪所有这些竞赛，并最近开始分析数据，以回答这个问题：是什么让一个参赛作品获胜？

我邀请了 [Eniola Olaleye](https://galileosolution.github.io/galileo/#/home)——Zindi平台上的前5名竞争者——仔细分析了2020年所有竞赛的获胜解决方案，并找出他们获胜的原因。他们使用了什么框架？他们有多少经验？他们使用了什么类型的建模？他们使用了深度学习吗？他们的团队有多大？在这篇文章中，我将重点介绍一些发现，这些发现可能帮助你提高赢得竞赛的机会。

### 我应该参加哪个竞赛？

首先，可能也是最重要的事情是决定参加哪个竞赛！显然，如果你将精力集中在一次一个竞赛上，努力做到最好，你获胜的机会会更大，而不是在多个竞赛中都有平庸的表现。

一件可能增加你获胜机会的事情是减少竞争。我们的数据显示，进入比赛的团队数量在不同竞赛和平台上差异很大！HackerEarth、DrivenData、Unearthed、Kaggle 和 CodaLab 竞赛的参赛者平均超过一千人。

![](../Images/b80fb0054f0c379e7336717d27b1fcab.png)

在另一端，我们有像 [Waymo 开放数据集挑战](https://waymo.com/open/challenges/) 或 [IARAI 的 Traffic4Cast 挑战](https://www.iarai.ac.at/traffic4cast/) 这样的单次比赛，参赛者只有几十人。虽然这些比赛可能更具领域特定性，并且没有 Kaggle Kernels 和 Notebooks 的便利，但如果你在相关领域有专长，参加这些比赛可能更合适。值得一提的是，今年，Waymo 再次举办其 [开放数据集挑战](https://waymo.com/open/challenges/)，包括四场比赛，每场比赛的奖金池为 22,000 美元！有关其他独立比赛的信息，请查看 [MLContests.com](http://mlcontests.com)。如果你发现有未列出的比赛，可以 [在 GitHub 上提交拉取请求](https://github.com/mlcontests/mlcontests.github.io) 与 ML Contests 社区分享。

### 哪种深度学习框架最好？

不足为奇，深度学习在竞争性 ML 中极为常见，特别是在 NLP 和基于视觉的任务中表现出色。

在过去两年中，有两个主要相关的深度学习框架：Google 的 [TensorFlow](https://www.tensorflow.org/) 和 Facebook 的 [PyTorch](https://pytorch.org/)。

由于其相对成熟和部署简便，TensorFlow 在“生产”行业环境中通常是更受欢迎的框架，这可以通过职位发布或 GitHub 星标来衡量。

而在研究领域，PyTorch 从不相关迅速成为大多数研究人员的明确偏好。它的 Python 风格、相对稳定的 API 和定制化的便捷，使其在大约 80% 的学术深度学习论文中被使用（仅计算提到 TensorFlow 或 PyTorch 的论文）。

![](../Images/c2c6e6a4755d7389faccf5f7425a03b0.png)

*来源：霍雷斯·赫，“[2019 年机器学习框架现状](https://thegradient.pub/state-of-ml-frameworks-2019-pytorch-dominates-research-tensorflow-dominates-industry/)”，The Gradient，2019。*

机器学习比赛显然位于研究和生产的研究-生产连续体的研究侧，这在深度学习框架的使用上得到了体现：超过 70% 的获胜解决方案使用了 PyTorch，而不到 30% 的解决方案使用了 TensorFlow。

这里是我们的答案：虽然成功的 ML 比赛参与者，和大多数其他研究人员一样，偏好 PyTorch，但如果你对 TensorFlow 很熟悉，也没有理由阻止你获胜。

### 你不需要在所有领域都成为专家

可能因为觉得其他人经验更丰富，或者你对某个领域还不熟悉，容易被打消参加比赛的念头。也许你对 Pandas 和统计学很熟悉，但对深度学习还不熟悉。或者你是某个领域的专家，理解 ML 概念，但编程能力有限。

噢，你真幸运！第一个好消息是，在我们的数据集中，超过80%的获胜者之前从未赢得过比赛。因此，虽然确实有一些突破性的重复获胜者，但一般来说，他们并不主导比赛，新手作为一个群体获胜的机会更大。

第二个好消息是，你不需要了解你所使用的每个组件的全部深度就能赢得比赛。

以深度学习为例，大约15%的深度学习解决方案使用了如Keras for TensorFlow或fast.ai for PyTorch这样的高级API。只要你对一般的机器学习感到熟悉，并且小心避免过拟合，这些API通常足以赢得比赛。你不一定需要从头编写自己的模型和训练循环，使用高级API可以让你将精力集中在其他方面获得优势。

另一个有趣的例子是在领域知识重要的比赛中。我们以[戈勒挑战赛](https://unearthed.solutions/u/competitions/exploresa)为例，该比赛预测南澳大利亚的矿产沉积区域，奖金池为25万美元。我们联系了获胜团队，了解他们是如何赢得10万美元大奖的。结果发现，他们是领域内的专家地质学家，主要依靠其在该领域的知识获得优势。他们将地质力学建模与随机森林结合起来。有趣的是，他们使用了[Weka](https://www.cs.waikato.ac.nz/ml/weka/)，一个用于随机森林实现的GUI工具。这使他们能够专注于自己擅长的领域，并显然获得了回报。

### 结论

所以结论是：如果你想赢：

1.  选择合适的比赛。考虑你可能拥有的优势以及哪些比赛可能被其他人忽视。

1.  专注于你的优势所在。如果你参加的比赛中你有一些领域知识，花更多时间充分利用这些知识可能比调整解决方案的其他部分更值得。

更多关于2020年机器学习比赛获胜者的统计数据，请参阅[这个总结](https://blog.mlcontests.com/p/winning-at-competitive-ml-in-2020)。

**简介：** [Harald Carlens](https://harald.co) 是一位独立的机器学习研究员，并且运营 [mlcontests.com](http://mlcontests.com)。他曾在剑桥大学学习数学，并在股权交易领域工作了八年。

**相关：**

+   [国际数据科学/机器学习比赛的Kaggle替代方案](https://www.kdnuggets.com/2020/09/international-alternatives-kaggle-data-science-competitions.html)

+   [我第一次Kaggle比赛的经验教训](https://www.kdnuggets.com/2020/09/lessons-first-kaggle-competition.html)

+   [我在Kaggle比赛中进入前2%的秘诀](https://www.kdnuggets.com/2018/11/secret-sauce-top-kaggle-competition.html)

### 更多相关话题

+   [赢得房间：创建和传达有效的数据驱动演示](https://www.kdnuggets.com/2022/04/franks-winning-room-creating-delivering-effective-data-driven-presentation.html)

+   [是什么让 Python 成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [什么使可视化效果好？](https://www.kdnuggets.com/2022/10/sphere-makes-visualization-good.html)

+   [面试加速数据科学面试课程 — 有什么特别之处](https://www.kdnuggets.com/2022/10/interview-kickstart-data-science-interview-course-makes-different.html)

+   [7 种方法让 ChatGPT 帮你编写更好、更快的代码](https://www.kdnuggets.com/2023/06/7-ways-chatgpt-makes-code-better-faster.html)

+   [避免这些 AI 新手常犯的 5 个错误](https://www.kdnuggets.com/avoid-these-5-common-mistakes-every-novice-in-ai-makes)
