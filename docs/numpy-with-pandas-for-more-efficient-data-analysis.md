# NumPy 与 Pandas 结合以更高效的数据分析

> 原文：[https://www.kdnuggets.com/numpy-with-pandas-for-more-efficient-data-analysis](https://www.kdnuggets.com/numpy-with-pandas-for-more-efficient-data-analysis)

![NumPy 与 Pandas 结合以更高效的数据分析](../Images/966f0eb71db7a03f3a0f947532966a6a.png)[图像由 jcomp 提供，来源 Freepik](https://www.freepik.com/free-vector/big-isolated-employee-working-office-workplace-flat-illustration_13744784.htm#fromView=search&page=1&position=26&uuid=c476a51c-f7d1-4d92-91d6-9107c4785ea2)

作为一个数据从业者，Pandas 是任何数据操作活动的首选包，因为它直观且易于使用。这就是为什么许多数据科学教育课程在学习课程中包括 Pandas 的原因。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在的组织的 IT

* * *

Pandas 是建立在 NumPy 包上的，特别是 NumPy 数组。许多 NumPy 函数和方法仍然与它们兼容，因此我们可以使用 NumPy 有效地提升 Pandas 数据分析。

这篇文章将探讨几个示例，说明 NumPy 如何帮助我们提高 Pandas 数据分析的体验。

让我们深入了解一下。

## 使用 NumPy 改进 Pandas 数据分析

在继续教程之前，我们应该安装所有必要的软件包。如果尚未安装，你可以使用以下`代码`来安装 Pandas 和 NumPy。

```py
pip install pandas numpy
```

我们可以首先解释 Pandas 和 NumPy 之间的联系。如上所述，Pandas 建立在 NumPy 包上。让我们看看它们如何相互补充，以改进我们的数据分析。

首先，让我们尝试使用相应的软件包创建一个 NumPy 数组和 Pandas DataFrame。

```py
import numpy as np
import pandas as pd

np_array= np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
pandas_df = pd.DataFrame(np_array, columns=['A', 'B', 'C'])

print(np_array)
print(pandas_df)
```

```py
Output>>
[[1 2 3]
 [4 5 6]
 [7 8 9]]
   A  B  C
0  1  2  3
1  4  5  6
2  7  8  9
```

如上面的代码所示，我们可以使用具有相同维度结构的 NumPy 数组来创建 Pandas DataFrame。

接下来，我们可以在 Pandas 数据处理和清理步骤中使用 NumPy。例如，我们可以使用 NumPy 的 NaN 对象作为缺失数据的占位符。

```py
df = pd.DataFrame({
    'A': [1, 2, np.nan, 4, 5],
    'B': [5, np.nan, np.nan, 3, 2],
    'C': [1, 2, 3, np.nan, 5]
})
print(df)
```

```py
Output>>
    A    B    C
0  1.0  5.0  1.0
1  2.0  NaN  2.0
2  NaN  NaN  3.0
3  4.0  3.0  NaN
4  5.0  2.0  5.0
```

如上面的结果所示，NumPy 的 NaN 对象在 Pandas 中成为任何缺失数据的同义词。

这段代码可以检查每个 Pandas DataFrame 列中的 NaN 对象数量。

```py
df.isnull().sum() 
```

```py
Output>>
A    1
B    2
C    1
dtype: int64
```

数据收集者可能会将 DataFrame 列中的缺失数据值表示为字符串。如果发生这种情况，我们可以尝试用 NumPy 的 NaN 对象替换该字符串值。

```py
df['A'] = df['A'].replace('missing data'', np.nan)
```

NumPy 还可以用于异常值检测。让我们看看如何做到这一点。

```py
df = pd.DataFrame({
    'A': np.random.normal(0, 1, 1000),
    'B': np.random.normal(0, 1, 1000)
})

df.loc[10, 'A'] = 100
df.loc[25, 'B'] = -100

def detect_outliers(data, threshold=3):
    z_scores = np.abs((data - data.mean()) / data.std())
    return z_scores > threshold

outliers = detect_outliers(df)
print(df[outliers.any(axis =1)])
```

```py
Output>>
            A           B
10  100.000000    0.355967
25    0.239933 -100.000000
```

在上面的代码中，我们使用NumPy生成随机数，然后创建一个使用Z-score和sigma规则检测离群值的函数。结果是包含离群值的DataFrame。

我们可以使用Pandas进行统计分析。NumPy可以帮助在聚合过程中实现更高效的分析。例如，这是使用Pandas和NumPy进行的统计聚合。

```py
df = pd.DataFrame({
    'Category': [np.random.choice(['A', 'B']) for i in range(100)],
    'Values': np.random.rand(100)
})

print(df.groupby('Category')['Values'].agg([np.mean, np.std, np.min, np.max]))
```

```py
Output>>
             mean       std      amin      amax
Category                                        
A         0.524568  0.288471  0.025635  0.999284
B         0.525937  0.300526  0.019443  0.999090
```

使用NumPy，我们可以将统计分析函数应用于Pandas DataFrame，并获得类似上述输出的汇总统计信息。

最后，我们将讨论使用Pandas和NumPy的矢量化操作。矢量化操作是一种同时对数据执行操作的方法，而不是逐个循环执行。结果将更快且内存优化。

例如，我们可以使用NumPy在DataFrame列之间执行逐元素加法操作。

```py
data = {'A': [15,20,25,30,35], 'B': [10, 20, 30, 40, 50]}

df = pd.DataFrame(data)
df['C'] = np.add(df['A'], df['B'])  

print(df)
```

```py
Output>>
   A   B   C
0  15  10  25
1  20  20  40
2  25  30  55
3  30  40  70
4  35  50  85
```

我们还可以通过NumPy数学函数转换DataFrame列。

```py
df['B_exp'] = np.exp(df['B'])
print(df)
```

```py
Output>>
   A   B   C         B_exp
0  15  10  25  2.202647e+04
1  20  20  40  4.851652e+08
2  25  30  55  1.068647e+13
3  30  40  70  2.353853e+17
4  35  50  85  5.184706e+21
```

还可以使用NumPy对Pandas DataFrame进行条件替换。

```py
df['A_replaced'] = np.where(df['A'] > 20, df['B'] * 2, df['B'] / 2)
print(df)
```

```py
Output>>
   A   B   C         B_exp  A_replaced
0  15  10  25  2.202647e+04         5.0
1  20  20  40  4.851652e+08        10.0
2  25  30  55  1.068647e+13        60.0
3  30  40  70  2.353853e+17        80.0
4  35  50  85  5.184706e+21       100.0
```

这些是我们探索的所有示例。这些NumPy函数无疑会帮助改善你的数据分析过程。

## 结论

本文讨论了NumPy如何通过Pandas帮助提高高效的数据分析。我们尝试了数据预处理、数据清理、统计分析和使用Pandas和NumPy的矢量化操作。

希望这对你有所帮助！

**[](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**** 是一位数据科学助理经理和数据撰写者。在全职工作于印尼安联时，他喜欢通过社交媒体和写作分享Python和数据技巧。Cornellius撰写了各种AI和机器学习主题。

### 更多相关内容

+   [如何在大型数据集上执行内存高效操作](https://www.kdnuggets.com/how-to-perform-memory-efficient-operations-on-large-datasets-with-pandas)

+   [使用Tableau创建高效的综合数据源](https://www.kdnuggets.com/2022/05/create-efficient-combined-data-sources-tableau.html)

+   [如何使用Hugging Face的Datasets库进行高效数据加载](https://www.kdnuggets.com/how-to-use-hugging-faces-datasets-library-for-efficient-data-loading)

+   [PEFT概述：最先进的参数高效微调](https://www.kdnuggets.com/overview-of-peft-stateoftheart-parameterefficient-finetuning)

+   [如何编写高效的Python代码：初学者教程](https://www.kdnuggets.com/how-to-write-efficient-python-code-a-tutorial-for-beginners)

+   [免费MIT课程：TinyML和高效深度学习计算](https://www.kdnuggets.com/free-mit-course-tinyml-and-efficient-deep-learning-computing)
