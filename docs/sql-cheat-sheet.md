# SQL 速查表

> 原文：[https://www.kdnuggets.com/2018/07/sql-cheat-sheet.html](https://www.kdnuggets.com/2018/07/sql-cheat-sheet.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

![SQL 速查表](../Images/6e35b8c6a9cceb36baf0ef9a0dea61cb.png)

SQL（结构化查询语言）是一种用于编程的领域特定语言，旨在查询数据库。与任何语言一样，拥有常见查询和函数名称的列表作为参考是有用的。我们希望这张速查表对你有所帮助：

### 基本关键字

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行IT工作

* * *

在我们深入了解一些基本常见查询之前，让我们先看看你将会遇到的一些关键字：

| 关键字 | 说明 |
| --- | --- |
| **SELECT** | 用于声明要查询的列。使用 * 表示所有 |
| **FROM** | 声明从哪个表/视图等中选择 |
| **WHERE** | 引入条件 |
| **=** | 用于将值与指定输入进行比较 |
| **LIKE** | 与 WHERE 子句一起使用的特殊运算符，用于在列中搜索特定模式 |
| **GROUP BY** | 将相同的数据安排成组 |
| **HAVING** | 指定仅返回汇总值符合指定条件的行。由于 WHERE 关键字不能与汇总函数一起使用，因此使用此关键字 |
| **INNER JOIN** | 返回所有主键记录在一个表中等于另一个表的主键记录的行 |
| **LEFT JOIN** | 返回“左”（第一个）表中的所有行及其在右（第二个）表中匹配的行 |
| **RIGHT JOIN** | 返回“右”（第二个）表中的所有行及其在左（第一个）表中匹配的行 |
| **FULL OUTER JOIN** | 返回在左表或右表中匹配的行 |

### 汇报汇总函数

在数据库管理中，汇总函数是一种将多行的值分组形成单一值的函数。它们对汇报很有用，一些常见汇总函数的示例如下：

| 函数 | 说明 |
| --- | --- |
| **COUNT** | 返回某个表/视图中的行数 |
| **SUM** | 累加值 |
| **AVG** | 返回一组值的平均值 |
| **MIN** | 返回组中的最小值 |
| **MAX** | 返回组中的最大值 |

### 从表中查询数据

数据库表是以垂直列和水平行的模型存储的数据元素（值）的集合。使用以下任一方法在SQL中查询表：

| SQL | 解释 |
| --- | --- |
| **SELECT c1 FROM t** | 从名为t的表中选择列c1的数据 |
| **SELECT * FROM t** | 从名为t的表中选择所有行和列 |
| **SELECT c1 FROM t****WHERE c1 = ‘test’**  | 从名为t的表中选择列c1的数据，其中c1的值为‘test’ |
| **SELECT c1 FROM t****ORDER BY c1 ASC (DESC)**  | 从名为t的表中选择列c1的数据，并按c1排序，可以是升序（默认）或降序 |
| **SELECT c1 FROM t****ORDER BY c1LIMIT n OFFSET offset**  | 从名为t的表中选择列c1的数据，跳过指定的行偏移量，并返回接下来的n行 |
| **SELECT c1, aggregate(c2)****FROM t****GROUP BY c1**  | 从名为t的表中选择列c1的数据，并使用聚合函数对行进行分组 |
| **SELECT c1, aggregate(c2)****FROM t****GROUP BY c1HAVING condition**  | 从名为t的表中选择列c1的数据，使用聚合函数对行进行分组，并使用‘HAVING’子句筛选这些组 |

### 从多个表中查询数据

除了从单个表中查询数据，SQL还允许从多个表中查询数据：

| SQL | 解释 |
| --- | --- |
| **SELECT c1, c2****FROM t1****INNER JOIN t2 on condition** | 从名为t1的表中选择列c1和c2，并在t1和t2之间执行内连接 |
| **SELECT c1, c2****FROM t1****LEFT JOIN t2 on condition**  | 从名为t1的表中选择列c1和c2，并在t1和t2之间执行左连接 |
| **SELECT c1, c2****FROM t1****RIGHT JOIN t2 on condition**  | 从名为t1的表中选择列c1和c2，并在t1和t2之间执行右连接 |
| **SELECT c1, c2****FROM t1****FULL OUTER JOIN t2 on condition** | 从名为t1的表中选择列c1和c2，并在t1和t2之间执行全外连接 |
| **SELECT c1, c2****FROM t1****CROSS JOIN t2** | 从名为t1的表中选择列c1和c2，并生成表中行的笛卡尔积 |
| **SELECT c1, c2****FROM t1, t2** | 同上 - 从名为t1的表中选择列c1和c2，并生成表中行的笛卡尔积 |
| **SELECT c1, c2****FROM t1 A****INNER JOIN t2 B on condition** | 从名为t1的表中选择列c1和c2，并使用INNER JOIN子句将其连接到自身 |

### 使用SQL操作符

SQL操作符是保留字或字符，主要用于SQL语句中的WHERE子句以执行操作：

| SQL | 解释 |
| --- | --- |
| **SELECT c1 FROM t1****UNION [ALL]****SELECT c1 FROM t2** | 从名为t1的表中选择列c1，并从名为t2的表中选择列c1，将这两个查询的结果合并 |
| **SELECT c1 FROM t1****INTERSECT****SELECT c1 FROM t2** | 从名为t1的表中选择列c1，并从名为t2的表中选择列c1，返回两个查询的交集 |
| **SELECT c1 FROM t1****MINUS****SELECT c1 FROM t2** | 从名为 t1 的表中选择列 c1 和从名为 t2 的表中选择列 c1，并从第一个结果集中减去第二个结果集 |
| **SELECT c1 FROM t****WHERE c1 [NOT] LIKE pattern** | 从名为 t 的表中选择列 c1，并使用模式匹配 % 查询行 |
| **SELECT c1 FROM t****WHERE c1 [NOT] in test_list** | 从名为 t 的表中选择列 c1，并返回在 test_list 中（或不在） 的行 |
| **SELECT c1 FROM t****WHERE c1 BETWEEN min AND max** | 从名为 t 的表中选择列 c1，并返回 c1 在 min 和 max 之间的行 |
| **SELECT c1 FROM t****WHERE c1 IS [NOT] NULL** | 从名为 t 的表中选择列 c1，并检查值是否为 NULL |

### 数据修改

数据修改是 SQL 的关键部分，除了添加和删除数据外，还能修改现有记录：

| SQL | 解释 |
| --- | --- |
| **INSERT INTO t(column_list)****VALUES(value_list)** | 向名为 t 的表中插入一行数据 |
| **INSERT INTO t(column_list)****VALUES (value_list), (value_list), …** | 向名为 t 的表中插入多行数据 |
| **INSERT INTO t1(column_list)****SELECT column_list FROM t2** | 将 t2 中的行插入到名为 t1 的表中 |
| **UPDATE tSET c1 = new_value** | 在表 t 的列 c1 中更新所有行的新值 |
| **UPDATE tSET c1 = new_value, c2 = new_value****WHERE condition** | 更新表 t 中符合条件的行的列 c1 和 c2 的值 |
| **DELETE FROM t** | 从名为 t 的表中删除所有行 |
| **DELETE FROM tWHERE condition** | 从名为 t 的表中删除所有符合某条件的行 |

### 视图

视图是一个虚拟表，是查询的结果。它们非常有用，通常用作安全机制，允许用户通过视图访问数据，而不是直接访问基础表：

| SQL | 解释 |
| --- | --- |
| **CREATE VIEW view1 AS****SELECT c1, c2****FROM t1****WHERE condition** | 创建一个视图，包含来自名为 t1 的表的列 c1 和 c2，其中符合某一条件。 |

### 索引

索引用于通过减少需要访问的数据库页面数量来加速查询性能：

| SQL | 解释 |
| --- | --- |
| **CREATE INDEX index_nameON t(c1, c2)** | 在表 t 的列 c1 和 c2 上创建索引 |
| **CREATE UNIQUE INDEX index_name****ON t(c3, c4)** | 在表 t 的列 c3 和 c4 上创建唯一索引 |
| **DROP INDEX index_name** | 删除一个索引 |

### 存储过程

存储过程是一组带有分配名称的 SQL 语句，这些语句可以被多个程序轻松重用和共享：

| SQL | 解释 |
| --- | --- |
| **CREATE PROCEDURE procedure_name****    @variable AS datatype = value****AS****   -- Comments****SELECT * FROM tGO** | 创建一个名为 procedure_name 的过程，创建一个局部变量，然后从表 t 中选择数据 |

### 触发器

触发器是一种特殊类型的存储过程，当用户尝试通过 DML 事件（数据操作语言）修改数据时，它会自动执行。DML 事件是对表或视图的 INSERT、UPDATE 或 DELETE 语句：

| SQL | 解释 |
| --- | --- |

| **CREATE OR MODIFY TRIGGER trigger_name****WHEN EVENT****ON table_name TRIGGER_TYPE****EXECUTE stored_procedure** | 何时：

+   **BEFORE** – 在事件发生之前调用

+   **AFTER** – 在事件发生之后调用

事件：

+   **INSERT** – 调用以插入

+   **UPDATE** – 调用以更新

+   **DELETE** – 调用以删除

TRIGGER_TYPE：

+   **FOR EACH ROW**

+   **FOR EACH STATEMENT**

|

| **DROP TRIGGER trigger_name** | 删除特定触发器 |
| --- | --- |

**相关：**

+   [PostgreSQL 查询优化的简单技巧](https://www.kdnuggets.com/2018/06/simple-tips-postgresql-query-optimization.html)

+   [如何在 SQL Server 中使用机器学习服务执行 R 和 Python](https://www.kdnuggets.com/2018/06/microsoft-azure-machine-learning-r-python-sql-server.html)

+   [Python 正则表达式速查表](https://www.kdnuggets.com/2018/04/python-regular-expressions-cheat-sheet.html)

### 更多关于此主题

+   [成为一名优秀数据科学家所需的 5 个关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应掌握的 6 种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021 年最佳 ETL 工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [停止学习数据科学以寻找目的，并找到目的...](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [一个 90 亿美元的 AI 失败案例分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [构建一个稳固的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)
