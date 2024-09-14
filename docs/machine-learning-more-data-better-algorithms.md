# 机器学习中，哪个更好：更多数据还是更好的算法？

> 原文：[https://www.kdnuggets.com/2015/06/machine-learning-more-data-better-algorithms.html](https://www.kdnuggets.com/2015/06/machine-learning-more-data-better-algorithms.html)

[评论](#comments)

**由 Xavier Amatriain**（Quora 的工程副总裁）。

“在机器学习中，更多数据是否总是比更好的算法更好？” 不一定。更多数据有时会有帮助，有时则没有。

可能最著名的支持数据力量的名言是 Google 的研究总监 Peter Norvig 宣称的 “我们没有更好的算法。我们只是有更多的数据。”。这句话通常与 Norvig 本人合著的《数据的非理性有效性》一文有关（你可能可以在网上找到 pdf，虽然 [原文](https://googleresearch.blogspot.com/2009/03/unreasonable-effectiveness-of-data.html) 被 IEEE 付费墙挡住了）。当 Norvig 被误引为说 “所有模型都是错误的，你根本不需要它们” 时，关于更好模型的最后一根钉子就被钉上了（阅读 [这里](http://norvig.com/fact-check.html) 了解作者对如何被误引的澄清）。

![](../Images/19fca3acedbb3ad590829f7049690d06.png)

Norvig 等人在他们的文章中提到的效果，早在微软研究员 Banko 和 Brill 的著名论文《Scaling to Very Very Large Corpora for Natural Language Disambiguation》[2001] 中就已经被捕捉到了。在那篇论文中，作者展示了下面的图表。

![](../Images/d5daaabc8d7789addd6dea6cbfa04418.png)

该图显示，对于给定的问题，非常不同的算法几乎表现相同。然而，向训练集添加更多示例（词汇）会单调地提高模型的准确性。

所以，你可能会认为案子已经结案了。实际上，情况并非如此。现实是，Norvig 的论断和 Banko 与 Brill 的论文在某种背景下都是正确的。但它们有时会被引用在与原始背景完全不同的上下文中。不过，为了理解为什么如此，我们需要稍微深入一些技术细节。（我不打算在这篇文章中提供完整的机器学习教程。如果你不理解我下面解释的内容，请阅读我对 [如何学习机器学习？](https://www.quora.com/How-do-I-learn-machine-learning-1/answer/Xavier-Amatriain) 的回答。）

**方差还是偏差？**

基本的想法是，模型表现不佳可能有两个可能（且几乎相反）的原因。

在第一种情况下，我们可能会有一个模型对我们拥有的数据量过于复杂。这种情况被称为*高方差*，导致模型过拟合。我们知道当训练误差远低于测试误差时，我们面临的是高方差问题。高方差问题可以通过减少特性数量来解决，并且…是的，通过增加数据点的数量来解决。那么，Banko & Brill 和 Norvig 处理的是哪种模型？是的，你猜对了：高方差。在这两种情况下，作者们都在处理语言模型，其中词汇表中的每个词大致都会成为一个特性。这些模型与训练示例相比，特性数量非常多。因此，它们很可能会过拟合。是的，在这种情况下，添加更多示例将有帮助。

但是，相反的情况是，我们可能会有一个模型过于简单，无法解释我们拥有的数据。在这种情况下，被称为*高偏差*，添加更多的数据是没有帮助的。下面是Netflix的真实生产系统的一个图示，以及随着我们添加更多训练示例时的性能。

![](../Images/7fc45f89a72a843c9fdae7b364438f4d.png)

所以，不，**更多数据并不总是有帮助**。正如我们刚刚看到的，有许多情况添加更多示例到我们的训练集中不会改善模型的性能。

**更多特性来救援**

如果你到目前为止跟上了我的思路，并且你已经做了功课理解了高方差和高偏差问题，你可能会认为我故意遗漏了某些讨论内容。是的，高偏差模型不会从更多的训练示例中受益，但它们可能会非常受益于更多的特性。所以，最终，这一切都是关于添加“更多”数据，对吗？嗯，再次，这要视情况而定。

以Netflix奖为例。在游戏初期，有[一篇博客文章](http://anand.typepad.com/datawocky/2008/03/more-data-usual.html)由连续创业者和斯坦福大学教授[Anand Rajaraman](https://en.wikipedia.org/wiki/Anand_Rajaraman)评论使用额外特性来解决问题。文章解释了一个学生团队通过添加来自IMDb的内容特性来提高预测准确率。

![](../Images/e5868e9ed42194643ec16d1e553f347c.png)

从 retrospect 来看，很容易批评这篇文章因为从一个数据点做了过度概括。更重要的是，[后续文章](http://anand.typepad.com/datawocky/2008/04/data-versus-alg.html)提到了SVD作为一种“复杂”的算法，不值得尝试，因为它限制了扩展到更多特性的能力。显然，Anand的学生没有赢得Netflix奖，他们现在可能意识到SVD确实在获胜条目中扮演了重要角色。

事实上，许多团队后来展示了，将来自 IMDB 或类似来源的内容特征添加到优化算法中几乎没有任何改善。[重力团队](http://www.gravityrd.com/references/netflix-prize?lang=en)，作为奖项的顶级竞争者之一，发布了一篇详细的论文，展示了这些基于内容的特征如何对高度优化的协同过滤矩阵分解方法没有任何改善。该论文题为[推荐新电影：即使是少量评分也比元数据更有价值](http://dl.acm.org/citation.cfm?id=1639731&dl=ACM&coll=DL&CFID=122239967&CFTOKEN=16331362)。

![](../Images/55db293f30aba4d9ca8f500e583052e5.png)

公平地说，论文的标题也是一种过度概括。基于内容的特征（或一般的不同特征）在许多情况下可能能够提高准确性。但你明白我的观点：**更多的数据并不总是有帮助的**。

**更好的数据 != 更多的数据（新增此部分** ***以回应一个评论）***

需要指出的是，在我看来，更好的数据总是更好。这一点没有争论。所以你可以投入任何精力去“改善”你的数据，总是值得的。问题是，更好的数据并不意味着**更多**的数据。事实上，有时这可能意味着**更少**！

以数据清洗或异常值移除作为我观点的一个简单例子。但，还有许多其他更微妙的例子。例如，我见过有人花费大量精力实施分布式[矩阵分解](https://www.quora.com/Matrix-Factorization)，而实际上他们可能只需对数据进行采样就能得到非常相似的结果。事实上，对你的人群进行某种形式的智能采样（例如，使用分层抽样）可以比使用完整的未过滤数据集得到更好的结果。

**科学方法的终结？**

当然，每当关于可能的范式变化进行激烈辩论时，总会有像马尔科姆·格拉德威尔或克里斯·安德森这样的人通过加剧讨论来谋生（不要误解我的意思，我是他们的粉丝，并且读过他们的大部分书籍）。在这种情况下，安德森挑剔了一些诺维格的评论，并在一篇题为[理论的终结：数据洪流使科学方法过时](http://www.wired.com/science/discoveries/magazine/16-07/pb_theory/)的文章中对其进行了误引。

![](../Images/b899e2f67ba919e641f8e16f215f3bff.png)

-   文章解释了数据丰富如何帮助个人和公司做出决策，即使不理解数据本身的意义。如Norvig在[他的反驳](http://norvig.com/fact-check.html)中指出，Anderson有些观点是正确的，但为了证明这些观点却走得太远。结果是一系列错误的陈述，从标题开始：数据洪流并没有使科学方法过时。我认为恰恰相反。

**没有良好方法的数据 = 噪声**

所以，我是否在试图表明大数据革命只是炒作？绝对不是。拥有更多数据，无论是更多的样本还是更多的特征，都是一种福祉。数据的可用性带来了更多更好的洞察和应用。更多的数据确实使方法更好。更重要的是，它**要求**更好的方法。

![](../Images/a3465a3c529b650114d425d05eb9c1ac.png)

-   总之，我们应该摒弃那些简单化的声音，它们宣称理论或模型毫无用处，或者数据战胜了这些。数据固然重要，但同样需要好的模型和解释它们的理论。总体来说，我们需要的是有效的方法，帮助我们理解如何解读数据、模型以及两者的局限性，从而产生最佳的输出。

-   换句话说，数据很重要。但没有良好方法的数据就成了噪声。

**[原文](https://www.quora.com/In-machine-learning-is-more-data-always-better-than-better-algorithms/answer/Xavier-Amatriain)**。已获得许可转载。

**简介：** [Xavier Amatriain](http://xavier.amatriain.net/)，是Quora的工程副总裁，因其在推荐系统和机器学习方面的工作而闻名。他建立了团队和算法，以解决具有商业影响的难题。此前，他曾是Netflix的研究/工程总监。

**相关**

+   [揭穿大数据神话，再次](/2015/01/debunking-big-data-myths-again.html)

+   [采访：Josh Hemann，Activision谈为什么对模糊性的容忍至关重要](/2015/03/interview-josh-hemann-activision-data-science.html)

+   [深度学习会取代机器学习，使其他算法过时吗？](/2014/10/deep-learning-make-machine-learning-algorithms-obsolete.html)

* * *

## -   我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的IT工作

* * *

### 更多相关话题

+   [15 本更多的免费机器学习和深度学习书籍](https://www.kdnuggets.com/2022/11/15-free-machine-learning-deep-learning-books.html)

+   [KDnuggets 新闻，6 月 22 日：主要监督学习算法…](https://www.kdnuggets.com/2022/n25.html)

+   [机器学习中使用的主要监督学习算法](https://www.kdnuggets.com/2022/06/primary-supervised-learning-algorithms-used-machine-learning.html)

+   [为什么越来越多的开发者选择 Python 进行机器学习项目？](https://www.kdnuggets.com/2022/01/developers-python-machine-learning-projects.html)

+   [评估机器学习模型的（更好）方法](https://www.kdnuggets.com/2022/01/much-better-approach-evaluate-machine-learning-model.html)

+   [基本机器学习算法：初学者指南](https://www.kdnuggets.com/2021/05/essential-machine-learning-algorithms-beginners.html)
