# 25个高级SQL面试问题

> 原文：[https://www.kdnuggets.com/2022/10/25-advanced-sql-interview-questions-data-scientists.html](https://www.kdnuggets.com/2022/10/25-advanced-sql-interview-questions-data-scientists.html)

![25个高级SQL面试问题](../Images/d05a8029eb726261d787ff39b89b1ce9.png)

图片由 [Freepik](https://www.freepik.com/free-vector/abstract-technology-sql-illustration_21743432.htm#query=SQL&position=32&from_view=search&track=sph) 提供

# 介绍

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT

* * *

所有数据库使用的标准语言是结构化查询语言（SQL）。

它在从数据库中获取需求数据方面起着关键作用。所有大型数据平台，如Hadoop和Spark，都使用SQL作为其组织的高级语言。

数据科学是唯一的研究和分析数据的领域。数据结果应准确无误，以便进行调查。

这就是SQL如何与数据科学结合的原因。虽然某些组织已转向 [**NoSQL**](/2020/12/nosql-beginners.html)，但基础仍在于SQL。

准备好迎接数据科学面试了吗？

# 25个高级SQL面试问题

这里有一些关于SQL的必要面试问题及答案，你必须学习。

## 1\. 解释规范化是什么？

规范化是一种组织数据库中数据的方法，以最小化冗余。通过使用规范形式的概念对关系进行规范化。

+   1NF：如果一个关系具有原子值，则它处于1NF。

+   2NF：一个关系要处于2NF，它应该处于1NF，并且所有非键属性完全依赖于主键。

+   3NF：一个关系要处于3NF，它应该处于2NF，并且没有过渡依赖关系。

+   BCNF：Boy Codd的规范形式是3NF的强表示。

## 2\. 解释DDL和DML之间的区别？

+   DDL代表数据定义语言，而DML称为数据操作语言。

+   DDL创建一个带有约束的模式。DML在该模式中添加、检索或更新数据。

+   DDL定义表的列属性。DML提供表的行属性。

+   DDL命令的例子包括创建、修改、删除、重命名等，而DML命令的例子包括合并、更新、插入等。

+   WHERE子句不用于DDL，但用于DML。

## 3\. SQL中的ACID属性是什么？

ACID属性如下：

+   原子性 - 数据的更改必须像一个单一的操作一样。

+   一致性 - 数据在事务前后必须保持一致。

+   隔离性 - 多个事务可以在没有任何障碍的情况下进行。

+   持久性 - 事务在系统故障的情况下会成功。

## 4\. 编写一个查询以查找表中所有的重复项？

在一般情况下，查找表中所有重复项的方法如下：

+   使用GROUP BY命令按目标列分组所有行。这里的目标列是你可以检查重复项的列。

+   使用HAVING命令并结合COUNT函数来检查是否有任何组的条目超过一个。

## 5\. 聚集索引和非聚集索引之间的区别是什么？

以下是聚集索引和非聚集索引之间的区别：

+   聚集索引用于定义排序索引或表的顺序，就像字典一样。非聚集索引将所有数据收集到一个地方，并在另一个地方记录它。

+   聚集索引比非聚集索引更快。

+   执行聚集索引操作时使用的内存比非聚集索引少。

## 6\. 解释反规范化？

数据库优化技术中，我们将数据（冗余）添加到表中的方法称为反规范化。该技术有助于减少数据库中昂贵的连接。

## 7\. 什么是排序规则？

在SQL服务器中，排序规则提供了数据的排序规则、重音和大小写敏感性属性。它通过定义排序规则来表示数据库中的每个字符的位模式。

一些排序规则级别如下：

+   大小写敏感 (_CS)

+   重音敏感 (_AS)

+   假名敏感 (_KS)

+   宽度敏感 (_WS)

+   变体选择器敏感 (_VSS)

+   二进制 (_BIN)

+   二进制代码点 (_BIN2)

## 8\. 内连接、外连接和全外连接之间的区别是什么？

以下是内连接、外连接和全外连接之间的区别：

+   在内连接中，两个表中共同的行是输出结果。

+   在全外连接中，返回两个表中的所有行。

+   外连接返回来自一个或两个表的值。

## 9\. 解释连接和联合之间的区别？

以下是连接和联合之间的区别：

+   连接从表中检索匹配的记录，而联合则用于合并两个不同SELECT语句的集合。

+   连接不会删除重复数据，而联合会从选定的语句中删除重复数据（行）。

## 10\. 解释UNION和UNION ALL之间的区别？

联合保留唯一记录，而联合所有保留所有记录，包括所有重复项。此外，联合在返回数据之前会进行去重步骤，而联合所有打印连接结果。

## 11\. 编写一个查询以打印表中最高的薪水？

考虑一个表，数据如下，表名为EMP-

**姓名    薪水**

**---------------------**

约翰     990000

Ellie      100000

Nick      400000

Sam      500000

查询将是：

```py
SELECT * FROM EMP WHERE salary=(select Max(salary) FROM EMP);
```

## 12\. 共享锁和包含锁的区别是什么？

+   在共享锁中，锁模式仅用于读取操作，而在包含锁中，它用于读取和写入操作。

+   共享锁防止其他人更新数据，而包含锁则防止其他人读取或更新数据。

+   任意数量的事务可以对特定项目持有共享锁，而只有一个事务可以持有排他锁。

## 13\. 解释如何使用 SQL 进行转置的机制？

SQL 中的转置被定义为将一行或一列转换为特定格式，以从新的角度可视化数据。

一种基本的转置是将一行转换为一列或将一列转换为一行。典型的转置方法包括动态转置和连接转置。

所有的转置方法都用于数据分析，以获得有益的结果。

## 14\. 解释 B 树索引的工作原理？

B 树被定义为一种自平衡搜索树，其中所有叶子节点都处于同一层级。要在 B 树中插入数据，数据将插入到叶子节点。

在 B 树索引中，树具有一个称为负载的键值对。该值被引用到实际的数据记录中。

当我们使用 B 树索引时，数据库根据 B 树搜索给定的键并获取索引。

## 15\. 解释 TRUNCATE 和 DELETE 对身份的影响？

以下是 DELETE 和 TRUNCATE 命令之间的差异：

+   DELETE 命令用于删除表中的特定行，而 TRUNCATE 删除所有行中的数据。

+   DELETE 是一个 DML 命令，比 TRUNCATE 命令（DDL 命令）慢。

+   DELETE 命令会触发活动，而 TRUNCATE 则不会。

## 16\. 解释拥有数据库索引的成本？

它会占用空间。表越大，索引就越大。

对表中某些行的添加、删除或更新也必须应用到我们的索引上。

一般来说，如果表中被索引的列经常需要使用，则必须在该表上创建索引。

## 17\. 拥有索引的优势是什么？

下面是拥有索引的优势：

+   它加速了 SELECT 查询。

+   数据检索变得相当简单。

+   索引可以用于排序。

+   它使得一行数据唯一，没有任何重复。

+   当索引设置为全文索引时，可以插入大型字符串文本。

## 18\. 解释数据库中的索引？

索引是一种数据结构技术，通过减少查询处理期间的磁盘访问来优化数据库性能。

索引具有以下属性：

+   访问类型：基于搜索类型

+   访问时间：基于搜索时间

+   插入时间：基于快速插入

+   删除时间：基于快速删除

+   空间开销：基于额外的开销空间。

## 19\. 什么是 OLAP 和 OLTP？

### 在线分析处理（OLAP）

企业必须理解一组数据，以帮助财务领导者和业务团队深入了解用户及多个相关因素的表现。

企业可以在业务流程和策略中拥有各种数据，这些数据帮助团队在更细化的层面上分析表现。

这些数据存储在多个数据库系统中，因此需要一致、实时且更灵活的处理过程，帮助团队分析数据并进一步处理以进行更多分析。

在线分析处理（OLAP）包含用于分析业务数据的软件工具，并帮助获得数据库的洞察。

例如，OLAP 可用于构建 Spotify 歌曲推荐引擎，该引擎根据用户的歌曲选择和历史收听数据自动生成播放列表。

### 在线事务处理（OLTP）

事务是每个业务的关键部分。一个提供服务的企业在客户使用服务时会收到一笔费用。

当用户为他们选择的服务付款时，发票会立即生成。

当我们需要存储所有用户的交易信息时，事务数据库就会发挥作用。

但在线事务处理（OLTP）提供了面向事务的应用程序，这些应用程序在 3 层架构中进行分类。

![25 Advanced SQL Interview Questions for Data Scientists](../Images/244348e7d41bbc66c076fe727a82416d.png)

来源: [InterviewBit](https://www.interviewbit.com/sql-interview-questions/)

OLTP 帮助企业执行与事务数据库相关的某些活动。

例如，通过在线银行进行的交易具有相同的目的，其中 OLTP 可以用于操作用户交易信息。

## 20\. CASE WHEN 适用于什么条件？

SQL 用于查询数据库和修改大型数据集中的值。它还提供了一种在多种情况下操作数据的方法。

例如，一个表包含员工列表（name），以及经验（EXP）和加入日期（ac_date）。

现在，如果我们想查询数据，如果经验大于（>）“4”，则在名为“exp_level”的新列中插入“Is a senior”，否则在该列中插入 NULL。

在这种情况下，你可以使用 CASE WHEN 语句查询数据库。

CASE WHEN 语句类似于编程语言中的 If/else 条件，比如 C、C++、[Python](/2022/09/free-python-data-science-course.html) 等。

CASE 语句后面跟随一对 WHEN 和 THEN 语句。然而，你可以根据需要添加嵌套语句。

一旦 CASE 语句完成，就通过添加以 ELSE 和 END AS 开头的结束语句来结束。

在上述示例中，我们有一个员工数据库，列出了公司的员工，包括员工姓名、加入年份、经验等属性。使用 CASE WHEN 语句在这个示例中，

```py
SELECT employee_name,
       exp,
       CASE WHEN exp > 4 THEN 'Is a Senior'
            ELSE NULL END AS exp_level
  FROM xyz.marketing_employees
```

## 21\. 解释 HAVING 与 WHERE 的使用情况？

### SQL 中的 HAVING 子句

SQL 提供了 HAVING 子句来过滤数据库中的数据组。

例如，XYZ 公司有一个员工列表，包含了有超过 1 年经验的员工。

数据库包含行和列，包括员工姓名、入职日期、经验和经验等级。

你可以检索出拥有超过 5 年经验的员工列表。

HAVING 子句为你提供了一种控制信息和修改数据的方式。

HAVING 子句的语法是：HAVING condition。

在这个例子中，我们可以通过以下查询从数据库中检索出拥有超过 5 年经验的员工列表：

```py
SELECT employee_name, exp_level,
FROM EMPLOYEE
GROUP BY experience
HAVING COUNT(experience) > 5;
```

### SQL 中的 WHERE 子句

当你想根据特定条件筛选记录时，可以使用 WHERE 子句。

例如，在员工表中，我们可以检索出在公司中有 exactly 5 年经验的所有员工的列表。WHERE 子句的语法是：WHERE condition。

在这个例子中，我们可以通过以下查询从数据库中提取出拥有 5 年经验的员工姓名：

```py
SELECT employee_name, exp_level,
FROM EMPLOYEE
WHERE experience=  5;
```

## 22\. 什么是 PL/SQL？

PL/SQL 是一种块结构化语言，鼓励开发人员使用过程语句的 SQL 功能。

它是一种功能齐全的过程语言，嵌入了决策能力以及其他许多 POP 特性。

使用单个块命令，PL/SQL 可以在支持广泛错误检查的情况下运行多个查询。

PL/SQL 是 SQL 的扩展，用于创建应用程序。

## 23\. SQL 中的 ETL 是什么？

ETL 代表“提取、转换和加载”。它使用数据仓库的概念来进行数据可视化和数据分析。在更广泛的背景下，它用于数据集成。

任何从一个系统提取数据并存储/复制到另一个目标系统且其数据表示形式不同的过程称为 ETL 过程。

## 24\. 什么是嵌套触发器？

SQL 提供数据操作语言（DML）和数据定义语言（DDL）以在查询数据库时执行某些操作。

当在数据库上执行特定类型的操作时，它会自动在数据库上执行一些操作。

当数据库中触发这些类型的操作时，它们被称为嵌套触发器。

SQL 将嵌套触发器分为两个主要类别——AFTER 触发器和 INSTEAD OF 触发器。

顾名思义，AFTER 触发器是在数据库上执行 DML 或 DDL 操作后执行的。

然而，INSTEAD OF 触发器在 DML 和 DDL 操作中被执行。

## 25\. 什么是提交和检查点？

提交确保数据在当前事务结束后保持一致并维持在更新状态。使用提交时，日志内存中会新增一条记录。

在检查点的情况下，它用于将所有提交的更改写入磁盘，并在控制文件和头文件中更新系统更改编号。

# 结论

在这篇文章中，我们详细讨论了数据科学面试中会问到的所有 SQL 问题。

如果你是初学者，可以从以下资源学习 SQL：

1.  [Matthew Mayo 的 4 小时 SQL 完整课程](/2022/09/free-sql-database-course.html)

1.  [SQL 维基百科](https://en.wikipedia.org/wiki/SQL)

1.  [Scaler 的 SQL 教程](https://www.scaler.com/topics/sql/)

快乐学习！

## 额外资源

1.  [为什么 SQL 将继续是数据科学家的最佳朋友](/2022/07/sql-remain-data-scientist-best-friend.html)

1.  [SQL 数据准备备忘单](/2021/05/data-preparation-sql-cheat-sheet.html)

1.  [掌握数据科学 SQL 的 7 个步骤](/2022/04/7-steps-mastering-sql-data-science.html)

1.  [数据科学面试中你应该知道的五大 SQL 窗口函数](/2022/01/top-five-sql-window-functions-know-data-science-interviews.html)

**[Vaishnavi Amira Yada](https://www.linkedin.com/in/vaishnavi-amira-yada/)** 是一位技术内容撰稿人。她掌握 Python、Java、数据结构与算法（DSA）、C 等知识。她发现自己对写作情有独钟，并且热爱写作。

### 更多相关话题

+   [你必须了解的 10 个高级数据科学 SQL 面试题…](https://www.kdnuggets.com/2023/01/top-10-advanced-data-science-sql-interview-questions-must-know-answer.html)

+   [数据科学中的 3 个 SQL 聚合函数面试题](https://www.kdnuggets.com/2023/01/3-sql-aggregate-function-interview-questions-data-science.html)

+   [数据分析师的 SQL 和 Python 面试题](https://www.kdnuggets.com/2023/02/sql-python-interview-questions-data-analysts.html)

+   [经验丰富的专业人士 SQL 面试题](https://www.kdnuggets.com/2022/01/sql-interview-questions-experienced-professionals.html)

+   [你可能会在下次面试中遇到的 24 个 SQL 问题](https://www.kdnuggets.com/2022/06/24-sql-questions-might-see-next-interview.html)

+   [KDnuggets 新闻，5 月 4 日：学习数据的 9 门免费哈佛课程…](https://www.kdnuggets.com/2022/n18.html)
