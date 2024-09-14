# Python中的客户细分：实用方法

> 原文：[https://www.kdnuggets.com/customer-segmentation-in-python-a-practical-approach](https://www.kdnuggets.com/customer-segmentation-in-python-a-practical-approach)

![Python中的客户细分：实用方法](../Images/030e6da7950544c232cbe08351c32b44.png)

作者提供的图片 | 使用Excalidraw和Flaticon创建

客户细分可以帮助企业量身定制营销工作并提高客户满意度。以下是方法。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的IT工作

* * *

从功能上讲，客户细分涉及将客户基础划分为不同的*组*或*细分*——基于共享的特征和行为。通过理解每个细分的需求和偏好，企业可以提供更个性化和有效的营销活动，从而提高客户保留率和收入。

在本教程中，我们将通过结合两种基本技术：**RFM（最近性、频率、货币）分析**和**K均值聚类**，来探索Python中的客户细分。RFM分析提供了评估客户行为的结构化框架，而K均值聚类则提供了一种数据驱动的方法，将客户分组到有意义的细分中。我们将使用来自零售行业的真实数据集：[在线零售数据集](https://archive.ics.uci.edu/dataset/352/online+retail)来自UCI机器学习库。

从数据预处理到聚类分析和可视化，我们将逐步编写代码。让我们开始吧！

# 我们的方法：RFM分析和K均值聚类

我们的目标是：通过将RFM分析和K均值聚类应用于这个数据集，我们希望获得关于客户行为和偏好的洞察。

RFM分析是一种简单但强大的方法，用于量化客户行为。它基于三个关键维度来评估客户：

+   **最近性 (R)**：特定客户最近一次购买的时间是什么时候？

+   **频率 (F)**：他们多频繁购买？

+   **货币价值 (M)**：他们花了多少钱？

我们将利用数据集中的信息计算最近性、频率和货币价值。然后，我们将这些值映射到通常使用的RFM评分尺度1 - 5。

如果你愿意，可以使用这些 RFM 分数进行进一步的探索和分析。但我们将尝试识别具有相似 RFM 特征的客户细分。为此，我们将使用 K-Means 聚类，这是一种无监督的机器学习算法，将相似的数据点分组到簇中。

那么，让我们开始编码吧！

🔗 [链接到 Google Colab notebook](https://github.com/balapriyac/python-data-analysis/blob/main/customer-segmentation/Customer_Segmentation_in_Python.ipynb)。

## 步骤 1 – 导入必要的库和模块

首先，让我们导入必要的库和特定的模块：

```py
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.cluster import KMeans
```

我们需要 pandas 和 matplotlib 来进行数据探索和可视化，并且需要来自 scikit-learn 的 `KMeans` 类来执行 K-Means 聚类。

## 步骤 2 – 加载数据集

如前所述，我们将使用在线零售数据集。该数据集包含客户记录：包括购买日期、数量、价格和客户ID的交易信息。

让我们从其 URL 中读取原本在 Excel 文件中的数据到 pandas 数据框中。

```py
# Load the dataset from UCI repository
url = "https://archive.ics.uci.edu/ml/machine-learning-databases/00352/Online%20Retail.xlsx"
data = pd.read_excel(url)
```

另外，你可以 [下载数据集](https://archive.ics.uci.edu/static/public/352/online+retail.zip) 并将 Excel 文件读取到 pandas 数据框中。

## 步骤 3 – 探索和清理数据集

现在让我们开始探索数据集。查看数据集的前几行：

```py
data.head()
```

![Python中的客户细分：实用方法](../Images/1b012739c46d0bb3bbea881b94a68792.png)

data.head()的输出

现在在数据框上调用 `describe()` 方法，以更好地了解数值特征：

```py
data.describe()
```

我们看到“CustomerID”列目前是浮点数值。当我们清理数据时，我们会将其转换为整数：

![Python中的客户细分：实用方法](../Images/066ce9e79f212f2a620fbd8fab6eca9e.png)

data.describe()的输出

还要注意数据集相当嘈杂。“Quantity”和“UnitPrice”列包含负值：

![Python中的客户细分：实用方法](../Images/2aa4d515986463219abe340683da9984.png)

data.describe()的输出

让我们仔细查看这些列及其数据类型：

```py
data.info()
```

我们看到数据集有超过541K条记录，其中“Description”和“CustomerID”列包含缺失值：

![Python中的客户细分：实用方法](../Images/88f36c7be0119b5c6475eff9bdb604c3.png)

让我们统计每一列中的缺失值数量：

```py
# Check for missing values in each column
missing_values = data.isnull().sum()
print(missing_values)
```

正如预期的那样，“CustomerID”和“Description”列包含缺失值：

![Python中的客户细分：实用方法](../Images/019048ec21eefa3ceb37371015a29fc0.png)

对于我们的分析，我们不需要“Description”列中的产品描述。然而，我们需要“CustomerID”以进行分析的下一步。所以让我们丢弃缺失“CustomerID”的记录：

```py
# Drop rows with missing CustomerID
data.dropna(subset=['CustomerID'], inplace=True)
```

还要记住，“数量”和“单价”列的值应该严格为非负值。但它们包含负值。所以我们还会删除“数量”和“单价”中负值的记录：

```py
# Remove rows with negative Quantity and Price
data = data[(data['Quantity'] > 0) & (data['UnitPrice'] > 0)]
```

我们还将“CustomerID”转换为整数：

```py
data['CustomerID'] = data['CustomerID'].astype(int)

# Verify the data type conversion
print(data.dtypes)
```

![客户细分在 Python 中：实际方法](../Images/7b849734cdca050b7c6349d8b347ec8f.png)

## 第四步 – 计算最近一次购买、购买频率和货币价值

让我们首先定义一个参考日期 `snapshot_date`，该日期比“发票日期”列中的最新日期晚一天：

```py
snapshot_date = max(data['InvoiceDate']) + pd.DateOffset(days=1)
```

接下来，创建一个“总计”列，其中包含所有记录的数量*单价：

```py
data['Total'] = data['Quantity'] * data['UnitPrice']
```

计算“最近一次购买”、“购买频率”和“货币价值”时，我们将按**按 CustomerID 分组**计算如下：

+   对于**最近一次购买**，我们将计算最近购买日期和参考日期（`snapshot_date`）之间的差异。这给出**客户最后一次购买的天数**。所以*较小的值*表示客户*最近*进行了购买。但是当我们谈论*最近分数*时，我们希望最近购买的客户具有更高的最近分数，对吗？我们将在下一步处理中解决这个问题。

+   因为**购买频率**衡量客户购买的频率，所以我们将其计算为每个客户的**唯一发票总数**或交易次数。

+   **货币价值**量化了客户花费了多少钱。因此，我们将计算所有交易的货币总值的平均值。

```py
rfm = data.groupby('CustomerID').agg({
    'InvoiceDate': lambda x: (snapshot_date - x.max()).days,
    'InvoiceNo': 'nunique',
    'Total': 'sum'
})
```

让我们重新命名这些列以提高可读性：

```py
rfm.rename(columns={'InvoiceDate': 'Recency', 'InvoiceNo': 'Frequency', 'Total': 'MonetaryValue'}, inplace=True)
rfm.head()
```

![客户细分在 Python 中：实际方法](../Images/c5671b7ae07d5464322b95df07ad4a79.png)

## 第五步 – 将 RFM 值映射到 1-5 规模

现在让我们将“最近一次购买”、“购买频率”和“货币价值”列映射到 1-5 的值范围内；即 {1,2,3,4,5}。

我们将基本上把值分配到五个不同的区间，并将每个区间映射到一个值。为了帮助我们确定区间边界，让我们使用“最近一次购买”、“购买频率”和“货币价值”列的分位数值：

```py
rfm.describe()
```

![客户细分在 Python 中：实际方法](../Images/dd35049e987fde3395ccd9fc46b9b14d.png)

以下是我们定义自定义区间边界的方法：

```py
# Calculate custom bin edges for Recency, Frequency, and Monetary scores
recency_bins = [rfm['Recency'].min()-1, 20, 50, 150, 250, rfm['Recency'].max()]
frequency_bins = [rfm['Frequency'].min() - 1, 2, 3, 10, 100, rfm['Frequency'].max()]
monetary_bins = [rfm['MonetaryValue'].min() - 3, 300, 600, 2000, 5000, rfm['MonetaryValue'].max()]
```

现在我们已经定义了区间边界，让我们将分数映射到 1 到 5（包括 1 和 5）之间的相应标签：

```py
# Calculate Recency score based on custom bins 
rfm['R_Score'] = pd.cut(rfm['Recency'], bins=recency_bins, labels=range(1, 6), include_lowest=True)

# Reverse the Recency scores so that higher values indicate more recent purchases
rfm['R_Score'] = 5 - rfm['R_Score'].astype(int) + 1

# Calculate Frequency and Monetary scores based on custom bins
rfm['F_Score'] = pd.cut(rfm['Frequency'], bins=frequency_bins, labels=range(1, 6), include_lowest=True).astype(int)
rfm['M_Score'] = pd.cut(rfm['MonetaryValue'], bins=monetary_bins, labels=range(1, 6), include_lowest=True).astype(int)
```

注意，基于区间的 R_Score 是 1（最近的购买）到 5（所有超过 250 天的购买）。但我们希望最近的购买具有 R_Score 为 5，而超过 250 天的购买具有 R_Score 为 1。

为了实现所需的映射，我们做：`5 - rfm['R_Score'].astype(int) + 1`。

让我们查看 R_Score、F_Score 和 M_Score 列的前几行：

```py
# Print the first few rows of the RFM DataFrame to verify the scores
print(rfm[['R_Score', 'F_Score', 'M_Score']].head(10))
```

![客户细分在 Python 中：实际方法](../Images/2406fb57788000d4e6ee2e0f654d436b.png)

如果你愿意，可以使用这些 R、F 和 M 分数进行深入分析。或者使用聚类来识别具有相似 RFM 特征的群体。我们将选择后者！

## 第6步 – 执行K均值聚类

K均值聚类对特征的尺度敏感。由于R、F和M值都在相同的尺度上，我们可以继续进行聚类，而无需进一步缩放特征。

让我们提取R、F和M得分来执行K均值聚类：

```py
# Extract RFM scores for K-means clustering
X = rfm[['R_Score', 'F_Score', 'M_Score']]
```

接下来，我们需要找到*最佳*的集群数量。为此，让我们对一系列K值运行K均值算法，并使用*肘部法*来选择最佳K：

```py
# Calculate inertia (sum of squared distances) for different values of k
inertia = []
for k in range(2, 11):
    kmeans = KMeans(n_clusters=k, n_init= 10, random_state=42)
    kmeans.fit(X)
    inertia.append(kmeans.inertia_)

# Plot the elbow curve
plt.figure(figsize=(8, 6),dpi=150)
plt.plot(range(2, 11), inertia, marker='o')
plt.xlabel('Number of Clusters (k)')
plt.ylabel('Inertia')
plt.title('Elbow Curve for K-means Clustering')
plt.grid(True)
plt.show()
```

我们看到曲线在4个集群处出现拐点。因此，让我们将客户基础划分为四个细分市场。

![Python中的客户细分：实用方法](../Images/445ef9cef78341fb88e185bec085b99d.png)

我们将K固定为4。因此，让我们运行K均值算法以获取数据集中所有点的集群分配：

```py
# Perform K-means clustering with best K
best_kmeans = KMeans(n_clusters=4, n_init=10, random_state=42)
rfm['Cluster'] = best_kmeans.fit_predict(X)
```

## 第7步 – 解释集群以识别客户细分

现在我们有了集群，接下来我们尝试根据RFM得分对它们进行描述。

```py
# Group by cluster and calculate mean values
cluster_summary = rfm.groupby('Cluster').agg({
    'R_Score': 'mean',
    'F_Score': 'mean',
    'M_Score': 'mean'
}).reset_index()
```

每个集群的平均R、F和M得分应该已经给你一个关于特征的概念。

```py
print(cluster_summary)
```

![Python中的客户细分：实用方法](../Images/e3193fe4aa77709beb2a8180fde47753.png)

但让我们可视化集群的平均R、F和M得分，以便更容易解释：

```py
colors = ['#3498db', '#2ecc71', '#f39c12','#C9B1BD']

# Plot the average RFM scores for each cluster
plt.figure(figsize=(10, 8),dpi=150)

# Plot Avg Recency
plt.subplot(3, 1, 1)
bars = plt.bar(cluster_summary.index, cluster_summary['R_Score'], color=colors)
plt.xlabel('Cluster')
plt.ylabel('Avg Recency')
plt.title('Average Recency for Each Cluster')

plt.grid(True, linestyle='--', alpha=0.5)
plt.legend(bars, cluster_summary.index, title='Clusters')

# Plot Avg Frequency
plt.subplot(3, 1, 2)
bars = plt.bar(cluster_summary.index, cluster_summary['F_Score'], color=colors)
plt.xlabel('Cluster')
plt.ylabel('Avg Frequency')
plt.title('Average Frequency for Each Cluster')
plt.grid(True, linestyle='--', alpha=0.5)
plt.legend(bars, cluster_summary.index, title='Clusters')

# Plot Avg Monetary
plt.subplot(3, 1, 3)
bars = plt.bar(cluster_summary.index, cluster_summary['M_Score'], color=colors)
plt.xlabel('Cluster')
plt.ylabel('Avg Monetary')
plt.title('Average Monetary Value for Each Cluster')
plt.grid(True, linestyle='--', alpha=0.5)
plt.legend(bars, cluster_summary.index, title='Clusters')

plt.tight_layout()
plt.show()
```

![Python中的客户细分：实用方法](../Images/8a77b0dbf09aaee9eb8455243a06fe3c.png)

注意每个细分市场中的客户如何根据最近性、频率和货币价值来描述：

+   **集群 0**：在所有四个集群中，这个集群具有*最高*的最近性、频率和货币价值。我们称这个集群中的客户为**冠军（或高消费客户）**。

+   **集群 1**：该集群的特征是*适中的*最近性、频率和货币价值。这些客户的支出和购买频率仍然高于集群2和集群3。我们称他们为**忠实客户**。

+   **集群 2**：这个集群中的客户倾向于花费较少。他们不经常购买，也没有最近进行过购买。这些客户可能是*非活跃*或**高风险客户**。

+   **集群 3**：该集群的特征是*高最近性*和相对较低的频率以及适中的货币价值。因此，这些是**近期客户**，他们可能会成为长期客户。

这里有一些如何定制营销工作的例子——以针对每个细分市场的客户——来增强客户参与度和留存率：

+   **对冠军/高消费客户**：提供个性化的特别折扣、优先访问和其他高级福利，以使他们感到被重视和欣赏。

+   **对忠实客户**：欣赏活动、推荐奖金和忠诚奖励。

+   **对高风险客户**：重新参与的努力，包括开展折扣或促销活动以鼓励购买。

+   **对近期客户**：针对性的活动，教育他们了解品牌，并对后续购买提供折扣。

理解不同细分市场中客户的百分比也是很有帮助的。这将进一步帮助简化营销工作并推动业务增长。

让我们使用饼图来可视化不同集群的分布：

```py
cluster_counts = rfm['Cluster'].value_counts()

colors = ['#3498db', '#2ecc71', '#f39c12','#C9B1BD']
# Calculate the total number of customers
total_customers = cluster_counts.sum()

# Calculate the percentage of customers in each cluster
percentage_customers = (cluster_counts / total_customers) * 100

labels = ['Champions(Power Shoppers)','Loyal Customers','At-risk Customers','Recent Customers']

# Create a pie chart
plt.figure(figsize=(8, 8),dpi=200)
plt.pie(percentage_customers, labels=labels, autopct='%1.1f%%', startangle=90, colors=colors)
plt.title('Percentage of Customers in Each Cluster')
plt.legend(cluster_summary['Cluster'], title='Cluster', loc='upper left')

plt.show()
```

![Python中的客户细分：实用方法](../Images/32d5956b8e13aa7df201cf8802848137.png)

开始吧！在这个示例中，我们在各个细分市场中有相当均匀的客户分布。因此，我们可以投入时间和精力来留住现有客户、重新吸引高风险客户和教育新客户。

# 总结

就这样！我们从*154K个客户记录*通过7个简单步骤分成了4个集群。我希望你能理解客户细分如何通过以下方式帮助你做出数据驱动的决策，从而影响业务增长和客户满意度：

+   **个性化**：细分允许企业根据每个客户群体的特定需求和兴趣来量身定制营销信息、产品推荐和促销活动。

+   **改进目标定位**：通过识别*高价值*和*高风险*客户，企业可以更有效地分配资源，将精力集中在最可能产生结果的地方。

+   **客户保留**：细分帮助企业通过了解什么因素让客户保持参与和满意，从而制定保留策略。

作为下一步，尝试将这种方法应用到另一个数据集上，记录你的过程，并与社区分享！但请记住，有效的客户细分和有针对性的活动需要对你的客户群有良好的理解——以及客户群的演变。因此，这需要定期分析以随着时间的推移优化你的策略。

# 数据集致谢

[在线零售数据集](https://archive.ics.uci.edu/dataset/352/online+retail)在[创意共享署名4.0国际](https://creativecommons.org/licenses/by/4.0/legalcode)（CC BY 4.0）许可证下许可：

在线零售。 (2015)。 UCI机器学习库。 https://doi.org/10.24432/C5BW33。

**[](https://twitter.com/balawc27)**[Bala Priya C](https://www.kdnuggets.com/wp-content/uploads/bala-priya-author-image-update-230821.jpg)**** 是来自印度的开发人员和技术作家。她喜欢在数学、编程、数据科学和内容创作的交叉点上工作。她的兴趣和专长领域包括DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编程和喝咖啡！目前，她正在通过编写教程、操作指南、观点文章等方式学习并与开发者社区分享她的知识。Bala还创建了引人入胜的资源概述和编码教程。

### 更多相关话题

+   [机器学习中的特征工程实用方法](https://www.kdnuggets.com/2023/07/practical-approach-feature-engineering-machine-learning.html)

+   [免费电子书：10个实用的Python编程技巧](https://www.kdnuggets.com/2023/04/free-ebook-10-practical-python-programming-tricks.html)

+   [如何成为数据科学家的指南（逐步方法）](https://www.kdnuggets.com/2021/05/guide-become-data-scientist.html)

+   [支持向量机：直观的方法](https://www.kdnuggets.com/2022/08/support-vector-machines-intuitive-approach.html)

+   [数据科学项目：烂番茄电影评分预测：…](https://www.kdnuggets.com/2023/06/data-science-project-rotten-tomatoes-movie-rating-prediction-first-approach.html)

+   [数据科学项目：烂番茄电影评分预测：…](https://www.kdnuggets.com/2023/07/data-science-project-rotten-tomatoes-movie-rating-prediction-second-approach.html)
