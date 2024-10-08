# 理解过拟合：机器学习中的不准确的迷思

> 原文：[`www.kdnuggets.com/2017/08/understanding-overfitting-meme-supervised-learning.html`](https://www.kdnuggets.com/2017/08/understanding-overfitting-meme-supervised-learning.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png)评论

**由 [Mehmet Suzen](http://msuzen.github.io/)，法兰克福大学**。

这篇文章的灵感来源于 [Andrew Gelman](http://andrewgelman.com/2017/07/15/what-is-overfitting-exactly/) 的一篇 [最近文章](http://andrewgelman.com/2017/07/15/what-is-overfitting-exactly/)，他将‘过拟合’定义如下：

> *过拟合是指你的模型过于复杂，导致预测效果通常比简单模型更差。*

**前言**

从业者对*过拟合*的概念有很多困惑。似乎在数据科学或相关领域流传着一种*城市传说*或*迷思*，其陈述如下：

*应用 [交叉验证](https://en.wikipedia.org/wiki/Cross-validation_(statistics)) 可以防止过拟合，而良好的样本外表现、未见数据中的低泛化误差，表明不是过拟合。*

这个说法显然是不正确的：交叉验证并不能防止你的模型过拟合，良好的样本外表现并不保证模型没有过拟合。实际上，人们在这一说法的一个方面提到的叫做*过度训练*。不幸的是，这种误解不仅在行业中传播，也存在于一些学术论文中。这可能在最佳情况下只是对*术语*的混淆。然而，如果我们在沟通结果时对*过拟合*的定义明确和清晰，这将是一个良好的实践。

**目标**

在这篇文章中，我们将直观地解释为什么 [模型验证](https://en.wikipedia.org/wiki/Regression_validation) 作为近似 [模型拟合](https://en.wikipedia.org/wiki/Goodness_of_fit) 的泛化误差和检测过拟合不能在单个模型上同时解决。我们将在一些概念介绍之后，通过一个具体的示例工作流程来理解*过拟合*、*过度训练*以及一个典型的最终模型构建阶段。我们将避免提及贝叶斯解释和正则化，限制讨论在回归和交叉验证范围内。虽然正则化由于其数学性质和先验分布在贝叶斯统计中有不同的影响，但我们假设读者具备机器学习的基础知识，因此这不是初学者教程。

最近，贝叶斯大师[安德鲁·盖尔曼](https://en.wikipedia.org/wiki/Andrew_Gelman)关于*什么是过拟合？*的提问，是本帖开发的原因之一，加上我看到从业者对*过拟合*的含义模糊不清，以及最近发表的关于数据科学的技术文章和一些学术论文中仍然流传的错误观点，我感到沮丧。

**我们在监督学习中需要满足什么？**

数学中最基本的任务之一是找到一个函数的解：如果我们将自己限制在*n*维实数上，并且我们的兴趣领域是**R**^(*n*)。现在想象在这个领域中存在一组*p*点，这些点形成一个数据集，这实际上是函数的*a 部分* 解。建模的主要目的是寻找数据集的解释，这意味着我们需要确定*m*个参数，a∈**R**^m，这些参数是未知的。（请注意，非参数模型并不意味着没有参数。）从数学上讲，这表现为我们之前所说的函数 f(x,a)。这个建模通常称为*回归*、*插值*或*监督学习*，这取决于你所阅读的文献。这是[逆问题](https://en.wikipedia.org/wiki/Inverse_problem)的一种形式，我们不知道参数，但对变量有部分信息。主要问题是解决方案不是[良构的](https://en.wikipedia.org/wiki/Well-posed_problem)。省略公理技术细节，实际问题是我们可以找到许多函数 f(x,a)或模型来解释数据集。

因此，我们希望我们的模型解决方案满足以下两个概念，f(x,a)=0。

1. 泛化：模型不应依赖于数据集。这个步骤称为*模型验证*。

2. 最小复杂度：一个模型应遵循[奥卡姆剃刀或简约原则](https://en.wikipedia.org/wiki/Occam%27s_razor)。这个步骤称为*模型选择*。

![](img/9867ff660ef3f68613dfff30792938af.png)

图 1：监督学习中模型验证和选择的工作流程。

模型的泛化可以通过[拟合优度](https://en.wikipedia.org/wiki/Goodness_of_fit)来衡量。这本质上告诉我们我们的模型（选定的函数）对数据集的解释有多好。要找到一个*最小复杂度*的模型，需要与另一个模型进行比较。

到目前为止，我们还没有命名一种技术来检查一个模型是否经过泛化并被选为最佳模型。不幸的是，目前没有唯一的方式来同时做到这两点，这正是数据科学家或量化从业者的任务，需要人类判断。

**模型验证：一个例子**

检查模型是否足够通用的一种方法是制定一个度量来评估它解释数据集的好坏。我们在模型验证中的任务是估计模型误差。例如，[均方根偏差](https://en.wikipedia.org/wiki/Root-mean-square_deviation) (RMDS) 是一个可以使用的度量。如果 RMSD 较低，我们可以说我们的模型拟合得很好，理想情况下它应该接近零。但如果我们使用相同的数据集来衡量拟合优度，它的通用性就不够。我们可以尽可能使用不同的数据集，特别是样本外数据集来验证这一点，即所谓的持出方法。样本外仅仅是说我们没有使用相同的数据集来寻找参数值 a。改进的方法是交叉验证。我们将数据集分成 k 个部分，并获得 k 个 RMDS 值来取平均。这在图 1 中总结了。请注意，相同模型的不同参数化并不构成不同的模型。

**模型选择：过拟合的检测**

当我们尝试满足“最简复杂模型”时，过拟合就会出现。这是一个比较问题，我们需要多个模型来判断一个给定模型是否过拟合。道格拉斯·霍金斯在他的经典论文 *[过拟合问题](http://pubs.acs.org/doi/abs/10.1021/ci0342472)* 中指出：

> *模型的过拟合被广泛认为是一个问题。然而，过拟合并不是绝对的，而是涉及比较的。如果一个模型比另一个拟合效果相同的模型更复杂，那么这个模型就是过拟合的。*

这里的重要点是我们所说的复杂模型是什么意思，或者我们如何量化模型复杂性？不幸的是，这没有唯一的方法。最常用的方法之一是，拥有更多参数的模型变得更复杂。但这仍然是一种*流行观念*，并不完全正确。实际上，可以使用不同的复杂性度量。例如，根据这个定义，f1(a,x)=ax 和 f2(a,x)=ax² 具有相同的复杂性，因为它们有相同数量的自由参数，但直观上 f2 更复杂，尽管它是非线性的。还有许多基于信息论的复杂性度量，但讨论这些超出了我们帖子的范围。为了演示的目的，我们将更多的参数和非线性的程度视为模型的复杂性。

![](img/9b071de1261d03d7b516f9dbb9c90fa4.png)

图 2：模拟数据与数据的非随机部分。

**实际示例**

我们直观地覆盖了为什么我们不能同时解决模型验证和判断过拟合的问题。现在尝试用一个简单的数据集和模型来展示这一点，同时基本捕捉上述前提。

通常的程序是从模型生成一个合成数据集或模拟数据集，作为标准数据集，并利用这个数据集构建其他模型。我们使用以下的功能形式，来自[经典文本《Bishop》](http://www.springer.com/de/book/9780387310732?referer=www.springer.de)，但加入了高斯噪声。

f(x)=sin(2πx)+N(0,0.1)。

我们生成一个足够大的数据集，100 个点，以避免《Bishop》书中讨论的样本大小问题，见图 2。我们决定应用两个模型到这个数据集上的监督学习任务中。注意，我们在这里不会讨论贝叶斯解释，因此这些模型在强先验假设下的等价性不是问题，因为我们使用这个示例是为了方便演示概念。我们使用了一个三次和六次的多项式模型，分别称之为 g(x) 和 h(x)，用来从模拟数据中学习。

g(x)=a[0] + a[1]x + a[2]x² + a[3]x³

和

h(x)=b[0] + b[1]x + b[2]x² + b[3]x³ + b[4]x⁴ + b[5]x⁵ + b[6]x⁶![](img/1da9363012227110e0b6705489ec5899.png)

图 3：g(x) 在数据使用量达到 40% 之后出现过训练现象。

**过训练并非过拟合**

*过训练* 意味着模型在学习模型参数对目标变量的性能下降，而目标变量会影响模型的构建，例如，目标变量可以是训练数据的大小或神经网络中的迭代周期。这在神经网络中更为普遍（见[Dayhoff 2011](http://onlinelibrary.wiley.com/doi/10.1002/1097-0142(20010415)91:8%2B%3C1615::AID-CNCR1175%3E3.0.CO;2-L/full)）。在我们的实际示例中，这将表现为在 g(x) 模型中使用保留方法测量 RMSD。换句话说，找到用于训练模型的最佳数据点数量，以在未见数据上提供更好的性能，见图 3 和 4。

**低验证误差的过拟合**

我们还可以估计 10 折交叉验证误差，CV-RMSD。对于这次采样，g 和 h 的 CV-RMSD 分别为 0.13 和 0.12。正如我们所见，我们遇到了一种更复杂的模型在交叉验证中达到类似的预测能力的情况，我们不能仅通过查看 CV-RMSD 值或从图 4 中检测‘过训练’曲线来区分这种过拟合。我们需要比较两个模型，因此需要图 3 和 4，并查看 CV-RMSD 值。我们可以认为，在小数据集中，我们可能通过查看测试和训练误差的差异来辨别，这正是《Bishop》解释的*过拟合*；他指出了小数据集中的*过训练*。

**部署哪个训练模型？**

现在的问题是，我们已经通过经验发现了性能最佳且复杂度最低的模型。一切都很好，但我们应该在生产中使用哪个训练模型呢？

实际上我们已经在模型选择中建立了模型。在上述情况下，由于我们得到了相似的结果

从 g 和 h 的预测能力来看，我们显然会使用 g，基于图 3 中的划分甜点进行训练。

![](img/6ede4111ec5dc636d31fae03616c4321.png)

图 4：在数据使用达到 30% 左右时，*过度训练* 发生

**结论**

这里的核心信息是良好的验证性能并不能保证检测到一个*过拟合*的模型。正如我们从使用一维合成数据的示例中看到的那样。*过度训练* 实际上是大多数从业者在使用“过拟合”一词时的意思。

**展望**

随着越来越多的人在学术界和工业界使用机器学习或反问题的技术，一些关键技术概念发生了偏差，并对不同的人有不同的定义和含义，因为人们从阅读文献中学习某些概念的方式往往不是很细致，而是通过直线经理或高级同事的口头解释。这就产生了*迷因*，这些迷因实际上是错误的，或者至少在术语上造成了很多混乱。作为从业者，我们必须*质疑*所有技术概念，并尝试从已发布的科学文献中寻求源头，而不是完全依赖于经验丰富的同事的口头解释。同时，我们应该强烈避免嘲笑同事提出的问题，即使这些问题听起来过于简单，毕竟我们永远在学习，而看似幼稚的问题可能在领域的基础上具有非常重要的影响。

**附言** 如上所述，写这篇文章的灵感来源于 Gelman 最近的一篇文章 ([帖子](http://andrewgelman.com/2017/07/15/what-is-overfitting-exactly/))。他将“过拟合”定义如下：

> *过拟合是指你有一个复杂的模型，其平均预测效果比一个简单模型更差。*

除去两个模型的先验和等效性，Gelman 的定义比 Hawkins 的定义更弱，他接受一个具有相似预测能力的复杂模型。因此，如果我们使用 Gelman 的定义，部署我们上面示例中的 *g* 或 *h* 都是可以的。但从 Hawkins 的角度来看，我们需要严格部署 *g*。

[原文](http://memosisland.blogspot.de/2017/08/understanding-overfitting-inaccurate.html)。经许可转载。

**简历：** [**Dr. Mehmet Suzen**](http://msuzen.github.io/) 是一位理论物理学家和研究科学家。他在 [Memos’ Island](https://memosisland.blogspot.de/) 博客上撰写数据科学相关内容。

**相关：**

+   使预测模型稳健：Holdout vs Cross-Validation

+   关于贝叶斯先验和过拟合的真相

+   如何用数据撒谎

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT

* * *

### 更多相关内容

+   [如何避免过拟合](https://www.kdnuggets.com/2022/08/avoid-overfitting.html)

+   [KDnuggets 新闻，8 月 24 日：在 Python 中实现 DBSCAN • 如何…](https://www.kdnuggets.com/2022/n34.html)

+   [缩小人类理解与机器学习之间的差距：…](https://www.kdnuggets.com/2023/06/closing-gap-human-understanding-machine-learning-explainable-ai-solution.html)

+   [理解机器学习算法：深入概述](https://www.kdnuggets.com/understanding-machine-learning-algorithms-an-indepth-overview)

+   [理解监督学习：理论与概述](https://www.kdnuggets.com/understanding-supervised-learning-theory-and-overview)

+   [免费 MIT 微积分课程：理解深度学习的关键](https://www.kdnuggets.com/2020/07/free-mit-courses-calculus-key-deep-learning.html)
