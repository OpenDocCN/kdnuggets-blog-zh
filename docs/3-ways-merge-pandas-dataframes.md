# 3 种合并 Pandas 数据框的方法

> 原文：[`www.kdnuggets.com/2023/03/3-ways-merge-pandas-dataframes.html`](https://www.kdnuggets.com/2023/03/3-ways-merge-pandas-dataframes.html)

![3 种合并 Pandas 数据框的方法](img/7c30c61ece82d6988b8a8623a38824f2.png)

编辑器提供的图像

现实世界中的数据是分散的，需要在一些共同的基础上将不同的来源结合在一起。为了提高效率并降低组织存储所有数据在单一表中的成本，保持数据在多个表中，并在需要时将它们连接在一起，是获取效率和有价值见解的最佳方式。

“Pandas” 提供了数据框合并功能，这在数据分析中非常有用，因为它允许你将来自多个来源的数据合并到一个数据框中。例如，假设你有一个包含客户订单信息的销售数据集和另一个包含客户人口统计信息的数据集。通过在客户 ID 上连接这两个数据框，你可以创建一个包含所有信息的新数据框，使得分析和理解客户人口统计与销售之间的关系变得更加容易。

合并这些数据框可以让你为数据添加额外的列，例如计算字段或汇总统计，这些列可以驱动复杂的机器学习系统。合并也有助于数据准备任务，如清理、标准化和预处理。

在本文中，你将学习三种合并 Pandas 数据框的方法以及输出之间的区别。你还将能够理解如何通过合并、连接和串联操作来促进不同的数据分析用例。

# 合并

`merge()` 操作是一种方法，用于根据一个或多个共同的列（也称为键）合并两个数据框。结果数据框仅包含两个数据框中具有匹配键的行。`merge()` 函数类似于 SQL 的 JOIN 操作。

使用 `merge()` 的基本语法是：

```py
merged_df = pd.merge(df1, df2, on='key')
```

这里，`df1` 和 `df2` 是你想要合并的两个数据框，而 “on” 参数定义了用于合并的列。

默认情况下，pandas 会执行内连接，这意味着仅包含两个数据框中具有匹配键的行。然而，你可以使用 `how` 参数指定其他类型的连接，例如左连接、右连接或外连接。

让我们通过下面的示例来理解这一点。

请注意，你可以使用 [Jupyter Notebook](https://jupyter.org/)（或你选择的 IDE）运行以下代码。

```py
import pandas as pd

# Define two dataframes
df1 = pd.dataframe({"key": ["A", "B", "C", "D"], "value1": [1, 2, 3, 4]})

df2 = pd.dataframe({"key": ["B", "D", "E", "F"], "value2": [5, 6, 7, 8]})

# Perform the merge
merged_df = pd.merge(df1, df2, on="key", how="inner")

# Show the resulting
print(merged_df)
```

这将产生以下输出：

```py
 key  value1  value2
0  B      2      5
1  D      4      6
```

从结果中可以明显看出，新的数据框 `merged_df` 仅包含 'key' 列中值匹配的行，即 B 和 D。

# 连接

另一方面，`join()` 操作基于索引而不是特定列来合并两个数据框。结果数据框仅包含两个数据框中具有匹配索引的行。

使用 `join()` 的基本语法是：

```py
joined_df = df1.join(df2)
```

其中 df1 和 df2 是要合并的两个数据框。

默认情况下，`join()` 执行左连接，这意味着第一个数据框（df1）中的所有行都会包含在结果数据框中，而第二个数据框（df2）中具有匹配索引值的任何行也会被添加。如果第二个数据框中有非匹配的行，它们将具有 NaN 值。通过使用 `how` 参数，你可以指定其他类型的连接，如右连接、内连接或外连接。

让我们通过一个示例来理解，如下所示。

```py
import pandas as pd

# Define two dataframes
df1 = pd.dataframe({"value1": [1, 2, 3, 4]}, index=["A", "B", "C", "D"])

df2 = pd.dataframe({"value2": [5, 6, 7, 8]}, index=["B", "D", "E", "F"])

# Perform the join
joined_df = df1.join(df2, how="inner")

# Show the resulting
print(joined_df)
```

上述代码将产生以下输出：

```py
 value1  value2
B      2      5
D      4      6
```

在这里，新数据框 joined_df 仅包含索引匹配的行，即 B 和 D。

# 合并

`concat()` 用于沿特定轴（行或列）连接多个 pandas 对象（数据框或 Series）。默认情况下，轴为 0，意味着数据沿行（垂直方向）拼接。它的第一个参数是一个 pandas 对象的列表，按列表中指定的顺序拼接。

```py
concatenated_df = pd.concat([df1, df2])
```

该函数可以通过各种参数进行自定义，如 `axis`、`join`、`ignore_index` 等。

使用 Pandas 的 concat 函数来合并两个数据框的示例如下所示：

```py
import pandas as pd

df1 = pd.dataframe(
    {
        "A": ["A0", "A1", "A2", "A3"],
        "B": ["B0", "B1", "B2", "B3"],
        "C": ["C0", "C1", "C2", "C3"],
        "D": ["D0", "D1", "D2", "D3"],
    },
    index=[0, 1, 2, 3],
)

df2 = pd.dataframe(
    {
        "A": ["A4", "A5", "A6", "A7"],
        "B": ["B4", "B5", "B6", "B7"],
        "C": ["C4", "C5", "C6", "C7"],
        "D": ["D4", "D5", "D6", "D7"],
    },
    index=[0, 1, 2, 3],
)

concatenated_df = pd.concat([df1, df2])
print(concatenated_df)
```

这将产生以下输出：

```py
 A   B   C   D
0  A0  B0  C0  D0
1  A1  B1  C1  D1
2  A2  B2  C2  D2
3  A3  B3  C3  D3
0  A4  B4  C4  D4
1  A5  B5  C5  D5
2  A6  B6  C6  D6
3  A7  B7  C7  D7
```

为了避免如上所示的索引重复（在拼接数据框中索引从 0 到 3 出现两次），请使用 `ignore_index=True`，如下所示。

```py
concatenated_df = pd.concat([df1, df2], ignore_index=True)
print(concatenated_df)
```

结果将如下所示。

```py
 A   B   C   D
0  A0  B0  C0  D0
1  A1  B1  C1  D1
2  A2  B2  C2  D2
3  A3  B3  C3  D3
4  A4  B4  C4  D4
5  A5  B5  C5  D5
6  A6  B6  C6  D6
7  A7  B7  C7  D7
```

# 概要

在这篇文章中，你学会了三种合并 Pandas 数据框的方法，并了解它们在任何 BI 项目中处理数据时如何解决不同的目的。该文章通过 Python 代码演示了合并、连接和拼接操作的示例。

**[Vidhi Chugh](https://vidhi-chugh.medium.com/)** 是一位 AI 策略师和数字化转型领导者，在产品、科学和工程交汇处工作，致力于构建可扩展的机器学习系统。她是获奖的创新领导者、作者和国际演讲者。她的使命是使机器学习大众化，并打破术语，让每个人都能参与到这一转型中。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

### 更多相关内容

+   [如何合并 Pandas 数据框](https://www.kdnuggets.com/2023/01/merge-pandas-dataframes.html)

+   [如何高效地使用 Pandas 合并大型数据框](https://www.kdnuggets.com/how-to-merge-large-dataframes-efficiently-with-pandas)

+   [向 Pandas 数据框追加行的 3 种方法](https://www.kdnuggets.com/2022/08/3-ways-append-rows-pandas-dataframes.html)

+   [简化 Pandas 数据框的合并](https://www.kdnuggets.com/2022/09/combining-pandas-dataframes-made-simple.html)

+   [将 JSON 转换为 Pandas 数据框：正确解析它们](https://www.kdnuggets.com/converting-jsons-to-pandas-dataframes-parsing-them-the-right-way)

+   [使用 SQL 查询你的 Pandas 数据框](https://www.kdnuggets.com/2021/10/query-pandas-dataframes-sql.html)
