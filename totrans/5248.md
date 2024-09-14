# 计算客户生命周期价值：SQL 示例

> 原文：[https://www.kdnuggets.com/2018/02/calculating-customer-lifetime-value-sql-example.html](https://www.kdnuggets.com/2018/02/calculating-customer-lifetime-value-sql-example.html)

![评论](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者：Luba Belokon，[Statsbot](https://statsbot.co/)**

![标题图片](../Images/6458b0318357d930e839206ca8a17523.png)

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业之路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT

* * *

*[Statsbot](https://statsbot.co/?utm_source=kdnuggets&utm_medium=article&utm_campaign=ltv&utm_content=predict_ltv)团队为不同客户和业务模型估算了LTV 592 次。我们在这篇文章和一个**[免费电子书](https://statsbot.co/ltv-ebook?utm_source=kdnuggets&utm_medium=article&utm_campaign=ltv&utm_content=predict_ltv)**中分享了我们的经验，介绍了如何在没有复杂统计模型的情况下使用SQL计算客户生命周期价值。*

客户生命周期价值（LTV）是指客户在其“生命周期”内，或者至少在他们与您关系中的一段时间里，会花费在您业务上的金额。这是一个重要的指标，能够帮助您了解可以花费多少资金来获取新客户。**例如，如果您的客户获取成本（CAC）为$150，而LTV为$600。**您将能够增加预算以吸引更多人，推动业务增长。CAC与LTV之间的平衡使您能够检查任何业务的市场生存能力。

估算LTV是一个预测指标，它依赖于基于过去模式的未来购买情况，并使您能够看到作为企业您面临的风险以及您可以花费多少来获取新客户。在个体层面，它还使您能够确定谁是您未来可能的高价值客户。

为了理解如何估算LTV，首先可以考虑在客户与我们关系结束时评估其生命周期价值。假设一个客户与我们保持12个月的关系，每月消费$50。

![](../Images/fd0ef86fe315668c269a328ce7c8c25e.png)

他们在整个生命周期内为我们的业务带来的收入为$50*12 = $600。简单！我们可以将*LTV 的基本定义*视为来自特定用户的支付总和。

同样的原则适用于群体。如果我们想要查看*群体的平均LTV*，我们可以查看总支出除以客户数量。当我们谈论*估算LTV*或预测LTV时，我们需要考虑客户与我们的停留时间。为了得到这一点，我们实际上是“倒退”来看：我们查看随着时间的推移我们失去多少客户，或*流失率*。

### 如何计算SaaS的LTV？

在群体层面，估算LTV的基本公式是这样的：

![](../Images/333440d57273d027f78f696cd2994000.png)

其中ARPU是每个用户的平均每月经常性收入，流失率是我们失去客户的速率（即留存率的倒数）。

这个基本公式可以通过假设得到：

***下个月收入 = (当前月收入) * (1 – 流失率)***

*注意：当我们为SaaS估算客户生命周期价值时，可以忽略毛利率，因为成本很小，不会影响结果的准确性。但当我们稍后在本文中为电商计算预测LTV时，我们会将COGS包括在公式中。*

上述LTV公式的主要限制是它假设流失是线性的，即：我们在会员服务的第一个月和第二个月之间失去客户的可能性与我们在之后很久失去客户的可能性是一样的。深入到预测LTV的本质，我们可以说它是一个[几何级数](https://en.wikipedia.org/wiki/Geometric_series)的和，线性流失看起来不像是一条直线（如许多关于LTV的文章所示）。

> 实际上，我们知道线性流失通常不是情况。

在一个灵活的订阅模式中，我们在一开始就会失去很多人，当他们在“试用”一个服务时，但是一旦他们和我们待了很长时间，他们就不太可能离开。

最终，这取决于客户和业务之间存在的合同类型：例如，年度续订，其中流失更线性，将导致LTV非常接近上述公式。

没有合同的服务可能会失去很高比例的新客户，但流失可能会减缓。

我们可以图形化地考虑这个概念：

![](../Images/a68f0a58f6846b5a598f564c1f69d50d.png)

如果群体的LTV是线下的面积，我们可以很清楚地看到我们失去客户的速度将非常显著地影响我们的LTV估算。因此，我们在进行计算时需要考虑这一点。然而，对于LTV的初步估算，使用最简单的公式是有意义的。在此之后，我们将增加复杂性。

### 使用SQL提取ARPU和流失率

为了做出最基本的LTV估算，我们需要查看我们的交易历史。然后，我们可以确定每个客户的平均收入以及我们查看的期间内的流失率。为了简化，我将查看过去一年。

你可以分两步计算ARPU：

```py
month_ARPU AS
(SELECT
     visit_month,
     Avg(revenue) AS ARPU 
FROM
     (SELECT
          Cust_id,
          Datediff(MONTH, ‘2010-01-01’, transaction_date) AS visit_month,
          Sum(transaction_size) AS revenue 
     FROM   transactions 
     WHERE  transaction_date &gt; Dateadd(‘year’, -1, CURRENT_DATE)
     GROUP BY
           1,
           2)
GROUP BY 1)
```

结果将如下所示：

![](../Images/d7ab31f06da8eac9ea118a48e8f9c0b7.png)

在上述情况下，这将给我们一个平均每月支出为**$987.33**。

计算流失率则复杂一些，因为我们需要计算从一个月到下一个月**未**返回的人的百分比，按他们第一次访问的月份将客户分组，然后检查他们在下个月是否返回。

> 问题在于，在事务性数据库中，我们的客户访问记录是分开行的，而不是全部在同一行上。

解决这个问题的方法是将事务性数据库与自身连接，以便我们可以在一行中看到客户的行为。

为了隔离那些流失的客户，我们将第1个月的访问数据与第2个月的访问数据按照cust_id左连接。第2个月访问数据中cust_id为null的行就是客户未返回的记录。

```py
WITH monthly_visits AS
(SELECT
     DISTINCT
     Datediff(month, ‘2010-01-01’, transaction_date) AS visit_month,
     cust_id 
FROM            transactions 
WHERE
transaction_date &gt; dateadd(‘year’, -1, current_date)),

(SELECT
avg(churn_rate)
FROM
     (SELECT
          current_month,
          Count(CASE
               WHEN cust_type='churn' THEN 1
               ELSE NULL
       END)/count(cust_id) AS churn_rate 
     FROM
          (SELECT
               past_month.visit_month + interval ‘1 month’ AS current_month,
               past_month.cust_id,
               CASE
                    WHEN this_month.cust_id IS NULL THEN 'churn'
                    ELSE 'retained'
               END AS cust_type 
          FROM
               monthly_visits past_month 
          LEFT JOIN monthly_visits this_month ON
                    this_month.cust_id=past_month.cust_id
                    AND this_month.visit_month=past_month.visit_month + interval ‘1 month’
          )data
     GROUP BY 1)
)
```

假设这给出了一个0.1的结果，仅为简单起见。

![](../Images/5b9c40480494ed2e9d18cda5f8b5df76.png)

因此，估算LTV是一个简单的计算：我们有每月ARPU和每月流失率，所以我们只需将一者除以另一者！

$987.33/0.1 = **$9873.3**

如前所述，这个公式有其局限性，主要是因为它做了一系列在现实中可能不成立的假设。主要的假设是留存率和流失率在*群体间和时间间*都是*稳定的*。

跨群体的稳定性意味着早期采用者的行为与晚期采用者类似，而跨时间的稳定性意味着客户在与你关系的开始时流失的可能性与例如两年后是一样的。根据这些假设的真实性，你可能需要下调LTV估算值。

**如果你想了解如何估算电子商务的LTV，针对群体和每个个体客户，请下载我们[免费的电子书](https://statsbot.co/ltv-ebook?utm_source=kdnuggets&utm_medium=article&utm_campaign=ltv&utm_content=predict_ltv)，内容涵盖了如何用SQL计算客户生命周期价值。**

**个人简介：[Luba Belokon](https://www.linkedin.com/in/luba-belokon-35078512b?utm_source=kdnuggets&utm_medium=post&utm_campaign=ebook)** 是Statsbot的内容专家。

[原文](https://statsbot.co/blog/calculating-customer-lifetime-value-sql-example/)。经许可转载。

**相关：**

+   [SQL客户留存分析指南](/2017/12/guide-customer-retention-analysis-sql.html)

+   [机器学习算法：选择哪一种解决你的问题](/2017/11/machine-learning-algorithms-choose-your-problem.html)

+   [集成学习提升机器学习结果](/2017/09/ensemble-learning-improve-machine-learning-results.html)

### 更多相关主题

+   [评估文档相似度计算方法](https://www.kdnuggets.com/evaluating-methods-for-calculating-document-similarity)

+   [键值数据库的解释](https://www.kdnuggets.com/2021/04/nosql-explained-understanding-key-value-databases.html)

+   [使用 Datawig，一个用于缺失值插补的 AWS 深度学习库](https://www.kdnuggets.com/2021/12/datawig-aws-deep-learning-library-missing-value-imputation.html)

+   [数据科学中的基础数学：奇异值分解的视觉介绍](https://www.kdnuggets.com/2022/06/essential-math-data-science-visual-introduction-singular-value-decomposition.html)

+   [机器学习未能为我的业务创造价值。为什么？](https://www.kdnuggets.com/2021/12/machine-learning-produce-value-business.html)

+   [通过第三名最佳在线数据科学硕士课程最大化你的价值](https://www.kdnuggets.com/2023/05/bay-path-maximize-value-online-masters-data-science.html)
