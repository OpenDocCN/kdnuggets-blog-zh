# 如何在 DataFrame 列上实现复杂过滤操作

> 原文：[`www.kdnuggets.com/how-to-implement-complex-filters-on-dataframe-columns-with-pandas`](https://www.kdnuggets.com/how-to-implement-complex-filters-on-dataframe-columns-with-pandas)

![如何在 DataFrame 列上实现复杂过滤操作](img/a721f50b2319dddafb503b4dc8c77e27.png)

编辑者提供的图片 | Ideogram

让我们学习如何在 Pandas 中执行复杂的过滤操作。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

## 准备工作

在开始之前，我们需要安装 Pandas 包。你可以使用以下代码进行安装：

```py
pip install pandas
```

安装好包后，让我们深入了解这篇文章。

## Pandas DataFrame 复杂过滤

DataFrame 是一个 Pandas 对象，可以存储数据并根据需要进行操作。它特别强大，因为我们可以使用条件、逻辑运算符和 Pandas 函数来过滤数据。

让我们尝试创建一个简单的 DataFrame 对象。

```py
import pandas as pd

df = pd.DataFrame({
    'Name': ['Alice', 'Leah', 'Jessica', 'Kenny', 'Brad'],
    'Age': [50, 27, 22, 30, 40],
    'Salary': [100000, 154000, 120000, 78000, 88000],
    'Occupation': ['Doctor', 'Soldier', 'Doctor', 'Accountant', 'Florist']
})
```

使用这些示例数据，我们将学习如何进行过滤。首先，我们可以根据特定条件过滤数据。

```py
df[df['Age'] > 30]
```

输出：

```py
 Name  Age  Salary Occupation
0  Alice   50  100000     Doctor
4   Brad   40   88000    Florist
```

将条件与 And (&) 运算符结合使用也是可能的。

```py
df[(df['Age'] > 25) & (df['Salary'] < 100000)]
```

```py
 Name  Age  Salary  Occupation
3  Kenny   30   78000  Accountant
4   Brad   40   88000     Florist
```

使用条件时，我们也可以将它们与 Or (|) 运算符结合使用。

```py
df[(df['Salary'] < 100000) | (df['Occupation'] == 'Soldier')]
```

输出：

```py
 Name  Age  Salary  Occupation
1   Leah   27  154000     Soldier
3  Kenny   30   78000  Accountant
4   Brad   40   88000     Florist
```

还有一些方法可以使用字符串函数来过滤数据。例如，我们可以过滤包含某些值的列。

```py
df[df['Occupation'].str.contains('Sol')]
```

输出：

```py
 Name  Age  Salary Occupation
1  Leah   27  154000    Soldier
```

如果你需要根据特定字符串值过滤数据，我们可以使用以下代码。

```py
df[df['Occupation'].isin(['Doctor', 'Florist'])]
```

输出：

```py
 Name  Age  Salary Occupation
0    Alice   50  100000     Doctor
2  Jessica   22  120000     Doctor
4     Brad   40   88000    Florist
```

还有一种方法是使用 lambda 函数来过滤数据。

```py
df[df['Name'].apply(lambda x: len(x) > 5)]
```

输出：

```py
 Name  Age  Salary Occupation
2  Jessica   22  120000     Doctor
```

如果你想简化，我们可以使用查询方法进行数据过滤。

```py
df.query('Age  100000')
```

输出：

```py
 Name  Age  Salary Occupation
1     Leah   27  154000    Soldier
2  Jessica   22  120000     Doctor
```

最后，我们可以像这样结合我们之前学到的任何过滤条件。

```py
df[(df['Age'] > 30) & (
                 (df['Salary'] > 60000) | 
                 (df['Occupation'].str.contains('Doc')))]
```

输出：

```py
 Name  Age  Salary Occupation
0  Alice   50  100000     Doctor
4   Brad   40   88000    Florist
```

掌握过滤函数以改进你的数据分析过程。

## 额外资源

+   [在 Pandas 中进行条件过滤的五种方法](https://www.kdnuggets.com/2022/12/five-ways-conditional-filtering-pandas.html)

+   [如何使用 Python 过滤数据](https://www.kdnuggets.com/2022/02/filter-data-python.html/)

+   [过滤 Python 列表的 5 种方法](https://www.kdnuggets.com/2022/11/5-ways-filtering-python-lists.html)

**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)** 是数据科学助理经理和数据撰写者。他在全职工作于 Allianz Indonesia 的同时，喜欢通过社交媒体和写作媒体分享 Python 和数据技巧。Cornellius 撰写了各种人工智能和机器学习主题的文章。

### 了解更多相关内容

+   [如何使用[ ], .loc, iloc, .at 等选择 Pandas 中的行和列](https://www.kdnuggets.com/2019/06/select-rows-columns-pandas.html)

+   [重命名 Pandas 列的 4 种方法](https://www.kdnuggets.com/2022/11/4-ways-rename-pandas-columns.html)

+   [卡尔曼滤波器简要介绍](https://www.kdnuggets.com/2022/12/brief-introduction-kalman-filters.html)

+   [如何使用 Pandas 将 JSON 数据转换为 DataFrame](https://www.kdnuggets.com/how-to-convert-json-data-into-a-dataframe-with-pandas)

+   [Pandas 与 Polars：Python 数据框库的比较分析](https://www.kdnuggets.com/pandas-vs-polars-a-comparative-analysis-of-python-dataframe-libraries)

+   [如何在几秒钟内处理包含百万行的 DataFrame](https://www.kdnuggets.com/2022/01/process-dataframe-millions-rows-seconds.html)
