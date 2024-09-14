# ChatGPT驱动的数据探索：解锁数据集中隐藏的见解

> 原文：[https://www.kdnuggets.com/2023/07/chatgptpowered-data-exploration-unlock-hidden-insights-dataset.html](https://www.kdnuggets.com/2023/07/chatgptpowered-data-exploration-unlock-hidden-insights-dataset.html)

![ChatGPT驱动的数据探索：解锁数据集中隐藏的见解](../Images/7f8d82444add9727d683bd276defd59f.png)

作者提供的图片

分析数据集以更好地理解它并获得见解是任何数据科学项目中的重要步骤。而ChatGPT可以帮助简化和加速这样的分析。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT

* * *

在这里，我们将使用ChatGPT对一个样本数据集进行探索性数据分析。我们将了解特征的分布、感兴趣的特征如何与其他特征变化等等。

要跟随并访问ChatGPT，你需要一个OpenAI账户。如果你还没有，可以[注册一个免费账户](https://platform.openai.com/signup)。

# 加载数据集并获取基本信息

[seaborn库](https://seaborn.pydata.org/)提供了一些可以直接使用的数据集，我们可以对其进行分析。我们将使用**tips**数据集。

**提示**：告诉我更多关于`tips`数据集的信息，它是内置在seaborn库中的。

![ChatGPT驱动的数据探索：解锁数据集中隐藏的见解](../Images/6f6595538cb4d768ebfc9c66c2335159.png)

了解数据集的内容是一个很好的起点。让我们开始做以下事情：

+   将数据集加载到pandas数据框中，然后

+   获取列的基本信息和汇总统计数据，以便更好地了解数据集。

**提示**：让我们使用`tips`数据集。编写代码以将数据集读入pandas数据框，并获取可用字段的基本信息和汇总统计数据。

![ChatGPT驱动的数据探索：解锁数据集中隐藏的见解](../Images/67c746078e17f713c06640243c927704.png)

```py
import seaborn as sns
import pandas as pd

# Load the 'tips' dataset from Seaborn
tips_data = sns.load_dataset('tips')

# Create a Pandas DataFrame from the loaded dataset
tips_df = pd.DataFrame(tips_data)

# Display the first few rows of the DataFrame
print("First few rows of the 'tips' dataset:")
print(tips_df.head())

# Get basic information about the fields
print("\nInformation about the 'tips' dataset:")
print(tips_df.info())

# Get summary statistics of the numeric fields
print("\nSummary statistics of the numeric fields:")
print(tips_df.describe())
```

我们看到我们对数据集有了相当有用的总结：

```py
First few rows of the 'tips' dataset:
   total_bill   tip     sex smoker  day    time  size
0       16.99  1.01  Female     No  Sun  Dinner     2
1       10.34  1.66    Male     No  Sun  Dinner     3
2       21.01  3.50    Male     No  Sun  Dinner     3
3       23.68  3.31    Male     No  Sun  Dinner     2
4       24.59  3.61  Female     No  Sun  Dinner     4

Information about the 'tips' dataset:
 <class>RangeIndex: 244 entries, 0 to 243
Data columns (total 7 columns):
 #   Column      Non-Null Count  Dtype   
---  ------      --------------  -----   
 0   total_bill  244 non-null    float64 
 1   tip         244 non-null    float64 
 2   sex         244 non-null    category
 3   smoker      244 non-null    category
 4   day         244 non-null    category
 5   time        244 non-null    category
 6   size        244 non-null    int64   
dtypes: category(4), float64(2), int64(1)
memory usage: 7.4 KB
None

Summary statistics of the numeric fields:
       total_bill         tip        size
count  244.000000  244.000000  244.000000
mean    19.785943    2.998279    2.569672
std      8.902412    1.383638    0.951100
min      3.070000    1.000000    1.000000
25%     13.347500    2.000000    2.000000
50%     17.795000    2.900000    2.000000
75%     24.127500    3.562500    3.000000
max     50.810000   10.000000    6.000000</class>
```

从汇总统计数据中，我们对数据集中的数值特征有了了解。我们知道数值特征的最小值和最大值、均值和中位数值，以及百分位数值。没有缺失值，所以我们可以继续进行下一步。

# 探索数据集——什么、为什么以及如何

现在我们对数据集有了了解，让我们进一步深入探讨。

这次探索性数据分析的目标是更好地理解小费行为。为此，我们可以提出有用的可视化。这些可视化应帮助我们理解小费金额与数据集中各种类别变量的关系。

由于这是一个简单的数据集，我们可以提示 ChatGPT 给出一系列步骤来进一步分析该数据集。

**提示**: 分析的目标是通过一些简单的可视化来获取对小费行为的见解。请建议相应的步骤。

![ChatGPT驱动的数据探索：揭示数据集中隐藏的见解](../Images/4445fcbd7ad13604d8b7d5c328d407f5.png)

ChatGPT 建议的数据探索步骤都似乎有效。因此，我们将使用这些步骤——逐步更好地理解数据集。我们会提示 ChatGPT 生成代码，尝试运行生成的代码，并根据需要进行修改。

# 探索小费金额的分布

作为第一步，让我们可视化小费金额的分布提示。

**提示**: 编写代码绘制小费金额的分布。

![ChatGPT驱动的数据探索：揭示数据集中隐藏的见解](../Images/77d79fb060fcc05c1ce1c0cda7dde4d5.png)

ChatGPT 给出了以下代码，用于生成直方图和核密度图，这有助于我们了解小费金额的分布：

```py
import matplotlib.pyplot as plt

# Create a histogram of tip amounts
plt.figure(figsize=(8, 6))
sns.histplot(data=tips_data, x='tip', kde=True)
plt.title("Distribution of Tip Amounts")
plt.xlabel("Tip Amount")
plt.ylabel("Frequency")
plt.show()
```

![ChatGPT驱动的数据探索：揭示数据集中隐藏的见解](../Images/600614bd11ab673d8a4abe2db5183088.png)

# 基于类别变量分析小费行为

数据集中有很多类别变量。显示不同类别变量的平均小费金额的简单条形图会很有帮助。

**提示**: 使用所有可用的类别变量（‘性别’，‘吸烟者’，‘日期’，和‘时间’）。编写代码生成条形图以理解小费行为。使用子图，以便我们可以在一个图形中显示所有四个条形图。

![ChatGPT驱动的数据探索：揭示数据集中隐藏的见解](../Images/6c53ee66b5dd77ca9a5a62cbab1db477.png)

这是代码。代码运行没有错误；我只需要修改图形的大小：

```py
# Define the categorical variables to analyze
categorical_vars = ['sex', 'smoker', 'day', 'time']

# Create subplots for each categorical variable
fig, axes = plt.subplots(nrows=2, ncols=2, figsize=(8, 5))
fig.suptitle("Tipping Behavior based on Categorical Variables", fontsize=16)

# Generate bar plots for each categorical variable
for ax, var in zip(axes.flatten(), categorical_vars):
    sns.barplot(data=tips_data, x=var, y='tip', ax=ax)
    ax.set_xlabel(var.capitalize())
    ax.set_ylabel("Average Tip Amount")

plt.tight_layout()
plt.show()
```

![ChatGPT驱动的数据探索：揭示数据集中隐藏的见解](../Images/f704141ae6550e8baf37399b06531c53.png)

从图表中我们可以看到，像性别和吸烟行为这样的特征对小费行为没有影响（这也是预期中的）。而日期和时间似乎会有影响。周末和晚餐时的小费金额似乎略高一些。

# 可视化总账单和小费金额之间的关系

现在，让我们看看总账单如何影响支付的小费金额。

**提示**: 我想了解总账单和小费金额之间的关系。请给我生成一个适合的图表的代码。我相信一个简单的散点图会很有帮助。

![ChatGPT驱动的数据探索：揭示数据集中隐藏的见解](../Images/9c15acf18a95c526f731978c18745261.png)

这是生成所需散点图的代码：

```py
# Create a scatter plot of total bill vs. tip amount
plt.figure(figsize=(6, 4))
sns.scatterplot(data=tips_data, x='total_bill', y='tip')
plt.title("Total Bill vs. Tip Amount")
plt.xlabel("Total Bill")
plt.ylabel("Tip Amount")
plt.show()
```

![ChatGPT驱动的数据探索：揭示数据集中的隐藏洞察](../Images/eeae008b50b20bb2443a0b21bb717038.png)

我们看到总账单和小费金额之间存在一些正相关关系。

# 可视化派对规模与小费金额之间的关系

下一步，让我们尝试可视化派对规模与小费金额之间的关系。

**提示**：现在，我想了解小费金额如何随着用餐人数（派对规模）的变化而变化。条形图和提琴图哪个更好？

![ChatGPT驱动的数据探索：揭示数据集中的隐藏洞察](../Images/8b02f2f42696ef81858759a5b7aa0ea3.png)

由于我们还希望了解小费金额的分布情况，因此我们继续创建一个提琴图。

**提示**：很好！请写出生成此可视化的提琴图的代码。

以下是代码：

```py
# Create a violin plot for tip amount by party size
plt.figure(figsize=(6, 4))
sns.violinplot(data=tips_data, x='size', y='tip')
plt.title("Tip Amount by Party Size")
plt.xlabel("Party Size")
plt.ylabel("Tip Amount")
plt.show()
```

![ChatGPT驱动的数据探索：揭示数据集中的隐藏洞察](../Images/4b0b97a84ef18b697b0a7aa06283c2d4.png)

# 基于时间和日期理解小费行为

接下来，让我们了解时间*和*日期如何影响小费行为。

**提示**：我想了解小费行为如何根据时间和日期变化。什么是好的可视化方式？此外，写出生成所需可视化的代码。

![ChatGPT驱动的数据探索：揭示数据集中的隐藏洞察](../Images/3a385057cd18fde8cb4fe6cdcef3fb28.png)

这段代码展示了如何使用日期作为索引进行透视，并汇总午餐和晚餐（一天中的时间）的小费金额的平均值：

```py
# Create a pivot table of average tip amount by time and day
pivot_table = tips_data.pivot_table(values='tip', index='day', columns='time', aggfunc='mean')

# Create a heatmap of tipping behavior based on time and day
plt.figure(figsize=(8, 6))
sns.heatmap(pivot_table, cmap='YlGnBu', annot=True, fmt=".2f", cbar=True)
plt.title("Tipping Behavior based on Time and Day")
plt.xlabel("Time")
plt.ylabel("Day")
plt.show()
```

![ChatGPT驱动的数据探索：揭示数据集中的隐藏洞察](../Images/5b7ad9f9cc3368aaa6387ec21418e591.png)

从热图中，我们看到周日晚餐的平均小费金额略高于其他时间-日期组合。一般来说，对于某一天，平均而言，晚餐的小费金额似乎高于午餐。

我们还发现周六和周日的午餐数据不足。由于我们总共有244条记录，因此可能没有周末午餐的记录。让我们继续验证一下。

**提示**：我想获取在周末点午餐的派对数量。写出代码以过滤`tips_data`数据框并获取该计数。

![ChatGPT驱动的数据探索：揭示数据集中的隐藏洞察](../Images/6320d61e37ae279070d9e73daf64e013.png)

这是过滤数据框并获得包含周末午餐记录的子集的代码：

```py
# Filter the data for lunch on weekends
weekend_lunch_data = tips_data[(tips_data['time'] == 'Lunch') & (tips_data['day'].isin(['Sat', 'Sun']))]

# Get the count of parties who ordered lunch on weekends
count = weekend_lunch_data['size'].count()
print("Number of parties who ordered lunch on weekends:", count)
```

我们看到没有相同的记录，所以到目前为止我们的分析是正确的：

```py
Number of parties who ordered lunch on weekends: 0
```

结束啦！我们探索了`tips`数据集，并通过提示ChatGPT生成了一些有用的可视化图表。

# 总结

在这篇文章中，我们学习了如何利用ChatGPT进行数据探索。如果你有兴趣将ChatGPT集成到你的数据科学工作流程中，可以查看[这份指南](/2023/05/integrating-chatgpt-data-science-workflows-tips-best-practices.html)。它通过一个示例项目，介绍了有效使用ChatGPT进行数据科学实验的技巧和最佳实践。

**[Bala Priya C](https://www.linkedin.com/in/bala-priya/)** 是来自印度的开发者和技术作家。她喜欢在数学、编程、数据科学和内容创作的交汇处工作。她的兴趣和专长领域包括DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编程和喝咖啡！目前，她正致力于通过撰写教程、操作指南、观点文章等，学习和与开发者社区分享她的知识。

### 更多相关主题

+   [BigQuery中的异常检测：发现隐藏的洞察并推动行动](https://www.kdnuggets.com/anomaly-detection-in-bigquery-uncover-hidden-insights-and-drive-action)

+   [解锁下一步：在热门数据技能提升课程中节省高达67%的费用](https://www.kdnuggets.com/2023/03/datacamp-unlock-next-move-save-67-indemand-data-upskilling.html)

+   [通过这个免费的DevOps速成课程发掘你的潜力](https://www.kdnuggets.com/2023/03/corise-unlock-potential-with-this-free-devops-crash-course.html)

+   [揭开选择完美机器学习算法的秘密！](https://www.kdnuggets.com/2023/07/ml-algorithm-choose.html)

+   [在60分钟内与Andrej Karpathy一起揭开LLM的秘密](https://www.kdnuggets.com/unlock-the-secrets-of-llms-in-a-60-minute-with-andrej-karpathy)

+   [释放AI的力量 - KDnuggets与Machine…的特别发布](https://www.kdnuggets.com/2023/07/mlm-unlock-power-ai-special-release-kdnuggets-machine-learning-mastery.html)
