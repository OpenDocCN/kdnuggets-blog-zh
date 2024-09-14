# 如何识别时间序列数据集中的缺失数据

> 原文：[https://www.kdnuggets.com/how-to-identify-missing-data-in-timeseries-datasets](https://www.kdnuggets.com/how-to-identify-missing-data-in-timeseries-datasets)

时间序列数据，几乎每秒钟从多个来源收集，通常会面临[若干数据质量问题](https://towardsdatascience.com/how-to-do-an-eda-for-time-series-cbb92b3b1913)，其中包括缺失数据。

在序列数据的背景下，缺失信息可能由于多种原因产生，例如获取系统出现错误（如传感器故障）、[传输过程中的错误](https://towardsdatascience.com/understand-your-data-in-real-time-1f6d9f6937e5)（例如，网络连接故障）或数据收集中的错误（例如，数据记录中的人为错误）。这些情况通常会在我们的数据集中产生零散和明确的缺失值，形成数据流中的小间隙。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的IT工作

* * *

此外，[缺失信息](https://ydata.ai/resources/what-is-missing-data-in-machine-learning)也可能由于领域本身的特性自然产生，导致数据中的较大间隙。例如，一个特征在某段时间内停止收集，从而产生非显式的缺失数据。

无论底层原因如何，时间序列数据中存在缺失数据对预测和建模都是极为不利的，并可能对个人（例如，误导性的风险评估）和商业结果（例如，偏颇的商业决策、收入和机会的损失）产生严重后果。

在准备数据以进行建模时，一个重要步骤是能够识别这些未知信息的模式，因为它们将帮助我们决定[处理数据的最佳方法](https://ydata.ai/resources/understanding-the-structure-of-time-series-datasets)，无论是通过某种形式的对齐修正、数据插补、数据填补，还是在某些情况下，逐例删除（即，忽略在特定分析中使用的特征的缺失值所在的案例）。

因此，进行彻底的探索性数据分析和数据概况分析不仅对理解数据特征至关重要，而且对如何最佳准备数据以进行分析做出明智决策也不可或缺。

在这个实操教程中，我们将探索[ydata-profiling](https://github.com/ydataai/ydata-profiling)如何通过新版本中最近引入的功能帮助我们解决这些问题。我们将使用[美国污染数据集](https://data.world/data-society/us-air-pollution-data)，该数据集可在[Kaggle](https://www.kaggle.com/datasets/sogun3/uspollution?resource=download)（许可证[DbCL v1.0](https://opendatacommons.org/licenses/dbcl/1-0/)）中获取，详细描述了美国各州NO2、O3、SO2和CO污染物的信息。

# 实操教程：对美国污染数据集的分析

为了启动我们的教程，我们首先需要安装ydata-profiling的最新版本：

```py
pip install ydata-profiling==4.5.1
```

然后，我们可以加载数据，去除不必要的特征，集中关注我们要调查的内容。为了这个示例，我们将专注于亚利桑那州Maricopa Scottsdale站点的空气污染物测量的特定行为：

```py
import pandas as pd

data = pd.read_csv("data/pollution_us_2000_2016.csv")
data = data.drop('Unnamed: 0', axis = 1) # dropping unnecessary index 

# Select data from Arizona, Maricopa, Scottsdale (Site Num: 3003)
data_scottsdale = data[data['Site Num'] == 3003].reset_index(drop=True)
```

现在，我们准备开始对数据集进行概况分析了！请记住，要使用时间序列分析，我们需要传递参数`tsmode=True`，以便ydata-profiling可以识别时间相关特征：

```py
# Change 'Data Local' to datetime
data_scottsdale['Date Local'] = pd.to_datetime(data_scottsdale['Date Local'])

# Create the Profile Report
profile_scottsdale = ProfileReport(data_scottsdale, tsmode=True, sortby="Date Local")
profile_scottsdale.to_file('profile_scottsdale.html')
```

## 时间序列概览

输出报告将与我们已经了解的内容相似，但体验有所改进，并且为时间序列数据提供了新的总结统计数据：

![XXXXX](../Images/8faa4792b0d13bde077357a8d7c6dd57.png)

从概览中，我们可以通过查看提供的总结统计数据对数据集有一个整体了解：

+   它包含14个不同的时间序列，每个序列有8674个记录值；

+   数据集报告了从2000年1月到2010年12月的10年数据；

+   时间序列的平均周期是11小时和（接近）7分钟。这意味着平均而言，我们每11小时进行一次测量。

我们还可以获得数据中所有系列的概览图，无论是原始值还是缩放值：我们可以轻松把握序列的整体变化，以及被测量的组件（NO2、O3、SO2、CO）和特征（均值、首次最大值、首次最大小时、AQI）。

## 检查缺失数据

在对数据有一个整体了解后，我们可以关注每个时间序列的具体细节。

在[ydata-profiling](https://github.com/ydataai/ydata-profiling)的最新版本中，概况报告得到了实质性的改进，专门针对时间序列数据进行了分析，即报告“时间序列”和“缺口分析”指标。通过这些新功能，趋势和缺失模式的识别变得极为简便，现在提供了具体的总结统计数据和详细的可视化。

一件立即显现的事情是所有时间序列都呈现的波动模式，其中某些“跳跃”似乎发生在连续的测量之间。这表明存在缺失数据（“缺失信息的差距”），需要更详细地研究。以`S02 Mean`为例。

![XXXXX](../Images/d380f53de2d6d5aa0e954d88f9d0fa24.png)![XXXXX](../Images/7100b2379969401e5d718e4401ed80b6.png)

在调查Gap Analysis中提供的细节时，我们得到了一些关于识别出差距特征的信息。总体来说，时间序列中存在25个差距，最短为4天，最长为32周，平均为10周。

从展示的可视化中，我们注意到一些“随机”的差距由较细的条纹表示，而较大的差距似乎遵循重复的模式。这表明我们似乎在数据集中有两种不同的缺失数据模式。

较小的差距对应于偶发事件生成的缺失数据，这些事件很可能是由于采集过程中的错误造成的，通常可以容易地插补或从数据集中删除。相反，较大的差距更为复杂，需要更详细地分析，因为它们可能揭示了需要更彻底解决的潜在模式。

在我们的例子中，如果我们调查较大的差距，我们实际上会发现它们反映了一个季节性模式：

```py
df = data_scottsdale.copy()
for year in df["Date Local"].dt.year.unique():
    for month in range(1,13):
        if ((df["Date Local"].dt.year == year) & (df["Date Local"].dt.month ==month)).sum() == 0:
            print(f'Year {year} is missing month {month}.')
```

```py
# Year 2000 is missing month 4.
# Year 2000 is missing month 5.
# Year 2000 is missing month 6.
# Year 2000 is missing month 7.
# Year 2000 is missing month 8.
# (...)
# Year 2007 is missing month 5.
# Year 2007 is missing month 6.
# Year 2007 is missing month 7.
# Year 2007 is missing month 8.
# (...)
# Year 2010 is missing month 5.
# Year 2010 is missing month 6.
# Year 2010 is missing month 7.
# Year 2010 is missing month 8.
```

如预期所示，时间序列中存在一些较大的信息差距，这些差距似乎是重复的，甚至是季节性的：在大多数年份中，数据在5月到8月（第5至8个月）之间未被收集。这可能是由于不可预测的原因，或已知的商业决策，例如与削减成本有关，或者只是与天气模式、温度、湿度和大气条件相关的污染物季节性变化有关。

基于这些发现，我们可以进一步调查为什么会发生这种情况，是否需要采取措施以防止未来发生类似情况，以及如何处理我们目前拥有的数据。

# 最终思考：插补、删除、重新对齐？

在本教程中，我们了解了时间序列中缺失数据模式的重要性，以及有效的数据分析如何揭示缺失信息背后的奥秘。从电信、医疗保健、能源到金融，所有收集时间序列数据的行业都会面临缺失数据的问题，并需要决定最佳处理和提取所有可能知识的方法。

通过全面的数据分析，我们可以根据手头的数据特征做出明智而有效的决策：

+   信息差距可能由收集、传输和采集过程中的错误引起的偶发事件造成。我们可以修复问题以防止再次发生，并根据差距的长度进行插补或填充；

+   信息的缺口也可以代表季节性或重复模式。我们可以选择重构我们的流程，开始收集缺失的信息，或者用来自其他分布式系统的外部信息填补这些缺口。我们还可以识别检索过程是否失败（可能是数据工程方面的操作失误，我们都有过那样的日子！）。

我希望这个教程能为你提供如何适当识别和描述时间序列数据中的缺失数据的一些见解，我迫不及待想看看你在自己的缺失分析中会发现什么！如果有任何问题或建议，请在评论中告诉我，或者在[数据驱动 AI 社区](https://tiny.ydata.ai/dcai-medium)找到我！

**[Fabiana Clemente](https://www.linkedin.com/in/fabiana-clemente/)** 是 YData 的联合创始人和首席数据官，她将数据理解、因果关系和隐私作为主要的工作和研究领域，致力于使数据对组织具有可操作性。作为一位充满热情的数据从业者，她主持了播客《机器学习如何遇见隐私》，并在 Datacast 和 Privacy Please 播客中担任嘉宾讲者。她还在 ODSC 和 PyData 等会议上发言。

### 更多相关内容

+   [识别机器学习可解问题的 4 个因素](https://www.kdnuggets.com/2022/04/4-factors-identify-machine-learning-solvable-problems.html)

+   [使用 Pandas fillna() 输入缺失数据的最佳方式](https://www.kdnuggets.com/2023/02/optimal-way-input-missing-data-pandas-fillna.html)

+   [学生在数据科学简历中缺少的 7 件事](https://www.kdnuggets.com/7-things-students-are-missing-in-a-data-science-resume)

+   [如何使用 Pandas 中的插值技术处理缺失数据](https://www.kdnuggets.com/how-to-deal-with-missing-data-using-interpolation-techniques-in-pandas)

+   [在 NumPy 中使用掩码数组处理缺失数据](https://www.kdnuggets.com/masked-arrays-in-numpy-to-handle-missing-data)

+   [如何使用 Scikit-learn 的 Imputer 模块处理缺失数据](https://www.kdnuggets.com/how-to-handle-missing-data-with-scikit-learns-imputer-module)
