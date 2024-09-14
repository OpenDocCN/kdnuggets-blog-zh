# 被遗忘的算法

> 原文：[https://www.kdnuggets.com/2020/02/forgotten-algorithm-monte-carlo-simulation.html](https://www.kdnuggets.com/2020/02/forgotten-algorithm-monte-carlo-simulation.html)

[评论](#comments)

**由 [Ian Xiao](https://www.linkedin.com/in/ianxiao/)，Dessa 的参与负责人**

**TL;DR** — 当我们谈论机器学习时，我们通常会想到监督学习和无监督学习。在这篇文章中，我想讨论一个经常被遗忘但同样强大的算法：蒙特卡洛模拟。我将分享一个通用设计框架和一些技术，并提供一个互动工具。最后，你还可以在文章末尾找到一些好的模拟工具列表。

***免责声明：*** 这不是由 Streamlit 赞助的，也不是我提到的任何工具或我工作的任何公司赞助的。我将数据科学和机器学习视为可互换的。*

***喜欢你读到的内容？*** 关注我在 *[*Medium*](https://medium.com/@ianxiao)*、*[*LinkedIn*](https://www.linkedin.com/in/ianxiao/)* 和 *[*Twitter*](https://twitter.com/ian_xxiao)* 上。

### 不应得的那个

在近期的机器学习（ML）崛起中，监督学习和无监督学习算法，如使用 [深度学习](https://machinelearningmastery.com/what-is-deep-learning/) 的分类和使用 KNN 的聚类，得到了大多数关注。当这些算法获得热情社区的高度赞扬时，一些同样强大而优雅的技术却在角落里静静地等待。它的名字是 **蒙特卡洛** — 这个在原子物理学、现代金融和赌博中被遗忘而不应得的英雄（或者说是恶棍，这取决于你对这些问题的看法）。

*注意：为了简洁，我将监督学习和无监督学习方法称为“ML算法”，将蒙特卡洛方法称为“模拟”。*

### 简短历史

[斯坦尼斯瓦夫·乌拉姆](https://en.wikipedia.org/wiki/Stanislaw_Ulam)、[恩里科·费米](https://en.wikipedia.org/wiki/Enrico_Fermi) 和 [约翰·冯·诺伊曼](https://en.wikipedia.org/wiki/John_von_Neumann) — 洛斯阿拉莫斯的天才们 — 在1930年代发明、改进并推广了蒙特卡洛方法，目的并不那么高尚 *(提示：不是为了原子弹)*。观看视频了解更多。

蒙特卡洛模拟简短历史（YouTube）

### 什么是蒙特卡洛模拟？

如果我用一句话总结蒙特卡洛模拟，这里是：**假装做一亿次，直到我们大概知道现实是什么。**

[通过 GIPHY](https://giphy.com/gifs/mrw-someone-aVytG2ds8e0tG)

在技术（和更严肃的）层面上，蒙特卡洛方法的目标是 **近似** **结果的期望**，以考虑各种输入、不确定性和系统动态。这个视频介绍了一些对有兴趣的人来说的高级数学。

蒙特卡洛近似，[YouTube](https://youtu.be/7TybpwBlcMk)

### 为什么使用模拟？

如果我必须强调仿较于机器学习算法的一个（过于简单化的）优势，那就是：**探索。**我们使用仿真来理解*任何规模系统的内部工作*（例如：世界、社区、公司、团队、个人、车队、汽车、车轮、原子等）

通过虚拟仿真重新创建系统，我们可以计算和分析假设结果，而无需实际改变世界或等待真实事件发生。换句话说，仿真允许我们**提出大胆的问题**并**制定战术**来管理各种未来结果，而风险和投资相对较小。

### 什么时候使用仿真，而不是机器学习？

根据[本杰明·舒曼](https://www.benjamin-schumann.com/blog/2018/5/7/time-to-marry-simulation-models-and-machine-learning)，一位著名的仿真专家，仿真是*过程驱动的*，而机器学习是*数据中心的*。要产生良好的仿真效果，我们需要理解系统的过程和基本原理。相比之下，我们可以仅通过使用数据仓库中的数据和一些现成的算法来创建合理的预测。

换句话说，创建良好的仿真在财务和认知上通常**更昂贵**。我们为什么还要使用仿真？

好吧，考虑三个简单的问题：

+   你是否在数据仓库中有数据来表示业务问题？

+   你是否有足够的数据——无论是数量上还是质量上——来构建一个好的机器学习模型？

+   预测是否比探索更重要（例如，提出假设性问题并制定支持业务决策的战术）？

如果你对这些问题的回答是“否”，那么你应该考虑使用仿真而不是机器学习算法。

### 如何设计蒙特卡洛仿真？

要创建一个蒙特卡洛仿真，至少需要遵循一个3步过程：

![图像](../Images/7cfa3ac902ecabf6ad63a7b51e6ec576.png)

仿真过程，作者分析

如你所见，创建一个蒙特卡洛仿真仍然需要数据，更重要的是，需要对系统动态有一定了解（例如销售量和价格之间的关系）。要获得这种知识，通常需要与专家交谈，研究流程，以及观察实际业务操作。

### Yet Another Simulator

要查看基本概念如何实现，你可以访问[**Yet Another Simulator**](https://yet-another-sim.herokuapp.com/)——这是我使用[Streamlit](https://www.streamlit.io/)开发的一个交互式工具。

在**欢迎页面**上，你可以尝试各种输入设置，并观察根据你应用的函数，结果如何变化。

![图像](../Images/d6aa3e2960ecb980663c0431c87d3809.png)

[Yet Another Simulator](https://yet-another-sim.herokuapp.com/)的欢迎页面，作者的工作

除了基本示例外，该工具还包括4个案例研究，讨论了各种设计技术，如影响图、敏感性分析、优化和将机器学习与仿真相结合。

例如，在**CMO示例**中，我讨论了如何使用影响图来帮助设计一个模拟以解决广告预算分配问题。

![图](../Images/170a8bada18519a92027c156df36e9da.png)

影响图，作者的工作

最终，你将踏入数据科学家的角色，建议首席营销官（CMO）。你的目标是帮助CMO决定广告支出金额，探索各种情景，并提出在不同不确定性下最大化回报的策略。

![图](../Images/97f89c1ef8e9b93adfbdfce136f81e8a.png)

广告预算分配，作者的工作

我希望这些示例能说明蒙特卡罗模拟的工作原理，它相较于机器学习算法的优势，以及如何使用不同的设计技巧设计有用的模拟。

一些案例研究仍在积极开发中。请[**在此处**](https://docs.google.com/forms/d/e/1FAIpQLSfL57Eb6Kd7fK3OLfXNUENa3H0rLhmcgxnLQp6SwSWNZ_pLaQ/viewform?usp=sf_link)注册，以便在它们准备好时接到通知。

### 总结

我希望这篇文章提供了对蒙特卡罗方法的另一种视角；在今天的机器学习讨论中，我们常常忽视这样一个有用的工具。模拟具有许多传统机器学习算法无法提供的优势——例如，在巨大不确定性下探索重大问题的能力。

在**即将发布的文章**中，我将讨论**如何在实际商业环境中结合机器学习和模拟**以获得两者的最佳效果，以及如何阐明不同模拟情景的含义。

通过关注我的[**Medium**](https://medium.com/@ianxiao)**、**[**LinkedIn**](https://www.linkedin.com/in/ianxiao/)**或[**Twitter**](https://twitter.com/ian_xxiao)**保持关注。**

下次见，

伊恩

[via GIPHY](https://giphy.com/gifs/two-9-two-9-nine-l4FGsEzqS8Zz8aOWs)

### 如果你喜欢这篇文章，你可能也会喜欢这些：

[**12小时机器学习挑战**](https://towardsdatascience.com/build-full-stack-ml-12-hours-50c310fedd51)

如何使用Streamlit和DevOps工具构建和部署机器学习应用

[**机器学习与敏捷的注定失败的结合**](https://towardsdatascience.com/a-doomed-marriage-of-ml-and-agile-b91b95b37e35)

如何不在机器学习项目中应用敏捷

[**数据科学是无聊的**](https://towardsdatascience.com/data-science-is-boring-1d43473e353e)

我如何应对部署机器学习的无聊日子

[**对抗另一场人工智能寒冬的最后防线**](https://towardsdatascience.com/the-last-defense-against-another-ai-winter-c589b48c561)

数字、五种战术解决方案和快速调查

[**人工智能的最后一公里问题**](https://towardsdatascience.com/fixing-the-last-mile-problems-of-deploying-ai-systems-in-the-real-world-4f1aab0ea10)

很多数据科学家没充分考虑的一件事

[**我们创造了一个懒惰的人工智能**](https://towardsdatascience.com/we-created-a-lazy-ai-5cea59a2a749)

如何为现实世界设计和实现强化学习

### 流行工具

当我讨论仿真时，很多人会询问工具建议。这里有一份我了解的工具列表，选择适合你需求的工具。请享用。

+   [AnyLogic](https://www.anylogic.com/) (这可能是模拟专业人士的首选工具；免费增值)

+   [Simio](https://www.simio.com/index.php) (免费增值)

+   [Yasai](http://yasai.rutgers.edu/) (Excel 插件，免费)

+   [Oracle Crystal Ball](https://www.crystalballservices.com/Store/Oracle-Crystal-Ball) (免费增值)

+   [SimPy](https://simpy.readthedocs.io/en/latest/index.html) (Python 包，免费)

+   [Hash](https://hash.ai/) (创企，当前处于隐秘模式。创始团队相当强大。可能是免费增值)

### 参考资料

**决策树的历史** — [http://pages.stat.wisc.edu/~loh/treeprogs/guide/LohISI14.pdf](http://pages.stat.wisc.edu/~loh/treeprogs/guide/LohISI14.pdf)

**聚类的历史** — [https://link.springer.com/chapter/10.1007/978-3-540-73560-1_15](https://link.springer.com/chapter/10.1007/978-3-540-73560-1_15)

**将仿真与机器学习结合的时机** — [https://www.benjamin-schumann.com/blog/2018/5/7/time-to-marry-simulation-models-and-machine-learning](https://www.benjamin-schumann.com/blog/2018/5/7/time-to-marry-simulation-models-and-machine-learning)

**仿真的分类** — [https://gamingthepast.net/theory-practice/simulation-design-guide/](https://gamingthepast.net/theory-practice/simulation-design-guide/)

**蒙特卡洛方法及其工作原理** — [https://www.palisade.com/risk/monte_carlo_simulation.asp](https://www.palisade.com/risk/monte_carlo_simulation.asp)

**简介：[Ian Xiao](https://www.linkedin.com/in/ianxiao/)** 是 Dessa 的参与负责人，负责在企业中部署机器学习。他领导业务和技术团队部署机器学习解决方案，并为 F100 企业改进市场营销和销售。

[原文](https://towardsdatascience.com/how-to-design-monte-carlo-simulation-138e9214910a). 经许可转载。

**相关：**

+   [数据科学很无聊（第1部分）](/2019/09/data-science-boring-part-1.html)

+   [我们创建了一个懒惰的 AI](/2020/01/created-lazy-ai.html)

+   [12小时机器学习挑战：使用 Streamlit 和 DevOps 工具构建并部署应用](/2020/02/machine-learning-challenge-build-deploy-app-streamlit-devops.html)

* * *

## 我们的前三课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT 工作

* * *

### 更多相关话题

+   [遗传算法关键术语解释](https://www.kdnuggets.com/2018/04/genetic-algorithm-key-terms-explained.html)

+   [决策树算法解析](https://www.kdnuggets.com/2020/01/decision-tree-algorithm-explained.html)

+   [机器学习算法的完整端到端部署](https://www.kdnuggets.com/2021/12/deployment-machine-learning-algorithm-live-production-environment.html)

+   [揭开选择完美机器学习算法的秘密！](https://www.kdnuggets.com/2023/07/ml-algorithm-choose.html)

+   [为数据集选择正确的聚类算法](https://www.kdnuggets.com/2019/10/right-clustering-algorithm.html)

+   [机器学习中的DBSCAN聚类算法](https://www.kdnuggets.com/2020/04/dbscan-clustering-algorithm-machine-learning.html)
