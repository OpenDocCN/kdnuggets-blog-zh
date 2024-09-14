# 7 个重要的数据质量检查与 Pandas

> 原文：[`www.kdnuggets.com/7-essential-data-quality-checks-with-pandas`](https://www.kdnuggets.com/7-essential-data-quality-checks-with-pandas)

![7 个重要的数据质量检查与 Pandas](img/c9dfcc4434fa36f099ef3af225d45b7b.png)

图片由作者提供

作为数据专业人士，你可能熟悉数据质量差的成本。对于所有数据项目——无论大小——你都应该执行必要的数据质量检查。

* * *

## 我们的前三推荐课程

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的信息技术

* * *

数据质量评估有专门的库和框架。但是，如果你是初学者，你可以使用 pandas 运行简单但重要的数据质量检查。本教程将教你如何做。

我们将使用 [加州住房数据集](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.fetch_california_housing.html) 作为本教程的示例。

# 加州住房数据集概述

我们将使用来自 Scikit-learn 的 [数据集模块](https://scikit-learn.org/stable/datasets/real_world.html#real-world-datasets) 的加州住房数据集。该数据集包含超过 20,000 条记录，涵盖八个数值特征和一个目标中位数房价。

让我们将数据集读取到 pandas 数据框 `df` 中：

```py
from sklearn.datasets import fetch_california_housing
import pandas as pd

# Fetch the California housing dataset
data = fetch_california_housing()

# Convert the dataset to a Pandas DataFrame
df = pd.DataFrame(data.data, columns=data.feature_names)

# Add target column
df['MedHouseVal'] = data.target
```

要获取数据集的详细描述，请运行 `data.DESCR`，如下所示：

```py
print(data.DESCR)
```

![7 个重要的数据质量检查与 Pandas](img/f75bc0f8c1f2fd389c4abd2454c2c71d.png)

data.DESCR 的输出

让我们获取数据集的一些基本信息：

```py
df.info()
```

这里是输出：

```py
Output >>>

 <class>RangeIndex: 20640 entries, 0 to 20639
Data columns (total 9 columns):
 #   Column   	Non-Null Count  Dtype  
---  ------   	--------------  -----  
 0   MedInc   	20640 non-null  float64
 1   HouseAge 	20640 non-null  float64
 2   AveRooms 	20640 non-null  float64
 3   AveBedrms	20640 non-null  float64
 4   Population   20640 non-null  float64
 5   AveOccup 	20640 non-null  float64
 6   Latitude 	20640 non-null  float64
 7   Longitude	20640 non-null  float64
 8   MedHouseVal  20640 non-null  float64
dtypes: float64(9)
memory usage: 1.4 MB</class>
```

由于我们有数值特征，让我们还使用 `describe()` 方法获取摘要统计：

```py
df.describe()
```

![7 个重要的数据质量检查与 Pandas](img/116989112eb8a816cb5b4b701c5f246c.png)

df.describe() 的输出

# 1\. 检查缺失值

现实世界的数据集通常会有缺失值。为了分析数据和建立模型，你需要处理这些缺失值。

为了确保数据质量，你应该检查缺失值的比例是否在特定的容忍范围内。然后，你可以使用适当的插补策略来填补缺失值。

因此，第一步是检查数据集中所有特征的缺失值。

这段代码检查数据框 `df` 中每一列的缺失值：

```py
# Check for missing values in the DataFrame
missing_values = df.isnull().sum()
print("Missing Values:")
print(missing_values)
```

结果是一个 pandas 系列，显示了每一列的缺失值计数：

```py
Output >>>

Missing Values:
MedInc     	0
HouseAge   	0
AveRooms   	0
AveBedrms  	0
Population 	0
AveOccup   	0
Latitude   	0
Longitude  	0
MedHouseVal	0
dtype: int64
```

如所见，此数据集中没有缺失值。

# 2\. 识别重复记录

数据集中的重复记录可能会扭曲分析结果。因此，你应该根据需要检查并删除重复记录。

这是用于识别和返回 `df` 中重复行的代码。如果存在任何重复行，它们将包含在结果中：

```py
# Check for duplicate rows in the DataFrame
duplicate_rows = df[df.duplicated()]
print("Duplicate Rows:")
print(duplicate_rows)
```

结果是一个空的 DataFrame。这意味着数据集中没有重复记录：

```py
Output >>>

Duplicate Rows:
Empty DataFrame
Columns: [MedInc, HouseAge, AveRooms, AveBedrms, Population, AveOccup, Latitude, Longitude, MedHouseVal]
Index: []
```

# 3\. 检查数据类型

在分析数据集时，你通常需要转换或缩放一个或多个特征。为了避免在执行这些操作时出现意外错误，重要的是检查所有列是否都是预期的数据类型。

这段代码检查 DataFrame `df` 中每一列的数据类型：

```py
# Check data types of each column in the DataFrame
data_types = df.dtypes
print("Data Types:")
print(data_types)
```

在这里，所有数值特征都是 `float` 数据类型，如预期：

```py
Output >>>

Data Types:
MedInc     	float64
HouseAge   	float64
AveRooms   	float64
AveBedrms  	float64
Population 	float64
AveOccup   	float64
Latitude   	float64
Longitude  	float64
MedHouseVal	float64
dtype: object
```

# 4\. 检查异常值

异常值是数据集中与其他点显著不同的数据点。如果你还记得，我们在 DataFrame 上运行了 `describe()` 方法。

根据四分位数值和最大值，你可以识别出某些特征包含异常值。具体来说，这些特征：

+   MedInc

+   AveRooms

+   AveBedrms

+   人口

处理异常值的一种方法是使用 [四分位距](https://www.khanacademy.org/math/cc-sixth-grade-math/cc-6th-data-statistics/cc-6th/a/interquartile-range-review)，即第 75 百分位数与第 25 百分位数之间的差异。如果 Q1 是第 25 百分位数，Q3 是第 75 百分位数，则四分位距为：Q3 - Q1。

然后我们使用四分位数和 IQR 来定义区间 `[Q1 - 1.5 * IQR, Q3 + 1.5 * IQR]`。所有超出此范围的点都是异常值。

```py
columns_to_check = ['MedInc', 'AveRooms', 'AveBedrms', 'Population']

# Function to find records with outliers
def find_outliers_pandas(data, column):
	Q1 = data[column].quantile(0.25)
	Q3 = data[column].quantile(0.75)
	IQR = Q3 - Q1
	lower_bound = Q1 - 1.5 * IQR
	upper_bound = Q3 + 1.5 * IQR
	outliers = data[(data[column] < lower_bound) | (data[column] > upper_bound)]
	return outliers

# Find records with outliers for each specified column
outliers_dict = {}

for column in columns_to_check:
	outliers_dict[column] = find_outliers_pandas(df, column)

# Print the records with outliers for each column
for column, outliers in outliers_dict.items():
	print(f"Outliers in '{column}':")
	print(outliers)
	print("\n")
```

![7 个使用 Pandas 进行的基本数据质量检查](img/2c799aaf132686d2ac77a2b57ab6cb4a.png)

'AveRooms' 列中的异常值 | 异常值检查的截断输出

# 5\. 验证数值范围

对于数值特征，一个重要的检查是验证范围。这确保了特征的所有观测值都在预期范围内。

这段代码验证了 'MedInc' 值是否在预期范围内，并识别出不符合此标准的数据点：

```py
# Check numerical value range for the 'MedInc' column
valid_range = (0, 16)  
value_range_check = df[~df['MedInc'].between(*valid_range)]
print("Value Range Check (MedInc):")
print(value_range_check)
```

你可以尝试其他你选择的数值特征。但我们看到 'MedInc' 列中的所有值都在预期范围内：

```py
Output >>>

Value Range Check (MedInc):
Empty DataFrame
Columns: [MedInc, HouseAge, AveRooms, AveBedrms, Population, AveOccup, Latitude, Longitude, MedHouseVal]
Index: []
```

# 6\. 检查跨列依赖性

大多数数据集包含相关特征。因此，基于列（或特征）之间逻辑相关关系的检查很重要。

尽管特征——单独来看——可能在预期范围内，但它们之间的关系可能不一致。

这是我们数据集的一个示例。在有效记录中，‘AveRooms’ 应通常大于或等于 ‘AveBedRms’。

```py
# AveRooms should not be smaller than AveBedrooms
invalid_data = df[df['AveRooms'] < df['AveBedrms']]
print("Invalid Records (AveRooms < AveBedrms):")
print(invalid_data)
```

在我们处理的加州住房数据集中，我们看到没有这些无效记录：

```py
Output >>>

Invalid Records (AveRooms < AveBedrms):
Empty DataFrame
Columns: [MedInc, HouseAge, AveRooms, AveBedrms, Population, AveOccup, Latitude, Longitude, MedHouseVal]
Index: []
```

# 7\. 检查不一致的数据输入

不一致的数据输入是大多数数据集中常见的数据质量问题。示例包括：

+   datetime 列中的格式不一致

+   分类变量值的不一致记录

+   以不同单位记录的读数

在我们的数据集中，我们已经验证了列的数据类型并识别出了异常值。但你也可以进行不一致数据录入的检查。

让我们来一个简单的例子，检查所有日期条目是否具有一致的格式。

在这里，我们使用正则表达式与 pandas 的 `apply()` 函数结合，以检查所有日期条目是否符合 `YYYY-MM-DD` 格式：

```py
import pandas as pd
import re

data = {'Date': ['2023-10-29', '2023-11-15', '23-10-2023', '2023/10/29', '2023-10-30']}
df = pd.DataFrame(data)

# Define the expected date format
date_format_pattern = r'^\d{4}-\d{2}-\d{2}$'  # YYYY-MM-DD format

# Function to check if a date value matches the expected format
def check_date_format(date_str, date_format_pattern):
	return re.match(date_format_pattern, date_str) is not None

# Apply the format check to the 'Date' column
date_format_check = df['Date'].apply(lambda x: check_date_format(x, date_format_pattern))

# Identify and retrieve entries that do not follow the expected format
non_adherent_dates = df[~date_format_check]

if not non_adherent_dates.empty:
	print("Entries that do not follow the expected format:")
	print(non_adherent_dates)
else:
	print("All dates are in the expected format.")
```

这将返回不符合预期格式的条目：

```py
Output >>>

Entries that do not follow the expected format:
     	Date
2  23-10-2023
3  2023/10/29
```

# 总结

在本教程中，我们讨论了 pandas 的常见数据质量检查。

当你在进行较小的数据分析项目时，这些使用 pandas 的数据质量检查是一个很好的起点。根据问题和数据集，你可以加入额外的检查。

如果你有兴趣学习数据分析，查看指南 7 步掌握 Pandas 和 Python 数据整理。

**[](https://twitter.com/balawc27)**[Bala Priya C](https://www.kdnuggets.com/wp-content/uploads/bala-priya-author-image-update-230821.jpg)** 是来自印度的开发者和技术作者。她喜欢在数学、编程、数据科学和内容创作的交汇点上工作。她的兴趣和专长包括 DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编程和喝咖啡！目前，她正在通过撰写教程、操作指南、观点文章等与开发者社区分享她的知识。Bala 还创建了引人入胜的资源概述和编码教程。

### 更多相关主题

+   [数据质量维度：用伟大期望保证你的数据质量](https://www.kdnuggets.com/2023/03/data-quality-dimensions-assuring-data-quality-great-expectations.html)

+   [用于合规检查的深度学习：有什么新变化？](https://www.kdnuggets.com/2022/05/deep-learning-compliance-checks-new.html)

+   [检测数据漂移以确保生产 ML 模型质量（使用 Eurybia）](https://www.kdnuggets.com/2022/07/detecting-data-drift-ensuring-production-ml-model-quality-eurybia.html)

+   [免费 4 周数据科学课程：AI 质量管理](https://www.kdnuggets.com/2022/02/truera-free-4-week-data-science-course-ai-quality-management.html)

+   [数据质量在成功的机器学习模型中的重要性…](https://www.kdnuggets.com/2022/03/significance-data-quality-making-successful-machine-learning-model.html)

+   [数据质量：好、坏、丑](https://www.kdnuggets.com/2022/01/data-quality-good-bad-ugly.html)
