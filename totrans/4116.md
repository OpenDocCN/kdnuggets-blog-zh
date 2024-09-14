# Python 中的统计函数

> 原文：[https://www.kdnuggets.com/2022/10/statistical-functions-python.html](https://www.kdnuggets.com/2022/10/statistical-functions-python.html)

![Python 中的统计函数](../Images/10d02a82d9c51f27e5813c0aa9955e49.png)

图片来源：[Andrea Piacquadio](https://www.pexels.com/photo/woman-draw-a-light-bulb-in-white-board-3758105/)

统计函数在数据分析和得出有意义的结论中非常有帮助。在本教程中，我们将深入探讨一些可以应用于 pandas 和 series 对象的有用统计函数。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

在教程中将涵盖以下统计函数：

+   pct_change()

+   cov()

+   corr()

+   corrwith ()

# pct_change()

pct_change() 方法可以应用于 pandas 的 series 和 Data Frame，以计算特定周期数的百分比变化

## 计算 pct_change() 时未指定周期数

**代码：**

```py
import pandas as pd
import numpy as np

series = pd.Series(np.random.randn(10))

series.pct_change()
```

**输出：**

```py
0         NaN

1   -0.881470

2   -5.025007

3    0.728078

4   -0.577371

5    1.173420

6   -1.578389

7   -3.520208

8   -1.927874

9   -1.600583

dtype: float64
```

## 通过指定周期数来计算 pct_change()

**代码：**

```py
df = pd.DataFrame(np.random.randn(10,2))

df.pct_change(periods = 2)
```

**输出：**

|  | **0** | **1** |
| --- | --- | --- |
| **0** | NaN | NaN |
| **1** | NaN | NaN |
| **2** | -0.095052 | -1.399525 |
| **3** | 0.073909 | -7.491512 |
| **4** | -0.882174 | -1.150202 |

# 协方差：cov()

cov() 方法用于计算 series 和 Data Frame 中的协方差。在计算 Data Frame 中的协方差时，会计算 Data Frame 中 series 之间的配对协方差。

在计算 series 和 Data Frame 中的协方差时，会排除任何缺失值

## 计算两个 series 之间的协方差

**代码：**

```py
series1 = pd.Series(np.random.randn(200))
series2 = pd.Series(np.random.randn(200))

series1.cov(series2)
```

**输出：**

```py
-0.14817157321848334
```

## 计算 Data Frame 的协方差

**代码：**

```py
df = pd.DataFrame(np.random.randn(4,5),columns = ["a","b","c","d","e"])
df.cov()
```

**输出：**

|  | **a** | **b** | **c** | **d** | **e** |
| --- | --- | --- | --- | --- | --- |
| **a** | 2.095402 | 0.191502 | 0.049185 | 0.090229 | -1.052856 |
| **b** | 0.191502 | 0.628889 | 0.377184 | -0.507893 | 0.404180 |
| **c** | 0.049185 | 0.377184 | 0.336220 | -0.077814 | 0.571139 |
| **d** | 0.090229 | -0.507893 | -0.077814 | 0.950198 | 0.164894 |
| **e** | -1.052856 | 0.404180 | 0.571139 | 0.164894 | 1.722546 |

# 相关性：corr()

相关性是使用 corr() 方法计算的，corr() 方法有一个方法参数，具有以下方法名称选项：

1.  Pearson（默认）是标准相关系数

1.  Kendall Tau 相关系数

1.  Spearman 秩相关系数

## 计算 Data Frame 中 series 之间的相关性，使用默认的 Pearson

**代码：**

```py
df = pd.DataFrame(np.random.randn(200,4), columns = ["a","b","c","d"])
df["a"]. corr(df["b"])
```

**输出：**

```py
0.08425780768544051
```

## 使用 spearman 方法计算 Data Frame 中序列之间的相关性

**代码：**

```py
df["a"]. corr(df["b"],method = "spearman")
```

**输出：**

```py
0.053819845496137414
```

## 计算 Data Frame 列之间的成对相关性

**代码：**

```py
df.corr()
```

**输出：**

|  | **a** | **b** | **c** | **d** |
| --- | --- | --- | --- | --- |
| **a** | 1.000000 | 0.084258 | -0.074284 | 0.054453 |
| **b** | 0.084258 | 1.000000 | 0.022995 | 0.029727 |
| **c** | -0.074284 | 0.022995 | 1.000000 | -0.028279 |
| **d** | 0.054453 | 0.029727 | -0.028279 | 1.000000 |

# corrwith ()

Corrwith () 方法应用于 Data Frame，以计算不同 Data Frame 对象中相同标签的 Series 之间的相关性

**代码：**

```py
index = ["a","b","c","d","e"]

columns = ["one","two","three","four"]

df1 = pd.DataFrame(np.random.randn(5,4), index = index, columns = columns )

df2 = pd.DataFrame(np.random.randn(4,4), index = index[:4], columns = columns)

df1.corrwith(df2)
```

**输出：**

```py
one      0.277569

two     -0.052151

three   -0.754392

four     0.526614

dtype: float64
```

**代码：**

```py
df2.corrwith(df1, axis=1)
```

**输出：**

```py
a    0.346955

b   -0.707590

c    0.711081

d    0.753457

e         NaN

dtype: float64
```

**[Priya Sengar](https://www.linkedin.com/in/priya-sengar/)** (**Medium**, **Github**) 是 Old Dominion University 的数据科学家。Priya 热衷于解决数据问题并将其转化为解决方案。

### 更多相关主题

+   [10 个 Python 统计函数](https://www.kdnuggets.com/10-python-statistical-functions)

+   [Python Lambda 函数解释](https://www.kdnuggets.com/2023/01/python-lambda-functions-explained.html)

+   [4 个你可能不知道的 Python Itertools 过滤函数](https://www.kdnuggets.com/2023/08/4-python-itertools-filter-functions-probably-didnt-know.html)

+   [提升 Python 函数编写技巧的 5 个建议](https://www.kdnuggets.com/5-tips-for-writing-better-python-functions)

+   [统计学习简介，Python 版：免费电子书](https://www.kdnuggets.com/2023/07/introduction-statistical-learning-python-edition-free-book.html)

+   [什么是矩生成函数？](https://www.kdnuggets.com/2022/12/momentgenerating-functions.html)
