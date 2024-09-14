# 逐步解析 DENSE_RANK()：SQL 爱好者的指南

> 原文：[https://www.kdnuggets.com/breaking-down-denserank-a-step-by-step-guide-for-sql-enthusiasts](https://www.kdnuggets.com/breaking-down-denserank-a-step-by-step-guide-for-sql-enthusiasts)

![逐步解析 DENSE_RANK()：SQL 爱好者的指南](../Images/2c67fa689690681dbae6745c0c6bb474.png)

图片由编辑提供

在当今数据驱动的世界中，SQL（结构化查询语言）是管理和操作数据库系统的基石。SQL的强大和灵活性的核心组成部分之一是其[窗口函数](/2017/12/sql-window-functions-tutorial-business-analytics.html/2)，这是一类在与当前行相关的行集上执行计算的函数。

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

想象一下你通过一个滑动窗口查看数据，并根据这个窗口的位置和大小，对数据进行计算或转换。这本质上就是 SQL 窗口函数的功能。它们处理如计算运行总计、平均值或排名等任务，这些任务使用标准 SQL 命令很难完成。

在窗口函数工具箱中，最强大的工具之一是排名函数，特别是 `DENSE_RANK()` 函数。这个函数对数据分析师来说是一个福音，它允许我们对不同的行进行排名而不产生任何间隙。无论你是在分析销售数据、网站流量数据，还是一个简单的学生考试分数列表，`DENSE_RANK()` 都是不可或缺的。

在这篇文章中，我们将深入探讨 `DENSE_RANK()` 的内部工作原理，将其与紧密相关的 `RANK()` 和 `ROW_NUMBER()` 进行对比，并展示如何避免在 SQL 旅程中可能遇到的常见陷阱。准备好提升你的数据分析技能了吗？那就开始吧。

# 理解 SQL 中排名函数的作用

SQL中的排名函数是窗口函数的一个子集，为结果集中的每一行分配一个唯一的排名。这些排名值对应于通过函数中的 ORDER BY 子句确定的特定顺序。排名函数是SQL的主要组成部分，在数据分析中被广泛使用，用于各种任务，如查找最佳销售员、识别表现最好的网页，或确定特定年份的最高票房电影。

SQL 中有三种主要的排名函数，即 `RANK()`、`ROW_NUMBER()` 和 `DENSE_RANK()`。这些函数的操作方式略有不同，但它们的共同目的都是根据指定的条件对数据进行排名。`RANK()` 和 `DENSE_RANK()` 函数的行为类似，即对相同值的行分配相同的排名。关键的区别在于它们处理随后的排名方式。`RANK()` 跳过下一个排名，而 `DENSE_RANK()` 不会。

另一方面，[`ROW_NUMBER()`](/sql-group-by-and-partition-by-scenarios-when-and-how-to-combine-data-in-data-science) 函数为每一行分配唯一的行号，无论排序列的值是否相同。虽然 `RANK()`、`DENSE_RANK()` 和 `ROW_NUMBER()` 从表面上看可能似乎可以互换使用，但理解它们的细微差别对 SQL 中的数据分析至关重要。这些函数之间的选择可以显著影响结果和从数据中获得的洞察。

# SQL 中的 DENSE_RANK() 是什么？

`DENSE_RANK()` 是 SQL 中一种强大的排名函数，它在指定的分区内分配唯一的排名值。简而言之，`DENSE_RANK()` 为数据提供无间隔的排名，这意味着每个唯一的值都会获得一个独特的排名，相同的值会获得相同的排名。与其对等函数 `RANK()` 不同，`DENSE_RANK()` 在值出现相同的情况下不会跳过任何排名。

举个例子，假设你有一个学生成绩的数据集，其中三名学生获得了相同的分数，比如 85 分。使用 `RANK()` 时，所有三名学生都将获得 1 的排名，但下一个最佳分数将被排名为 4，跳过了排名 2 和 3。然而，`DENSE_RANK()` 处理方式不同。它会将所有三名学生的排名都定为 1，接下来的最佳分数将获得 2 的排名，确保排名中没有间隔。

那么，什么时候应该使用 `DENSE_RANK()` 呢？它特别适用于需要连续排名且没有任何间隔的场景。考虑一个需要奖励前三名表现者的用例。如果你的数据中存在并列情况，使用 `RANK()` 可能会导致遗漏应得的候选人。这时 `DENSE_RANK()` 就会派上用场，确保所有的顶尖得分者都得到应有的认可，并且排名不会被跳过。

# SQL 中的 DENSE_RANK() 与 RANK() 和 ROW_NUMBER() 对比

理解 `DENSE_RANK()`、`RANK()` 和 `ROW_NUMBER()` 之间的差异对于高效的数据分析非常重要。这三种函数各有强大之处，但它们的细微差别可能会显著影响数据分析的结果。

让我们从`RANK()`开始。这个函数会给数据集中的每个不同值分配一个唯一的排名，对于相同的值分配相同的排名。然而，当`RANK()`遇到相同的值时，它会跳过序列中的下一个排名。例如，如果你有三种产品的销售数据相同，`RANK()`会为这些产品分配相同的排名，但随后会跳过下一个排名。这意味着如果这三种产品是最畅销的，它们都会被分配排名1，但下一个最畅销的产品将被分配排名4，而不是排名2。

接下来，让我们考虑`DENSE_RANK()`。与`RANK()`类似，`DENSE_RANK()`会给相同的值分配相同的排名，但不会跳过任何排名。使用之前的例子，使用`DENSE_RANK()`时，这三种最畅销的产品仍会被分配排名1，但下一个最畅销的产品会被分配排名2，而不是排名4。

最后，`ROW_NUMBER()`采取了不同的方法。它会给每一行分配一个唯一的排名，不论这些值是否相同。这意味着即使三种产品的销售数据相同，`ROW_NUMBER()`也会为每种产品分配一个唯一的编号，这使得它非常适合需要为每一行分配唯一标识符的情况。

# 解密 SQL 中 DENSE_RANK() 的语法和使用方法

`DENSE_RANK()`的语法很简单。它与`OVER()`子句一起使用，在分配排名之前对数据进行分区。语法如下：`DENSE_RANK() OVER (ORDER BY column)`。这里的`column`指的是你希望用来排名的数据列。假设我们有一个名为`Sales`的表，包含`SalesPerson`和`SalesFigures`列。为了根据销售数据对销售人员进行排名，我们可以使用`DENSE_RANK()`函数，如下所示：`DENSE_RANK() OVER (ORDER BY SalesFigures DESC)`。这个SQL查询会根据销售数据从高到低对销售人员进行排名。

将`DENSE_RANK()`与`PARTITION BY`结合使用可以特别有洞察力。例如，如果你想在每个地区内对销售人员进行排名，你可以通过`Region`对数据进行分区，然后在每个分区内进行排名。语法如下：`DENSE_RANK() OVER (PARTITION BY Region ORDER BY SalesFigures DESC)`。通过这种方式，你不仅能获得全面的排名，还能对每个地区的表现有更细致的了解。

# SQL DENSE_RANK() 函数的实际示例

## Apple SQL 问题：找出每个销售日期的最佳销售表现者

**表格：sales_data**

```py
+------------+-----------+------------+
|employee_id | sales_date| total_sales|
+------------+-----------+------------+
|101         |2024-01-01 |500         |
|102         |2024-01-01 |700         |
|103         |2024-01-01 |600         |
|101         |2024-01-02 |800         |
|102         |2024-01-02 |750         |
|103         |2024-01-02 |900         |
|101         |2024-01-03 |600         |
|102         |2024-01-03 |850         |
|103         |2024-01-03 |700         |
+------------+-----------+------------+
```

**输出**

```py
+------------+-----------+------------+
|employee_id | sales_date| total_sales|
+------------+-----------+------------+
|101         |2024-01-01 |800         |
|103         |2024-01-02 |900         |
|102         |2024-01-03 |850         |
+------------+-----------+------------+
```

## Apple 顶级销售表现者解决方案

**第1步：了解数据**

首先，让我们了解`sales_data`表中的数据。该表有三列：employee_id、sales_date和total_sales。这个表代表了销售数据，包含有关员工、销售日期和总销售额的信息。

**第2步：分析 DENSE_RANK() 函数**

该查询使用 DENSE_RANK() 窗口函数根据每个销售日期分区内的 total_sales 对员工进行排名。DENSE_RANK() 用于在 sales_date 的分区内为每行分配一个排名，排序基于 total_sales 降序排列。

**第 3 步：分析查询结构**

现在，让我们来分析查询的结构：

```py
SELECT 
  employee_id, 
  sales_date, 
  total_sales 
FROM 
  (
    SELECT 
      employee_id, 
      sales_date, 
      total_sales, 
      DENSE_RANK() OVER (
        PARTITION BY sales_date 
        ORDER BY 
          total_sales DESC
      ) AS sales_rank 
    FROM 
      sales_data
  ) ranked_sales 
WHERE 
  sales_rank = 1;
```

+   SELECT 子句：这指定了将在最终结果中包含的列。在这种情况下，它是 employee_id、sales_date 和 total_sales。

+   FROM 子句：这是实际数据来源的地方。它包括一个子查询（用括号括起来），该子查询从 sales_data 表中选择列，并使用 DENSE_RANK() 添加一个计算列。

+   DENSE_RANK() 函数：此函数在子查询中用于根据 total_sales 列为每行分配一个排名，并按 sales_date 进行分区。这意味着排名是针对每个销售日期单独进行的。

+   WHERE 子句：此子句筛选结果，只包括 sales_rank 等于 1 的行。这确保了每个销售日期的 top sales performer 只被包含在最终结果中。

**第 4 步：执行查询**

当你执行此查询时，它将生成一个结果集，其中包括每个销售日期的 top sales performer 的 employee_id、sales_date 和 total_sales。

**第 5 步：查看输出**

最终输出表格，名为 top_performers，将包含所需的信息：根据 DENSE_RANK() 计算的每个销售日期的 top sales performer。

## Google SQL 问题：找出每个产品中提供最高评分的客户

**表格：product_reviews**

```py
+------------+-----------+-------------+-------------------------------+
|customer_id | product_id| review_date | review_score | helpful_votes  |
+------------+-----------+-------------+--------------+----------------+
|301         |101        |2024-04-01   |4.5           | 12             |
|302         |102        |2024-04-01   |3.8           | 8              |
|303         |103        |2024-04-01   |4.2           | 10             |
|301         |101        |2024-04-02   |4.8           | 15             |
|302         |102        |2024-04-02   |3.5           | 7              |
|303         |103        |2024-04-02   |4.0           | 11             |
|301         |101        |2024-04-03   |4.2           | 13             |
|302         |102        |2024-04-03   |4.0           | 10             |
|303         |103        |2024-04-03   |4.5           | 14             |
+------------+-----------+-------------+--------------+----------------+
```

**输出**

```py
+------------+-----------+-------------+--------------+----------------+
|customer_id | product_id| review_date | review_score | helpful_votes  |
+------------+-----------+-------------+--------------+----------------+
|301         |101        |2024-04-01   |4.5           | 12             |
|301         |101        |2024-04-02   |4.8           | 15             |
|303         |103        |2024-04-03   |4.5           | 14             |
+------------+-----------+-------------+--------------+----------------+
```

## Google 最高评分解决方案

**第 1 步：了解数据**

product_reviews 表格包含有关各种产品的客户评价信息。它包括 customer_id、product_id、review_date、review_score 和 helpful_votes 等列。此表格表示与客户评价相关的数据，包含有关客户、被评价的产品、评价日期、评价分数以及获得的 helpful votes 数量。

**第 2 步：分析 DENSE_RANK() 函数**

在此查询中，DENSE_RANK() 窗口函数用于在 product_id 和 review_date 定义的每个分区内对行进行排名。排名是基于两个标准：review_score 降序排列和 helpful_votes 降序排列。这意味着评分较高且有更多 helpful votes 的行将被分配更低的排名。

**第 3 步：分析查询结构**

现在，让我们来分析查询的结构：

```py
SELECT 
  customer_id, 
  product_id, 
  review_date, 
  review_score, 
  helpful_votes 
FROM 
  (
    SELECT 
      customer_id, 
      product_id, 
      review_date, 
      review_score, 
      helpful_votes, 
      DENSE_RANK() OVER (
        PARTITION BY product_id, 
        review_date 
        ORDER BY 
          review_score DESC, 
          helpful_votes DESC
      ) AS rank_within_product 
    FROM 
      product_reviews
  ) ranked_reviews 
WHERE 
  rank_within_product = 1;
```

+   SELECT 子句：指定将在最终结果中包含的列。它包括 customer_id、product_id、review_date、review_score 和 helpful_votes。

+   FROM子句：这一部分包括一个子查询（括号内）选择product_reviews表中的列，并使用`DENSE_RANK()`添加一个计算列。计算是在由product_id和review_date定义的分区上进行的，排名基于review_score和helpful_votes的降序。

+   `DENSE_RANK()`函数：此函数在子查询中应用，根据指定的标准为每一行分配一个排名。排名是根据每个product_id和review_date的组合单独进行的。

+   WHERE子句：筛选结果以仅包括rank_within_product等于1的行。这确保了最终结果中只包含每个产品在每个评论日期的排名最高的行。

**步骤 4：执行查询**

执行此查询将生成包含所需信息的结果集：每个产品和评论日期组合中基于评论分数和有用票数的排名最高评论的customer_id、product_id、review_date、review_score和helpful_votes。

**步骤 5：检查输出**

最终输出表，名为top_reviewers，将显示每个产品在每个评论日期的排名最高的评论，同时考虑评论分数和有用票数。

# 避免SQL中使用`DENSE_RANK()`的常见错误

虽然`DENSE_RANK()`是一个非常有用的SQL函数，但分析师，特别是那些刚接触SQL的人，使用时出错并不少见。让我们更详细地探讨一些常见的错误及其避免方法。

一个常见的错误是误解`DENSE_RANK()`如何处理NULL值。与某些SQL函数不同，`DENSE_RANK()`将所有NULL视为相同。这意味着，如果你在排名数据中有NULL值，`DENSE_RANK()`会将所有NULL值赋予相同的排名。在处理包含NULL值的数据集时，请注意这一点，并考虑用在你的上下文中代表其含义的值替代NULL，或根据具体要求将其排除。

另一个常见的错误是忽视在使用`DENSE_RANK()`时分区的重要性。`PARTITION BY`子句允许你将数据分为不同的段，并在这些分区内进行排名。不使用`PARTITION BY`可能会导致错误的结果，特别是当你希望对不同的类别或组重新开始排名时。

与此相关的是`ORDER BY`子句在`DENSE_RANK()`中的不当使用。`DENSE_RANK()`默认按升序分配排名，意味着最小值获得排名1。如果你需要排名按降序进行，你必须在`ORDER BY`子句中包含`DESC`关键字。不这样做将产生可能与你的期望不符的排名。

最后，一些分析师错误地使用 `DENSE_RANK()` 而应使用 `ROW_NUMBER()` 或 `RANK()`，反之亦然。正如我们讨论的，这三种函数都有独特的行为。理解这些细微差别并根据你的特定用例选择正确的函数，对于进行准确和有效的数据分析至关重要。

## **掌握 DENSE_RANK() 如何提升 SQL 数据分析的效率**

掌握 `DENSE_RANK()` 的使用可以显著提升 SQL 数据分析的效率，特别是在涉及排名和比较的情况下。这个函数提供了一种细致的排名方法，通过对相同的值分配相同的排名而不跳过任何排名数字，保持了排名尺度的连续性。

这在分析大型数据集时特别有用，因为数据点经常可能有相同的值。例如，在一个销售数据集中，多个销售人员可能达到了相同的销售额。`DENSE_RANK()` 使得排名公平，每个销售人员都分配相同的排名。此外，`DENSE_RANK()` 与 `PARTITION BY` 结合使用时，可以进行针对特定类别的分析。

这个函数的应用在处理空值时变得更加有效。`DENSE_RANK()` 不会将这些空值排除在排名过程之外，而是将所有空值视为相同并分配相同的排名。这确保了即使确切的值可能缺失，数据点也不会被忽略，从而提供了更全面的分析。

# 常见问题

## 我可以在哪里练习[SQL 面试题](https://bigtechinterviews.com/sql-interview-questions/)，包括 `DENSE_RANK()`？

为了提升你的 SQL 技能，我们推荐在 BigTechInterviews、Leetcode 或类似的平台上进行在线练习。

## `DENSE_RANK()` 在 SQL 中的作用是什么？

`DENSE_RANK()` 是一个 SQL 窗口函数，根据指定的列给数据行分配排名。它通过给相同的排名来处理并列情况，而不会在排名序列中留下任何空隙。

## RANK()、ROW_NUMBER() 和 DENSE_RANK() 在 SQL 中有什么区别？

RANK() 和 ROW_NUMBER() 为数据分配排名，但处理并列情况的方式不同。RANK() 对于并列数据留有排名空隙，而 ROW_NUMBER() 为每一行分配一个唯一的编号，不考虑并列情况。另一方面，`DENSE_RANK()` 给并列数据点分配相同的排名，没有任何空隙。

## 如何在 SQL 中的 WHERE 子句中使用 `DENSE_RANK()`？

`DENSE_RANK()` 是一个窗口函数，不能直接在 WHERE 子句中使用。相反，它可以与其他函数如 `ROW_NUMBER()` 或 `RANK()` 结合使用，这些函数可以在 WHERE 子句中用来根据排名过滤数据。

## `DENSE_RANK()` 可以在没有 PARTITION BY 的情况下使用吗？

不，指定 PARTITION BY 对于 DENSE_RANK() 的正常运行至关重要。如果没有它，所有数据将被视为一个组，从而导致不准确和无意义的排名。掌握 DENSE_RANK() 在 SQL 中的使用可以显著提升你的数据分析技能。

## `RANK()` 和 `DENSE_RANK()` 之间的区别是什么？

`RANK()` 和 `DENSE_RANK()` 主要的区别在于它们处理平局的方式。`RANK()` 会在排名中留下空缺，而 `DENSE_RANK()` 会为平局的数据点分配相同的排名，没有空缺。此外，`RANK()` 总是为每一行的新数据递增排名，而 `DENSE_RANK()` 维持连续的排名。

**[](https://www.linkedin.com/in/john-h-24005524a/)**[约翰·休斯](https://www.linkedin.com/in/john-h-24005524a/)**** 曾是 Uber 的数据分析师，后来创办了名为 BigTechInterviews (BTI) 的 SQL 学习平台。他对学习新编程语言充满热情，并帮助候选人获得通过技术面试的信心和技能。他的家在科罗拉多州丹佛市。

### 更多相关内容

+   [深入解析 AutoGPT](https://www.kdnuggets.com/2023/05/breaking-autogpt.html)

+   [量子计算解析：对数据科学和 AI 的影响](https://www.kdnuggets.com/breaking-down-quantum-computing-implications-for-data-science-and-ai)

+   [逐步指南：阅读和理解 SQL 查询](https://www.kdnuggets.com/a-step-by-step-guide-to-reading-and-understanding-sql-queries)

+   [通过集成 Jupyter 和 KNIME 减少实施时间](https://www.kdnuggets.com/2021/12/cutting-implementation-time-integrating-jupyter-knime.html)

+   [成为数据科学家的指南（逐步方法）](https://www.kdnuggets.com/2021/05/guide-become-data-scientist.html)

+   [如何构建数据科学项目：逐步指南](https://www.kdnuggets.com/2022/05/structure-data-science-project-stepbystep-guide.html)
