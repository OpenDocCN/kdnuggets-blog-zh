# 3 个更多 SQL 聚合函数面试问题用于数据科学

> 原文：[https://www.kdnuggets.com/2023/01/3-sql-aggregate-function-interview-questions-data-science.html](https://www.kdnuggets.com/2023/01/3-sql-aggregate-function-interview-questions-data-science.html)

![3 个更多 SQL 聚合函数面试问题用于数据科学](../Images/1c0430f5a5a6da2036d7955721ed515d.png)

图片由作者提供

聚合函数是 SQL 的核心功能之一。从 SQL 作为必备的 [数据科学工具](https://www.stratascratch.com/blog/the-10-most-useful-data-analysis-tools-for-data-scientists/) 可以得出结论，没有哪个数据科学家没有使用过 SQL 聚合函数的活例子。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT

* * *

否则他们如何分析数据并提供有意义的指标？ 数据科学家的生活没有数据聚合就像一支断了的铅笔——毫无意义。

面试官都知道这一点。他们会全方位检查你的聚合技能。

# **五大 SQL 聚合函数**

[SQL 聚合函数](https://www.stratascratch.com/blog/the-ultimate-guide-to-sql-aggregate-functions/) 从多行中提取值并返回一个单一的聚合值。它们通常与 GROUP BY 子句一起使用。这使你能够查看组名称和聚合值。

SQL 中有很多聚合函数。它们的数量也取决于你使用的 SQL 方言。

但无论如何，SQL 中最常见的聚合函数就是这五个。

![3 个更多 SQL 聚合函数面试问题用于数据科学](../Images/53c8a03dfe75e615438654eb8347f32a.png)

图片由作者提供

函数的名称并不神秘。任何人都可以直观地推断出每个函数的作用。

然而，在面对业务问题时，知道如何使用它们是另一回事。这些技能需要一点练习。

接下来的三个面试问题应该会对你有所帮助。

## 1\. 获奖柔道运动员的平均体重

这是 ESPN 提出的一个问题，你可以在 StrataScratch 上找到。

> “找出每个团队中获得奖牌的柔道运动员的平均体重，年龄在 20 到 30 岁之间，包括 20 岁和 30 岁的运动员。输出团队名称和运动员的平均体重。”

这里是[链接](https://platform.stratascratch.com/coding/10144-average-weight-of-medal-winning-judo?code_type=2&utm_source=blog&utm_medium=click&utm_campaign=kdnuggets)以便你跟着我。

这个问题要求你按团队查找获奖柔道选手的平均体重。这显然意味着你需要使用AVG()聚合函数。

只有20到30岁之间的获奖者应被考虑。使用MIN()和MAX()函数来实现。

放入代码后，它看起来是这样的。

```py
SELECT team,
       AVG(weight) AS average_player_weight
FROM olympics_athletes_events
WHERE sport = 'Judo'
  AND medal IS NOT NULL
GROUP BY team
HAVING MIN(age) >= 20
AND MAX(age) <= 30;
```

查询选择团队，也会在GROUP BY中使用，并从表olympics_athletes_events计算平均体重。WHERE子句用于仅包括柔道项目，并排除未获奖的竞争者。

最后，在HAVING子句中使用MIN()和MAX()函数来显示年龄在20到30岁之间的获胜者。

这里还有代码输出。

| **team** | **average_player_weight** |
| --- | --- |
| France | 77 |
| Georgia | 84 |
| Japan | 70 |
| Romania | 48 |

## 2\. 计算各部门的学生人数

LeetCode的问题要求你完成以下任务。

> “编写一个SQL查询，报告**Department**表中每个部门的部门名称和学生人数（即使某些部门没有在读学生）。返回结果表**按学生人数降序排列**。若有并列情况，按部门名称**字母顺序排列**。”

这里是[链接](https://leetcode.com/problems/count-student-number-in-departments/description/)以便你跟着我。

提示在问题标题中。解决这个问题需要使用COUNT()聚合函数。

```py
SELECT dept_name,
       COUNT(student_id) AS student_number
FROM department
LEFT JOIN student ON department.dept_id = student.dept_id
GROUP BY department.dept_name
ORDER BY student_number DESC,
         department.dept_name;
```

与之前的问题不同，这里有两个表：department和student。

第一步是选择部门名称，并使用COUNT()函数通过其ID查找学生人数。

要获取两个列，使用LEFT JOIN连接表。为什么使用这种连接？因为问题要求返回所有部门名称，即使没有学生。

输出按部门分组，按学生人数降序排列，部门名称升序排列。

这里是期望的输出。

| **dept_name** | **student_number** |
| --- | --- |
| Engineering | 2 |
| Science | 1 |
| Law | 0 |

## 3\. 比赛排行榜

最后的问题来自HackerRank。

> “你在帮助Julia完成她上一个编程挑战时做得非常出色，所以她希望你也能处理这个问题！黑客的总分是他们在所有挑战中的最高分之和。编写一个查询，打印出黑客ID、姓名和总分，按总分降序排列。如果有多个黑客的总分相同，则按黑客ID升序排列。排除所有总分为0的黑客。”

如果你想跟我一起操作，可以点击这个 [链接](https://www.hackerrank.com/challenges/contest-leaderboard/problem?isFullScreen=true)。

该解决方案使用了两个聚合函数：SUM() 和 MAX()。

```py
SELECT h.hacker_id,
       h.name,
       SUM(max_score) AS sum_max_score
FROM hackers h
JOIN
  (SELECT hacker_id,
          challenge_id,
          MAX(score) AS max_score
   FROM submissions
   GROUP BY hacker_id,
            challenge_id) s ON h.hacker_id = s.hacker_id
GROUP BY h.name,
         h.hacker_id
HAVING SUM(max_score) > 0
ORDER BY SUM(max_score) DESC, h.hacker_id;
```

让我们首先关注子查询，它用于计算每个黑客和挑战的最高分。为此，它使用了 MAX() 聚合函数。

主查询从 hackers 表中选择黑客的 ID 和名字。它还使用 SUM() 函数通过引用子查询来汇总最高分。

这两个查询通过列 hacker_id 使用 JOIN 连接。

之后，输出被过滤以仅显示总分高于零的记录。最后，结果按总分降序排列，并按黑客ID升序排列。

这里显示了输出的前几行。

| **hacker_id** | **name** | **sum_max_score** |
| --- | --- | --- |
| 76971 | Ashley | 760 |
| 84200 | Susan | 710 |
| 76615 | Ryan | 700 |
| 82382 | Sara | 640 |
| 79034 | Marilyn | 580 |

# 摘要

就这样：展示了三道中等难度面试问题中的五个最受欢迎的 SQL 聚合函数。对于一个关键的 SQL 概念，这并不算太难，对吧？

这三个问题是你可以从面试中期待的良好示例。如你所见，SQL 聚合函数并不是单独存在的。它们通常与其他 SQL 概念同时考察，如数据分组和过滤、表连接以及编写子查询。

使用这些问题作为指导，但确保你自己解决几个问题。那是你在面试前能接近面试的最好方法。

**[内特·罗西迪](https://www.stratascratch.com)** 是一位数据科学家和产品战略专家。他还是一名兼职教授，教授分析学，并且是 [StrataScratch](https://www.stratascratch.com/) 的创始人，该平台帮助数据科学家通过来自顶级公司的真实面试问题准备面试。你可以在 [Twitter: StrataScratch](https://twitter.com/StrataScratch) 或 [LinkedIn](https://www.linkedin.com/in/nathanrosidi/) 上与他联系。

### 更多相关内容

+   [你必须知道的前10个高级数据科学 SQL 面试问题……](https://www.kdnuggets.com/2023/01/top-10-advanced-data-science-sql-interview-questions-must-know-answer.html)

+   [25个高级SQL面试问题](https://www.kdnuggets.com/2022/10/25-advanced-sql-interview-questions-data-scientists.html)

+   [数据分析师的 SQL 和 Python 面试问题](https://www.kdnuggets.com/2023/02/sql-python-interview-questions-data-analysts.html)

+   [经验丰富的专业人士 SQL 面试问题](https://www.kdnuggets.com/2022/01/sql-interview-questions-experienced-professionals.html)

+   [你下次面试中可能会看到的24个SQL问题](https://www.kdnuggets.com/2022/06/24-sql-questions-might-see-next-interview.html)

+   [KDnuggets 新闻，5月4日：9门免费哈佛课程学习数据……](https://www.kdnuggets.com/2022/n18.html)
