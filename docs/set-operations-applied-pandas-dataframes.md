# 应用于 Pandas DataFrames 的集合操作

> 原文：[https://www.kdnuggets.com/2019/11/set-operations-applied-pandas-dataframes.html](https://www.kdnuggets.com/2019/11/set-operations-applied-pandas-dataframes.html)

[评论](#comments)

**由 [Eduardo Corrêa Gonçalves](https://www.researchgate.net/profile/Eduardo_Goncalves17)，ENCE/IBGE**

### 介绍

在某些实际情况中，将 pandas DataFrame 视为一个 **数学集合** 可能是有趣的。在这种情况下，DataFrame 的每一行可以被视为集合的 **元素** 或 **成员**。

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

那么问题变成了：为什么这会有用？答案是这样的。正如我们所知，数据科学问题通常需要分析来自多个来源的数据。在数据分析过程中，你可能会遇到需要比较两个或多个 DataFrames 的内容，以确定它们是否有共同的元素（行）的问题。在本教程中，你将学习集合操作是执行此类任务的最佳和最自然的技术之一。

### 实际示例

假设你有两个 DataFrames，分别命名为 P 和 S，它们分别包含注册了 SQL 和 Python 两门不同课程的学生的姓名和电子邮件。

![图示](../Images/a6a3df6dea2d135e34c3bc8c517f24f0.png)![图示](../Images/bbf6fee30f92261470e8da08ebd0f29b.png)

考虑你需要回答以下问题：

1.  这两个 DataFrames 中有多少不同的学生？

1.  是否有学生同时注册了 Python 和 SQL 课程？

1.  哪些学生在上 Python 课程，但没有上 SQL 课程（反之亦然）？

如果将 DataFrames 视为两个不同的数学集合，答案可以通过简单的方式获得。然后，你只需要应用基本的 **并集**、**交集** 和 **差集** 操作：

P ∪ S，即 P 和 S 的并集，是在 P 或 S 或两者中都存在的元素的集合。注意元素（学生）Elizabeth 在结果中只出现一次。

![图示](../Images/f95c23f5e568a2505b219b35f75f905a.png)

P ∩ S，即 P 和 S 的交集，是既在 P 中又在 S 中的元素的集合。现在，只有 Elizabeth 出现，因为她是两个集合中的唯一一个。

![图示](../Images/bf379304640b60f3bc5dfa4a1f7cba76.png)

P − S，即 P 和 S 的差集，是包含所有在 P 中但不在 S 中的元素的集合：

![图示](../Images/842ac2a9f6a32fc97fdb3e8c2f868293.png)

请注意，S − P 与 P − S 是不同的：

![图示](../Images/af2e6f3d74aed541a4d35629658c4c71.png)

重要的是要指出，对这些操作应用的 DataFrames 必须具有相同的属性（如示例中所示）。

### Pandas 中的集合操作

尽管 pandas 没有提供专门的集合操作方法，但我们可以通过以下方法轻松模拟这些操作：

+   **并集**：concat() + drop_duplicates()

+   **交集**：merge()

+   **差集**：isin() + 布尔索引

在下面的程序中，我们演示了如何做到这一点。代码列表后给出了详细的解释。

结果如下所示：

```py
------------------------------
all students (UNION):
        name                 email
0  Elizabeth        bennet@xyz.com
1      Darcy  darcy@acmecorpus.com
2    Bingley       bingley@xyz.com
------------------------------
Students enrolled in both courses (INTERSECTION):
        name           email
0  Elizabeth  bennet@xyz.com
------------------------------
Python students who are not taking SQL (DIFFERENCE):
    name                 email
1  Darcy  darcy@acmecorpus.com
------------------------------
SQL students who are not taking Python (DIFFERENCE):
      name            email
0  Bingley  bingley@xyz.com
```

这是对代码的完整解释。最初，我们创建了两个 DataFrame，P（Python 学生）和 S（SQL 学生）。创建完成后，它们被提交给程序的第二部分中的三种集合操作。

### 并集

为了执行并集操作，我们应用了两个方法：**concat()** 然后是 **drop_duplicates()**。第一个方法实现数据的连接，即将一个 DataFrame 的行放在另一个 DataFrame 的行下方。因此，以下语句：

```py
all_students = pd.concat([P, S], ignore_index = True) 
```

生成一个由 4 行组成的 DataFrame（P 中 2 行加上 S 中 2 行）。

```py
        name                 email
0  Elizabeth        bennet@xyz.com
1      Darcy  darcy@acmecorpus.com
2    Bingley       bingley@xyz.com
3  Elizabeth        bennet@xyz.com
```

然而，请注意，有两行指的是 Elizabeth，因为她是唯一一个同时注册了两个课程的学生。为了只保留一个该元素的出现，只需使用 drop_duplicates() 方法：

```py
all_students = all_students.drop_duplicates() 
```

```py
        name                 email
0  Elizabeth        bennet@xyz.com
1      Darcy  darcy@acmecorpus.com
2    Bingley       bingley@xyz.com
```

### 交集

多功能的 **merge()** 方法被用来执行交集操作。该方法可以用来以不同的方式组合或连接 DataFrame。然而，当在涉及两个兼容 DataFrame 的操作中未指定任何参数时，它会生成它们的交集：

```py
sql_and_python = P.merge(S)
```

```py
        name           email
0  Elizabeth  bennet@xyz.com
```

### 差集

差集操作的代码稍微复杂一些。如我们所知，两集合 P 和 S 之间的差集是一个旨在确定 P 中不属于 S 的元素的操作。在 pandas 中，我们可以使用**isin()**方法结合**布尔索引**来实现这一操作：

```py
python_only = P[P.email.isin(S.email) == False]
```

为了说明这一点，我们将其分为两部分。第一部分是：

```py
P.email.isin(S.email)
```

上述命令生成一个布尔结构，指出 DataFrame P 中哪些电子邮件存在于 S 中：

```py
0     True
1    False
```

这个布尔结构随后用于过滤 P 中的行：

```py
python_only = P[P.email.isin(S.email) == False]
```

```py
    name                 email
1  Darcy  darcy@acmecorpus.com
```

获取未学习 Python 的 SQL 学生是类似的操作：

```py
sql_only = S[S.email.isin(P.email) == False]
```

```py
      name            email
0  Bingley  bingley@xyz.com
```

### 参考文献/进一步阅读

**Pandas 文档**

[https://pandas.pydata.org/pandas-docs/stable/](https://pandas.pydata.org/pandas-docs/stable/)

**斯坦福哲学百科全书 - 基本集合论**

[https://plato.stanford.edu/entries/set-theory/basic-set-theory.html](https://plato.stanford.edu/entries/set-theory/basic-set-theory.html)

**詹妮弗·维多姆 - 关系代数 2 第 1 部分**

[https://www.youtube.com/watch?v=r_h9yBnNh0U](https://www.youtube.com/watch?v=r_h9yBnNh0U)

**个人简介： [Eduardo Corrêa Gonçalves](https://www.researchgate.net/profile/Eduardo_Goncalves17)** 在巴西地理与统计研究所（IBGE）担任数据库管理员，并在国家统计科学学院（ENCE/IBGE）担任助理教授。他参与了不同经济和农业调查的数据库建模和实施的各个阶段，例如：“企业中央登记统计”、“市级牲畜统计”和“农业生产系统调查”。他的研究、教学和专业活动集中在算法、人工智能和数据库领域。

**相关内容：**

+   [5 个 Pandas 高级功能及其使用方法](/2019/10/5-advanced-features-pandas.html)

+   [了解你的数据：第 1 部分](/2019/09/know-data-part-1.html)

+   [25 个 Pandas 小技巧](/2019/08/25-tricks-pandas.html)

### 相关话题更多内容

+   [使用 SQL 查询 Pandas 数据框](https://www.kdnuggets.com/2021/10/query-pandas-dataframes-sql.html)

+   [使用 Pandas 数据框的 apply() 方法](https://www.kdnuggets.com/2022/07/apply-method-pandas-dataframes.html)

+   [简化合并 Pandas 数据框](https://www.kdnuggets.com/2022/09/combining-pandas-dataframes-made-simple.html)

+   [如何合并 Pandas 数据框](https://www.kdnuggets.com/2023/01/merge-pandas-dataframes.html)

+   [合并 Pandas 数据框的 3 种方法](https://www.kdnuggets.com/2023/03/3-ways-merge-pandas-dataframes.html)

+   [将 JSON 转换为 Pandas 数据框：正确解析的方式](https://www.kdnuggets.com/converting-jsons-to-pandas-dataframes-parsing-them-the-right-way)
