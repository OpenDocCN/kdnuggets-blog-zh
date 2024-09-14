# 如何在Python中生成FiveThirtyEight图表

> 原文：[https://www.kdnuggets.com/2017/12/generate-fivethirtyeight-graphs-python.html](https://www.kdnuggets.com/2017/12/generate-fivethirtyeight-graphs-python.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由[Alex Olteanu](https://www.dataquest.io/blog/author/alex-olteanu/)，Dataquest.io的学生成功专家**

如果你阅读数据科学文章，可能已经碰到过[FiveThirtyEight](https://fivethirtyeight.com/)的内容。自然，你对他们的[精彩可视化](https://fivethirtyeight.com/features/the-52-best-and-weirdest-charts-we-made-in-2016/)印象深刻。你想制作自己的精彩可视化，因此询问了[Quora](https://www.quora.com/How-does-FiveThirtyEight-create-their-data-visualizations)和[Reddit](https://www.reddit.com/r/statistics/comments/2jon2b/anyone_knows_how_are_made_the_graphs_on/)怎么做。你收到了些回答，但它们相当模糊。你仍然无法自己制作图表。

在这篇文章中，我们将帮助你。使用Python的[matplotlib](https://matplotlib.org/index.html#)和[pandas](http://pandas.pydata.org/pandas-docs/stable/index.html)，我们将看到复制任何FiveThirtyEight（FTE）可视化的核心部分是相当简单的。

我们从这里开始：

![default_graph](../Images/0c08238c4969eb5ff53e0ccfa7e48e54.png)

然后，在教程结束时，达到这里：

![final3](../Images/02b129309908aee0d2db50c351d3d94d.png)

要跟随教程，你至少需要一些Python的基础知识。如果你知道方法和属性之间的区别，那么你已经准备好了。

### 介绍数据集

我们将使用描述1970年至2011年间美国女性获得学士学位百分比的数据。我们将使用数据科学家[Randal Olson](http://www.randalolson.com/2014/06/14/percentage-of-bachelors-degrees-conferred-to-women-by-major-1970-2012/)编制的数据集，他从[国家教育统计中心](https://nces.ed.gov/about/)收集了这些数据。

如果你想自己动手编写代码，可以从[Randal's blog](http://www.randalolson.com/wp-content/uploads/percent-bachelors-degrees-women-usa.csv)下载数据。为了节省时间，你可以跳过下载文件，直接将直接链接传递给pandas的`read_csv()`[函数](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html?highlight=read_csv#pandas.read_csv)。在下面的代码单元中，我们：

+   导入pandas模块。

+   将指向数据集的直接链接分配给名为`direct_link`的变量。

+   使用`read_csv()`读取数据，并将内容分配给`women_majors`。

+   通过使用 `info()` [方法](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.info.html?highlight=dataframe%20info#pandas.DataFrame.info) 打印数据集的信息。我们要找出行数和列数，同时检查空值。

+   使用 `head()` [方法](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.head.html?highlight=dataframe%20head#pandas.DataFrame.head) 显示前五行，以便更好地了解数据集的结构。

```py
import pandas as pd

direct_link = 'http://www.randalolson.com/wp-content/uploads/percent-bachelors-degrees-women-usa.csv'
women_majors = pd.read_csv(direct_link)

print(women_majors.info())
women_majors.head()

```

```py
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 42 entries, 0 to 41
Data columns (total 18 columns):
Year                             42 non-null int64
Agriculture                      42 non-null float64
Architecture                     42 non-null float64
Art and Performance              42 non-null float64
Biology                          42 non-null float64
Business                         42 non-null float64
Communications and Journalism    42 non-null float64
Computer Science                 42 non-null float64
Education                        42 non-null float64
Engineering                      42 non-null float64
English                          42 non-null float64
Foreign Languages                42 non-null float64
Health Professions               42 non-null float64
Math and Statistics              42 non-null float64
Physical Sciences                42 non-null float64
Psychology                       42 non-null float64
Public Administration            42 non-null float64
Social Sciences and History      42 non-null float64
dtypes: float64(17), int64(1)
memory usage: 6.0 KB
None

```

|  | YEAR | AGRICULTURE | ARCHITECTURE | ART AND PERFORMANCE | BIOLOGY | BUSINESS | COMMUNICATIONS AND JOURNALISM | COMPUTER SCIENCE | EDUCATION | ENGINEERING | ENGLISH | FOREIGN LANGUAGES | HEALTH PROFESSIONS | MATH AND STATISTICS | PHYSICAL SCIENCES | PSYCHOLOGY | PUBLIC ADMINISTRATION | SOCIAL SCIENCES AND HISTORY |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | 1970 | 4.229798 | 11.921005 | 59.7 | 29.088363 | 9.064439 | 35.3 | 13.6 | 74.535328 | 0.8 | 65.570923 | 73.8 | 77.1 | 38.0 | 13.8 | 44.4 | 68.4 | 36.8 |
| 1 | 1971 | 5.452797 | 12.003106 | 59.9 | 29.394403 | 9.503187 | 35.5 | 13.6 | 74.149204 | 1.0 | 64.556485 | 73.9 | 75.5 | 39.0 | 14.9 | 46.2 | 65.5 | 36.2 |
| 2 | 1972 | 7.420710 | 13.214594 | 60.4 | 29.810221 | 10.558962 | 36.6 | 14.9 | 73.554520 | 1.2 | 63.664263 | 74.6 | 76.9 | 40.2 | 14.8 | 47.6 | 62.6 | 36.1 |
| 3 | 1973 | 9.653602 | 14.791613 | 60.2 | 31.147915 | 12.804602 | 38.4 | 16.4 | 73.501814 | 1.6 | 62.941502 | 74.9 | 77.4 | 40.9 | 16.5 | 50.4 | 64.3 | 36.4 |
| 4 | 1974 | 14.074623 | 17.444688 | 61.9 | 32.996183 | 16.204850 | 40.5 | 18.9 | 73.336811 | 2.2 | 62.413412 | 75.3 | 77.9 | 41.8 | 18.2 | 52.6 | 66.1 | 37.3 |

除了 `Year` 列外，其他每一列的名称表示学士学位的学科。学士列中的每个数据点表示授予女性的学士学位的百分比。因此，每一行描述了给定年份授予女性的各种学士学位的百分比。

如前所述，我们的数据从1970年到2011年。为了确认后一个限制，让我们使用 `tail()` [方法](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.tail.html?highlight=tail#pandas.DataFrame.tail) 打印数据集的最后五行：

```py
women_majors.tail()

```

|  | YEAR | AGRICULTURE | ARCHITECTURE | ART AND PERFORMANCE | BIOLOGY | BUSINESS | COMMUNICATIONS AND JOURNALISM | COMPUTER SCIENCE | EDUCATION | ENGINEERING | ENGLISH | FOREIGN LANGUAGES | HEALTH PROFESSIONS | MATH AND STATISTICS | PHYSICAL SCIENCES | PSYCHOLOGY | PUBLIC ADMINISTRATION | SOCIAL SCIENCES AND HISTORY |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 37 | 2007 | 47.605026 | 43.100459 | 61.4 | 59.411993 | 49.000459 | 62.5 | 17.6 | 78.721413 | 16.8 | 67.874923 | 70.2 | 85.4 | 44.1 | 40.7 | 77.1 | 82.1 | 49.3 |
| 38 | 2008 | 47.570834 | 42.711730 | 60.7 | 59.305765 | 48.888027 | 62.4 | 17.8 | 79.196327 | 16.5 | 67.594028 | 70.2 | 85.2 | 43.3 | 40.7 | 77.2 | 81.7 | 49.4 |
| 39 | 2009 | 48.667224 | 43.348921 | 61.0 | 58.489583 | 48.840474 | 62.8 | 18.1 | 79.532909 | 16.8 | 67.969792 | 69.3 | 85.1 | 43.3 | 40.7 | 77.1 | 82.0 | 49.4 |
| 40 | 2010 | 48.730042 | 42.066721 | 61.3 | 59.010255 | 48.757988 | 62.5 | 17.6 | 79.618625 | 17.2 | 67.928106 | 69.0 | 85.0 | 43.1 | 40.2 | 77.0 | 81.7 | 49.3 |
| 41 | 2011 | 50.037182 | 42.773438 | 61.2 | 58.742397 | 48.180418 | 62.2 | 18.2 | 79.432812 | 17.5 | 68.426730 | 69.5 | 84.8 | 43.1 | 40.1 | 76.7 | 81.9 | 49.2 |

### 我们 FiveThirtyEight 图形的背景

几乎每个 FTE 图表都是文章的一部分。图表通过展示小故事或有趣的想法来补充文本。在复制我们的 FTE 图表时，我们需要注意这一点。

为了避免在本教程中跑题，我们假装已经写了大部分关于美国教育性别差距演变的文章。现在我们需要创建一个图表，帮助读者可视化1970年女性情况非常糟糕的学士学位的性别差距演变。我们已经设置了20%的阈值，现在我们想要绘制所有1970年女性毕业生百分比低于20%的学士学位的演变。

首先识别这些特定的学士学位。在以下代码单元中，我们将：

+   使用`.loc`，这是一个[基于标签的索引器](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.loc.html?highlight=loc#pandas.DataFrame.loc)，来：

    +   选择第一行（对应于1970年）；

    +   选择第一行中值小于20的项；`Year`字段也将被检查，但显然不会包含在内，因为1970远大于20。

+   将结果内容分配给`under_20`。

```py
under_20 = women_majors.loc[0, women_majors.loc[0] < 20]
under_20

```

```py
Agriculture           4.229798
Architecture         11.921005
Business              9.064439
Computer Science     13.600000
Engineering           0.800000
Physical Sciences    13.800000
Name: 0, dtype: float64

```

### 使用 matplotlib 的默认样式

让我们开始制作我们的图表。我们将首先查看默认情况下可以构建的内容。在以下代码块中，我们将：

+   运行 Jupyter 魔法`%matplotlib`来[启用 Jupyter 和 matplotlib 的有效协作](http://ipython.readthedocs.io/en/stable/interactive/plotting.html#id1)，并添加`inline`以使我们的图表显示在笔记本内部。

+   使用`plot()` [方法](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.plot.html?highlight=plot#pandas.DataFrame.plot)绘制图形，应用于`women_majors`。我们传入`plot()`以下参数：

    +   `x` - 指定用于 x 轴的`women_majors`中的列；

    +   `y` - 指定用于 y 轴的`women_majors`中的列；我们将使用`under_20`的索引标签，这些标签存储在该对象的`.index`属性中；

    +   `figsize` - 将图形的大小设置为`tuple`格式的`(宽度, 高度)`，单位为英寸。

+   将绘图对象分配给名为 `under_20_graph` 的变量，并打印其类型以显示 pandas 在幕后使用 `matplotlib` 对象。

```py
%matplotlib inline
under_20_graph = women_majors.plot(x = 'Year', y = under_20.index, figsize = (12,8))
print('Type:', type(under_20_graph))

```

![](../Images/a593bbd04871b2356ba6b00209b670ab.png)

### 使用 matplotlib 的 fivethirtyeight 样式

上图具有一些特征，例如脊柱的宽度和颜色、y轴标签的字体大小、没有网格等。所有这些特征构成了 matplotlib 的默认样式。

简短插曲，值得一提的是，在本文中我们将使用一些图形部分的技术术语。如果你在任何时候感到迷失，可以参考下面的图例。

![anatomy1](../Images/0f47f940dc093daadce8595661fb8dca.png)

来源：[Matplotlib.org](http://matplotlib.org/faq/usage_faq.html#parts-of-a-figure)

除了默认样式外，matplotlib 还带有几个内置样式，我们可以直接使用。要查看可用样式的列表，我们将：

+   导入 `matplotlib.style` [模块](https://matplotlib.org/api/style_api.html?highlight=style%20available#module-matplotlib.style)，并命名为 `style`。

+   探索 `matplotlib.style.available` 的内容（这是该模块的一个预定义变量），其中包含所有可用的内置样式列表。

```py
import matplotlib.style as style
style.available

```

```py
['seaborn-deep',
 'seaborn-muted',
 'bmh',
 'seaborn-white',
 'dark_background',
 'seaborn-notebook',
 'seaborn-darkgrid',
 'grayscale',
 'seaborn-paper',
 'seaborn-talk',
 'seaborn-bright',
 'classic',
 'seaborn-colorblind',
 'seaborn-ticks',
 'ggplot',
 'seaborn',
 '_classic_test',
 'fivethirtyeight',
 'seaborn-dark-palette',
 'seaborn-dark',
 'seaborn-whitegrid',
 'seaborn-pastel',
 'seaborn-poster']

```

你可能已经观察到，有一个内置样式叫做 `fivethirtyeight`。让我们使用这个样式，看看会发生什么。为此，我们将使用同一 `matplotlib.style` 模块中恰如其分命名的 `use()` [函数](https://matplotlib.org/api/style_api.html?highlight=style%20available#matplotlib.style.use)（我们以 `style` 名义导入）。然后我们将使用之前相同的代码生成图形。

```py
style.use('fivethirtyeight')
women_majors.plot(x = 'Year', y = under_20.index, figsize = (12,8))

```

![538_graphs_AO_11_1](../Images/cdee0cf7d81b8bba45d2ceb49508800d.png)

![](../Images/c69d43b978cacd11f996c059d1cb2047.png)

哇，这真是一个重大变化！与我们的第一个图形相比，我们可以看到这个图形有不同的背景颜色，有网格线，完全没有脊柱，主要刻度标签的粗细和字体大小也不同等。

你可以在 [这里](https://github.com/matplotlib/matplotlib/blob/38be7aeaaac3691560aeadafe46722dda427ef47/lib/matplotlib/mpl-data/stylelib/fivethirtyeight.mplstyle) 阅读 `fivethirtyeight` 样式的技术描述，这也应能让你对使用此样式时运行的代码有一个很好的了解。样式表的作者，[卡梅伦·大卫-皮隆](https://github.com/CamDavidsonPilon)，在 [这里](https://dataorigami.net/blogs/napkin-folding/17543615-replicating-538s-plot-styles-in-matplotlib) 讨论了一些特征。

*有关在 Python 中生成 FiveThirtyEight 图形的更多信息，请参见 [原始文章的其余部分](https://www.dataquest.io/blog/making-538-plots/)。*

**简历: [亚历克斯·奥尔特亚努](https://www.dataquest.io/blog/author/alex-olteanu/)** 是 Dataquest.io 的学生成功专员。他喜欢学习和分享知识，并为新的 AI 革命做准备。

[原始文章](https://www.dataquest.io/blog/making-538-plots/)。经授权转载。

**相关：**

+   [分析科学研究人员的迁移](/2017/11/analyzing-migration-scientific-researchers.html)

+   [7 种技术可视化地理空间数据](/2017/10/7-techniques-visualize-geospatial-data.html)

+   [Python 图表画廊](/2017/11/python-graph-gallery.html)

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT 部门

* * *

### 更多相关内容

+   [如何生成合成表格数据集](https://www.kdnuggets.com/2022/03/generate-tabular-synthetic-dataset.html)

+   [使用开源工具生成合成时间序列数据](https://www.kdnuggets.com/2022/06/generate-synthetic-timeseries-data-opensource-tools.html)

+   [利用 ChatGPT 生成被动收入的 4 种方法](https://www.kdnuggets.com/2023/03/4-ways-generate-passive-income-chatgpt.html)

+   [使用 Google MusicLM 从文本生成音乐](https://www.kdnuggets.com/2023/06/generate-music-text-google-musiclm.html)

+   [利用 Stable Diffusion 生成超真实的面孔的 3 种方法](https://www.kdnuggets.com/3-ways-to-generate-hyper-realistic-faces-using-stable-diffusion)

+   [结合数据管理与数据讲述以创造价值](https://www.kdnuggets.com/combining-data-management-and-data-storytelling-to-generate-value)
