# 如何使用插值技术处理 Pandas 中的缺失数据

> 原文：[https://www.kdnuggets.com/how-to-deal-with-missing-data-using-interpolation-techniques-in-pandas](https://www.kdnuggets.com/how-to-deal-with-missing-data-using-interpolation-techniques-in-pandas)

![使用插值技术处理缺失数据的 Pandas](../Images/dda5fbd49996fb026458e295787c567f.png)

图片来源 | DALLE-3 & Canva

现实世界数据集中的缺失值是一个常见问题。这可能由于各种原因发生，例如遗漏的观察、数据传输错误、传感器故障等。我们不能简单忽略它们，因为这可能会扭曲我们模型的结果。我们必须从分析中移除这些值或对其进行处理，以确保数据集的完整性。删除这些值会导致信息丧失，这是我们不希望的。因此，科学家们设计了各种处理缺失值的方法，如插补和插值。人们经常混淆这两种技术；插补是初学者更常用的术语。在我们进一步讨论之前，让我为这两种技术划定一个清晰的界限。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT 工作

* * *

插补基本上是用统计测量值（如均值、中位数或众数）来填补缺失值。这相当简单，但它没有考虑数据集的趋势。然而，插值基于周围的趋势和模式来估算缺失值。当缺失值没有过多分散时，这种方法更为可行。

现在我们已经了解了这些技术之间的区别，让我们讨论一些 Pandas 中可用的插值方法，然后我将带你通过一个示例。之后，我将分享一些技巧，帮助你选择合适的插值技术。

## Pandas 中的插值方法类型

Pandas 提供了各种插值方法 *('linear', 'time', 'index', 'values', 'pad', 'nearest', 'zero', 'slinear', 'quadratic', 'cubic', 'barycentric', 'krogh', 'polynomial', 'spline', 'piecewise_polynomial', 'from_derivatives', 'pchip', 'akima', 'cubicspline')*，你可以使用 `interpolate()` 函数访问这些方法。该方法的语法如下：

```py
DataFrame.interpolate(method='linear', **kwargs, axis=0, limit=None, inplace=False, limit_direction=None, limit_area=None, downcast=_NoDefault.no_default, **kwargs)
```

我知道这些方法很多，我不想让你感到不知所措。所以，我们将讨论一些常用的方法：

+   **线性插值：** 这是默认方法，计算速度快且简单。它通过绘制一条直线连接已知数据点，并使用这条直线来估计缺失值。

+   **时间插值：** 当你的数据在位置上不均匀分布但在时间上线性分布时，时间插值非常有用。为此，你的索引需要是一个 datetime 索引，它通过考虑数据点之间的时间间隔来填补缺失值。

+   **索引插值：** 这类似于时间插值，它使用索引值来计算缺失值。然而，这里不需要是 datetime 索引，但需要传达一些有意义的信息，如温度、距离等。

+   **填充（前向填充）和反向填充方法：** 这指的是复制已存在的值来填补缺失值。如果传播方向是向前，它将前向填充最后一个有效观测值。如果是向后，它将使用下一个有效观测值。

+   **最近邻插值：** 顾名思义，它利用数据的局部变化来填补缺失值。离缺失值最近的值将被用来填补它。

+   **多项式插值：** 我们知道实际数据集通常是非线性的。因此，这个函数将多项式函数拟合到数据点上，以估计缺失值。你还需要指定多项式的阶数（例如，order=2 代表二次多项式）。

+   **样条插值：** 不要被这个复杂的名称吓倒。样条曲线是使用分段多项式函数连接数据点，从而形成一个最终的平滑曲线。你会注意到插值函数还有 `piecewise_polynomial` 作为一个单独的方法。这两者的区别在于后者不保证在边界处导数的连续性，这意味着它可以发生更突兀的变化。

理论够了；让我们使用包含 1949 到 1960 年每月乘客数据的航空乘客数据集，看看插值是如何工作的。

## 代码实现：航空乘客数据集

我们将在航空乘客数据集中引入一些缺失值，然后使用上述技术之一对其进行插值。

#### 步骤 1: 导入库 & 加载数据集

按如下方式导入基本库，并使用 `pd.read_csv` 函数将该数据集的 CSV 文件加载到 DataFrame 中。

```py
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Load the dataset
url = "https://raw.githubusercontent.com/jbrownlee/Datasets/master/airline-passengers.csv"
df = pd.read_csv(url, index_col='Month', parse_dates=['Month'])
```

`parse_dates` 将‘Month’列转换为 `datetime` 对象，而 `index_col` 将其设置为 DataFrame 的索引。

#### 步骤 2: 引入缺失值

现在，我们将随机选择 15 个不同的实例，并将‘Passengers’列标记为 `np.nan`，表示缺失值。

```py
# Introduce missing values
np.random.seed(0)
missing_idx = np.random.choice(df.index, size=15, replace=False)
df.loc[missing_idx, 'Passengers'] = np.nan
```

#### 步骤 3: 绘制带有缺失值的数据

我们将使用 Matplotlib 可视化引入 15 个缺失值后的数据情况。

```py
# Plot the data with missing values
plt.figure(figsize=(10,6))
plt.plot(df.index, df['Passengers'], label='Original Data', linestyle='-', marker='o')
plt.legend()
plt.title('Airline Passengers with Missing Values')
plt.xlabel('Month')
plt.ylabel('Passengers')
plt.show()
```

![插值后的图表](../Images/e9dd40ecbd7dbfdc49f35a8dc895278b.png)

原始数据集的图表

你可以看到图表在中间被分割，显示了这些位置上缺少值的情况。

#### 步骤4：使用插值

尽管稍后我会分享一些技巧来帮助你选择合适的插值技术，但让我们先专注于这个数据集。我们知道这是时间序列数据，但由于趋势似乎不是线性的，简单的线性插值不适合这里。我们可以观察到一些模式和振荡，以及线性趋势在小范围内。考虑到这些因素，样条插值将在这里表现良好。因此，让我们应用它，并检查插值后缺失值的可视化效果。

```py
# Use spline interpolation to fill in missing values
df_interpolated = df.interpolate(method='spline', order=3)

# Plot the interpolated data
plt.figure(figsize=(10,6))
plt.plot(df_interpolated.index, df_interpolated['Passengers'], label='Spline Interpolation')
plt.plot(df.index, df['Passengers'], label='Original Data', alpha=0.5)
plt.scatter(missing_idx, df_interpolated.loc[missing_idx, 'Passengers'], label='Interpolated Values', color='green')
plt.legend()
plt.title('Airline Passengers with Spline Interpolation')
plt.xlabel('Month')
plt.ylabel('Passengers')
plt.show()
```

![插值后的图表](../Images/2d70d3ccedc65c63fac5ce32bdffcbe0.png)

插值后的图表

从图表中可以看到，插值后的值填补了数据点，并且保留了模式。现在可以用于进一步分析或预测。

## 选择插值方法的技巧

这篇文章的附加部分集中于一些技巧：

1.  可视化你的数据以了解其分布和模式。如果数据均匀间隔和/或缺失值随机分布，简单的插值技术将效果良好。

1.  如果你在时间序列数据中观察到趋势或季节性，使用样条或多项式插值更能保留这些趋势，同时填补缺失值，如上例所示。

1.  高次多项式可以更灵活地拟合，但容易过拟合。保持低次，以避免不合理的形状。

1.  对于不均匀间隔的值，使用基于索引的方法，如索引和时间来填补间隙，而不会扭曲比例。你也可以在这里使用回填或前向填充。

1.  如果你的值变化不频繁或遵循上升和下降的模式，使用最近的有效值也很有效。

1.  在数据样本上测试不同的方法，并评估插值值与实际数据点的拟合程度。

如果你想探索`dataframe.interpolate`方法的其他参数，Pandas文档是最佳的参考资料：[Pandas Documentation](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.interpolate.html)。

**[](https://www.linkedin.com/in/kanwal-mehreen1/)**[Kanwal Mehreen](https://www.linkedin.com/in/kanwal-mehreen1/)** Kanwal是一位机器学习工程师和技术作家，对数据科学及人工智能与医学的交汇处充满热情。她合著了电子书《利用ChatGPT最大化生产力》。作为2022年APAC地区的Google Generation Scholar，她倡导多样性和学术卓越。她还被认定为Teradata多样性技术学者、Mitacs Globalink研究学者和哈佛WeCode学者。Kanwal是变革的坚定倡导者，创办了FEMCodes，以赋权女性进入STEM领域。

### 相关主题更多内容

+   [如何处理机器学习中的分类数据](https://www.kdnuggets.com/2021/05/deal-with-categorical-data-machine-learning.html)

+   [应对机器学习中数据不足的5种方法](https://www.kdnuggets.com/2019/06/5-ways-lack-data-machine-learning.html)

+   [黑色星期五特惠 - 通过DataCamp以更低的价格精通机器学习](https://www.kdnuggets.com/2022/11/datacamp-black-friday-deal-master-machine-learning-less-datacamp.html)

+   [使用Pandas fillna()填补缺失数据的最佳方法](https://www.kdnuggets.com/2023/02/optimal-way-input-missing-data-pandas-fillna.html)

+   [使用Datawig，一个AWS深度学习库进行缺失值插补](https://www.kdnuggets.com/2021/12/datawig-aws-deep-learning-library-missing-value-imputation.html)

+   [如何识别时间序列数据集中的缺失数据](https://www.kdnuggets.com/how-to-identify-missing-data-in-timeseries-datasets)
