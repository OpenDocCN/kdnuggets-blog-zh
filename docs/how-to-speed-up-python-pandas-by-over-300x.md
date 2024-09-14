# 如何将Python Pandas的速度提升超过300倍

> 原文：[https://www.kdnuggets.com/how-to-speed-up-python-pandas-by-over-300x](https://www.kdnuggets.com/how-to-speed-up-python-pandas-by-over-300x)

![如何将Python Pandas的速度提升超过300倍](../Images/4ccb0c246d992f847174ad475d82ca91.png)

## 如何加速Pandas代码 - 向量化

如果我们希望我们的深度学习模型在一个数据集上进行训练，我们必须优化我们的代码，以便快速解析数据。我们希望使用优化的方法来编写代码，以便尽可能快地读取数据表。即使是最小的性能提升也会在数万条数据点上成倍地提高性能。在这篇博客中，我们将定义Pandas，并提供一个示例，说明如何向量化你的Python代码，以使用Pandas优化数据集分析，使你的代码速度提高超过300倍。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的IT工作

* * *

## 什么是Python的Pandas？

[Pandas](https://pandas.pydata.org/docs/)是Python编程语言中一个重要且流行的开源数据操作和数据分析库。Pandas广泛应用于金融、经济学、社会科学和工程等多个领域。它在数据科学和机器学习任务中对于数据清理、准备和分析非常有用。

它提供了强大的数据结构（如DataFrame和Series）和数据操作工具，用于处理结构化数据，包括以各种格式（例如CSV、Excel、JSON）读取和写入数据，以及数据的过滤、清理和转换。此外，它还支持时间序列数据，并通过与其他流行库如NumPy和Matplotlib的集成，提供强大的数据聚合和可视化功能。

## 我们的数据集和问题

#### 数据

在这个示例中，我们将使用NumPy在一个[Jupyter Notebook](https://jupyter.org/)中创建一个随机数据集，用任意值和字符串填充我们的Pandas数据框。在这个数据集中，我们命名了10,000个年龄不同的人，记录了他们的工作时间和工作中高效时间的百分比。还将随机分配一个最喜欢的零食，以及一个随机的不良因果事件。

我们首先将导入我们的框架，并生成一些随机代码，然后再开始：

```py
import pandas as pd
import numpy as np
```

接下来，我们将创建一些随机数据来构建我们的数据集。虽然你的代码很可能依赖于实际数据，但对于我们的用例，我们将创建一些任意数据。

```py
def get_data(size = 10_000):
    df = pd.DataFrame()
    df['age'] = np.random.randint(0, 100, size)
    df['time_at_work'] = np.random.randint(0,8,size)
    df['percentage_productive'] = np.random.rand(size)
    df['favorite_treat'] = np.random.choice(['ice_cream', 'boba', 'cookie'], size)
    df['bad_karma'] = np.random.choice(['stub_toe', 'wifi_malfunction', 'extra_traffic'])
    return df
```

#### 参数和规则

+   如果一个人的‘time_at_work’至少为2小时，并且‘percentage_productive’超过50%，我们返回‘favorite treat’。

+   否则，我们将其设为`bad_karma`。

+   如果他们超过65岁，我们返回‘favorite_treat’，因为我们希望老年人感到幸福。

```py
def reward_calc(row):
  if row['age'] >= 65:
    return row ['favorite_treat']
  if (row['time_at_work'] >= 2) & (row['percentage_productive'] >= 0.5):
    return row ['favorite_treat']
  return row['bad_karma']
```

现在我们有了数据集和我们想要返回的参数，可以继续探索执行这种类型分析的最快方法。

## 哪种Pandas代码最快：循环、应用还是矢量化？

为了对我们的函数进行计时，我们将使用Jupyter Notebook中的魔法函数`%%timeit`来简化操作。虽然在Python中有其他计时函数的方法，但为了演示目的，我们的Jupyter Notebook足够用了。我们将用3种方法（循环/迭代、应用和矢量化）在相同的数据集上进行演示运行，并计算和评估我们的问题。

#### 循环/迭代

循环和迭代是逐行进行相同计算的最基本方法。我们调用数据框架，迭代行，并用一个新的单元格称为`reward`进行计算，以根据我们之前定义的`reward_calc`代码块填充新的`reward`。这是最基本的方法，也可能是类似于For循环的编码时学到的第一个方法。

```py
%%timeit
df = get_data()
for index, row in df.iterrows():
  df.loc[index, 'reward'] = reward_calc(row)
```

这是它返回的结果：

```py
3.66 s ± 119 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```

缺乏经验的数据科学家可能会觉得几秒钟不算什么。但，3.66秒在数据集中运行一个简单的函数还是相当长的。让我们看看`apply`函数能为我们的速度做些什么。

#### 应用

`apply`函数的效果与循环相同。它将创建一个标题为`reward`的新列，并对每1行应用计算函数，定义为`axis=1`。`apply`函数是对数据集运行循环的更快方法。

```py
%%timeit
df = get_data()
df['reward'] = df.apply(reward_calc, axis=1)
```

运行所需的时间如下：

```py
404 ms ± 18.2 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```

哇，快了这么多！大约快了9倍，较循环有了巨大的改进。现在`apply`函数完全可以使用，并适用于某些场景，但对于我们的用例，看看是否能更快一点。

#### 矢量化

我们评估数据集的最后一种方法是使用矢量化。我们将调用数据集，并将默认的`reward`应用于整个数据框架。然后，我们只会检查那些满足我们参数的行，使用布尔索引。可以把它当作给每行设置一个真/假值。如果任何行或所有行在计算中返回假，则`reward`行将保持为`bad_karma`。而如果所有行都为真，我们将重新定义`reward`行的数据框架为`favorite_treat`。

```py
%%timeit
df = get_data()
df['reward'] = df['bad_karma']
df.loc[((df['percentage_productive'] >= 0.5) &
      (df['time_at_work'] >= 2)) |
      (df['age'] >= 65), 'reward'] = df['favorite_treat']
```

在我们的数据集上运行此函数所需的时间如下：

```py
10.4 ms ± 76.2 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)
```

这非常快。**比Apply快40倍**，并且大约**比循环快360倍……**

## 为什么Pandas中的矢量化快300倍

向量化比循环/迭代和 Apply 更快的原因在于，它不会每次都计算整行，而是将参数应用于整个数据集。向量化是一种将操作一次性应用于整个数据数组的过程，而不是逐个元素操作。这使得内存和 CPU 资源的使用更加高效。

当使用 Loops 或 Apply 对 Pandas 数据框进行计算时，操作是按顺序应用的。这会导致重复访问内存、计算和更新值，这可能会很慢且资源消耗大。

另一方面，向量化操作是用 Cython（Python 在 C 或 C++ 中）实现的，并利用 CPU 的向量处理能力，这可以一次执行多个操作，从而通过同时计算多个参数进一步提高性能。向量化操作还避免了不断访问内存的开销，这正是 Loop 和 Apply 的短板。

## 如何向量化你的 Pandas 代码

1.  使用内置的 Pandas 和 NumPy 函数，例如 **sum()**、**mean()** 或 **max()**，这些函数已经实现了 C 语言的效率。

1.  使用可以应用于整个 DataFrame 和 Series 的向量化操作，包括数学运算、比较和逻辑操作，以创建布尔掩码以选择数据集中的多个行。

1.  你可以使用 **.values** 属性或 `.to_numpy()` 返回底层的 NumPy 数组，并直接在数组上执行向量化计算。

1.  使用向量化的字符串操作来处理你的数据集，例如 `.str.contains()`、`.str.replace()` 和 `.str.split()`。

每当你在 Pandas DataFrames 上编写函数时，尽量将你的计算向量化。随着数据集越来越大，计算变得越来越复杂，利用向量化时节省的时间会呈指数级增长。值得注意的是，并不是所有操作都可以向量化，有时需要使用循环或 apply 函数。然而，只要可能，向量化操作可以大大提高性能，使你的代码更高效。

**[Kevin Vu](https://blog.exxactcorp.com/)** 负责 [Exxact Corp 博客](https://blog.exxactcorp.com/)，并与许多才华横溢的作者合作，这些作者撰写关于深度学习不同方面的文章。

### 了解更多相关内容

+   [加密数据上的机器学习](https://www.kdnuggets.com/2022/08/machine-learning-encrypted-data.html)

+   [学术界是否因方法论而忽视真正的洞见？](https://www.kdnuggets.com/is-academia-obsessing-over-methodology-at-the-cost-of-true-insights)

+   [加速你的 Python 代码的 3 种简单方法](https://www.kdnuggets.com/2022/10/3-simple-ways-speed-python-code.html)

+   [如何通过索引加速 SQL 查询 [Python 版]](https://www.kdnuggets.com/2023/08/speed-sql-queries-indexes-python-edition.html)

+   [如何通过缓存加速 Python 代码](https://www.kdnuggets.com/how-to-speed-up-python-code-with-caching)

+   [5 个提升数据效率和速度的 Python 技巧](https://www.kdnuggets.com/5-python-tips-for-data-efficiency-and-speed)
