# 24个数据科学面试中的A/B测试问题及其破解方法

> 原文：[https://www.kdnuggets.com/2022/09/24-ab-testing-interview-questions-data-science-interviews-crack.html](https://www.kdnuggets.com/2022/09/24-ab-testing-interview-questions-data-science-interviews-crack.html)

![数据科学面试中的24个A/B测试问题及其破解方法](../Images/3208ff6b9b04af8c1c7ce3bf3f86398b.png)

一种常见的[顶级公司数据科学面试问题](https://www.stratascratch.com/blog/40-data-science-interview-questions-from-top-companies/)就是关于朴素的A/B测试。A/B测试对企业非常重要——它们帮助企业决定从构建哪个产品到如何设计用户界面等一切。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你在IT领域的组织

* * *

A/B测试，也称为拆分测试，是一种受控实验。你基本上将一组人，如用户或最终客户，分成两半。给一半的人提供选项A。给另一半的人提供选项B。然后看看哪个选项表现更好。

例如，Instagram可能想知道将购物页面变得更突出是否会影响用户的整体消费。一组数据科学家会通过选择一部分现有的Instagram用户，并测试原始设计和新设计，来看新设计是否能提升消费。

（另一个更近期的例子是Instagram [更改](https://techcrunch.com/2022/05/03/instagram-test-full-screen-video-home-feed/) 他们的动态为全屏模式——测试组中的用户 [讨厌](https://www.ladbible.com/news/instagram-users-vow-to-boycott-app-20220629) 这一变化！）

A/B测试对公司至关重要。如果你即将参加数据科学面试，这一概念尤为重要。你需要对A/B测试背后的统计概念、如何设计一个好的实验以及如何解释结果有足够的信心。

这里是你需要了解的关于数据科学面试中A/B测试问题的所有信息。

# 如何解决A/B测试问题

![数据科学面试中的24个A/B测试问题及其破解方法](../Images/7408acf914e6966f416e5e3ebc241c43.png)

我在上面做了简要描述，但我想利用这一部分深入探讨这个概念。当你在数据科学面试中被问及 A/B 测试时，面试官希望你展示出对概念的理解以及你的商业头脑。

首先让我们讨论概念。A/B 测试是一种统计学和两样本假设测试。当你进行 [统计假设测试](https://en.wikipedia.org/wiki/Statistical_hypothesis_testing) 时，你是在将样本数据集与总体数据进行比较。[两样本假设测试](https://en.wikipedia.org/wiki/Two-sample_hypothesis_testing) 是使用统计学来确定两个样本之间的差异是否显著。

当你设置 A/B 测试时，需要两个假设：原假设和备择假设。原假设认为你观察到的任何变化完全是偶然的。基本上，它意味着你认为对照组和测试组之间没有差异。

你的备择假设认为，对照组和测试组之间会有统计学上的显著差异。

要制定假设，你应该使用 [PICOT](https://libraryguides.nau.edu/c.php?g=665927&p=4682772) 方法：人群、干预、对照、结果和时间。

这是一个可以使用的模板：

在 _______(P) 中，______ (I) 对 ______(O) 的影响，与 _______(C) 相比，在 ________ (T) 内如何？

以下是 PICOT 格式的 Outlook 示例：

在 **Instagram 用户 (P)** 中，**使购物页面更突出 (I)** 对 **消费 (O)** 的影响，与 **未更改的购物页面 (C)** 相比，**三个月 (T)** 的时间内如何？

你还需要为对照组和测试组制定标准。你可能没有资源对所有用户进行 A/B 测试，因此需要找到一个选择样本群体的好方法。关键是要随机抽样，以确保结果具有显著性。

如果你不小心选择了一组全是百万富翁的新用户，这将会独立地扭曲你的结果，与您尝试进行的测试无关。

你可以使用 Laerd Dissertation 的 [指南](https://dissertation.laerd.com/getting-started.php) 来选择随机样本。确保你的样本足够大，以具有统计学意义。我推荐 Andrew Fisher 的 [指南](https://www.researchgate.net/publication/259607496_Stratified_Fisher's_Exact_Test_and_its_Sample_Size_Calculation)。

一旦你创建了假设、选择了样本并运行了测试，就该分析结果了。在推荐做出全面改变之前，你需要绝对确定这是否值得。

假设你在每组中测试了 1,000 个用户。你发现对照组的平均消费为 $50，标准差为 $15。测试组的平均消费为 $53，标准差为 $16。

这可能看起来不多，但有如此大的样本量，你可以非常有信心地拒绝零假设——这给我们一个p值（双尾）= 0.000016，这意味着结果通过运气或偶然发生的概率仅为0.0016%。

选择适当测试时有很多不同的因素。如果你的样本量较小（n<30），你应该选择t检验而不是z检验。如果你认为你的样本均值可能大于、小于或等于对照均值，你应该选择双尾检验。我很喜欢Antoine Soetewey的[流程图](https://statsandr.com/blog/files/overview-statistical-tests-statsandr.pdf)来做决策。查看这个图表可以让你全面了解A/B测试及其设计背后的假设。

# A/B测试的注意事项

当你回答这些问题时，你可能会被测试你的思维灵活性。A/B测试很好，但情况通常不理想。雇主想知道你如何应对挑战。

如果你在B2B领域，你可能会面临较小的样本量。由于[社交网络效应](https://engineering.linkedin.com/blog/2019/06/detecting-interference--an-a-b-test-of-a-b-tests)，你可能无法假设独立的总体。你可能没有足够的资源或时间来设计一个完美的测试。有时候进行A/B测试可能没有意义。

面试官希望听到你计划如何面对这些挑战，以及你对统计测试的掌握程度。

你将会面临以下问题：

+   如果你不能运行测试到想要的时间，你将如何计划弥补样本量小的问题？

+   你将如何最小化控制组和测试组之间的干扰？

+   如果你不能运行测试到想要的时间，你将如何使测试具有鲁棒性？

# A/B测试问题的示例

![数据科学面试中的24个A/B测试问题及破解方法](../Images/c0878830b6e9bc800bf2d41799a4e8a9.png)

让我们解决一个来自StrataScratch平台的[现实世界A/B测试问题](https://platform.stratascratch.com/technical/2341-ab-testing-a-campaign)示例：

Robinhood希望鼓励更多人创建账户，并计划发起一个活动，向新用户提供一些免费股票。详细描述你将如何实施A/B测试以确定此活动的成功。

当你回答这种类型的面试问题时，你需要按顺序解决问题：

+   做出任何澄清声明或问题。

+   在这种情况下，“几只股票”是什么意思？是否可以测试多个免费股票的数量？

+   什么指标衡量成功？保留率、花费的钱，还是其他什么？

+   选择哪只股票？还是提供任何X值的股票？

+   确定：

    +   实验的时长。

    +   你将如何选择你的样本。

    +   你的样本量。

    +   对于这些问题，没有正确的答案。只是你如何确定这些的思考过程。

+   发展你的假设：

    +   在新的Robinhood用户（P）中，与不给新的注册用户免费股票（C）相比，免费股票（I）对你的成功指标（O）的影响如何，在你选择的时间范围内（T）？

+   进行分析。

+   测量结果：

    +   你会使用什么测试，为什么？

    +   什么才算成功？

+   最后的想法：

    +   你将如何实施测试结果？

    +   你将如何向利益相关者传达结果？

    +   你还会试验哪些其他测试？

    # 你可能会遇到的24个A/B测试面试问题

    我已经从网上收集了几个实际的面试问题，以提供面试官可能会问你的这些概念的示例。在点击链接获取详细答案之前，尽量自己先回答这些问题。

    **实验设计与设置**

    +   A/B测试的目标是什么？ – [Hackr.io](https://hackr.io/blog/data-science-interview-questions)

    +   A/B测试的理想条件是什么？ – [Amplitude Blog](https://amplitude.com/blog/a-b-testing-interview-questions)

    +   有一些增加电子商务网站转化率的想法，比如启用多商品结账（目前用户只能逐一结账）、允许未注册用户结账、改变“购买”按钮的大小和颜色等，你会如何选择投资哪个想法？ - [Emma Ding, 数据科学的未来](https://towardsdatascience.com/7-a-b-testing-questions-and-answers-in-data-science-interviews-eee6428a8b63#306e)

    +   X公司测试了一项新功能，目标是增加每个用户创建的帖子数量。他们将每个用户随机分配到控制组或实验组。测试结果在帖子数量上胜出1%。你期望在所有用户都上线新功能后会发生什么？如果不是1%，那会更多还是更少？（假设没有新颖效应） - [Emma Ding, 数据科学的未来](https://towardsdatascience.com/7-a-b-testing-questions-and-answers-in-data-science-interviews-eee6428a8b63#5072)

    +   我们正在推出一项新功能，为骑行者提供优惠券。目标是通过降低每次骑行的价格来增加骑行次数。请概述一个测试策略来评估新功能的效果。 - [Emma Ding, 数据科学的未来](https://towardsdatascience.com/7-a-b-testing-questions-and-answers-in-data-science-interviews-eee6428a8b63#22a0)

    +   如果A/B测试不可用，你会如何回答问题？ – [Amplitude Blog](https://amplitude.com/blog/a-b-testing-interview-questions)

    +   FB推出了一个类似Zoom的功能。这个功能普遍受到欢迎，使用量在增长。你在Instagram工作。你会如何评估Instagram是否应该添加这个类似Zoom的功能？ – [u/Objective-patient-37 在Reddit](https://www.reddit.com/r/datascience/comments/o35vgb/faang_interview_prep_ab_testing_please_be/)

    **解决实验问题**

    +   我们对一个新功能进行了A/B测试，测试结果胜出，因此我们将更改推出给所有用户。然而，在推出功能一周后，我们发现处理效果迅速下降。发生了什么？– [艾玛·丁，《数据科学的前沿》](https://towardsdatascience.com/7-a-b-testing-questions-and-answers-in-data-science-interviews-eee6428a8b63#e963)

    +   我们正在进行一个有10种变体的测试，尝试不同版本的登录页面。一个处理获胜，p值小于0.05。你会进行更改吗？– [艾玛·丁，《数据科学的前沿》](https://towardsdatascience.com/7-a-b-testing-questions-and-answers-in-data-science-interviews-eee6428a8b63#4848)

    +   你会运行实验多长时间？– [Amplitude Blog](https://amplitude.com/blog/a-b-testing-interview-questions)

    +   在A/B测试中，你如何检查分配到各个组是否真正随机？– [StrataScratch](https://platform.stratascratch.com/technical/2055-random-bucketing)

    +   你如何处理小样本量问题？– [Amplitude Blog](https://amplitude.com/blog/a-b-testing-interview-questions)

    +   在我们产品的开发周期中，哪些问题可能会影响你的A/B测试结果？– [Amplitude Blog](https://amplitude.com/blog/a-b-testing-interview-questions)

    +   你如何减轻这些问题？– [Amplitude Blog](https://amplitude.com/blog/a-b-testing-interview-questions)

    **业务相关问题**

    +   讲讲你设计的一个成功的A/B测试。你想要学习什么，学到了什么，这段经历如何帮助你在我们公司工作？– [Amplitude Blog](https://amplitude.com/blog/a-b-testing-interview-questions)

    +   根据你使用我们产品的经验，你会建议哪些改进，并为这些改进设置什么实验？– [Amplitude Blog](https://amplitude.com/blog/a-b-testing-interview-questions)

    +   假设我们想在用户流程实验中比较功能A和功能B。根据你对我们产品的了解，你会如何设计这个测试？– [Amplitude Blog](https://amplitude.com/blog/a-b-testing-interview-questions)

    +   你如何处理超长期指标，比如在测试用户在看到功能后的两个月内花费多少钱时，需要等待两个月才能得到指标？– [Amplitude Blog](https://amplitude.com/blog/a-b-testing-interview-questions)

    **数据分析**

    +   运行测试后，你看到期望的指标，比如点击率上升，而展示次数下降。你会如何做出决策？– [艾玛·丁，《数据科学的前沿》](https://towardsdatascience.com/7-a-b-testing-questions-and-answers-in-data-science-interviews-eee6428a8b63#3d12)

    +   如果你的实验结果不明确，看起来更像是AA测试，你会怎么做？你会如何分析测试结果，重点关注什么？– [Amplitude Blog](https://amplitude.com/blog/a-b-testing-interview-questions)

    +   给定两个产品活动的数据，如果我们看到其中一个产品增长了3%，你如何进行A/B测试？– [igotanoffer](https://igotanoffer.com/blogs/tech/google-data-science-interview)

    +   当你知道存在社交网络效应且独立性假设不成立时，这会如何影响你的分析和决策？– [Amplitude Blog](https://amplitude.com/blog/a-b-testing-interview-questions)

    +   在我们的A/B测试中，结果没有统计学意义。这可能有哪些潜在原因？– [Amplitude Blog](https://amplitude.com/blog/a-b-testing-interview-questions)

    **未来调查**

    +   如果你的A/B测试团队已经有了X、Y、Z角色的团队成员，你会建议招聘哪些新成员？– [Amplitude Blog](https://amplitude.com/blog/a-b-testing-interview-questions)

    +   在你的产品团队中，哪些角色应该参与到测试中？你会如何让他们轻松参与？– [Amplitude Blog](https://amplitude.com/blog/a-b-testing-interview-questions)

    # 准备A/B测试数据科学面试问题的更多资源

    这个A/B测试面试问题指南和24个示例问题应该能让你完全准备好应对任何A/B测试面试问题。如果你对上述提到的术语不熟悉，或者想要更多内容进行复习，请查看以下资源：

    +   [A/B 测试数据科学面试问题指南](https://www.stratascratch.com/blog/ab-testing-data-science-interview-questions-guide/)

    +   [数据科学中统计测试的基本类型](https://www.stratascratch.com/blog/basic-types-of-statistical-tests-in-data-science/)

    +   [阅读更多关于p值的内容](https://platform.stratascratch.com/technical/2043-p-value)

    +   [什么是A/B测试？| 数据科学简明教程](https://www.youtube.com/watch?v=zFMgpxG-chM)

    +   [破解数据科学面试中的A/B测试问题 | 产品感觉 | 案例面试](https://www.youtube.com/watch?v=X8u6kr4fxXc)

    +   [A/B 测试指南](https://vwo.com/ab-testing/)

    +   [A/B 测试实际案例：逐步演示](https://www.youtube.com/watch?v=VuKIN9S8Ivs)

    像任何数据科学面试问题一样，记得以好奇心来面对这些问题。你不仅要展示你对统计和数学的掌握，还要展现你的思维过程和查看大局的能力。祝好运，并且玩得开心。

    **[内特·罗西迪](https://www.stratascratch.com)** 是一名数据科学家和产品策略专家。他还担任分析学的兼职教授，并且是[StrataScratch](https://www.stratascratch.com/)的创始人，这个平台帮助数据科学家通过顶级公司的真实面试问题为面试做好准备。可以在[Twitter: StrataScratch](https://twitter.com/StrataScratch)或[LinkedIn](https://www.linkedin.com/in/nathanrosidi/)与他联系。

    ### 了解更多相关内容

    +   [破解机器学习工程师面试需要具备的条件](https://www.kdnuggets.com/2022/10/interview-kickstart-crack-machine-learning-engineer-interviews.html)

    +   [假设检验和 A/B 测试](https://www.kdnuggets.com/hypothesis-testing-and-ab-testing)

    +   [KDnuggets 新闻，5月4日：9 门免费哈佛课程学习数据…](https://www.kdnuggets.com/2022/n18.html)

    +   [数据分析师的 SQL 和 Python 面试问题](https://www.kdnuggets.com/2023/02/sql-python-interview-questions-data-analysts.html)

    +   [如何回答数据科学编码面试问题](https://www.kdnuggets.com/2022/01/answer-data-science-coding-interview-questions.html)

    +   [15 个你必须知道的 Python 编码面试问题](https://www.kdnuggets.com/2022/04/15-python-coding-interview-questions-must-know-data-science.html)
