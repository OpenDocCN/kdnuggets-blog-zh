# Python 数据准备案例文件：基于分组的插补

> 原文：[https://www.kdnuggets.com/2017/09/python-data-preparation-case-files-group-based-imputation.html](https://www.kdnuggets.com/2017/09/python-data-preparation-case-files-group-based-imputation.html)

![标题图像](../Images/00e1dafff08a57b5dc7e3551854f78ad.png)

这是本系列关于 Python 数据准备的第二篇文章，专注于基于分组的插补。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 复习

虽然第一篇文章展示了一种简单的插补缺失值的方法，基于相同变量的均值，但这并不是填补缺失值的最复杂方法。假设，如我们的数据集示例（见[第一篇文章](/2017/09/python-data-preparation-case-files-basic-imputation.html)），我们有四个州的客户，我们希望利用这些州的居住地作为区分客户的手段，并相应地填补缺失值。这是一种更为细致的方法。

更具体地说，让我们使用给定状态下客户的平均账户余额来确定在相同状态下其他客户缺失余额值的替代值。

我不会详细介绍模拟数据或任务；可以查看[第一篇文章](/2017/09/python-data-preparation-case-files-basic-imputation.html)以获取必要的背景。同时，[在这里获取数据集](https://drive.google.com/file/d/0B6GhBwm5vaB2ekdlZW5WZnppb28/view?usp=sharing)（在第一篇文章练习后的状态）。

![数据集](../Images/b227855f302f981c69eaedf6495c4b1a.png)

**图 1：** 我们练习的模拟银行数据集（练习 1 之后）。

### 过程

这次的过程非常简单，因为我们只有一个任务。

**插补缺失账户余额**

我们将假设我们的领域专家已得出结论，缺失的账户余额最好用同一状态下客户的平均账户余额来替代（这也是为什么我们必须删除缺少状态值的实例）。这将适用于支票账户和储蓄账户。

首先，让我们看看数据集中缺失值的当前状态。

```py
import pandas as pd
import numpy as np

# Importing the dataset
dataset_filename = 'mock_bank_data_original.csv'
df = pd.read_csv(dataset_filename)

# Summarize missing values
print 'Null values by variable:'
print '------------------------'
df.isnull().sum()

```

```py
Null values by variable:
------------------------
sex                108
age                107
state                0
cheq_balance        23
savings_balance     91
credit_score         0
special_offer        0
dtype: int64
```

我们的支票账户余额（请原谅“q”；我是加拿大人）和储蓄账户余额分别有 23 个和 91 个缺失值。

### 代码

首先，让我们比较按州划分的余额均值与总体均值（插补前），以满足兴趣。

```py
print 'Overall cheq_balance mean:', df['cheq_balance'].mean().round(2)
print 'Overall savings_balance mean:', df['savings_balance'].mean().round(2)

Overall cheq_balance mean: 4938.91
Overall savings_balance mean: 9603.64
```

```py
print 'cheq_balance mean by state:'
print '---------------------------'
print df.groupby(['state']).mean().groupby('state')['cheq_balance'].mean().round(2)

cheq_balance mean by state:
---------------------------
state
CA    4637.23
FL    4993.99
NY    4932.80
TX    5175.78
Name: cheq_balance, dtype: float64
```

```py
print 'savings_balance mean by state:'
print '------------------------------'
print df.groupby(['state']).mean().groupby('state')['savings_balance'].mean().round(2)

savings_balance mean by state:
------------------------------
state
CA     9174.56
FL     9590.59
NY    10443.61
TX     9611.70
Name: savings_balance, dtype: float64
```

虽然可能没有预期的那么戏剧性，但很明显为什么这种基于分组的填补方法对于这种问题是有效的。例如，加利福尼亚州（$9174.56）和纽约州（$10443.61）之间的平均储蓄账户余额差异接近$1270。使用$9603.64的整体均值来填补缺失值不会提供最准确的画面。这涉及到[拟合优度](https://en.wikipedia.org/wiki/Goodness_of_fit)。

现在我们可以使用Pandas的‘groupby’和‘transform’功能，以及一个lambda函数来填补这些缺失值。然后我们在下面的代码行中对结果进行四舍五入。

```py
# Replace cheq_balance NaN with mean cheq_balance of same state
df['cheq_balance'] = df.groupby('state').cheq_balance.transform(lambda x: x.fillna(x.mean()))
df.cheq_balance = df.cheq_balance.round(2)

# Replace savings_balance NaN with mean savings_balance of same state
df['savings_balance'] = df.groupby('state').savings_balance.transform(lambda x: x.fillna(x.mean()))
df.savings_balance = df.savings_balance.round(2)
```

就这样。我们相关的缺失值现在已经用相同州的均值填补好了。

检查结果：

```py
Null values by variable:
------------------------
sex                108
age                107
state                0
cheq_balance         0
savings_balance      0
credit_score         0
special_offer        0
dtype: int64
```

好了！

现在让我们保存数据集以备下次使用。

```py
# Output modified dataset to CSV
df.to_csv('mock_bank_data_original_PART2.csv', index=False)
```

这就是本次练习的全部内容。我们仍然需要处理‘性别’和‘年龄’变量的缺失值。下次我们将探讨基于回归的填补方法，以帮助处理其中之一。

**相关**：

+   [7步掌握Python数据准备](/2017/06/7-steps-mastering-data-preparation-python.html)

+   [从零开始的Python机器学习工作流第1部分：数据准备](/2017/05/machine-learning-workflows-python-scratch-part-1.html)

+   [数据准备技巧、窍门和工具：与内部人士的访谈](/2016/10/data-preparation-tips-tricks-tools.html)

### 更多相关内容

+   [数据填补的3种方法](https://www.kdnuggets.com/2022/12/3-approaches-data-imputation.html)

+   [数据填补方法](https://www.kdnuggets.com/2023/01/approaches-data-imputation.html)

+   [使用Datawig，一个AWS深度学习库进行缺失值填补](https://www.kdnuggets.com/2021/12/datawig-aws-deep-learning-library-missing-value-imputation.html)

+   [机器学习中的数据准备和原始数据](https://www.kdnuggets.com/2022/07/data-preparation-raw-data-machine-learning.html)

+   [SQL数据准备备忘单](https://www.kdnuggets.com/2021/05/data-preparation-sql-cheat-sheet.html)

+   [R中的数据准备备忘单](https://www.kdnuggets.com/2021/10/data-preparation-r-dplyr-cheat-sheet.html)
