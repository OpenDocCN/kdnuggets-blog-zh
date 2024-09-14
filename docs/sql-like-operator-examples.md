# SQL LIKE 运算符示例

> 原文：[`www.kdnuggets.com/2022/09/sql-like-operator-examples.html`](https://www.kdnuggets.com/2022/09/sql-like-operator-examples.html)

![SQL LIKE 运算符示例](img/3b7b662b2dc0a025696544a14f94ea61.png)

SQL LIKE 运算符示例

# 什么是 SQL LIKE 运算符

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 需求

* * *

SQL **LIKE** 运算符用于在列中搜索特定模式。它总是与**WHERE**子句一起使用，我们可以使用**AND**或**OR**运算符组合任意数量的条件。

通常与 LIKE 运算符一起使用的有两个通配符：

1.  “%”符号表示零个、一个或多个字符。例如：“a%”，它将查找以“a”开头的名称。没有大小限制。

1.  “_”下划线表示一个字符。你可以添加多个下划线来定义固定长度。例如：“a___”，它将找到以“a”开头且长度固定为 4 的名称。

在我们深入了解 LIKE 的实际例子之前，先看看各种通配符和描述的示例。

| **LIKE 运算符** | **描述** |
| --- | --- |
| WHERE EmployeeName LIKE 'g%' | 显示所有以“g”开头的员工姓名 |
| WHERE EmployeeName LIKE '%g' | 显示所有以“g”结尾的员工姓名 |
| WHERE EmployeeName LIKE '%en%' | 显示任何位置包含“en”的员工姓名 |
| WHERE EmployeeName LIKE '_g%' | 显示第二个位置有“g”的员工姓名 |
| WHERE EmployeeName LIKE 'g__%' | 显示以“g”开头且长度至少为 3 个字符的员工姓名 |
| WHERE EmployeeName NOT LIKE 'g%' | 显示所有不以“a”开头的员工姓名。 |
| WHERE EmployeeName LIKE 'g%l' | 显示以“g”开头且以“l”结尾的员工姓名 |

# SQL LIKE 示例

在本节中，我们将学习在示例数据集中使用 LIKE 运算符的 9 种不同方法。[SF Salaries](https://www.kaggle.com/datasets/kaggle/sf-salaries)数据集包含了旧金山市政府员工及其薪酬信息。数据集包括 2011 年至 2014 年的年薪数据。

在我们的例子中，我们将重点关注 EmployeeName、JobTitle、BasePay、OvertimePay、OtherPay 和 TotalPay，如下所示。

| **Id** | **EmployeeName** | **JobTitle** | **BasePay** | **OvertimePay** | **OtherPay** | **TotalPay** |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | Nathaniel Ford | 总经理-大都会交通管理局 | 167411.2 | 0 | 400184.3 | 567595.4 |
| 2 | 加里·吉门内斯 | 队长 III（警察部门） | 155966 | 245131.9 | 137811.4 | 538909.3 |
| 3 | 阿尔伯特·帕尔迪尼 | 队长 III（警察部门） | 212739.1 | 106088.2 | 16452.6 | 335279.9 |
| 4 | 克里斯托弗·钟 | 钢丝绳电缆维护技工 | 77916 | 56120.71 | 198306.9 | 332343.6 |
| 5 | 帕特里克·加德纳 | 副部门主管（消防部门） | 134401.6 | 9737 | 182234.6 | 326373.2 |

**注意：** 我正在使用在线 SQL 查询工具 ([sqliteonline.com](https://sqliteonline.com/#)) 来创建示例。

## 示例 #1

它将显示所有 EmployeName 为“Nathaniel Ford”的列。你也可以使用“=”等号来代替 LIKE，但通配符与等号不起作用。

```py
SELECT *

FROM Salaries

WHERE EmployeeName LIKE 'Nathaniel Ford';
```

![SQL LIKE 运算符示例](img/75053a9ec967311aa84c32bae5d57a32.png)

## 示例 #2

查询将显示以“Nathaniel”开头的前 5 个员工姓名。如我们所见，我们得到了 Nathaniel Ford、Nathaniel Steger、Nathaniel Yuen 等。

结尾的“%”代表该字符后面的任何字符。

```py
SELECT *

FROM Salaries

WHERE EmployeeName LIKE 'Nathaniel%'

LIMIT 5;
```

![SQL LIKE 运算符示例](img/6b2e7b0515cc7a93e1fabc83806ff140.png)

## 示例 # 3

查询将搜索所有以“Ford”结尾的员工姓名，并显示前五个结果。

```py
SELECT *

FROM Salaries

WHERE EmployeeName LIKE '% Ford'

LIMIT 5;
```

![SQL LIKE 运算符示例](img/8b66b05ab94d8ec04a510e720db58462.png)

## 示例 #4

查询将搜索所有在任意位置包含“Ford”的员工姓名，并显示前五个结果。

我们得到了一些名字，比如雷·克劳福德、基思·桑福德、约翰·桑福德 Jr 等。

```py
SELECT *

FROM Salaries

WHERE EmployeeName LIKE '%Ford%'

LIMIT 5;
```

![SQL LIKE 运算符示例](img/1b3bc28d2a0b3705da1c046186a0e2a2.png)

## 示例 #5

查询将搜索所有以“k”开头并以“o”结尾的员工姓名。例如，我们得到了 Kenneth Esposto。

```py
SELECT *

FROM Salaries

WHERE EmployeeName LIKE 'k%o'

LIMIT 5;
```

![SQL LIKE 运算符示例](img/c20034a943c0602365cc6d3fa55932ee.png)

## 示例 #6

我们将使用下划线来搜索第二个位置是“t”且第三个位置是“h”的员工姓名。

我们得到了以“那个第二位置”开头的伊桑·杰克逊、奥莎·科顿、厄尔斯特伯特·奥布赫等。

```py
SELECT *

FROM Salaries

WHERE EmployeeName LIKE '_th%'

LIMIT 5;
```

![SQL LIKE 运算符示例](img/644808e909bb1173b370752589f6740d.png)

## 示例 #7

这是一个有趣的查询，我们正在寻找名字的前四个字符是“kenn”且两个下划线后跟“h”的员工姓名。这意味着 LIKE 运算符将查找以“Kenn”开头并以“h”结尾的名字。名字长度固定为 7 个字符。

```py
SELECT *

FROM Salaries

WHERE EmployeeName LIKE 'Kenn__h%'

LIMIT 5;
```

![SQL LIKE 运算符示例](img/625c8c6a628991a8f824af77a731ea65.png)

## 示例 #8

让我们了解一下没有“%”的下划线通配符。我们将找到以“ob”结尾且长度为 9 个字符的名字。为此，我们使用了 7 个下划线来将名字长度固定为 9。

结果中我们只有一个名字，即“Carly Gorb”。

```py
SELECT *

FROM Salaries

WHERE EmployeeName LIKE '________ob'

LIMIT 5;
```

![SQL LIKE 运算符示例](img/e7cfa46460d4865e231841e594d70b41.png)

## 示例 #9

我们可以使用 AND、OR 和 NOT 与 LIKE 运算符。这可以帮助你执行更具体的搜索。

在这个示例中，我们将使用 NOT 来显示除了以“ken”开头的名字以外的所有名字。

```py
SELECT *

FROM Salaries

WHERE EmployeeName NOT LIKE 'ken%'

LIMIT 5;
```

![SQL LIKE 操作符示例](img/05a5b4d465661857dd0a65b2e9df01f9.png)

# 结论

带通配符的 LIKE 操作符可以帮助你编写有效的查询并搜索特定的记录。你甚至可以将其与逻辑函数结合使用，以缩小搜索范围。

在这篇文章中，我们学习了使用 LIKE 操作符的多种方法。我强烈建议你自己练习。你甚至可以解决[w3schools.com](https://www.w3schools.com/SQL/sql_like.asp)的互动练习，以更好地理解这一概念。

> “如果你希望更频繁地看到这种内容，请在下方评论告诉我。”

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan)) 是一名认证的数据科学专业人士，热衷于构建机器学习模型。目前，他专注于内容创作，并撰写关于机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是利用图神经网络为那些正在与心理健康问题作斗争的学生打造一款 AI 产品。

### 更多相关内容

+   [如何（不）使用 Python 的海象操作符](https://www.kdnuggets.com/how-not-to-use-pythons-walrus-operator)

+   [示例中的集成学习](https://www.kdnuggets.com/2022/10/ensemble-learning-examples.html)

+   [挑选示例以理解机器学习模型](https://www.kdnuggets.com/2022/11/picking-examples-understand-machine-learning-model.html)

+   [机器学习不像你的大脑 第一部分：神经元很慢，……](https://www.kdnuggets.com/2022/04/machine-learning-like-brain-part-one-neurons-slow-slow-slow.html)

+   [用这些课程构建类似 ChatGPT 的聊天机器人](https://www.kdnuggets.com/2023/05/build-chatgptlike-chatbot-courses.html)

+   [像老板一样进行 MLOps：一份无泪机器学习指南](https://www.kdnuggets.com/2023/06/mlops-like-boss-guide-machine-learning-without-tears.html)
