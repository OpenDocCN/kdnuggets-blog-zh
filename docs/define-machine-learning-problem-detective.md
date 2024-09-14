# 如何像侦探一样定义机器学习问题

> 原文：[`www.kdnuggets.com/2018/10/define-machine-learning-problem-detective.html`](https://www.kdnuggets.com/2018/10/define-machine-learning-problem-detective.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) comments

**由[Spencer Norris](https://www.spencernorris.xyz/)，数据科学家，独立记者**。

![](img/6b7db80ffebecc19b28072006a9b5680.png)

本文最初发表在[OpenDataScience.com](https://OpenDataScience.com)。

让我们看看能否通过为我们对机器学习的基本理解奠定基础来开始这个方向——即，究竟什么构成了一个机器学习问题？这似乎是一个你可能认为自己知道答案的奇怪问题，但实际上它有一个非常正式的定义，我们将在这里概述。

### 定义机器学习问题

你可以采取的最重要的步骤是首先问自己：我认为存在模式吗？

所有机器学习问题的基本假设是*存在某种模式*。你不能在没有鸡蛋的情况下做煎蛋卷；如果没有任何模式，那么我们就已经完成了。在决定开始一个项目之前问问自己这个问题——这可能会为你节省一些心痛。

如果你仍然认为存在模式，那么我们可以继续。我们寻找的模式是某个函数`f`，它将某个输入`X`映射到输出`Y`。在符号中，这就是`f:XY`。

当然，你不知道`f`——你*不能*知道`f`，否则你就不会在读这篇文章了。这是机器学习的核心：如果我有某种模式，我不能直接观察到，如何至少可以近似它？

### 像侦探一样定义问题

像侦探一样做。如果`f`留下了证据或观察，我们可以开始重建`f`的样子。每个观察都是一个输入**xi**=[x1,x2,…xd] Ɛ Rd（即每个`xi`是一个长度为*d*的实数数组）和一个观察到的输出`yi`。这些观察{(x1,y1), (x2,y2)… (xn,yn)}统称为我们的数据集，*D*。

问题是，即使有这些观察，仍然有无限多的可能性可以解释它们。例如，取一个*D*，仅包含以下两个观察。

![](img/34fede020a28b73c80591562bb2826a8.png)

我们对什么样的函数`f`可以解释这些观察有一些很好的猜测。很可能，它是直线——对吧？

![](img/fc260dbd08bc14bd0f43cbd57e402dcb.png)

那样最有意义。但是为什么它不能是一个多项式函数呢？

![](img/578cf61a8a8fb74eccd365e37ce77cd0.png)

为什么它不能是更复杂的东西？

![](img/91523165a11be2d56f8615c486a308dd.png)

实际情况是，这些解释中的任何一种都没有完全不合理的理由。*它们都是同样有效的假设*。

那么为什么还要费心呢？如果没有办法确定真相，我们不是从一开始就注定失败了吗？

如果那是真的，你可能不会查看这一页，大多数犯罪也可能不会被解决。我们稍后会形式化这个概念，但目前，请将其视为：我们尝试根据 *可能* 的情况来制定假设。我们试图从一个无限的假设空间 *H* 中挑选出我们认为最有效的假设 *g*。

我们该如何进行呢？当然是用我们的侦探——算法 *A*。算法将负责在我们无限大的假设空间 *H* 中筛选，并找出最合适的一个。我们最终选择的那个是 *g*，我们说它足够好地近似了我们的真实函数 *f*，以至于我们可以使用它：*gf*。

这就是将机器学习问题形式化的全部内容。我们有一些未知的目标函数 *f:XY*。我们不知道 *f* 是什么，但我们有输入和输出的示例，这些示例称为 *D*。我们还有无限多的可能假设 *H*，我们将逐步缩小范围，直到找到一个看起来足够好的假设 *g*。我们将通过将 *D* 输入给 *A*，逐渐确定 *g*，以便找到最接近真实情况的答案。

### 解开谜团

在我们的侦探类比中，代理人 *A* 拥有一堆证据 *D* 以确定凶手 *f* 是谁。*A* 将在一个庞大的嫌疑人池 *H* 中筛选，直到他认为找到了一个符合条件的 *g*。没有保证 *g* 就是 *f* —— 在机器学习中，我们几乎从未有这样的保证 —— 但这是基于现有证据的最佳猜测。

如果有更多的证据，我们可能会得出不同的结论。我们有可能完全错认嫌疑人，这将是灾难性的。在实践中，你需要根据你的要求来确定算法允许多少误差（这是我们稍后会讨论的内容）。

如果是不同的代理人查看证据，他们可能会建议完全不同的嫌疑人。这就是当我们切换用于评估数据的算法时发生的事情。这也有代价，我们会讨论这个问题。在此期间，明智地选择你的算法。

当你花更多时间深入理论时，各种细微之处会浮现出来。有时候这可能是一场艰难的战斗，但从长远来看，这将使你成为更出色的机器学习从业者，并且如果你想在现实世界中使用它，它将是至关重要的。

下次我会谈论一下探索 *H* 的意义，为什么从这堆数据中挑选正确答案如此困难，以及我们为何能够做到这一点。

**个人简介**：[斯宾塞·诺里斯](https://www.spencernorris.xyz/)是一位数据科学家和自由撰稿人。他目前以承包商身份工作，并在[Medium](https://medium.com/@spencernorris)上发表文章。

[原文](https://opendatascience.com/how-to-define-a-machine-learning-problem-like-a-detective/)。经许可转载。

**相关：**

+   [机器学习——皇帝穿衣服了吗？](https://www.kdnuggets.com/2018/10/machine-learning-emperor-wearing-clothes.html)

+   [如果数据告诉你要种族主义怎么办？当算法明确惩罚时](https://www.kdnuggets.com/2018/09/siegel-when-algorithms-explicitly-penalize.html)

+   [为什么理解真相在数据科学中很重要？](https://www.kdnuggets.com/2018/01/understanding-truth-data-science.html)

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

### 进一步阅读

+   [机器学习不像你的大脑 第一部分：神经元是慢的,…](https://www.kdnuggets.com/2022/04/machine-learning-like-brain-part-one-neurons-slow-slow-slow.html)

+   [如何像老板一样进行 MLOps：无需眼泪的机器学习指南](https://www.kdnuggets.com/2023/06/mlops-like-boss-guide-machine-learning-without-tears.html)

+   [机器学习不像你的大脑 第二部分：感知器与神经元](https://www.kdnuggets.com/2022/05/machine-learning-like-brain-part-two-perceptrons-neurons.html)

+   [机器学习不像你的大脑 第三部分：基本架构](https://www.kdnuggets.com/2022/06/machine-learning-like-brain-part-3-fundamental-architecture.html)

+   [机器学习不像你的大脑 第四部分：神经元的…](https://www.kdnuggets.com/2022/06/machine-learning-like-brain-part-4-neuron-limited-ability-represent-precise-values.html)

+   [机器学习不像你的大脑 第五部分：生物神经元…](https://www.kdnuggets.com/2022/07/machine-learning-like-brain-part-5-biological-neurons-cant-summation-inputs.html)
