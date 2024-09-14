# 如何成为10倍数据科学家，第1部分

> 原文：[https://www.kdnuggets.com/2017/09/become-10x-data-scientist-part1.html](https://www.kdnuggets.com/2017/09/become-10x-data-scientist-part1.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png)[评论](#comments)

**由Stephanie Kim， [Algorithmia](https://algorithmia.com/)**。

最近我在[PyData Seattle](https://pydata.org/seattle2017/)上进行了一个讲座，讨论如何通过借鉴开发者社区的技巧来提升你的数据科学技能。这些建议将帮助你成为一个受团队成员和利益相关者喜爱、更高效的数据科学家。

本文分为五部分，包括：

+   10倍开发者的历史与争议。

+   项目设计。

+   代码设计。

+   工作所需的工具。

+   模型生产化。

* * *

如果你想观看完整的原始讲座，请点击[这里](https://www.youtube.com/watch?v=nFVjLSvK5po)。

![](../Images/4c74ea17cfcb848862ae86dcea757850.png)

10倍开发者是指生产力比普通开发者高10倍的人。

10倍开发者不仅每小时编写的代码比平均开发者多，而且调试能力强，测试代码减少了错误，指导初级开发者，撰写自己的文档，还具备许多超越编程知识的广泛技能。

![](../Images/2f3fa3d46c93e3256382267a8306657e.png)

1968年，由[H. Sackman](http://dl.acm.org/author_page.cfm?id=81100057953&coll=DL&dl=ACM&trk=0&cfid=956367268&cftoken=72985028)、[W. J. Erikson](http://dl.acm.org/author_page.cfm?id=81544973356&coll=DL&dl=ACM&trk=0&cfid=956367268&cftoken=72985028)和[E. E. Grant](http://dl.acm.org/author_page.cfm?id=81332501500&coll=DL&dl=ACM&trk=0&cfid=956367268&cftoken=72985028)进行的实验“探索性实验研究比较了在线和离线编程性能”发现程序员完成编码任务的时间差异很大。

研究聚焦于平均有7年经验的程序员，并发现程序员之间存在20:1的差异。

尽管实验中发现了[缺陷](http://www.construx.com/10x_Software_Development/Origins_of_10X_%E2%80%93_How_Valid_is_the_Underlying_Research_/)（如将使用低级语言的程序员与使用高级语言的程序员混合），但后来有[更多研究](http://www.ybrikman.com/writing/2013/09/29/the-10x-developer-is-not-myth/)得出了类似的结果。

虽然关于10倍开发者是否存在的辩论广泛，但本文将重点介绍如何通过借鉴经验丰富的开发者的技巧和窍门，成为更高效的数据科学家，这些开发者被认为比同行显著更快。

![](../Images/2aa1958f09ae4ecb063e43e6719898a5.png)

**了解业务**

无论你是为教育、生物技术还是金融公司工作，你都应该至少对你所解决问题的业务有一个高层次的理解。

为了有效地传达数据分析背后的故事，你应该找出推动业务发展的因素，并了解业务的目标。

例如，如果你专注于优化食品车的位置，你需要了解人流量、竞争、区域内发生的事件，甚至是天气。你需要了解为什么业务要优化位置，可能是为了增加现有食品车的销售额，或者他们可能正在考虑增加食品车。

即使你今天可能是求职网站的数据科学家，明天可能是在金融公司，你也应该知道是什么让业务运转，以使你的分析对利益相关者相关。

你还应该了解你项目的业务流程，例如谁需要对最终结果进行签署，数据模型在你的部分完成后将传递给谁，以及预期的时间框架是什么。

最后，你应该确保知道谁是利益相关者，并向非技术利益相关者介绍现实的期望。你需要做好教育者的角色，教导非技术利益相关者为什么实现他们的目标可能需要比他们预期更多的时间或资源。

当你理解了利益相关者的目标，并确保你传达了实现他们解决方案所需的技术、专业知识和时间时，你将成为公司更有价值的资产。

![](../Images/4c4d7cd5011b7e5a446b00d996c3aae2.png)

**了解数据**

尽管理解业务很重要，但理解数据更为重要。你需要知道数据是如何提取的，何时提取的，谁负责质量控制，为什么数据中可能会有缺口（例如供应商更换或提取方法更改），可能缺少什么以及还有哪些其他数据源可以添加以创建更准确的模型。

关键在于与不同团队沟通并提出大量问题。不要害怕询问别人正在做什么，并讨论你自己正在做的工作，因为你永远不知道别人是否在做重复工作，或者他们是否有你需要访问的更清洁的数据版本。例如，能够查询数据库将节省你大量时间，而不是对SiteCatalyst进行多次API调用。

![](../Images/48cdda77275cb9cab151690501cde079.png)

为什么在项目设计中花时间和精力会让你成为一个10倍的数据科学家？

+   你只会做需要做的工作（在编码之前考虑一下），因此你会更快地完成项目，因为你会做更少的工作！

+   通过发现客户/用户认为他们需要的东西与他们真正需要的东西之间的误解，你将能够将自己定位为专家和共识的建立者。

+   你将加深对需求的理解，从而避免犯下昂贵的错误。

### 代码设计

尽管在设计代码时有许多最佳实践，但有一些特别突出的做法可以显著提高你的 x 值。

我第一次听到“清晰度胜过巧妙”这个想法是在大学的写作课程中。很容易被自己的巧妙性所吸引，使用最新的流行词来表达你的想法，但就像编程一样，你不仅可能会让自己困惑，还会让他人困惑。

![](../Images/093e5fd1893655d018296b3a13cd45b5.png)

在上面的 Scala 示例中，第一行展示了 `sortBy` 方法的简写语法。虽然它很简洁，但很难理解下划线代表什么。尽管这是许多人在匿名函数中作为参数名称使用的常见模式，对于较少经验的开发者（或当你有一段时间没有查看你的代码时），理解代码的功能会变得繁琐。

在第二个示例中，我们至少使用了一个参数名，并且它展示了赋值，我们可以看到它是按序列 x 中倒数第二个元素进行排序的。

当代码的抽象程度较低时，后续调试会更容易，因此在第三个示例中，我将明确命名我的参数，使其代表数据。

当你的大脑必须经过每一步，并查找或回忆简写代码的功能时，调试的时间会更长，添加新功能也会更费时。因此，即使使用如上例中的简写最初更简洁、打字更快，但从长远来看，避免过于巧妙会对你和其他人都有好处。

![](../Images/ad37f8cc290b9dda1ec9002c8a263b07.png)

虽然我们不会深入探讨缓存，但我们会讨论命名的重要性。想象一下你在查看一些旧代码时，你看到一个像 Scala 示例中的序列被排序：

```py` ```   .sortBy(x => -x._2)   ```py ````

使用单个字母来命名序列完全没有提供有用的信息，因为你可能是从 API、数据库或 Spark 中的数据流中提取数据，在这种情况下，你需要运行代码才能知道“x”是什么。

所以继续之前的 Scala 示例：

```py` ```   sortBy(clothesCount => -clothesCount._2)   ```py ````

你可以在不运行代码的情况下了解我们正在排序的内容。

然而，有时使用 X 作为变量名是完全有理由的。例如，X 经常在机器学习库中使用，其中 X 通常表示观察到的数据，而 y 是尝试预测的变量。在这种情况下，最好使用你所在领域的惯例，其中“model”、“fit”、“predicted”，以及“x”和“y”对该领域的每个人来说都有相同的含义。

在数据科学之外，你需要遵循所使用编程语言的规范。例如，我建议你查看 Python 的 PEP 文档，以了解最佳实践，或者

通过注意命名规范，并且在编码时保持清晰而不是聪明，这将使重构和调试变得更简单、更快捷。遵循这两条编码设计原则，你将更接近成为一个 10 倍的数据科学家。

这里是 [如何成为 10 倍数据科学家，第二部分](/2017/09/become-10x-data-scientist-part2.html)。

[原文](https://blog.algorithmia.com/becoming-a-10x-data-scientist/)。经授权转载。

**简介: [Stephanie Kim](https://www.linkedin.com/in/stephanielkim/)** 是 Algorithmia 的开发者倡导者。

**相关:**

+   [机器学习项目中最重要的步骤是什么？](/2017/08/most-important-step-machine-learning-project.html)

+   [深度学习从零到一：初学者的 5 个令人惊叹的演示及代码，第二部分](/2017/07/deep-learning-demos-code-beginners-part2.html)

+   [数据版本控制：迭代机器学习](/2017/05/data-version-control-iterative-machine-learning.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在的组织进行 IT 工作

* * *

### 相关话题

+   [成为伟大的数据科学家需要的 5 项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家都应掌握的 6 种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [每个数据科学家都应了解的三种 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [停止学习数据科学以寻找目标，并找到目标以…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [构建一个坚实的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)
