# 机器学习中的偏差全都不好吗？

> 原文：[https://www.kdnuggets.com/2019/07/bias-machine-learning-bad.html](https://www.kdnuggets.com/2019/07/bias-machine-learning-bad.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)![图像名称](../Images/1efd83fa84124475a2da048279623546.png)

Tom M. Mitchell 在1980年发表了一篇论文：[学习归纳中的偏差需求](http://dml.cs.byu.edu/~cgc/docs/mldm_tools/Reading/Need%20for%20Bias.pdf?source=post_page---------------------------)中指出：

> 学习涉及从过去的经验中归纳，以应对与这些经验“相关”的新情况。处理新情况所需的归纳飞跃似乎只能在选择某种归纳而非另一种的特定偏差下才可能实现。本文准确地定义了归纳问题中的偏差概念，并表明偏差对于归纳飞跃是必要的。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在IT领域

* * *

在我们多年的预测模型构建过程中，我们被教导偏差会损害我们的模型。偏差控制需要由能够区分正确和错误偏差的人来掌握。

他的论文指出，某些偏差有助于我们为手头的问题创建适当的模型：

1.  **领域的事实知识**

+   在为特定目的学习归纳时，可能会出现以下情况

    通过参考关于任务领域的知识来限制考虑的归纳。比如在一个学习棒球规则的程序中（Soloway & Riseman 1978），关于竞争性游戏的一般知识限制了考虑的归纳数量。这种先验知识可以为所考虑的归纳提供强有力、合理的约束。在这种情况下，归纳系统的目标变为“确定与训练实例及任务领域中的其他已知事实一致的归纳”。

1.  **学习到的归纳的预期用途**

+   对学习到的泛化的预期用途的知识可以为学习提供强大的偏差。举个简单的例子，如果学习到的泛化的预期用途涉及到错误的正类比错误的负类分类更高的成本，那么学习程序应该更倾向于选择更具体的泛化，而不是选择那些与相同训练数据一致的更一般的替代方案。

1.  **关于训练数据来源的知识**

+   关于训练实例来源的知识也可以为学习提供有用的约束。例如，在从人类教师那里学习时，我们似乎利用了关于存在一个有组织课程的许多假设来约束我们对合适泛化的搜索。在有组织的课程中，我们的注意力会集中在实例的特定特征上，这种方式

    消除关于哪个可能的泛化最合适的模糊性。

1.  **倾向于简单性和普遍性**

    +   人类似乎使用的一种偏差是倾向于简单、一般的解释。可解释性使我们能够向外行观众传达我们的模型，而黑箱模型可能会使模型构建者陷入困境。

1.  **与之前学到的概念的类比**

+   如果一个系统正在学习一系列相关的概念或泛化，那么对这些概念的泛化可能会受到其他概念成功泛化的制约。例如，考虑学习块世界对象的结构描述任务，如“拱门”、“塔”等。学习了几个概念后，学到的描述可能会揭示某些特征在描述这一类概念时比其他特征更为重要。例如，如果泛化语言包含了结构中每个块的“形状”、“颜色”和“年龄”等特征，系统可能会注意到“年龄”很少是一个相关特征。

    用于描述结构类别的偏差，可能会产生对其他特征的偏好。这种学习到的偏差的理由必须基于对所学习概念预期用途的某种相似性。

尽管这篇论文的时间是1980年，但其背后的逻辑确实让我们思考如何区分模型中的必要偏差和不必要的偏差。显然，我们必须消除那些会成为障碍的偏差，但随着我们不断实践并深入挖掘数据，我们可能会意识到一些偏差实际上可以使我们的模型更接近我们真正想要的效果。

在后续文章中，我将讨论在机器学习中应该避免的偏差类型。

**相关**：

+   [大数据的三个主要问题及解决方法](/2019/04/3-big-problems-big-data.html)

+   [设计伦理算法](/2019/03/designing-ethical-algorithms.html)

+   [算法并非偏见，我们才是](/2019/01/algorithms-arent-biased-we-are.html)

### 更多相关话题

+   [数据质量：优、劣与丑](https://www.kdnuggets.com/2022/01/data-quality-good-bad-ugly.html)

+   [揭秘糟糕的科学](https://www.kdnuggets.com/2022/01/demystifying-bad-science.html)

+   [3分钟了解偏差-方差权衡](https://www.kdnuggets.com/2020/09/understanding-bias-variance-trade-off-3-minutes.html)

+   [处理推荐系统和搜索中的位置偏差](https://www.kdnuggets.com/2023/03/dealing-position-bias-recommendations-search.html)

+   [偏差-方差权衡](https://www.kdnuggets.com/2022/08/biasvariance-tradeoff.html)

+   [免费的数据科学学习路线图：适用于所有级别，与IBM合作](https://www.kdnuggets.com/a-free-data-science-learning-roadmap-for-all-levels-with-ibm)
