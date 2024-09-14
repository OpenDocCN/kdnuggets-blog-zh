# 4 个你不应该使用机器学习的理由

> 原文：[`www.kdnuggets.com/2021/12/4-reasons-shouldnt-machine-learning.html`](https://www.kdnuggets.com/2021/12/4-reasons-shouldnt-machine-learning.html)

**由[Terence Shin](https://www.linkedin.com/in/terenceshin/)，数据科学家 | MSc Analytics & MBA 学生**

![4 个你不应该使用机器学习的理由](img/977db7fcadc23ddf6ae042fd5b5843b2.png)

照片由[Nadine Shaabana](https://unsplash.com/@nadineshaabana?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，来源于[Unsplash](https://unsplash.com/s/photos/stop?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

## 介绍

当机器学习最初出现时，许多人猜测它会引发另一次工业革命。快进到今天，许多人会说它不过是一个流行词。

别误解我的意思。机器学习是一个有用的工具，但仅仅是一个工具。说它像瑞士军刀一样是不切实际的——我更愿意把它看作是一个水刀（某种小众工具）。

根据我的经验，机器学习确实在一些应用场景中表现出色。例如，[亚马逊的推荐系统将销售额提高了 30%以上](https://evdelo.com/amazons-recommendation-algorithm-drives-35-of-its-sales/)。然而，还有更多应用场景中，机器学习是一个次优的解决方案。

在本文中，我们将讨论 4 个你不应该使用机器学习的理由。

话虽如此，让我们深入了解吧！

## 1\. 与数据相关的问题

如[AI 需求层次](https://hackernoon.com/the-ai-hierarchy-of-needs-18f111fcc007)所示，机器学习依赖于几个作为基础的其他因素。这个基础包括从数据收集、数据存储、数据传输到数据转换的所有内容。拥有一个健全的流程来实现这些初步步骤是很重要的，否则你可能不会有可靠的数据。

为什么这如此重要？你一定听过“垃圾进，垃圾出”的说法——你的机器学习模型的性能受限于数据的质量，这就是为什么有可靠的数据是如此重要的原因。

你不仅需要你的数据是**可靠的**，还需要**足够的**数据来利用机器学习的力量。没有这两个条件，你将无法发挥机器学习的全部潜力。

## 2\. 可解释性

模型通常分为两大类：预测模型和解释模型：

+   **预测模型**专注于模型产生准确预测的能力。

+   **解释模型**更多关注于理解数据中变量之间的关系。

机器学习模型，特别是集成学习模型和神经网络，是**预测模型**——它们在制定预测方面表现优异，远超传统模型如线性/逻辑回归的预测能力。

也就是说，当涉及到理解预测变量和目标变量之间的关系时，这些模型是一个黑箱。虽然你可能理解这些模型的基本机制，但它们如何得出最终结果仍然不太清楚。

尽管存在像特征重要性和相关矩阵这样的技术，但它们在理解数据关系方面仍然相当有限。总体来说，机器学习和深度学习在预测方面表现出色，但在可解释性方面有所欠缺。

## 3\. 技术债务

维护机器学习模型随着时间的推移是具有挑战性和高成本的。特别是，维护机器学习模型时需要考虑几种“债务”：

+   **依赖债务：** 依赖债务指的是由于数据依赖的不稳定性和未充分利用的数据依赖所产生的债务。简单来说，这指的是维护同一模型的多个版本、遗留特性和未充分利用的包的成本。

+   **分析债务**：这指的是机器学习系统通常会在更新时影响自身行为，导致直接和隐性的反馈循环。

+   **配置债务：** 机器学习系统自身的配置也会产生类似于任何软件系统的债务。应该容易进行小的配置，难以出现手动错误，并且应该容易区分不同的模型。

*完整的论文请参见*[*这里*](https://proceedings.neurips.cc/paper/2015/file/86df7dcfd896fcaf2674f757a2463eba-Paper.pdf)*，如果你想了解机器学习系统中的隐性技术债务。*

## 4\. 更好的替代方案

最后，当存在同样有效的简单替代方案时，不应使用机器学习。在我之前的文章中，*“*[*想成为数据科学家？不要从机器学习开始*](https://towardsdatascience.com/want-to-be-a-data-scientist-dont-learn-machine-learning-28e418d9af2f)*，”* 我强调了机器学习不是解决所有问题的答案。

一个简单的解决方案，花费 1 周时间构建，准确率 90%，**几乎总是**会被选择，胜过一个需要 3 个月构建、准确率 95%的机器学习模型。

理想情况下，你应该从最简单的解决方案开始实施，并逐步判断下一最佳替代方案的边际收益是否超过边际成本。

如果你能用 Python 脚本或 SQL 查询解决你的问题，你应该先使用这些方法。如果你能用决策树解决你的问题，你应该先使用决策树。如果你能用线性回归模型解决你的问题，你应该先使用线性回归模型。

你明白了。越简单=越好。

## 感谢阅读！

我希望这能提供一些关于机器学习局限性的见解，并说明它并不是一刀切的解决方案。请记住，这篇文章更多是基于个人经验的观点文章，所以请根据需要取舍。但无论如何，我祝愿你在学习过程中一切顺利！

## Terence Shin

+   ***如果你喜欢这个内容， ***[***订阅我的 Medium***](https://terenceshin.medium.com/subscribe)*** 以获取独家内容！***

+   ***同样，你也可以 ***[***关注我在 Medium 上的动态***](https://medium.com/@terenceshin)

+   ***有兴趣合作吗？让我们在 ***[***LinkedIn 上连接***](https://www.linkedin.com/in/terenceshin/)

**个人简介： [Terence Shin](https://www.linkedin.com/in/terenceshin/)** 是一位数据爱好者，拥有 3 年以上 SQL 经验和 2 年以上 Python 经验，同时也是 Towards Data Science 和 KDnuggets 的博客作者。

[原文](https://towardsdatascience.com/4-reasons-why-you-shouldnt-use-machine-learning-639d1d99fe11)。经许可转载。

### 更多相关话题

+   [你不应该成为数据科学家的 7 个理由](https://www.kdnuggets.com/7-reasons-why-you-shouldnt-become-a-data-scientist)

+   [你应该使用线性回归模型而不是…的 3 个理由](https://www.kdnuggets.com/2021/08/3-reasons-linear-regression-instead-neural-networks.html)

+   [数据科学家应该使用 LightGBM 的 3 个理由](https://www.kdnuggets.com/2022/01/data-scientists-reasons-lightgbm.html)

+   [你需要合成数据的 5 个理由](https://www.kdnuggets.com/2023/02/5-reasons-need-synthetic-data.html)

+   [你应该避免从事数据科学职业的前 5 个理由](https://www.kdnuggets.com/2022/04/top-5-reasons-avoid-data-science-career.html)

+   [你应该获得认证的 5 个理由](https://www.kdnuggets.com/2023/05/sas-5-reasons-get-certified.html)
