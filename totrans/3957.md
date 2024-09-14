# 如何在数据集中使用Pandas处理异常值

> 原文：[https://www.kdnuggets.com/how-to-handle-outliers-in-dataset-with-pandas](https://www.kdnuggets.com/how-to-handle-outliers-in-dataset-with-pandas)

![如何在数据集中使用Pandas处理异常值](../Images/6bd591dc6b7d3f45f9bd398b27035931.png)

图片作者

异常值是与其余数据显著不同的异常观察值。它们可能由于实验误差、测量误差或数据本身的变异性而出现。这些异常值可能会严重影响模型的表现，导致结果偏差——就像大学中的相对评分的顶尖表现者可以提高平均分数并影响评分标准一样。处理异常值是数据清理过程中的关键部分。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的IT工作

* * *

在本文中，我将分享如何发现异常值以及在数据集中处理它们的不同方法。

## 检测异常值

有几种方法可以用来检测异常值。如果我来分类的话，情况如下：

1.  **基于可视化的方法：** 绘制散点图或箱线图，以查看数据分布并检查异常数据点。

1.  **基于统计的方法：** 这些方法涉及z分数和IQR（四分位距），它们提供可靠性，但可能不够直观。

我不会详细介绍这些方法，以保持主题的专注。然而，我会在最后提供一些参考资料以供探索。我们将在示例中使用IQR方法。方法如下：

*IQR（四分位距） = Q3（75百分位数） - Q1（25百分位数）*

IQR方法指出，任何低于**Q1 - 1.5 * IQR**或高于**Q3 + 1.5 * IQR**的数据点都被标记为异常值。让我们生成一些随机数据点，并使用此方法检测异常值。

导入必要的库并使用`np.random`生成随机数据：

```py
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Generate random data
np.random.seed(42)
data = pd.DataFrame({
    'value': np.random.normal(0, 1, 1000)
})
```

使用IQR方法从数据集中检测异常值：

```py
# Function to detect outliers using IQR
def detect_outliers_iqr(data):
    Q1 = data.quantile(0.25)
    Q3 = data.quantile(0.75)
    IQR = Q3 - Q1
    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 + 1.5 * IQR
    return (data < lower_bound) | (data > upper_bound)

# Detect outliers
outliers = detect_outliers_iqr(data['value'])

print(f"Number of outliers detected: {sum(outliers)}")
```

**输出 ⇒** 检测到的异常值数量：8

使用散点图和箱线图可视化数据集，以查看其外观

```py
# Visualize the data with outliers using scatter plot and box plot
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 6))

# Scatter plot
ax1.scatter(range(len(data)), data['value'], c=['blue' if not x else 'red' for x in outliers])
ax1.set_title('Dataset with Outliers Highlighted (Scatter Plot)')
ax1.set_xlabel('Index')
ax1.set_ylabel('Value')

# Box plot
sns.boxplot(x=data['value'], ax=ax2)
ax2.set_title('Dataset with Outliers (Box Plot)')
ax2.set_xlabel('Value')

plt.tight_layout()
plt.show()
```

![原始数据集](../Images/4f18796d0b43052494447fa3076c1afe.png)

原始数据集

既然我们已经检测到异常值，接下来让我们讨论处理异常值的几种不同方法。

## 处理异常值

#### 1\. 移除异常值

这是一种最简单的方法，但并不总是正确的。你需要考虑一些因素。如果移除这些离群点显著减少了你的数据集大小，或者它们包含有价值的见解，那么将它们排除在分析之外可能不是最好的决策。然而，如果它们是由于测量误差且数量较少，那么这种方法是合适的。让我们将这种技术应用于上述生成的数据集：

```py
# Remove outliers
data_cleaned = data[~outliers]

print(f"Original dataset size: {len(data)}")
print(f"Cleaned dataset size: {len(data_cleaned)}")

fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 6))

# Scatter plot
ax1.scatter(range(len(data_cleaned)), data_cleaned['value'])
ax1.set_title('Dataset After Removing Outliers (Scatter Plot)')
ax1.set_xlabel('Index')
ax1.set_ylabel('Value')

# Box plot
sns.boxplot(x=data_cleaned['value'], ax=ax2)
ax2.set_title('Dataset After Removing Outliers (Box Plot)')
ax2.set_xlabel('Value')

plt.tight_layout()
plt.show()
```

![移除离群点](../Images/3e24516c50b914c495dc20d4588f5806.png)

移除离群点

请注意，通过移除离群点，数据的分布实际上可以发生变化。如果你移除一些初始的离群点，离群点的定义可能会发生变化。因此，之前在正常范围内的数据，可能会在新的分布下被视为离群点。你可以通过新的箱线图看到新的离群点。

#### 2\. 截尾离群点

当你不想丢弃数据点但保持这些极端值也会影响你的分析时，可以使用这种技术。因此，你设定一个最大值和最小值的阈值，然后将离群点调整到这个范围内。你可以将这种截尾应用于离群点或整个数据集。让我们将截尾策略应用于我们的完整数据集，使其在第5到第95百分位数的范围内。这是你可以执行的方法：

```py
def cap_outliers(data, lower_percentile=5, upper_percentile=95):
    lower_limit = np.percentile(data, lower_percentile)
    upper_limit = np.percentile(data, upper_percentile)
    return np.clip(data, lower_limit, upper_limit)

data['value_capped'] = cap_outliers(data['value'])

fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 6))

# Scatter plot
ax1.scatter(range(len(data)), data['value_capped'])
ax1.set_title('Dataset After Capping Outliers (Scatter Plot)')
ax1.set_xlabel('Index')
ax1.set_ylabel('Value')

# Box plot
sns.boxplot(x=data['value_capped'], ax=ax2)
ax2.set_title('Dataset After Capping Outliers (Box Plot)')
ax2.set_xlabel('Value')

plt.tight_layout()
plt.show()
```

![截尾离群点](../Images/e37acd91d38a3c64ca676c3afb13d6ba.png)

截尾离群点

从图表中可以看到，散点图中的上点和下点由于**截尾**而似乎在一条直线上。

#### 3\. 填补离群点

有时从分析中移除值不是一个选项，因为这可能会导致信息丢失，而你也不希望这些值像**截尾**那样被设为最大值或最小值。在这种情况下，另一种方法是用更有意义的选项，如均值、中位数或众数，来替代这些值。选择依据观察数据的领域而异，但在使用这种技术时要注意不要引入偏差。让我们用众数（出现频率最高的值）替换我们的离群点，看看图表效果如何：

```py
data['value_imputed'] = data['value'].copy()
median_value = data['value'].median()
data.loc[outliers, 'value_imputed'] = median_value

fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 6))

# Scatter plot
ax1.scatter(range(len(data)), data['value_imputed'])
ax1.set_title('Dataset After Imputing Outliers (Scatter Plot)')
ax1.set_xlabel('Index')
ax1.set_ylabel('Value')

# Box plot
sns.boxplot(x=data['value_imputed'], ax=ax2)
ax2.set_title('Dataset After Imputing Outliers (Box Plot)')
ax2.set_xlabel('Value')

plt.tight_layout()
plt.show()
```

![填补离群点](../Images/1743fdb42e0d8bf5950eb9642a87bb47.png)

填补离群点

请注意，现在我们没有任何离群点，但这并不能保证离群点会被移除，因为在填补数据后，IQR（四分位距）也会发生变化。你需要进行实验以找到最适合你情况的方法。

#### 4\. 应用变换

转换应用于你的完整数据集，而不是特定的异常值。你基本上是改变数据的表示方式，以减少异常值的影响。有几种转换技术，如对数转换、平方根转换、Box-Cox转换、Z缩放、Yeo-Johnson转换、最小-最大缩放等。选择适合你的情况的正确转换取决于数据的性质和分析的最终目标。以下是一些提示，帮助你选择正确的转换技术：

+   **对于右偏数据：** 使用对数、平方根或Box-Cox转换。对数转换在你想要压缩在大范围内分布的小数值时效果更佳。平方根转换在你想要一种较少极端的转换并处理零值时更好，而Box-Cox还会对数据进行标准化，这是其他两种方法做不到的。

+   **对于左偏数据：** 先反射数据，然后应用针对右偏数据的技术。

+   **为了稳定方差：** 使用Box-Cox或Yeo-Johnson（类似于Box-Cox，但也处理零和负值）。

+   **对于均值中心化和缩放：** 使用z-score标准化（标准差 = 1）。

+   **对于范围缩放（固定范围即[2,5]）：** 使用最小-最大缩放。

让我们生成一个右偏的数据集，并对完整的数据应用对数转换，看看效果如何：

```py
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Generate right-skewed data
np.random.seed(42)
data = np.random.exponential(scale=2, size=1000)
df = pd.DataFrame(data, columns=['value'])

# Apply Log Transformation (shifted to avoid log(0))
df['log_value'] = np.log1p(df['value'])

fig, axes = plt.subplots(2, 2, figsize=(15, 10))

# Original Data - Scatter Plot
axes[0, 0].scatter(range(len(df)), df['value'], alpha=0.5)
axes[0, 0].set_title('Original Data (Scatter Plot)')
axes[0, 0].set_xlabel('Index')
axes[0, 0].set_ylabel('Value')

# Original Data - Box Plot
sns.boxplot(x=df['value'], ax=axes[0, 1])
axes[0, 1].set_title('Original Data (Box Plot)')
axes[0, 1].set_xlabel('Value')

# Log Transformed Data - Scatter Plot
axes[1, 0].scatter(range(len(df)), df['log_value'], alpha=0.5)
axes[1, 0].set_title('Log Transformed Data (Scatter Plot)')
axes[1, 0].set_xlabel('Index')
axes[1, 0].set_ylabel('Log(Value)')

# Log Transformed Data - Box Plot
sns.boxplot(x=df['log_value'], ax=axes[1, 1])
axes[1, 1].set_title('Log Transformed Data (Box Plot)')
axes[1, 1].set_xlabel('Log(Value)')

plt.tight_layout()
plt.show() 
```

![应用对数转换](../Images/c917598f2da447d949b0d68e87c141f7.png)

应用对数转换

你可以看到一个简单的转换已经处理了大部分异常值，并将它们减少到仅一个。这显示了转换在处理异常值中的强大能力。在这种情况下，必须小心并充分了解你的数据，以选择适当的转换，因为不这样做可能会给你带来问题。

## 总结

这使我们结束了关于异常值、不同检测方法以及如何处理异常值的讨论。本文是pandas系列的一部分，你可以在我的作者页面查看其他文章。如上所述，以下是一些额外的资源，供你深入研究异常值：

1.  [机器学习中的异常值检测方法](https://towardsdatascience.com/outlier-detection-methods-in-machine-learning-1c8b7cca6cb8)

1.  [机器学习中的不同转换](https://medium.com/@vinodkumargr/09-data-transformations-in-ml-different-transformations-in-machine-learning-log-transformer-d794d61d143d)

1.  [为了更好的正态分布的转换类型](https://towardsdatascience.com/types-of-transformations-for-better-normal-distribution-61c22668d3b9)

**[](https://www.linkedin.com/in/kanwal-mehreen1/)**[Kanwal Mehreen](https://www.linkedin.com/in/kanwal-mehreen1/)**** Kanwal 是一名机器学习工程师和技术作家，对数据科学以及人工智能与医学的交叉领域充满深厚的热情。她共同编写了电子书《利用 ChatGPT 提高生产力》。作为 2022 年亚太地区的 Google Generation 学者，她倡导多样性和学术卓越。她还被认定为 Teradata 技术多样性学者、Mitacs Globalink 研究学者和哈佛 WeCode 学者。Kanwal 是变革的积极倡导者，创立了 FEMCodes 以赋能女性进入 STEM 领域。

### 更多相关话题

+   [使用 Python 中的标准差去除异常值](https://www.kdnuggets.com/2017/02/removing-outliers-standard-deviation-python.html)

+   [如何使用 Pandas 准确处理时区和时间戳](https://www.kdnuggets.com/how-to-handle-time-zones-and-timestamps-accurately-with-pandas)

+   [处理不平衡数据的 7 种技巧](https://www.kdnuggets.com/2017/06/7-techniques-handle-imbalanced-data.html)

+   [NumPy 中的掩码数组处理缺失数据](https://www.kdnuggets.com/masked-arrays-in-numpy-to-handle-missing-data)

+   [KDnuggets 新闻，8 月 31 日：完整的数据科学学习路线图…](https://www.kdnuggets.com/2022/n35.html)

+   [如何使用 Scikit-learn 的 Imputer 模块处理缺失数据](https://www.kdnuggets.com/how-to-handle-missing-data-with-scikit-learns-imputer-module)
