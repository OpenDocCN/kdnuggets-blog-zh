# SQL 案例研究：帮助初创公司 CEO 管理数据

> 原文：[https://www.kdnuggets.com/2018/09/sql-case-study-helping-startup-ceo-manage-data.html/2](https://www.kdnuggets.com/2018/09/sql-case-study-helping-startup-ceo-manage-data.html/2)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](/2018/09/sql-case-study-helping-startup-ceo-manage-data.html?page=2#comments)

**表中存在多少行？**

我们想知道这个初创公司有多少人。我们通过使用 count(*) 计算行数来实现：

```py

SELECT COUNT(*)
FROM startup;

```

**列中有多少行具有最小值？**

我们想知道在初创公司中哪些人的薪水最低以及有多少人。让我们尝试这个查询：

```py

SELECT *
FROM startup
WHERE salary = MIN(salary);

```

这将导致一个错误，提示：

```py

ERROR:  aggregate functions are not allowed in WHERE

```

聚合函数如 COUNT()、MIN()、MAX()、AVG()、SUM() 等，它们将列中的值作为输入并返回一个单一值（或 NULL）。这里 MIN() 在 WHERE 子句之后使用，因此我们可以通过检查薪水是否等于最小值（或不是）来实现，这个最小值我们可以从另一个查询中获取，而不是通过聚合来获取，如下所示：

```py

SELECT MIN(salary)
FROM startup;

```

但这将产生不合理的值，即 1000$，那么发生了什么？！

这是因为排序顺序，如果你对 ASCII 排序顺序感兴趣，可以查看这个[链接](https://www.ibm.com/support/knowledgecenter/SS6SG3_6.1.0/com.ibm.cobol61.ent.doc/PGandLR/ref/rlebcasc.html)。

那么，我们现在应该怎么做？

实际上，我在创建表的开始阶段做了一些效率不高的事情，那就是将薪水列存储为 varchar。原因存在于这个[stackoverflow 问题](https://stackoverflow.com/questions/3008371/storing-numbers-as-varchar)的*RidFilter*的*回答*中。

相信与否，我处理过一些类似的数据。它包括存储为 varachar 的美元符号，因此让我们修复它，以便能够对其进行操作。

**从字符串中删除字符：**

这个问题可以通过首先删除美元符号，然后将该 varchar 转换为可以是整数的数值来解决。

```py

SELECT REPLACE(salary, '$', '')
FROM startup;

```

这个 REPLACE() 函数将导致薪水列中的值去除美元符号（即用空替换 $），但请注意这并没有编辑到表中。所以，下次我们需要操作时需要使用它。此外，请注意，这一列仍然是字符串而非数值，因此我们需要将其转换为小数。

现在，我们可以对转换后的薪水值应用 MIN()：

```py

SELECT MIN(
CAST(
REPLACE(salary, '$', '')
as DECIMAL))
FROM startup;

```

CAST() 函数将新的薪水列转换为小数值。

记住，我们在初创公司表中仍然看到带有美元符号的薪水。

为了在表中去除美元符号，我们使用 UPDATE() 函数：

```py

UPDATE startup
SET salary = REPLACE(salary, '$', '');

```

现在，我们回到我们的难题，即找出初创公司中薪水最低的人员。

```py

SELECT *
FROM startup
WHERE CAST(salary AS DECIMAL) = 800;

```

我们可以只使用一个带有子查询的查询，而不是最后两个独立的查询：

```py

SELECT *
FROM startup
WHERE CAST(
salary AS DECIMAL) = (
SELECT MIN( CAST(salary AS DECIMAL) ) FROM startup );

```

我们还可以使用 COUNT(*) 进行计数：

```py

SELECT COUNT(*)
FROM startup
WHERE CAST(salary AS DECIMAL) = 800;

```

**最低薪水的三个人：**

我们希望获取初创公司中薪资最低的三名工程师。我们可以通过首先使用 ORDER BY 子句并跟随 ASC，或者仅使用 ORDER BY，这将默认按升序排列输出。

```py

SELECT *
FROM startup
ORDER BY
CAST( salary AS DECIMAL );

```

添加 ‘LIMIT 3’ 将带来前三个对应薪资最低的行。

**薪资最高的三名：**

注意我们做了什么改变！

```py

SELECT *
FROM startup
ORDER BY
CAST( salary AS DECIMAL ) DESC
LIMIT 3;

```

**总薪资：**

假设 CEO 想了解薪资总成本，我们可以使用聚合函数 SUM() 来实现。

```py

SELECT SUM(
CAST( salary AS DECIMAL )
) FROM startup;

```

**向表中添加列：**

假设他现在想雇佣女性，因此他会添加一个名为 sex 的列。这可以通过 ALTER 子句完成：

```py

ALTER TABLE
startup
ADD sex char(1);

```

我们应该这样更新每一行：

```py

UPDATE startup
SET sex = 'M'
WHERE id = 1;

```

**使用 for 循环：**

当然，如果我们手动操作，这会很繁琐。因此，我们应该改用循环。

SQL 没有循环，但可以在过程性语言函数或 ‘Do’ 语句中使用，如 [这里](https://stackoverflow.com/questions/19145761/postgres-for-loop) 所答：

```py

DO
$do$
BEGIN
FOR i IN 2..8 LOOP
UPDATE startup2 set sex = 'M' WHERE id = i;
END LOOP;
END
$do$;

```

**（类似）直方图：**

其中一个可能的要求是了解某事发生的频率。我们可以通过计算每个城市值的每行出现次数来获取城市在初创公司工程师中的频率。这就是我们使用 GROUP BY 子句的原因：

```py

SELECT city, COUNT(*)
FROM startup
GROUP BY city;

```

这类似于直方图；它展示了值出现的频率。

如果我们跟随 AS，可以为任何列命名：

```py

SELECT city, COUNT(*) AS frequency
FROM startup
GROUP BY city;

```

希望你觉得这篇文章有用。如果你想了解我在获得第一个数据科学实习之前的故事，我在 Medium 上写了一篇 [文章](https://medium.com/@ezzeddinabdullah/from-electronics-and-communications-engineering-to-data-science-a373448a783d)。如果你想看到更多更新和挑战，请关注我：

+   Linkedin: [https://www.linkedin.com/in/ezzeddinabdullah/](https://www.linkedin.com/in/ezzeddinabdullah/)

+   Medium: [https://medium.com/@ezzeddinabdullah](https://medium.com/@ezzeddinabdullah)

+   Twitter: [https://twitter.com/EzzEddinAbdulah](https://twitter.com/EzzEddinAbdulah)

+   Quora: [https://www.quora.com/profile/Ezz-El-Din-Abdullah](https://www.quora.com/profile/Ezz-El-Din-Abdullah)

**简历： [Ezz El Din Abdullah](https://www.linkedin.com/in/ezzeddinabdullah/) 是前数据科学实习生和编程导师。**

**相关：**

+   [SQL 备忘单](/2018/07/sql-cheat-sheet.html)

+   [SQL 中的可扩展随机行选择](/2018/04/scalable-select-random-rows-sql.html)

+   [选择 SQL 还是不选择 SQL：这是一个问题！](/2018/05/sql-not-sql-question.html)

* * *

## 我们的前 3 门课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织进行IT管理

* * *

### 主题更多信息

+   [使用Python的自动化机器学习：案例研究](https://www.kdnuggets.com/2023/04/automated-machine-learning-python-case-study.html)

+   [12个建议：从数据分析师到创业公司联合创始人](https://www.kdnuggets.com/2021/12/12-tips-data-analyst-to-co-founder.html)

+   [ChatGPT时代建立深科技创业公司的10个障碍](https://www.kdnuggets.com/2023/04/10-hurdles-building-deep-tech-startup-age-chatgpt.html)

+   [KDnuggets新闻，8月31日：完整的数据科学学习路线图…](https://www.kdnuggets.com/2022/n35.html)

+   [超级学习指南：一本免费的算法与数据结构电子书](https://www.kdnuggets.com/2022/06/super-study-guide-free-algorithms-data-structures-ebook.html)

+   [完整的数据科学学习路线图](https://www.kdnuggets.com/2022/08/complete-data-science-study-roadmap.html)
