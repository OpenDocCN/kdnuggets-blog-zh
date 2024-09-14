# 用 Python 可视化 COVID-19 新病例的时间变化

> 原文：[`www.kdnuggets.com/2020/09/visualization-covid-19-new-cases-over-time-python.html`](https://www.kdnuggets.com/2020/09/visualization-covid-19-new-cases-over-time-python.html)

评论

**作者：[Jason Bowling](https://www.linkedin.com/in/jasonbowlingoh/)，阿克伦大学网络通讯经理**

![图像](https://i.ibb.co/p3B8VZR/bowling-covid-full.png)

按天计算的每 10 万人中的新 COVID-19 病例热图（点击放大）

这个热图展示了 COVID-19 大流行在美国的时间进展。地图从左到右阅读，颜色编码显示了各州新增病例的相对数量，已按人口调整。

这个可视化灵感来源于我在一个讨论论坛帖子上看到的类似热图。我始终无法找到其来源，因为那只是一个粘贴的图片，没有链接。原版的热图也是为了表达一个政治观点，将州按照主要政党归属进行区分，我对此不太感兴趣。我对它简明地展示疫情进展的方式感到着迷，因此决定自己制作一个类似的可视化图，并且可以定期更新。

源代码托管在我的[Github 仓库](https://github.com/JasonRBowling/covid19NewCasesPer100KHeatmap)。如果你只对查看这个热图的更新版本感兴趣，我每周在我的[Twitter](https://twitter.com/JRBowling)上发布。需要注意的是，比较不同周的图表时要小心，因为随着新数据的加入，颜色地图可能会发生变化。比较只能在给定的热图内有效。

脚本依赖于 pandas、numpy、matplotlib 和 seaborn。

数据来源于[纽约时报 COVID-19 Github 仓库](https://github.com/nytimes/covid-19-data)。一个简单的启动脚本克隆了最新的仓库副本并复制了所需的文件，然后启动 Python 脚本以创建热图。实际上只需要一个文件，因此可以进一步优化，但这已经有效。

```py
echo "Clearing old data..."
rm -rf covid-19-data/
rm us-states.csv
echo "Getting new data..."
git clone https://github.com/nytimes/covid-19-data
echo "Done."

cp covid-19-data/us-states.csv .
echo "Starting..."

python3 heatmap-newcases.py
echo "Done."
```

脚本首先将包含州人口的 CSV 文件加载到一个字典中，该字典用于缩放每日新增病例结果。新增病例是根据《纽约时报》的数据中的累计总数计算得出的，然后[缩放到每 10 万人中的新增病例数](https://www.robertniles.com/stats/percap.shtml)。

我们可以在这一点上展示热图，但如果这样做，每 10 万人中病例非常多的州会掩盖病例较少的州的细节。应用[log(x+1)](https://onbiostatistics.blogspot.com/2012/05/logx1-data-transformation.html#:~:text=A%3A%20log(x%2B1,in%20which%20x%20was%20measured.)变换显著改善了对比度和可读性。

最后，使用 Seaborn 和 Matplotlib 生成热图并将其保存为图像文件。

就这些！欢迎将其作为你自己可视化的框架。你可以根据需要自定义它，专注于感兴趣的领域。

完整源代码见下方。感谢阅读，希望你觉得有用。

```py
import numpy as np
import seaborn as sns
import matplotlib.pylab as plt
import pandas as pd
import csv
import datetime

reader = csv.reader(open('StatePopulations.csv'))

statePopulations = {}
for row in reader:
    key = row[0]
    if key in statePopulations:
        pass
    statePopulations[key] = row[1:]

filename = "us-states.csv"
fullTable = pd.read_csv(filename)
fullTable = fullTable.drop(['fips'], axis=1)
fullTable = fullTable.drop(['deaths'], axis=1)

# generate a list of the dates in the table
dates = fullTable['date'].unique().tolist()
states = fullTable['state'].unique().tolist()

result = pd.DataFrame()
result['date'] = fullTable['date']

states.remove('Northern Mariana Islands')
states.remove('Puerto Rico')
states.remove('Virgin Islands')
states.remove('Guam')

states.sort()

for state in states:
    # create new dataframe with only the current state's date
    population = int(statePopulations[state][0])
    print(state + ": " + str(population))
    stateData = fullTable[fullTable.state.eq(state)]

    newColumnName = state
    stateData[newColumnName] = stateData.cases.diff()
    stateData[newColumnName] = stateData[newColumnName].replace(np.nan, 0)
    stateData = stateData.drop(['state'], axis=1)
    stateData = stateData.drop(['cases'], axis=1)

    stateData[newColumnName] = stateData[newColumnName].div(population)
    stateData[newColumnName] = stateData[newColumnName].mul(100000.0)

    result = pd.merge(result, stateData, how='left', on='date')

result = result.drop_duplicates()
result = result.fillna(0)

for state in states:
    result[state] = result[state].add(1.0)
    result[state] = np.log10(result[state])
    #result[state] = np.sqrt(result[state])

result['date'] = pd.to_datetime(result['date'])
result = result[result['date'] >= '2020-02-15']
result['date'] = result['date'].dt.strftime('%Y-%m-%d')

result.set_index('date', inplace=True)
result.to_csv("result.csv")
result = result.transpose()

plt.figure(figsize=(16, 10))
g = sns.heatmap(result, cmap="coolwarm", linewidth=0.05, linecolor='lightgrey')
plt.xlabel('')
plt.ylabel('')

plt.title("Daily New Covid-19 Cases Per 100k Of Population", fontsize=20)

updateText = "Updated " + str(datetime.date.today()) + \
    ". Scaled with Log(x+1) for improved contrast due to wide range of values. Data source: NY Times Github. Visualization by @JRBowling"

plt.suptitle(updateText, fontsize=8)

plt.yticks(np.arange(.5, 51.5, 1.0), states)

plt.yticks(fontsize=8)
plt.xticks(fontsize=8)
g.set_xticklabels(g.get_xticklabels(), rotation=90)
g.set_yticklabels(g.get_yticklabels(), rotation=0)
plt.savefig("covidNewCasesper100K.png")
```

**简介： [杰森·鲍林](https://www.linkedin.com/in/jasonbowlingoh/)** 是阿克伦大学网络通信部经理。杰森是一位有着网络管理、安全和医疗设备设计重点的技术专业人士。出色的故障排除技能，优秀的书面沟通能力，和丰富的项目管理经验。你可以在 [Medium 上找到他的更多文章](https://medium.com/@kb8rnu)。

[原文](https://towardsdatascience.com/visualization-of-covid-19-new-cases-over-time-in-python-8c6ac4620c88)。经许可转载。

**相关：**

+   COVID-19 可视化：有效可视化在大流行故事讲述中的力量

+   可视化 COVID-19 影响下的欧洲国家流动趋势

+   通过互动可视化了解 COVID-19 大流行

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT

* * *

### 更多关于这个主题

+   [如何将 Python Pandas 提速超过 300 倍](https://www.kdnuggets.com/how-to-speed-up-python-pandas-by-over-300x)

+   [企业中的机器学习：应用案例与挑战](https://www.kdnuggets.com/2022/08/dss-machine-learning-enterprise-cases-challenges.html)

+   [为什么 TinyML 案例越来越受欢迎？](https://www.kdnuggets.com/2022/10/tinyml-cases-becoming-popular.html)

+   [NoSQL 数据库及其应用案例](https://www.kdnuggets.com/2023/03/nosql-databases-cases.html)

+   [DALLE-3 的 5 个应用案例](https://www.kdnuggets.com/5-use-cases-of-dalle-3)

+   [AI 和 LLM 使用案例中的向量数据库](https://www.kdnuggets.com/vector-databases-in-ai-and-llm-use-cases)
