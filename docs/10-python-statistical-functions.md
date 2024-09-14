# 10 个 Python 统计函数

> 原文：[`www.kdnuggets.com/10-python-statistical-functions`](https://www.kdnuggets.com/10-python-statistical-functions)

![10 Python 统计函数](img/dbee76aec2f97bf997f9faf6e6be977d.png)

图片由 [freepik](https://www.freepik.com/free-photo/top-view-office-desk-with-growth-chart-glasses_11383330.htm#fromView=search&page=1&position=24&uuid=a78a7cd4-2cc8-4097-878b-62664fe9c5e1) 提供

统计函数是从原始数据中提取有意义洞察的基石。Python 为统计学家和数据科学家提供了强大的工具包，用于理解和分析数据集。像 NumPy、Pandas 和 SciPy 这样的库提供了全面的函数套件。本指南将深入探讨这三个库中 10 个必备的统计函数。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT 需求

* * *

## 统计分析库

Python 提供了许多专门用于统计分析的库。其中三个最广泛使用的是 NumPy、Pandas 和 SciPy stats。

+   **NumPy：** 是 Numerical Python 的缩写，这个库提供了对数组、矩阵以及各种数学函数的支持。

+   **Pandas：** Pandas 是一个数据操作和分析库，适用于处理表格和时间序列数据。它建立在 NumPy 之上，并添加了数据操作的额外功能。

+   **SciPy stats：** 是 Scientific Python 的缩写，这个库用于科学和技术计算。它提供了大量的概率分布、统计函数和假设检验。

Python 库在使用之前必须下载并导入到工作环境中。要安装库，请使用终端和 pip install 命令。安装完成后，可以通过 import 语句将其加载到 Python 脚本或 Jupyter notebook 中。NumPy 通常导入为 `np`，Pandas 导入为 `pd`，通常只从 SciPy 中导入 stats 模块。

```py
pip install numpy
pip install pandas
pip install scipy

import numpy as np
import pandas as pd
from scipy import stats
```

在使用多个库可以计算不同函数的情况下，将显示每个库的示例代码。

## 1\. 均值（平均数）

均值，也称为平均数，是最基本的统计测量。它提供了一组数字的中心值。从数学上讲，它是所有值的总和除以值的数量。

```py
mean_numpy = np.mean(data) 
mean_pandas = pd.Series(data).mean()
```

## 2\. 中位数

中位数是另一种集中趋势的度量。它通过报告将所有值按顺序排列后数据集中的中间值来计算。与均值不同，它不受离群值的影响。这使得它在偏斜分布中成为一种更稳健的度量。

```py
median_numpy = np.median(data) 
median_pandas = pd.Series(data).median()
```

## 3\. 标准差

标准差是衡量一组值的变异程度或离散程度的指标。它通过每个数据点与均值之间的差异来计算。较低的标准差表示数据集中的值趋向于接近均值，而较大的标准差表示值分布较为分散。

```py
std_numpy = np.std(data) 
std_pandas = pd.Series(data).std()
```

## 4\. 百分位数

百分位数表示值在数据集中相对的排名，当所有数据按顺序排列时。例如，第 25 百分位数是低于 25%数据的值。中位数从技术上定义为第 50 百分位数。

百分位数是使用 NumPy 库计算的，必须在函数中包含感兴趣的具体百分位数。在示例中计算了第 25、第 50 和第 75 百分位数，但从 0 到 100 的任何百分位数值都是有效的。

```py
percentiles = np.percentile(data, [25, 50, 75])
```

## 5\. 相关性

两个变量之间的相关性描述了它们关系的强度和方向。这是指一个变量在另一个变量变化时发生变化的程度。相关系数的范围是-1 到 1，其中-1 表示完全负相关，1 表示完全正相关，0 表示变量之间没有线性关系。

```py
corr_numpy = np.corrcoef(x, y) 
corr_pandas = pd.Series(x).corr(pd.Series(y))
```

## 6\. 协方差

协方差是一个统计度量，表示两个变量共同变化的程度。它不像相关性那样提供关系的强度，但提供了变量之间关系的方向。它也是许多统计方法的关键，这些方法研究变量之间的关系，例如主成分分析。

```py
cov_numpy = np.cov(x, y) 
cov_pandas = pd.Series(x).cov(pd.Series(y))
```

## 7\. 偏度

偏度衡量连续变量分布的非对称性。零偏度表示数据对称分布，例如正态分布。偏度有助于识别数据集中的潜在离群值，且建立对称性是某些统计方法和变换的要求。

```py
skew_scipy = stats.skew(data) 
skew_pandas = pd.Series(data).skew()
```

## 8\. 峰度

偏度常与峰度一起使用，峰度描述了分布的尾部相对于正态分布的面积多少。它用于指示离群值的存在，并描述分布的整体形状，例如是否高度尖锐（称为尖峰型）或更为平坦（称为平峰型）。

```py
kurt_scipy = stats.kurtosis(data) 
kurt_pandas = pd.Series(data).kurt()
```

## 9\. T 检验

t 检验是一种统计检验，用于确定两组均值之间是否存在显著差异。或者，在单样本 t 检验的情况下，它可以用来确定样本的均值是否显著不同于预定的总体均值。

该测试使用 SciPy 库中的 stats 模块运行。测试提供了两个输出，t 统计量和 p 值。通常，如果 p 值小于 0.05，则结果被认为在统计上显著，表明两个均值彼此不同。

```py
t_test, p_value = stats.ttest_ind(data1, data2)
onesamp_t_test, p_value = stats.ttest_1samp(data, popmean = 0)
```

## 10. 卡方检验

卡方检验用于确定两个分类变量之间是否存在显著的关联，例如职位和性别。该检验同样使用 SciPy 库中的 stats 模块，并需要输入观察数据和期望数据。与 t 检验类似，输出提供了卡方检验统计量和 p 值，可以与 0.05 进行比较。

```py
chi_square_test, p_value = stats.chisquare(f_obs=observed, f_exp=expected)
```

## 总结

本文重点介绍了 Python 中的 10 个关键统计函数，但在各种包中还有许多其他函数可用于更具体的应用。利用这些统计和数据分析工具，可以从数据中获得强大的洞察力。

[](https://www.linkedin.com/in/mehrnazsiavoshi/)**[Mehrnaz Siavoshi](https://www.linkedin.com/in/mehrnazsiavoshi/)**拥有数据分析硕士学位，是一名全职生物统计学家，专注于复杂的机器学习开发和医疗保健统计分析。她有 AI 经验，并在 People 大学教授生物统计学和机器学习课程。

### 更多相关主题

+   [Python 中的统计函数](https://www.kdnuggets.com/2022/10/statistical-functions-python.html)

+   [Python Lambda 函数解释](https://www.kdnuggets.com/2023/01/python-lambda-functions-explained.html)

+   [你可能不知道的 4 个 Python Itertools 筛选函数](https://www.kdnuggets.com/2023/08/4-python-itertools-filter-functions-probably-didnt-know.html)

+   [提高 Python 函数编写质量的 5 个技巧](https://www.kdnuggets.com/5-tips-for-writing-better-python-functions)

+   [统计学习导论，Python 版：免费书籍](https://www.kdnuggets.com/2023/07/introduction-statistical-learning-python-edition-free-book.html)

+   [什么是矩生成函数？](https://www.kdnuggets.com/2022/12/momentgenerating-functions.html)
