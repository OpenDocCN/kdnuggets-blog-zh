# 数据插补的三种方法

> 原文：[`www.kdnuggets.com/2022/12/3-approaches-data-imputation.html`](https://www.kdnuggets.com/2022/12/3-approaches-data-imputation.html)

![数据插补的三种方法](img/b2163a5083acf6a2b2a8ca411d8199b4.png)

图片由作者提供

在处理数据时，如果你的数据没有缺失值，那将是非常棒的。

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

不幸的是，我们生活的世界并不完美，尤其是在数据方面。因此，你需要为这些缺失的数据点找到解决方案。

这就是数据插补发挥作用的地方。

# 什么是数据插补？

数据插补利用各种统计方法，将缺失的数据值替换为替代值。这使得你、企业和客户能够充分利用数据，并提供有价值的见解。

如果你替代一个数据点，这被称为“单元插补”。如果你替代一个数据点的某个组成部分，这被称为“项插补”。

那么，为什么我们要插补数据，而不是直接删除呢？

尽管删除缺失值是更简单的选项，但这并不总是最佳选择。删除缺失值不仅会减少数据集的大小，还可能导致进一步的错误和不可靠的分析。

# 为什么数据插补很重要？

缺失数据无法为我们提供良好的数据集范围，并可能降低整体结果的可靠性。缺失数据可能会：

## 1. 使用 Python 库时的冲突。

有些特定的库在处理缺失数据时表现不佳，自然不兼容。这可能会导致你的工作流程/过程的延迟，并引发更多错误。

## 2. 数据集的问题

如果你删除缺失值，将导致数据集规模的减少。然而，保留缺失值可能对变量、相关性、统计分析和整体结果产生重大影响。

## 3. 最终结果

在处理数据和模型时，你的优先任务是确保最终模型无错误、高效，并在实际案例中值得信赖。缺失数据自然会导致数据集的偏差，从而导致不正确的分析。

那么，我们如何替代这些缺失值呢？让我们来找出答案。

# 数据插补的方法

## 1. 使用均值的 SimpleImputer

```py
class sklearn.impute.SimpleImputer(*, missing_values=nan, strategy='mean', fill_value=None, verbose='deprecated', copy=True, add_indicator=False)
```

使用 Scikit-learn 的 [SimpleImputer](https://scikit-learn.org/stable/modules/generated/sklearn.impute.SimpleImputer.html#sklearn.impute.SimpleImputer) 类，你可以使用常数值、均值、中位数或众数来插补缺失值。

**例如：**

```py
# Imports
import numpy as np
from sklearn.impute import SimpleImputer

# Using the mean value of the columns
imp = SimpleImputer(missing_values=np.nan, strategy='mean')
imp.fit([[1, 3], [5, 6], [8, 4], [11,2], [9, 3]])

# Replace missing values, encoded as np.nan
X = [[np.nan, 3], [5, np.nan], [8, 4], [11,2], [np.nan, 3]]
print(imp.transform(X))
```

**输出：**

```py
[[ 6.8  3\. ]
 [ 5\.   3.6]
 [ 8\.   4\. ]
 [11\.   2\. ]
 [ 6.8  3\. ]] 
```

## 2\. 使用最频繁的 SimpleImputer

使用上述相同的类，我提到你可以使用均值、中位数或众数。众数适用于分类特征，无论是字符串还是数字。它将缺失值替换为该列中最常见的值。

使用上述相同的示例，你可以这样做：

```py
# Imports
import numpy as np
from sklearn.impute import SimpleImputer

# Using the most frequent value of the columns
imp = SimpleImputer(missing_values=np.nan, strategy='most_frequent')
imp.fit([[1, 3], [5, 6], [8, 4], [11,2], [9, 3]])

# Replace missing values, encoded as np.nan
X = [[np.nan, 3], [5, np.nan], [8, 4], [11,2], [np.nan, 3]]
print(imp.transform(X))
```

**输出：**

```py
[[ 1\.  3.]
 [ 5\.  3.]
 [ 8\.  4.]
 [11\.  2.]
 [ 1\.  3.]] 
```

## 3\. 使用 k-NN

如果你不知道 k-NN 是什么，它是一种通过计算当前训练数据点之间的距离来对测试数据集进行预测的算法。它假设相似的事物存在于接近的地方。

使用 [Impyute](https://impyute.readthedocs.io/en/master/#) 库进行插补是使用 k-NN 的一种简单易行的方法。例如：

**插补前：**

```py
n = 4
arr = np.random.uniform(high=5, size=(n, n))
for _ in range(3):
 arr[np.random.randint(n), np.random.randint(n)] = np.nan
print(arr)
```

**输出：**

```py
[[       nan 3.14058295 2.2712381  0.92148091]
 [       nan 3.24750479 1.35688761 2.54943751]
 [4.47019496 0.79944618 3.61855558 3.12191146]
 [3.09645292        nan 0.43638625 4.05435414]] 
```

**插补后：**

```py
import impyute as impy
print(impy.mean(arr))
```

**输出：**

```py
[[3.78332394 3.14058295 2.2712381  0.92148091]
 [3.78332394 3.24750479 1.35688761 2.54943751]
 [4.47019496 0.79944618 3.61855558 3.12191146]
 [3.09645292 2.39584464 0.43638625 4.05435414]] 
```

# 总结

到此为止，你已经了解了什么是数据插补，它的重要性，以及三种不同的处理方法。如果你仍然对了解更多有关缺失值和统计分析感兴趣，我强烈推荐阅读 [缺失数据的统计分析](https://www.amazon.co.uk/Missing-Data-Wiley-Probability-Statistics/dp/0471183865) 由 Roderick J.A. Little 和 Donald B. Rubin 著。

**[Nisha Arya](https://www.linkedin.com/in/nisha-arya-ahmed/)** 是一位数据科学家和自由职业技术作家。她特别感兴趣于提供数据科学职业建议或教程，以及围绕数据科学的理论知识。她还希望探索人工智能如何能够促进人类寿命的延续。作为一个热衷于学习的人，她寻求拓宽技术知识和写作技能，同时帮助指导他人。

### 更多相关话题

+   [数据插补方法](https://www.kdnuggets.com/2023/01/approaches-data-imputation.html)

+   [使用 Datawig，一个 AWS 深度学习库进行缺失值插补](https://www.kdnuggets.com/2021/12/datawig-aws-deep-learning-library-missing-value-imputation.html)

+   [掌握数据分析的力量：分析数据的四种方法](https://www.kdnuggets.com/2023/03/master-power-data-analytics-four-approaches-analyzing-data.html)

+   [数据分析：分析数据的四种方法及其有效应用](https://www.kdnuggets.com/2023/04/data-analytics-four-approaches-analyzing-data-effectively.html)

+   [机器学习的数据标注：市场概况、方法和工具](https://www.kdnuggets.com/2021/12/data-labeling-ml-overview-and-tools.html)

+   [一本将彻底改变你所在组织数据处理方式的新书……](https://www.kdnuggets.com/2022/02/manning-new-book-revolutionize-way-organization-approaches-data.html)
