# A/B 测试：数据科学面试中的 7 个常见问题及答案（第 2 部分）

> 原文：[https://www.kdnuggets.com/2021/04/ab-testing-7-common-questions-answers-data-science-interviews-2.html](https://www.kdnuggets.com/2021/04/ab-testing-7-common-questions-answers-data-science-interviews-2.html)

[评论](#comments)

**注意**：这是本文的第二部分。你可以在 [这里](/2021/04/ab-testing-7-common-questions-answers-data-science-interviews-1.html) 阅读第一部分。

### 分析测试结果

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你在 IT 领域的组织

* * *

![](../Images/ba0fb626475d727e8ad4e9abf3d7b835.png)

图片由 [Scott Graham](https://unsplash.com/@sctgrhm?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

### 新奇效应和首因效应

当产品发生变化时，人们的反应会有所不同。有些人习惯了产品的工作方式，并且不愿意改变。这被称为**首因效应**或对变化的抗拒。其他人可能会欢迎变化，并且新功能会吸引他们更多地使用产品。这被称为**新奇效应**。然而，这两种效应不会持续太久，因为人们的行为会在一段时间后趋于稳定。如果 A/B 测试的初始效应较大或较小，可能是由于新奇效应或首因效应。这是实践中的一个常见问题，许多面试问题都涉及这个话题。一个典型的面试问题是：

> 我们对一个新功能进行了 A/B 测试，测试结果获胜，因此我们将更改推送给所有用户。然而，在推出该功能一周后，我们发现处理效应迅速下降。这是怎么回事？

答案是新奇效应。随着时间的推移，随着新奇感的减退，重复使用将减少，因此我们观察到处理效应的下降。

现在你了解了新奇效应和首因效应，**我们如何解决潜在问题**？这是面试中的一个典型后续问题。

处理这种效应的一种方法是完全排除这些效应的可能性。我们可以仅对首次使用者进行测试，因为新奇效应和首因效应显然不会影响这些用户。如果我们已经在运行测试并且想要分析是否存在新奇效应或首因效应，我们可以 1) 比较控制组中新用户的结果与处理组中相同的新用户结果，以评估新奇效应 2) 比较处理组中新用户的结果与现有用户的结果，以获得新奇效应或首因效应的实际影响估计。

### 多重检验问题

在最简单的 A/B 测试中，有两个变体：控制组（A）和处理组（B）。有时，我们会进行多个变体的测试，以查看哪一个在所有功能中表现最佳。这种情况可能发生在我们想测试按钮的多种颜色或不同的主页时。这样我们将有多个处理组。在这种情况下，我们不应该仅使用 0.05 的显著性水平来决定测试是否显著，因为我们处理的是超过 2 个变体，并且虚假发现的概率增加。例如，如果我们有 3 个处理组与控制组进行比较，观察到至少 1 个虚假正例的概率是多少（假设我们的显著性水平是 0.05）？

我们可以得到没有虚假正例的概率（假设各组是独立的），

Pr(FP = 0) = 0.95 * 0.95 * 0.95 = 0.857

然后得到至少有 1 个虚假正例的概率

Pr(FP >= 1) = 1 — Pr(FP = 0) = 0.143

在仅有 3 个处理组（4 个变体）的情况下，虚假正例（或 I 型错误）的概率超过 14%。这被称为“**多重检验**”问题。一个常见的面试问题是

> 我们正在进行一个有 10 个变体的测试，尝试不同版本的登录页面。一个处理组获胜，且 p 值小于 0.05。你会做出改变吗？

答案是否定的，因为存在多重检验问题。有几种方法可以应对。常用的方法之一是**邦费罗尼校正**。它将显著性水平 0.05 除以测试数量。对于这个面试问题，因为我们要测量 10 个测试，所以测试的显著性水平应为 0.05 除以 10，即 0.005。基本上，我们只有当测试的 p 值小于 0.005 时，才会声称该测试具有显著性。邦费罗尼校正的缺点是它往往过于保守。

另一种方法是控制**虚假发现率**（FDR）：

FDR = E[# 的虚假正例 / # 的拒绝]

这衡量了所有原假设的拒绝，即所有声明具有统计显著差异的指标。实际存在差异的有多少，与假阳性的有多少。这仅在你有**大量的指标**时才有意义，比如几百个。假设我们有200个指标，并将FDR上限设为0.05。这意味着我们接受5%的假阳性。我们将在这200个指标中每次观察到至少10个假阳性。

### 做决策

![](../Images/383fec710c8c2560b462536873c210b6.png)

照片由 [You X Ventures](https://unsplash.com/@youxventures?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

理想情况下，我们会看到具有实际意义的处理结果，我们可以考虑将该功能推出给所有用户。但有时，我们会看到**矛盾的结果**，例如一个指标上升而另一个下降，因此我们需要做出权衡。一道面试题是：

> 在运行测试后，你看到所需的指标，例如点击率上升而展示次数下降。你会如何做决策？

事实上，做出产品发布决策可能涉及多个因素，如实施复杂性、项目管理工作、客户支持成本、维护成本、机会成本等。

在面试中，我们可以提供一个简化版本的解决方案，重点关注实验的**当前目标**。是最大化参与度、留存、收入，还是其他？此外，我们还要量化负面影响，即非目标指标的负面变化，以帮助我们做出决定。例如，如果收入是目标，我们可以选择收入优先于最大化参与度，前提是负面影响可以接受。

### 资源

最后，我想推荐两个资源，帮助你更好地了解A/B测试。

+   [Udacity的免费A/B测试课程](https://www.udacity.com/course/ab-testing--ud257) 涵盖了A/B测试的所有基础知识。

+   [值得信赖的在线对照实验——A/B测试实用指南](https://www.amazon.com/Trustworthy-Online-Controlled-Experiments-Practical/dp/1108724264) 由Ron Kohavi、Diane Tang和Ya Xu编写。它深入探讨了如何在行业中进行A/B测试、潜在的陷阱以及解决方案。书中包含了许多有用的内容，所以我实际上计划写一篇文章总结这本书的内容。如果你感兴趣，请继续关注！

**简介: [Emma Ding](https://www.youtube.com/c/DataInterviewPro)** 是Airbnb的数据科学家和软件工程师。

[原文](https://medium.com/@emmading/48df1844ae4c) 已获得许可重新发布。

**相关：**

+   [A/B测试：数据科学面试中的7个常见问题与答案，第1部分](/2021/04/ab-testing-7-common-questions-answers-data-science-interviews-1.html)

+   [关于 A/B 测试的 5 件事](/2018/09/5-things-know-about-ab-testing.html)

+   [如何获得数据科学面试机会：寻找工作、联系关卡人员以及获取推荐](/2021/02/data-science-interviews-finding-jobs-reaching-gatekeepers-getting-referrals.html)

### 了解更多相关话题

+   [数据科学面试中的 24 道 A/B 测试面试题及解答…](https://www.kdnuggets.com/2022/09/24-ab-testing-interview-questions-data-science-interviews-crack.html)

+   [识别伪数据科学家：20 个问题（附答案）：ChatGPT…](https://www.kdnuggets.com/2023/01/20-questions-detect-fake-data-scientists-chatgpt-1.html)

+   [识别伪数据科学家：20 个问题（附答案）：ChatGPT…](https://www.kdnuggets.com/2023/02/20-questions-detect-fake-data-scientists-chatgpt-2.html)

+   [7 道数据分析面试题及答案](https://www.kdnuggets.com/2022/09/7-data-analytics-interview-questions-answers.html)

+   [5 道 Python 面试题及答案](https://www.kdnuggets.com/2022/09/5-python-interview-questions-answers.html)

+   [假设检验与 A/B 测试](https://www.kdnuggets.com/hypothesis-testing-and-ab-testing)
