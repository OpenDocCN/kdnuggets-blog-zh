# 做到不可能的事？少于一个示例的机器学习

> 原文：[https://www.kdnuggets.com/2020/11/machine-learning-less-than-one-example.html](https://www.kdnuggets.com/2020/11/machine-learning-less-than-one-example.html)

[评论](#comments)

![](../Images/e18f71d0759fc5c1745c0815d776efed.png)

*“少于一个示例学习”使机器学习算法能够用少于 N 个训练示例来分类 N 个标签。*

如果我让你想象一种介于马和鸟之间的生物——比如一匹会飞的马——你是否需要看到一个具体的例子？这样的生物并不存在，但这并不妨碍我们用想象力创造一个：飞马。

![](../Images/d717c55796dfc05b6fc7b6bf5b00934d.png)

人类的思维具备各种机制，可以通过结合其对现实世界的抽象和具体知识来创造新概念。我们可以想象出我们可能从未见过的现有事物（如长脖子的马——长颈鹿），以及在现实生活中不存在的事物（如喷火的有翅蛇——龙）。这种认知灵活性使我们能够用很少甚至没有新的示例来学习新事物。

相比之下，[机器学习](https://bdtechtalks.com/2017/08/28/artificial-intelligence-machine-learning-deep-learning/)和[深度学习](https://bdtechtalks.com/2019/02/15/what-is-deep-learning-neural-networks/)，作为当前人工智能领域的领先技术，通常需要许多示例才能学习新任务，即使这些任务与它们已经知道的内容相关。

克服这一挑战促成了大量的研究工作和机器学习领域的创新。尽管我们仍然远未创造出能够复制[大脑理解能力](https://bdtechtalks.com/2020/07/13/ai-barrier-meaning-understanding/)的人工智能，但这一领域的进展是显著的。

例如，[迁移学习](https://bdtechtalks.com/2019/06/10/what-is-transfer-learning/)是一种技术，它使开发人员能够对人工神经网络进行微调，以适应新任务，而不需要大量的训练示例。少样本学习和[单样本学习](https://bdtechtalks.com/2020/08/12/what-is-one-shot-learning/)使得一个在某一任务上训练的机器学习模型可以通过一个或很少的新示例来执行相关任务。例如，如果你有一个图像分类器已经训练来检测排球和足球，你可以使用单样本学习将篮球加入它可以检测的类别中。

一种名为“少于一个示例学习”（或 LO-shot 学习）的新技术，最近由滑铁卢大学的 AI 科学家开发，将单次学习提升到了一个新水平。LO-shot 学习的理念是，要训练一个机器学习模型以检测 M 个类别，你需要每个类别少于一个样本。这项技术在 [arXiv 预印本](https://arxiv.org/abs/2009.08449) 中介绍，由 [Ilia Sucholutsky](https://ilia10000.github.io/) 和 [Matthias Schonlau](https://uwaterloo.ca/statistics-and-actuarial-science/people-profiles/matthias-schonlau) 提供，仍处于早期阶段，但显示出潜力，可以在数据不足或类别过多的各种场景中发挥作用。

### k-NN 分类器

![](../Images/407814ec1d5b83e6cc6d2180e4aad3af.png)

*k-NN 机器学习算法通过寻找最接近的实例来对数据进行分类。*

研究人员提出的 LO-shot 学习技术适用于“k-最近邻”机器学习算法。k-NN 可用于分类（确定输入的类别）或回归（预测输入的结果）任务。但为了讨论的方便，我们将重点讨论分类。

正如名字所示，k-NN 通过将输入数据与其 *k* 个最近邻进行比较来进行分类（*k* 是一个可调整的参数）。假设你想创建一个 k-NN 机器学习模型来分类手写数字。首先，你需要提供一组标记的数字图像。然后，当你给模型提供一张新的、未标记的图像时，它将通过查看其最近邻来确定其类别。

例如，如果你将 *k* 设置为 5，机器学习模型将为每个新输入找到五张最相似的数字照片。如果说，其中三张属于类别“7”，那么它将把图像分类为数字七。

k-NN 是一种“基于实例”的机器学习算法。当你提供更多每个类别的标记样本时，它的准确性会提高，但其性能会下降，因为每个新样本都会增加新的比较操作。

在他们的 LO-shot 学习论文中，研究人员展示了你可以在提供比类别少的示例时使用 k-NN 实现准确的结果。“我们提出了‘少于一个’-shot 学习（LO-shot 学习），这是一个模型必须在只有 *M < N* 个示例的情况下学习 *N* 个新类别的设置，”AI 研究人员写道。“乍一看，这似乎是不可能的任务，但我们从理论和实证上都展示了其可行性。”

### 每个类别少于一个示例的机器学习

经典的 k-NN 算法提供“硬标签”，这意味着对于每个输入，它提供了一个确切的类别。另一方面，软标签则提供输入属于每个输出类别的概率（例如，有 20% 的概率是“2”，70% 的概率是“5”，以及 10% 的概率是“3”）。

在他们的工作中，滑铁卢大学的 AI 研究人员探索了是否可以使用软标签来概括 k-NN 算法的能力。LO-shot 学习的提议是，软标签原型应该允许机器学习模型使用少于 *N* 个标记实例来分类 *N* 个类别。

该技术建立在研究人员之前关于[软标签和数据蒸馏](https://arxiv.org/abs/1910.02551)的工作基础上。论文的共同作者 Ilia Sucholutsky 告诉*TechTalks*：“数据集蒸馏是生成小型合成数据集的过程，这些数据集可以训练模型达到与使用完整训练集相同的准确度。在软标签之前，数据集蒸馏能够使用每个类别仅一个示例来表示像 MNIST 这样的数据集。我意识到，添加软标签意味着我实际上可以使用每个类别少于一个示例来表示 MNIST。”

[MNIST](http://yann.lecun.com/exdb/mnist/) 是一个手写数字图像数据库，通常用于训练和测试机器学习模型。Sucholutsky 和他的同事 Matthias Schonlau 仅使用五个合成示例，在[卷积神经网络](https://bdtechtalks.com/2020/01/06/convolutional-neural-networks-cnn-convnets/)LeNet 上实现了超过 90% 的准确度。

“这个结果真的让我很惊讶，这也是让我更广泛地思考 LO-shot 学习设置的原因，”Sucholutsky 说。

基本上，LO-shot 通过在现有类别之间划分空间来使用软标签创建新类别。

![](../Images/f6bf799d37192639d351108ea722fb60.png)

*LO-shot 学习使用软标签来划分现有类别之间的空间。*

在上面的示例中，有两个实例来调整机器学习模型（用黑点表示）。经典的 k-NN 算法会将两个点之间的空间分割为两个类别之间的空间。但是，被称为 OL-shot 学习模型的“软标签原型 k-NN”（SLaPkNN）算法，在两个类别之间创建了一个新的空间（绿色区域），该区域表示一个新标签（想象一下有翅膀的马）。在这里，我们已经实现了 *N* 个类别，具有 *N-1* 个样本。

在论文中，研究人员展示了 LO-shot 学习可以扩展到使用 *N* 个标签甚至更多的 *3N-2* 类别。

![](../Images/180a91d5144256abca75295a6bfa6261.png)

*LO-shot 学习可以扩展以每个实例获得多个类别。左侧：从四个实例获得的 10 个类别。右侧：从五个实例获得的 13 个类别。*

在他们的实验中，Sucholutsky 和 Schonlau 发现，通过正确配置软标签，LO-shot 机器学习即使在数据嘈杂的情况下也能提供可靠的结果。

“我认为 LO-shot 学习也可以从其他信息来源中获得成功——类似于许多零-shot 学习方法的做法——但软标签是最直接的方法，”Sucholutsky 说，并补充道，目前已经有几种方法可以为 LO-shot 机器学习找到合适的软标签。

虽然论文展示了 LO-shot 学习在 k-NN 分类器中的威力，但 Sucholutsky 表示，这种技术也适用于其他机器学习算法。“论文中的分析专门集中在 k-NN 上，因为它更容易分析，但它应该适用于任何可以利用软标签的分类模型，”Sucholutsky 说。研究人员很快将发布一篇更全面的论文，展示 LO-shot 学习在深度学习模型中的应用。

### 机器学习研究的新领域

![](../Images/c13bb4a384a89236bc87fe06934c78b6.png)

“对于像 k-NN 这样的基于实例的算法，LO-shot 学习的效率提升是相当显著的，特别是对于具有大量类别的数据集，”Susholutsky 说。“更广泛地说，LO-shot 学习在任何将分类算法应用于具有大量类别的数据集的场景中都很有用，特别是如果某些类别的样本很少，甚至没有样本。基本上，大多数零-shot 学习或少-shot 学习有用的场景中，LO-shot 学习也可能有用。”

例如，一个必须从图像和视频帧中识别数千个对象的 [计算机视觉](https://bdtechtalks.com/2019/01/14/what-is-computer-vision/) 系统可以从这种机器学习技术中受益，特别是当某些对象没有示例时。另一个应用是那些自然具有软标签信息的任务，例如 [自然语言处理](https://bdtechtalks.com/2018/02/20/ai-machine-learning-nlg-nlp/) 系统进行情感分析（例如，一句话可以同时是悲伤和愤怒的）。

在他们的论文中，研究人员将“少于一次”学习描述为“机器学习研究中的一种可行的新方向”。

“我们相信，创建一个专门优化 LO-shot 学习原型的软标签原型生成算法是探索这一领域的重要下一步，”他们写道。

“软标签在以前的几种设置中已经被探索过。这里的新颖之处在于我们探索它们的极端设置，”Susholutsky 说。“我认为在一次学习和零-shot 学习之间还有另一种状态，这一点并不明显。”

[原文](https://bdtechtalks.com/2020/10/01/less-than-one-shot-machine-learning/)。经许可转载。

**相关：**

+   [使用示例介绍 K 最近邻算法](https://www.kdnuggets.com/2020/04/introduction-k-nearest-neighbour-algorithm-using-examples.html)

+   [2020 年需要阅读的 AI 论文](https://www.kdnuggets.com/2020/09/ai-papers-read-2020.html)

+   [13 篇 AI 专家必读论文](https://www.kdnuggets.com/2020/05/13-must-read-papers-ai-experts.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织进行 IT 工作

* * *

### 更多相关话题

+   [机器学习算法一分钟内讲解](https://www.kdnuggets.com/2022/07/machine-learning-algorithms-explained-less-1-minute.html)

+   [KDnuggets 新闻，7月20日：机器学习算法讲解…](https://www.kdnuggets.com/2022/n29.html)

+   [少于 15 行代码实现的多模态深度学习](https://www.kdnuggets.com/2023/01/predibase-multi-modal-deep-learning-less-15-lines-code.html)

+   [在不到 6 个月的时间里成为商业智能分析师](https://www.kdnuggets.com/become-a-business-intelligence-analyst-in-less-than-6-months)

+   [为什么你需要学习不止一种编程语言！](https://www.kdnuggets.com/2022/06/need-learn-one-programming-language.html)

+   [黑色星期五优惠 - 通过 DataCamp 更低价格掌握机器学习](https://www.kdnuggets.com/2022/11/datacamp-black-friday-deal-master-machine-learning-less-datacamp.html)
