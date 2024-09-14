# 如何轻松分析你的时间序列

> 原文：[https://www.kdnuggets.com/2020/03/painlessly-analyze-time-series.html](https://www.kdnuggets.com/2020/03/painlessly-analyze-time-series.html)

[评论](#comments)

**作者 [Andrew Van Benschoten](https://www.linkedin.com/in/andrewhvbs/)，Matrix Profile Foundation 联合创始人**

我们周围充满了时间序列数据。从金融到物联网，再到营销，许多组织产生了数以千计的这些指标，并对其进行挖掘以揭示商业关键洞察。一个网站可靠性工程师可能会监控来自服务器农场的数十万条时间序列流，希望检测异常事件并防止灾难性故障。另一方面，一家实体零售商可能关心识别客户流量的模式，并利用这些模式来指导库存决策。

识别异常事件（或称“discords”）和重复模式（“motifs”）是两个基本的时间序列任务。那么，如何开始呢？针对这两个问题有数十种方法，每种方法都有其独特的优点和缺陷。

此外，时间序列数据分析 notoriously 困难，而数据科学社区的爆炸性增长带来了对更多“黑箱”自动化解决方案的需求，这些解决方案可以被具有广泛技术背景的开发人员所利用。

![图](../Images/346db31b632fe4278a72c26744bdae53.png)

如何用一张图分析你的时间序列（图片来源：Matrix Profile Foundation）

我们在 Matrix Profile Foundation 相信，问题的答案其实很简单。虽然说没有免费的午餐，但 Matrix Profile（一个由 [Keogh 研究组](https://www.cs.ucr.edu/~eamonn/MatrixProfile.html) 在 UC-Riverside 开发的数据结构和相关算法集合）是一个强大的工具，可以帮助解决异常检测和模式发现这两个双重问题。Matrix Profile 具有稳健性、可扩展性，并且基本无需参数：我们已经看到它在包括网站用户数据、订单量和其他商业关键应用在内的广泛指标中发挥了作用。

如下文所述，Matrix Profile Foundation 已经在三种最常见的数据科学语言（Python、R 和 Golang）中实现了 Matrix Profile，作为一个易于使用的 API，适合时间序列的新手和专家。

### 那么，Matrix Profile 是什么呢？

Matrix Profile 的基本概念很简单：如果我拿一段数据并将其沿着时间序列滑动，那么它在每个新位置上的重叠程度如何？更具体地说，我们可以评估一个子序列与每个相同长度的时间序列段之间的欧几里得距离，从而构建所谓的片段“距离配置文件”。

如果数据中子序列重复，将至少有一个完美匹配，最小欧几里得距离将为零（或在噪声存在的情况下接近零）。相比之下，如果子序列非常独特（例如包含显著的异常值），匹配将很差，所有重叠分数都将很高。请注意，数据类型并不重要：我们只关心一般模式的保留。

然后，我们将每一个可能的片段滑过时间序列，建立起距离剖析的集合。通过取每个时间步长上所有距离剖析的最小值，我们可以建立最终的矩阵剖析。注意，矩阵剖析值谱的两个端点都是有用的。高值表示不常见的模式或异常事件；相反，低值突出可重复的模式，并为你的时间序列提供有价值的洞察。

![图像](../Images/7913adf0d7c793e66eb67d40e2678cc1.png)

图片来源：矩阵剖析基金会

对于感兴趣的读者，[我们的一位联合创始人撰写的这篇文章](https://towardsdatascience.com/introduction-to-matrix-profiles-5568f3375d90)提供了对矩阵剖析的更深入讨论。

尽管矩阵剖析（Matrix Profile）在时间序列分析中可以成为改变游戏规则的工具，但利用它生成洞察力是一个多步骤的计算过程，每一步都需要一定的领域经验。然而，我们相信，数据科学中最强大的突破发生在复杂的事物变得易于接触时。关于矩阵剖析，易于接触有三个方面：“开箱即用”的实现，温和的核心概念介绍，能够自然引导深入探索，以及多语言的可访问性。

今天，我们很自豪地推出矩阵剖析 API（MPA），这是一个用 R、Python 和 Golang 编写的通用代码库，实现了所有这三个目标。

### MPA：它的工作原理

使用矩阵剖析包括三个步骤。

首先，你需要计算矩阵剖析本身。然而，这还不是终点：你需要利用你创建的矩阵剖析来发现一些东西。你是想寻找重复的模式？还是发现异常事件？最后，关键是你需要可视化你的发现，因为时间序列分析在一定程度上的可视化检查中获益匪浅。

通常，你需要阅读大量文档（包括学术和技术）来了解如何执行这三个步骤中的每一个。如果你是一个对矩阵剖析有先前知识的专家，这可能不是问题，但我们发现许多用户只是希望通过突破方法论来分析他们的数据，以获得一个基本的起点。代码是否可以简单地利用一些合理的默认值来生成合理的输出？

为了与这种自然的计算流程相并行，MPA 包含三个核心组件：

1.  计算（计算矩阵剖析）

1.  发现（评估MP以寻找模式、异异常等）

1.  可视化（通过基本图表展示结果）

由于 Matrix Profile 仅有几年的历史，我们预期其会持续发展，并计划相应地更新该库；我们热忱欢迎社区反馈和开发协助！我们还希望通过不断增加的 Jupyter Notebook 教程来教育数据科学社区这一技术。开源工具构成了现代分析的基础，我们希望这一实现能像对我们一样有效地服务于其他用户。

这三种功能被封装成一个称为 Analyze 的高级功能。这是一个用户友好的界面，使得不了解 Matrix Profile 内部工作的用户也能快速利用它来处理自己的数据。随着用户对 MPA 的经验和直觉的增加，他们可以轻松深入了解这三个核心组件，以实现进一步的功能提升。

### MPA：一个玩具示例

作为示例，我们将使用 [MPA 的 Python 版本](https://github.com/matrix-profile-foundation/matrixprofile) 来分析下面显示的合成时间序列：

```py` ``` from matplotlib import pyplot as plt  import numpy as np  import matrixprofile as mp    %matplotlib inline    data = pd.read_csv('rawdata.csv')  vals = data.data.values    fig,ax = plt.subplots(figsize=(20,10))  ax.plot(np.arange(len(vals)),vals, label = '测试数据') ```py ```` ![图像](../Images/077d379d2c747a12f13d32d204985afa.png)

图片来源：Matrix Profile Foundation

视觉检查显示存在模式和不一致。然而，一个直接的问题是你选择的子序列长度将改变模式的数量和位置！在索引 0–500 之间只有两个正弦模式，还是每个周期都是该模式的一个实例？让我们看看 MPA 如何处理这个挑战：

```py` ``` profile, figures = mp.analyze(vals) ```py ```` ![图像](../Images/346db31b632fe4278a72c26744bdae53.png)

图片来源：Matrix Profile Foundation

因为我们没有指定任何关于子序列长度的信息，`analyze` 通过利用一种称为全矩阵配置文件（或 PMP）的强大计算来生成帮助我们评估不同子序列长度的见解。我们将在后续文章中讨论 PMP 的细节（或者你可以阅读相关论文），但简而言之，它是将所有可能的子序列长度的全局计算浓缩成一个单一的视觉摘要。X 轴是矩阵配置文件的索引，Y 轴是对应的子序列长度。阴影越深，那个点的欧几里得距离越小。我们可以使用三角形的“峰值”来直观地找到合成时间序列中存在的 6 个“大”模式。

PMP 本身很不错，但我们承诺提供一种简单的方式来理解你的时间序列。为此，`analyze` 将结合 PMP 和底层算法，从所有可能的窗口大小中选择合适的模式和异常值。`analyze` 创建的附加图表展示了前三个模式和前三个异常值，以及对应的窗口大小和在 Matrix Profile 中的位置（并且，扩展到你的时间序列）。

![图示](../Images/86955e87759c0cd37366f19d7341a02d.png)

图片来源：Matrix Profile Foundation![图示](../Images/2ba3b61bb0ac6e537d74e9707b8dfcf3.png)

图片来源：Matrix Profile Foundation![图示](../Images/de8f7a67b135280283a5af0c6e092540.png)

图片来源：Matrix Profile Foundation![图示](../Images/9d8450c40c54daf033e1bceaa20777e1.png)

图片来源：Matrix Profile Foundation![图示](../Images/45342715c29e4ab33523f1584281561a.png)

图片来源：Matrix Profile Foundation

不出所料，默认设置下的信息量很大。我们的目标是让这个核心功能调用能够作为你未来许多分析的起点。例如，PMP 表明我们的时间序列中存在一个长度约为 175 的保守模式。尝试对该子序列长度调用 `analyze`，看看会发生什么！

```py` ``` profile, figures = mp.analyze(vals,windows=175) ```py ````

### 总结

我们希望 MPA 能使你更轻松地分析时间序列！欲了解更多信息，请访问我们的 [网站](https://matrixprofile.org/)、[GitHub 仓库](https://github.com/matrix-profile-foundation) 或关注我们的 [Twitter](https://twitter.com/matrixprofile)。如果你发现代码有用，请给我们的 GitHub 仓库加星！MPF 还运营一个 Discord 频道，你可以在这里与其他 Matrix Profile 用户互动并提问。祝你在时间序列分析中好运！

**致谢**

感谢 Tyler Marrs、Frankie Cancino、Francisco Bischoff、Austin Ouyang 和 Jack Green 对本文的审阅及创作协助。最重要的是，感谢 Eamonn Keogh、Abdullah Mueen 及他们众多的研究生，感谢他们创建了 Matrix Profile 并持续推动其发展。

**补充信息**

1.  Matrix Profile 研究论文可以在 Eamonn Keogh 的 UCR 页面找到：[https://www.cs.ucr.edu/~eamonn/MatrixProfile.html](https://www.cs.ucr.edu/~eamonn/MatrixProfile.html)

1.  Matrix Profile 算法的 Python 实现可以在这里找到：[https://github.com/matrix-profile-foundation/matrixprofile](https://github.com/matrix-profile-foundation/matrixprofile)

1.  Matrix Profile 算法的 R 实现可以在这里找到：[https://github.com/matrix-profile-foundation/tsmp](https://github.com/matrix-profile-foundation/tsmp)

1.  Matrix Profile 算法的 Golang 实现可以在这里找到：[https://github.com/matrix-profile-foundation/go-matrixprofile](https://github.com/matrix-profile-foundation/go-matrixprofile)

**简历：[Andrew Van Benschoten](https://www.linkedin.com/in/andrewhvbs/)** 是一位数据科学领袖，拥有利用复杂信息生成有价值洞察的良好记录。擅长人员和项目管理，并且能够有效地向跨学科的利益相关者传达结果。

[原文](https://matrixprofile.org/posts/how-to-painlessly-analyze-your-time-series/)。经许可转载。

**相关：**

+   [时间序列分类：合成与真实金融时间序列](/2020/03/time-series-classification-synthetic-real-financial-time-series.html)

+   [利用 R、SQL 和 Tableau 进行地理时间序列预测介绍](/2020/02/introduction-geographical-time-series-crime-r-sql-tableau.html)

+   [预测故事：这是季节性还是其他？](/2020/03/forecasting-stories-seasonality.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT。

* * *

### 更多相关内容

+   [拖拽、放置、分析：无代码数据科学的崛起](https://www.kdnuggets.com/drag-drop-analyze-the-rise-of-nocode-data-science)

+   [市场数据和新闻：时间序列分析](https://www.kdnuggets.com/2022/06/market-data-news-time-series-analysis.html)

+   [使用 AI 和分析引擎更快准备时间序列数据](https://www.kdnuggets.com/2021/12/piexchange-faster-way-prepare-timeseries-data-ai-analytics-engine.html)

+   [为多变量时间序列构建一个可处理的特征工程管道…](https://www.kdnuggets.com/2022/03/building-tractable-feature-engineering-pipeline-multivariate-time-series.html)

+   [使用 Ploomber、Arima、Python 和 Slurm 进行时间序列预测](https://www.kdnuggets.com/2022/03/time-series-forecasting-ploomber-arima-python-slurm.html)

+   [使用开源工具生成合成时间序列数据](https://www.kdnuggets.com/2022/06/generate-synthetic-timeseries-data-opensource-tools.html)
