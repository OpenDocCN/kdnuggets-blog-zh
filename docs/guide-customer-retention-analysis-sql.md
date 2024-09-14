# 使用 SQL 进行客户留存分析的指南

> 原文：[https://www.kdnuggets.com/2017/12/guide-customer-retention-analysis-sql.html](https://www.kdnuggets.com/2017/12/guide-customer-retention-analysis-sql.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](/2017/12/guide-customer-retention-analysis-sql.html?page=2#comments)

**由 Luba Belokon，[Statsbot](https://statsbot.co/)**

无论你是销售食品、金融服务还是健身会员，新客户的成功招募只有在他们再次购买时才真正成功。反映这一点的指标称为**留存率**，我们使用的方法是客户留存分析。这是影响收入的关键指标之一。当客户留存率很低时，你将把所有的收入花费在营销上。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

同时，如果你能通过 SQL 和数据库以正确的方式进行计算，那么提升留存率也会变得更容易。在这篇文章中，我们将逐步指导你如何进行基本的客户留存分析，如何随着时间的推移建立客户留存，新的与现有客户的留存曲线，以及如何在不同群体中计算留存分析。

![](../Images/4dafe15272d5880e5a4895c828c73092.png)

### **基础客户留存曲线**

客户留存曲线对任何希望了解客户的业务至关重要，它将有助于解释其他事物，例如销售数据或营销活动的影响。它们是可视化客户与业务之间关键互动的简单方式，也就是说，客户在第一次访问后是否会回来——以及回来的速度。

构建客户留存曲线的第一步是识别在参考期内访问你业务的客户，这里我称之为 p1。选择的周期长度应该是合理的，并且反映预期的访问频率。

不同类型的业务对客户的回访率有不同的期望：

+   一家咖啡店可能选择每周访问一次的预期频率。

+   超市可能会选择更长的周期，比如2周或一个月。

在以下示例中，我将使用一个月，并假设我们在观察2016年1月访问的客户在接下来一年的留存情况。

如前所述，第一步是识别原始客户池：

```py
January_pool AS

(                
                SELECT DISTINCT cust_id

                FROM            dataset

                WHERE           month(transaction_date)=1

                AND             year(transaction_date)=2016)
```

然后，我们观察这些客户随着时间的推移的行为：例如，他们在剩下的年度里每个月返回的数量是多少？

```py
SELECT *Year*(transaction_date),
       *Month*(transaction_date),
       count (distinct cust_id) AS number

FROM dataset

WHERE year(transaction_date)=2016

AND cust_id IN january_pool

GROUP BY 1,

         2
```

正如你所见，原始的 SELECT 函数被包含在第二步中。

如果我们在一月份有1000名独特客户，我们可以预期我们的结果会是这样的：

![](../Images/9e5c1c04cff6ee179ae47d2e59425704.png)

结果图将类似于这样：

![](../Images/8e8edcb227c54109061bdea06175072b.png)

数据可视化使用 [Statsbot](https://statsbot.co/solutions/product-analytics?utm_source=blog&utm_medium=article&utm_campaign=retention)

### **客户保留的演变**

上述内容显然只是第一步，因为我们还希望查看客户保留是否有任何趋势，即我们是否在变得更好？

因此，我们可能会有一个想法：在一月份来访的客户中，有多少人在二月份返回？在二月份来访的客户中，有多少人在三月份返回？以及其他一个月的间隔。

因此，我们需要建立一个迭代模型，这可以通过几个简单的步骤完成。首先，我们需要创建一个表格，记录每个用户按月的访问情况，并允许这些访问可能跨越多个年份，因为这些访问可能发生在我们的业务开始运营后的任何时间。我在这里假设开始日期是2000年，但你可以根据需要调整。

```py
Visit_log AS

SELECT cust_id,

       datediff(month, ‘2000–01–01’, transaction_date) AS visit_month

FROM dataset

GROUP BY 1,

         2

ORDER BY 1,

         2
```

这将给我们一个类似于这样的视图：

![](../Images/b3facb2b4c2007e2a35a9a0ad14377b5.png)

然后，我们需要重新整理这些信息，以确定每次访问之间的时间间隔。因此，对于每个人和每个月，查看下次访问的时间。

```py
Time_lapse AS

    SELECT cust_id,

           visit_month lead(visit_month, 1) over (partition BY cust_id ORDER BY cust_id, visit_month)

    FROM visit_log
```

![](../Images/f711558056104b3bd5419449a25b0abb.png)

接下来，我们需要计算访问之间的时间间隔：

```py
Time_diff_calculated AS

    SELECT cust_id,

           visit_month,

           lead,

           lead — visit_month AS time_diff

    FROM time_lapse
```

![](../Images/3d0161fb4389e0fa881503732e980adf.png)

现在，让我们回顾一下客户保留分析所测量的内容：它是指在某段时间（x 延迟）后返回的客户比例。因此，我们要做的是比较在某个月访问的客户数量与这些客户在下个月返回的数量。我们还需要定义那些在某段时间后返回的客户，以及那些完全不返回的客户。为了做到这一点，我们需要根据他们的访问模式对客户进行分类。

```py
Custs_categorized AS

SELECT cust_id,

       visit_month,

       CASE

             WHEN time_diff=1 THEN ‘retained’,

             WHEN time_diff>1 THEN ‘lagger’,

             WHEN time_diff IS NULL THEN ‘lost’

       END AS cust_type

FROM time_diff_calculated
```

这将使我们在最后一步能够统计出在某个月访问的客户数量，以及其中多少人在下个月返回。

```py
SELECT visit_month,

       count(cust_id where cust_type=’retained’)/count(cust_id) AS retention

FROM custs_categorized

GROUP BY 1
```

这为我们提供了每个月返回客户的比例。

![](../Images/92c95efb32cb152b198bfe7dbffaace8.png)

数据可视化使用 [Statsbot](https://statsbot.co/solutions/product-analytics?utm_source=blog&utm_medium=article&utm_campaign=retention)

### 更多相关话题

+   [如何收集客户情感分析的数据](https://www.kdnuggets.com/2022/12/collect-data-customer-sentiment-analysis.html)

+   [逐步指南：阅读和理解 SQL 查询](https://www.kdnuggets.com/a-step-by-step-guide-to-reading-and-understanding-sql-queries)

+   [深入解析 DENSE_RANK()：SQL 爱好者的逐步指南](https://www.kdnuggets.com/breaking-down-denserank-a-step-by-step-guide-for-sql-enthusiasts)

+   [SQL 执行顺序的必备指南](https://www.kdnuggets.com/the-essential-guide-to-sql-execution-order)

+   [SQL 中的数据清理：如何为分析准备混乱的数据](https://www.kdnuggets.com/data-cleaning-in-sql-how-to-prepare-messy-data-for-analysis)

+   [掌握 SQL、Python、数据清理、数据处理和探索性数据分析的指南合集](https://www.kdnuggets.com/collection-of-guides-on-mastering-sql-python-data-cleaning-data-wrangling-and-exploratory-data-analysis)
�等构建群体。

### **最终思考**

客户保留分析将为任何业务分析增添深度，并允许决策者跟踪他们的招聘策略的成功情况，以及在客户体验方面的表现。如果您的客户没有回头，那么需要改进的地方可能在于产品质量或与客户的关系。保留分析便于轻松标记此类问题。

**简历：[卢巴·贝洛孔](https://www.linkedin.com/in/luba-belokon-35078512b/)** 是 Statsbot 的内容专家。

[原文](https://blog.statsbot.co/customer-retention-analysis-93af9daee46b?utm_source=kdnuggets&utm_medium=post&utm_campaign=sql)。已获许可转载。

**相关：**

+   [机器学习算法：如何选择适合您问题的算法](/2017/11/machine-learning-algorithms-choose-your-problem.html)

+   [集成学习以提高机器学习结果](/2017/09/ensemble-learning-improve-machine-learning-results.html)

+   [使用递归神经网络（LSTM）的时间序列预测指南](/2017/10/guide-time-series-prediction-recurrent-neural-networks-lstms.html)

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT 需求

* * *

### 更多相关话题

+   [如何收集客户情感分析的数据](https://www.kdnuggets.com/2022/12/collect-data-customer-sentiment-analysis.html)

+   [逐步指南：阅读和理解 SQL 查询](https://www.kdnuggets.com/a-step-by-step-guide-to-reading-and-understanding-sql-queries)

+   [拆解 DENSE_RANK()：SQL 爱好者的逐步指南](https://www.kdnuggets.com/breaking-down-denserank-a-step-by-step-guide-for-sql-enthusiasts)

+   [SQL 执行顺序的必备指南](https://www.kdnuggets.com/the-essential-guide-to-sql-execution-order)

+   [SQL 数据清洗：如何准备混乱的数据进行分析](https://www.kdnuggets.com/data-cleaning-in-sql-how-to-prepare-messy-data-for-analysis)

+   [掌握 SQL、Python、数据清洗、数据…的指南合集](https://www.kdnuggets.com/collection-of-guides-on-mastering-sql-python-data-cleaning-data-wrangling-and-exploratory-data-analysis)
