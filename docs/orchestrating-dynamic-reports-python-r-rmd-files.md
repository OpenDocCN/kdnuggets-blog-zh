# 用 Rmd 文件在 Python 和 R 中编排动态报告

> 原文：[https://www.kdnuggets.com/2019/11/orchestrating-dynamic-reports-python-r-rmd-files.html](https://www.kdnuggets.com/2019/11/orchestrating-dynamic-reports-python-r-rmd-files.html)

[评论](#comments)

**由 [Marija Ilic](https://www.linkedin.com/in/marija-ilić-65b8a53)，数据分析师/科学家**

![图像](../Images/5c2258e25bcfe24c68809d146abdad7d.png)

### **Python 和 R 互相嵌套**

几个用 Python 和 R 编写的支持包允许分析师将 Python 和 R 结合在一个 Python 或 R 脚本中。熟悉 R 的人可以使用 [reticulate](https://rstudio.github.io/reticulate/) 包在 R 中调用 Python 代码。然后，一个 R 脚本可以在 Python 和 R 之间互操作（Python 对象被转换为 R 对象，反之亦然）。然而，如果你使用 Python 但希望使用 R 的一些功能，考虑使用用 Python 编写的 [rpy2](https://rpy2.readthedocs.io/en/version_2.8.x/introduction.html) 包来启用嵌入的 R 代码。

[R markdown](https://rmarkdown.rstudio.com/)，一个将代码和结果合并到一个输出中的流行框架，提供了优雅的 Python 和 R 集成。我们将创建一个动态报告，将两种语言结合在一个 Rmd 脚本中。我们将使用 [外汇交易数据](https://en.wikipedia.org/wiki/Foreign_exchange_market) 来捕捉 15 分钟间隔的价格变动，然后绘制交易分析师在定价模型中使用的烛台图（[OHLC](https://www.investopedia.com/terms/o/ohlcchart.asp) 图表）。

### **在 R Markdown 文档中运行 R 和 Python 代码**

R markdown，或 Rmd，是一个包含文本或评论（结合文本格式）和由 ```py. From a file, inside R or R Studio, you can create and render useful reports in output formats like HTML, pdf, or word. However, the primary benefit is that source code, outputs, and comments are contained in one file, facilitating easy collaboration among your team.

Even R lovers may not know that Rmd files can contain Python chunks. More conveniently, objects are shared between the environments, allowing programmers to call objects in Python and R in the opposing language.

### **R Markdown with Python**

Let’s examine how to use Python in Rmd. First, ensure Python is installed on your computer and all Python libraries or modules you’re planning to use in Rmd are installed (pip works and [virtual environments](https://www.lukaskawerau.com/rmarkdown-with-python-and-virtual-envs/) can be utilized, if preferable).

In Rmd files, Python code chunks are similar to R chunks: Python code is placed inside marks: ```{python} 和 ```py.

Here’s a simple R markdown with embedded Python code:

![](../Images/c3be788ab84d38fdb1fd17dbd3ef6028.png)

In the example above the csv is loaded with the help of the pandas library, a column is renamed, and the first rows are printed. In the file heading, the report is defined with ### and a single author comment is printed. Here’s the result when we run the Rmd:

![](../Images/5d86bbd6c4c669b245ea6a44398a5591.png)

Beside the code and output, the heading and author comment prints. Now that the data has been loaded using Python, it can be used inside R:

The R code starts with ```{r} 包围的 R 代码块的文本文件，文件以 ``` 结束。代码后面跟着一个 Python 代码块和在 R 代码中引用的对象。在我们的示例中，R 对象在 reticulate 包的帮助下从 Python 对象转换过来。命令 py$data 检索在 Python 中创建的对象并将其转换为 R 数据框。现在，当创建了 R 数据框后，它可以在 R 代码中进一步使用。

输出效果如下：

![](../Images/e79ee6da0e91603cbfa1355b16d54d05.png)

现在我们将继续使用 R 并创建一个交易者常用的可视化：烛台图。以下是使用 [plotly](https://plot.ly/r/candlestick-charts/) 库编写的 [Candlestick](https://en.wikipedia.org/wiki/Candlestick_chart) 图表的 R 代码：

这应该显示如下内容：

![](../Images/44785a783d26575e45f404bf58bc22d9.png)

这个简单的例子演示了如何将 Python 和 R 用于报告创建。流行的 Python pandas 库用于加载和数据准备。然后，R 被用于可视化。

可以创建一个 R 数据对象，然后在 Python 环境中引用。以下是一个示例，其中使用 Python mpl_finance 模块创建了一个可视化：

就这些了！现在你可以选择使用哪种语言，或者让你的团队使用他们偏好的语言进行协作。

### **开始使用 Rmd**

R 和 Python 课程在多个流行平台上均可用（例如：Coursera、Udemy、Vertabelo Academy、Data Camp）。许多课程也涵盖了数据可视化概念。R 和 Python 都是数据科学的优秀工具，可以同时使用。如果你有兴趣开始学习，可以考虑这些：

1.  在 Coursera 上，有一个很棒的关于可重复研究和 R markdown 基础的课程：

    [https://www.coursera.org/lecture/reproducible-research/r-markdown-5NzHN](https://www.coursera.org/lecture/reproducible-research/r-markdown-5NzHN)

1.  如果你对 R 不太熟悉，Data Camp 提供了一个很好的入门课程：

    [https://www.datacamp.com/courses/free-introduction-to-r](https://www.datacamp.com/courses/free-introduction-to-r)

1.  作为 Vertabelo Academy 的作者，我个人推荐我们的 Python 和 R 课程。它们特别适合那些来自商业背景的人士：

    [https://academy.vertabelo.com/course/python-data-science](https://academy.vertabelo.com/course/python-data-science)

    [https://academy.vertabelo.com/course/data-visualization-101](https://academy.vertabelo.com/course/data-visualization-101)

1.  Edx 提供了许多 Python 和 R 课程，包括来自哈佛、IBM、微软的课程。对于 Python 初学者，可以尝试这个 IBM 课程：[https://www.edx.org/course/python-basics-for-data-science-2](https://www.edx.org/course/python-basics-for-data-science-2)

### **总结**

Python 和 R 是当前最热门的数据科学语言。熟悉这两种语言是很有好处的，因为项目可能需要每种语言的不同方面。有许多包可以帮助将两者集成到使用场景中。一个是 R markdown，一种用于在 R 中创建动态文档的文件格式。Rmd 文件无缝集成了可执行代码和评论。在 reticulate 包的帮助下，可以轻松地在 Python 中访问 R 对象，反之亦然。分析师现在无需在 Python 和 R 之间做出选择——可以在一个文件中集成两者。

**简介： [Marija Ilic](https://www.linkedin.com/in/marija-ilić-65b8a53)** 是一名数据分析师/科学家。她喜欢分析大量数据。Marija 在银行业的 DWH/ETL 开发方面有着丰富的背景。

**相关：**

+   [应用于 Pandas DataFrames 的集合操作](/2019/11/set-operations-applied-pandas-dataframes.html)

+   [你可能不知道的 R 中的十个随机有用的东西](/2019/07/ten-more-random-useful-things-r.html)

+   [R 用户的客户细分](/2019/09/customer-segmentation-r-users.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全领域。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

### 主题拓展

+   [如何利用数据可视化提升工作报告的影响力](https://www.kdnuggets.com/2022/08/data-visualization-add-impact-work-reports-presentations.html)

+   [最先进的深度学习下的可解释预测与实时预测](https://www.kdnuggets.com/2021/12/sota-explainable-forecasting-and-nowcasting.html)

+   [使用Python的Pathlib组织、搜索和备份文件](https://www.kdnuggets.com/organize-search-and-back-up-files-with-pythons-pathlib)

+   [处理CSV文件的3种方法](https://www.kdnuggets.com/2022/10/3-ways-process-csv-files-python.html)

+   [如何在Bash中管理文件和目录](https://www.kdnuggets.com/how-to-manage-files-and-directories-in-bash)

+   [停止在数据科学项目中硬编码 - 改用配置文件](https://www.kdnuggets.com/2023/06/stop-hard-coding-data-science-project-config-files-instead.html)
