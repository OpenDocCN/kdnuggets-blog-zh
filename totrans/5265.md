# 如何编写更好的 SQL 查询：权威指南 – 第1部分

> 原文：[https://www.kdnuggets.com/2017/08/write-better-sql-queries-definitive-guide-part-1.html/2](https://www.kdnuggets.com/2017/08/write-better-sql-queries-definitive-guide-part-1.html/2)

### 3\. 不要让查询变得比需要的更复杂

数据类型转换引出下一个要点：你不应该过度设计你的查询。尽量保持简单和高效。这可能看起来太简单或愚蠢，尤其是因为查询可能会变得复杂。

然而，你将在接下来的示例中看到，你可以轻易地使简单的查询变得比需要的更复杂。

**`OR`运算符**

当你在查询中使用`OR`运算符时，很可能你没有使用索引。

**记住**，索引是一种数据结构，可以提高数据库表中数据检索的速度，但代价是：需要额外的写入和额外的存储空间来维护索引数据结构。索引用于快速定位或查找数据，而不必每次访问数据库表时都搜索每一行数据。索引可以通过使用数据库表中的一个或多个列来创建。

如果你不利用数据库中包含的索引，你的查询将不可避免地运行得更慢。因此，最好寻找替代`OR`运算符的方法；

考虑以下查询：

```py
SELECT driverslicensenr, name 
FROM Drivers 
WHERE driverslicensenr = 123456 
OR driverslicensenr = 678910 
OR driverslicensenr = 345678;
```

你可以通过以下方式替换运算符：

+   带有 IN 的条件；或

```py
SELECT driverslicensenr, name 
FROM Drivers 
WHERE driverslicensenr IN (123456, 678910, 345678);
```

+   两个`SELECT`语句与一个`UNION`。

**提示**：在这里，你需要小心不要不必要地使用`UNION`操作，因为你会多次遍历同一个表。同时，你需要意识到，当你在查询中使用`UNION`时，执行时间会增加。`UNION`操作的替代方案包括：重新制定查询，将所有条件放在一个`SELECT`语句中，或者使用`OUTER JOIN`代替`UNION`。

**提示**：在这里也要记住，尽管`OR`—以及接下来会提到的其他运算符—可能不使用索引，但索引查找并不总是优选的！

**`NOT`运算符**

当你的查询包含`NOT`运算符时，很可能索引不会被使用，就像`OR`运算符一样。这将不可避免地降低查询速度。如果你不知道这里的意思，考虑以下查询：

```py
SELECT driverslicensenr, name 
FROM Drivers 
WHERE NOT (year > 1980);
```

这个查询会比你预期的要慢很多，主要是因为它的表达方式比实际需要的要复杂：在这种情况下，最好寻找替代方案。考虑用比较运算符如`>`、`<>`或`!>`来替代`NOT`；上面的示例确实可以重写成如下形式：

```py
SELECT driverslicensenr, name FROM Drivers WHERE year <= 1980;
```

这看起来已经整洁多了，不是吗？

**`AND`运算符**

`AND`操作符是另一个不使用索引的操作符，如果以过于复杂和低效的方式使用，如下面的例子，会使查询变慢：

```py
SELECT driverslicensenr, name 
FROM Drivers 
WHERE year >= 1960 AND year <= 1980;
```

最好是重写这个查询，并使用`BETWEEN`操作符：

```py
SELECT driverslicensenr, name 
FROM Drivers 
WHERE year BETWEEN 1960 AND 1980;
```

**`ANY`和`ALL`操作符**

`ALL`操作符也是需要小心的，因为在查询中包含这些操作符时，索引不会被使用。这里的有用替代方案是像`MIN`或`MAX`这样的聚合函数。

**提示**：在使用建议的替代方案时，你应该意识到，像`SUM`、`AVG`、`MIN`、`MAX`等聚合函数在处理大量行时可能会导致查询时间较长。在这种情况下，你可以尝试减少处理的行数或预先计算这些值。你会再次发现，了解你的环境、查询目标等在决定使用哪种查询时非常重要！

**在条件中隔离列**

同样地，当一个列在计算或标量函数中使用时，索引也不会被使用。一个可能的解决方案是简单地隔离特定的列，使其不再是计算或函数的一部分。考虑以下示例：

```py
SELECT driverslicensenr, name 
FROM Drivers 
WHERE year + 10 = 1980;
```

这看起来有点怪吧？相反，可以重新考虑计算方式，并将查询重写成如下形式：

```py
SELECT driverslicensenr, name 
FROM Drivers 
WHERE year = 1970;
```

### 4\. 避免暴力破解

最后一条提示实际上意味着你不应该过度限制查询，因为这可能会影响其性能。这在连接操作和`HAVING`子句中特别如此。

**连接操作**

+   **表的顺序**

当你连接两个表时，考虑表在连接中的顺序可能很重要。如果你发现一个表明显比另一个表大，你可能需要重写查询，以便将最大表放在连接的最后。

+   **连接中的冗余条件**

当你为连接添加过多条件时，你基本上强迫SQL选择某条路径。然而，这条路径可能并不总是性能最优的。

**`HAVING`子句**

`HAVING`子句最初是因为`WHERE`关键字不能与聚合函数一起使用而添加到SQL中的。`HAVING`通常与`GROUP BY`子句一起使用，以将返回的行组限制为仅符合某些条件的组。然而，如果你在查询中使用这个子句，索引将不会被使用，正如你已经知道的，这可能会导致查询的性能不佳。

如果你在寻找替代方案，可以考虑使用`WHERE`子句。考虑以下查询：

```py
SELECT state, COUNT(*) 
FROM Drivers 
WHERE state IN ('GA', 'TX') 
GROUP BY state 
ORDER BY state
```

```py
SELECT state, COUNT(*) 
FROM Drivers 
GROUP BY state 
HAVING state IN ('GA', 'TX') 
ORDER BY state
```

第一个查询使用`WHERE`子句来限制需要求和的行数，而第二个查询则对表中的所有行进行求和，然后使用`HAVING`来丢弃计算出的总和。在这种情况下，显然使用`WHERE`子句的替代方法更好，因为你不会浪费任何资源。

你会看到这不是关于限制结果集，而是关于限制查询中的中间记录数。

**注意** 这两个子句之间的区别在于 `WHERE` 子句对单独的行引入条件，而 `HAVING` 子句对聚合或选择结果引入条件，其中产生了一个结果，如 `MIN`、 `MAX`、 `SUM`… 这是从多行中生成的。

你看，评估查询的质量、编写和重写查询不是一项容易的工作，尤其是在考虑到它们需要尽可能高效的情况下；避免反模式并考虑替代方案也是你在专业环境中编写要在数据库上运行的查询时的责任之一。

这个列表只是一些反模式和提示的小概述，希望对初学者有所帮助；如果你想了解更多资深开发人员认为最常见的反模式，可以查看 [这个讨论](https://stackoverflow.com/questions/346659/what-are-the-most-common-sql-anti-patterns)。

**个人简介：[Karlijn Willems](https://www.linkedin.com/in/karlijnwillems)** 是一位数据科学记者，为 [DataCamp 社区](https://www.datacamp.com/community/authors/karlijn-willems)撰写文章，专注于数据科学教育、最新新闻和热门趋势。她拥有文学、语言学和信息管理方面的学位。

[原文](https://www.datacamp.com/community/tutorials/sql-tutorial-query#gs.QQP_Fhg)。经许可转载。

**相关：**

+   [掌握数据科学的 SQL 七步法](/2016/06/seven-steps-mastering-sql-data-science.html)

+   [SQL 中最被低估的函数](/2017/03/most-underutilized-function-sql.html)

+   [使用 SQL 进行统计分析](/2016/08/doing-statistics-sql.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的捷径。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

### 更多相关话题

+   [转行数据科学的权威指南](https://www.kdnuggets.com/2022/05/definitive-guide-switching-career-data-science.html)

+   [Python 函数参数：权威指南](https://www.kdnuggets.com/2023/02/python-function-arguments-definitive-guide.html)

+   [解决 MySQL 中幻读问题的权威指南](https://www.kdnuggets.com/2022/06/definitive-guide-solving-phantom-read-mysql.html)

+   [逐步指南：阅读和理解 SQL 查询](https://www.kdnuggets.com/a-step-by-step-guide-to-reading-and-understanding-sql-queries)

+   [如何在原生 Python 中编写 SQL](https://www.kdnuggets.com/2022/02/easy-sql-native-python.html)

+   [KDnuggets 新闻，12月7日：破解数据科学十大迷思 • 4…](https://www.kdnuggets.com/2022/n47.html)
