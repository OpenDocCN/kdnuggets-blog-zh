# 如何优化 SQL 查询以加快数据检索速度

> 原文：[https://www.kdnuggets.com/2023/06/optimize-sql-queries-faster-data-retrieval.html](https://www.kdnuggets.com/2023/06/optimize-sql-queries-faster-data-retrieval.html)

![如何优化 SQL 查询以加快数据检索速度](../Images/164222626d74bf6e1b3a054356161524.png)

图片作者

SQL（结构化查询语言），如你所知，帮助你从数据库中收集数据。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能。

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT。

* * *

它是专门为此设计的。换句话说，它处理行和列，使你能够使用 SQL 查询操作数据库中的数据。

# 什么是 SQL 查询？

SQL 查询是一组你给数据库的指令，用以收集信息。

你可以通过使用这些查询来收集和操作数据库中的数据。

通过使用它们，你可以创建报告、进行数据分析等。

由于这些查询的形式和长度，执行时间可能会很长，特别是当你处理更大的数据表时。

# 为什么我们需要 SQL 查询优化？

SQL 查询优化的目的是确保你有效地使用资源。简单来说，它可以减少执行时间，节省成本，并提高性能。这是开发人员和数据分析师的重要技能。不仅仅是从数据库中返回正确的数据同样重要，你还需要知道你是多么高效地做到这一点。

你应该总是问自己：“有没有更好的方式来编写我的查询？”

让我们更深入地探讨一下原因。

**资源效率：** 未优化的 SQL 查询会消耗过多的系统资源，如 CPU 和内存。这可能导致整体系统性能降低。优化 SQL 查询可以确保这些资源的高效使用，从而带来更好的性能和可扩展性。

**减少执行时间：** 如果查询运行缓慢，这会对用户体验产生负面影响。或者如果你有一个运行中的应用程序，这也会影响应用程序性能。优化查询可以帮助减少执行时间，提供更快的响应时间和更好的用户体验。

**成本节约：** 优化的查询可以减少支持数据库系统所需的硬件和基础设施。这可以导致硬件、能源和维护成本的节省。

查看 “[编写 SQL 查询的最佳实践](https://www.stratascratch.com/blog/best-practices-to-write-sql-queries-how-to-structure-your-code/?utm_source=blog&utm_medium=click&utm_campaign=kdn+optimize+sql+queries)” 以了解如何改善你的代码结构，即使它是正确的。

# SQL 查询优化技术

这是我们将在本文中涵盖的 SQL 查询优化技术概述。

![如何优化 SQL 查询以加快数据检索速度](../Images/19c39f9909d480fde0965e046fa9c692.png)

作者提供的图片

这是一个显示优化 SQL 查询时建议遵循的步骤的流程图。我们将在示例中采用相同的方法。值得注意的是，优化工具也可以帮助提高查询性能。因此，让我们从著名的 SQL 命令 SELECT 开始探索这些技术。

![如何优化 SQL 查询以加快数据检索速度](../Images/83bc85102717f9785edc955128d9e1a3.png)

作者提供的图片

## 使用指定字段的 SELECT 代替 SELECT *

当你使用 SELECT * 时，它将返回表中的所有行和所有列。你需要问自己是否真的需要这样做。

使用特定字段替代扫描整个数据库。

在这个示例中，我们将用具体的列名替换 SELECT *。如你所见，这将减少检索的数据量。

结果是，查询运行得更快，因为数据库只需获取和提供请求的列，而不是表的所有列。

这可以最小化数据库的 I/O 负担，尤其是在表包含大量列或大量数据行时特别有用。

以下是优化前的代码。

```py
SELECT * FROM customer;
```

这是输出。

![如何优化 SQL 查询以加快数据检索速度](../Images/ef37bb4735e456e630794c838950c315.png)

总查询运行时间为 260 毫秒。这是可以改进的。

为了展示这一点，我将仅选择 3 个不同的列，而不是选择所有列。

你可以根据项目需求选择所需的列。

这是代码。

```py
SELECT customer_id, 
       age, 
       country 
FROM customer;
```

这是输出。

![如何优化 SQL 查询以加快数据检索速度](../Images/7dd07451fedb7a309579844433ec465e.png)

正如你所见，通过定义我们要选择的字段，我们不会强迫数据库扫描所有数据，因此运行时间从 260 毫秒减少到 79 毫秒。

想象一下，如果有数百万或数十亿行，或者数百列会是什么情况。

## 避免使用 SELECT DISTINCT

SELECT DISTINCT 用于返回指定列中的唯一值。为此，数据库引擎必须扫描整个表并删除重复值。在许多情况下，使用诸如 GROUP BY 之类的替代方法可以通过减少处理的数据量来提高性能。

这是代码。

```py
SELECT DISTINCT segment 
FROM customer;
```

这是输出。

![如何优化 SQL 查询以加快数据检索速度](../Images/7fd929898ef89a5f6a0095c009b3d362.png)

我们的代码?? 从客户表中检索 segment 列中的唯一值。数据库引擎必须处理表中的所有记录，识别重复值，并仅返回唯一值。这可能在时间和资源上是昂贵的，特别是对于大型表。

在替代版本中，以下查询通过使用 GROUP BY 子句来检索 segment 列中的唯一值。GROUP BY 子句根据指定的列对记录进行分组，并为每个组返回一条记录。

这是代码。

```py
SELECT segment
FROM customer
GROUP BY segment;
```

这是输出结果。

![如何优化 SQL 查询以加快数据检索速度](../Images/0d7f498479d2c7c0f4459e52f2c1b4fc.png)

在这种情况下，GROUP BY 子句有效地根据 segment 列对记录进行分组，结果与 SELECT DISTINCT 查询相同。

通过避免使用 SELECT DISTINCT，而改用 GROUP BY，你可以优化 SQL 查询，将总查询时间从 198 毫秒减少到 62 毫秒，速度提高了三倍多。

## 避免使用循环

循环可能会导致查询变慢，因为它们强迫数据库逐条记录进行处理。

在可能的情况下，使用内置操作和 SQL 函数，这可以利用数据库引擎的优化并更高效地处理数据。

让我们定义一个带有循环的自定义函数。

```py
CREATE OR REPLACE FUNCTION sum_ages_with_loop() RETURNS TABLE (country_name TEXT, sum_age INTEGER) AS $$
DECLARE
    country_record RECORD;
    age_sum INTEGER;
BEGIN
    FOR country_record IN SELECT DISTINCT country FROM customer WHERE segment = 'Corporate'
    LOOP
        SELECT SUM(age) INTO age_sum FROM customer WHERE country = country_record.country AND segment = 'Corporate';
        country_name := country_record.country;
        sum_age := age_sum;
        RETURN NEXT;
    END LOOP;
END;
$$ LANGUAGE plpgsql;
```

上面的代码使用基于循环的方法来计算每个国家中客户 segment 为 'Corporate' 的年龄总和。

它首先检索一个唯一国家的列表，然后使用循环遍历每个国家，计算该国客户的年龄总和。这种方法可能会很慢且效率低，因为它逐行处理数据。

现在让我们运行这个函数。

```py
SELECT * 
FROM sum_ages_with_loop()
```

这是输出结果。

![如何优化 SQL 查询以加快数据检索速度](../Images/443a4d8bf35f207bd0056112c6d42521.png)

使用这种方法的运行时间为 198 毫秒。

让我们看看我们优化后的 SQL 代码。

```py
SELECT country, 
       SUM(age) AS sum_age
FROM customer
WHERE segment = 'Corporate'
GROUP BY country;
```

这是它的输出结果。

![如何优化 SQL 查询以加快数据检索速度](../Images/695c2e444db041a42254006aa023cd92.png)

一般来说，使用单个 SQL 查询的优化版本会表现得更好，因为它可以利用数据库引擎的优化能力。

为了在我们第一个代码中获得相同的结果，我们在 PL/pgSQL 函数中使用了循环，这通常比使用单个 SQL 查询更慢且效果较差。并且它强迫你编写更多的代码行！

## 正确使用通配符

适当地使用通配符对优化 SQL 查询至关重要，特别是在匹配字符串和模式时。

通配符是 SQL 查询中用于查找特定模式的特殊字符。

SQL 中最常用的通配符是“%”和“_”，其中“%”代表任意字符序列，“_”代表单个字符。

明智地使用通配符很重要，因为不当使用可能会导致性能问题，尤其是在大型数据库中。

然而，效率地使用它们可以大大提高字符串匹配和模式匹配查询的性能。

现在让我们查看我们的例子。

这个查询使用 RIGHT() 函数提取 customer_name 列的最后三个字符，然后检查它是否等于 'son'。

```py
SELECT customer_name
FROM customer
WHERE RIGHT(customer_name, 3) = 'son';
```

这是输出结果。

![如何优化 SQL 查询以加快数据检索速度](../Images/50bb79f387c0bbaf207967fd6593f3ed.png)

尽管这个查询达到了预期的结果，但效率不是很高，因为 RIGHT() 函数必须应用到表中的每一行。

让我们通过使用通配符来优化我们的代码。

```py
SELECT customer_name
FROM customer
WHERE customer_name LIKE '%son';
```

这是输出结果。

![如何优化 SQL 查询以加快数据检索速度](../Images/1f4c7b3e2e885aea23bb8a9fe980f87f.png)

这个优化后的 SQL 查询使用 LIKE 运算符和通配符 “%” 来查找 customer_name 列以 'son' 结尾的记录。

这种方法更高效，因为它利用了数据库引擎的模式匹配能力，如果有索引的话，它能更好地利用索引。

如我们所见，总查询时间从 436 毫秒减少到 62 毫秒，几乎快了 7 倍。

## 使用 TOP 或 LIMIT 来限制样本结果的数量

使用 TOP 或 LIMIT 来限制样本结果对于优化 SQL 查询至关重要，尤其是处理大型表时。

这些子句允许你从表中检索指定数量的记录，而不是所有记录，这对性能是有益的。

现在，让我们从客户表中检索所有信息。

```py
SELECT *
FROM customer
```

这是输出结果。

![如何优化 SQL 查询以加快数据检索速度](../Images/ef37bb4735e456e630794c838950c315.png)

在处理大型表时，这个操作可能增加 I/O 和网络延迟，从而降低 SQL 查询性能。

现在让我们通过限制输出为 10 条来优化代码。

```py
SELECT *
FROM customer
LIMIT 10;
```

这是输出结果。

![如何优化 SQL 查询以加快数据检索速度](../Images/408c8dcaf70162f7f10082c2592ec7b7.png)

通过限制输出，你将减少网络延迟和内存使用，改善响应时间，特别是在处理大型表时。在我们的例子中，SQL 查询优化后，总查询运行时间从 260 毫秒减少到 89 毫秒。

所以我们的查询速度变快了近 3 倍。

## 使用索引

这次，我们将识别并创建适当的索引，用于 WHERE、JOIN 和 ORDER BY 子句中的列，以提高查询性能。

通过对频繁访问的列建立索引，数据库可以更快地检索数据。

现在，让我们首先运行以下查询。

```py
SELECT customer_id, 
       customer_name
FROM customer
WHERE segment = 'Corporate';
```

这是输出结果。

![如何优化 SQL 查询以加快数据检索速度](../Images/853a5aaa0d06964a4879b13bb1bdd2da.png)

我们的查询运行时间为 259 毫秒。

让我们尝试通过创建索引来进一步改进。

```py
CREATE INDEX idx_segment ON customer (segment);
```

很好，现在我们再次运行代码。

```py
SELECT customer_id, 
       customer_name
FROM customer WITH (INDEX(idx_segment))
WHERE segment = 'Corporate';
```

这是输出结果。

![如何优化 SQL 查询以加快数据检索速度](../Images/0f64e18143aa8147cafa3574a7688843.png)

通过在 INDEX() 中使用 idx_segment，数据库引擎能够基于 segment 列高效搜索客户表，使查询速度更快——将总查询时间从 259 毫秒减少到 75 毫秒。

## 奖励部分：使用 SQL 查询优化工具

由于长代码和高度复杂查询的复杂性，你可能考虑使用查询优化工具。

这些工具可以分析你的查询执行计划，识别缺失的索引，并建议替代查询结构，以帮助优化查询。一些流行的查询优化工具包括：

1.  **SolarWinds 数据库性能分析器**：该工具帮助你监控和提升数据库性能。它展示了查询中的问题及其运行方式。它兼容多种数据库系统，如 SQL Server、Oracle 和 MySQL。

你可以在[这里](https://www.solarwinds.com/database-performance-analyzer?CMP=KNC-TAD-GGL-SW_TK_X_PP_CPC_LD_EN_PBRDB_TIN-XDB&19231722695~147153297809_g_c_solarwinds%20database%20performance%20analyzer-e~640975433012~~p74562349021&s_kwcid=AL!11508!3!640975433012!e!!g!!solarwinds%20database%20performance%20analyzer&ds_cid=71700000103710951&ds_agid=58700008203730912&network=g&device=c&keyword=Solarwinds%20database%20performance%20analyzer&matchtype=e&creative=640975433012&feeditemid=&gad=1&gclid=CjwKCAjwjYKjBhB5EiwAiFdSfo-vO3O-Mg5UZXZDnWSnZS3KSBufeQBaDTkUv64FBjfdMqPoO4OtyBoC4kwQAvD_BwE&gclsrc=aw.ds)找到它。

1.  **SQL Query Tuner for SQL Diagnostic Manager**：该工具具有先进的功能来提升查询效果，如性能提示、索引检查以及展示查询的运行方式。它帮助你通过发现和修复问题来改进 SQL 查询。

1.  **SQL Server Management Studio (SSMS)**：SSMS 具有内置工具来检查性能和优化查询，如活动监视器、执行计划分析和索引调整向导。

1.  **EverSQL**：EverSQL 是一个在线工具，通过查看数据库结构和查询运行方式自动优化你的查询。它提供建议并重写你的 SQL 查询以提高速度。

使用 SQL 查询优化工具和资源对于改进查询至关重要。通过这些工具，你可以了解查询的工作原理，发现问题，并采用最佳实践来加快数据获取速度，提升应用程序性能。

如果你想简化复杂的 SQL 查询，可以查看 “[如何简化复杂 SQL 查询](https://www.stratascratch.com/blog/how-to-simplify-complex-sql-queries-with-examples/?utm_source=blog&utm_medium=click&utm_campaign=kdn+optimize+sql+queries)”。

# 最后的说明

通过优化 SQL 查询所做的更改可能由于其规模（毫秒）看起来微不足道。但随着你处理的数据量增长，这些毫秒将变为秒、分钟，甚至小时。到那时，你会意识到这些 SQL 查询优化技术是非常重要的。

如果你想了解更多，这里有[30个顶级SQL查询面试问题](https://www.stratascratch.com/blog/top-30-sql-query-interview-questions/?utm_source=blog&utm_medium=click&utm_campaign=kdn+optimize+sql+queries)，这将帮助那些在学习时也想为面试做准备的人。

感谢阅读！

**[Nate Rosidi](https://www.stratascratch.com)** 是一名数据科学家及产品战略专家。他还是一名兼职教授，教授分析课程，并且是[StrataScratch](https://www.stratascratch.com/)的创始人，这个平台帮助数据科学家通过顶级公司的真实面试问题准备面试。可以在[Twitter: StrataScratch](https://twitter.com/StrataScratch)或[LinkedIn](https://www.linkedin.com/in/nathanrosidi/)上与他联系。

### 更多相关内容

+   [检索增强生成：信息检索与文本生成的交汇点](https://www.kdnuggets.com/retrieval-augmented-generation-where-information-retrieval-meets-text-generation)

+   [如何优化Dockerfile指令以加快构建时间](https://www.kdnuggets.com/how-to-optimize-dockerfile-instructions-for-faster-build-times)

+   [检索增强生成如何使LLMs更智能](https://www.kdnuggets.com/how-retrieval-augment-generation-makes-llms-smarter)

+   [十大MLOps工具以优化和管理机器学习生命周期](https://www.kdnuggets.com/2022/10/top-10-mlops-tools-optimize-manage-machine-learning-lifecycle.html)

+   [KDnuggets 新闻，12月7日：揭示十大数据科学误区 • 4…](https://www.kdnuggets.com/2022/n47.html)

+   [4个有用的中级SQL查询用于数据科学](https://www.kdnuggets.com/2022/12/4-useful-intermediate-sql-queries-data-science.html)
