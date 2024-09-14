# 如何在大型数据集上使用 Pandas 执行内存高效的操作

> 原文：[`www.kdnuggets.com/how-to-perform-memory-efficient-operations-on-large-datasets-with-pandas`](https://www.kdnuggets.com/how-to-perform-memory-efficient-operations-on-large-datasets-with-pandas)

![如何在大型数据集上执行内存高效的操作](img/2d5df1e34d710f034fb115f832b43dcb.png)

图片由编辑提供 | Midjourney

让我们学习如何在 Pandas 中对大型数据集进行操作。

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT 工作

* * *

## 准备工作

由于我们正在讨论 Pandas 包，你应该已经安装了它。此外，我们还会使用 Numpy 包。所以，请同时安装这两个包。

```py
pip install pandas numpy
```

然后，让我们进入教程的核心部分。

## 使用 Pandas 执行内存高效的操作

Pandas 通常不以处理大型数据集而闻名，因为 Pandas 包的内存密集型操作可能会耗费大量时间甚至占满整个 RAM。然而，有一些方法可以提高 Pandas 操作的效率。

在本教程中，我们将向你展示如何提升在 Pandas 中处理大型数据集的体验。

首先，尝试使用内存优化参数加载数据集。还要尝试更改数据类型，尤其是更改为内存友好的类型，并删除任何不必要的列。

```py
import pandas as pd

df = pd.read_csv('some_large_dataset.csv', low_memory=True, dtype={'column': 'int32'}, usecols=['col1', 'col2'])
```

将整数和浮点数转换为最小类型可以帮助减少内存占用。将具有少量唯一值的类别列转换为类别类型也有帮助。更小的列也有助于提高内存效率。

接下来，我们可以使用分块处理来避免使用全部内存。如果迭代处理会更高效。例如，我们想要计算列的均值，但数据集太大了。我们可以每次处理 100,000 条数据，最后得到总结果。

```py
chunk_results = []

def column_mean(chunk):
    chunk_mean = chunk['target_column'].mean()
    return chunk_mean

chunksize = 100000
for chunk in pd.read_csv('some_large_dataset.csv', chunksize=chunksize):
    chunk_results.append(column_mean(chunk))

final_result = sum(chunk_results) / len(chunk_results) 
```

此外，避免使用带有 lambda 函数的 apply 方法，因为这可能会占用大量内存。更好的做法是使用矢量化操作或带有普通函数的`.apply`方法。

```py
df['new_column'] = df['existing_column'] * 2
```

对于 Pandas 中的条件操作，使用`np.where`通常比直接使用带有`.apply`的 Lambda 函数要快。

```py
import numpy as np 
df['new_column'] = np.where(df['existing_column'] > 0, 1, 0)
```

然后，在许多 Pandas 操作中使用`inplace=True`比将其分配回 DataFrame 要节省更多内存。这种方法更高效，因为将其分配回 DataFrame 会在将它们放回同一个变量之前创建一个单独的 DataFrame。

```py
df.drop(columns=['column_to_drop'], inplace=True)
```

最后，如果可能的话，尽早筛选数据。这将限制我们处理的数据量。

```py
df = df[df['filter_column'] > threshold]
```

尝试掌握这些技巧以改善你在大型数据集中的 Pandas 使用体验。

## 额外资源

+   [如何在大型数据集中移除重复项](https://www.kdnuggets.com/2016/04/clevertap-remove-duplicates-large-datasets.html)

+   [处理大型数据文件以进行机器学习的 7 种方法](https://machinelearningmastery.com/large-data-files-machine-learning)

+   [如何从目录中加载大型数据集以进行 Keras 深度学习](https://machinelearningmastery.com/how-to-load-large-datasets-from-directories-for-deep-learning-with-keras/)

**[](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**** 是一名数据科学助理经理和数据撰稿人。在全职工作于 Allianz Indonesia 的同时，他喜欢通过社交媒体和写作分享 Python 和数据技巧。Cornellius 在多个人工智能和机器学习主题上撰写文章。

### 更多相关主题

+   [如何使用 NumPy 执行矩阵操作](https://www.kdnuggets.com/how-to-perform-matrix-operations-with-numpy)

+   [如何高效合并大型 DataFrame](https://www.kdnuggets.com/how-to-merge-large-dataframes-efficiently-with-pandas)

+   [提升数学效率：导航 NumPy 数组操作](https://www.kdnuggets.com/elevate-math-efficiency-navigating-numpy-array-operations)

+   [假装直到成功：生成真实的合成客户数据集](https://www.kdnuggets.com/2022/01/fake-realistic-synthetic-customer-datasets-projects.html)

+   [管理深度学习数据集的新方法](https://www.kdnuggets.com/2022/03/new-way-managing-deep-learning-datasets.html)

+   [如何识别时间序列数据集中的缺失数据](https://www.kdnuggets.com/how-to-identify-missing-data-in-timeseries-datasets)
