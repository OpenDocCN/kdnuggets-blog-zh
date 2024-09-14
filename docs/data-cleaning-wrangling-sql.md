# SQL 中的数据清理和整理

> 原文：[`www.kdnuggets.com/2021/01/data-cleaning-wrangling-sql.html`](https://www.kdnuggets.com/2021/01/data-cleaning-wrangling-sql.html)

评论

**由[Antonio Emilio Badia](https://engineering.louisville.edu/faculty/antonio-e-badia/)，路易斯维尔大学 CSE 系副教授。**

![](img/7b90faf67d8c5a9b93c5838add6004a0.png)

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速通道进入网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

了解 SQL 被认为是数据科学家的基本技能之一，因为大量数据存在于（并持续被收集到）关系数据库中。数据分析的典型方法是从这些存储库中提取数据，并使用 R 或 Python 中的强大包/库进行分析。在这种方法中，SQL 仅用于数据的子集化和提取；基本的*查询*即可满足需求。

然而，实践者知道从原始数据到分析之间存在一个([漫长的过程](https://hdsr.mitpress.mit.edu/pub/577rq08d/release/3)：数据必须经过仔细准备，这是一项复杂的任务，通常包括标记的*数据清理*、*数据整理*或*数据预处理*等多个过程。一个问题是在哪个环境中进行这个过程。一些分析师更喜欢在 R 或 Python 中完成整个过程，因为它们都提供了[丰富的支持](https://r4ds.had.co.nz/)，并且它们（尤其是 R）高度互动，而 SQL 缺乏丰富的包/库，使得在 R 或 Python 中许多操作变成了简单的一行代码，其语法也可能相当受限。另一方面，这种方法也有一些缺点：在处理非常大的数据集时，R 和 Python 可能会变得缓慢或遇到问题，而且包/库的多样性（许多功能重叠）在这些环境中增加了额外的复杂性。因此，在某些情况下，至少在 SQL 中进行部分数据清理和整理可能是有益的。正如我们所展示的，许多常见任务可以在 SQL 中完成而不会增加太多额外负担。为了说明这一点，我们提供了一个示例，重点介绍了缺失值的识别和替换。

缺失值在 SQL 中由 NULL 标记表示，但数据可能并不总是明确标记。假设有一个包含医学研究中患者信息的表*Patients*的数据集。一个属性是*i*d，一个标识符，另外两个属性是*Height*和*Weight*，分别表示研究开始时每位患者的身高和体重。我们注意到一些体重值缺失，由-1 表示。然后我们按如下方式“清理”表：

```py
UPDATE Patients
SET Weight = NULL
WHERE Weight = -1;

```

一旦完成这一点，我们可以使用谓词*IS NULL*来统一处理所有缺失值：查询

```py
SELECT count(*)
FROM Patient
WHERE Weight IS NULL;

```

将精确告诉我们缺失了多少值。如果只有少量值缺失，我们可能想要丢弃不完整的数据。SQL 命令

```py
DELETE FROM Patient
WHERE Weight IS NULL;

```

将消除缺少权重的行（观察值）。如果该属性被认为在后续分析中不重要，则命令

```py
UPDATE TABLE Patient
DROP ATTRIBUTE Weight;

```

将从整个表中消除该属性——即，从所有行（观察值）中消除。如果我们更愿意*填补*缺失值，则命令：

```py
UPDATE TABLE Patient
SET Weight = (SELECT avg(Weight) FROM Patient)
WHERE Weight IS NULL;

```

将所有缺失值替换为现有值的均值。更复杂的方法是从相关属性中填补缺失值。我们可能会想知道*Height*是否与体重相关；查询：

```py
SELECT cor(Weight, Height)
FROM Patient;

```

将计算系统中两个属性之间的[相关系数](https://en.wikipedia.org/wiki/Correlation_and_dependence)，其中存在内置的相关函数`cor`。即使在没有该函数的系统中，SQL 允许我们自己编写：

```py
SELECT (avg(Weight * Height) - (avg(Weight)* avg(Height))) / (std(Weight) * std(Height))
FROM Patient;

```

因为均值（*avg*）和标准差（*std*）函数是普遍存在的。如果我们认为相关性足够高，我们可以使用[kNN 算法](https://link.springer.com/book/10.1007/978-3-319-14142-8)来推断适当的值。在一般情况下，SQL 允许我们使用常见的距离函数（如欧氏距离或其他范数）计算数据集元素之间的*距离*，并按计算出的距离对结果进行排序，以便使用*k*最近邻来外推所需的值。在这个具体的例子中，计算非常简单：我们使用高度差的绝对值作为我们的距离，并以最近的 5 个邻居的平均权重作为我们新的填补权重：

```py
SELECT id, avg(Weight)
FROM (SELECT P2.id, P.Weight
      FROM Patient P, Patient P2
      WHERE P2.Weight IS NULL and P.id <> P2.id
      ORDER BY abs(P.Height - P2.Height)
      LIMIT 5) as KNN;
GROUP BY id; 

```

在此查询中，*FROM* 子查询计算两个*不同*数据点（患者）之间的距离，其中一个患者的体重缺失；按此距离排序（*ORDER BY*）并仅保留 5 个最近的结果（*LIMIT*）；然后使用这 5 个邻居的体重平均值作为填补值（注意：平局的情况会被任意打破）。我们为每个缺失体重的数据点返回一个结果，因此我们提供了这样一个点的*id*。该结果可以在第二个查询中使用，类似于上面的第二个 UPDATE，来更改*Patient*中的数据值。

SQL 的一个额外优势是作为一个成熟的标准，能非常高效地处理非常大的数据集（回想一下，一个单独的*查询优化器*分析所有上述语句，并确定系统在当前数据和资源条件下执行 SQL 命令的最有效方式）。在从数据库中提取数据之前，能够进行一些数据清洗、整理和筛选，可能会使数据管道更简单、更高效。

**简介：** [安东尼奥·巴迪亚](https://engineering.louisville.edu/faculty/antonio-e-badia)是路易斯维尔大学计算机科学与工程系的副教授。他在数据库领域的研究得到了 NSF（包括 CAREER 奖）的支持，并发表了 50 多篇论文。他教授数据库课程以及面向非计算机科学专业学生的数据管理和分析入门课程。他是[《数据科学中的 SQL：使用关系数据库进行数据清洗、整理和分析，Springer》](https://www.springer.com/gp/book/9783030575915)一书的作者，该帖子摘录自此书。

**相关：**

+   [5 个棘手的 SQL 查询解决方案](https://www.kdnuggets.com/2020/11/5-tricky-sql-queries-solved.html)

+   [艰难的 SQL 学习之路](https://www.kdnuggets.com/2020/01/learning-sql-hard-way.html)

+   [你所需要的最后一本 SQL 数据分析指南](https://www.kdnuggets.com/2019/10/last-sql-guide-data-analysis-ever-need.html)

### 更多相关内容

+   [掌握 SQL、Python、数据清洗、数据整理和探索性数据分析的指南汇总](https://www.kdnuggets.com/collection-of-guides-on-mastering-sql-python-data-cleaning-data-wrangling-and-exploratory-data-analysis)

+   [掌握使用 Pandas 和 Python 进行数据整理的 7 个步骤](https://www.kdnuggets.com/7-steps-to-mastering-data-wrangling-with-pandas-and-python)

+   [SQL 中的数据清洗：如何为分析准备混乱的数据](https://www.kdnuggets.com/data-cleaning-in-sql-how-to-prepare-messy-data-for-analysis)

+   [通过这本免费电子书学习数据清洗和预处理数据科学](https://www.kdnuggets.com/2023/08/learn-data-cleaning-preprocessing-data-science-free-ebook.html)

+   [掌握数据清洗和预处理技术的 7 个步骤](https://www.kdnuggets.com/2023/08/7-steps-mastering-data-cleaning-preprocessing-techniques.html)

+   [利用 ChatGPT 进行自动化数据清洗和预处理](https://www.kdnuggets.com/2023/08/harnessing-chatgpt-automated-data-cleaning-preprocessing.html)
