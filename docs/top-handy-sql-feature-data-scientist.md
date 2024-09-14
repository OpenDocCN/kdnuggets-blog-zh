# 数据科学家必备的顶级 SQL 功能

> 原文：[`www.kdnuggets.com/2019/08/top-handy-sql-feature-data-scientist.html`](https://www.kdnuggets.com/2019/08/top-handy-sql-feature-data-scientist.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**由 [Saurabh Hooda](https://www.linkedin.com/in/hoodasaurabh/)，Hackr.io**

![图示](img/7b90faf67d8c5a9b93c5838add6004a0.png)

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

SQL 对数据科学的意义就像自然对生命的重要性一样。没有自然的保护，就没有生命的可能性。没有 SQL 知识的保护，就没有数据科学。

尽管 NoSQL 数据库和像 R 和 Python 这样的编程语言表现非常出色，但每当我们听到“数据”时，首先想到的还是 SQL！Python 确实有优秀的库帮助数据科学家 —— 查看顶级 [数据科学 Python 库](https://hackr.io/blog/top-data-science-python-libraries) —— 但 SQL 仍然是处理数据的首选方式。

*为什么？*

SQL（结构化查询语言）存在已久，对于许多开发者来说，是一种非常舒适的编程语言。SQL 拥有易于学习的功能，用于组织和检索数据，并对数据执行操作以获得有用的见解。你可以阅读一些常见的 [SQL 面试问题](https://hackr.io/blog/top-sql-interview-questions)，以获取关于 SQL 的基本了解。它也是高效且接近数据的。

如果你对数据库一无所知且从未使用过，应该从基础开始。首先阅读关于 DBMS（数据库管理系统）规范化的内容，这将帮助你理解结构对数据分析的重要性。

如果你从事网页开发，你知道数据是从后端（数据库）获取并呈现在前端（UI）。同样，数据是由用户输入到数据库中的。访问 Squareboat 了解我们如何帮助你进行 [网页开发](https://squareboat.com/services/web-development)。

话虽如此，让我们现在关注数据科学家应该掌握的 SQL 顶级实用功能。

### **1\. 选择语句**

作为数据科学家，你将从不同的表中选择（读取）大量数据，以获取模式、统计信息和其他重要信息。基本的选择查询

```py
select * from <table name>;
```

可能返回大量记录。在现实世界中，数据库有数百万条记录，如果一个表中有多个列，你会被结果的数量压倒。你可以只选择你需要的列。

```py
select <column1>, <column2> from <table name>;
```

例如，如果你想知道所有注册学生的姓名和年龄，你可以这样查询数据库 –

```py
select name, age from student;
```

### **2\. 分组和排序函数（排名和 top n 分析）**

大多数时候，你会想要处理你感兴趣的数据子集。例如，如果你想知道使用你产品的青少年数量，你可以通过 where 子句轻松实现。

```py
select name, age from student where age between 13 and 19;
```

同样，你可以根据年龄对学生进行分组。下面的查询将为你提供每个分支/部门的学生数量（计数）。

```py
select count(student_id), deptt from student group by deptt;
```

假设你想知道哪个部门的学生最多或最少。你只需使用 order by 子句即可实现。

```py
select count(student_id), deptt from student group by deptt order by count(student_id) desc;
```

现在，如果我们只想了解那些至少有 10 名学生使用我们产品的部门，我们应该怎么做？

我们可以使用 ‘having’ 子句，它类似于 ‘where’ 子句，但作用于数据组。

```py
select count(student_id), deptt from student group by deptt having count(student_id) >10 order by count(student_id) desc;
```

从一个庞大的数据集中获取这样的有限数量记录称为 top-n 分析。一些现实生活中的例子包括：得分最高的前 2 名学生、花费最多时间浏览的前 10 名 Facebook 用户、公司销售的前 3 种产品、每月的前 5 名员工、每月表现最差的 3 名等。例如 –

```py
select rownum as toppers, first_name, last_name, total_marks
from (select first_name, last_name 
 from student
 order by total_marks)
where rownum<3;
```

第一个 select 语句（内联视图）获取按 total_marks 排序的所有记录，而第 2 个语句仅获取前两个（n）顶级记录。

我们很容易在不进行广泛扫描或搜索的情况下获得数据到精确的过滤级别。我们可以进行很多这样的过滤，以进一步满足更多条件。

### **3\. 字符串函数（文本挖掘）**

SQL 中有许多有用的字符串函数，就像我们在编程语言中一样。当数据库管理系统本身提供这些功能时，你可以避免编写大量代码，因为它更快。一些常见的字符串函数包括 –

*1\. upper 和 lower*

要将整个字符串转换为大写或小写，我们可以使用此函数。这在我们希望以特定大小写打印内容时非常有用。例如，如果我们希望学生的名字全大写，我们可以使用 upper 函数 –

```py
select UPPER(first_name) from student;

```

*2\. replace*

如果我们需要将一个或多个字符替换为另一个字符，可以使用此函数。例如，假设我们需要获取没有中划线的手机号码列表。数据库中的值是 1-832-234-1098，我们想将所有连字符替换为空格。

```py
select first_name, REPLACE(phone_number, ‘-‘, ‘ ‘) as number from student;
```

这将以 1 832 234 1098 的形式呈现值。

*3\. concat*

将两个或多个不同的列或字符串连接成一个结果。例如，如果你想显示全名作为名加姓，可以使用 concat。

```py
select CONCAT(first_name, ‘ ‘, last_name) as fullName from student;
```

*4\. substring*

就像在编程语言中一样，SQL 的子字符串功能从字符串中提取特定的字符。例如，如果考试表有一个 hall_ticket_no（例如 00019812345），而我们只想要最后几个字符，我们可以将查询写成 –

```py
select substring(hall_ticket_no,7,11) as sequence_num from exam;
```

子字符串还有两个变体，左侧和右侧，分别获取左侧或右侧的几个字符。在我们上面的例子中，我们也可以使用 RIGHT 函数。

*5\. len*

获取字符串中的字符数。例如 –

```py
select student_profile from student where len(student_profile) < 25;
```

这将只显示那些描述较短的个人资料。

*6\. ltrim, rtrim*

ltrim 和 rtrim 分别去除字符串的前导和尾随空格。有时数据库条目在开头或结尾有很多空格——可能是用户输入的，或者是程序填充的空格；我们不知道。如果你希望修剪这些空格并展示数据，可以使用这些函数。示例 –

```py
select ltrim(first_name) from student;
```

要去除前导和尾随空格，你可以使用 –

```py
select trim(first_name) from student;
```

### **4\. 日期函数**

处理日期稍显复杂，但 SQL 可以轻松处理。

有许多函数，如

+   **DATEADD** – 在现有日期上增加一年

+   **TO_DATE** – 将字符串转换为日期

+   **DATEDIFF** – 计算两个给定日期之间的差异

+   **DATEPART** – 获取日期的特定部分；例如，年份、月份或日期

+   **DAY** – 获取给定日期的日期

+   **CURRENT_TIMESTAMP** – 获取日期和时间（时间戳）

上述所有内容对于分析各种类型的数据都非常有用。例如，你可以使用 DATEDIFF 来获取特定时间范围内的数据，或者使用 DAY 函数来找出大多数学生请假的日期，等等。

### **5\. 聚合（统计函数）**

聚合函数允许我们从一组数据中找到总和（SUM）、平均值（AVG）、最小值（MIN）、最大值（MAX）和计数（COUNT）。我们使用这些函数与 group by 和 having 子句。例如，如果我们想知道每个部门的学生集体的平均分数百分比，我们可以使用 AVG 函数。

```py
select AVG(total_marks) from students group by deptt;
```

同样地，要获取特定部门的学生数量，我们可以使用 count。

```py
select count(*) from students group by deptt;
```

### 6\. 连接

通常，你会想从多个表中整理数据，并仅获取符合某些条件或模式的列。为了从两个或更多表中获取数据，我们使用 SQL 连接。

### 7\. 正则表达式

了解正则表达式非常有用。例如，如果你想验证电话号码、信用卡或任何符合某种模式的数字值，可以使用正则表达式。

例如，如果你想获取以数字开头的电话号码（而不是括号或其他字符），你可以使用 –

```py
select * from contact where phone like '[0-9]%';
```

你可以使用任何编程语言，如 JavaScript，来完成相同的操作，但它比执行代码更简单、更节省时间。正则表达式也可以用来查找表中某列的特定模式。例如，如果你想获取包含“ya”的学生姓名，你可以使用 LIKE 子句。

```py
select first_name, deptt from student where first_name like ‘%ya%’;
```

### **8\. 加载和复制数据到数据库**

如果你有大量的数据在 Excel 或 .csv 格式中，并且你想将这些数据全部复制到 DBMS 中，SQL 可以为你完成这项工作。

你可以使用 `COPY FROM` 将数据从文件复制到数据库。同样地，要将数据从数据库复制到文件，可以使用 `COPY INTO` 命令。

### **9\. 数据分桶**

正如我们之前看到的，使用 `group by` 可以帮助我们获得数据集，以便我们可以通过分析找到趋势和推断业务机会。分桶是用于查找这些分组（大多数情况下是时间戳和数字）并生成直方图的术语。这减少了人工观察错误。例如，通过使用 `truncate` 函数，我们可以获得一个小数的最接近的四舍五入值。

```py
select truncate (average, 0) from students; -- if the actual value was 78.333, the result will be 78.
```

同样，`date_trunc` 用于将日期分组在一起。这在跟踪用户在一段时间内的活动时可能很有用。例如，跟踪在线学习门户中学生在特定日期区间的活动，例如工作日和周末。

了解更多关于数据分桶的信息，请[点击这里](https://en.wikipedia.org/wiki/Data_binning)。

### **10\. 排序**

使用 SQL 序列，我们可以按需生成一个升序或降序的数字序列。可以通过提供必要的细节使用 `create sequence <sequence_name>` 来创建序列。序列不与任何表关联，因此在需要时直接使用序列来检索表值是有用的。调用 `next value` 函数可以检索下一行，而不是从表中选择。

### **最终总结**

如果你还没有实际操作过 SQL，你应该尝试[这些](https://hackr.io/tutorials/learn-sql)教程。作为一名数据科学家，你可能不会使用 SQL 的所有强大功能。然而，一旦你开始操作数据，你肯定会喜欢深入挖掘。上述列表应该可以帮助你了解作为数据科学家预期使用的功能。请访问[这个链接](https://hackr.io/blog/data-science-interview-questions)阅读所有重要的数据科学面试问题，并开始打造你的梦想职业。

**简介: [Saurabh Hooda](https://www.linkedin.com/in/hoodasaurabh/)** 曾在全球范围内为电信和金融巨头工作。在 Infosys 和 Sapient 工作了十年后，他创办了自己的第一个创业公司 Leno，旨在解决超本地的书籍共享问题。他对产品营销和分析感兴趣。他的最新项目 [Hackr.io](https://hackr.io/) 推荐了每种编程语言的最佳 [数据科学教程](https://hackr.io/tutorials/learn-data-science) 和在线编程课程。所有教程由编程社区提交并投票。

**相关:**

+   数据科学家需要 SQL 吗？

+   掌握数据科学 SQL 的 7 个步骤 — 2019 年版

+   Python 数据科学入门

### 更多相关内容

+   [成为优秀数据科学家所需的 5 项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家都应该掌握的 6 种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021 年最佳 ETL 工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [学习数据科学的顶级统计资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [停止学习数据科学来寻找目的，并通过寻找目的来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [建立一个强大的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)
