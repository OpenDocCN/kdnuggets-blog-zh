# 经验丰富的 SQL 面试问题

> 原文：[https://www.kdnuggets.com/2022/01/sql-interview-questions-experienced-professionals.html](https://www.kdnuggets.com/2022/01/sql-interview-questions-experienced-professionals.html)

![SQL 经验丰富的专业人士面试问题](../Images/f143ab433b355c0aa77f2582cbd24965.png)

## 介绍

如果你是一个经验丰富的数据科学家正在寻找工作，那么现在是最佳时机。目前，许多知名组织正在寻找对数据科学领域了如指掌的数据科学家。然而，高需求并不意味着你可以或应该轻松申请高级职位而没有某些技能。公司在招聘经验丰富的数据科学家时，期望他们能处理最困难的任务。这些员工应该对即使是最晦涩的功能也有良好的掌握，以便在必要时使用它们。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 工作

* * *

当面试高级职位时，经验丰富的数据科学家被问到更难的问题并不奇怪。通常，在同一职位上工作几年后，数据科学家会非常擅长执行某些重复性任务。专业人士需要意识到 SQL 不仅仅停留在现有的知识上。在高级 SQL 概念方面，他们的知识可能仍有一些空白。因此，寻求帮助以便在数据科学家面试中成功并没有坏处。

SQL 是管理数据库的主要语言，因此执行 SQL 操作是数据科学家工作的重要部分。大多数数据科学家面试都安排以确定候选人对 SQL 的知识。

日常工作中可能不包括编写复杂查询，但你必须展示当需要这些技能时，你是能够做到的。因此，面试官询问各种 [SQL 面试问题](https://www.stratascratch.com/blog/sql-interview-questions-you-must-prepare-the-ultimate-guide/?utm_source=kdn&utm_medium=click&utm_campaign=kdn) 来测试候选人的 SQL 流利程度也就不足为奇了。

在这篇文章中，我们总结了一些在面试中会问到的复杂问题和概念。即使你对自己的 SQL 知识充满信心，浏览这些关键词并确保你已经掌握所有内容也无妨。

## 经验丰富的专业人员的基本概念

### CASE / WHEN

彻底理解 CASE（及其伴随的 When 语句）的概念对于完全掌握 SQL 至关重要。CASE 语句允许我们检查某些条件，并根据这些条件是否为真或假返回一个值。结合 WHERE 和 ORDER BY 等子句，CASE 允许我们将逻辑、条件和顺序引入 SQL 查询中。

CASE 语句的价值不仅限于在查询中提供简单的条件逻辑。经验丰富的数据科学家应该对 CASE 语句及其用途有超过表面层次的理解。面试官可能会问你关于不同类型 CASE 表达式的问题以及如何编写它们。

经验丰富的候选人应该准备回答理论性问题，例如解释 Valued 和 Searched CASE 语句之间的区别，它们是如何工作的以及如何编写它们。这需要对它们的语法和常见实践有深入的理解。不用说，这也包括正确使用 ELSE 子句。

经验丰富的数据科学家需要知道如何使用 CASE 语句与聚合函数一起使用。你还可能会被要求编写简洁的 CASE 语句，这种语句重复性较少且更易于理解。你应该能够智能地讨论使用简洁 CASE 语句的注意事项和可能的风险。

一般来说，经验丰富的数据科学家必须能够使用 CASE 编写更高效的查询。毕竟，CASE 语句的整个目的是避免编写过多的单独查询来整合数据。

这里有一个可以使用 CASE / WHEN 语句解决的问题的示例：[https://platform.stratascratch.com/coding/9634-host-response-rates-with-cleaning-fees?python=](https://platform.stratascratch.com/coding/9634-host-response-rates-with-cleaning-fees?python=)

这是在 Airbnb 面试中提出的一个难题，候选人需要找出平均房东响应率、邮政编码及其对应的清洁费用。

在这种情况下，CASE/ WHEN 语句用于将结果格式化为数字并将其表示为百分比值，此外还包括邮政编码。

### SQL 连接

对 SQL 连接的知识感到自信是很容易的，但你越是深入探讨这个话题，就会发现自己不知道的东西越多。面试官经常会问一些关于 SQL 连接高级方面的[面试问题](https://www.stratascratch.com/blog/sql-join-interview-questions/?utm_source=kdn&utm_medium=click&utm_campaign=kdn)，这些问题常常被忽视。因此，深入研究这个概念并彻底掌握它是很重要的。

除了基本概念，面试官可能会询问什么是自交叉连接，并通过要求解决实际问题来了解你的知识深度。你应该知道所有不同类型的连接，包括更复杂的类型，如哈希连接或复合连接。你也可能会被要求解释自然连接是什么，以及它们在什么情况下最有用。有时你需要解释自然连接和内连接之间的区别。

一般来说，你应该对将连接与其他语句结合使用以实现期望结果有深入的经验和掌握。例如，你应该知道如何使用 WHERE 子句将 Cross Join 当作 Inner Join 来使用。你还需要知道如何使用连接来生成新表，而不会对服务器施加过大压力。或者如何使用外连接来识别和填补查询数据库时的缺失值。或者外连接的内部工作原理，例如重新排列其顺序可能会改变输出。

[这是一个涉及编写内连接语句的问题的示例](https://platform.stratascratch.com/coding/9899-percentage-of-total-spend?python)。

这是一个相当困难的问题，要求候选人显示订单大小占总支出的百分比。

## 高级概念 N1：日期时间处理

![针对有经验专业人士的 SQL 面试问题](../Images/ee6b088b74a74d5acb0f42a990897272.png)

数据库中包含日期和时间是很常见的，因此任何有经验的数据科学家都应该对如何处理这些数据有深入的了解。这种数据使我们能够跟踪事件发生的顺序、频率变化、计算时间间隔，并获得其他重要见解。很多时候，执行这些操作需要完全掌握 SQL 中的日期时间处理。因此，拥有这些技能的专业人士将在竞争中占据优势。如果你对自己的技能没有 100% 的信心，可以查看下面描述的概念，看看有多少个听起来很熟悉。

由于在 SQL 中有许多不同（但有效）的方法来格式化数据，优秀的编码人员应该至少熟悉它们。面试中，招聘经理期望了解基本的数据格式化概念以及能够聪明地讨论选择合适函数的能力。这包括了解重要的 FORMAT() 函数及其相关语法，以充分利用该函数。还期望了解其他基本函数，例如 NOW()。此外，有经验的专业人士也可能会被问及基本概念，如时间序列数据及其目的。

同样，考虑你申请的职位的背景也很重要。一个 AI 或物联网公司可能更关注从传感器收集的数据，而一个股票交易应用可能需要你追踪日、周或月内的价格波动。

在某些情况下，雇主可能会询问关于 SQL 的更高级的日期/时间函数，例如 CAST()、EXTRACT() 或 DATE_TRUNC()。这些函数在处理包含日期的大量数据时非常有用。经验丰富的数据科学家应该了解每个函数的目的及其应用。在理想情况下，他或她应该有使用这些函数的经验。

SQL 中最复杂的日期时间操作将涉及基本函数和高级函数的组合。因此，了解所有这些函数是必要的，从更基础的 FORMAT()、NOW()、CURRENT_DATE 和 CURRENT_TIME 开始，包括上述的更高级函数。作为一名经验丰富的数据科学家，你还应该知道 INTERVAL 的作用及其使用时机。

[这里有一个在 Airbnb 面试中提出的问题示例](https://platform.stratascratch.com/coding/9637-growth-of-airbnb?python)，要求候选人利用可用数据追踪 Airbnb 的增长情况。

### 前提：

在这个问题中，要求候选人根据每年注册的房东数量变化来追踪 Airbnb 的增长。换句话说，我们将使用每年新增注册的房东数量作为增长的指标。我们通过计算上一年与当前年房东数量之间的差异，并将该数字除以前一年注册的房东数量来找出增长率。然后，我们通过将结果乘以 100 来找出百分比值。

输出表格应包含当前年、上一年房东数量以及逐年增长百分比的列。百分比必须四舍五入到最接近的整数，行必须按年份升序排列。

### 解决方案：

要回答这个问题，候选人必须使用名为 ‘airbnb_search_details’ 的表格，该表格包含许多列。我们需要的列标记为 ‘host_since’，它表示房东首次注册网站的年份、月份和日期。对于这个练习，月份和日期无关紧要，因此我们需要做的第一件事是从值中提取年份。然后，我们必须创建一个视图，其中包含当前年、上一年以及该年的总房东数量的单独列。

```py
Select 
      extract(year FROM host_since::DATE)
FROM airbnb_search_details
WHERE host_since IS NOT NULL
```

到目前为止，我们已经完成了两件事：

1.  我们确保只包含 host_since 列不为空的行。

1.  我们从数据中提取年份并将其转换为 DATE 值。

```py
Select 
      extract(year FROM host_since::DATE)
      count(id) as current_year_host
FROM airbnb_search_details
WHERE host_since IS NOT NULL
GROUP BY extract(year FROM host_since::DATE)
ORDER BY year asc
```

然后我们继续计算 ID，并为每年设置 GROUP BY 子句，并按升序显示。

这将为我们提供一个包含两列的表格：年份和注册该年的主机数量。我们仍然没有解决问题所需的完整信息，但这是朝着正确方向的一步。我们还需要针对前一年注册的主机的单独列。这就是 LAG() 函数的作用。

```py
SELECT 
      Year,
      current_year_host,
      LAG(current_year_host, 1) OVER (ORDER BY year) as prev_year_host
Select 
      extract(year FROM host_since::DATE)
      count(id) as current_year_host
FROM airbnb_search_details
WHERE host_since IS NOT NULL
GROUP BY extract(year FROM host_since::DATE)
ORDER BY year asc
```

在这里，我们添加了第三列，标记为“prev_year_host”，其值将来自“current_year_host”，但延迟一行。这可能是这样的：

![SQL 面试问题针对有经验的专业人士](../Images/650e8090ee481469b8c536cc39809fdd.png)

以这种方式排列表格可以非常方便地计算最终的增长率。我们为方程中的每个值都有一个单独的列。最终，我们的代码应该类似于以下内容：

```py
SELECT year,
       current_year_host,
       prev_year_host,
       round(((current_year_host - prev_year_host)/(cast(prev_year_host AS numeric)))*100) estimated_growth
FROM
  (SELECT year,
          current_year_host,
          LAG(current_year_host, 1) OVER (ORDER BY year) AS prev_year_host
   FROM
     (SELECT extract(year
                     FROM host_since::date) AS year,
             count(id) current_year_host
      FROM airbnb_search_details
      WHERE host_since IS NOT NULL
      GROUP BY extract(year
                       FROM host_since::date)
      ORDER BY year) t1) t2
```

在这里，我们添加了另一个查询和另一个列来计算增长率。我们必须将初始结果乘以 100 并四舍五入，以满足任务的要求。

这就是解决此任务的方法。很明显，日期时间处理函数对于完成任务至关重要。

## 高级概念 N2：窗口函数和分区

![SQL 面试问题针对有经验的专业人士](../Images/e112a727a805ff5dc22f561f0f0a1d28.png)

[SQL 窗口函数](https://www.stratascratch.com/blog/the-ultimate-guide-to-sql-window-functions/?utm_source=kdn&utm_medium=click&utm_campaign=kdn)是编写复杂而高效的 SQL 查询的最重要概念之一。经验丰富的专业人士应对窗口函数有深入的实践和理论知识。这包括了解 OVER 子句是什么并掌握其使用。面试官可能会问 OVER 子句如何将聚合函数转换为窗口函数。你还可能被问到可以作为窗口函数使用的三种聚合函数。经验丰富的数据科学家还应了解其他非聚合的窗口函数。

为了最好地利用窗口函数，必须了解 PARTITION BY 子句是什么以及如何使用它。你可能会被要求解释它并提供一些使用案例的示例。有时，你需要使用 ORDER_BY 子句在分区内组织行。

能够展示每一个窗口函数的全面知识的候选人，如 ROW_NUMBER()，将具有优势。不用说，单靠理论知识是不够的——专业人士还应具备实际使用这些函数的经验，无论是否涉及分区。例如，一位经验丰富的专业人士应该能够解释 RANK() 和 DENSE_RANK() 之间的区别。理想的候选人应了解一些最先进的概念，如分区中的帧，并能够清晰地解释它们。

优秀的候选人还应解释 NTH_VALUE() 函数的用法。提到该函数的替代方案，如 FIRST_VALUE() 和 LAST_VALUE() 函数，也不会有坏处。公司通常喜欢衡量四分位数、分位数和百分位数。执行此操作时，数据科学家还必须知道如何使用 NTILE() 窗口函数。

在 SQL 中，通常有许多方法来处理任务。然而，窗口函数提供了执行常见但复杂操作的最简单方法。一个好的窗口函数例子是 LAG() 或 LEAD()，因此你也应该熟悉它们。例如，我们来看看之前解决的一个难度较大的 Airbnb 面试题：

为了显示前一年主机的数量，我们使用了带有 OVER 语句的 LAG() 函数。这可以通过许多其他方式完成，但窗口函数允许我们只用一行 SQL 代码就能获得所需的结果：

```py
LAG(current_year_host, 1) OVER (ORDER BY year) as prev_year_host
```

许多公司需要计算某一时间段的增长。LAG() 函数在完成这种任务时可以非常有用。

## 高级概念 N3：月度增长

![SQL 面试问题（经验丰富的专业人士）](../Images/881f00abd3dff325f9da9cbac80bddd1.png)

许多组织使用数据分析来衡量自己的绩效。这可能涉及衡量营销活动的效果或特定投资的 ROI。执行这种分析需要深入了解 SQL，例如日期、时间和窗口函数。

数据科学家还必须证明他们在格式化数据和以百分比或其他形式显示数据的技能。通常，要解决涉及计算月度增长的实际问题，你必须使用多种技能的组合。一些所需的概念将是高级的（窗口函数、日期时间操作），而其他的则是基础的（聚合函数和常见 SQL 语句）。

我们来看看亚马逊面试官提出的一个例题。

### 前提：

在这个问题中，我们必须处理一个购买表并计算月度收入的增长或下降。最终结果必须以特定的方式格式化（YYYY-MM 格式），并且百分比应四舍五入到第二位小数。

### 解决方案：

在处理类似任务时，首先需要了解表的结构。你还应该确定需要处理的列，以回答问题，并确定输出的形式。

在我们的例子中，数据值具有对象类型，因此我们需要使用 CAST() 函数将其转换为日期类型。

```py
SELECT
  to_char(cast(created_at as date), 'YYYY-MM')
FROM sf_transactions
```

问题还指定了日期的格式，因此我们可以在 SQL 中使用 TO_CHAR() 函数将日期输出为这种格式。

要计算增长，我们还应选择 created_at 和 SUM() 聚合函数以获取该日期的总销售量。

```py
SELECT
  to_char(cast(created_at as date), 'YYYY-MM'),
  created_at,
  sum(value)
FROM sf_transactions
```

这时，我们必须再次使用窗口函数。具体来说，我们将使用 LAG() 函数来访问上个月的数量，并将其显示为单独的列。为此，我们还需要一个 OVER 子句。

```py
SELECT
  to_char(cast(created_at as date), 'YYYY-MM') AS year_month,
  created_at,
  sum(value)
  lag(sum(value), 1) OVER (ORDER BY created_at::date)
FROM sf_transactions
GROUP BY created_at
```

根据我们目前编写的代码，我们的表格将如下所示：

![有经验的专业人士 SQL 面试问题](../Images/43282c12f6fe23a929718ebc47994b99.png)

在这里，我们有日期和总值的对应关系在 sum 列中，以及最后一个日期的值在 lag 列中。现在我们可以将这些值代入公式，并在单独的列中显示增长率。

我们还应该删除不必要的 created_at 列，并将 GROUP BY 和 ORDER BY 子句更改为 year_month。

```py
SELECT
  to_char(cast(created_at as date), 'YYYY-MM') AS year_month,
  sum(value),
  lag(sum(value), 1) OVER (ORDER BY to_char(cast(created_at as date))
FROM sf_transactions
GROUP BY year_month
```

一旦我们运行代码，我们的表格应该只包含对计算至关重要的列。

![有经验的专业人士 SQL 面试问题](../Images/43282c12f6fe23a929718ebc47994b99.png)

现在我们终于可以得到解决方案了。这是最终代码的样子：

```py
SELECT to_char(created_at::date, 'YYYY-MM') AS year_month,
       round(((sum(value) - lag(sum(value), 1) OVER w) / (lag(sum(value), 1) OVER w)) * 100, 2) AS revenue_diff_pct
FROM sf_transactions
GROUP BY year_month 
WINDOW w AS (
         ORDER BY to_char(created_at::date, 'YYYY-MM'))
ORDER BY year_month ASC
```

在这段代码中，我们取用之前示例中的两个列值，并计算它们之间的差异。注意，我们还利用了窗口别名来减少代码的重复性。

然后，根据算法，我们将其除以本月的收入，并乘以 100 以获得百分比值。最后，我们将百分比值四舍五入到小数点后两位。我们得出一个满足所有任务要求的答案。

## 高级概念 N4：流失率

尽管与增长相反，流失率也是一个重要的指标。许多公司都会追踪其流失率，尤其是当其商业模式是基于订阅时。通过这种方式，他们可以跟踪丢失的订阅或账户数量，并预测造成这些流失的原因。一位经验丰富的数据科学家应该知道使用哪些函数、语句和子句来计算流失率。

订阅数据非常私密，并包含用户的私人信息。数据科学家还需要知道如何在不暴露数据的情况下处理这些数据。计算流失率通常涉及公用表表达式（CTE），这是一种相对较新的概念。最优秀的数据科学家应该知道 CTE 的作用以及何时使用它们。当处理旧版数据库时，如果 CTE 不可用，理想的候选人仍然应该能够完成任务。

[这里是一个难度较大的任务示例](https://platform.stratascratch.com/coding/10017-year-over-year-churn?python)。在 Lyft 面试的候选人会收到这个任务，以计算公司司机的流失率。

为了解决这个问题，数据科学家必须使用 case/when 语句、窗口函数如 LAG()，以及 FROM / WHERE 和其他基本子句。

## 结论

拥有多年的数据科学家工作经历无疑在简历上显得非常出色，并且能为你赢得许多面试机会。然而，一旦你进入面试阶段，你仍然需要展示出足够的知识来补充多年经验的积累。即便你在[编写 SQL 查询](https://www.stratascratch.com/blog/best-practices-to-write-sql-queries-how-to-structure-your-code/?utm_source=kdn&utm_medium=click&utm_campaign=kdn)方面经验丰富，利用像[StrataScratch](https://www.stratascratch.com/?utm_source=kdn&utm_medium=click&utm_campaign=kdn)这样的资源来刷新你的知识也不失为一种好方法。

**[内特·罗西迪](https://www.stratascratch.com)** 是一名数据科学家，专注于产品战略。他还是一名兼职教授，教授分析学，并且是[StrataScratch](https://www.stratascratch.com/)，一个帮助数据科学家通过顶级公司实际面试问题准备面试的平台的创始人。你可以通过[Twitter: StrataScratch](https://twitter.com/StrataScratch) 或 [LinkedIn](https://www.linkedin.com/in/nathanrosidi/)与他联系。

### 更多相关话题

+   [你下一次面试可能会遇到的 24 道 SQL 问题](https://www.kdnuggets.com/2022/06/24-sql-questions-might-see-next-interview.html)

+   [数据科学家用的 25 道高级 SQL 面试问题](https://www.kdnuggets.com/2022/10/25-advanced-sql-interview-questions-data-scientists.html)

+   [你必须知道的前 10 个高级数据科学 SQL 面试问题……](https://www.kdnuggets.com/2023/01/top-10-advanced-data-science-sql-interview-questions-must-know-answer.html)

+   [数据科学中的 3 道 SQL 聚合函数面试问题](https://www.kdnuggets.com/2023/01/3-sql-aggregate-function-interview-questions-data-science.html)

+   [数据分析师的 SQL 和 Python 面试问题](https://www.kdnuggets.com/2023/02/sql-python-interview-questions-data-analysts.html)

+   [专业人士的 SQL 笔记：免费的电子书评测](https://www.kdnuggets.com/2022/05/sql-notes-professionals-free-ebook-review.html)
