# SQL 中最被低估的函数

> 原文：[`www.kdnuggets.com/2017/03/most-underutilized-function-sql.html`](https://www.kdnuggets.com/2017/03/most-underutilized-function-sql.html)

**Tristan Handy, Fishtown Analytics 的创始人和总裁。**

在过去的九个月里，我与十几家风险投资资助的初创公司合作，构建了他们的内部分析。在这个过程中，有一个 SQL 函数我意外地使用得非常频繁。起初我并不清楚为什么我要使用这个函数，但随着时间的推移，我发现它的用途越来越多。

* * *

## 我们的前 3 个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

![SQL](img/7b90faf67d8c5a9b93c5838add6004a0.png)

这是什么？ **md5()**。如果你不熟悉，这里有一个来自 Redshift 文档的示例代码片段：

```py
select md5('Amazon Redshift');
md5
----------------------------------
f7415e33f972c03abd4f3fed36748f7a
(1 row)

```

给 md5() 一个 varchar，它会返回其 [MD5 哈希](https://en.wikipedia.org/wiki/MD5)。简单……但看似无用。*我为什么要使用这个？！*

很好的问题。在这篇文章中，我将向你展示 md5() 的两个用法，使其成为我 SQL 工具包中最强大的工具之一。

### #1：构建唯一的 ID

我将做一个非常强烈的声明，但我真的相信这一点：**你仓库中的每一个** [**数据模型**](http://dbt.readthedocs.io/en/docs-0.6.0/guide/building-models/) **都应该有一个坚如磐石的唯一 ID**。

这种情况非常常见。一种原因是你的源数据没有唯一键——如果你通过 [Stitch](http://stitchdata.com/) 或 [Fivetran](http://fivetran.com/) 同步 Facebook Ads 的广告表现数据，你的 ad_insights 表中的源数据没有可以依赖的唯一键。相反，你有一个组合字段（在这种情况下是日期和 ad_id），它是可靠唯一的。利用这些信息，你可以使用 md5() 构建一个唯一 id。

```py
select 
    md5(date_start::varchar || ad_id::varchar) as insight_id
from 
    stitch_fb_ads.facebook_ads_insights
limit 5;
insight_id
----------------------------------
6d475ea96f23b097b51ed500116d8c5e     822c9429eabb28ccbcd7286836d7cd60     8b7fcd2aff879772ccac4f0f8bcb6a45     8a2cfd7eb1a723c49db47232e73ca29c     10338719dfadb3d4c9d44c608063998a
(5 rows)

```

结果哈希是一个无意义的字母数字字符串，作为记录的唯一标识符。当然，你可以创建一个单独的连接 varchar 字段来执行相同的功能，但 *实际上隐藏哈希背后的底层逻辑是重要的*：如果字段看起来像一个 ID 而不是像一堆人类可读的文本，你会本能地以不同的方式对待它。

创建唯一 ID 是一个重要实践的原因有几个：

1.  最常见的错误原因之一是分析师期望唯一的键中存在重复值。在该字段上的联接会以意想不到的方式“扩展”结果集，并可能导致难以排查的显著错误。为避免这种情况，只在你已经验证了基数并在必要时构建了唯一键的字段上进行联接。

1.  一些 BI 工具要求你必须有一个唯一键才能提供某些功能。例如，Looker 的[对称聚合](https://discourse.looker.com/t/symmetric-aggregates/261)需要一个唯一键才能正常工作。

我们为每个表创建唯一键，然后使用[dbt schema 测试](http://dbt.readthedocs.io/en/master/guide/testing/)测试该键的唯一性。我们每天多次在[Sinter](http://sinterdata.com/)上运行这些测试，并对任何失败情况进行通知。这使我们对在这些数据模型上实施的分析充满信心。

### #2：简化复杂的联接

这个案例在执行上类似于 #1，但解决了一个完全不同的问题。假设你有与之前提到的相同的 Facebook Ads 数据集，但这次你面临一个新的挑战：将该数据与网页分析会话表中的数据进行联接，以便计算 Facebook 的[ROAS](http://www.verticalrail.com/kb/calculate-roas/)。

在这种情况下，你可以使用的联接键是日期和你的[UTM 参数](https://en.wikipedia.org/wiki/UTM_parameters)（utm_medium、source、campaign 等）。看起来很简单，对吧？只需在所有 6 个字段上进行联接，然后就完成了。

不幸的是，这行不通，原因非常简单：这些字段中的某些子集为空是极其常见的，而空值无法与另一个空值联接。所以，那 6 个字段的联接是一条死胡同。你可以用大量的条件逻辑拼凑出一些非常复杂的东西，但那段代码非常丑陋且性能极差（我试过）。

相反，使用 md5()。在这两个数据集中，你可以将我们提到的 6 个字段连接成一个字符串，然后对整个字符串调用 md5()。下面是我们在一个客户项目中确切执行此操作的代码片段：

你可以看到这段代码实际上是在更多字段上构建 ID：在这个例子中，我们实际上是将来自 7 个不同广告渠道的广告支出数据进行联合，而 Bing 和 Adwords 的数据通过 ad_group_id 和 keyword_id 而不是 UTM 参数进行标识。这种方法干净地扩展。

在会话表中，你接着创建完全相同的哈希 ID 字段。**结果联接简单、可读，并且易于用于下游分析**：

### 资源

有兴趣自己实现类似的功能吗？以下是一些资源：

+   [dbt](https://github.com/fishtown-analytics/dbt)

    （我们构建和使用的开源工具，用于进行所有数据建模）

+   [Facebook 广告代码](https://github.com/fishtown-analytics/facebook-ads)

    （我们的开源 Facebook 广告 dbt 包）

感谢阅读！我非常好奇是否有人对`md5()`有其他巧妙的用法。

**个人简介：[Tristan Handy](https://twitter.com/jthandy)** 是 [**Fishtown Analytics**](http://fishtownanalytics.com/) 的创始人兼总裁。他为高级分析构建开源工具。

[原文](https://blog.fishtownanalytics.com/the-most-underutilized-function-in-sql-9279b536ed1a#.qb2x0foea)。经许可转载。

**相关内容：**

+   掌握数据科学 SQL 的 7 个步骤

+   让 Python 用 pandasql 说 SQL

+   用 SQL 做统计

### 更多相关主题

+   [数据科学中的 3 个 SQL 聚合函数面试问题](https://www.kdnuggets.com/2023/01/3-sql-aggregate-function-interview-questions-data-science.html)

+   [你应该了解的梯度下降和成本函数的 5 个概念](https://www.kdnuggets.com/2020/05/5-concepts-gradient-descent-cost-function.html)

+   [什么是函数？](https://www.kdnuggets.com/2022/11/function.html)

+   [Python 函数参数：权威指南](https://www.kdnuggets.com/2023/02/python-function-arguments-definitive-guide.html)

+   [多标签自然语言处理：类别不平衡和损失函数的分析…](https://www.kdnuggets.com/2023/03/multilabel-nlp-analysis-class-imbalance-loss-function-approaches.html)

+   [如何使用 pivot_table 函数进行高级数据汇总…](https://www.kdnuggets.com/how-to-use-the-pivot_table-function-for-advanced-data-summarization-in-pandas)
