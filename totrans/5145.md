# 数据科学中你应该了解的 7 个 SQL 概念

> 原文：[https://www.kdnuggets.com/2022/11/7-sql-concepts-needed-data-science.html](https://www.kdnuggets.com/2022/11/7-sql-concepts-needed-data-science.html)

![你应该了解的 7 个 SQL 概念](../Images/184746f937e157a2a5362420e31a7325.png)

编辑提供的图片

# 介绍

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

随着世界向数字化发展，大多数公司现在都是数据驱动的。他们收集的大量数据存储在数据库中。这些数据的管理、分析和处理通过数据库管理系统（DBMS）完成。由于这种转变，数据科学成为了一个极具前景的领域，提供了无数的就业机会。数据科学家需要从数据库中提取数据，这就是 SQL 发挥作用的地方。你一定听说过掌握数据科学领域的顶级技能，而 SQL 就是其中之一。现在，问题是：**作为一个优秀的数据科学家，我真的需要精通 SQL 吗？**

答案是否定的，但掌握 SQL 的基本知识是必要的，因为它已成为许多数据库系统的标准。这篇文章旨在提及你必须了解的 SQL 关键要素，并获得数据科学从业者的推荐。

# 数据科学中 SQL 的需求

SQL 代表结构化查询语言，旨在管理关系数据库。让我们首先理解 SQL 在数据科学中的需求。是什么使它独特，并成为数据科学中最受追捧的技能之一？以下是一些帮助你理解其重要性的要点：

+   **广泛使用：** 尽管 SQL 约有 40 年的历史，但它仍用于大多数关系数据库系统中，并且已成为实验数据的标准工具。

+   **简化数据理解：** SQL 在浏览数据库内容时非常方便。它使你能够有效地理解数据的特性。

+   **易于学习：** 这是初学者的完美起点，语法类似于简单的英语，你只需几行代码即可提取有价值的见解。

+   **支持大数据处理：** SQL 使你能够以有组织的方式管理大量数据，使其成为数据科学应用的理想选择。

+   **与其他编程语言和应用程序的兼容性**：将 SQL 与 Python、C++、R 等语言集成非常方便。它还支持商业智能和数据可视化工具，如 Power BI 和 Tableau，使开发过程变得稍微容易一些。

# 七个 SQL 概念

## 1) 基本命令的理解

基本命令的知识构建了终身学习的基础。否则，你将只是记忆事实，而不了解它们如何相互关联。一些常用的 SQL 命令如下：

+   **SELECT & FROM**：从提到的表中检索数据的属性。

+   **SELECT DISTINCT**：它消除重复的行，只显示唯一的记录。

+   **WHERE**：它过滤记录，只显示满足给定条件的记录。

+   **AND, OR, NOT**：当条件不为真时不执行查询。而 AND 和 OR 用于应用多个条件。

+   **ORDER BY**：它将数据按升序或降序排序

+   **GROUP BY**：它将相同的数据分组。

+   **HAVING**：通过 GROUP BY 聚合的数据可以在这里进一步筛选。

+   **聚合函数**：聚合函数如 COUNT()、MAX()、MIN()、AVG() 和 SUM() 用于对给定数据进行操作。

让我们以 Employee 表为例进行应用，

| **ID** | **Name** | **Department** | **Salary ($)** | **Gender** |
| --- | --- | --- | --- | --- |
| 1 | Julia | Admin | 20000 | F |
| 2 | Jasmine | Admin | 15000 | F |
| 3 | John | IT | 20000 | M |
| 4 | Mark | Admin | 17000 | M |

现在，我们想要获取在 Admin 部门工作的女性的平均薪资。

```py
SELECT Department,
       AVG(Salary)
FROM Employees
WHERE Gender="F"
GROUP BY Department
HAVING Department = "Admin";
```

**输出：**

```py
Admin | 17500.0
```

## 2) Case When

这是 SQL 中一个非常强大和灵活的语句，用于编写复杂的条件语句。它提供了 IF.THEN.ELSE 语句的功能。我们来看看它的语法，

```py
CASE expression

   WHEN value_1 THEN result_1
   WHEN value_2 THEN result_2
   ...
   WHEN value_n THEN result_n

   ELSE result

END
```

它按顺序执行语句，并在条件变为真时立即返回值。如果没有满足条件，则执行 ELSE 块，如果没有 ELSE 块，则返回 NULL。

假设我们有一个学生数据库，我们想根据他们的分数进行评分，则可以使用以下 SQL 语句：

```py
SELECT student_name,
       marks,
       CASE
           WHEN marks >= 85 THEN 'A'
           WHEN marks >= 75
                AND marks < 85 THEN 'B+'
           WHEN marks >= 65
                AND marks < 75 THEN 'B'
           WHEN marks >= 55
                AND marks < 65 THEN 'C'
           WHEN marks >= 45
                AND marks < 55 THEN 'D'
           ELSE 'F'
       END AS grading
FROM Students;
```

## 3) 子查询

作为数据科学家，对子查询的了解至关重要，因为他们需要处理不同的表，并且一个查询的结果可能会被再次使用，以进一步限制主查询中的数据。这也被称为嵌套查询或内查询。子查询必须用括号括起来，并且在主查询之前执行。如果返回多于一行，则称为多行子查询，必须与多行操作符一起使用。

假设保险公司引入了一项新政策，取消了年龄超过 80 岁的人的保险。这可以通过如下的子查询来完成：

```py
DELETE
FROM INSURANCE_CUSTOMERS
WHERE AGE IN
    (SELECT AGE
     FROM INSURANCE_CUSTOMERS
     WHERE AGE > 80 );
```

内部子查询选择所有年龄超过 80 岁的客户，然后在这一组上执行删除操作。

## 4) 连接

SQL连接用于根据表之间的逻辑关系组合多个表中的行。以下列出了4种SQL连接类型：

+   **内连接**：内连接仅显示两个表中满足给定条件的行。在集合术语中，它可以被视为交集。![7 SQL Concepts You Should Know For Data Science](../Images/353f4ae304e4f1ffbdad35966db409a8.png)

```py
SELECT Student.Name
FROM Student
INNER JOIN Sports ON Student.ID = Sports.ID;
```

它返回那些已注册参加体育活动的学生。注意：体育ID与学生的注册ID相同。

+   **左连接**：它返回LEFT表中的所有记录，而右表中仅显示匹配的记录。![7 SQL Concepts You Should Know For Data Science](../Images/87f10f0ca92cdd6ffb46cd6f3d5ba1f6.png)

```py
SELECT Student.Name
FROM Student
LEFT JOIN Sports ON Student.ID = Sports.ID;
```

+   **右连接**：它与左连接的作用正好相反。![7 SQL Concepts You Should Know For Data Science](../Images/4944ef7dac0a63a8eadbd350896d0cb7.png)

```py
SELECT Student.Name
FROM Student
RIGHT JOIN Sports ON Student.ID = Sports.ID;
```

+   **全外连接**：它包含了两个表中的所有行，如果没有对应的匹配项，则显示为NULL值。![7 SQL Concepts You Should Know For Data Science](../Images/9678706ff9db213a3437d3beba754ec5.png)

```py
SELECT Student.Name
FROM Student
FULL JOIN Sports ON Student.ID = Sports.ID;
```

## 5) 存储过程

存储过程允许我们在数据库中存储多个SQL语句以便后续使用。它支持重用，并且在调用时还可以接受参数值。它提高了性能，并且修改起来更加容易。

```py
CREATE PROCEDURE SelectStudents @Major nvarchar(30),
                                       @Grade char(1) AS
SELECT *
FROM Students
WHERE Major = @Major
  AND Grade = @Grade GO;

EXEC SelectStudents @Major = 'Data Science',
                    @Grade = 'A';
```

这个过程允许我们根据学生的成绩提取不同专业的学生。例如，我们试图提取所有在数据科学专业中获得A等级的学生。请注意，CREATE PROCEDURE类似于函数声明，需要通过EXEC调用以进行执行。

## 6) 字符串格式化

我们都知道，原始数据需要清洗，以提高整体生产力，从而做出更高质量的决策。在这个背景下，字符串格式化发挥了重要作用，它涉及到操作字符串以去除无关的内容。SQL提供了广泛的字符串函数来转换和处理字符串。其中最常用的5个如下：

+   **CONCAT:** 用于将两个或更多字符串连接在一起。

```py
SELECT CONCAT(Name, ' has a major of  ', Major)
FROM Students
WHERE student_Id = 37; 
```

+   **SUBSTR:** 它返回字符串的一部分，并在参数中接收起始位置和要返回的子字符串的长度。

```py
SELECT student_name,admission_date,
     SUBSTR(admission_date, 4, 2) AS day
FROM Students 
```

日期列将会从admission_date中单独提取出来。

+   **TRIM:** trim的主要功能是去除字符串开头、结尾或两者的字符（如果指定的话）。你必须指定开头、结尾或两者，然后是要移除的字符，再跟上要移除的字符串。

```py
SELECT age,
   TRIM(trailing ' years' FROM age)
FROM Students 
```

它将把“26 years”改为“26”。

+   **INSERT:** 它允许我们在指定的位置插入字符串到给定字符串中。你需要指定新子字符串的位置和长度。请注意，这个新字符串将覆盖之前的文本。

```py
SELECT INSERT("OldWebsite.com", 1, 9, "NewWebsite"); 
```

将更新到 NewWebsite.come。

+   **COALESCE：** 它可以用来用用户定义的值替换空值，这在数据科学中经常需要。

```py
SELECT COALESCE (NULL, NULL, 10, 'John’')
```

这将返回 10。

## 7) 窗口函数

窗口函数类似于聚合函数，但它不会在计算后使行合并成一行。相反，行保持其独立的身份。它们分为三个主要类别：

+   **聚合函数：** 它展示了来自数值列的汇总值，如 AVG()、COUNT()、MAX()、MIN()、SUM() 等。

```py
SELECT name,
       AVG(salary) over (PARTITION BY department) 
FROM Employees; 
```

它展示了来自员工表的不同部门的平均工资。

+   **值函数：** 每个分区使用值窗口函数分配一些值。一些常用的值函数包括 LAG()、LEAD()、FIRST_VALUE()、LAST_VALUE() 和 NTH_VALUE()。

```py
SELECT 
	 bank_branch, month, income,
	LAG(income,1) OVER (
		PARTITION BY bank_branch
		ORDER BY month
	) income_next_month
FROM Bank; 
```

我们比较了银行不同分支机构本月与上月的收入。

+   **排名函数：** 它们用于根据某些预定义的排序给行分配排名。ROW_NUMBER()、RANK()、DENSE_RANK()、PERCENT_RANK()、NTILE() 是其中的一些。

```py
SELECT
	product_name, price,
	RANK () OVER ( 
		ORDER BY list DESC
	) price_hightolow
FROM Products; 
```

产品按价格使用 RANK() 进行排名。

# 结论

我希望你喜欢阅读这篇文章，它为你提供了作为数据科学家需要了解多少 SQL 的全面理解。如果你想深入探讨这些概念，这里有一些资源：

[SQLServertutorial](https://www.sqlservertutorial.net/)

[TutorialsPoint](https://www.sqlservertutorial.net/)

[W3Schools](https://www.w3schools.com/sql/)

**[Kanwal Mehreen](https://www.linkedin.com/in/kanwal-mehreen1)** 是一位有抱负的软件开发者，对数据科学和人工智能在医学中的应用充满兴趣。Kanwal 被选为 2022 年亚太地区的 Google Generation Scholar。Kanwal 喜欢通过撰写有关热门话题的文章来分享技术知识，并且热衷于提高女性在科技行业中的代表性。

### 更多相关话题

+   [关于梯度下降和代价函数的 5 个概念](https://www.kdnuggets.com/2020/05/5-concepts-gradient-descent-cost-function.html)

+   [在进入 Transformers 之前你应该了解的概念](https://www.kdnuggets.com/2023/01/concepts-know-getting-transformer.html)

+   [那些不那么性感的 SQL 概念让你脱颖而出](https://www.kdnuggets.com/2022/02/not-so-sexy-sql-concepts-stand-out.html)

+   [KDnuggets™ 新闻 22:n03, 1月19日：深入探讨 13 个数据…](https://www.kdnuggets.com/2022/n03.html)

+   [数据科学面试中你应该了解的五大 SQL 窗口函数](https://www.kdnuggets.com/2022/01/top-five-sql-window-functions-know-data-science-interviews.html)

+   [KDnuggets 新闻，4月13日：数据科学家应该了解的 Python 库…](https://www.kdnuggets.com/2022/n15.html)
