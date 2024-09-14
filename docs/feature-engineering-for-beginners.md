# 初学者特征工程

> 原文：[`www.kdnuggets.com/feature-engineering-for-beginners`](https://www.kdnuggets.com/feature-engineering-for-beginners)

![初学者特征工程](img/2f15c607ba1b2815fa1fd5510d9140e1.png)

图片由作者创建

## 介绍

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 需求

* * *

特征工程是机器学习管道中最重要的方面之一。它是创建和修改特征或变量以提高模型性能的实践。设计良好的特征可以将弱模型转变为强模型，通过特征工程，模型可以变得更强大和准确。特征工程充当数据集和模型之间的桥梁，提供模型有效解决问题所需的一切。

这是为新数据科学家、数据工程师和机器学习从业者准备的指南。本文的目标是传达基本的特征工程概念，并提供一套可以应用于实际场景的技术工具箱。我的目标是，通过本文，你将掌握足够的特征工程知识，以便能够将其应用于自己的数据集，从而全面准备好开始创建强大的机器学习模型。

## 了解特征

特征是我们观察到的现象的可测量特性。它们是构成数据的细节元素，模型根据这些元素进行预测。特征的例子可以包括年龄、收入、时间戳、经度、值，以及几乎任何可以以某种形式进行测量或表示的事物。

特征类型有很多种，主要包括：

+   数值特征：连续或离散的数值类型（例如，年龄、工资）

+   类别特征：表示类别的定性值（例如，性别、鞋码类型）

+   文本特征：单词或单词字符串（例如，“this”或“that”或“even this”）

+   时间序列特征：按时间顺序排列的数据（例如，股票价格）

特征在机器学习中至关重要，因为它们直接影响模型的预测能力。构造良好的特征能提高模型性能，而糟糕的特征则使得模型更难产生强有力的预测。特征选择和特征工程是机器学习过程中用于准备数据以供学习算法使用的预处理步骤。

特征选择和特征工程之间有区别，尽管它们各自都至关重要：

+   特征选择：从所有可用特征的集合中筛选出重要特征，从而减少维度并提高模型性能

+   特征工程：创建新特征并随之改变现有特征，以帮助模型表现得更好

通过仅选择最重要的特征，特征选择有助于保留数据中的信号，而特征工程则创建新的特征，以更好地建模结果。

## 特征工程中的基本技术

尽管我们可以使用多种基本特征工程技术，但我们将逐步介绍一些更重要且使用广泛的技术。

### 处理缺失值

数据集通常包含缺失信息。这可能会对模型的性能产生不利影响，因此，实施处理缺失数据的策略非常重要。常见的几种解决方法如下：

+   均值/中位数填补：用列的均值或中位数填补数据集中的缺失区域

+   众数填补：用同一列中最常见的条目填补数据集中的缺失位置

+   插值法：用周围数据点的值填补缺失的数据

这些填补方法应根据数据的性质以及方法可能对最终模型产生的效果来应用。

处理缺失信息对于保持数据集的完整性至关重要。以下是一个示例 Python 代码片段，演示了使用 `pandas` 库的各种数据填补方法。

```py
import pandas as pd
from sklearn.impute import SimpleImputer

# Sample DataFrame
data = {'age': [25, 30, np.nan, 35, 40], 'salary': [50000, 60000, 55000, np.nan, 65000]}
df = pd.DataFrame(data)

# Fill in missing ages using the mean
mean_imputer = SimpleImputer(strategy='mean')
df['age'] = mean_imputer.fit_transform(df[['age']])

# Fill in the missing salaries using the median
median_imputer = SimpleImputer(strategy='median')
df['salary'] = median_imputer.fit_transform(df[['salary']])

print(df)
```

### 分类变量编码

记住，大多数机器学习算法最好（或仅能）处理数值数据，因此，分类变量通常必须映射到数值，以便这些算法能够更好地解读它们。最常见的编码方案如下：

+   独热编码：为每个类别生成单独的列

+   标签编码：为每个类别分配一个整数

+   目标编码：通过各类别的个别结果变量平均值对类别进行编码

对分类数据进行编码是许多机器学习模型理解数据的基础。正确的编码方法是根据具体情况选择的，包括使用的算法和数据集。

以下是一个使用`pandas`和`scikit-learn`元素对分类特征进行编码的示例 Python 脚本。

```py
import pandas as pd
from sklearn.preprocessing import OneHotEncoder, LabelEncoder

# Sample DataFrame
data = {'color': ['red', 'blue', 'green', 'blue', 'red']}
df = pd.DataFrame(data)

# Implementing one-hot encoding
one_hot_encoder = OneHotEncoder()
one_hot_encoding = one_hot_encoder.fit_transform(df[['color']]).toarray()
df_one_hot = pd.DataFrame(one_hot_encoding, columns=one_hot_encoder.get_feature_names_out(['color']))

# Implementing label encoding
label_encoder = LabelEncoder()
df['color_label'] = label_encoder.fit_transform(df['color'])

print(df)
print(df_one_hot)
```

### 数据缩放和归一化

为了许多机器学习方法的良好表现，必须对数据进行缩放和归一化。有几种数据缩放和归一化的方法，例如：

+   标准化：将数据转换为均值为 0 和标准差为 1

+   Min-Max 缩放：将数据缩放到一个固定范围，如 [0, 1]

+   鲁棒缩放：通过中位数和四分位数范围分别迭代缩放高值和低值

数据的缩放和归一化对确保特征贡献的公平性至关重要。这些方法使不同特征值对模型的贡献保持一致。

以下是一个使用`scikit-learn`的实现，展示了如何完成经过缩放和归一化的数据。

```py
import pandas as pd
from sklearn.preprocessing import StandardScaler, MinMaxScaler, RobustScaler

# Sample DataFrame
data = {'age': [25, 30, 35, 40, 45], 'salary': [50000, 60000, 55000, 65000, 70000]}
df = pd.DataFrame(data)

# Standardize data
scaler_standard = StandardScaler()
df['age_standard'] = scaler_standard.fit_transform(df[['age']])

# Min-Max Scaling
scaler_minmax = MinMaxScaler()
df['salary_minmax'] = scaler_minmax.fit_transform(df[['salary']])

# Robust Scaling
scaler_robust = RobustScaler()
df['salary_robust'] = scaler_robust.fit_transform(df[['salary']])

print(df)
```

上述基本技术及其对应的示例代码为处理缺失数据、编码分类变量，以及使用强大的 Python 工具`pandas`和`scikit-learn`进行数据缩放和归一化提供了实用的解决方案。这些技术可以集成到你自己的特征工程过程中，以提升你的机器学习模型。

## 高级特征工程技术

现在我们将关注更高级的特征工程技术，并包含一些实现这些概念的 Python 示例代码。

### 特征创建

通过特征创建，生成或修改新特征以打造性能更好的模型。一些创建新特征的技术包括：

+   多项式特征：利用现有特征创建更高阶特征，以捕捉更复杂的关系

+   交互项：通过组合多个特征生成的特征，用于捕捉它们之间的交互作用

+   特定领域的特征生成：根据特定问题领域的复杂性设计的特征

创建具有调整意义的新特征可以大大提高模型性能。下一个脚本展示了如何利用特征工程将数据中的潜在关系揭示出来。

```py
import pandas as pd
import numpy as np

# Sample DataFrame
data = {'x1': [1, 2, 3, 4, 5], 'x2': [10, 20, 30, 40, 50]}
df = pd.DataFrame(data)

# Polynomial Features
df['x1_squared'] = df['x1'] ** 2
df['x1_x2_interaction'] = df['x1'] * df['x2']

print(df)
```

### 降维

为了简化模型并提高其性能，减少模型特征的数量可能会有所帮助。实现这一目标的降维技术包括：

+   PCA（主成分分析）：将预测变量转换为由线性独立模型特征组成的新特征集

+   t-SNE（t-分布随机邻域嵌入）：用于可视化的降维

+   LDA（线性判别分析）：寻找有效的模型特征组合，以解构不同的类别

为了缩小数据集的规模并保持其相关性，维度减少技术将有所帮助。这些技术旨在解决与数据相关的高维问题，如过拟合和计算需求。

接下来展示的是使用`scikit-learn`实现的数据缩减演示。

```py
import pandas as pd
from sklearn.decomposition import PCA

# Sample DataFrame
data = {'feature1': [2.5, 0.5, 2.2, 1.9, 3.1], 'feature2': [2.4, 0.7, 2.9, 2.2, 3.0]}
df = pd.DataFrame(data)

# Use PCA for Dimensionality Reduction
pca = PCA(n_components=1)
df_pca = pca.fit_transform(df)
df_pca = pd.DataFrame(df_pca, columns=['principal_component'])

print(df_pca)
```

### 时间序列特征工程

对于基于时间的数据集，必须使用特定的特征工程技术，例如：

+   滞后特征：使用先前的数据点来推导模型预测特征

+   滚动统计：在数据窗口中计算数据统计量，如滚动均值

+   季节性分解：将数据划分为信号、趋势和随机噪声类别

与直接模型拟合相比，时间序列模型需要不同的增强。这些方法遵循时间依赖性和模式，使预测模型更加精确。

接下来还将展示使用`pandas`应用的时间序列特征增强演示。

```py
import pandas as pd
import numpy as np

# Sample DataFrame
date_rng = pd.date_range(start='1/1/2022', end='1/10/2022', freq='D')
data = {'date': date_rng, 'value': [100, 110, 105, 115, 120, 125, 130, 135, 140, 145]}
df = pd.DataFrame(data)
df.set_index('date', inplace=True)

# Lag Features
df['value_lag1'] = df['value'].shift(1)

# Rolling Statistics
df['value_rolling_mean'] = df['value'].rolling(window=3).mean()

print(df)
```

上述示例展示了通过使用`pandas`和`scikit-learn`的先进特征工程技术的实际应用。通过应用这些方法，你可以提高模型的预测能力。

## 实用技巧和最佳实践

在进行特征工程过程中，有一些简单但重要的技巧需要记住。

+   迭代：特征工程是一个试错过程，每次迭代都会变得更好。测试不同的特征工程想法，以找到最佳的特征集。

+   领域知识：在创建特征时，利用那些对主题了解深入的专家的知识。有时，领域特定的知识能够捕捉到微妙的关系。

+   特征的验证和理解：通过了解哪些特征对模型最重要，你可以做出重要决策。确定特征重要性的工具包括：

    +   SHAP（Shapley 加性解释）：帮助量化每个特征在预测中的贡献

    +   LIME（局部可解释模型无关解释）：以简单的语言展示模型预测的含义

为了获得既良好又简单易懂的结果，复杂性和可解释性的最佳混合是必要的。

## 结论

本简短指南涵盖了基本的特征工程概念，以及基础和高级技术、实用建议和最佳实践。涵盖了许多人认为最重要的特征工程实践——处理缺失信息、类别数据编码、数据标准化和新特征创建。

特征工程是一个随着执行而不断改进的实践，我希望你能从中获得一些提升数据科学技能的经验。我鼓励你将这些技术应用到自己的工作中，并从你的经验中学习。

请记住，虽然具体的百分比因人而异，但任何机器学习项目的大部分时间都花在数据准备和预处理阶段。特征工程是这个漫长阶段的一部分，因此应该以其应有的重要性来看待。学习将特征工程视为建模过程中的一个辅助手段，应能让新手更容易理解。

祝工程愉快！

[](https://www.linkedin.com/in/mattmayo13/)****[Matthew Mayo](https://www.kdnuggets.com/wp-content/uploads/./profile-pic.jpg)**** ([**@mattmayo13**](https://twitter.com/mattmayo13)) 拥有计算机科学硕士学位和数据挖掘研究生文凭。作为[KDnuggets](https://www.kdnuggets.com/)与[Statology](https://www.statology.org/)的主编，以及[Machine Learning Mastery](https://machinelearningmastery.com/)的贡献编辑，Matthew 的目标是让复杂的数据科学概念变得易于理解。他的职业兴趣包括自然语言处理、语言模型、机器学习算法和探索新兴的人工智能。他致力于在数据科学社区中普及知识。Matthew 从 6 岁开始编程。

### 更多相关内容

+   [2022 年特征存储峰会：关于特征工程的免费会议](https://www.kdnuggets.com/2022/10/hopsworks-feature-store-summit-2022-free-conference-feature-engineering.html)

+   [为多变量时间序列构建可处理的特征工程管道](https://www.kdnuggets.com/2022/03/building-tractable-feature-engineering-pipeline-multivariate-time-series.html)

+   [使用 RAPIDS cuDF 在特征工程中利用 GPU](https://www.kdnuggets.com/2023/06/rapids-cudf-leverage-gpu-feature-engineering.html)

+   [机器学习中特征工程的实用方法](https://www.kdnuggets.com/2023/07/practical-approach-feature-engineering-machine-learning.html)

+   [初学者免费数据工程课程](https://www.kdnuggets.com/free-data-engineering-course-for-beginners)

+   [实时 AI 与机器学习的特征存储](https://www.kdnuggets.com/2022/03/feature-stores-realtime-ai-machine-learning.html)
