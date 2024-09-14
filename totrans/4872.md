# 使用fast.ai进行快速日期特征工程

> 原文：[https://www.kdnuggets.com/2018/03/feature-engineering-dates-fastai.html](https://www.kdnuggets.com/2018/03/feature-engineering-dates-fastai.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

正如你所了解的，简单的日期字段是潜在的数据宝库。虽然乍一看，日期只是时间线上的一个特定点，但了解这个点相对于其他点的位置可以为数据集生成各种洞察。例如，可以从日期字段中提取出大量简单的问题：

+   那是一个周末吗？

+   那天是星期几？

+   日期是否在一个季度末？

+   那天是节假日吗？

+   那天是否有奥运会？

你从日期中想要什么取决于你在做什么。拥有包含一些上述不那么固有的问题的外部资源（“那天是否有奥运会？”——根据你的项目，可能是一个完全有效的问题）肯定是必要的，但即使是解决更基础的问题也可能极为有用。

对日期进行简单的特征工程可以不费吹灰之力地处理这些任务。当我最近坐下来编写一个通用的、可重用的函数来做这件事时，我很快意识到[Jeremy Howard](https://twitter.com/jeremyphoward)已经在他的fast.ai库中处理了这个问题。

如果你不知道，[fast.ai库](https://github.com/fastai/fastai)是一个流行机器学习库的补充包装集合，旨在消除编写自己的函数来处理机器学习工作流中的一些重复任务的必要性。

在fast.ai中确实存在这样一个函数，它在后台利用Pandas来自动化一些简单的日期相关特征工程。

让我们来看看。但首先，让我们获取一些简单的数据进行测试。有关数据和如何在下面的代码中收集数据的更多信息，请参阅[数据科学的红利——金融数据分析的温和介绍](/2017/04/data-science-dividends-intro-financial-data-analysis.html)。此外，理应说明，这假设你已经安装了fast.ai库。

```py
import datetime
import pandas as pd
from pandas_datareader import data
from fastai.structured import add_datepart

# Set source and rate of interest
source = 'fred'
rate = 'DEXCAUS'

# Date range is 2016
start = datetime.datetime(2016, 1, 1)
end = datetime.datetime(2016, 12, 31)
cadusd = data.DataReader(rate, source, start, end)

# Write it to a CSV 
cadusd.to_csv('cadusd.csv')

# Read the CSV back in (there is a peculiarity accounting for this)
cadusd = pd.read_csv('cadusd.csv')

# Set datetime precision to 'day'
cadusd['DATE'] = cadusd['DATE'].astype('datetime64[D]')

cadusd['DEXCAUS'].plot(kind='line', grid=True, title='CAD to USD Conversion Rates, 2016')
cadusd.head()
```

![dexcaus](../Images/41bb73ec010a7d22c85be4c4aa6778c5.png)

为了让我们了解将要使用的fast.ai函数的代码有多么简单，下面是[当前版本的源代码](https://github.com/fastai/fastai/blob/master/fastai/structured.py)：

```py
def add_datepart(df, fldname, drop=True):
    fld = df[fldname]
    if not np.issubdtype(fld.dtype, np.datetime64):
        df[fldname] = fld = pd.to_datetime(fld, infer_datetime_format=True)
    targ_pre = re.sub('[Dd]ate$', '', fldname)
    for n in ('Year', 'Month', 'Week', 'Day', 'Dayofweek', 'Dayofyear',
            'Is_month_end', 'Is_month_start', 'Is_quarter_end', 'Is_quarter_start', 'Is_year_end', 'Is_year_start'):
        df[targ_pre+n] = getattr(fld.dt,n.lower())
    df[targ_pre+'Elapsed'] = fld.astype(np.int64) // 10**9
    if drop: df.drop(fldname, axis=1, inplace=True)
```

Howard在许多地方表示，他编写fast.ai包装器的目标是用尽可能少的代码创建有用的函数，使其简洁、易读并集中于目标。

对函数的简单调用会展开DATE字段：

```py

add_datepart(cadusd, 'DATE')
```

唯一可能的附加参数是'drop'，当设置为True时，会从数据框中删除原始日期列。

这是部分结果：

![新功能](../Images/473171a63c95941aeee84a883634b76b.png)

特征的完整列表：

![功能列表](../Images/63a2d87efc8cb7ea2cc53f7932d49bb4.png)

由于没有机器学习算法能够查看一个日期时间对象并自动推断上述内容，这样简单的步骤提供了对数据集进行自动洞察的可能性。在不需要思考的情况下，现在各种日期元数据都触手可及，例如这一天是否是年初、季度初或月初，并且通过进一步简单的推断，是否是周末、是否在三月、是哪一年等等。

这种基本的特征工程应该在我看来表明两个具体的事情：

1.  对时间序列数据进行特征工程不应感到畏惧

1.  你可能想看看 fast.ai 库

**相关**：

+   [时间序列基础知识——三步流程](/2018/03/time-series-dummies-3-step-process.html)

+   [深度特征合成：自动特征工程如何运作](/2018/02/deep-feature-synthesis-automated-feature-engineering.html)

+   [时间序列数据的自动特征工程](/2017/11/automated-feature-engineering-time-series-data.html)

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

### 更多相关主题

+   [特征存储峰会 2022：关于特征工程的免费会议](https://www.kdnuggets.com/2022/10/hopsworks-feature-store-summit-2022-free-conference-feature-engineering.html)

+   [使用 RAPIDS cuDF 利用 GPU 进行特征工程](https://www.kdnuggets.com/2023/06/rapids-cudf-leverage-gpu-feature-engineering.html)

+   [寻找合适的注释人员的快速指南](https://www.kdnuggets.com/2022/04/quick-guide-find-right-minds-annotation.html)

+   [学习 SAS 的快速数据科学技巧和窍门](https://www.kdnuggets.com/2022/05/sas-quick-data-science-tips-tricks-learn.html)

+   [7 个 Pandas 绘图函数以快速数据可视化](https://www.kdnuggets.com/7-pandas-plotting-functions-for-quick-data-visualization)

+   [Voronoi 图的快速概述](https://www.kdnuggets.com/2022/11/quick-overview-voronoi-diagrams.html)
