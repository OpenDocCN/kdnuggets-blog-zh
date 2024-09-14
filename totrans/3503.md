# 机器学习的5个部落 – 问题与答案

> 原文：[https://www.kdnuggets.com/2015/11/domingos-5-tribes-machine-learning-questions-answers.html](https://www.kdnuggets.com/2015/11/domingos-5-tribes-machine-learning-questions-answers.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

11月24日，我主持了[![Pedro Domingos 200](../Images/c41e30824c383ee55110db8ec743c09c.png) **ACM网络研讨会与Pedro Domingos**](/2015/11/domingos-5-tribes-machine-learning-acm-webinar.html)。

[Pedro](http://homes.cs.washington.edu/~pedrod/)，机器学习和数据科学的领先研究者之一，详细讲解了他的优秀书籍，[**《主算法》**](/2015/09/book-master-algorithm-pedro-domingos.html)，其中解释了机器学习的5个部落的方法：符号主义者、连接主义者、进化主义者、贝叶斯主义者和类比主义者，以及如何结合这些方法以寻找能够彻底改变我们世界的主算法。

![5部落机器学习](../Images/3a989ea3d4a02fc25ea1ca60e828ebb1.png)

超过1500人参加了此次网络研讨会。

介绍Pedro后，我准备放松并享受演讲，但我的主持人角色却意外地变得非常紧张，因为仅仅几分钟后，许多问题开始涌入，我必须迅速阅读和优先处理这些问题。

Pedro只有时间回答了100多个提交中的5个问题，但许多问题都非常好，因此我要求他回答一些其他有趣的问题。

另见Pedro对我早期问题的回答：

[![主算法](../Images/26df3a3796057f3761d707ebaa659b2e.png)**深度学习是主算法吗？**](/2015/11/domingos-5-tribes-machine-learning-acm-webinar.html)

（帖子底部）

这里是Pedro Domingos对网络研讨会中他没有时间现场回答的额外问题的回答。

Juan Alvarado提问：为了获得主算法，您是否考虑过由于哥德尔不完备定理而使其不可能？

**Pedro Domingos:** 主算法仅学习可学习的内容；它无法证明哥德尔定理无法证明的命题。

Pramod Anantharam：你认为可解释的模型是否会在未来战胜基于黑箱的方法？

**PD:** 这取决于应用程序。可解释性是一个非常重要的属性，对于许多应用程序，它优先于从（比如）深度学习中获得的额外准确性。

Mohamed Helmy：我们能简要了解一下规则引擎和符号逻辑（决策树）吗？

**PD:** 当然，可以查看《主算法》第三章或我的[MOOC（Coursera上的机器学习）](https://www.coursera.org/course/machlearning)的第二和第三周。

Jesus Morales：对于想要追随这条路径的大学生，他们在开始之前需要了解哪些建议？

**PD:** 上MOOC课程，阅读教科书，尝试一些算法。请参见《大师算法》中的进一步阅读部分。

Cristal Jones-Harris, 你有360度推荐系统的文本样本吗？

**PD:** 参见我在《华尔街日报》上发表的文章《["为你的数字模型做好准备"](http://www.wsj.com/articles/get-ready-for-your-digital-model-1447351480)》。

Subhra Mazumdar: 360度推荐系统会影响人的决策能力吗？它会成为人和机器的共生关系，还是更倾向于主奴关系？

**PD:** 360度推荐系统是你大脑的扩展，因此你掌控一切。它所做的只是尽力做出你在有时间时会做出的选择，并且当它出错时，它应该学会在下次做得更好。

Abdelaziz Mahoui, 在新的360度推荐系统中，偶然性将如何表现？

**PD:** 推荐系统应该具有随机组件，就像现实生活一样。

Delane Pickel: “主算法”这个问题意味着只有一个。终极学习机器必须具备某种成功学习的理解能力。一些学习实际上可能变得具有破坏性。这不正是癌症吗？

**PD:** 大师算法有多于一种版本，就像有许多等同于图灵机的计算方案一样。它所学到的内容取决于你给它的数据和评估函数。

Aude Dufresne: 如果评估和优化改变现实怎么办？

**PD:** 不确定你指的是什么，但学习结果的部署往往会导致被建模的人改变他们的行为。这是一个重要且尚未充分探索的问题，但可以参考我在KDD-04会议上发表的论文《["对抗分类"](http://homes.cs.washington.edu/~pedrod/papers/kdd04.pdf)》。

Michael Valenzuela: Wolpert 的《无免费午餐定理》证明了不存在“通用”机器学习算法。你如何调和这个证明和你的目标？

**PD:** 大师算法只需在我们的世界中有效，而非所有可能的世界，“无免费午餐”定理涉及的是后者。此外，大师算法不仅以数据为输入；它还输入知识（例如，逻辑公式）。

Jim Talley: 首先，那是一个非常好的总结。不过，在我看来，仍然缺少的部分是感知，或者更具体地说，是学习如何构建和准备这些五种范式的输入。对此有什么想法？你认为这已经是这些范式之一或多种的组成部分吗？

**PD:** 每种范式都有其解决这个问题的方法，确实需要解决。例如，深度学习的主要主张是它从原始数据（例如，像素）中学习自身的表示。

Joanna Biega: 那么关于存储大量数据的传统数据结构呢？我们是否忘记了不仅需要良好的机器学习算法或渐进线性算法，还需要有效的大数据数据结构？

**PD:** 确实，一个通用的学习者必须高效，数据结构是其中的重要部分。

Bhaskar Veeraraghavan, 主动强化学习有什么应用吗？

**PD:** 强化学习在某种意义上是主动学习。我听说Deep Mind正在将其应用于网页搜索，这将是一个主要应用，如果部署在Google的引擎中。

Habibollah Daneshpajouh, 正如你提到的，每个领域的这些“部落”在某些问题上表现更好，我们如何检测出在特定问题上最适合的ML“部落”？

**PD:** 尽管进行了大量研究，但没有人能给出一个好的理论答案，不过有很多实用的启发式方法，你也可以尝试所有的方法。

Latha Krishnaswamy, ML算法通常是无监督的。在你看来，这种情况允许或受限于缺乏人工监督的影响是什么？

**PD:** 实际上，监督学习的使用频率高于无监督学习。如果有监督（或获取成本不高），肯定要使用监督。

Parlinggoman Hasibuan, 你认为机器学习能做决策并取代人类吗？

**PD:** 是的，在许多领域已经如此（例如，信用评分、直销）。

Raul Sierra: 你谈到主算法，但我们是否也需要类似主任务的东西来评估主算法？

**PD:** 当然了。我们需要一组足够多样且困难的任务，如果学习算法能解决这些任务，我们就可以合理地称之为主算法。

Theodore Grammatikopoulos: 从一个主算法构建一个可能为不同的数据片段（列、行）选择不同算法并赋予适当权重的角度来看，这会是个好主意吗？

**PD:** 这实际上是一种模型集成，正如我讨论的那样，它比单一模型更好，但还不是主算法。

Wee Kiat: 你认为主算法是否能解决当前无法解决的问题？如果可以，机器学习会比我们的大脑更优越吗？

**PD:** 是的，确实如此，比如治愈癌症。但问题不在于机器学习是否会优于我们的脑，而在于我们的脑结合机器学习是否会优于没有机器学习的脑。

Victor: 你认为下一个接近突破的机器学习进展是什么？

**PD:** 我不知道，但我正在研究一些候选者 :)

### 更多相关话题

+   [7个数据分析面试问题与答案](https://www.kdnuggets.com/2022/09/7-data-analytics-interview-questions-answers.html)

+   [5 Python面试问题与答案](https://www.kdnuggets.com/2022/09/5-python-interview-questions-answers.html)

+   [20个问题（含答案）识别伪数据科学家：ChatGPT…](https://www.kdnuggets.com/2023/01/20-questions-detect-fake-data-scientists-chatgpt-1.html)

+   [识别伪数据科学家的20个问题（附答案）：ChatGPT…](https://www.kdnuggets.com/2023/02/20-questions-detect-fake-data-scientists-chatgpt-2.html)

+   [使用HuggingFace Pipelines和Streamlit回答问题](https://www.kdnuggets.com/2021/10/simple-question-answering-web-app-hugging-face-pipelines.html)

+   [数据科学面试中的24个A/B测试问题](https://www.kdnuggets.com/2022/09/24-ab-testing-interview-questions-data-science-interviews-crack.html)
