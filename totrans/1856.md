# 如何成为（A型）数据科学家

> 原文：[https://www.kdnuggets.com/2016/08/become-type-a-data-scientist.html](https://www.kdnuggets.com/2016/08/become-type-a-data-scientist.html)

[![2016年8月顶级KDnuggets博客作者](../Images/705fa03d5e99099392dc8250fc150d31.png)](/2016/08/top-news-week-0822-0828.html)。

在Quora上有一篇由Michael Hochster撰写的有趣文章，关于[数据科学是什么](https://www.quora.com/What-is-data-science)，讨论了两种类型的数据科学家：A型数据科学家和B型数据科学家（强调的是我个人的观点）

![数据科学家](../Images/df705f7dabbe34f4bbd090660555c7cb.png)

> * * *
> 
> ## 我们的三大课程推荐
> ## 
> ![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。
> 
> ![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析水平
> 
> ![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织IT
> 
> * * *
> 
> 数据科学家是那些拥有编程和统计技能混合的人，他们以各种方式使数据变得有用。在我的领域，有两种主要类型：
> 
> A型数据科学家：A代表分析。这种类型的数据科学家主要关注于对数据进行理解或以相对静态的方式处理数据。A型数据科学家**与统计学家非常相似（甚至可能是统计学家），但掌握所有统计课程中未教授的实际数据处理细节**：**数据清洗、处理非常大数据集的方法、可视化、特定领域的深厚知识、写作技巧等**。
> 
> A型数据科学家**能够很好地编程以处理数据，但不一定是专家**。A型数据科学家可能在实验设计、预测、建模、统计推断或其他统计部门通常教授的领域中是专家。然而，通常来说，数据科学家的工作产品不是“p值和置信区间”，虽然学术统计学有时会这么暗示（比如传统统计学家在制药行业工作的情况）。在谷歌，A型数据科学家有时被称为统计学家、定量分析师、决策支持工程分析师或数据科学家，还有可能有更多的称谓。
> 
> **Type B 数据科学家：B 代表构建**。Type B 数据科学家在统计背景方面与 Type A 有一些共同之处，但他们也是非常强的编码员，可能是受过培训的软件工程师。Type B 数据科学家主要对在“生产环境”中使用数据感兴趣。他们构建与用户互动的模型，通常提供推荐（产品、可能认识的人、广告、电影、搜索结果）。

在这篇文章中，我们讨论了将你的职业生涯**转型为（Type A）数据科学家**的策略。

我在这里讨论的观点是基于我与[物联网数据科学课程](http://www.opengardensblog.futuretext.com/archives/2016/06/data-science-for-internet-of-things-course-aug-sep-2016-now-in-its-fourth-batch.html)参与者的合作。

当我第一次读到 Type A 和 Type B 数据科学家的概念时，我觉得它非常解放。

这让你意识到，‘独角兽’数据科学家（无所不知！）——就像他们的骑马同行——也在很大程度上是虚构的。

承认这一点后，你就可以开始朝着将你的职业生涯转向数据科学的策略取得实际进展。

在这里，我讨论了基于我与课程参与者合作的经验，如何从 Type A 数据科学家转型的十二种不常见策略。

但首先，让我们讨论一个共同的主题。是的，你必须构建一个应用程序来学习数据科学。但仅仅构建是不够的。当你从有限的资源开始，尝试构建一个严肃的数据科学应用时，你会很快发现最大的限制是缺乏严肃的数据。因此，像其他人一样，你最终也会选择基于[UCI 数据集](https://archive.ics.uci.edu/ml/datasets.html)（或类似的）来构建你的应用。因此，除了构建之外，你还需要考虑更广泛的策略。以下是我们在教学中遵循的一些策略：

注意事项：

+   这里的受众是那些自己探索数据科学的人。

+   我将 Type A 分类为那些利用数据科学解决许多复杂问题的人——但不负责在生产环境中处理高性能模型。

1.  **从你所知道的开始**：这看起来很明显，但经常被忽视。例如，假设你在 Oracle 工作了很长时间，现在你想成为一名数据科学家。为什么不从 Oracle 开始呢？有一整套[Oracle BI 工具](https://cloud.oracle.com/en_US/business_intelligence)。这些工具涵盖了从可视化到高级分析的整个范围。它们使用 Pl/SQL 或 R。这可以帮助你更快地入门，而不是学习完全新的东西。我也会对微软（[Azure 和 Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-azure-and-power-bi/)）和[亚马逊（AWS IoT）](https://aws.amazon.com/iot/how-it-works/)使用相同的策略——这两个平台都很先进。

1.  **专注于统计学**：存在将数据科学（分析和算法应用）与大数据（基于Hadoop的分布式基础设施）混淆的倾向。要掌握前者，你必须具备一些统计学背景，并理解预测背后的数学。我认为你不可能在早期阶段同时掌握两者。但人们仍然尝试！而且他们在这两个方面都不会走得很远。因此，我在教学中的建议是专注于使用统计学（算法）来解决问题。值得阅读这篇深刻的文章 - [为什么大数据陷入困境？因为他们忘记了应用统计学](/2016/07/big-data-trouble-forgot-applied-statistics.html)。同时请记住，一些当今最著名的数据科学家，如[格雷戈里·皮亚泰斯基-夏皮罗](/gps.html)和[柯克·D·博恩](http://www.boozallen.com/careers/meet-our-people/kirk-borne)都有强大的数学/统计学背景。

1.  **关注处理大量数据的问题**：这不一定与大数据相同（因为它可能不是分布式数据处理）——但关注非常大的数据集可以让你获得更丰富的经验。即使这些数据集与未来你可能涉及的应用程序无直接关系，这也是有效的。换句话说，处理大数据集的经验本身就是有价值的。这也解释了为什么许多数据科学家喜欢[柯克·D·博恩](http://www.boozallen.com/careers/meet-our-people/kirk-borne)有NASA（或太空研究）的背景——因为数据的丰富性。此外，许多优秀的数据科学家来自天气预测背景（出于同样的原因）。在我们的业务中（物联网的数据科学），数据量很重要。你可以使用简单的Raspberry Pi/Arduino进行物联网分析，或者你可以分析整个航空公司或油井的数据。这两者完全不同。我最喜欢的一本数据科学书籍是[《统计学、数据挖掘与天文学中的机器学习：调查数据分析的实用Python指南》 Željko Ivezić, Andrew J. Connolly, Jacob T. VanderPlas & Alexander Gray](http://press.princeton.edu/titles/10159.html)。即使有550页，它也非常易读。但更重要的是，它处理[非常大数据集](http://www.astroml.org/user_guide/datasets.html)。

1.  **使用Zulu原则**：[Zulu原则](https://en.wikipedia.org/wiki/The_Zulu_Principle)基于成为某一主题明确且狭窄子集的专家的想法：我自己在物联网数据科学中使用过这个原则，即识别一个利基并在其上深入工作以展示专业知识。

1.  **系统思维 - 解决行业范围的问题**：与微小利基策略相反的是[系统思维方法](https://www.amazon.co.uk/gp/product/0471925632/ref=oh_aui_detailpage_o05_s00?ie=UTF8&psc=1)。这需要广泛的理解和经验，以识别行业中的相互联系或缺口。同样，我与 Jean Jacques Bernard 合作使用的策略是创建 [用于 IoT 分析的开放方法](http://www.opengardensblog.futuretext.com/archives/2016/07/a-methodology-for-solving-problems-with-datascience-for-internet-of-things.html)。

1.  **Kaggle 算法**：如果你关注 Kaggle 竞赛，它们往往使用一些算法多于其他算法——例如 [Xgboost](http://xgboost.readthedocs.io/en/latest/model.html)。最好从一开始就牢记这些，因为你可能很快就会使用这些算法。

1.  **SQL 和 NoSQL——关注 SQL 和 NoSQL**：使用 SQL 和 NoSQL 可以实现很多目标。查看这些步骤来开始学习 [SQL](/2016/06/seven-steps-mastering-sql-data-science.html) 和 [NoSQL](/2016/07/seven-steps-understanding-nosql-databases.html)。

1.  **垂直领域的分析知识**：这显而易见，但常被忽视。业务知识将变得越来越重要，在每个垂直领域都有特定的指标需要预测或分析。

1.  **人工智能和深度学习**：人工智能和深度学习在许多数据科学领域确实适用，应成为重点关注对象。但需谨慎，如[Yann Le Cun 的文章所示](https://m.facebook.com/yann.lecun/posts/10153426023477143)。

1.  **关注工具**：有许多工具可以使数据科学家的工作更轻松。在大多数情况下，它们有与编程语言如 R 的接口。我们在课程中使用的完整生命周期工具包括：[h2o.ai](http://www.h2o.ai/)、[dataiku](http://www.dataiku.com/)、[Tibco Spotfire](http://spotfire.tibco.com/) 和 [Pentaho](http://www.pentaho.com/)。

1.  **关注 R**：无论你对 R 与 Python 的争论持何种看法，R 在企业层面正获得越来越多的关注，例如针对企业的供应商。Oracle、Microsoft、HPE（Vertica）、SAP（Hana）、Hitachi（Pentaho）等。

1.  最后，关注新兴领域——物联网（IoT）仍然是一个新兴领域，还有数据科学。物联网具有独特的特点，如更强调时间序列、传感器融合等。即使在物联网内部，也会出现新的领域——例如，我们目前正在探索[用于 Apache NiFi 的 IoT 分析](https://www.zdnet.com/article/hortonworks-cto-on-apache-nifi-what-is-it-and-why-does-it-matter-to-iot/)。区块链也将是当前分析的新兴领域。

**总结**：

1.  不试图成为‘独角兽’可能会令人解脱。这可以帮助你专注于掌握数据科学的实际步骤。

1.  有许多可能的途径。如果你想成为数据科学家，理想情况下，你应该在某一领域取得卓越成就，同时对其他领域也有足够的了解。

1.  你必须知道编程——但如果你想从成为A类数据科学家开始，你不必成为狭义的专家。

1.  你必须进行实际构建——构建服务是不可替代的——但你需要考虑一下你到底想要构建什么，如上所述。

1.  理解并应用统计学。

1.  记住，数据科学家是一个通才角色。

1.  数据科学是一个快速扩展的领域，许多机会在纵向、新领域、新工具等方面仍待发掘。

1.  从A类数据科学家开始，随后进阶到B类（生产）可能会相对容易。

我在[物联网数据科学课程](http://www.opengardensblog.futuretext.com/archives/2016/06/data-science-for-internet-of-things-course-aug-sep-2016-now-in-its-fourth-batch.html)中探讨了许多这些想法。

**相关**：

+   [物联网关键术语解释](/2016/07/internet-of-things-key-terms-explained.html)

+   [35个物联网开源工具](/2016/07/open-source-tools-internet-things.html)

+   [强化学习与物联网](/2016/08/reinforcement-learning-internet-things.html)

### 更多相关话题

+   [I型错误和II型错误：有什么区别？](https://www.kdnuggets.com/2022/08/type-type-ii-errors-difference.html)

+   [成为出色数据科学家所需的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应该掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [建立一个稳固的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)

+   [使用管道编写干净的Python代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)
