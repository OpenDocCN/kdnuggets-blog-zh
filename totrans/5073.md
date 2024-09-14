# Python 可视化工具概述

> 原文：[https://www.kdnuggets.com/2015/11/overview-python-visualization-tools.html](https://www.kdnuggets.com/2015/11/overview-python-visualization-tools.html)

**作者 [Chris Moffitt](http://pbpython.com/author/chris-moffitt.html)**。

### 介绍

在 Python 世界里，有多种选项可以用于数据可视化。由于这种多样性，确定何时使用哪个工具可能非常具有挑战性。本文包含了一些较为流行的工具示例，并演示了如何使用它们创建简单的条形图。我将使用以下工具来创建数据绘图示例：

+   [Pandas](http://pandas.pydata.org/)

+   [Seaborn](http://stanford.edu/~mwaskom/software/seaborn/)

+   [ggplot](http://ggplot.yhathq.com/)

+   [Bokeh](http://bokeh.pydata.org/)

+   [pygal](http://pygal.org/)

+   [Plotly](https://plot.ly/)

在这些示例中，我将使用 pandas 来处理数据，并利用这些数据进行可视化。在大多数情况下，这些工具可以在没有 pandas 的情况下使用，但我认为 pandas 和可视化工具的组合非常常见，因此这是最好的起点。

### 那么 Matplotlib 呢？

[Matplotlib](http://matplotlib.org/) 是 Python 可视化包的“祖父”。它非常强大，但这种强大也带来了复杂性。通常，你可以使用 matplotlib 做任何你需要的事情，但这并不总是容易弄清楚。我不会详细讲解一个纯粹的 Matplotlib 示例，因为许多工具（尤其是 Pandas 和 Seaborn）都是 matplotlib 的简化封装。如果你想了解更多，我在我的 [简单绘图](http://pbpython.com/simple-graphing-pandas.html) 文章中讲解了几个示例。

我对 Matplotlib 最大的不满是，获取合理外观的图表需要花费太多工作。在尝试一些这些示例时，我发现无需大量代码就能更容易地获得漂亮的可视化。举一个 matplotlib 冗长特性的例子，看看这个 [ggplot 帖子](http://blog.yhathq.com/posts/ggplot-for-python.html)中的分面示例。

### 方法论

对于本文的方法，我有一个快速说明。我确信一旦人们开始阅读，就会指出更好的使用这些工具的方法。我的目标不是在每个示例中创建完全相同的图表。我希望在每个示例中以大致相同的方式可视化数据，并花费大致相同的时间来研究解决方案。

在这个过程中，我面临的最大挑战是格式化 x 轴和 y 轴，并且在某些大标签的情况下让数据看起来合理。还花了一些时间搞清楚每个工具如何要求数据格式。一旦弄清楚了这些部分，其余的相对简单。

另一个需要考虑的点是，条形图可能是最简单的一种图表类型。这些工具允许你创建更多类型的图表。我的示例更侧重于格式化的简便性，而非创新的可视化示例。此外，由于标签的原因，一些图表占用了很多空间，因此我自行裁剪了一些图表——只是为了保持文章长度的可控性。最后，我调整了图像的大小，因此任何模糊现象都是缩放问题，而不是实际输出质量的问题。

最后，我从尝试使用另一种工具来代替 Excel 的思路来处理这个问题。我认为我的示例更适合在报告、演示文稿、电子邮件或静态网页上展示。如果你正在评估用于实时数据可视化或通过其他机制共享的工具，那么其中一些工具提供了更多的功能，这些我没有深入探讨。

### 数据集

[上一篇文章](http://pbpython.com/web-scraping-mn-budget.html) 描述了我们将使用的数据。我将抓取示例深入了一层，并确定了每个类别中的详细支出项目。此数据集包含 125 个条目，但我选择只展示前 10 个，以保持简单。你可以在[这里](http://pbpython.com/extras/mn-budget-detail-2014.csv)找到完整的数据集。

### Pandas

我使用 pandas DataFrame 作为所有图表的起点。幸运的是，pandas 提供了一个内置的绘图功能，这是一层在 matplotlib 之上的工具。我将以此为基础。首先，导入我们的模块并将数据读入预算 DataFrame。我们还需要对数据进行排序，并将其限制为前 10 项。

`budget = pd.read_csv("mn-budget-detail-2014.csv") budget = budget.sort('amount',ascending=False)[:10]`

我们将使用相同的预算行进行所有示例。以下是前 5 项的样子：

|  | 类别 | 详细 | 金额 |
| --- | --- | --- | --- |
| 46 | 行政部门 | 议会大厦翻修与恢复续篇 | 126300000 |
| 1 | 明尼苏达大学 | 明尼阿波利斯；泰特实验室翻修 | 56700000 |
| 78 | 人力服务 | 明尼苏达安全医院 – 圣彼得 | 56317000 |
| 0 | 明尼苏达大学 | 高等教育资产保护与更换… | 42500000 |
| 5 | 明尼苏达州立大学及大学 | 高等教育资产保护与更换… | 42500000 |

现在，设置我们的显示以使用更好的默认值并创建条形图：

`pd.options.display.mpl_style = 'default' budget_plot = budget.plot(kind="bar",x=budget["detail"], title="MN 资本预算 - 2014", legend=False)`

这将利用“详细”列创建图表，同时显示标题并移除图例。以下是保存图像为 png 格式所需的额外代码。

`fig = budget_plot.get_figure() fig.savefig("2014-mn-capital-budget.png")`

这里是其样子（截断以保持文章长度适中）：

![Pandas image](../Images/9a85d90b9d744494b5e25628696021c3.png)

基本效果看起来非常好。理想情况下，我希望对 y 轴进行更多的格式化，但这需要进行一些 matplotlib 的操作。这是一个完全可以使用的可视化，但仅通过 pandas 很难进行更多的自定义。

### Seaborn

[Seaborn](http://stanford.edu/~mwaskom/software/seaborn/) 是一个基于 matplotlib 的可视化库。它旨在使默认的数据可视化更加美观，并且目标是简化更复杂的图表的创建。它与 pandas 也有很好的集成。我的例子没有使 seaborn 显著地展现其优势。我喜欢 seaborn 的一个特点是其内置的各种样式，这使得你可以快速更改颜色调色板，使其看起来更加漂亮。否则，seaborn 在这个简单的图表中对我们帮助不大。标准导入和数据读取：

`import pandas as pd import seaborn as sns import matplotlib.pyplot as plt`

`budget = pd.read_csv("mn-budget-detail-2014.csv") budget = budget.sort('amount',ascending=False)[:>10]`

我发现一个问题是，我必须明确设置 x 轴上项目的顺序，使用 `x_order`。这段代码设置了顺序，并对图表和条形图的颜色进行了样式设置：

`sns.set_style("darkgrid") bar_plot = sns.barplot(x=budget["detail"],y=budget["amount"], palette="muted", x_order=budget["detail"].tolist()) plt.xticks(rotation=>90) plt.show()`

![Pandas image](../Images/21b021e7e23ea41286f0a1bdd838c967.png)

如你所见，我不得不使用 matplotlib 旋转 x 轴标题，以便能够实际读取它们。在视觉上，显示效果很好。理想情况下，我希望格式化 y 轴上的刻度，但我无法在不使用 matplotlib 的 `plt.yticks` 的情况下做到这一点。

### ggplot

[ggplot](http://ggplot.yhathq.com/) 与 Seaborn 类似，因为它也是基于 matplotlib 并且旨在以简单的方式改善 matplotlib 可视化的视觉效果。它与 seaborn 的不同之处在于，它是 R 的 ggplot2 的移植版。鉴于这一目标，部分 API 的设计不符合 Python 的习惯，但它是非常强大的。我没有在 R 中使用过 ggplot，因此有一定的学习曲线。然而，我开始看到 ggplot 的吸引力。这个库正在积极开发中，我希望它能继续成长和成熟，因为我认为它可能成为一个非常强大的选项。在学习过程中，我确实遇到过几次解决不了的问题。经过查看代码和一些谷歌搜索后，我能够解决大部分问题。继续导入并读取我们的数据：

`import pandas as pd from ggplot import *`

`budget = pd.read_csv("mn-budget-detail-2014.csv") budget = budget.sort('amount',ascending=False)[:>10]`

现在我们通过将多个 ggplot 命令串联在一起来构建图表：

`p = ggplot(budget, aes(x="detail",y="amount")) + \ geom_bar(stat="bar", labels=budget["detail"].tolist()) +\ ggtitle("MN Capital Budget - 2014") + \ xlab("Spending Detail") + \ ylab("Amount") + scale_y_continuous(labels='millions') + \ theme(axis_text_x=element_text(angle=>90)) print p`

这似乎有点奇怪——特别是使用`print p`来显示图表。然而，我发现弄清楚这点相对简单。确实花了一些时间弄明白如何将文本旋转90度，以及如何排列x轴上的标签。我发现的最酷的功能是`scale_y_continous`，它使标签显示得更加美观。如果你想保存图像，使用`ggsave`很简单：

`ggsave(p, "mn-budget-capital-ggplot.png")`

这是最终的图像。我知道这是一大片灰度。我本可以给它上色，但没有花时间去做。

![Pandas 图像](../Images/07a6a2614bd0dd12f908e56d78acb594.png)

* * *

## 我们的前三课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织IT

* * *

### 更多相关主题

+   [机器学习的数据标注：市场概览、方法和工具](https://www.kdnuggets.com/2021/12/data-labeling-ml-overview-and-tools.html)

+   [2023年前十名开源数据科学工具的比较概述](https://www.kdnuggets.com/a-comparative-overview-of-the-top-10-open-source-data-science-tools-in-2023)

+   [5种SQL可视化工具推荐给数据工程师](https://www.kdnuggets.com/2023/02/5-sql-visualization-tools-data-engineers.html)

+   [文本摘要方法概述](https://www.kdnuggets.com/2019/01/approaches-text-summarization-overview.html)

+   [逻辑回归概述](https://www.kdnuggets.com/2022/02/overview-logistic-regression.html)

+   [水银概述：创建数据科学作品集和…](https://www.kdnuggets.com/2022/05/overview-mercury-creating-data-science-portfolio-notebook-based-webapps.html)
