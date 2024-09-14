# 为什么要从头实现机器学习算法？

> 原文：[https://www.kdnuggets.com/2016/05/implement-machine-learning-algorithms-scratch.html](https://www.kdnuggets.com/2016/05/implement-machine-learning-algorithms-scratch.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

从头实现算法有几个不同的原因可能会很有用：

1.  它可以帮助我们理解算法的内部工作原理。

1.  我们可以尝试更高效地实现一个算法。

1.  我们可以为算法添加新特性或实验不同的核心概念变体。

1.  我们可以绕过许可问题（例如，Linux vs. Unix）或平台限制。

1.  我们想发明新算法或实现尚未实现/分享的算法。

1.  我们对现有的 API 不满意，或者我们想将其更“自然”地集成到现有的软件库中。

![机器学习分类](../Images/757705b20ace87e918f33e32ca073999.png)

让我们在上述六点的背景下进一步缩小“从头实现”这个短语的范围。当我们谈论“从头实现”时，我们需要缩小范围，使这个问题真正切实可行。让我们讨论一个具体的算法，简单的逻辑回归，以便用具体的例子来解决不同的点。我认为逻辑回归已经实现了不下于千次。

我们仍然希望从头实现逻辑回归的一个原因可能是我们觉得自己没有完全理解它的工作原理；我们读了一些论文，虽然理解了核心概念。通过编程语言进行原型设计（例如，Python、MATLAB、R 等），我们可以从论文中获取想法，并尝试逐步用代码表达它们。一个成熟的库，如 scikit-learn，可以帮助我们再次检查结果，看看我们的实现——我们对算法如何工作的理解——是否正确。在这里，我们并不真正关心效率；尽管我们花了很多时间实现算法，但如果我们想在研究实验室或公司中进行一些严肃的分析，我们可能还是会想使用成熟的库。成熟的库通常更值得信赖——它们经过了许多人使用的考验，可能已经遇到了一些边界情况，并确保没有奇怪的意外。此外，这些代码也更有可能随着时间的推移被高度优化以提高计算效率。在这里，从头实现的目的仅仅是为了自我评估。阅读一个概念是一回事，但将其付诸实践则是对理解的更高层次——能够向他人解释则是锦上添花。

另一个我们可能希望从头开始重新实现逻辑回归的原因是我们对其他实现的“特性”不满意。让我们天真地假设其他实现没有正则化参数，或者不支持多类设置（即通过一对多、一对一或softmax）。或者如果计算（或预测）效率是一个问题，也许我们想使用另一种求解器（例如牛顿法与梯度下降法与随机梯度下降法等）来实现它。但是，关于计算效率的改进不一定需要通过算法的修改来实现，我们可以使用更底层的编程语言，例如使用Scala代替Python，或使用Fortran代替Scala……这可能涉及到汇编语言或机器代码，甚至设计一个优化用于运行这种分析的芯片。然而，如果你是一个机器学习（或“数据科学”）从业者或研究人员，这可能是你应该委托给软件工程团队的事情。

![决策树伪代码](../Images/a6c56537d0cfda62a29f8883c70c6a7f.png)

回到主要问题：不同的人因各种原因从头实现算法。就个人而言，当我从头实现算法时，是因为学习的体验。

**简介： [Sebastian Raschka](https://twitter.com/rasbt)** 是一位“数据科学家”和机器学习爱好者，对Python和开源有极大热情。《[Python机器学习](https://www.packtpub.com/big-data-and-business-intelligence/python-machine-learning)》的作者。密歇根州立大学。

[原文](https://github.com/rasbt/python-machine-learning-book/blob/master/faq/implementing-from-scratch.md)。经许可转载。

**相关：**

+   [深度学习何时优于支持向量机或随机森林？](/2016/04/deep-learning-vs-svm-random-forest.html)

+   [分类发展作为学习机器](/2016/04/development-classification-learning-machine.html)

+   [十大数据挖掘算法解析](/2015/05/top-10-data-mining-algorithms-explained.html)

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT

* * *

### 更多相关话题

+   [如何使用医疗数据实施联邦学习项目](https://www.kdnuggets.com/2023/02/implement-federated-learning-project-healthcare-data.html)

+   [机器学习算法 - 什么、为什么以及如何？](https://www.kdnuggets.com/2022/09/machine-learning-algorithms.html)

+   [一个简单易实施的端到端项目，使用HuggingFace](https://www.kdnuggets.com/a-simple-to-implement-end-to-end-project-with-huggingface)

+   [通过数据隐私学习如何实施技术隐私解决方案…](https://www.kdnuggets.com/2022/04/manning-data-privacy-learn-implement-technical-privacy-solutions-tools-scale.html)

+   [学习如何设计、测量和实施可靠的A/B测试…](https://www.kdnuggets.com/2023/01/sphere-design-measure-implement-trustworthy-ab-tests-ronny-kohavi.html)

+   [如何使用LangChain实现Agentic RAG：第一部分](https://www.kdnuggets.com/how-to-implement-agentic-rag-using-langchain-part-1)
