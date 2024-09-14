# 如何通过网络抓取构建足球数据集

> 原文：[https://www.kdnuggets.com/2020/11/build-football-dataset-web-scraping.html](https://www.kdnuggets.com/2020/11/build-football-dataset-web-scraping.html)

[评论](#comments)

**由 [Otávio Simões Silveira](https://www.linkedin.com/in/otavioss28/)，经济学家，志向成为数据科学家**

![图像](../Images/be7c4298589981b19ff9ac0c107a06fe.png)

由 [Bermix Studio](https://unsplash.com/@bermixstudio?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上提供的照片

当使用 Python 的库如 *BeautifulSoup*、*requests* 或 *urllib* 抓取网站时，常常会遇到无法访问网站某些部分的问题。这是因为这些部分是在客户端生成的，使用 JavaScript，而这些库无法处理。

为了解决这个问题，使用 [Selenium](https://selenium-python.readthedocs.io/index.html) 可以是一个有趣的选择。Selenium 通过打开一个自动化浏览器来工作，然后它能够访问整个内容并与页面交互。

本文将涵盖使用 Selenium 抓取 JavaScript 渲染内容的过程，以英超联赛网站为例，并抓取 2019/20 赛季每场比赛的统计数据。

### 了解网站

英超联赛网站通过其非常直接的 URL 使得抓取多个比赛变得非常简单。比赛的 URL 基本上由“https://www.premierleague.com/match/”后跟一个唯一的比赛 ID 组成。

每个 ID 由一个数字组成，每个赛季的所有比赛 ID 都是按顺序排列的。例如，整个 2019/20 赛季的 ID 从 46605 到 46984。然后我们需要做的就是遍历这个区间并收集每场比赛的数据。

在本文中，我们将使用利物浦 5 比 3 战胜切尔西作为例子。这场比赛的 ID 是 46968。你可以在“premierleague.com/match/”后输入这个 ID 以访问页面，这样你可以跟随文章中描述的抓取过程。必要时请随时参考该页面。

### 正在抓取…

为了开始编写代码，我们将进行导入并初始化两个空列表，一个用于处理错误，稍后将在文章中解释，另一个用于存储我们抓取的每场比赛的数据。

在循环中，将使用比赛 ID 创建 URL，将实例化 *driver* 对象，并设置 Selenium。这里不会使用高级配置。`option.headless = True` 行表示我们不希望实际看到浏览器打开并访问网站以收集数据。完成这些操作后，我们将使用 *driver* 对象来获取页面。

现在我们准备开始抓取数据。我们将首先收集比赛的日期和涉及的团队。我们还将使用 Datetime 将日期格式从“Wed 22 Jul 2020”转换为 07/22/2020。

每个元素是通过其 Xpath 查找的，但也可以通过名称、类、标签等查找。请查看所有选择器[这里](https://selenium-python.readthedocs.io/locating-elements.html)。

请注意，我们在收集比赛日期时不得不使用*WebDriverWait*和`expected_conditions`。这是因为这是页面中通过 JavaScript 生成的部分，因此我们需要等待元素渲染完毕，以避免引发错误。

如果我们尝试仅使用*requests*和*BeautifulSoup*来收集比赛日期，我们将无法访问这些信息，因为*BeautifulSoup*无法解析 JavaScript 渲染的内容。

为了抓取最终比分，我们首先需要从我称之为比分框的区域中获取文本，该文本返回“5–3”，然后分配主队得分和客队得分。

下一步是获取比赛的统计数据。这些数据是页面上统计标签下的一个表格。我们可以简单地使用 Pandas 的`read_html`函数读取页面源代码，但页面的这一部分仅在我们点击选项卡后才会渲染。

那么，首先要做的是找到选项卡元素，并用 Selenium 单击它。之后，我们可以使用`read_html`函数。此函数返回一个包含页面上所有表格的列表，存储为 DataFrame。然后我们选择列表中的最后一个元素，即我们所需的元素。抓取完成后，我们可以退出驱动程序。

### 错误处理

有时 Selenium 可能会不太稳定，加载页面所需的时间可能会很长。这可能会引发一些错误，因为我们在抓取数百个页面。

为了处理这个问题，我们需要使用 try 和 except 子句。如果在收集数据时发生错误，代码将把比赛 ID 添加到错误列表中，并在不崩溃的情况下继续处理下一场比赛。当所有抓取完成后，你可以轻松查看这个列表，以便仅抓取缺失的比赛。这是所有这些的代码：

### 操作统计数据

目前统计数据的 DataFrame 看起来是这样的：

```py
 Liverpool       Unnamed: 1  Chelsea
0          50     Possession %       50
1           7  Shots on target        5
2          10            Shots       10
3         749          Touches      752
4         584           Passes      575
5          19          Tackles        9
6          14       Clearances       15
7           6          Corners        0
8           0         Offsides        3
9           1     Yellow cards        0
10          8   Fouls conceded       11
```

由于我们需要将所有这些数据存储在 DataFrame 的一行中，这种格式并不好。为了解决这个问题，我们将创建两个字典，每个字典对应一个球队，其中每个键都代表一个统计数据。整个过程如下：

### 使数据一致

请注意，我们在统计数据 DataFrame 中没有红牌统计数据。这是因为这场比赛中没有红牌。当没有统计数据出现时，网站不会显示这些统计数据。

如果这没有修复，一些行将比其他行长，数据将不一致。为了解决这个问题，我们将使用一个包含所有预期统计数据的列表，如果该列表中的任何值不是统计数据字典的键（我们只需检查其中一个字典），那么该统计数据将作为键添加到两个字典中，值为零。

现在剩下的工作是创建一个新的列表，包含为这场比赛抓取的所有内容，并将这个列表附加到包含所有比赛的*赛季*列表中。

当我们完成对赛季所有比赛的抓取后，我们可以将赛季的列表列表转换为 DataFrame，并将数据导出为 *.csv* 文件。*stats_check* 列表被用来创建一个列表，用于命名 DataFrames 的列。

你可以在 [这里](https://github.com/otavio-s-s/data_science/tree/master/Premier%20League%20Scraping) 查看完整的代码。

### 总结

最后，这就是抓取的数据：

![图片](../Images/ef53bd9eafda232575c9ba59d8e9074c.png)

图片由作者提供

380 场比赛。这是整个英超 2019/20 赛季的数据集！你还可以做更多的事情：如果使用 ID 1，你可以回到 1992/93 赛季。但 ID 从 1992 年到今天并不是线性的，因为在某个时点，ID 开始涵盖杯赛、青少年比赛和女子比赛等。

不过，如果你想要一个包含数千场比赛的数据集，可以在 [这里](https://github.com/otavio-s-s/data_science/blob/master/Premier%20League%20Scraping/PL_ids.csv) 找到几乎所有自 2011/12 赛季以来的英超比赛 ID。

如果你打算这样做，确保在代码中插入更多的暂停，使用 WebDriverWait 或甚至 sleep 函数，以避免因对网站发起过多请求而被封锁 IP。另一个可能的解决方案是联系代理服务提供商，如 [Infatica](https://infatica.io/)，他们能够提供更好的 IP 地址基础设施，以保持你的代码正常运行。

如果想更进一步，你总是可以抓取更多关于每场比赛的数据。只需再增加几行代码，你的数据集中就可以包含裁判、比赛场地和城市、观众人数、半场比分、进球者、首发阵容等信息，甚至更多！

继续抓取数据！

希望你喜欢这篇文章，并且它能在某种程度上对你有所帮助。如果你有任何问题、建议，或只是想保持联系，请随时通过 [Twitter](https://twitter.com/_otavioss)、[GitHub](https://github.com/otavio-s-s) 或 [Linkedin](https://www.linkedin.com/in/otavioss28/) 联系我。

**个人简介: [Otávio Simões Silveira](https://www.linkedin.com/in/otavioss28/)** 是一名经济学家和有抱负的数据科学家。

[原文](https://medium.com/evolve-you/how-to-build-a-football-dataset-with-web-scraping-d4deffcaa9ca)。经授权转载。

**相关内容:**

+   [Python, Selenium & Google 用于地理编码自动化: 免费与付费](/2019/11/automate-geocoding-free-paid-python-selenium-google.html)

+   [用任务调度程序自动化你的 Python 脚本: 使用 Windows 任务调度程序抓取替代数据](/2019/09/automate-python-scripts-task-scheduler.html)

+   [创建真实数据科学项目组合的逐步指南](/2020/10/guide-authentic-data-science-portfolio-project.html)

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT 需求

* * *

### 更多相关主题

+   [Python 网页抓取初学者指南](https://www.kdnuggets.com/2022/10/beginner-guide-web-scraping-python.html)

+   [使用 Python 和 Beautiful Soup 进行网页抓取的逐步指南](https://www.kdnuggets.com/2023/04/stepbystep-guide-web-scraping-python-beautiful-soup.html)

+   [掌握使用 BeautifulSoup 进行网页抓取](https://www.kdnuggets.com/mastering-web-scraping-with-beautifulsoup)

+   [如何使用 Python 和机器学习预测足球比赛赢家](https://www.kdnuggets.com/2023/01/python-machine-learning-predict-football-match-winners.html)

+   [在 5 分钟内构建一个机器学习网页应用](https://www.kdnuggets.com/2022/03/build-machine-learning-web-app-5-minutes.html)

+   [在 5 分钟内使用 Python 构建网页抓取器](https://www.kdnuggets.com/2022/02/build-web-scraper-python-5-minutes.html)
