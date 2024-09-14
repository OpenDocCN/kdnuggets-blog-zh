# 如何在 Python（和 R）中可视化数据

> 原文：[https://www.kdnuggets.com/2019/11/visualize-data-python-and-r.html](https://www.kdnuggets.com/2019/11/visualize-data-python-and-r.html)

[评论](#comments)

**由 [SuperDataScience](https://www.superdatascience.com/courses/python-programming) 提供**。

在某些鸡尾酒会上，你可以通过争论许多问题可以归结为数据的展示，而不是数据本身来获得成功。脱欧？你可能会说这是因为没有制作引人入胜、易于理解的预测生活质量变化的数据可视化。或者你可能会提到 Facebook，即使按照松散的加州标准，实际上也在进行数据可视化；数据是社交网络中的数据，被人为地更加具体。 [空气](https://projects.sfchronicle.com/trackers/california-fire-map/air-quality/) [质量](https://www.nytimes.com/interactive/2019/10/10/climate/driving-emissions-map.html)? 交通？你甚至可以阐述如何通过正确的数据可视化来灵活运用工具，尽管一切看起来仍像你的拇指，但至少你会是对的。

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 事务

* * *

![](../Images/80e8bc73e8b9d19f0adfbaec4f798241.png)

*平均速度图：湾区大桥上每五分钟的速度变化，基于一个传感器（位于靠近珍珠岛）。在所有合理的通勤时间，速度骤降，展示了固定供应和需求的规律，这大多数人称之为交通。使用 Matplotlib 制作。*

没有人说找到这些特定的鸡尾酒会很容易或令人兴奋。但无论如何，能够制作易于理解的数据可视化是数据科学的关键技能。让我们为数据可视化师们干杯；那些勇敢地将抽象数字变得更直观、使电子表格更具吸引力和技术报告更易于管理的人。如果你还需要更多的鸡尾酒会谈资，你还可以提到 [W.E. Dubois](https://www.brainpickings.org/2017/10/09/w-e-b-du-bois-diagrams/) 和 [Kurt Vonnegut](https://www.youtube.com/watch?v=oP3c1h8v2ZQ) 作为其他可视化大师。

### Python 和 R 走进酒吧……

[教会-图灵论题](https://en.wikipedia.org/wiki/Church–Turing_thesis)表示，你在一个程序中能做的事情，理论上可以在任何其他程序中做。抽象来说，这是对的。然而，实际上，某些语言或软件包中容易做到的事情，在另一个语言或软件包中可能需要数小时的宝贵挫折感来完成。（我在看你，Matlab。）显然，这些差异与我们的大脑如何与编程语言互动、我们对它的了解程度以及编程语言的基本操作如何适应手头的问题有很大关系。正如你可能知道的那样，两个主要的通用数据编程语言是Python和R，但直接比较它们是不公平的。更好的比较是R与使用[Pandas包](https://pandas.pydata.org/)配合Jupyter Notebook的效果。（为了完全透明，我是“Pandas通常更酷，除非你有一些尚未移植到Python包中的非常具体的问题”阵营的一员。）

说完这些，你需要知道以下内容。

Pandas首次创建于2008年，而Python本身则在1991年首次发布。许多使用Python的人声称它“易于思考”。另一方面，R实际上是统计编程语言S的90年代中期实现，而S本身是在70年代中期的Bell Labs发明的。

尽管R的管理机构总部设在维也纳，但使用它并不会让你在华尔兹舞方面变得更好（实际上，它可能让你更糟），也不会让你在吃[Manner Wafer饼干](https://en.wikipedia.org/wiki/Manner_(confectionery))时更出色，维也纳爱乐乐团的折扣票也不会出现在你的终端上。我能说什么呢，生活就是这么艰难。此外，给明智的人提个醒：将编程技能转移到华尔兹舞上唯一的办法是使用[三进制计算机](https://en.wikipedia.org/wiki/Ternary_computer)进行编码；两者都是基于三进制的。话虽如此，R被设置用于实验室环境中所需的数据分析，产生的材料经过同行评审。鉴于Church、Turing以及每一位开源贡献者的上述工作，Pandas可以做同样的事情（只需确保导入[statsmodels](https://www.statsmodels.org/stable/index.html)），通常运行得更快，且更容易优化（使用[Numba](https://numba.pydata.org/)和[Numpy](https://numpy.org/)）。

在我看来，当由专家或用于小众分析时，R可以是一个强大的语言。然而，对于非专家来说，R可能更难以审计。由于类似的原因，R也更容易在数据处理管道中引入未被发现的隐性错误。换句话说，我认为R代码比Python代码更容易积累技术债务。另一方面，能够阅读R是有用的，显然如果你想在基于R的环境中工作，这些建议会有所不同。[这是Pandas与R的简短语法比较](https://pandas.pydata.org/pandas-docs/stable/getting_started/comparison/comparison_with_r.html)。如果你和主要使用R的人谈论这一段，他们要么会热情地承认失败，要么会提出与我的观点截然相反的合理观点。你的体验可能会有所不同。

本文中的建议旨在帮助你更好地理解数据集、获取洞察力并向他人传达结果。这与例如《纽约时报》的艺术化可视化目的不同。（如果你希望做出同样出色的效果，你可能还想了解D3，或者D3在[R](https://rstudio.github.io/r2d3/index.html)中的封装，或[python](https://github.com/altair-viz/altair)中的等效工具。）

最后，有很多选择。尽管感到沮丧的程序员可能会不同意教会-图灵论题的实际解释，但数据可视化库的情况更是如此。如果一个数据可视化库无法完成所有常见的可视化，那么它还有什么用呢？

### 一般数据可视化建议

+   [阅读Tufte](https://www.edwardtufte.com/tufte/)。

+   每次做新项目时，开始一个新文件夹，将所有相关论文下载到研究子文件夹中。（无论你在做什么，阅读之前提出的内容都是有帮助的。）

+   从一开始就开始写报告/白皮书/论文/总结。作为一个经历过这个过程的人，保存到最后会带来大量的麻烦，这也是一些研究生需要很长时间才能完成论文的原因之一。

+   从一开始就做好笔记。

+   唯一完全有资格写出他们想要的精确描述的人，也正是能够完成这个工作的那类人。

+   三维图像是一个独立的类别，但将几个2D帧拼接在一起的Gif，无论是前后振动还是将视角改变几度，都可以帮助你表达观点。

+   查看[Mike Bockstock的数据可视化](https://bl.ocks.org/mbostock/5562380)以及[NYTimes](https://www.nytimes.com/interactive/2019/10/10/climate/driving-emissions-map.html)创建的可视化。

+   如果你有非技术人员需要答案，弄清楚是否可以让定量结果与视觉结果一致。例如，如果有办法可视化聚类分析的结果，请在讨论任何指标之前先展示这个结果。具体胜于抽象，这就是数据可视化的要点。

### 特定的数据可视化建议

+   数据元素的色度、“色彩丰富度”或饱和度可以被调整以发挥你的优势，因为色度是可加的。如果两个数据元素重叠，它们的饱和度可以加在一起，使得重叠部分更加生动。这是一种“多通道增强”的类型，其中两个数据点重叠的事实通过空间和颜色通道传达。在 matplotlib 中，这可以通过 alpha 控制。

+   [感知均匀色彩系列](https://colorcet.pyviz.org/)也可以用于多通道增强，或为你的可视化添加额外的信息维度。例如，以下图表展示了圣塔莫尼卡市中心停车场的利用情况。在第一个图中，我使用了基本颜色，而在第二个图中，每一年都通过感知颜色映射的等距样本来着色。

![](../Images/b05acde9774f9507e5ad835ea83f5b1f.png)

*y 轴显示了圣塔莫尼卡市中心停车场每五分钟增量的平均停车位数的五个 14 天移动平均值。较高的值意味着停车场更空，x 轴上的‘0’对应于每年新的前五分钟。2019 年的线条在四月底停止，这张图表暗示了以下结论：零售业正在衰退，零售业永存。使用 Matplotlib 制作。*

这是我写的立即生成以下图表的代码。未显示的是我所做的预处理。我将“c”参数设置为一个名为 plasma 的感知色图。

```py
import matplotlib.cm as cm #gets the colormaps

N = 4032 #the number of five minute increments in 14 days

rcParams['figure.figsize'] = 30, 15 #controls a jupyter notebook setting

plt.title("14 Day Moving Average, All Years -- Stacked")

plt.plot(g2015.iloc[N:]['Available'].rolling(N).mean()[N:].values,alpha=.4,c=cm.plasma(1/5,1),label='2015')

plt.plot(g2016.iloc[N:]['Available'].rolling(N).mean()[N:].values,alpha=.4,c=cm.plasma(2/5,1),label='2016')

plt.plot(g2017.iloc[N:]['Available'].rolling(N).mean()[N:].values,alpha=.5,c=cm.plasma(3/5,1),label='2017')

plt.plot(g2018.iloc[N:]['Available'].rolling(N).mean()[N:].values,alpha=.75,c=cm.plasma(4/5,1),label='2018')

plt.plot(g2019.iloc[N:]['Available'].rolling(N).mean()[N:].values,alpha=.9,c=cm.plasma(5/5.5,1),label="2019")

plt.legend()

```

+   在数据点周围添加深色边界可以使它们看起来更清晰，这在你没有大量点需要可视化且点相对较大的情况下有效。查找 matplotlib 设置中的“edge_colors=True”。

+   [Sparklines](https://en.wikipedia.org/wiki/Sparkline) 的一课是，人脑能够解释小的数据元素，特别是当重要的是宏观趋势时。

这些原则在以下数据可视化中得到了说明。例如，大多数点太小而无法辨认。此外，颜色的饱和度或“alpha”属性设置为较少且为100%，这样当点重叠时，它们看起来会变得更暗。

![](../Images/61c33d7e13459d250a9823a1a527bc95.png)

*将高维数据投影到二维空间。这是通过 Matplotlib 制作的。*

+   对于直方图，尝试调整控制箱数的参数，直到你对箱子边界问题有一定的感觉。

+   节点-链接图有其自身的特殊挑战，你可以在[这篇插图文章中](https://medium.com/kineviz-blog/visualizing-node-link-graphs-84a40a9b2fcc)找到更多信息。

### 高级的 Python 数据可视化之旅

在我们生活在这个尘土飞扬的星球上时，有一些我们都应该知道的至关重要的真理。变化是唯一的常量；“自由市场效率”是关于信息流动和感知的一个命题，而不是市场运行的效果；社会基本上就像是“燃烧人节”，只是墙壁更坚固；我们都以每秒一秒的悠闲速度走向死亡（更不用提税季了），而在 Python 中使数据可视化效果更好的最快方法是将以下三行代码放在 Jupyter Notebook 的顶部：

```py
from Matplotlib import pyplot as plt

import Seaborn as sns

sns.set()

```

一旦你完成了这些，你可以回到使用 Matplotlib 和思考时空的广阔以及整个的人类努力，仿佛什么都没有发生。实际上，发生的事情是我们使用了[Seaborn](https://seaborn.pydata.org/)的默认设置来清理 Matplotlib。（如果你不知道，Seaborn 基本上是一个经过清理的、更高级的 Matplotlib 版本，而 Matplotlib 本身是以 Matlab 为模型的。）如果 Matplotlib 证明过于繁琐，可以尝试 Seaborn。

让我给你展示一下区别。首先，这里有一些 matplotlib 代码用于可视化数据：

```py
plt.scatter(range(len(counts)),counts)

plt.title("A Random Scatter Plot")

```

![](../Images/34795b935fcd56e6dfe51ec8e9dace71.png)

*“之前”。*

对比一下如果我运行以下代码会发生什么：

```py
sns.set()

plt.scatter(range(len(counts)),counts,s=12)

plt.title("A Random Scatter Plot: Seaborn Defaults and Marker Size Adjustment")

```

![](../Images/85c29c83350262fcfa325a74b090e876.png)

Python 有几个用于创建数据可视化的包和包生态系统；[点击这里阅读详细指南](https://dsaber.com/2016/10/02/a-dramatic-tour-through-pythons-data-visualization-landscape-including-ggplot-and-altair/)。Matplotlib 是其中最常用的工具。虽然没有人会因为制作 Matplotlib 插图而赢得“年度设计师”称号，但它非常适合可视化较小的数据集。同时，Matplotlib 并不适合快速可视化 10k 行数据，也不适合做很多偏离常规的操作。

要可视化大量数据，你可能需要查看一下[DataShader 生态系统](https://datashader.org/)。[Bokeh](https://bokeh.org/)非常适合交互式仪表板。对于 3D，你可以使用[Matplotlib 扩展（mplot3d）](https://matplotlib.org/mpl_toolkits/mplot3d/tutorial.html)，或者查看[Mayavi](https://docs.enthought.com/mayavi/mayavi/)。如果你想用相对较少的代码制作出色的可视化效果，可以尝试[Altair](https://altair-viz.github.io/getting_started/overview.html)。说真的，试试 Altair，它可能会改变你的生活。

在R语言的世界里，标准的绘图库包括[ggplot2](https://ggplot2.tidyverse.org/)和[lattice](http://lattice.r-forge.r-project.org/)。前者是一个通用绘图库，后者则可以方便地从同一个数据集中生成多个小图。你可以在这里找到一份全面的R[数据可视化包清单](https://www.computerworld.com/article/2921176/great-r-packages-for-data-import-wrangling-visualization.html)。查看基础R数据可视化时，很容易产生这样的想法。

### 结论

数据可视化是理解数据集的工具。虽然某些可视化可以变成艺术，但制作高质量的日常视觉效果的基本技能对于任何数据导向的人来说都是无价的。尽管通常没有可视化就不能得出宏大的结论，但知道如何操控、调整大小、着色以及数据元素的运动是我们都可以达成一致的。

**简介：** [SuperDataScience](https://www.superdatascience.com/)是一个面向数据科学家的在线学习平台，旨在帮助他们学习数据科学或提升职业生涯。我们使复杂变得简单！

**相关：**

+   [为什么数据可视化是数据分析师工具包中最重要的技能](https://www.kdnuggets.com/2019/08/simpliv-data-visualization-data-analyst.html)

+   [数据科学家的高级数据可视化简单方法](https://www.kdnuggets.com/2019/08/advanced-data-visualisation-data-scientists.html)

+   [如何选择可视化方式](https://www.kdnuggets.com/2019/06/how-choose-visualization.html)

### 更多相关话题

+   [KDnuggets™ 新闻 22:n01，1月5日：3种工具来跟踪和可视化…](https://www.kdnuggets.com/2022/n01.html)

+   [3种工具来跟踪和可视化你的Python代码执行](https://www.kdnuggets.com/2021/12/3-tools-track-visualize-execution-python-code.html)

+   [通过《快速Python数据科学》提升你的Python技能！](https://www.kdnuggets.com/2022/06/manning-step-python-game-fast-python-data-science.html)

+   [理解Python的迭代和成员资格：指南](https://www.kdnuggets.com/understanding-pythons-iteration-and-membership-a-guide-to-__contains__-and-__iter__-magic-methods)

+   [优化Python代码性能：深入探讨Python剖析器](https://www.kdnuggets.com/2023/02/optimizing-python-code-performance-deep-dive-python-profilers.html)

+   [Python Enum：如何在Python中构建枚举](https://www.kdnuggets.com/python-enum-how-to-build-enumerations-in-python)
