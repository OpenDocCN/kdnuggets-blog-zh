# 我在数据科学硕士课程中学到的 7 件事

> 原文：[https://www.kdnuggets.com/2019/10/top-7-things-learned-data-science-masters.html](https://www.kdnuggets.com/2019/10/top-7-things-learned-data-science-masters.html)

[评论](#comments)

**由 [Dario Radečić](https://www.linkedin.com/in/darioradecic/), 数据科学学生**

其中一些你可能已经熟悉，但我不建议跳过它们——另一个观点总是很有用的。

![图](../Images/77f5ff4df4cdc500f70858e86967d74f.png)

图片由 [Charles DeLoye](https://unsplash.com/@charlesdeloye?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的捷径。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在的组织的 IT

* * *

### 1\. 始终咨询领域专家

所谓“总是”，是指如果你有这样做的余地。

这是学到的第一件事之一。我们被介绍到这个在数据世界中如同摇滚明星般的人物，具体来说是流失建模。听到那句话的时候，可能是我对数据科学的第一个误解被打破的时候。

你可以在这里阅读更多关于他和整个案例的信息：

**[Python 中的属性相关性分析 — IV 和 WoE](https://towardsdatascience.com/attribute-relevance-analysis-in-python-iv-and-woe-b5651443fc04?source=post_page-----bd89fdbab769----------------------)**

最近我写了关于递归特征消除——我最常用的特征选择技术之一……

最初我认为数据科学家是一种稀有的物种，只要有合适的数据，就可以做几乎任何事情。但在大多数情况下，**事实远非如此**。是的，你可以用任何东西分析一切，通过这样做你会发现一些有趣的东西，但这真的是你时间的最佳利用吗？

问问自己，

> 问题是什么？X 如何与 Y 连接？为什么？

了解这些将引导你朝着解决问题的正确方向前进。这时领域专家就派上用场了。

另一个非常重要的事情是**特征工程**。这位教授强调，你可以利用领域专家来进行特征工程过程。如果你稍微思考一下，这就会变得合理。

### 2\. 你将大部分时间花在准备数据上

是的，你没看错。

我决定从事数据科学硕士课程的主要原因之一是**机器学习**——我不太关心数据的内容以及它是如何收集和准备的。由于这种态度，当学期开始时，我感到有些震惊和失望。

当然，如果你在一家有数据工程师和机器学习工程师的大公司工作，这可能不会适用于你，因为你大部分时间都在做机器学习。但如果情况不是这样，你将只有大约15%的时间用于机器学习。

这其实是很好的。**机器学习并不是特别有趣**。在你带着大批F字词跳到评论区之前，请听我说完。

[via GIPHY](https://giphy.com/gifs/breaking-bad-huh-say-what-l41YkFIiBxQdRlMnC)

我认为机器学习并不是特别有趣的原因是，对于大多数项目来说，它归结为尝试几个学习算法，然后对最好的那个进行优化。

如果你在这之前的工作没有做好，即在数据准备过程中，你的模型很可能会很糟糕，而且你也很难对此做出太多改进——只能调整超参数、调整阈值等。

这就是为什么**数据准备和探索性分析才是王道**，而机器学习只是自然出现的结果。

一旦我意识到自己对机器学习的大部分兴奋感已经消失，我发现我更喜欢数据收集和可视化，因为在这些过程中我学到了最多的关于数据的知识。

我特别喜欢网络爬虫，因为相关的数据集很难找到。如果这听起来是你会喜欢的事情，请查看这篇文章：

[**没有数据集？没问题。自己抓取一个。**](https://towardsdatascience.com/no-dataset-no-problem-scrape-one-yourself-57806dea3cac?source=post_page-----bd89fdbab769----------------------)

使用Python和BeautifulSoup的强大功能来抓取对你重要的数据。

### 3. 不要重新发明轮子

库的存在是有原因的。**在行动之前先谷歌一下**。

我会给你展示一个你可能没有犯的非常简单的‘错误’，但它会帮助你理解这一点。

这涉及到计算中位数的两种方法。中位数定义为：

> 排序列表中的中间位置。

所以要计算它，你需要实现以下逻辑：

+   对输入列表进行排序

+   检查列表的长度是偶数还是奇数

+   如果是偶数，中位数就是两个中间数字的平均值

+   如果是奇数，中位数就是中间的数字

幸运的是，存在像[Numpy](https://numpy.org/)这样的库，它们为你完成了所有繁重的工作。只需看一下下面的代码，前17行是自己计算中位数，而最后两行则使用Numpy的强大功能来实现相同的结果：

中位数计算 — [https://gist.github.com/dradecic/7f295913c01172ffebe84052c8158703](https://gist.github.com/dradecic/7f295913c01172ffebe84052c8158703)

正如我所说，这只是一个你可能没有亲身经历过的微不足道的例子。但是想象一下，你可能因为没有意识到已经有相关的库而徒劳地写了多少行代码。

### 4\. 精通 lambdas 和列表推导式

虽然这并不是数据科学特有的，但我会说我一直在使用***列表推导式***来进行特征工程，而使用***lambda*** 函数来进行数据清理和准备。

下面是一个简单的特征工程示例。给定一个字符串列表，你需要创建一个变量，如果给定的字符串包含问号（?），则该变量等于1，否则为0。你可以看到如何使用和不使用列表推导式来实现这一点（***提示***：它们节省了大量时间*）：

列表推导式示例 — [https://gist.github.com/dradecic/9f23eb0c8073ecc8957f8fd533388cef](https://gist.github.com/dradecic/9f23eb0c8073ecc8957f8fd533388cef)

现在来说说***lambdas***，假设你有一个电话号码列表，格式你不喜欢。基本上，你想用‘**-**’替换‘**/**’。这是一个几乎微不足道的过程，前提是你的数据集是*Pandas***DataFrame**格式：

Lambdas — [https://gist.github.com/dradecic/68e81f6610b26fe8da68e25d217c5052](https://gist.github.com/dradecic/68e81f6610b26fe8da68e25d217c5052)

花一点时间考虑一下你如何将这些应用到你的数据集上。很酷，对吧？

### 5\. 了解你的统计学

如果你没有一直生活在石头下，你会知道统计学在数据科学中的重要性。这是你**必须**发展的基本技能。

让我引用 [Edureka](https://www.edureka.co/)：

> 统计学用于处理现实世界中的复杂问题，以便数据科学家和分析师可以寻找数据中的有意义的趋势和变化。简单来说，统计学可以通过对数据进行数学计算来得出有意义的见解。[1]

根据我在硕士学习统计学过程中学到的知识，你需要了解统计学，以便能够提出正确的问题。

如果你的统计技能有些生疏，我强烈建议你查看 YouTube 上的 [StatQuest](https://www.youtube.com/user/joshstarmer) 频道，更准确地说，是这个关于统计学基础的播放列表：

统计学播放列表

### 6\. 学习算法和数据结构

如果你不能提供解决方案，那么提出正确的问题（见第5点）也没有意义——对吧？

我曾经忽视了算法和数据结构，因为我认为只有软件工程师才需要关注这些内容。说实话，我的想法完全错误。

现在我不是说你必须熟练掌握如何实现二分查找算法，但基本的理解会帮助你更清楚地看到如何在代码中思考——也就是说，如何编写既能完成任务又能尽可能快地完成任务的代码。

对于没有计算机科学背景的人，我强烈推荐这门课程：

[**学习Python以进行数据结构、算法与面试**](https://www.udemy.com/course/python-for-data-structures-algorithms-and-interviews/?source=post_page-----bd89fdbab769----------------------)

请注意：如果你是完全的Python初学者，请查看我的另一门课程：**完整Python培训营**以学习……

另外，一定要查看面试问题——它们帮助很大！

### 7\. 超越范围

永远要成为那个工作最努力的人。**这会得到回报**。

至少在我的案例中，我的团队是根据在一个课程中的初始表现进行评估的。评估标准不是谁知道得最多，因为在第一学期这样做是愚蠢的，而是看谁展现出工作伦理和纪律。

由于那时我没有全职工作，我为这个项目拼尽了全力。因为我这样做了，而其他人没有，所以我被指派到一个为期两年的全面数据科学项目，该项目将作为我的硕士论文。

对了，**我能够把这个放到我的简历上**。

那么，牺牲了几周的个人生活是否值得？自己判断吧，但我会说是值得的。

**参考资料**

1.  [https://www.edureka.co/blog/math-and-statistics-for-data-science/](https://www.edureka.co/blog/math-and-statistics-for-data-science/)

**简介： [Dario Radečić](https://www.linkedin.com/in/darioradecic/)** 是一位22岁的数据科学学生，同时也在该领域工作了一段时间。Medium和Towards Data Science的作者。

[原文](https://towardsdatascience.com/top-7-things-i-learned-on-my-data-science-masters-bd89fdbab769)。经许可转载。

**相关：**

+   [从数据分析师成长为数据科学家的秘密武器](/2019/08/secret-sauce-growing-from-data-analyst-data-scientist.html)

+   [2019年5个著名的深度学习课程/学校](/2019/09/famous-deep-learning-courses-schools-2019.html)

+   [高级特征工程和预处理的4个技巧](/2019/08/4-tips-advanced-feature-engineering-preprocessing.html)

### 更多此主题的内容

+   [我从使用ChatGPT进行数据科学中学到的东西](https://www.kdnuggets.com/what-i-learned-from-using-chatgpt-for-data-science)

+   [你不知道的SAS数据科学学院的3件事](https://www.kdnuggets.com/2022/07/sas-3-things-didnt-know-sas-academy-data-science.html)

+   [选择下一个数据科学工作前要记住的5件事](https://www.kdnuggets.com/2022/01/5-things-keep-mind-selecting-next-job.html)

+   [我开始学习数据科学时希望知道的3件事](https://www.kdnuggets.com/2023/01/3-things-wish-knew-started-data-science.html)

+   [学生在数据科学简历中遗漏的7件事](https://www.kdnuggets.com/7-things-students-are-missing-in-a-data-science-resume)

+   [让数据科学家与其他职业区别开的5件事](https://www.kdnuggets.com/2021/11/5-things-set-data-scientist-apart-other-professions.html)
