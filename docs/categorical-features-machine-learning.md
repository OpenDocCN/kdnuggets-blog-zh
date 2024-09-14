# 处理机器学习中的类别特征

> 原文：[`www.kdnuggets.com/2019/07/categorical-features-machine-learning.html`](https://www.kdnuggets.com/2019/07/categorical-features-machine-learning.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**由[Hugo Ferreira](https://www.linkedin.com/in/hugo-ferreira8)，数据科学家 | 机器学习爱好者 | 物理学家**

如何在 Python 中轻松实现独热编码

![figure-name](img/dc204dce3ab7ebd72a010b66b8188048.png)照片由[Max Nelson](https://unsplash.com/@maxcodes?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT 需求

* * *

类别数据在许多数据科学和机器学习问题中很常见，但通常比数值数据更具挑战性。特别是，许多机器学习算法要求其输入为数值，因此必须将类别特征转换为数值特征，才能使用这些算法。

转换这些特征的最常见方法之一是**独热编码**类别特征，特别是当类别之间没有自然顺序时（例如，一个包含城市名称如‘伦敦’，‘里斯本’，‘柏林’等的特征‘City’）。对于特征的每一个唯一值（比如‘伦敦’），会创建一列（例如‘City_London’），当实例的原始特征取该值时，该列的值为 1，否则为 0。

尽管这种编码方式非常常见，但在使用 Python 中的 scikit-learn 时实现起来可能会很令人沮丧，因为目前没有简单的转换器可用，特别是当你想将其作为机器学习管道的一部分使用时。在这篇文章中，我将描述如何仅使用 scikit-learn 和 pandas 来实现它（但需要一点努力）。但是，在那之后，我还会展示如何使用[类别编码库](http://contrib.scikit-learn.org/categorical-encoding/index.html)以更简单的方式实现相同的功能。

为了说明整个过程，我将使用[乳腺癌数据集](https://archive.ics.uci.edu/ml/datasets/Breast+Cancer)来自[UCI 机器学习仓库](https://archive.ics.uci.edu/ml/index.html)，该数据集有许多分类特征，可以用来实现一热编码。

### 加载数据

我们要使用的数据是来自[UCI 机器学习仓库](https://archive.ics.uci.edu/ml/index.html)的[乳腺癌数据集](https://archive.ics.uci.edu/ml/datasets/Breast+Cancer)。这个数据集较小，包含几个分类特征，这将允许我们快速探索几种使用 Python、pandas 和 scikit-learn 实现一热编码的方法。

从仓库下载数据后，我们将其读入一个 pandas dataframe `df`。

```py
import pandas as pd# names of columns, as per description
cols_names = ['Class', 'age', 'menopause', 'tumor-size',
              'inv-nodes', 'node-caps', 'deg-malig', 'breast',
              'breast-quad', 'irradiat']# read the data
df = (pd.read_csv('breast-cancer.data',
                 header=None, names=cols_names)
        .replace({'?': 'unknown'}))  # NaN are represented by '?'
```

该数据集有 286 个实例，包含 9 个特征和一个目标（‘Class’）。目标和特征在数据集描述中如下：

```py
 1\. Class: no-recurrence-events, recurrence-events.
 2\. age: 10-19, 20-29, 30-39, 40-49, 50-59, 60-69, 70-79, 80-89, 90-99.
 3\. menopause: lt40, ge40, premeno.
 4\. tumor-size: 0-4, 5-9, 10-14, 15-19, 20-24, 25-29, 30-34, 35-39, 40-44,45-49, 50-54, 55-59.
 5\. inv-nodes: 0-2, 3-5, 6-8, 9-11, 12-14, 15-17, 18-20, 21-23, 24-26, 27-29, 30-32, 33-35, 36-39.
 6\. node-caps: yes, no.
 7\. deg-malig: 1, 2, 3.
 8\. breast: left, right.
 9\. breast-quad: left-up, left-low, right-up, right-low, central.
10\. irradiat: yes, no.
```

我们将所有列都视为‘object’类型，并使用 scikit-learn 的`train_test_split`来拆分训练集和测试集。

```py
from sklearn.model_selection import train_test_splitX = df.drop(columns='Class')
y = df['Class'].copy()X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.5, random_state=42)
```

### 一热编码

编码分类特征有几种方法（例如，参见[这里](http://psych.colorado.edu/~carey/courses/psyc5741/handouts/Coding%20Categorical%20Variables%202006-03-03.pdf)）。在这篇文章中，我们将重点关注其中一种最常见和有用的方法——**一热编码**。经过转换后，结果数据集的每一列对应于每个原始特征的一个唯一值。

例如，假设我们有以下具有三种不同唯一值的分类特征。

```py
+---------+
| Feature |
+---------+
| value_1 |
| value_2 |
| value_3 |
+---------+
```

一热编码后，数据集如下所示：

```py
+-----------------+-----------------+-----------------+
| Feature_value_1 | Feature_value_2 | Feature_value_3 |
+-----------------+-----------------+-----------------+
|               1 |               0 |               0 |
|               0 |               1 |               0 |
|               0 |               0 |               1 |
+-----------------+-----------------+-----------------+
```

我们希望对乳腺癌数据集实现一热编码，使得结果数据集适合用于机器学习算法。请注意，数据集中的许多特征具有类别之间的自然顺序（例如，肿瘤大小），因此其他类型的编码可能更合适，但为了具体说明，我们在这篇文章中只关注一热编码。

### 使用 scikit-learn

让我们看看如何使用 scikit-learn 实现一热编码。这里有一个方便的[转换器](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html)，名为`OneHotEncoder`，乍一看，它似乎正是我们所寻找的。

```py
from sklearn.preprocessing import OneHotEncoderohe = OneHotEncoder(sparse=False)
X_train_ohe = ohe.fit_transform(X_train)
```

如果我们尝试应用上述代码，会得到一个`ValueError`，因为`OneHotEncoder`要求所有值必须是整数，而不是我们所用的字符串。这意味着我们首先必须将所有可能的值编码为整数：对于一个特征，如果它有*n*个可能值（由*n*个不同的字符串给出），我们将它们编码为介于 0 和*n*-1 之间的整数。幸运的是，scikit-learn 中还有另一个[转换器](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.LabelEncoder.html)，叫做`LabelEncoder`，正是用来做这件事的！

```py
from sklearn.preprocessing import LabelEncoderle = LabelEncoder()
X_train_le = le.fit_transform(X_train)
```

而且……我们得到了另一个 `ValueError`！实际上，`LabelEncoder` 仅打算用于目标向量，因此它不能与多列一起使用。不幸的是，在 scikit-learn 0.19 版本中，没有可以处理多列的转换器（对版本 0.20 有一些[希望](http://scikit-learn.org/dev/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder)）。

一个解决方案是制作我们自己的转换器，我们创意性地称之为 `MultiColumnLabelEncoder`，它在每个特征上应用 `LabelEncoder`。

```py
class MultiColumnLabelEncoder:

    def __init__(self, columns = None):
        self.columns = columns # list of column to encode    def fit(self, X, y=None):
        return self    def transform(self, X):
        '''
        Transforms columns of X specified in self.columns using
        LabelEncoder(). If no columns specified, transforms all
        columns in X.
        '''

        output = X.copy()

        if self.columns is not None:
            for col in self.columns:
                output[col] = LabelEncoder().fit_transform(output[col])
        else:
            for colname, col in output.iteritems():
                output[colname] = LabelEncoder().fit_transform(col)

        return output    def fit_transform(self, X, y=None):
        return self.fit(X, y).transform(X)
```

这次会有效吗？

它有效！为了更好地理解发生了什么，让我们查看原始训练集。

我们可以看到，例如，年龄段 30–39 被标记为 0，40–49 被标记为 1，等等，其他特征也是类似的。

应用 `MultiColumnLabelEncoder` 后，我们可以（终于！）使用 `OneHotEncoder` 对训练集和测试集进行独热编码。

一些评论：

+   `OneHotEncoder` 被拟合到训练集中，这意味着对于每个在训练集中*存在的*唯一值，对于每个特征，都创建了一个新列。编码后我们有 39 列。

+   输出是一个 numpy 数组（当使用选项 `sparse=False` 时），这有一个缺点，就是丢失了原始列名和值的所有信息。

+   当我们尝试将测试集转换为训练集时，经过编码器拟合后，我们再次得到一个 `ValueError`。这是因为测试集中存在*新的、之前未见过*的唯一值，编码器不知道如何处理这些值。为了在机器学习算法中同时使用转换后的训练集和测试集，我们需要它们具有相同数量的列。

这个最后的问题可以通过使用 `OneHotEncoder` 的 `handle_unknown='ignore'` 选项来解决，正如其名所示，它会在转换测试集时忽略之前未见过的值。

就这样！训练集和测试集都具有 39 列，现在以适合用于需要数值数据的机器学习算法的形式存在。

然而，上述过程相当不优雅，我们丧失了数据的 dataframe 格式。有没有更简单的方法来实现这一切？

确实有！

### 使用类别编码器

[**类别编码器**](http://contrib.scikit-learn.org/categorical-encoding/index.html)是一个与 scikit-learn 兼容的分类变量编码器库，例如：

+   序数

+   独热编码

+   二进制

+   赫尔默特对比

+   总和对比

+   多项式对比

+   向后差分对比

+   哈希

+   BaseN

+   留一法

+   目标编码

序数编码器、独热编码器和哈希编码器是对 scikit-learn 中现有编码器的改进版本，具有以下优点：

+   支持 pandas 数据框作为输入，并且可选地作为输出；

+   可以通过名称或索引显式配置数据中编码的列，或者推断非数字列，无论输入类型如何；

+   兼容 scikit-learn 管道。

对于我们的目的，我们将使用改进的`OneHotEncoder`并查看我们可以简化多少工作流程。首先，我们导入类别编码器库。

```py
import category_encoders as ce
```

然后，让我们尝试将`OneHotEncoder`直接应用于训练集和测试集。

它立刻生效了！无需先使用`LabelEncoder`！

一些观察：

+   输出是数据框，这使我们可以更容易地检查各个特征如何被转换。与之前一样，每个转换后的数据集有 39 列，可以检查 0 和 1 的矩阵是否与之前获得的相同。

+   我使用了`use_cat_names=True`选项，以便将每个特征的可能值添加到每个新列的特征名称中（例如`age_60-69`）。这样做是为了清晰起见，但默认情况下，它只是添加一个索引（例如`age_1`、`age_2`等）。

+   我们现在可以更容易理解为什么在不要求忽略测试集中的未见值时会得到错误。下面，我们展示了在将编码器拟合到训练集（第一）和测试集（第二）后，经过独热编码的测试集的所有列。在第二种情况下，我们看到现在有 42 列，因为创建了 3 列新列：`age_20-29`、`tumor-size_5-9` 和 `breast-quad_unknown`。这些对应于测试集中的三个值（`age`中的 20–29，`tumor-size`中的 5–9 和 `breast-quad`中的未知），这些值在训练集中*不存在*。

比较以上两种方法，我们可以明显看出，如果我们打算对数据集中的分类特征进行独热编码，应该使用类别编码器库中的`OneHotEncoder`。

正如开始时提到的，独热编码不是编码分类特征的唯一方法，类别编码器库有[几种编码器](http://contrib.scikit-learn.org/categorical-encoding/index.html)可供探索，因为其他编码器可能更适合不同的分类特征和机器学习问题。

你可以在以下位置找到我的 LinkedIn：

[Hugo Ferreira - 研究员 - Instituto Superior Técnico | LinkedIn

在 LinkedIn 上查看 Hugo Ferreira 的个人资料，这是世界上最大的专业社区。Hugo 在他们的…](https://www.linkedin.com/in/hugo-ferreira8)

**简历: [Hugo Ferreira](https://medium.com/@hugorcf)** 是一名数据科学家，拥有数学和物理学背景，并在分析天体物理学和量子物理学中感兴趣的复杂系统的框架和计算工具方面进行了多年的研究。

[原文](https://medium.com/hugo-ferreiras-blog/dealing-with-categorical-features-in-machine-learning-1bb70f07262d)。经许可转载。

**相关:**

+   特征提取的外星人指南

+   掌握机器学习数据准备的 7 个步骤（Python 版）— 2019 年版

+   特征工程快速指南

### 更多相关内容

+   [停止学习数据科学以找到目标，并找到目标来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [数据科学学习统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [一个 90 亿美元的 AI 失败，深入分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么让 Python 成为初创公司理想的编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每个数据科学家都应该知道的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
