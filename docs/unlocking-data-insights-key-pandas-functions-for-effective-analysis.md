# 解锁数据洞察：关键 Pandas 函数用于有效分析

> 原文：[`www.kdnuggets.com/unlocking-data-insights-key-pandas-functions-for-effective-analysis`](https://www.kdnuggets.com/unlocking-data-insights-key-pandas-functions-for-effective-analysis)

![解锁数据洞察：关键 Pandas 函数用于有效分析](img/635370e173001e5f17673fc95f67ba04.png)

作者提供的图片 | Midjourney 和 Canva

Pandas 提供了多种函数，使用户能够清理和分析数据。在本文中，我们将介绍一些关键的 Pandas 函数，这些函数对于从数据中提取有价值的洞察至关重要。这些函数将为你提供将原始数据转换为有意义信息所需的技能。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

## 数据加载

数据加载是数据分析的第一步。它允许我们将数据从各种文件格式读取到 Pandas DataFrame 中。这一步对于在 Python 中访问和操作数据至关重要。让我们探索如何使用 Pandas 加载数据。

```py
import pandas as pd
# Loading pandas from CSV file
data = pd.read_csv('data.csv')
```

这段代码导入了 Pandas 库，并使用 **read_csv()** 函数从 CSV 文件中加载数据。默认情况下，read_csv() 假设第一行包含列名，并使用逗号作为分隔符。

## 数据检查

我们可以通过检查关键属性，如行数和列数以及摘要统计数据，来进行数据检查。这有助于我们在进一步分析之前，对数据集及其特征有一个全面的了解。

**df.head()**：默认情况下，它返回 DataFrame 的前五行。这对于检查数据的顶部部分以确保其正确加载非常有用。

```py
 A    B     C
0  1.0  5.0  10.0
1  2.0  NaN  11.0
2  NaN  NaN  12.0
3  4.0  8.0  12.0
4  5.0  8.0  12.0
```

**df.tail()**：默认情况下，它返回 DataFrame 的最后五行。这对于检查数据的底部部分非常有用。

```py
 A    B     C
1  2.0  NaN  11.0
2  NaN  NaN  12.0
3  4.0  8.0  12.0
4  5.0  8.0  12.0
5  5.0  8.0   NaN
```

**df.info()**：此方法提供 DataFrame 的简明总结。包括条目数、列名、非空计数和数据类型。

```py
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 6 entries, 0 to 5
Data columns (total 3 columns):
 #   Column  Non-Null Count  Dtype  
---  ------  --------------  -----  
 0   A       5 non-null      float64
 1   B       4 non-null      float64
 2   C       5 non-null      float64
dtypes: float64(3)
memory usage: 272.0 bytes
```

**df.describe()**：这会生成 DataFrame 中数值列的描述性统计数据。包括计数、均值、标准差、最小值、最大值以及四分位数值（25%、50%、75%）。

```py
 A         B          C
count  5.000000  4.000000   5.000000
mean   3.400000  7.250000  11.400000
std    1.673320  1.258306   0.547723
min    1.000000  5.000000  10.000000
25%    2.000000  7.000000  11.000000
50%    4.000000  8.000000  12.000000
75%    5.000000  8.000000  12.000000
max    5.000000  8.000000  12.000000
```

## 数据清理

数据清理是数据分析过程中的关键步骤，因为它确保了数据集的质量。Pandas 提供了多种函数来解决常见的数据质量问题，如缺失值、重复项和不一致性。

**df.dropna()**：用于删除包含缺失值的任何行。

示例：`clean_df = df.dropna()`

**df.fillna()**：用于用其相应列的均值替换缺失值。

示例：`filled_df = df.fillna(df.mean())`

**df.isnull()**：识别数据框中的缺失值。

示例：`missing_values = df.isnull()`

## 数据选择和过滤

数据选择和过滤是 Pandas 中操作和分析数据的基本技巧。这些操作允许我们根据特定条件提取特定的行、列或数据子集。这使我们能够更轻松地关注相关信息并进行分析。以下是 Pandas 中数据选择和过滤的各种方法：

**df['column_name']**：选择单个列。

示例：`df[“Name”]`

```py
0      Alice
1        Bob
2    Charlie
3      David
4        Eva
Name: Name, dtype: object
```

**df[['col1', 'col2']]**：选择多个列。

示例：`df["Name, City"]`

```py
0      Alice
1        Bob
2    Charlie
3      David
4        Eva
Name: Name, dtype: object
```

**df.iloc[]**：通过整数位置访问行和列组。

示例：`df.iloc[0:2]`

```py
 Name  Age
0  Alice   24
1   Bob   27
```

## 数据汇总和分组

在 Pandas 中对数据进行汇总和分组对于数据总结和分析至关重要。这些操作允许我们通过应用各种汇总函数（如均值、总和、计数等）将大型数据集转化为有意义的见解。

**df.groupby()**：根据指定的列对数据进行分组。

示例：`df.groupby(['Year']).agg({'Population': 'sum', 'Area_sq_miles': 'mean'})`

```py
 Population  Area_sq_miles
Year                              
2020       15025198     332.866667
2021       15080249     332.866667
```

**df.agg()**：提供了一次应用多个聚合函数的方法。

示例：`df.groupby(['Year']).agg({'Population': ['sum', 'mean', 'max']})`

```py
 Population                          
          sum          mean       max
Year                                  
2020  15025198  5011732.666667  6000000
2021  15080249  5026749.666667  6500000
```

## 数据合并和连接

Pandas 提供了几种强大的函数来合并、连接和联接 DataFrames，使我们能够高效且有效地整合数据。

**pd.merge()**：根据公共键或索引合并两个 DataFrames。

示例：`merged_df = pd.merge(df1, df2, on='A')`

**pd.concat()**：沿特定轴（行或列）连接 DataFrames。

示例：`concatenated_df = pd.concat([df1, df2])`

## 时间序列分析

使用 Pandas 进行时间序列分析涉及使用 Pandas 库来可视化和分析时间序列数据。Pandas 提供了专门为处理时间序列数据设计的数据结构和函数。

**to_datetime()**：将字符串列转换为 datetime 对象。

示例：`df['date'] = pd.to_datetime(df['date'])`

```py
 date       value
0 2022-01-01     10
1 2022-01-02     20
2 2022-01-03     30
```

**set_index()**：将 datetime 列设置为 DataFrame 的索引。

示例：`df.set_index('date', inplace=**True**)`

```py
 date     value  
2022-01-01     10
2022-01-02     20
2022-01-03     30
```

**shift()**：将时间序列数据的索引向前或向后移动指定的周期数。

示例：`df_shifted = df.shift(periods=1)`

```py
 date       value
2022-01-01    NaN
2022-01-02   10.0
2022-01-03   20.0
```

## 结论

在这篇文章中，我们介绍了一些在数据分析中至关重要的 Pandas 函数。通过掌握这些工具，你可以轻松处理缺失值、删除重复项、替换特定值，并执行多种数据操作任务。此外，我们还探索了数据聚合、合并和时间序列分析等高级技术。

**[Jayita Gulati](https://www.linkedin.com/in/jayitagulati1998/)** 是一位对机器学习充满热情的技术作家，致力于构建机器学习模型。她拥有利物浦大学计算机科学硕士学位。

### 更多相关内容

+   [超越 Numpy 和 Pandas：释放鲜为人知的……](https://www.kdnuggets.com/2023/08/beyond-numpy-pandas-unlocking-potential-lesserknown-python-libraries.html)

+   [影响洞察时间的关键因素](https://www.kdnuggets.com/2023/03/key-factors-affecting-time-insights.html)

+   [每个数据科学家都应该知道的 10 个必备 Pandas 函数](https://www.kdnuggets.com/10-essential-pandas-functions-every-data-scientist-should-know)

+   [7 个 Pandas 绘图函数，快速数据可视化](https://www.kdnuggets.com/7-pandas-plotting-functions-for-quick-data-visualization)

+   [你可能不知道的 5 个 Pandas 绘图函数](https://www.kdnuggets.com/2023/02/5-pandas-plotting-functions-might-know.html)

+   [合成数据平台：释放生成 AI 的力量来……](https://www.kdnuggets.com/2023/07/synthetic-data-platforms-unlocking-power-generative-ai-structured-data.html)
