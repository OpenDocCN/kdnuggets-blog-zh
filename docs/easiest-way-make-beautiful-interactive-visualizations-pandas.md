# 使用 Pandas 制作美丽的交互式可视化的最简单方法

> 原文：[https://www.kdnuggets.com/2021/12/easiest-way-make-beautiful-interactive-visualizations-pandas.html](https://www.kdnuggets.com/2021/12/easiest-way-make-beautiful-interactive-visualizations-pandas.html)

**由 [Frank Andrade](https://frank-andrade.medium.com/) 数据科学家和 [Python 教师](https://bit.ly/30A2X3C)**

![使用 Pandas 制作美丽的交互式可视化的最简单方法](../Images/92a35e18a664bd94794bebbed34a9d4b.png)

图片由 [Jerry Zhang](https://unsplash.com/@z734923105?utm_source=medium&utm_medium=referral) 拍摄，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 管理

* * *

如果你一直关注我的[data visualization guides](https://frank-andrade.medium.com/list/data-visualization-in-python-7aea0e3f93ca)，你可能知道我喜欢在 Python 中创建既美观又易读的可视化，而不需要过于技术化或浪费大量时间。

交互式可视化也不例外，因此我已经寻找了一段时间友好的 Python 库。虽然有许多库可以完成这个工作，但当涉及到与 Pandas 配合使用时，情况变得复杂。

幸运的是，现在有一种简单的方法可以直接从 Pandas 创建交互式可视化，我们将在本指南中详细了解其工作原理。

## 首先

## 安装库

要轻松创建交互式可视化，我们需要安装 Cufflinks。这个库将 Pandas 与 Plotly 连接在一起，因此我们可以直接从 Pandas 创建可视化（以前你需要学习解决方法才能让它们一起工作，但现在更简单了）。

首先，确保你通过终端运行以下命令来安装 Pandas 和 Plotly：

```py
pip install pandas
pip install plotly
```

请注意，你也可以使用 conda 安装 Plotly`conda install -c plotly`。

安装 Plotly 后，运行以下命令来安装 Cufflinks：

```py
pip install cufflinks
```

## 导入库

开始工作之前，导入以下库：

```py
import pandas as pd
import cufflinks as cf
from IPython.display import display,HTMLcf.set_config_file(sharing='public',theme='ggplot',offline=True) 
```

在这种情况下，我使用了`‘ggplot’`主题，但可以随意选择任何你喜欢的主题。运行命令`cf.getThemes()`获取所有可用的主题。

要在接下来的部分中使用 Pandas 制作交互式可视化，我们只需要使用语法`dataframe.iplot()`。

## 数据

在本指南中，我们将使用一个人口数据框。首先，从[Google Drive](https://drive.google.com/file/d/1QpCcE4U8NIhznbqf0kdeO2ITKPEs9OSm/view?usp=sharing)或[Github](https://github.com/ifrankandrade/data-visualization/tree/main/datasets/population)下载CSV文件，将文件移动到你的 Python 脚本所在的位置，然后如下面所示在 Pandas 数据框中读取它。

```py
df_population = pd.read_csv('population_total.csv')
```

数据框包含了世界上大多数国家多年来的人口数据，长这样：

![用 Pandas 制作美丽交互式可视化的最简单方法](../Images/66516d5a70a2fd8c78da9f79d407574f.png)

作者提供的图片

这个数据框几乎已经准备好绘制，我们只需要丢弃空值，重新塑形，然后选择几个国家来测试我们的交互式图表。下面的代码完成了这一切。

```py
# dropping null values
df_population = df_population.dropna()# reshaping the dataframe
df_population = df_population.pivot(index='year', columns='country',
                                    values='population')# selecting 5 countries
df_population = df_population[['United States', 'India', 'China', 
                               'Indonesia', 'Brazil']]
```

现在数据框看起来像下面的图片，已经准备好绘制了。

![用 Pandas 制作美丽交互式可视化的最简单方法](../Images/c8f40adc1ec4f5eff95da4c7f26a9622.png)

作者提供的图片

如果你想了解数据科学家如何收集像“population_total.csv”这样的真实世界数据，*请查看我写的这个指南*（[查看这个指南](https://betterprogramming.pub/how-to-use-scrapy-to-build-a-dataset-for-your-data-science-project-8f04af3548c6)）。*（你不一定总是会有 Kaggle 数据集用于数据科学项目）*

现在让我们开始制作交互式可视化吧！

## 线形图

让我们制作一个线形图，以比较1955年到2020年这5个国家的人口增长情况。

如前所述，我们将使用`df_population.iplot(kind='name_of_plot')`语法来制作下图所示的图表。

```py
df_population.iplot(kind='**line**',xTitle='Years', yTitle='Population',
                    title='Population (1955-2020)')
```

![用 Pandas 制作美丽交互式可视化的最简单方法](../Images/4cf4d93a41d01ffa59c4070545952aa4.png)

作者提供的图片

一眼看去，可以很容易看出印度的人口增长速度比其他国家快。

## 条形图

我们可以制作一个单独的条形图来展示按类别分组的条形图。我们来看看。

## 单个条形图

让我们创建一个条形图，展示每个国家到2020年的总人口。为此，首先，我们从索引中选择2020年，然后将行与列转置，以便将年份放入列中。我们将这个新的数据框命名为`df_population_2020`（我们在绘制饼图时还会用到这个数据框）

```py
df_population_2020 = df_population[df_population.index.isin([2020])]
df_population_2020 = df_population_2020.T
```

现在我们可以用`.iplot()`来绘制这个新的数据框。在这种情况下，我将使用`color`参数将条形图颜色设置为浅绿色。

```py
df_population_2020.iplot(kind='**bar**', color='lightgreen',
                         xTitle='Years', yTitle='Population',
                         title='Population in 2020')
```

![用 Pandas 制作美丽交互式可视化的最简单方法](../Images/ea171837b7e5ab08fbb6219a84084116.png)

作者提供的图片

## 按“n”变量分组的条形图

现在让我们看看每个十年初的人口变化。

```py
# filter years out
df_population_sample = df_population[df_population.index.isin([1980, 1990, 2000, 2010, 2020])]# plotting
df_population_sample.iplot(kind='**bar**', xTitle='Years',
                           yTitle='Population')
```

![用 Pandas 制作美丽交互式可视化的最简单方法](../Images/1b5a78575876458ee6b69fecd7fbb53a.png)

作者提供的图片

自然，所有国家的总人口都在这些年中增长了，但有些国家的增长速度更快。

## 箱形图

箱型图在我们想查看数据分布时非常有用。箱型图将揭示最小值、第一个四分位数（Q1）、中位数、第三个四分位数（Q3）和最大值。查看这些值的最简单方法是创建互动可视化。

让我们看看美国的人口分布。

```py
df_population['United States'].iplot(kind='**box**', color='green', 
                                     yTitle='Population')
```

![用 Pandas 制作美丽互动可视化的最简单方法](../Images/0525777f50ea01220867f042673cb1d3.png)

作者提供的图片

假设我们现在想获取所有选定国家的相同分布。

```py
df_population.iplot(kind='**box**', xTitle='Countries',
                    yTitle='Population')
```

![用 Pandas 制作美丽互动可视化的最简单方法](../Images/3792cedd09244d5e8fe4352e0429f69b.png)

作者提供的图片

如我们所见，我们还可以通过点击右侧的图例来过滤任何国家。

## 直方图

直方图表示数值数据的分布。让我们看看美国和印度尼西亚的人口分布。

```py
df_population[['United States', 'Indonesia']].iplot(kind='**hist**',
                                                xTitle='Population')
```

![用 Pandas 制作美丽互动可视化的最简单方法](../Images/48756348b485a7d7dd63129b02ecc675.png)

作者提供的图片

## 饼图

让我们再次比较2020年的人口数据，但这次使用饼图。为此，我们将使用在“单条柱状图”部分创建的`df_population_2020`数据框。

然而，为了制作饼图，我们需要将“国家”作为列而不是索引，因此我们使用`.reset_index()`来恢复列。然后我们将`2020`转换为字符串。

```py
# transforming data
df_population_2020 = df_population_2020.reset_index()
df_population_2020 =df_population_2020.rename(columns={2020:'2020'})# plotting
df_population_2020.iplot(kind='**pie**', labels='country',
                         values='2020',
                         title='Population in 2020 (%)')
```

![用 Pandas 制作美丽互动可视化的最简单方法](../Images/b50b37bc5b5c28326e46edbb51556f95.png)

作者提供的图片

## 散点图

尽管人口数据不适合做散点图（数据遵循常见模式），但为了本指南的目的，我仍会制作这个图。

制作散点图类似于折线图，但我们必须添加`mode`参数。

```py
df_population.iplot(kind='**scatter'**, **mode**='markers')
```

![用 Pandas 制作美丽互动可视化的最简单方法](../Images/75638c1317f0366c0c812d1e856f93e5.png)

就这样！现在你已经准备好使用 Pandas 制作自己的美丽互动可视化。如果你想学习 Python 中其他可视化库，如 Matplotlib 和 Seaborn，并且还想了解如何制作词云，[查看我制作的这些指南](https://frank-andrade.medium.com/list/data-visualization-in-python-7aea0e3f93ca)。

[**加入我的3k+人邮件列表，获取我在所有教程中使用的 Python 数据科学备忘单（免费 PDF）**](https://frankandrade.ck.page/bd063ff2d3)

如果你喜欢阅读这样的故事并且想支持我作为作者，请考虑注册成为 Medium 会员。每月$5，你可以无限制访问成千上万的 Python 指南和数据科学文章。如果你通过[我的链接](https://frank-andrade.medium.com/membership)注册，我将获得少量佣金，对你没有额外费用。

**简历: [Frank Andrade](https://frank-andrade.medium.com/)** 是数据科学家，[Python 教师](https://bit.ly/30A2X3C)，以及 Medium 上的前1000名作者。

[原文](https://towardsdatascience.com/the-easiest-way-to-make-beautiful-interactive-visualizations-with-pandas-cdf6d5e91757)。经许可转载。

### 了解更多相关内容

+   [在本地运行 Llama 3 的最简单方法](https://www.kdnuggets.com/easiest-way-of-running-llama-3-locally)

+   [使用 Python 图形库制作惊人的可视化](https://www.kdnuggets.com/2022/12/make-amazing-visualizations-python-graph-gallery.html)

+   [使用 Seaborn 创建美丽的直方图](https://www.kdnuggets.com/2023/01/creating-beautiful-histograms-seaborn.html)

+   [使用 Python 和 Beautiful Soup 进行网页抓取的逐步指南](https://www.kdnuggets.com/2023/04/stepbystep-guide-web-scraping-python-beautiful-soup.html)

+   [使用 Pandas fillna() 输入缺失数据的最佳方法](https://www.kdnuggets.com/2023/02/optimal-way-input-missing-data-pandas-fillna.html)

+   [将 JSON 转换为 Pandas DataFrames：正确解析的方法](https://www.kdnuggets.com/converting-jsons-to-pandas-dataframes-parsing-them-the-right-way)
