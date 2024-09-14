# NIPS 2017 重点与总结笔记

> 原文：[https://www.kdnuggets.com/2017/12/nips-2017-key-points-summary-notes.html](https://www.kdnuggets.com/2017/12/nips-2017-key-points-summary-notes.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

![NIPS 2017](../Images/1f1b14b56de270908653a9751281aac8.png)[NIPS 2017](https://nips.cc/)上周在长滩举行，根据所有人的评价，它确实没有辜负期待。虽然我未能亲自到场（我希望能去），但布朗大学的三年级博士生[David Abel](https://cs.brown.edu/~dabel/)，确实出席了，并且他辛勤地编写和整理了一份[精彩的43页笔记](https://cs.brown.edu/~dabel/blog/posts/misc/nips_2017.pdf)，这些笔记只能用令人自愧不如来形容。他已经将这些笔记以PDF形式提供给所有人，并鼓励进行传播。

虽然David显然无法参加NIPS上的每一个讲座和教程，但他显然安排得非常紧凑，我们可以通过他的经历间接感受那种体验，即使是事后。

* * *

## 我们的前三课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的IT需求

* * *

代表所有未能在Twitter显示名称上临时添加"**@ #NIPS2017**"的我们，David，谢谢你的努力。如果你有兴趣阅读这些笔记的讨论，可以[在这里找到](https://news.ycombinator.com/item?id=15909710)。

另值得注意的是，David在NIPS的[层次化RL研讨会](https://sites.google.com/view/hrlnips2017/home?authuser=0)上展示了一篇题为"[朝向良好的终身学习抽象](https://cs.brown.edu/~dabel/papers/nips_hrl_good_abstr.pdf)"的论文（David Abel, Dilip Arumugam, Lucas Lehnert, Michael L. Littman）。

以下是David笔记中的几个亮点（重点突出），以及一些相关讲座的视频。

> **阿里·拉希米的时间测试讲座。** 这引发了会议期间的大量讨论。在下半场，他提出了一些关于当前机器学习研究状态的看法，呼吁我们的方法更加严格。这在会议期间被广泛讨论，（大多数）人支持阿里的观点（至少是我交谈过的那些人），也有一些人表示他的观点没有根据，因为他似乎针对的一些方法（主要是深度学习）在实践中效果很好。我的个人看法是，他并不一定在呼吁理论来支持我们的方法，而是对严格性的呼吁。我认为对严格性的呼吁是深刻的。我认为说有效的实验对ML社区是有益的毫无争议。究竟这意味着什么，当然是有待辩论的（见下一个要点）。
> 
> **乔厄尔·皮诺关于深度强化学习中的可重复性讲座。** 一个实验表明，两种方法，假设我们称之为A和B，在相同任务上取决于所选择的随机种子，相互主导。也就是说，A在一个随机种子下实现了统计上显著优于B的表现，而这种主导性在不同的随机种子下则会被翻转。我非常喜欢这项工作，并且认为这正好是时候。特别是在深度强化学习中，大多数结果的形式是：“我们的算法在X和Y任务上表现更好”。
> 
> **乔什·特嫩鲍姆正在讲解如何从人类行为中逆向工程智能。**
> 
> Warneken和Tomasello的论文[55]讨论了婴儿的 emergent helping behavior。他展示了一个视频，视频中一个婴儿主动去帮助一个成人，没有人告诉他这样做。太可爱了！
> 
> 目标：逆向工程常识。
> 
> 工具：概率程序。能够生成世界下一状态的模型，充当近似的“脑中的游戏引擎”，真的很强大。可能是缺失的部分。直观物理学和直观心理学的混合。
> 
> 使用概率程序进行常识工程：
> 
> +   什么？建模程序（脑中的游戏引擎）。
> +   
> +   如何？用于推理和模型构建的元程序，在多个时间尺度上工作，权衡速度、可靠性和灵活性。
> +   
> **凯特·克劳福德：偏见的问题**
> 
> 凯特真了不起！今天她讲述了AI/ML中的偏见问题。
> 
> 收获：偏见是一个高度复杂的问题，渗透到机器学习的每个方面。我们必须问：我们的工作将会使谁受益，谁可能受到伤害？要把
> 
> 首先考虑公平性，我们必须问这个问题。

**[下载David的完整PDF笔记](https://cs.brown.edu/~dabel/blog/posts/misc/nips_2017.pdf)在这里。**

**相关内容：**

+   [免费观看2017年开放数据科学最佳演讲](/2017/12/watch-best-odsc-data-science-talks-2017.html)

+   [2017年旧金山AI会议的主要内容 – 第1天](/2017/09/key-takeaways-ai-conference-san-francisco-2017-day-1.html)

+   [2017 年开放数据科学会议（ODSC）西部大会的主要收获](/2017/11/key-takeaways-open-data-science-conference-odsc-west-2017.html)

### 更多相关主题

+   [最佳 Python 课程：分析总结](https://www.kdnuggets.com/2022/01/best-python-courses-analysis-summary.html)

+   [SQL 专业笔记：免费电子书评审](https://www.kdnuggets.com/2022/05/sql-notes-professionals-free-ebook-review.html)

+   [KDnuggets 新闻，5 月 11 日：SQL 专业笔记；如何…](https://www.kdnuggets.com/2022/n19.html)

+   [人工智能、分析、机器学习、数据科学、深度学习……](https://www.kdnuggets.com/2021/12/developments-predictions-ai-machine-learning-data-science-research.html)

+   [2021 年数据科学与分析行业主要发展及关键…](https://www.kdnuggets.com/2021/12/developments-predictions-data-science-analytics-industry.html)

+   [成为优秀数据科学家所需的 5 项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)
