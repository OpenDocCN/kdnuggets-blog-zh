# 数据集整理的网页抓取，第1部分：收集手工啤酒数据

> 原文：[https://www.kdnuggets.com/2017/02/web-scraping-dataset-curation-part-1.html](https://www.kdnuggets.com/2017/02/web-scraping-dataset-curation-part-1.html)

**作者：Jean-Nicholas Hould，[JeanNicholasHould.com](http://JeanNicholasHould.com/?utm_source=kdnugget)。**

如果你读过我过去的一些文章，你现在知道我喜欢好的手工啤酒。我决定将工作与乐趣结合起来，编写一个关于如何用Python从网站抓取手工啤酒数据集的教程。

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的IT工作

* * *

本文分为两个部分：抓取和整理数据。在第一部分中，我们将规划和编写代码以从网站收集数据集。在第二部分中，我们将把“整洁数据”原则应用于这个新抓取的数据集。文章末尾，我们将拥有一个干净的手工啤酒数据集。

### 网页抓取

网页抓取器是一段代码，它会自动加载网页并提取特定数据。网页抓取器会执行一个重复的任务，这个任务如果由你手动完成会非常耗时。

例如，我们可以编写一个网络抓取器，从一个电子商务网站提取产品名称及其评分，并将其写入CSV文件中。

抓取网站是获取原本无法获得的新数据集的好方法。

### 几条关于抓取的规则

正如Greg Reda几年前在他的[出色的网页抓取教程](http://www.gregreda.com/2013/03/03/web-scraping-101-with-python/)中指出的那样，关于抓取你需要知道一些规则：

1.  尊重网站的条款和条件。

1.  不要给服务器带来压力。*一个抓取器可以在一秒钟内发出成千上万次网页请求。确保你不会给服务器施加过多压力。*

1.  你的抓取器代码将会失效。*网页经常变化。你的抓取器代码很快就会过时。*

### 规划

构建抓取器的第一步是规划阶段。显然，你需要决定你想要提取什么数据以及从哪个网站提取。

在我们的案例中，我们想要从一个名为[CraftCans](http://craftcans.com/db.php?search=all&sort=beerid&ord=desc&view=text)的网站提取数据。这个网站列出了2692种手工罐装啤酒。对于这个特定的数据集，我们不需要构建一个抓取器来提取数据。按照它的布局，我们可以很容易地将数据复制粘贴到Excel表格中。

![](../Images/df6135b546e71463baa458485cec59da.png)

对于每种啤酒，网站提供了一些详细信息：

+   名称

+   风格

+   尺寸

+   酒精浓度（ABV）

+   IBU（国际苦味单位）

+   酿造商名称

+   酿造商位置

**检查 HTML**

我们希望我们的抓取器为我们提取所有这些信息。为了给我们的抓取器提供具体指令，我们需要查看 CraftCans 网站的 HTML 代码。大多数现代浏览器提供了一种通过右键单击页面来检查网页 HTML 源代码的方法。

在 Google Chrome 上，你可以右键单击网页上的元素，然后点击“检查”以查看 HTML 代码。

![](../Images/eba7c3a77238c4f275dcdb502c0793b0.png)

**识别模式**

从主页面上的 HTML 代码来看，你可以看到这个大列表实际上是一个 HTML 表格。每种啤酒代表表格中的一行。通常，像 HTML 表格这样的重复模式非常适合网页抓取，因为逻辑简单明了。

![](../Images/0a729e08a7fa6263370d4df52fd81299.png)

### 使用的库

对于这个项目，我们将导入四个库。

**urlopen**

第一个`urlopen`将用于请求网页上的 HTML 页面并返回其内容。就这么简单。

**BeautifulSoup4**

第二个，`BeautifulSoup4`，是一个使在 HTML 文档中导航变得简单的库。例如，使用这个库你可以轻松选择 HTML 文档中的一个表格并遍历其行。

**pandas**

第三个是`pandas`。我们不会在抓取部分使用这个库。我们将使用它来整理数据。`pandas`是一个旨在简化数据操作和分析的库。

**用于正则表达式的 re**

最后，我们将使用`re`，它是 Python 标准库的一部分。这个库提供了正则表达式匹配操作。正则表达式是操纵字符串的方式。例如，我们可以使用正则表达式列出字符串中的所有数字。

### 编写代码

**HTML 的挑战**

在对 CraftCans 网页进行一些调查后，我意识到没有干净的方法来抓取 CraftCans 网站。

CraftCans 的 HTML 结构有些老派。整个页面布局都在表格中。这曾经是常见做法，但现在布局通常使用 CSS 设置。

此外，HTML 表格或包含啤酒条目的行上没有类或标识符。没有干净的 HTML 结构或标识符，定位到我们想要的特定表格是具有挑战性的。

**解决方案：列出所有表格行**

我找到的抓取网站的解决方案可能不是最干净的，但它有效。

由于包含数据的表格上没有标识符，我使用`BeautifulSoup4`的`findAll`函数加载 CraftCans 页面上所有的表格行`tr`。此函数返回一个全面的表格行列表，无论它们是否来自我们要抓取的表格。

对于每一行，我运行测试以确定它是否包含啤酒条目或其他内容。判断一行是否为啤酒数据条目的启发式方法很简单：该行需要包含八个单元格，并且第一个单元格必须包含有效的数字 ID。

现在我们已经有了判断一行是否确实为啤酒条目的函数，我们可以抓取整个网页。我们需要决定以何种格式存储从网站收集的数据。我希望每个 CraftCans 的啤酒条目都像这样的 JSON 文档。

**示例啤酒 JSON 条目**

我喜欢将数据存储在 JSON 文档中的原因是，我可以轻松地将其转换为 pandas `DataFrame`。

### 运行抓取器

函数编写完成后，我们可以使用 `urlopen` 请求 CraftCans 网页，并让代码处理其余部分。

有了 `get_all_beers` 返回的啤酒列表，我们可以轻松创建一个新的 `pandas`*DataFrame* 来方便地可视化和操作数据。

**简介：Jean-Nicholas Hould** 是来自 [加拿大蒙特利尔的数据科学家](http://jeannicholashould.com/?utm_source=kdnugget)。JeanNicholasHould.com 的作者。

[原文](http://www.jeannicholashould.com/python-web-scraping-tutorial-for-craft-beers.html)。经许可转载。

**相关：**

+   [在 Python 中整理数据](/2017/01/tidying-data-python.html)

+   [使用 SQL 进行统计分析](/2016/08/doing-statistics-sql.html)

+   [数据科学统计 101](/2016/07/data-science-statistics-101.html)

### 更多相关主题

+   [Python 网页抓取初学者指南](https://www.kdnuggets.com/2022/10/beginner-guide-web-scraping-python.html)

+   [使用 Python 和 Beautiful Soup 的逐步网页抓取指南](https://www.kdnuggets.com/2023/04/stepbystep-guide-web-scraping-python-beautiful-soup.html)

+   [使用 BeautifulSoup 掌握网页抓取](https://www.kdnuggets.com/mastering-web-scraping-with-beautifulsoup)

+   [Octoparse 8.5：赋能本地抓取及更多](https://www.kdnuggets.com/2022/02/octoparse-85-empowering-local-scraping.html)

+   [ChatGPT 驱动的数据探索：解锁数据集中的隐藏洞察](https://www.kdnuggets.com/2023/07/chatgptpowered-data-exploration-unlock-hidden-insights-dataset.html)

+   [如何在机器学习中正确选择来自庞大数据集的样本](https://www.kdnuggets.com/2019/05/sample-huge-dataset-machine-learning.html)
