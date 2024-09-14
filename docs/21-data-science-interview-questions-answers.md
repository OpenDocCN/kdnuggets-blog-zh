# 21个必须知道的数据科学面试问题及答案

> 原文：[https://www.kdnuggets.com/2016/02/21-data-science-interview-questions-answers.html/3](https://www.kdnuggets.com/2016/02/21-data-science-interview-questions-answers.html/3)

### 8\. 什么是统计功效？

由[Gregory Piatetsky](/author/gregory-piatetsky)回答：

维基百科定义了[统计功效或敏感性](https://en.wikipedia.org/wiki/Statistical_power)，即当备择假设（H1）为真时，二元假设检验正确拒绝零假设（H0）的概率。

换句话说，[统计功效](http://effectsizefaq.com/2010/05/31/what-is-statistical-power/)是研究在效应存在时检测到效应的可能性。统计功效越高，犯第二类错误（即当实际上存在效应时却得出没有效应的结论）的可能性就越小。

这里有一些工具可以[计算统计功效](https://www.dssresearch.com/KnowledgeCenter/toolkitcalculators/statisticalpowercalculators.aspx)。

### 9\. 解释什么是重采样方法及其作用。还要解释它们的局限性。

由[Gregory Piatetsky](/author/gregory-piatetsky)回答：

经典的统计参数检验将观察到的统计量与理论采样分布进行比较。重采样是一种数据驱动的方法，而非理论驱动的方法，基于在相同样本内重复采样。

重采样是指用于执行以下方法的一种方法

+   通过使用可用数据的子集（切割法）或从数据点集合中随机抽取（自助法）来估计样本统计量（中位数、方差、百分位数）的精确性。

+   在进行显著性测试时（置换测试，也称为精确测试、随机化测试或重新随机化测试），对数据点标签进行交换。

+   通过使用随机子集（自助法、交叉验证）来验证模型。

更多信息请参见维基百科上的[自助法](https://en.wikipedia.org/wiki/Bootstrapping_(statistics))和[切割法](https://en.wikipedia.org/wiki/Jackknife_(statistics))。

另请参见[如何使用自助法和Apache Spark检查假设](https://en.wikipedia.org/wiki/Bootstrapping_(statistics))

![自助法与Spark](../Images/dc7c46df22358d12bdd13546d79a8881.png)](/2016/01/hypothesis-testing-bootstrap-apache-spark.html)

这里有一个关于[重采样统计](http://userwww.sfsu.edu/efc/classes/biol710/boots/rs-boots.htm)的良好概述。

### 10\. 是出现较多假阳性好，还是较多假阴性好？请解释。

由[**Devendra Desale**](/author/devendra-desale)回答。

这取决于问题以及我们试图解决的问题的领域。

在医学测试中，假阴性可能会给患者和医生提供一种虚假的安慰信息，认为疾病不存在，实际上却是存在的。这有时会导致对患者及其疾病的不适当或不充分的治疗。因此，出现较多假阳性是更为理想的。

在垃圾邮件过滤中，假阳性发生在垃圾邮件过滤或阻挡技术错误地将合法邮件归类为垃圾邮件，从而干扰其投递。虽然大多数反垃圾邮件策略可以阻止或过滤高比例的垃圾邮件，但在不产生显著的假阳性结果的情况下进行这种操作要困难得多。因此，我们宁愿接受过多的假阴性，也不愿接受过多的假阳性。

### 11. 什么是选择偏差？它为什么重要？你如何避免它？

答案由[**马修·梅奥**](/author/matt-mayo)提供。

选择偏差通常是指由于样本非随机而引入的误差。例如，如果一个由100个测试用例组成的样本在实际人群中相对均等的四个类别中以60/20/15/5的比例分布，那么一个模型可能会错误地假设概率是决定性预测因素。避免非随机样本是应对偏差的最佳方法；然而，当这不切实际时，诸如[重采样](https://en.wikipedia.org/wiki/Resampling_(statistics))、[提升](https://en.wikipedia.org/wiki/Boosting_(machine_learning))和加权等技术是应对这种情况的策略。

这是[答案的第二部分](https://www.kdnuggets.com/2016/02/21-data-science-interview-questions-answers-part2.html)。

### 更多相关话题

+   [建立一个稳固的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)

+   [使用管道编写干净的Python代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [7个数据分析面试问题与答案](https://www.kdnuggets.com/2022/09/7-data-analytics-interview-questions-answers.html)

+   [5个Python面试问题与答案](https://www.kdnuggets.com/2022/09/5-python-interview-questions-answers.html)

+   [检测假数据科学家的20个问题（含答案）：ChatGPT…](https://www.kdnuggets.com/2023/01/20-questions-detect-fake-data-scientists-chatgpt-1.html)

+   [检测假数据科学家的20个问题（含答案）：ChatGPT…](https://www.kdnuggets.com/2023/02/20-questions-detect-fake-data-scientists-chatgpt-2.html)
通过反复询问“为什么？”来找到问题的根本原因并展示原因之间的关系，直到找到问题的根源。这种技术通常被称为“5个为什么”，尽管它可以涉及多于或少于5个问题。

![5 Whys](../Images/4acc58e7ea72ac641ad9a044ed8b7408.png)

**图 5 Whys 分析示例，来自 [根本原因分析艺术](http://asq.org/quality-progress/2015/02/back-to-basics/the-art-of-root-cause-analysis.html) 。**

### Q7. 你对价格优化、价格弹性、库存管理、竞争情报有了解吗？举例说明。

由[Gregory Piatetsky](/author/gregory-piatetsky)的回答：

这些是经济学术语，数据科学家不常被问到，但了解它们是有用的。

[价格优化](https://en.wikipedia.org/wiki/Price_optimization)是利用数学工具来确定客户如何通过不同渠道对不同价格的产品和服务作出反应。

大数据和数据挖掘使个性化价格优化成为可能。现在像亚马逊这样的公司甚至可以进一步优化，根据不同访问者的历史显示不同的价格，尽管是否公平仍存在强烈的争议。

常用的价格弹性通常指

+   [需求价格弹性](https://en.wikipedia.org/wiki/Price_elasticity_of_demand)，即价格敏感度的测量。它的计算方法是：

    价格需求弹性 = 需求量变化百分比 / 价格变化百分比。

类似地，[供给价格弹性](https://en.wikipedia.org/wiki/Price_elasticity_of_supply)是一个经济学指标，显示了商品或服务的供给量如何响应价格变化。

[库存管理](http://www.investopedia.com/terms/i/inventory-management.asp)是对公司用于生产销售商品的组件的订购、储存和使用进行监督和控制，以及对销售的成品数量进行监督和控制。

维基百科定义

> [竞争情报](https://en.wikipedia.org/wiki/Competitive_intelligence)：定义、收集、分析和分发关于产品、客户、竞争对手及环境的情报，以支持高管和管理人员为组织做出战略决策。

工具如Google Trends、Alexa、Compete可以用来确定一般趋势并分析你在网络上的竞争对手。

这里是有用的资源：

+   [竞争情报指标、报告](http://www.kaushik.net/avinash/competitive-intelligence-analysis-tools-metrics-reports-techniques/)由Avinash Kaushik提供

+   [37个最佳营销工具来监视你的竞争对手](https://blog.kissmetrics.com/james-bond-of-the-web/)来自Kissmetrics

+   [10个最佳竞争情报工具，来自10位专家](http://barnraisersllc.com/2014/10/10-best-competitive-intelligence-tools-10-experts/)

### 更多相关主题

+   [建立一个强大的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)

+   [使用管道编写干净的Python代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [7 个数据分析面试问题及答案](https://www.kdnuggets.com/2022/09/7-data-analytics-interview-questions-answers.html)

+   [5 个 Python 面试问题及答案](https://www.kdnuggets.com/2022/09/5-python-interview-questions-answers.html)

+   [识别虚假数据科学家的 20 个问题（及答案）：ChatGPT…](https://www.kdnuggets.com/2023/01/20-questions-detect-fake-data-scientists-chatgpt-1.html)

+   [识别虚假数据科学家的 20 个问题（及答案）：ChatGPT…](https://www.kdnuggets.com/2023/02/20-questions-detect-fake-data-scientists-chatgpt-2.html)
