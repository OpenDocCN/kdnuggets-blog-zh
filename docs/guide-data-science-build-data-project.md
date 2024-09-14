# 《外行人数据科学指南》。第二部分：如何构建数据项目

> 原文：[https://www.kdnuggets.com/2020/04/guide-data-science-build-data-project.html](https://www.kdnuggets.com/2020/04/guide-data-science-build-data-project.html)

[评论](#comments)

**由 [Sciforce](https://sciforce.solutions/) 提供，基于科学驱动的信息技术的软件解决方案。**

![](../Images/f4a9c428eb294a028035505ef01858e0.png)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT

* * *

我们的博客中经常探讨尖端技术之间的复杂联系，或探究新技术的迷人深度。然而，人工智能或数据科学不仅仅是吹嘘提高准确率 2%（这是一个很大的提升）的新方法，而是让数据和技术为你服务。它将帮助你增加销售、了解你的顾客、预测生产线中的未来故障，或者制作一份有见地的报告、提交一个学期项目，甚至和朋友们一起合作一个能改变世界的新想法。在这个意义上，每个人都可以——并在某种程度上应该——成为数据科学家。

我们在本指南的第一部分中已经讨论了[什么使一个优秀的数据科学家](https://www.kdnuggets.com/2019/10/good-data-scientist-beginner-guide.html)以及你在真正开始一个项目之前应该学习什么。在这篇文章中，我们将通过简单的步骤带你了解如何构建一个基础数据项目。

![](../Images/9573d8a175564c497bf4d8c7d2b73fc1.png)

*寻找一个想法背后的故事。*

你脑海中有一个绝妙的想法——也许是你从小就珍视的一个玩具清洁机器人，或者是你刚刚想到的一个新点子，通过发送基于购买偏好的幸运饼干来接触你店里的顾客。然而，为了让你的想法付诸实践，你需要其他人的关注。为它找到一个引人入胜的叙述；确保它有一个吸引眼球的开头或迷人的目的，确保它是最新的和相关的。找到叙述结构将帮助你决定你是否真的有一个故事要讲。

这样的叙述将成为你商业模型的基础。问问自己：你开发了什么，什么资源是必需的，你为客户提供了什么价值？客户会为哪些价值付费？

一种不错的方法是 [商业模式画布](https://en.wikipedia.org/wiki/Business_Model_Canvas)。它简单且便宜，你可以在一张纸上创建它。

![](../Images/9596f265983c46f570e81bc665fa976c.png)

*准备数据。*

第一步是收集数据以推动你的项目。根据你的领域和目标，你可以搜索互联网上现成的数据集，例如这个 [集合](https://www.springboard.com/blog/free-public-data-sets-data-science-project/)。你可以选择从网站抓取数据，或通过公共 API 从社交网络获取数据。对于后者选项，你需要编写一个小程序，以你最熟悉的编程语言从社交网络下载数据。对于云选项，你可以启动一个简单的 [AWS EC2 Linux 实例](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html) (nano 或 micro)，并在上面运行你的软件。

存储数据的最佳方法是使用简单的 .csv 格式，每行包括文本和元信息，如人员、时间戳、回复和点赞。

关于所需数据量的经验法则是，在合理的时间内获取尽可能多的数据，例如，运行几天你的程序。另一个重要考虑因素是收集尽可能多的数据，以便你的分析机器可以处理。获取多少数据不是一门精确的科学，而是取决于技术限制和你所提出的问题。

最后，在收集和管理数据时，至关重要的是 [避免偏见](http://www.nytimes.com/interactive/2012/11/02/us/politics/paths-to-the-white-house.html?_r=0)，不要在数据的包含或排除上有选择性。这种选择性包括在数据是连续时使用离散值；处理缺失、离群和超出范围的值；任意时间范围；封顶值、体积、范围和区间。即使是有争议的影响，也应基于数据的实际情况，而不是你希望它所说的。

![](../Images/9cd95b24349af0cd8d8b8c2a85564769.png)

*选择合适的工具。*

要进行有效分析，你需要找到合适的工具。获取数据后，你需要选择合适的工具来探索它。为了做出选择，你可以列出你认为需要的分析功能，并比较现有工具。有时你可以使用用户友好的图形工具，如 Orange、Rapid Miner 或 Knime。在其他情况下，你将需要使用 Python 或 R 等语言自行编写分析。

![](../Images/67743658fe6cad922a50a0c0b3009547.png)

*验证你的理论。*

利用现有的数据和工具，你可以验证你的理论。在数据科学中，理论是关于世界应该是怎样或实际怎样的陈述，它们源于对世界的假设或先前理论（[Das, 2013](https://srdas.github.io/Papers/DSA_Book.pdf)）。模型是理论的实现；在数据科学中，它们通常是基于理论的算法，这些算法在数据上运行。运行模型的结果会基于理论、模型和数据对世界有更深刻的理解。

在初步评估你的理论时，结合更一般和传统的内容分析，你可以指出数据中存在的趋势。我们常用的一种方法是选择已报告的重要事件。然后你可以尝试创建一个分析过程来发现这些趋势。如果分析能够找到你指定的趋势，那么你就走在了正确的道路上。寻找分析发现的新趋势的实例。例如，可以通过搜索互联网来确认这些趋势。结果不可能100%可靠，因此你需要决定容忍多少错误报告的趋势（错误率）。

![](../Images/401c43ffb6f3fa72c372d6b307bf5bb8.png)

*构建一个最小可行产品。*

当你有了商业模型和验证过的理论，就该构建你的产品的第一个版本，即所谓的最小可行产品（MVP）。基本上，这可以是你提供给客户的第一个版本。由于最小可行产品（MVP）是一个具有足够功能以满足早期客户并提供未来开发反馈的产品，它应仅关注核心功能而没有任何花哨的解决方案。你应该坚持使用最初能正常工作的简单功能，并在后续扩展系统。在这个阶段，系统可能看起来像这样：

![](../Images/03002524b564c55e4f4409120d2392e5.png)

![](../Images/4aa0460f936b81c257a55dae348884bd.png)

*自动化你的系统。*

原则上，你的重点应放在产品的未来发展上，而不是系统的运行。为此，你需要尽可能多地进行自动化：上传到S3，启动分析或数据存储。在[this article](https://medium.com/sciforce/automation-in-it-31550abbda41)中，我们详细讨论了自动化。

自动化的另一面是日志记录。当一切都被自动化时，你可能会感觉自己对系统的控制力减弱，不知道它的性能如何。此外，你需要知道接下来要开发什么，无论是新功能还是修复问题。为此，你需要建立一个系统来记录、监控和测量所有有意义的数据。例如，你应该记录数据的下载统计信息或将其上传到S3，分析过程的时间，以及用户的行为。

有很多 [工具](https://stackify.com/best-log-management-tools/) 可以帮助你记录服务器统计数据，如CPU、RAM、网络、代码级性能和错误监控，其中许多工具都有用户友好的界面。

![](../Images/2f19315e0c6c8d35b89137def3d62481.png)

*重复和扩展。*

你可能知道，人工智能、机器学习、数据科学及其他新发展都涉及重复和微调。因此，当你的MVP运行、自动化和监控就位后，你可以开始提升你的系统。这是时候摆脱弱点，优化整体性能和稳定性，并添加新功能。实施新功能还会让你提供新的服务或产品。

![](../Images/f82ecba9d8ced30ecea8cab1a213351b.png)

*展示你的产品。*

最后，当你的产品准备好时，你需要向客户展示它。这时你讲述的数据和商业模型背后的故事就派上用场了。

首先，考虑你的目标受众。你的客户是谁，你打算如何向他们销售你的产品？你要向他们展示的受众对这个话题了解多少？故事需要围绕受众已经掌握的信息水平进行构建，包括正确和不正确的部分：

+   [新手](http://www.visual-literacy.org/periodic_table/periodic_table.html)：第一次接触该主题，但不希望过度简化

+   [通才](https://vimeo.com/6437816#at=0)：对话题有一定了解，但寻求概览理解和主要主题

+   [管理层](http://www.flickr.com/photos/densitydesign/3409542518/sizes/l/in/photostream/): 对复杂性和相互关系的深入、可操作的理解，并可以访问详细信息

+   [专家](http://web.mit.edu/hdenny/Public/blog%20post%20images/DataGovScreenshot_blog.jpg)：更多的探索和发现，讲述细节较少

+   [高管](https://hbr.org/resources/images/article_assets/2011/11/BI_dashboard_example.jpg)：只有时间了解加权概率的意义和结论

之后，视觉化你的数据，并将你构建的项目中的趋势、意义和比例融入到叙述中。关于产品的故事不应该以固定的事件结束，而是应以一组选项或问题来引发受众的行动。永远不要忘记数据讲故事的目标是激发和激励对商业决策或购买你产品的关键思维。

[原始](https://medium.com/sciforce/a-laymans-guide-to-data-science-part-2-how-to-build-a-data-project-58237a78860e)。转载已获许可。

**简介：** [SciForce](https://sciforce.solutions) 是一家总部位于乌克兰的IT公司，专注于基于科学驱动的信息技术开发软件解决方案。我们在许多关键的人工智能技术领域具有广泛的专业知识，包括数据挖掘、数字信号处理、自然语言处理、机器学习、图像处理和计算机视觉。

**相关：**

+   [如何成为一名（优秀的）数据科学家 – 初学者指南](https://www.kdnuggets.com/2019/10/good-data-scientist-beginner-guide.html)

+   [对新手和初级数据科学家的建议](https://www.kdnuggets.com/2019/11/advice-new-junior-data-scientists.html)

+   [作为数据科学家的第一年学到的九个教训](https://www.kdnuggets.com/2020/03/nine-lessons-first-year-data-scientist.html)

### 了解更多相关内容

+   [构建一个可复现且可维护的数据科学项目：一本免费的…](https://www.kdnuggets.com/2022/08/free-book-build-reproducible-maintainable-data-science-project.html)

+   [如何构建数据科学项目：一步一步的指南](https://www.kdnuggets.com/2022/05/structure-data-science-project-stepbystep-guide.html)

+   [数据科学项目管理方法指南](https://www.kdnuggets.com/2023/07/guide-data-science-project-management-methodologies.html)

+   [如何建立数据科学支持团队：完整指南](https://www.kdnuggets.com/2022/10/build-data-science-enablement-team-complete-guide.html)

+   [数据科学面试指南 - 第2部分：面试资源](https://www.kdnuggets.com/2022/04/data-science-interview-guide-part-2-interview-resources.html)

+   [数据科学面试指南 - 第1部分：结构](https://www.kdnuggets.com/2022/04/data-science-interview-guide-part-1-structure.html)
