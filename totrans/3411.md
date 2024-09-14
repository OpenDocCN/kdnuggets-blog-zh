# 自动化数据科学与机器学习：对 Auto-sklearn 团队的采访

> 原文：[https://www.kdnuggets.com/2016/10/interview-auto-sklearn-automated-data-science-machine-learning-team.html](https://www.kdnuggets.com/2016/10/interview-auto-sklearn-automated-data-science-machine-learning-team.html)

KDnuggets 最近举办了一场 [自动化数据科学和机器学习博客比赛](/2016/06/kdnuggets-blog-contest-automated-data-science.html)，收到了众多参赛作品，并对获奖文章及一对荣誉提名表示了高度赞赏。

获奖文章题为 [比赛获胜者：用 Auto-sklearn 赢得 AutoML 挑战](/2016/08/winning-automl-challenge-auto-sklearn.html)，由 Matthias Feurer、Aaron Klein 和 Frank Hutter 共同撰写，均来自弗赖堡大学，概述了 [Auto-sklearn](http://automl.github.io/auto-sklearn/stable/)，这是一个开源 Python 工具，能够自动确定适用于分类和回归数据集的有效机器学习管道。该项目围绕成功的 scikit-learn 库构建，并赢得了最近的 AutoML 挑战。

鉴于这篇文章的受欢迎程度，我们询问了作者们是否有兴趣回答一些关于他们自己、他们的项目以及自动化数据科学的一般性后续问题。以下是这次对话的结果。

![Auto-sklearn 概述](../Images/a92a06f67c259ed9e81f5f88d0c81352.png)

**Matthew Mayo**：首先，恭喜你们赢得了 KDnuggets 自动化数据科学和机器学习博客比赛，展示了你们的项目 Auto-sklearn。如果我们从介绍一下团队成员开始，并提供每个人的一些背景信息怎么样？

**Matthias Feurer**：我是 Frank 组的二年级博士生，主要从事超参数优化和自动化机器学习工作。我主要关注于优化预定义的机器学习管道。在攻读硕士学位期间，我开始为 Frank 工作，因为在那时的大多数研究项目中，我都对超参数调整感到厌烦。

**Aaron Klein**：我也是一名二年级博士生，专注于自动化深度学习。像 Matthias 一样，在加入 Frank 组之前，我曾是弗赖堡大学的硕士生。

**Frank Hutter**：我是弗赖堡大学计算机科学的助理教授，主要兴趣领域包括人工智能、机器学习和自动化算法设计。在搬到弗赖堡之前，我在加拿大温哥华的英属哥伦比亚大学工作了九年。

**All**：除了我们三个人（为 KDnuggets 博客比赛撰写了博客文章），我们获奖提交的团队还包括来自弗赖堡大学的几位博士生和博士后：Katharina Eggensperger、Jost Tobias Springenberg、Hector Mendoza、Manuel Blum、Stefan Falkner 和 Marius Lindauer。

**这篇文章信息丰富，对Auto-sklearn的描述非常到位。是否有任何额外的信息您希望我们的读者了解Auto-sklearn，或者自这篇文章以来发生了什么进展？您能否分享一下未来的发展计划？**

短期目标是回归分析，我们可以在这方面做得更多。从长远来看，我们希望Auto-sklearn成为scikit-learn的一个灵活扩展，帮助用户优化他们的机器学习流程。我们还希望在Auto-Net的方向上做更多的工作，并希望通过对数据集、数据子集以及时间（针对任何时间算法）的更多推理来大幅加快优化过程。

**您认为机器学习和数据科学能在多大程度上实现自动化？所谓的完全自动化系统需要多少人类互动？**

尽管有几种方法可以调整机器学习流程的超参数，但迄今为止，发现新的流程构建块的工作还很少。Auto-sklearn使用预定义的一组预处理器和分类器，按照固定的顺序进行。一个有效的方法是提出新的流程，这将非常有帮助。当然可以继续这个思路，尝试像最近几篇论文中那样自动发现新算法，例如*通过梯度下降学习梯度下降*。当机器学习模型的训练非常昂贵时，例如用于大数据集的最先进的深度神经网络，人类仍然能比自动化方法更好地调整超参数。我们正在研究将人类专家启发式方法转化为完全形式化算法的方法；例如，我们的[ Fabolas 方法](https://arxiv.org/abs/1605.07079)​优化神经网络在数据小子集上的超参数，以加速学习整个数据集的最佳超参数。

**考虑到之前的问题，数据科学家是否会很快失业？或者，如果这个想法过于激进，当前对数据科学家的热潮是否会在不久的将来被自动化所缓和？如果是的话，会到什么程度？**

当然不会。所有的自动化机器学习方法都是为了支持数据科学家，而不是替代他们。这些方法可以解放数据科学家，让他们从那些机器更擅长解决的繁琐、复杂任务（如超参数优化）中解脱出来。但分析和得出结论仍然需要由人类专家来完成——特别是了解应用领域的数据科学家将仍然极其重要。不过，我们确实相信，自动化会使个体数据科学家的生产力大大提高，因此这可能会影响所需的数据科学家数量。

**数据科学家能做些什么来避免被淘汰？当然，这个问题是为了增加价值，而不是捣乱。**

数据科学家始终需要分析和解释统计分析的结果，因此对于刚开始从事数据科学工作的年轻毕业生，这些技能可能比其他一些技能更具未来保障（例如，手动超参数调优以最大化神经网络的效果）。

**由于你过去积极参与机器学习竞赛，你有什么有趣的技巧、窍门或见解可以分享吗？**

自动化和精心的重采样策略。自动化允许运行大量实验，而诸如精心交叉验证等重采样策略则有助于防止过拟合。通常，带着开放的心态进入也是很重要的，让数据说明哪种方法在不同的数据集上效果最好。

**最后，你认为机器学习技术在 5 年内会处于什么位置？**

很难预测未来会发生什么，尤其是在机器学习这样的快速变化领域。例如，五年前并没有多少人预见到深度学习的崛起。但我们相当确信，机器学习将会被越来越多地使用，并嵌入到每个人使用的商业工具中。

**感谢你的时间。我知道你的时间非常宝贵，我们感谢你抽出几分钟来为我们的读者提供这些信息。**

**相关**：

+   [竞赛获胜者：使用 Auto-sklearn 赢得 AutoML 挑战](/2016/08/winning-automl-challenge-auto-sklearn.html)

+   [竞赛第二名：数字广告中的自动化数据科学和机器学习](/2016/08/automated-data-science-digital-advertising.html)

+   [竞赛第二名：自动化数据科学](/2016/08/automating-data-science.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 方面

* * *

### 更多相关主题

+   [停止学习数据科学以寻找目标，并通过寻找目标…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [建立一个坚实的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)

+   [学习数据科学统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [每个数据科学家都应该知道的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [一个90亿美元的人工智能失败，分析与检讨](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)
