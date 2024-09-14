# 重新定义数据可视化：掌握 Pandas 中的时间基础重采样

> 原文：[https://www.kdnuggets.com/revamping-data-visualization-mastering-timebased-resampling-in-pandas](https://www.kdnuggets.com/revamping-data-visualization-mastering-timebased-resampling-in-pandas)

![重新定义数据可视化：掌握 Pandas 中的时间基础重采样](../Images/bd136345f15863c634d50578c1797adb.png)

图片来源于 [Freepik](https://www.freepik.com/free-photo/data-analysis-marketing-business-report-concept_17123810.htm#query=analytics&position=46&from_view=keyword&track=sph)

这篇综合文章将讨论使用 Pandas 库进行时间基础的数据可视化。正如你所知，时间序列数据是一座宝贵的洞察宝藏，通过熟练的数据重采样技巧，你可以将原始时间数据转化为视觉上引人注目的叙事。不论你是数据爱好者、科学家、分析师，还是对揭示时间基础数据中的隐藏故事充满好奇，这篇文章将为你提供重塑数据可视化技能的知识和工具。所以，让我们开始讨论 Pandas 重采样技巧，将数据转化为信息丰富且引人入胜的时间作品。 

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在的组织的 IT

* * *

# 为什么需要数据重采样？

在处理基于时间的数据可视化时，数据重采样至关重要且非常有用。它允许你控制数据的粒度，从而提取有意义的见解并创建视觉上引人注目的表示，以更好地理解数据。在下面的图片中，你可以观察到，你可以根据需求对时间序列数据进行上采样或下采样。

![重新定义数据可视化：掌握 Pandas 中的时间基础重采样](../Images/3daa2b2418638c6042fb44c9c1372f8c.png)

图片来源于 [SQLRelease](https://www.google.com/imgres?imgurl=https%3A%2F%2Fsqlrelease.com%2F%2Fwp-content%2Fuploads%2F2018%2F09%2FPython-use-case-Resampling-time-series-data-Upsampling-and-downsampling-SQL-Server-2017-Upsampling-and-downsampling.png&tbnid=mMi4gM1XYtqOxM&vet=12ahUKEwjWienfyr6BAxWKzaACHa8FCYsQMygNegQIARBv..i&imgrefurl=https%3A%2F%2Fsqlrelease.com%2Fpython-use-case-resampling-time-series-data-upsampling-and-downsampling-sql-server-2017&docid=Kr0kzdsRLm36DM&w=578&h=620&q=time%20based%20data%20resampling%20image&ved=2ahUKEwjWienfyr6BAxWKzaACHa8FCYsQMygNegQIARBv)

基本上，数据重采样的两个主要目的如下：

1.  **细粒度调整：** 收集大数据可以让你更改数据点的收集或汇总时间间隔。你可以只获取重要信息，而不是噪声。这可以帮助你去除噪声数据，使数据更易于进行可视化。

1.  **对齐：** 这还帮助对齐来自多个来源的数据，这些数据具有不同的时间间隔，确保在创建可视化或进行分析时的一致性。

**例如，**

假设你从股票交易所获取了某公司每日股价数据，并且你希望在分析中可视化长期趋势，而不包含噪声数据点。因此，你可以通过计算每个月的平均收盘价，将这些日数据重采样为月频率，结果是可视化的数据量减少，你的分析可以提供更好的洞察。

```py
import pandas as pd
# Sample daily stock price data
data = {
'Date': pd.date_range(start='2023-01-01', periods=365, freq='D'),
'StockPrice': [100 + i + 10 * (i % 7) for i in range(365)]
}
df = pd.DataFrame(data)
# Resample to monthly frequency
monthly_data = df.resample('M', on='Date').mean()
print(monthly_data.head())
```

在上述示例中，你可以观察到我们将日数据重采样为月间隔，并计算了每个月的平均收盘价，这使你获得了更平滑、噪声更少的股票价格数据表示，便于识别长期趋势和模式以作出决策。

# 选择正确的重采样频率

在处理时间序列数据时，重采样的主要参数是频率，你必须正确选择，以获得有洞察力和实用的可视化。基本上，数据的细粒度（即数据的详细程度）与数据的清晰度（即数据模式的显现程度）之间存在权衡。

**例如，**

想象一下你有一年的每分钟记录的温度数据。如果你需要可视化年度温度趋势，使用分钟级数据会导致图表过于密集和混乱。另一方面，如果将数据聚合到每年平均值，你可能会丧失有价值的信息。

```py
# Sample minute-level temperature data
data = {
    'Timestamp': pd.date_range(start='2023-01-01', periods=525600, freq='T'),
    'Temperature': [20 + 10 * (i % 1440) / 1440 for i in range(525600)]
}

df = pd.DataFrame(data)

# Resample to different frequencies
daily_avg = df.resample('D', on='Timestamp').mean()
monthly_avg = df.resample('M', on='Timestamp').mean()
yearly_avg = df.resample('Y', on='Timestamp').mean()

print(daily_avg.head())
print(monthly_avg.head())
print(yearly_avg.head())
```

在这个例子中，我们将分钟级温度数据重采样为每日、每月和每年的平均值。根据你的分析或可视化目标，你可以选择最符合你目的的详细级别。每日平均值揭示了每日温度模式，而每年平均值提供了年度趋势的高层次概览。

通过选择最佳的重采样频率，你可以平衡数据细节的数量与可视化的清晰度，确保你的观众能够轻松辨别你想传达的模式和洞察。

# 聚合方法和技术

在处理基于时间的数据时，了解各种聚合方法和技术至关重要。这些方法使你能够有效地总结和分析数据，揭示时间信息的不同方面。标准聚合方法包括计算总和和均值或应用自定义函数。

![重塑数据可视化：掌握 pandas 中基于时间的重采样](../Images/8ec94c373aad9032523a121587800eff.png)

图片来源于 [TowardsDataScience](https://towardsdatascience.com/time-series-analysis-resampling-shifting-and-rolling-f5664ddef77e)

**例如，**

假设你有一个包含零售店一年内每日销售数据的数据集。你想分析年度收入趋势。为此，你可以使用聚合方法计算每个月和每年的总销售额。

```py
# Sample daily sales data
data = {
'Date': pd.date_range(start='2023-01-01', periods=365, freq='D'),
'Sales': [1000 + i * 10 + 5 * (i % 30) for i in range(365)]
}
df = pd.DataFrame(data)

# Calculate monthly and yearly sales with the aggregation method
monthly_totals = df.resample('M', on='Date').sum()
yearly_totals = df.resample('Y', on='Date').sum()

print(monthly_totals.head())
print(yearly_totals.head())
```

在这个例子中，我们使用 sum() 聚合方法将每日销售数据重采样为每月和每年的总数。通过这样做，你可以在不同的粒度层次上分析销售趋势。每月总额提供了季节性变化的见解，而年度总额则提供了年度表现的高层次概览。

根据你的具体分析需求，你还可以使用其他聚合方法，如计算均值和中位数，或根据数据集分布应用自定义函数，这对于问题来说是有意义的。这些方法使你能够通过以符合你的分析或可视化目标的方式总结数据，从而从基于时间的数据中提取有价值的洞察。

# 处理缺失数据

处理缺失数据是处理时间序列时的一个关键方面，确保即使在处理数据中的空白时，你的可视化和分析仍然准确和有用。

**例如，**

想象一下你正在处理一个历史温度数据集，但由于设备故障或数据收集错误，一些天的温度读数缺失。你必须处理这些缺失值，以创建有意义的可视化并保持数据的完整性。

```py
# Sample temperature data with missing values
data = {
    'Date': pd.date_range(start='2023-01-01', periods=365, freq='D'),
    'Temperature': [25 + np.random.randn() * 5 if np.random.rand() > 0.2 else np.nan for _ in range(365)]
}
df = pd.DataFrame(data)

# Forward-fill missing values (fill with the previous day's temperature)
df['Temperature'].fillna(method='ffill', inplace=True)

# Visualize the temperature data
import matplotlib.pyplot as plt
plt.figure(figsize=(12, 6))
plt.plot(df['Date'], df['Temperature'], label='Temperature', color='blue')
plt.title('Daily Temperature Over Time')
plt.xlabel('Date')
plt.ylabel('Temperature (°C)')
plt.grid(True)
plt.show()
```

**输出：**

![重塑数据可视化：掌握 pandas 中基于时间的重采样](../Images/b37bfe3a294355427476caad189dae28.png)

图片由作者提供

在上面的例子中，你可以看到首先，我们模拟了缺失的温度值（约占数据的20%），然后使用前向填充（ffill）方法来填补这些空白，这意味着缺失的值被替换为前一天的温度。

因此，处理缺失数据可以确保你的可视化准确地代表时间序列中的潜在趋势和模式，防止空白扭曲你的洞察或误导你的受众。根据数据的性质和研究问题，可以采用各种策略，如插值或向后填充。

# 可视化趋势和模式

在 pandas 中的数据重采样使你能够可视化顺序或基于时间的数据中的趋势和模式，这进一步帮助你收集洞察并有效地向他人传达结果。因此，你可以找到清晰且信息丰富的数据可视化，突出显示不同的组成部分，包括趋势、季节性和不规则模式（可能是数据中的噪声）。

**例如，**

假设你有一个包含过去几年每天网站流量数据的数据集。你的目标是可视化未来几年的总体流量趋势，识别任何季节性模式，并发现流量中的不规则峰值或下降。

```py
# Sample daily website traffic data
data = {
'Date': pd.date_range(start='2019-01-01', periods=1095, freq='D'),
'Visitors': [500 + 10 * ((i % 365) - 180) + 50 * (i % 30) for i in range(1095)]
}
df = pd.DataFrame(data)

# Create a line plot to visualize the trend
plt.figure(figsize=(12, 6))
plt.plot(df['Date'], df['Visitors'], label='Daily Visitors', color='blue')
plt.title('Website Traffic Over Time')
plt.xlabel('Date')
plt.ylabel('Visitors')
plt.grid(True)

# Add seasonal decomposition plot
from statsmodels.tsa.seasonal import seasonal_decompose
result = seasonal_decompose(df['Visitors'], model='additive', freq=365)
result.plot()
plt.show()
```

**输出：**

![重新定义数据可视化：掌握 Pandas 中的基于时间的重采样](../Images/f8b4387de8e58f87b2d640b22c633fb4.png)

图片由作者提供

在上面的示例中，我们首先创建了一张折线图来可视化网站流量随时间的趋势。这张图描述了数据集的总体增长以及任何不规则的模式。此外，为了将数据分解为不同的组成部分，我们使用了 statsmodels 库中的季节性分解技术，包括趋势、季节性和残差组成部分。

通过这种方式，你可以有效地向利益相关者传达网站的流量趋势、季节性和异常情况，这增强了你从基于时间的数据中提取重要洞察并将其转化为数据驱动决策的能力。

# 总结

**Colab Notebook 链接：** [https://colab.research.google.com/drive/19oM7NMdzRgQrEDfRsGhMavSvcHx79VDK#scrollTo=nHg3oSjPfS-Y](https://colab.research.google.com/drive/19oM7NMdzRgQrEDfRsGhMavSvcHx79VDK#scrollTo=nHg3oSjPfS-Y)

在这篇文章中，我们讨论了 Python 中的数据的基于时间的重采样。因此，为了总结我们的讨论，让我们回顾一下文章中涵盖的重要点：

1.  基于时间的重采样是一种强大的技术，用于转换和总结时间序列数据，以便获得更好的决策洞察。

1.  精确选择重采样频率对平衡数据可视化中的粒度和清晰度至关重要。

1.  聚合方法如求和、均值和自定义函数有助于揭示基于时间的数据的不同方面。

1.  有效的可视化技术有助于识别趋势、季节性和不规则模式，促进发现的清晰沟通。

1.  现实世界中的金融、天气预报和社交媒体分析用例展示了基于时间的重采样的广泛影响。

**[Aryan Garg](https://www.linkedin.com/in/aryan-garg-1bbb791a3/)** 是一名 B.Tech. 电气工程专业的学生，目前正处于本科的最后一年。他对 Web 开发和机器学习领域充满兴趣。他已经追求了这个兴趣，并渴望在这些方向上继续工作。

**[](https://www.linkedin.com/in/aryan-garg-1bbb791a3/)**[Aryan Garg](https://www.linkedin.com/in/aryan-garg-1bbb791a3/)**** 是一名 B.Tech. 电气工程专业的学生，目前正处于本科的最后一年。他对 Web 开发和机器学习领域充满兴趣。他已经追求了这个兴趣，并渴望在这些方向上继续工作。

### 更多相关主题

+   [数据科学中重采样技术的作用](https://www.kdnuggets.com/2023/02/role-resampling-techniques-data-science.html)

+   [掌握数据可视化的 30 个资源](https://www.kdnuggets.com/2022/11/30-resources-mastering-data-visualization.html)

+   [掌握 Pandas 和 Python 数据处理的 7 个步骤](https://www.kdnuggets.com/7-steps-to-mastering-data-wrangling-with-pandas-and-python)

+   [掌握 Python 和 Pandas 数据清洗的 7 个步骤](https://www.kdnuggets.com/7-steps-to-mastering-data-cleaning-with-python-and-pandas)

+   [7 个 Pandas 绘图函数用于快速数据可视化](https://www.kdnuggets.com/7-pandas-plotting-functions-for-quick-data-visualization)

+   [如何在 Pandas 中使用条件格式化来增强数据可视化](https://www.kdnuggets.com/how-to-use-conditional-formatting-in-pandas-to-enhance-data-visualization)
