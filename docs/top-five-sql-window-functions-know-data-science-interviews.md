# 数据科学面试中你应知道的五大 SQL 窗口函数

> 原文：[`www.kdnuggets.com/2022/01/top-five-sql-window-functions-know-data-science-interviews.html`](https://www.kdnuggets.com/2022/01/top-five-sql-window-functions-know-data-science-interviews.html)

![数据科学面试中你应知道的五大 SQL 窗口函数](img/de892b475bedeefa0235111dd9749785.png)

SQL 是数据世界中的通用语言，是数据专业人士必须掌握的最重要的技能。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

SQL 之所以如此重要，是因为它是数据处理阶段所需的主要技能。许多数据探索、数据操作、管道开发和仪表板创建都是通过 SQL 完成的。

将优秀的数据科学家与普通的数据科学家区分开来的是，优秀的数据科学家可以充分利用 SQL 的能力来处理数据。完全使用 SQL 的一个重要部分是了解如何使用窗口函数。

说到这里，让我们深入了解一下！

## 1\. 使用 LEAD()和 LAG()计算增量

LEAD()和 LAG()通常用于比较某一度量在某个时间段与前一个时间段的差异。举几个例子…

+   你可以获取每年销售额与前一年销售额之间的增量

+   你可以获取每月注册/转化/网站访问量的增量

+   你可以按月比较用户流失情况

**示例：**

以下查询展示了如何查询成本的月度百分比变化

```py
with monthly_costs as (
    SELECT
        date
      , monthlycosts
      , LEAD(monthlycosts) OVER (ORDER BY date) as
        previousCosts
    FROM
        costs
)SELECT
    date
  , (monthlycosts - previousCosts) / previousCosts * 100 AS
    costPercentChange
FROM monthly_costs
```

## 2\. 使用 SUM()或 COUNT()计算累积和

通过以 SUM()或 COUNT()开头的窗口函数可以简单地计算运行总和。这是一个强大的工具，当你想展示某个特定指标随时间的增长时尤为有效。更具体地说，它在以下情况下非常有用：

+   获取随时间变化的收入和成本的运行总和

+   获取每个用户在应用上花费的时间的运行总和

+   获取随时间变化的转化运行总和

**示例：**

以下示例展示了如何包含月度成本的累积和列：

```py
SELECT
    date
  , monthlycosts
  , SUM(monthlycosts) OVER (ORDER BY date) as cumCosts
FROM
    cost_table
```

## 3\. 使用 AVG()进行移动平均

AVG()在窗口函数中非常强大，因为它允许你计算[移动平均](https://en.wikipedia.org/wiki/Moving_average)。

移动平均线是一种简单但有效的短期值预测方法。它们在平滑图表上的波动曲线时也极为有用。通常，移动平均线用于衡量事物的发展方向。

更具体地说……

+   它们可以用来获取每周销售的总体趋势（平均水平是否在上升？）。这将表明公司的增长。

+   它们同样可以用来获取每周转换或网站访问的总体趋势。

**示例：**

以下查询是获取 10 天移动平均转换率的示例。

```py
SELECT
    Date
  , dailyConversions
  , AVG(dailyConversions) OVER (ORDER BY Date ROWS 10 PRECEDING) AS
    10_dayMovingAverage
FROM
    conversions
```

## 4\. ROW_NUMBER()

ROW_NUMBER()在你想要获取首条或最后一条记录时特别有用。例如，如果你有一个记录健身会员到访时间的表格，并且你想获取他们第一次到健身房的日期，你可以通过 PARTITION BY customer（name/id）和 ORDER BY purchase date。然后，为了获取第一行，你只需筛选出 rowNumber 等于一的行。

**示例：**

这个例子展示了如何使用 ROW_NUMBER()获取每个会员（用户）访问的首次日期。

```py
with numbered_visits as (
    SELECT
        memberId
      , visitDate
      , ROW_NUMBER() OVER (PARTITION BY customerId ORDER BY
        purchaseDate) as rowNumber
    FROM
        gym_visits
)SELECT
    *
FROM
    numbered_visits
WHERE 
    rowNumber = 1
```

总结一下，如果你需要获取第一条或最后一条记录，ROW_NUMBER()是实现这一点的好方法。

## 5\. 使用 DENSE_RANK() 进行记录排名

DENSE_RANK()类似于 ROW_NUMBER()，不同之处在于它对相等值返回相同的排名。稠密排名在检索顶级记录时非常有用，例如：

+   如果你想拉取本周观看次数最多的前 10 个 Netflix 节目

+   如果你想基于消费金额获取前 100 个用户

+   如果你想查看 1000 个最不活跃用户的行为

**示例：**

如果你想按总销售额对你的主要客户进行排名，DENSE_RANK()将是一个合适的函数。

```py
SELECT
    customerId
  , totalSales
  , DENSE_RANK() OVER (ORDER BY totalSales DESC) as rank
FROM
    customers
```

## 感谢阅读！

就这些了！希望这些对你的面试准备有所帮助——我相信，如果你能完全掌握这 5 个概念，你在大多数 SQL 窗口函数问题中会表现出色。

一如既往，我祝你在学习过程中一切顺利！

**[Terence Shin](https://www.linkedin.com/in/terenceshin/)** 是一位数据爱好者，拥有超过 3 年的 SQL 经验和超过 2 年的 Python 经验，并且是 Towards Data Science 和 KDnuggets 的博客作者。

[原文](https://towardsdatascience.com/top-five-sql-window-functions-you-should-know-for-data-science-interviews-b8b334af437)。经许可转载。

### 更多相关主题

+   [KDnuggets™ 新闻 22:n03，1 月 19 日：深入分析 13 个数据…](https://www.kdnuggets.com/2022/n03.html)

+   [SQL 窗口函数](https://www.kdnuggets.com/2022/04/sql-window-functions.html)

+   [你应该知道的 7 个 SQL 概念](https://www.kdnuggets.com/2022/11/7-sql-concepts-needed-data-science.html)

+   [每个数据科学家都应该知道的 10 个 Pandas 函数](https://www.kdnuggets.com/10-essential-pandas-functions-every-data-scientist-should-know)

+   [KDnuggets 新闻，4 月 13 日：数据科学家应了解的 Python 库…](https://www.kdnuggets.com/2022/n15.html)

+   [SQL 面试准备材料资源](https://www.kdnuggets.com/2023/02/sql-interviews-preparations-material-resources.html)
