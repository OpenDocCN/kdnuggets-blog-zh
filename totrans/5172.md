# 数据科学面试中的最常见 SQL 错误

> 原文：[https://www.kdnuggets.com/2021/11/common-sql-mistakes-data-science-interviews.html](https://www.kdnuggets.com/2021/11/common-sql-mistakes-data-science-interviews.html)

[评论](#comments)

![](../Images/c43d8e46ee83bd41246ce4db2f52d87d.png)

这些错误涉及到在 [数据科学编程面试问题](https://www.stratascratch.com/blog/data-science-coding-interview-questions-with-5-technical-concepts/) 中经常出现的概念，这些概念可能会导致你的面试失败，因此了解如何避免它们以及如何纠正它们非常重要。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 工作

* * *

### 错误 1: LIMIT 用于最大值/最小值

可能最常见的错误与在数据集中查找最高或最低值的记录有关。这听起来是一个简单的问题，但由于 SQL 的工作方式，我们通常不能简单地使用 MAX 或 MIN 函数。相反，我们需要设计另一种方法来输出相关的行。在这样做时，有一种常用的方法在逻辑上似乎是正确的，更糟糕的是，它经常产生预期的结果。问题在于这种解决方案没有考虑所有可能性，并跳过了一些重要的边缘情况。让我们看一个例子来理解这个错误。

![](../Images/163f3659d0e8283a035e890259544add.png)

*链接: [https://platform.stratascratch.com/coding/10353-workers-with-the-highest-salaries](https://platform.stratascratch.com/coding/10353-workers-with-the-highest-salaries)*

这个问题来源于 DoorDash 数据科学家职位的实际面试。我们需要在员工和他们的职位数据集中找到薪资最高的职位名称。一个常见且可能是最简单的解决方案如下：

```py
SELECT t.worker_title
FROM worker AS w
LEFT JOIN title AS t ON w.worker_id = t.worker_ref_id
ORDER BY w.salary DESC
LIMIT 1

```

我们需要合并两个表，因为列 worker_title 来自一个数据集，而列 'salary 来自另一个，但这不是我们应该关注的部分。其余的代码是一种相当直接的方式来返回具有最高值的行。我们选择要返回的列，按某个值降序排列表格，并仅通过使用 LIMIT 1 输出第一行。你能看到这个解决方案的问题吗？

在许多情况下，使用这种方法会产生预期的输出，但问题是它忽略了一个重要且常见的边界情况。也就是说，如果数据中存在并列怎么办？如果两行有相同的最高值怎么办？正如你在这个例子中看到的，这个解决方案被标记为不正确，因为实际上有两个职位的薪资相同，而且还是最高薪资。使用 LIMIT 语句要求我们知道多少行共享最高值，而在大多数情况下，我们不知道，因此这种方法是不正确的。

如何纠正这个解决方案？实际上，有多种方法可以以稳健的方式找到具有最大或最小值的行。一种方法是使用一个子查询，在其中找到感兴趣的值，然后在主查询中使用它作为过滤条件。在我们的例子中，我们可以保留前三行代码，但将 ORDER BY 和 LIMIT 语句替换为 WHERE 子句。在其中，我们要告诉引擎只留下那些“薪资”列等于整个数据集最高薪资的行。在 SQL 中，这看起来是这样的：

```py
SELECT t.worker_title
FROM worker AS w
LEFT JOIN title AS t ON w.worker_id = t.worker_ref_id
WHERE w.salary IN
    (SELECT max(salary)
     FROM worker)

```

在 WHERE 子句中，有一个子查询只返回一个值——最高薪资。然后我们使用这个数字来过滤数据集，只输出薪资等于该数字的行。

另一种常见的方法是使用窗口函数 RANK() 添加一个新的列，以根据职位薪资进行排名。

```py
SELECT t.worker_title,
          RANK() OVER (
                       ORDER BY w.salary DESC) AS rnk
   FROM worker AS w
   LEFT JOIN title AS t ON w.worker_id = t.worker_ref_id

```

如你所见，RANK() 函数考虑了结果中的并列情况，因为在这里，两个职位的薪资相同，它们都被赋予了第一名。然后我们可以将这个排名用作内部查询，并添加主查询，只留下排名等于 1 的这些职位。

```py
SELECT worker_title
FROM
  (SELECT t.worker_title,
          RANK() OVER (
                       ORDER BY w.salary DESC) AS rnk
   FROM worker AS w
   LEFT JOIN title AS t ON w.worker_id = t.worker_ref_id) a
WHERE rnk = 1

```

### 错误 2：Row_Number() 与 Rank() 与 Dense_Rank()

![](../Images/96c1aac0a09f342759de1f3fd8cfb24e.png)

*链接: [https://platform.stratascratch.com/coding/10062-fans-vs-opposition](https://platform.stratascratch.com/coding/10062-fans-vs-opposition)*​

说到 RANK() 和窗口函数，常见的一个错误与它们有关。看看上面 Facebook 的 [SQL 面试问题](https://www.stratascratch.com/blog/sql-interview-questions-you-must-prepare-the-ultimate-guide/)。这里 Facebook 说他们对员工进行了调查，以量化一些新编程语言的受欢迎程度。现在他们希望将最喜欢它的人与最讨厌它的人匹配。最大的粉丝与最大的反对者配对，第二大的粉丝与第二大的反对者配对，以此类推。

尽管这个面试问题很长且看起来很难，但解决方案并不特别复杂。我们可以简单地将数据集按受欢迎程度降序排序，然后再按升序排序，将这两张表放在一起，或者用技术术语说，合并它们。解决方案可能是这样的：

```py
SELECT fans.employee_fan_id,
      opposition.employee_opposition_id
FROM
  (SELECT employee_id AS employee_fan_id,
          RANK() OVER (
                             ORDER BY s.popularity DESC) AS position
  FROM  facebook_hack_survey s ) fans
INNER JOIN
  (SELECT employee_id AS employee_opposition_id,
          RANK() OVER (
                             ORDER BY s.popularity ASC) AS position
  FROM  facebook_hack_survey s ) opposition 
  ON fans.position = opposition.position

```

这个解决方案的关键部分是这个 ‘INNER JOIN’ 语句两边的两个子查询。它们返回相同的表，但排序方式相反。为了将这些表合并在一起并创建匹配，SQL 中最简单的方法是根据这些排序方式给员工分配连续的编号，并将这些编号匹配在一起。因此，最受欢迎的粉丝将得到编号 1，并与在 ‘opposition’ 表中也得到编号 1 的最大对手匹配。

但当我们运行这段代码时，出现了问题。从结果的顶部可以看到，员工 17 与员工 13 和 2 都配对了。更有甚者，相同的员工 13 和 2 也与员工 5 配对了。然后再次与员工 8 配对。显然有什么地方出错了。让我们修改 SELECT 子句以显示排名。

```py
SELECT fans.employee_fan_id,
      opposition.employee_opposition_id,
      Fans.position AS position_fans,
      Opposition.position AS position_opp
FROM
  (SELECT employee_id AS employee_fan_id,
          RANK() OVER (
                             ORDER BY s.popularity DESC) AS position
  FROM  facebook_hack_survey s ) fans
INNER JOIN
  (SELECT employee_id AS employee_opposition_id,
          RANK() OVER (
                             ORDER BY s.popularity ASC) AS position
  FROM  facebook_hack_survey s ) opposition 
  ON fans.position = opposition.position

```

这里发生了什么？似乎调查中有得分相同的员工。换句话说，数据中存在并列的情况。正如你可能还记得之前的错误，RANK() 函数会给所有具有相同值的行分配相同的分数。所以在这里，员工 17、5 和 8 都得到了相同的最高人气分数，而员工 13 和 2 则得到了相同的最低分数。这就是为什么这五个人在所有可能的组合中都被匹配在一起。但是，如果只有 13 和 2 共享了相同的最低分数，那么本应有一个排名为 3 的对手员工。然而，由于没有排名为 3 的粉丝，他们没有与任何人匹配。

如何解决这个问题？有些人可能会说，如果 RANK() 函数不起作用，我们可以尝试用 DENSE_RANK() 替换它。这两者的区别在于处理并列的方式。如果我们有四个值，其中前两个相同，那么 RANK() 会给它们分配排名 1、1、3 和 4。与此同时，DENSE_RANK() 会将它们排名为 1、1、2 和 3。但在这种情况下这是正确的解决方案吗？

```py
SELECT fans.employee_fan_id,
      opposition.employee_opposition_id,
      fans.position as position_fans,
      opposition.position as position_opp
FROM
  (SELECT employee_id AS employee_fan_id,
          DENSE_RANK() OVER (
                             ORDER BY s.popularity DESC) AS position
  FROM  facebook_hack_survey s ) fans
INNER JOIN
  (SELECT employee_id AS employee_opposition_id,
          DENSE_RANK() OVER (
                             ORDER BY s.popularity ASC) AS position
  FROM  facebook_hack_survey s ) opposition 
  ON fans.position = opposition.position

```

我们解决了员工 10 之前没有与任何人匹配的问题，但其他问题仍然存在。DENSE_RANK() 仍然对具有相同值的行给予相同的排名，因此仍然有一些员工与多于一个人配对。那么正确的解决方案是什么呢？有一个叫做 ROW_NUMBER() 的类似窗口函数。有人认为它“基础”是因为它只是计数行而不处理并列值。但这正是使这个函数在这种情况下完美的特性。

```py
SELECT fans.employee_fan_id,
      opposition.employee_opposition_id,
      fans.position as position_fans,
      opposition.position as position_opp
FROM
  (SELECT employee_id AS employee_fan_id,
          ROW_NUMBER() OVER (
                             ORDER BY s.popularity DESC) AS position
  FROM  facebook_hack_survey s ) fans
INNER JOIN
  (SELECT employee_id AS employee_opposition_id,
          ROW_NUMBER() OVER (
                             ORDER BY s.popularity ASC) AS position
  FROM  facebook_hack_survey s ) opposition 
  ON fans.position = opposition.position

```

现在所有员工都被赋予了唯一的排名，因此可以在不重复和不遗漏任何人的情况下进行配对。虽然在数据科学面试中，粉丝与对手配对的问题并不是最常见的，但基于这个例子，我们想展示一下理解 RANK()、DENSE_RANK() 和 ROW_NUMBER() 之间的区别有多么重要——它们都是非常相似的 [窗口函数](https://www.stratascratch.com/blog/types-of-window-functions-in-sql-and-questions-asked-by-airbnb-netflix-twitter-and-uber/)。

### 错误 3：引号中的别名

![](../Images/c1299e21f58af834d8d60e89fc46e5f1.png)

在这个例子中，我们给一些子查询和列命名，如‘fans’和‘opposition’。这些是所谓的别名，在SQL中非常流行，但也是一些常见错误的来源。让我们换个简单的例子来看一下这些错误。

![](../Images/fa25d424331eee091f4e873c3ccf5a9c.png)

*链接: [https://platform.stratascratch.com/coding/2061-users-with-many-searches?python=](https://platform.stratascratch.com/coding/2061-users-with-many-searches?python=)*​

在这个最近的数据科学面试问题中，Facebook要求根据某些搜索数据库计算2021年8月进行过超过五次搜索的用户数量。解决方案如下：

```py
SELECT count(user_id) AS result
FROM
  (SELECT user_id,
          count(search_id) AS "AugustSearches"
   FROM fb_searches
   WHERE date::date BETWEEN '2021-08-01' AND '2021-08-31'
   GROUP BY user_id) a
WHERE AugustSearches > 5

```

在内部查询中，我们统计了每个用户在所需时间框架内的搜索次数，然后在外部查询中，我们只考虑进行过超过五次搜索的用户，并对他们进行计数。但是，当我们尝试运行时，出现了错误。它说‘*列 "augustsearches" 不存在*’。这怎么可能呢？毕竟，我们在内部查询中将别名赋予了‘AugustSearches’，所以我们应该可以在外部查询的WHERE子句中使用它，对吗？是的，但不幸的是，我们在分配别名时犯了一个错误。你能看出来吗？修复它的最简单方法是去掉别名中的引号：

```py
SELECT count(user_id) AS result
FROM
  (SELECT user_id,
          count(search_id) AS AugustSearches
   FROM fb_searches
   WHERE date::date BETWEEN '2021-08-01' AND '2021-08-31'
   GROUP BY user_id) a
WHERE AugustSearches > 5

```

这是一个常见错误，因为我们在SQL中使用引号来书写字符串，而别名感觉像是字符串。但实际上，这个问题起因于其他地方，涉及到大写字母。为了节省内存，SQL总是将列名转换为小写字母。所以即使我们写‘WHERE AugustSearches’，SQL也会将其解释为‘WHERE augustsearches’。但当我们在引号中定义别名时，SQL保留所有的大写字母，但当我们将别名与不带引号的别名进行比较时，会导致问题。理论上，我们可以始终使用带引号的别名：

```py
SELECT count(user_id) AS result
FROM
  (SELECT user_id,
          count(search_id) AS "AugustSearches"
   FROM fb_searches
   WHERE date::date BETWEEN '2021-08-01' AND '2021-08-31'
   GROUP BY user_id) a
WHERE “AugustSearches” > 5

```

这段代码会运行，但计算时间会更长，消耗更多内存，并可能造成一些混淆。我们需要记住每次使用别名时都要加上引号。因此这里的教训是不使用别名，并且只使用小写字母作为别名。

```py
SELECT count(user_id) AS result
FROM
  (SELECT user_id,
          count(search_id) AS august_searches
   FROM fb_searches
   WHERE date::date BETWEEN '2021-08-01' AND '2021-08-31'
   GROUP BY user_id) a
WHERE august_searches > 5

```

### 错误4：别名的不一致性

![](../Images/163f3659d0e8283a035e890259544add.png)

*链接: [https://platform.stratascratch.com/coding/10353-workers-with-the-highest-salaries](https://platform.stratascratch.com/coding/10353-workers-with-the-highest-salaries)*​

另一个常见的问题是别名使用的不一致。这不是一个重大错误，只要不同表中没有相同的列名，它不会造成主要问题。但这是一个可以决定你的数据科学面试成功与否的细节。让我们回到开始的问题，即关于最高收入职位的那个。这是另一个可能的正确解决方案：

```py
SELECT t.worker_title
FROM worker w
LEFT JOIN title t ON w.worker_id = t.worker_ref_id
WHERE salary =
    (SELECT MAX(salary)
     FROM worker w
     LEFT JOIN title t ON w.worker_id = t.worker_ref_id)
ORDER BY worker_title ASC

```

你能看到别名的问题吗？首先，我们为两个表定义了别名。在这种情况下，是字母 ‘w’ 和 ‘t’。然后我们说 ‘t.worker_title’，但为什么在 WHERE 子句中 ‘salary’ 没有任何别名？为什么 ‘worker_title’ 列在 ORDER BY 子句中突然没有别名？再看看子查询：我们为表指定了别名，但在选择这个查询中的唯一列时没有使用它们。另一个问题是内外查询中使用了相同的别名，但由于在这种情况下它们总是用于相同的表，我们可以忽略。保持一致地使用别名，这个解决方案会显得更加干净和清晰。

```py
SELECT t.worker_title
FROM worker w
LEFT JOIN title t ON w.worker_id = t.worker_ref_id
WHERE w.salary =
    (SELECT MAX(w.salary)
     FROM worker w
     LEFT JOIN title t ON w.worker_id = t.worker_ref_id)
ORDER BY t.worker_title ASC

```

### 错误 5: 不必要的 JOIN

但我们还没有完全解决问题。这个解决方案还有一个错误。是的，它仍然有效并产生预期结果，但我们不能说它是高效的。你能看到为什么吗？看看内层查询。在其中，我们只选择了一列，salary。这个列来自于一个名为‘worker’的表。现在我们已经修复了别名，这一点尤为清晰。但如果是这种情况，那我们为什么在内层查询中合并这两个表呢？这并不必要，并且使得解决方案的效率降低。毕竟，每次 JOIN 操作都需要一些计算时间，如果表很大，这个时间可能会很可观。我们知道，当问题涉及多个表时，逻辑上的第一步是将它们合并在一起。但正如这个例子所示，我们应该仅在必要时才这样做。

```py
SELECT t.worker_title
FROM worker w
LEFT JOIN title t ON w.worker_id = t.worker_ref_id
WHERE w.salary =
    (SELECT MAX(w.salary)
     FROM worker w
ORDER BY t.worker_title ASC

```

### 错误 6: 在包含 NULL 值的列上进行 JOIN

![](../Images/2b82ad2a07c716522a58e9548ee991a9.png)

*链接: [https://platform.stratascratch.com/coding/9627-3-bed-minimum](https://platform.stratascratch.com/coding/9627-3-bed-minimum)*

让我们再展示一个常见的错误，同样与 JOIN 语句有关。看看这个问题，Airbnb 要求找出每个至少有三个床的邻里中的平均床数。虽然存在更简单的解决方案，但一种有效的方法是将原始表与自身合并，但以汇总的方式，这样我们就已经得到每个邻里的床数，前提是床数至少为三个。我们可以通过子查询或像这个解决方案中的 JOIN 语句来实现：

```py
SELECT anb.neighbourhood,
       avg(anb.beds) AS avg_n_beds
FROM airbnb_search_details AS anb
RIGHT JOIN
  (SELECT neighbourhood,
          sum(beds)
   FROM airbnb_search_details
   GROUP BY 1
   HAVING sum(beds)>=3) AS fil_anb 
ON anb.neighbourhood = fil_anb.neighbourhood
GROUP BY 1
ORDER BY avg_n_beds DESC

```

当我们运行这段代码时，乍一看还不错，但顶部的这个空白是什么？当我们将输出与预期结果进行比较时，发现我们的代码不正确。那么发生了什么？让我们仅运行内层查询以寻找线索。

```py
SELECT neighbourhood,
          sum(beds)
   FROM airbnb_search_details
   GROUP BY 1
   HAVING sum(beds)>=3

```

你能看到这一行吗？结果显示有 33 张床没有分配到任何邻里。相反，邻里的值为‘NULL’。为什么这会造成问题？因为在我们的主要查询中，我们在邻里列上对两个表进行了 JOIN - 这两个列都包含 NULL 值。而在 SQL 中，如果我们在 NULL 值之间使用操作符‘=’，引擎将无法正确匹配它们。

如何解决这个问题？如果我们确实需要基于包含 NULL 值的列合并两个表，我们需要明确告诉引擎如何处理它们。因此，我们可以像之前一样开始，写下‘anb.neighbourhood = fil_anb.neighbourhood’，然后继续添加第二种情况：如果 anb.neighbourhood 中的值为 NULL，则 fil_anb.neighbourhood 中的值也应为‘NULL’。在代码中，它看起来像这样：

```py
SELECT anb.neighbourhood,
       avg(anb.beds) AS avg_n_beds
FROM airbnb_search_details AS anb
RIGHT JOIN
  (SELECT neighbourhood,
          sum(beds)
   FROM airbnb_search_details
   GROUP BY 1
   HAVING sum(beds)>=3) AS fil_anb 
ON (anb.neighbourhood= fil_anb.neighbourhood) or (anb.neighbourhood is NULL 
and fil_anb.neighbourhood is NULL)
GROUP BY 1
ORDER BY avg_n_beds DESC

```

### 结论

这些就是我们为你准备的所有示例。我们展示了不同的编码错误，有的更严重，有的较轻，但都同样重要，特别是在面试环境中。在那里，不仅是解决方案是否有效，还有它的书写方式以及你作为候选人是否注重细节。如果你记住这些最常见的错误，并在未来尽量避免它们，这将提高你在[data science interview](https://www.stratascratch.com/blog/data-science-interview-guide-questions-from-80-different-companies/)中成功的机会！

[原文](https://www.stratascratch.com/blog/most-common-coding-mistakes-on-data-science-interviews/)。已获许可转载。

**相关：**

+   [如何通过进行作品集项目来在数据科学面试中脱颖而出](https://www.kdnuggets.com/2021/10/ace-data-science-interview-portfolio-projects.html)

+   [顶级科技公司提出的数据科学 SQL 面试问题](https://www.kdnuggets.com/2021/10/data-science-sql-interview-questions.html)

+   [最常见的数据科学面试问题及答案](https://www.kdnuggets.com/2021/08/common-data-science-interview-questions-answers.html)

### 更多相关话题

+   [成为优秀数据科学家所需的 5 项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家都应该掌握的 6 种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021 年最佳 ETL 工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [停止学习数据科学以寻找目标，并寻找目标以……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [建立一个稳固的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)
