# 利用 ChatGPT 进行自动化数据清理和预处理

> 原文：[`www.kdnuggets.com/2023/08/harnessing-chatgpt-automated-data-cleaning-preprocessing.html`](https://www.kdnuggets.com/2023/08/harnessing-chatgpt-automated-data-cleaning-preprocessing.html)

![利用 ChatGPT 进行自动化数据清理和预处理](img/253506759c944a8380bc213fa5f4eaa5.png)

作者提供的图像

ChatGPT 已经成为一个可以用于多种应用的瑞士军刀，而且有广阔的空间将 ChatGPT 集成到数据科学工作流中。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

如果你曾经在真实世界的数据集上训练过机器学习模型，你知道数据清理和预处理的步骤对于构建稳健的机器学习模型至关重要。在本指南中，我们将看到如何利用 ChatGPT 在示例数据集上执行这些任务。

我们将使用[UCI 机器学习库中的银行营销数据集](https://archive.ics.uci.edu/dataset/222/bank+marketing)，并提示 ChatGPT 完成以下任务：

+   获取并加载数据集

+   检查缺失值

+   编码分类变量

你可以通过登录[OpenAI 账户](https://platform.openai.com/login)并启动一个新的 ChatGPT 会话来跟随学习。如果你愿意，也可以在[Google Colab](https://colab.research.google.com/)中编写代码。

# 获取并加载数据集

数据集有[几种不同版本](https://archive.ics.uci.edu/dataset/222/bank+marketing)。我们将使用 bank-full.csv。

> **提示**：今天我们将对一个真实世界的数据集进行数据清理和预处理。编写代码完成以下任务：1\. 从 UCI 机器学习库中获取银行营销数据集。使用 requests 或 urllib 等库。2\. 下载 zip 文件，解压内容，并将 bank-full.csv 文件读取到 pandas 数据框中（称之为 data）。

获取和读取数据集的代码如下：

```py
import pandas as pd
import urllib
import zipfile

# URL of the dataset
url = "https://archive.ics.uci.edu/ml/machine-learning-databases/00222/bank.zip"

# Fetch and extract the dataset
urllib.request.urlretrieve(url, "bank.zip")

with zipfile.ZipFile("bank.zip", "r") as zip_ref:
    zip_ref.extractall()

# Load the dataset into a pandas DataFrame
data = pd.read_csv("bank-full.csv", delimiter=";",skipinitialspace=True)
```

# 获取数据集的基本信息

让我们更好地理解数据集。

```py
# Display the first few rows of the dataset
print(data.head())
```

这将输出：

![利用 ChatGPT 进行自动化数据清理和预处理](img/c943e5830c9d9e43f5e1034a1e06e925.png)

数据.head()的截断输出

> **提示**：使用 pandas 获取数据框的维度、列的描述性统计信息以及各列的数据类型。

这一步骤实际上不需要提示 ChatGPT，因为 pandas 方法相对简单。

```py
# Get the dimensions of the dataset (rows, columns)
print(data.shape)
```

```py
Output >>> (45211, 17)
```

我们有超过 45000 条记录和 16 个特征（17 包括了输出标签）。

```py
# Get statistical summary of numerical columns
print(data.describe())
```

![利用 ChatGPT 进行自动化数据清理和预处理](img/cb09912fbc4731c724029ebaedb9c3dd.png)

数据 `data.describe()` 的截断输出

了解各种列的数据类型也很有帮助。我们在对分类变量进行编码时会更详细地查看它们。

```py
# Check data types of each column
print(data.dtypes)
```

```py
Output >>>

age           int64
job          object
marital      object
education    object
default      object
balance       int64
housing      object
loan         object
contact      object
day           int64
month        object
duration      int64
campaign      int64
pdays         int64
previous      int64
poutcome     object
y            object
dtype: object
```

到目前为止，我们已经了解了银行营销数据集。输出标签表示客户是否会订阅定期存款。数据集包含几个特征，如年龄、月份、教育、婚姻状况、之前活动的结果等。

# 检查缺失值

> **提示**：我想知道每列中的缺失值数量。请给我代码来实现。使用 pandas。

```py
# Check for missing values in each column
missing_values = data.isnull().sum()
print(missing_values)
```

以下是输出：

```py
Output >>>

age          0
job          0
marital      0
education    0
default      0
balance      0
housing      0
loan         0
contact      0
day          0
month        0
duration     0
campaign     0
pdays        0
previous     0
poutcome     0
y            0
dtype: int64
```

这个版本的银行营销数据集——拥有超过 45000 条记录——没有任何缺失值。然而，在实践中，大多数现实世界的数据集都有缺失值。你应该使用合适的插补技术来处理缺失值。

作为一个可选练习，你可以在这里添加一个步骤，让 ChatGPT 从一部分列中删除一小部分值，以便你可以练习如何处理缺失值。

# 编码分类变量

下一步是对数据集中的分类变量进行编码。我们将从获取所有分类列的列表开始。

> **提示**：给出代码以获取数据集中所有分类列的列表。

```py
# Count the number of categorical variables
categorical_columns = data.select_dtypes(include=['object']).columns
print(categorical_columns)
num_categorical_variables = len(categorical_columns)

# Display the count
print("Number of categorical variables:", num_categorical_variables)
```

分类列列表还包括输出标签 **y**。但我们先关注其他分类变量。

```py
Output >>>
Index(['job', 'marital', 'education', 'default', 'housing', 'loan', 'contact',
       'month', 'poutcome', 'y'],
      dtype='object')
Number of categorical variables: 10
```

## 更深入了解分类变量的值

接下来，让我们查看每个分类变量的值。

> **提示**：我想了解每个分类变量的各种值。请给出 pandas 代码来实现。

```py
# Get the different values and their counts for each categorical column
categorical_columns = ['job', 'marital', 'education', 'default', 'housing', 'loan', 'contact', 'month', 'poutcome']
for column in categorical_columns:
    value_counts = data[column].value_counts()
    print(f"Column: {column}")
    print(value_counts,"\n")
```

输出难以解析：

```py
Output >>>

Column: job
blue-collar      9732
management       9458
technician       7597
admin.           5171
services         4154
retired          2264
self-employed    1579
entrepreneur     1487
unemployed       1303
housemaid        1240
student           938
unknown           288
Name: job, dtype: int64 

...
...
...
Column: poutcome
unknown    36959
failure     4901
other       1840
success     1511
Name: poutcome, dtype: int64 
```

## 可视化分类变量的值

让我们让 ChatGPT 创建一个可视化图表，以便更容易理解分类变量的值。

> **提示**：我想要一个图表，展示每个分类变量的值。排除输出标签 y。包括所有其他分类变量。

那我们开始吧！

```py
import matplotlib.pyplot as plt

# Get the different values and their counts for each categorical column
categorical_columns = ['job', 'marital', 'education', 'default', 'housing', 'loan', 'contact', 'month', 'poutcome']

# Set up the subplot grid
fig, axes = plt.subplots(3, 3, figsize=(10,8))

# Iterate over each categorical column and plot the value counts
for i, column in enumerate(categorical_columns):
    row = i // 3
    col = i % 3
    value_counts = data[column].value_counts()
    ax = axes[row, col]
    value_counts.plot(kind='bar', ax=ax)
    ax.set_xlabel(column)
    ax.set_ylabel('Count')
    ax.set_title(f'{column.title()} Value Counts')

# Adjust spacing between subplots
plt.tight_layout()

# Show the plot
plt.show()
```

![利用 ChatGPT 进行自动化数据清理和预处理](img/e27c7a292f8f5588afd63b06a0a27503.png)

分类变量的值

## 独热编码

对于部分分类列，我们可以使用独热编码。

> **提示**：给出 pandas 代码以对一组列进行独热编码。

我们使用 pandas 中的 `get_dummies` 对以下列进行独热编码：

```py
# Select the categorical columns (excluding education and poutcome)
categorical_columns = ['job', 'marital', 'default', 'housing', 'loan', 'contact']

# Perform one-hot encoding
encoded_data = pd.get_dummies(data, columns=categorical_columns)
```

> **提示**：我想打印出新添加列的前几行。编写代码来实现这一点。

```py
# Select the newly added columns
new_columns = encoded_data.columns.difference(data.columns)

# Print the head of the newly added columns
print(encoded_data[new_columns].head())
```

![利用 ChatGPT 进行自动化数据清理和预处理](img/9bc20dfbab384edf86a1e63b3a860186.png)

编码后的数据 `encoded_data['new_colums'].head()` 的截断输出

## 定义自定义映射

对于像‘education’和‘poutcome’（先前结果）这样的变量列，使用自定义映射比独热编码更好，这样可以在值之间进行比较。

此外，当我们使用独热编码来编码一个取 k 个不同值的分类变量时，会创建 k 个新列。对于像‘month’这样的分类列，这样做会显得过于冗余而没有增加太多信息。

因此，对于这两种情况，我们将定义一个自定义映射，然后转换值。我们可以提示 ChatGPT 来获取一个通用代码片段，然后对其进行修改以定义特定列的映射。

> **提示**：我想对一些分类变量进行自定义映射。首先，给我一个通用代码片段，用于定义从分类列中的唯一值到不同输出值的自定义映射。然后我们应该使用此映射来转换列中的值。

```py
# Define the custom mapping dictionary
custom_mapping = {
    'category1': 'value1',
    'category2': 'value2',
    'category3': 'value3',
    # Add more categories and corresponding values as needed
}

# Specify the categorical column
categorical_column = 'column_name'  # Replace 'column_name' with the actual column name

# Perform the mapping using the custom mapping dictionary
data[categorical_column] = data[categorical_column].map(custom_mapping)
```

对于‘month’列，让我们将月份字符串转换如下：

```py
# Define the mapping dictionary for months
month_mapping = {
    'jan': 1,
    'feb': 2,
    'mar': 3,
    'apr': 4,
    'may': 5,
    'jun': 6,
    'jul': 7,
    'aug': 8,
    'sep': 9,
    'oct': 10,
    'nov': 11,
    'dec': 12
}

# Map the values in the month column
encoded_data['month'] = encoded_data['month'].map(month_mapping)
```

让我们将‘poutcome’和‘education’列映射到数值，如下所示：

```py
# Define the custom mapping for poutcome and education
poutcome_mapping = {
    'unknown': 0,
    'failure': 1,
    'other': 2,
    'success': 3
}

education_mapping = {
    'unknown': 0,
    'primary': 1,
    'secondary': 2,
    'tertiary': 3
}

# Perform ordinal encoding using pandas map
encoded_data['poutcome'] = encoded_data['poutcome'].map(poutcome_mapping)
encoded_data['education'] = encoded_data['education'].map(education_mapping)

# Select the newly added columns
new_columns = ['month','poutcome', 'education']

# Print the head of the newly added columns
print(encoded_data[new_columns].head(10))
```

## 编码输出标签

让我们也将输出标签‘yes’和‘no’分别映射为 1 和 0。

```py
encoded_data['y'] = encoded_data['y'].replace({'no': 0, 'yes': 1})
print(encoded_data['y'])
```

```py
Output >>>

0        0
1        0
2        0
3        0
4        0
        ..
45206    1
45207    1
45208    1
45209    0
45210    0
Name: y, Length: 45211, dtype: int64
```

记住我们对‘housing’，‘default’和‘loan’列使用了独热编码。由于这些列也取‘yes’和‘no’值，你还可以将‘yes’和‘no’分别映射为 1 和 0。

## 可视化类别标签的分布

检查类别标签的分布也是有帮助的，这样我们在构建模型时可以根据需要考虑类别不平衡。

> **提示**：我希望了解类别标签的分布。请给我生成适当图表的代码。

下面是生成 Seaborn 中的计数图的代码：

```py
import seaborn as sns
import matplotlib.pyplot as plt

# Plot the distribution of output labels
sns.countplot(data=data, x='y')
plt.xlabel('Output Label')
plt.ylabel('Count')
plt.title('Output Label Distribution')
plt.show()
```

![利用 ChatGPT 实现自动化数据清洗和预处理](img/3124a915de369d6eac9b0510c9911766.png)

类别标签的分布

我们看到存在类别不平衡。因此，如果你正在构建模型，应该确保使用适当的重采样方法来解决类别不平衡问题。

# 通用数据清洗和预处理管道

尽管数据清洗和预处理需要开发者的努力，但让我们尝试得到一个适用于简单数据集的通用步骤序列。

> **提示**：你能否给我一个基于我们目前所做的通用数据清洗和预处理管道。获取数据集的基本信息，检查并处理缺失值，识别分类列，并对分类列进行编码。仅使用 pandas。

这里是：

```py
import pandas as pd

# Load the dataset into a DataFrame
data = pd.read_csv('data.csv')

# Step 1: Basic Info
print(data.info())

# Step 2: Missing Values
missing_values = data.isnull().sum()
print(missing_values)

# Step 3: Handle Missing Values
# Identify columns with missing values
columns_with_missing_values = missing_values[missing_values > 0].index

# Impute missing values using fillna() or other suitable method
data[columns_with_missing_values] = data[columns_with_missing_values].fillna(data[columns_with_missing_values].mean())

# Step 4: Get Categorical Columns
categorical_columns = data.select_dtypes(include=['object']).columns

# Step 5: Encode Categorical Columns
# Perform ordinal encoding or one-hot encoding using pandas' get_dummies() function
encoded_data = pd.get_dummies(data, columns=categorical_columns)

# Print the encoded data
print(encoded_data.head())
```

# 总结

正如所见，数据科学专业知识与 ChatGPT 的协同作用可以帮助简化和加速数据清洗和预处理。现在你已经准备好预处理的数据集，可以通过在这个银行营销数据集上构建一个简单的预测模型来进一步处理。

如果感兴趣，你还可以探索如何利用 ChatGPT 进行数据探索。

# 数据集致谢

银行营销数据集根据 [知识共享署名 4.0 国际 (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/legalcode) 许可证进行许可。

Moro, S., Rita, P., 和 Cortez, P.. (2012). 银行营销. UCI 机器学习资料库. [`doi.org/10.24432/C5K306`](https://doi.org/10.24432/C5K306).

**[Bala Priya C](https://www.linkedin.com/in/bala-priya/)** 是来自印度的开发者和技术作家。她喜欢在数学、编程、数据科学和内容创作的交汇处工作。她的兴趣和专长领域包括 DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编程和咖啡！目前，她正在通过撰写教程、操作指南、观点文章等，学习并与开发者社区分享她的知识。

### 相关主题更多信息

+   [利用数据的力量的 3 个步骤](https://www.kdnuggets.com/2022/05/3-steps-harnessing-power-data.html)

+   [揭示 CTGAN 的潜力：利用生成性 AI 生成合成数据…](https://www.kdnuggets.com/2023/04/unveiling-potential-ctgan-harnessing-generative-ai-synthetic-data.html)

+   [通过这本免费电子书学习数据清理和预处理](https://www.kdnuggets.com/2023/08/learn-data-cleaning-preprocessing-data-science-free-ebook.html)

+   [掌握数据清理和预处理技术的 7 个步骤](https://www.kdnuggets.com/2023/08/7-steps-mastering-data-cleaning-preprocessing-techniques.html)

+   [在 Pandas 中清理和预处理文本数据以进行 NLP 任务](https://www.kdnuggets.com/cleaning-and-preprocessing-text-data-in-pandas-for-nlp-tasks)

+   [Python 数据预处理的简易指南](https://www.kdnuggets.com/2020/07/easy-guide-data-preprocessing-python.html)
