# 机器学习和数据科学中最受欢迎的语言是…

> 原文：[https://www.kdnuggets.com/2017/01/most-popular-language-machine-learning-data-science.html](https://www.kdnuggets.com/2017/01/most-popular-language-machine-learning-data-science.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png)[评论](#comments)

**由[Jean-Francois Puget](https://www.ibm.com/developerworks/mydeveloperworks/blogs/jfp/?lang=en)，IBM**。

应该学习什么编程语言才能获得机器学习或数据科学职位？这是一个至关重要的问题。它在许多论坛中被讨论。我可以在这里提供我自己的答案并解释原因，但我更愿意先看一些数据。毕竟，这就是机器学习者和数据科学家应该做的：看数据，而不是意见。

* * *

## 我们的前三课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

那么，让我们来看看一些数据。我将使用 indeed.com 上的趋势搜索功能。它查看在职位招聘中所选术语的时间发生频率。这能提供雇主正在寻找哪些技能的指示。然而，请注意，这不是关于哪些技能实际在使用的调查。这更像是技能受欢迎程度如何演变的高级指示器（更正式地说，它可能接近于受欢迎程度的一阶导数，因为后者是招聘技能加上再培训技能减去退休和离职技能的差值）。

说够了，让我们来看数据。我搜索了与“机器学习”和“数据科学”相关联的技能，其中技能包括主要编程语言 Java、C、C++ 和 Javascript。我还包括了 Python 和 R，我们知道这两者在机器学习和数据科学中非常受欢迎，以及 Scala 因为它与 Spark 的关联，还有 Julia，一些人认为它是下一个重要的语言。运行[这个查询](https://www.indeed.com/jobtrends/q-python-and-%28%22machine-learning%22-or-%22data-science%22%29-q-R-and-%28%22machine-learning%22-or-%22data-science%22%29-q-Java-and-%28%22machine-learning%22-or-%22data-science%22%29-q-Javascript-and-%28%22machine-learning%22-or-%22data-science%22%29-q-C-and-%28%22machine-learning%22-or-%22data-science%22%29-q-C++-and-%28%22machine-learning%22-or-%22data-science%22%29-q-Julia-and-%28%22machine-learning%22-or-%22data-science%22%29-q-scala-and-%28%22machine-learning%22-or-%22data-science%22%29.html)我们得到我们所需的数据：

![image](../Images/8d02117d13f6193fc1c88dbb5c436d34.png)

当我们专注于机器学习时，数据类似：

![image](../Images/fa40a2642a44c833a6b402cc760a8b56.png)

我们可以从这些数据中得出什么？

首先，我们看到一种方式并不适合所有情况。在这个背景下，一些语言相当受欢迎。

第二，所有这些语言的受欢迎程度都急剧上升，反映了近年来对机器学习和数据科学的兴趣增加。

第三，Python 是明确的领导者，其次是 Java，然后是 R，再是 C++。Python 对 Java 的领先优势在增加，而 Java 对 R 的领先优势在减少。我必须承认看到 Java 排在第二位让我感到惊讶，我本以为是 R。

第四，Scala 的增长令人印象深刻。三年前几乎不存在，而现在与更成熟的语言相当。这在我们切换到 indeed.com 上的数据的*相对*视角时更容易发现：

![image](../Images/3b4a741d15b4225bdaa8375576def66e.png)

第五，Julia 的受欢迎程度远不及其他语言，但最近几个月确实有所上升。Julia 会成为机器学习和数据科学中的流行语言之一吗？未来会告诉我们答案。

如果我们忽略 Scala 和 Julia 以便放大其他语言的增长，那么我们确认 Python 和 R 的增长速度快于通用语言。

![image](../Images/7246397fe993e7eec10ac4889140a8b6.png)

鉴于增长率的差异，R 的受欢迎程度可能很快会超过 Java。

当我们专注于深度学习 [通过这个查询](https://www.indeed.com/jobtrends/q-Python-and-%22deep-learning%22-q-R-and-%22deep-learning%22-q-Java-and-%22deep-learning%22-q-Javascript-and-%22deep-learning%22-q-C-and-%22deep-learning%22-q-C++-and-%22deep-learning%22-q-Julia-and-%22deep-learning%22-q-Scala-and-%22deep-learning%22-q-Lua-and-%22deep-learning%22.html)时，数据是相当不同的：

![image](../Images/42cb4251cedf2ec9b92bab667fb3a18a.png)

在那里，Python 仍然是领先者，但 C++ 现在排第二，其次是 Java，C 排在第四位。R 仅排在第五。这里明显强调了高性能计算语言。虽然 Java 增长很快，可能很快会达到第二位，就像一般的机器学习一样。R 不会在短期内接近前列。让我感到惊讶的是 Lua 的缺席，尽管它被用于一个主要的深度学习框架（Torch）。Julia 也不在其中。

对于原始问题的答案现在应该很清楚了。Python、Java 和 R 是与机器学习和数据科学工作相关的最受欢迎的技能。如果你想专注于深度学习而不是一般的机器学习，那么 C++，以及程度稍低的 C，也值得考虑。然而请记住，这只是看待问题的一个方式。如果你在寻找学术界的职位，或者只是想在闲暇时间学习机器学习和数据科学，你可能会得到不同的答案。

那我个人的回答呢？我在[这个博客](https://www.ibm.com/developerworks/community/blogs/jfp/entry/Why_Python?lang=en)中早些时候回答了这个问题。除了得到许多顶级机器学习框架的支持，Python 适合我，因为我有计算机科学背景。我也会对用 C++ 开发新算法感到舒适，因为我在这个语言中编程的时间占据了我职业生涯的大部分。但这只是我，背景不同的人可能会对其他语言感到更合适。一个编程技能有限的统计学家肯定会更喜欢 R。一个强大的 Java 开发者可以继续使用他最喜欢的语言，因为有很多开源项目提供 Java API。对这些图表中的任何语言都可以做出合理的论证。

因此，我的建议是，在投入大量时间学习一种语言之前，阅读其他讨论相同问题的博客。

**更新于2016年12月23日。** 该帖子在[HackerNews](https://news.ycombinator.com/item?id=13239530)上讨论过。

[原始帖子](https://www.ibm.com/developerworks/community/blogs/jfp/entry/What_Language_Is_Best_For_Machine_Learning_And_Data_Science?lang=en)。经授权重新发布。

**简介：**[Jean-François Puget (@JFPuget)](https://twitter.com/jfpuget?lang=en)，博士，是IBM杰出工程师，专注于机器学习和优化。他驻法国圣拉斐尔。

**相关：**

+   [数据科学101：如何提高R语言技能](/2016/11/data-science-101-good-at-r.html)

+   [Dataiku DSS 3.1 – 现在支持5种机器学习后端和Scala！](/2016/08/dataiku-dss-31-machine-learning-backends-scala.html)

+   [在Python中整理数据](/2017/01/tidying-data-python.html)

### 更多关于此主题

+   [建立一个坚实的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)

+   [使用管道编写干净的Python代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [成为伟大的数据科学家所需的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应该掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [是什么让Python成为创业公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)
