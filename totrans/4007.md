# 掌握数据清理的7个步骤，使用Python和Pandas

> 原文：[https://www.kdnuggets.com/7-steps-to-mastering-data-cleaning-with-python-and-pandas](https://www.kdnuggets.com/7-steps-to-mastering-data-cleaning-with-python-and-pandas)

![7-steps-pandas](../Images/044f1484c6d7a0a69ebf6b54bfd06a62.png)

图片由作者提供

Pandas是最广泛使用的Python数据分析和操作库。但从源头读取的数据通常需要一系列的数据清理步骤——在你能够分析数据以获得洞察、回答业务问题或构建机器学习模型之前。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行IT管理

* * *

本指南将数据清理过程拆解为7个实际步骤。我们将创建一个示例数据集并逐步完成数据清理。

让我们开始吧！

## 创建一个示例DataFrame

[Colab笔记本链接](https://github.com/balapriyac/data-science-tutorials/blob/main/pandas/data_cleaning_with_pandas.ipynb)

在开始实际的数据清理步骤之前，让我们创建一个包含员工记录的pandas数据框。我们将使用Faker进行合成数据生成。因此，请先安装它：

```py
!pip install Faker
```

如果你愿意，可以跟随相同的示例，也可以使用你选择的数据集。这里是生成1000条记录的代码：

```py
import pandas as pd
from faker import Faker
import random

# Initialize Faker to generate synthetic data
fake = Faker()

# Set seed for reproducibility
Faker.seed(42)

# Generate synthetic data
data = []
for _ in range(1000):
    data.append({
        'Name': fake.name(),
        'Age': random.randint(18, 70),
        'Email': fake.email(),
        'Phone': fake.phone_number(),
        'Address': fake.address(),
        'Salary': random.randint(20000, 150000),
        'Join_Date': fake.date_this_decade(),
        'Employment_Status': random.choice(['Full-Time', 'Part-Time', 'Contract']),
        'Department': random.choice(['IT', 'Engineering','Finance', 'HR', 'Marketing'])
    }) 
```

让我们稍微调整一下这个数据框，以引入缺失值、重复记录、异常值等：

```py
# Let's tweak the records a bit!
# Introduce missing values
for i in random.sample(range(len(data)), 50):
    data[i]['Email'] = None

# Introduce duplicate records
data.extend(random.sample(data, 100))

# Introduce outliers
for i in random.sample(range(len(data)), 20):
    data[i]['Salary'] = random.randint(200000, 500000)
```

现在让我们创建一个包含这些记录的数据框：

```py
# Create dataframe
df = pd.DataFrame(data)
```

请注意，我们设置的是Faker的种子，而不是随机模块。因此，你生成的记录会有一些随机性。

## 第一步：理解数据

**第0步始终是理解你要解决的业务问题/问题**。一旦你知道这一点，你就可以开始处理你已读入pandas数据框的数据。

但在你对数据集进行任何有意义的操作之前，首先获取数据集的总体概述是很重要的。这包括获取不同字段的一些基本信息和记录的总数，检查数据框的头部等。

这里我们对数据框运行`info()`方法：

```py
df.info()
```

```py
Output >>>
 <class>RangeIndex: 1100 entries, 0 to 1099
Data columns (total 9 columns):
 #   Column             Non-Null Count  Dtype 
---  ------             --------------  ----- 
 0   Name               1100 non-null   object
 1   Age                1100 non-null   int64 
 2   Email              1047 non-null   object
 3   Phone              1100 non-null   object
 4   Address            1100 non-null   object
 5   Salary             1100 non-null   int64 
 6   Join_Date          1100 non-null   object
 7   Employment_Status  1100 non-null   object
 8   Department         1100 non-null   object
dtypes: int64(2), object(7)
memory usage: 77.5+ KB</class>
```

检查数据框的头部：

```py
df.head()
```

![df-head](../Images/27e6ebda1c0d9224cc96b742098b8b2f.png)

df.head()的输出

## 第二步：处理重复项

重复记录是一个常见问题，会影响分析结果。因此，我们应该识别并删除所有重复记录，以便我们只处理唯一的数据记录。

下面是如何在数据框中查找所有重复项，然后删除所有重复项的方法：

```py
# Check for duplicate rows
duplicates = df.duplicated().sum()
print("Number of duplicate rows:", duplicates)

# Removing duplicate rows
df.drop_duplicates(inplace=True)
```

```py
Output >>>
Number of duplicate rows: 100
```

## 第3步：处理缺失数据

缺失数据是许多数据科学项目中的常见数据质量问题。如果你快速查看上一步的`info()`方法的结果，你应该会看到非空对象的数量在所有字段中并不相同，并且在电子邮件列中有缺失值。我们仍然会得到准确的计数。

要获取每列缺失值的数量，你可以运行：

```py
# Check for missing values
missing_values = df.isna().sum()
print("Missing Values:")
print(missing_values)
```

```py
Output >>>
Missing Values:
Name                  0
Age                   0
Email                50
Phone                 0
Address               0
Salary                0
Join_Date             0
Employment_Status     0
Department            0
dtype: int64
```

如果一个或多个数值列中有缺失值，我们可以应用合适的填补技术。但由于'Email'字段缺失，我们暂时将缺失的电子邮件设置为占位符电子邮件，如下所示：

```py
 # Handling missing values by filling with a placeholder
df['Email'].fillna('unknown@example.com', inplace=True)
```

## 第4步：数据转换

当你处理数据集时，可能会有一个或多个字段没有预期的数据类型。在我们的示例数据框中，'Join_Date'字段必须被转换成有效的日期时间对象：

```py
# Convert 'Join_Date' to datetime
df['Join_Date'] = pd.to_datetime(df['Join_Date'])
print("Join_Date after conversion:")
print(df['Join_Date'].head())
```

```py
Output >>>
Join_Date after conversion:
0   2023-07-12
1   2020-12-31
2   2024-05-09
3   2021-01-19
4   2023-10-04
Name: Join_Date, dtype: datetime64[ns]
```

由于我们有加入日期，实际上拥有一个`Years_Employed`列会更有帮助，如下所示：

```py
# Creating a new feature 'Years_Employed' based on 'Join_Date'
df['Years_Employed'] = pd.Timestamp.now().year - df['Join_Date'].dt.year
print("New feature 'Years_Employed':")
print(df[['Join_Date', 'Years_Employed']].head())
```

```py
Output >>>
New feature 'Years_Employed':
   Join_Date  Years_Employed
0 2023-07-12               1
1 2020-12-31               4
2 2024-05-09               0
3 2021-01-19               3
4 2023-10-04               1
```

## 第5步：清理文本数据

字符串字段中经常会遇到格式不一致或类似的问题。清理文本可以像应用大小写转换一样简单，也可以像编写复杂的正则表达式以使字符串符合要求格式一样困难。

在我们拥有的示例数据框中，我们看到'地址'列包含许多‘\n’字符，这些字符影响了可读性。所以让我们将它们替换为空格，如下所示：

```py
# Clean address strings
df['Address'] = df['Address'].str.replace('\n', ' ', regex=False)
print("Address after text cleaning:")
print(df['Address'].head())
```

```py
Output >>>
Address after text cleaning:
0    79402 Peterson Drives Apt. 511 Davisstad, PA 35172
1     55341 Amanda Gardens Apt. 764 Lake Mark, WI 07832
2                 710 Eric Estate Carlsonfurt, MS 78605
3                 809 Burns Creek Natashaport, IA 08093
4    8713 Caleb Brooks Apt. 930 Lake Crystalbury, CA...
Name: Address, dtype: object
```

## 第6步：处理异常值

如果你向上滚动，你会看到我们将'薪水'列中的一些值设置得极高。这些异常值也应被识别和适当处理，以免影响分析结果。

你通常需要考虑*什么*使数据点成为异常值（如果是错误的数据输入，还是它们实际上是有效值而不是异常值）。然后你可以选择处理它们：删除异常值记录或获取包含异常值的行的子集并单独分析。

让我们使用z-score找到那些比均值偏离三倍标准差以上的薪资值：

```py
# Detecting outliers using z-score
z_scores = (df['Salary'] - df['Salary'].mean()) / df['Salary'].std()
outliers = df[abs(z_scores) > 3]
print("Outliers based on Salary:")
print(outliers[['Name', 'Salary']].head())
```

```py
Output >>>
Outliers based on Salary:
                Name  Salary
16    Michael Powell  414854
131    Holly Jimenez  258727
240  Daniel Williams  371500
328    Walter Bishop  332554
352     Ashley Munoz  278539
```

## 第7步：合并数据

在大多数项目中，你拥有的数据可能*不是*你想用于分析的数据。你必须找到最相关的字段并从其他数据框中合并数据，以获得更多有用的数据用于分析。

作为一个快速练习，创建另一个相关的数据框，并在一个公共列上与现有数据框合并，使合并具有意义。pandas中的合并与SQL中的连接非常相似，因此我建议你将其作为练习尝试！

## 总结

这就是本教程的全部内容！我们创建了一个包含记录的示例数据框，并逐步完成了各种数据清理步骤。以下是步骤概述：理解数据、处理重复数据、缺失值、数据转换、清理文本数据、处理异常值和合并数据。

如果你想了解关于 pandas 数据整理的所有内容，可以查看 [掌握 Pandas 和 Python 数据整理的 7 个步骤](https://www.kdnuggets.com/7-steps-to-mastering-data-wrangling-with-pandas-and-python)。

**[](https://twitter.com/balawc27)**[Bala Priya C](https://www.kdnuggets.com/wp-content/uploads/bala-priya-author-image-update-230821.jpg) 是来自印度的开发者和技术作家。她喜欢在数学、编程、数据科学和内容创作的交汇点上工作。她的兴趣和专长领域包括 DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编程和咖啡！目前，她正致力于学习并通过编写教程、操作指南、观点文章等与开发者社区分享她的知识。Bala 还制作引人入胜的资源概述和编程教程。

### 更多相关主题

+   [掌握 Pandas 和 Python 数据整理的 7 个步骤](https://www.kdnuggets.com/7-steps-to-mastering-data-wrangling-with-pandas-and-python)

+   [掌握数据清理和预处理技术的 7 个步骤](https://www.kdnuggets.com/2023/08/7-steps-mastering-data-cleaning-preprocessing-techniques.html)

+   [关于掌握 SQL、Python、数据清理、数据整理和探索性数据分析的指南合集](https://www.kdnuggets.com/collection-of-guides-on-mastering-sql-python-data-cleaning-data-wrangling-and-exploratory-data-analysis)

+   [掌握 Python 数据清理的艺术](https://www.kdnuggets.com/mastering-the-art-of-data-cleaning-in-python)

+   [KDnuggets™ 新闻 22:n05，2 月 2 日：掌握机器学习的 7 个步骤…](https://www.kdnuggets.com/2022/n05.html)

+   [掌握数据科学 Python 的 7 个步骤](https://www.kdnuggets.com/2022/06/7-steps-mastering-python-data-science.html)
