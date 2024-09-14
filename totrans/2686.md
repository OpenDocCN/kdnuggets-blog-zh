# 5 篇必读的数据科学论文（以及如何使用它们）

> 原文：[https://www.kdnuggets.com/2020/10/5-must-read-data-science-papers.html](https://www.kdnuggets.com/2020/10/5-must-read-data-science-papers.html)

[评论](#comments)

![](../Images/49c4709a878e9936f82cec20829980e1.png)

*照片由 [Rabie Madaci](https://unsplash.com/@rbmadaci?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供。*

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

数据科学可能是一个年轻的领域，但这并不意味着你不会面临对某些话题的认知期望。本文涵盖了几项最重要的最新发展和有影响力的思想文章。

这些论文涵盖的主题从 **数据科学工作流的协调** 到 **更快的神经网络突破** 再到 **重新思考我们用统计学解决问题的基本方法**。对于每篇论文，我提供了如何将这些想法应用到你自己工作的建议。

### #1 — [机器学习系统中的隐性技术债务](https://papers.nips.cc/paper/5656-hidden-technical-debt-in-machine-learning-systems.pdf)

Google Research 团队提供了 **关于设置数据科学工作流时要避免的反模式的明确说明**。这篇论文借用了软件工程中的技术债务隐喻，并将其应用于数据科学。

![](../Images/d7a3eb411e8454e50fc5b0cf639b5931.png)

*通过 [DataBricks](https://databricks.com/resources)。*

正如下一篇论文更详细探讨的那样，构建机器学习产品是软件工程的一个高度专业化的子集，因此从这一学科中汲取的许多经验也适用于数据科学。

**如何使用**：遵循专家的 [实用技巧](https://papers.nips.cc/paper/5656-hidden-technical-debt-in-machine-learning-systems.pdf) 来简化开发和生产过程。

### #2 — [软件 2.0](https://medium.com/@karpathy/software-2-0-a64152b37c35)

[Andrej Karpathy](https://medium.com/u/ac9d9a35533e?source=post_page-----487cce9a2020--------------------------------) 的这篇经典文章阐述了机器学习模型是 **基于数据的代码的软件应用程序** 这一范式。

如果数据科学是软件，那么我们到底在建设什么？Ben Bengafort在一篇有影响力的博客文章中探讨了这个问题，标题为 [《数据产品的时代》](https://districtdatalabs.silvrback.com/the-age-of-the-data-product)。

![](../Images/37f3758d76bb90a36b2120462fcdeb7a.png)

*数据产品代表了机器学习项目的操作化阶段。照片由 [Noémi Macavei-Katócz](https://unsplash.com/@noemieke?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。*

**如何使用**：进一步阅读关于数据产品如何融入 [模型选择过程](https://medium.com/atlas-research/model-selection-d190fb8bbdda)的内容。

### #3 — [BERT：深度双向转换器的预训练用于语言理解](https://arxiv.org/abs/1810.04805)

在这篇论文中，Google Research团队提出了一种自然语言处理（NLP）模型，这代表了我们在文本分析能力上的一次跃迁。

尽管关于BERT为何如此有效有 [一些争议](https://text-machine-lab.github.io/blog/2020/bert-secrets/)，但这提醒我们，机器学习领域可能已经发现了一些成功的方法，而并未完全理解其工作原理。[正如自然界中的情况](https://www.youtube.com/watch?v=B7m0e-3u-1Y&t=1s)，人工神经网络充满了神秘感。

*在这段有趣的片段中，Nordstrom的数据科学总监解释了人工神经网络如何从自然界中汲取灵感。*

**如何使用**：

+   [BERT论文](https://arxiv.org/abs/1810.04805)非常易读，并包含了一些建议的默认超参数设置作为宝贵的起点（见附录A.3）。

+   无论你是否对NLP新手，都可以查看Jay Alammar的 [《首次使用BERT的视觉指南》](http://jalammar.github.io/a-visual-guide-to-using-bert-for-the-first-time/) 来获得对BERT能力的生动插图。

+   另外，查看 [ktrain](https://arxiv.org/abs/2004.10703)，这是一个基于Keras（进而基于TensorFlow）的包，它允许你轻松地在工作中实现BERT。[Arun Maiya](https://medium.com/u/4581d07591d5?source=post_page-----487cce9a2020--------------------------------) 开发了这个强大的库，以加快NLP、图像识别和基于图的方法的洞察速度。

### #4 — [彩票票据假设：寻找稀疏的、可训练的神经网络](https://arxiv.org/abs/1803.03635)

尽管NLP模型越来越大（例如GPT-3有1750亿参数），但也有一种正交的努力在寻找更小、更快、更高效的神经网络。这些网络承诺更快的运行时间、更低的训练成本和更少的计算资源需求。

在这篇开创性的论文中，机器学习天才Jonathan Frankle和Michael Carbin概述了一种剪枝方法，以发现能够达到与原始的显著更大神经网络相当性能的稀疏子网络。

![](../Images/0bab24fe5c328bfb0161b39e85b4fbb3.png)

*通过[Nolan Day](https://medium.com/u/6438fd23c99a?source=post_page-----487cce9a2020--------------------------------)的“[解析彩票票据假设](https://towardsdatascience.com/breaking-down-the-lottery-ticket-hypothesis-ca1c053b3e58)”*

**彩票票据**指的是具有初始权重的连接，使得它们特别有效。这个发现提供了许多在存储、运行时间和计算性能上的优势——并且在ICLR 2019上获得了最佳论文奖。进一步的研究基于这一技术，[证明了其适用性](https://arxiv.org/abs/2002.00585)和[将其应用于原本稀疏的网络](https://arxiv.org/abs/1911.11134)。

**如何使用**：

+   在将神经网络投入生产之前，请考虑[剪枝](https://jacobgil.github.io/deeplearning/pruning-deep-learning)。剪枝网络权重可以将参数数量减少90%以上，同时仍能实现与原始网络相同的性能水平。

+   此外，查看这个[Data Exchange播客的剧集](https://thedataexchange.media/software-and-commodity-hardware-can-handle-deep-learning/)，Ben Lorica与[Neural Magic](https://neuralmagic.com/about/)的创始人讨论了该初创公司如何利用[剪枝和量化](https://www.youtube.com/watch?v=3JWRVx1OKQQ)等技术，并提供了一个使得实现稀疏性的UI更加简单的解决方案。

**阅读更多**：

+   [看看这篇有趣的侧边栏](https://ml-retrospectives.github.io/neurips2019/accepted_retrospectives/2019/lottery-ticket/)来自“彩票票据”作者之一，讨论了机器学习社区在评估好主意时存在的缺陷。

### #5 — [摆脱零假设统计检验的束缚 (*p* < .05)](https://www.researchgate.net/publication/312395254_Releasing_the_death-grip_of_null_hypothesis_statistical_testing_p_05_Applying_complexity_theory_and_somewhat_precise_outcome_testing_SPOT)

> 经典假设检验方法导致过度确定性，并产生通过统计方法已识别原因的错误观念。 ([阅读更多](http://wmbriggs.com/public/Briggs.ReplacementForHypothesisTesting.pdf))

假设检验早于计算机的使用。鉴于这种方法所面临的挑战（例如，[即使是统计学家也发现解释p值几乎是不可能的](https://fivethirtyeight.com/features/statisticians-found-one-thing-they-can-agree-on-its-time-to-stop-misusing-p-values/)），也许是时候考虑一些替代方案，例如稍微精确的结果测试（SPOT）。

![](../Images/bb6b71489ba73c9b787f9ebd902bbff7.png)

*“显著”通过[xkcd](https://xkcd.com/882/)。*

**如何使用**：

+   查看这篇博文，“[统计假设检验的终结](https://www.datasciencecentral.com/profiles/blogs/the-death-of-the-statistical-test-of-hypothesis)”，一位沮丧的统计学家阐述了传统方法的一些挑战，并解释了使用置信区间的替代方案。

> [注册以获取“2020年最后几个月提升数据科学的资源”发布通知](https://page.co/ahje9p)

[原文](https://towardsdatascience.com/must-read-data-science-papers-487cce9a2020)。经许可转载。

**简介：** [妮可·詹维·比尔斯](http://www.linkedin.com/in/nicole-janeway-bills) 是一位拥有商业和联邦咨询经验的机器学习工程师。妮可精通Python、SQL和Tableau，具有自然语言处理（NLP）、云计算、统计测试、定价分析和ETL流程的业务经验，旨在利用这些背景将数据与业务成果连接起来，并继续发展技术技能。

**相关：**

+   [2020年需要阅读的AI论文](https://www.kdnuggets.com/2020/09/ai-papers-read-2020.html)

+   [数据科学家必读的NLP和深度学习文章](https://www.kdnuggets.com/2020/08/must-read-nlp-deep-learning-articles.html)

+   [AI专家必读的13篇论文](https://www.kdnuggets.com/2020/05/13-must-read-papers-ai-experts.html)

### 了解更多相关话题

+   [KDnuggets™ 新闻 22:n06，2月9日：数据科学编程…](https://www.kdnuggets.com/2022/n06.html)

+   [数据科学编程语言及其使用时机](https://www.kdnuggets.com/2022/02/data-science-programming-languages.html)

+   [数据分析：分析数据的四种方法及其有效应用](https://www.kdnuggets.com/2023/04/data-analytics-four-approaches-analyzing-data-effectively.html)

+   [数据科学面试中的24个A/B测试面试问题及其他…](https://www.kdnuggets.com/2022/09/24-ab-testing-interview-questions-data-science-interviews-crack.html)

+   [进入数据科学：必备技能及如何学习](https://www.kdnuggets.com/breaking-into-data-science-essential-skills-and-how-to-learn-them)

+   [5个常见的数据科学错误及如何避免它们](https://www.kdnuggets.com/5-common-data-science-mistakes-and-how-to-avoid-them)
