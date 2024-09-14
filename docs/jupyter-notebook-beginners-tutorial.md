# Jupyter Notebook 初学者教程

> 原文：[https://www.kdnuggets.com/2018/05/jupyter-notebook-beginners-tutorial.html/2](https://www.kdnuggets.com/2018/05/jupyter-notebook-beginners-tutorial.html/2)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](/2018/05/jupyter-notebook-beginners-tutorial.html?page=2#comments)

### 内核

每个笔记本的背后都运行着一个内核。当你运行一个代码单元格时，该代码在内核中执行，任何输出会返回到单元格中显示。内核的状态会随着时间的推移和单元格之间持续存在——它涉及整个文档而不是单个单元格。

例如，如果你在一个单元格中导入库或声明变量，它们将在另一个单元格中可用。通过这种方式，你可以把笔记本文件看作有点类似于脚本文件，只不过它是多媒体的。让我们试一下以感受一下。首先，我们将导入一个 Python 包并定义一个函数。

```py
import numpy as np

def square(x):
    return x * x

```

一旦我们执行了上面的单元格，我们可以在任何其他单元格中引用`np`和`square`。

```py
x = np.random.randint(1, 10)
y = square(x)

print('%d squared is %d' % (x, y))

```

```py
1 squared is 1

```

这将无论笔记本中的单元格顺序如何都有效。你可以自己尝试一下，让我们再次打印出我们的变量。

```py
print('Is %d squared is %d?' % (x, y))

```

```py
Is 1 squared is 1?

```

这里没有什么意外！但现在让我们改变`y`。

```py
y = 10

```

如果我们再次运行包含`print`语句的单元格，你认为会发生什么？我们会得到输出`Is 4 squared is 10?`！

大多数情况下，你的笔记本流程将是自上而下的，但通常会返回进行更改。在这种情况下，单元格左侧标示的执行顺序，比如`In [6]`，会告诉你是否有任何单元格的输出过时。如果你想重置所有内容，可以从内核菜单中选择几个非常有用的选项：

+   重新启动：重新启动内核，从而清除所有已定义的变量等。

+   重新启动并清除输出：与上面相同，但还会清除你代码单元格下方显示的输出。

+   重新启动并运行所有：与上面相同，但还会按顺序从第一个到最后一个运行所有单元格。

如果你的内核在计算中卡住了，并且你希望停止它，你可以选择中断选项。

**选择内核**

你可能已经注意到，Jupyter 允许你更改内核，实际上有很多不同的选项可供选择。当你从仪表板创建一个新笔记本时，选择一个 Python 版本，实际上是选择了要使用的内核。

不仅有针对不同版本的 Python 的内核，还有针对[100 多种语言](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels)的内核，包括 Java、C，甚至 Fortran。数据科学家可能特别感兴趣于[R](https://irkernel.github.io/)和[Julia](https://github.com/JuliaLang/IJulia.jl)的内核，以及[imatlab](https://github.com/imatlab/imatlab)和[Calysto MATLAB 内核](https://github.com/calysto/matlab_kernel)的内核。 [SoS 内核](https://github.com/vatlab/SOS)提供了在单个笔记本中支持多种语言的功能。每个内核都有自己的安装说明，但可能需要你在计算机上运行一些命令。

### 示例分析

现在我们已经了解了*什么是* Jupyter Notebook，是时候看看*如何*在实践中使用它们了，这将帮助你更清楚地理解*为什么*它们如此受欢迎。现在终于可以开始使用之前提到的《财富500强》数据集了。记住，我们的目标是了解美国最大公司历史上利润的变化情况。

值得注意的是，每个人都会形成自己的偏好和风格，但一般原则仍然适用。如果你愿意，可以在自己的笔记本中跟随本节内容，这样你可以进行一些实验。

### 命名你的笔记本

在你开始编写项目之前，你可能会想给它一个有意义的名字。也许有点令人困惑的是，你不能直接在笔记本应用中命名或重命名笔记本，而是必须使用仪表板或文件浏览器来重命名`.ipynb`文件。我们将返回仪表板来重命名你之前创建的文件，它将有一个默认的笔记本文件名`Untitled.ipynb`。

你不能在笔记本运行时重命名它，所以你需要先将其关闭。最简单的方法是从笔记本菜单中选择“文件 > 关闭并停止”。然而，你也可以通过在笔记本应用中选择“内核 > 关闭”来关闭内核，或者通过在仪表板中选择笔记本并点击“关闭”（见下图）来关闭内核。

![正在运行的笔记本](../Images/beb3b6fedbb2c81625553af42f03e64e.png)

然后，你可以选择你的笔记本，并点击仪表板控制中的“重命名”。

![正在运行的笔记本](../Images/90719bbcf4b73a92d1a4b6cf21b8ff48.png)

请注意，关闭浏览器中的笔记本标签页**不会**像在传统应用中关闭文档那样“关闭”笔记本。笔记本的内核将继续在后台运行，需要在真正“关闭”之前手动关闭——不过如果你不小心关闭了标签页或浏览器，这一点非常方便！如果内核已经关闭，你可以关闭标签页而无需担心是否仍在运行。

一旦你给笔记本命名，重新打开它，我们就可以开始了。

### 设置

通常，首先会有一个专门用于导入和设置的代码单元，这样如果你选择添加或更改任何内容，你可以简单地编辑并重新运行该单元，而不会造成任何副作用。

```py
%matplotlib inline

import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

sns.set(style="darkgrid")

```

我们导入了[pandas](https://pandas.pydata.org/)以处理数据， [Matplotlib](https://matplotlib.org/)以绘制图表， 和[Seaborn](https://seaborn.pydata.org/)以使图表更加美观。 通常也会导入[NumPy](http://www.numpy.org/)但在这种情况下，虽然我们通过pandas使用它，但不需要显式导入。而且那第一行不是Python命令，而是使用了称为行魔法的东西来指示Jupyter捕获Matplotlib图表并在单元格输出中呈现；这是一系列高级功能中的一个，超出了本文的范围。

让我们继续加载数据。

```py
df = pd.read_csv('fortune500.csv')

```

这样做也是明智的，以防我们需要在任何时候重新加载它。

### 保存和检查点

现在我们已经开始，最好的做法是定期保存。按`Ctrl + S`将通过调用“保存和检查点”命令来保存你的笔记本，但这个检查点是什么呢？

每次你创建一个新的笔记本时，都会创建一个检查点文件以及你的笔记本文件；它将位于你保存位置的一个名为`.ipynb_checkpoints`的隐藏子目录中，并且也是一个`.ipynb`文件。默认情况下，Jupyter会每120秒自动保存你的笔记本到这个检查点文件中，而不会更改你的主要笔记本文件。当你“保存和检查点”时，笔记本和检查点文件都会更新。因此，检查点使你能够在遇到意外问题时恢复未保存的工作。你可以通过菜单中的“文件 > 恢复到检查点”来恢复到检查点。

### 调查我们的数据集

现在我们真的开始了！我们的笔记本已经安全保存，我们已经将数据集`df`加载到最常用的pandas数据结构中，它叫做`DataFrame`，基本上看起来像一个表格。我们的数据集看起来怎么样呢？

```py
df.head()

```

|  | 年份 | 排名 | 公司 | 收入（百万） | 利润（百万） |
| --- | --- | --- | --- | --- | --- |
| 0 | 1955 | 1 | 通用汽车 | 9823.5 | 806 |
| 1 | 1955 | 2 | 埃克森美孚 | 5661.4 | 584.8 |
| 2 | 1955 | 3 | 美国钢铁公司 | 3250.4 | 195.4 |
| 3 | 1955 | 4 | 通用电气 | 2959.1 | 212.6 |
| 4 | 1955 | 5 | Esmark | 2510.8 | 19.1 |

```py
df.tail()

```

|  | 年份 | 排名 | 公司 | 收入（百万） | 利润（百万） |
| --- | --- | --- | --- | --- | --- |
| 25495 | 2005 | 496 | Wm. Wrigley Jr. | 3648.6 | 493 |
| 25496 | 2005 | 497 | 皮博迪能源公司 | 3631.6 | 175.4 |
| 25497 | 2005 | 498 | 温迪国际 | 3630.4 | 57.8 |
| 25498 | 2005 | 499 | Kindred Healthcare | 3616.6 | 70.6 |
| 25499 | 2005 | 500 | 辛辛那提金融公司 | 3614.0 | 584 |

看起来不错。我们有了需要的列，每一行都对应一个公司在一个特定年份的数据。

我们只是重命名这些列，以便以后可以引用它们。

```py
df.columns = ['year', 'rank', 'company', 'revenue', 'profit']

```

接下来，我们需要探索一下我们的数据集。数据集是否完整？pandas 是否按预期读取了数据？是否有缺失值？

```py
len(df)

```

```py
25500

```

好的，这看起来不错——从1955年到2005年，每年有500行数据。

让我们检查一下我们的数据集是否已按预期导入。一个简单的检查方法是查看数据类型（或 dtypes）是否被正确解释。

```py
df.dtypes

```

```py
year         int64
rank         int64
company     object
revenue    float64
profit      object
dtype: object

```

哎呀。看起来利润列有问题——我们希望它像收入列一样是`float64`类型。这表明它可能包含一些非整数值，因此让我们来看看。

```py
non_numberic_profits = df.profit.str.contains('[^0-9.-]')
df.loc[non_numberic_profits].head()

```

|  | 年份 | 排名 | 公司 | 收入 | 利润 |
| --- | --- | --- | --- | --- | --- |
| 228 | 1955 | 229 | Norton | 135.0 | N.A. |
| 290 | 1955 | 291 | Schlitz Brewing | 100.0 | N.A. |
| 294 | 1955 | 295 | Pacific Vegetable Oil | 97.9 | N.A. |
| 296 | 1955 | 297 | Liebmann Breweries | 96.0 | N.A. |
| 352 | 1955 | 353 | Minneapolis-Moline | 77.4 | N.A. |

正如我们所怀疑的那样！一些值是字符串，用于表示缺失数据。是否还有其他值混入了？

```py
set(df.profit[non_numberic_profits])

```

```py
{'N.A.'}

```

这使得解释变得容易，但我们应该怎么做呢？这取决于缺失值的数量。

```py
len(df.profit[non_numberic_profits])

```

```py
369

```

这是我们数据集的一小部分，但并不是完全无关紧要，因为它仍然约占1.5%。如果包含`N.A.`的行大致上在各个年份中均匀分布，最简单的解决方案就是删除这些行。所以让我们快速查看一下分布情况。

```py
bin_sizes, _, _ = plt.hist(df.year[non_numberic_profits], bins=range(1955, 2006))

```

![缺失值分布](../Images/9a7b7cae1a0e30a28f5faaa379801552.png)

一眼看去，我们可以看到单一年份中无效值的数量不到25，而每年有500个数据点，删除这些值在最坏的情况下占数据的不到4%。实际上，除了90年代的激增，大多数年份的缺失值数量不到峰值的一半。就我们的目的而言，我们可以认为这是可以接受的，接着删除这些行。

```py
df = df.loc[~non_numberic_profits]
df.profit = df.profit.apply(pd.to_numeric)

```

我们应该检查一下是否成功。

```py
len(df)

```

```py
25131

```

```py
df.dtypes

```

```py
year         int64
rank         int64
company     object
revenue    float64
profit     float64
dtype: object

```

太棒了！我们已经完成了数据集的设置。

如果你打算将你的笔记本作为报告呈现，你可以删除我们创建的调查性单元格，这些单元格在这里作为工作流演示，合并相关单元格（有关更多信息，请参见下面的高级功能部分）以创建一个单一的数据集设置单元格。这意味着如果我们在其他地方搞砸了数据集，我们只需重新运行设置单元格即可恢复。

### 使用 matplotlib 绘图

接下来，我们可以通过绘制每年的平均利润来解决手头的问题。我们也可以绘制收入，所以首先我们可以定义一些变量和方法来简化我们的代码。

```py
group_by_year = df.loc[:, ['year', 'revenue', 'profit']].groupby('year')
avgs = group_by_year.mean()
x = avgs.index
y1 = avgs.profit

def plot(x, y, ax, title, y_label):
    ax.set_title(title)
    ax.set_ylabel(y_label)
    ax.plot(x, y)
    ax.margins(x=0, y=0)

```

现在让我们绘图吧！

```py
fig, ax = plt.subplots()
plot(x, y1, ax, 'Increase in mean Fortune 500 company profits from 1955 to 2005', 'Profit (millions)')

```

![1955年至2005年《财富》500强公司利润的增长](../Images/c6061d92ec4e570f1803e0a2db5bc8f4.png)

哇，这看起来像是一个指数曲线，但有一些巨大的波动。它们一定对应于[1990年代初期的衰退](https://en.wikipedia.org/wiki/Early_1990s_recession)和[互联网泡沫](https://en.wikipedia.org/wiki/Dot-com_bubble)。在数据中看到这些现象相当有趣。但为什么每次衰退后，利润会恢复到更高的水平呢？

也许收入能告诉我们更多信息。

```py
y2 = avgs.revenue
fig, ax = plt.subplots()
plot(x, y2, ax, 'Increase in mean Fortune 500 company revenues from 1955 to 2005', 'Revenue (millions)')

```

![1955年至2005年财富500强公司收入的增加](../Images/44f82f195a35df4340d486dc1dd59663.png)

这为故事增添了另一面。收入并没有受到如此严重的打击，这得归功于财务部门的出色会计工作。

在[Stack Overflow](https://stackoverflow.com/a/47582329/604687)的帮助下，我们可以在这些图上叠加其标准差的正负范围。

```py
def plot_with_std(x, y, stds, ax, title, y_label):
    ax.fill_between(x, y - stds, y + stds, alpha=0.2)
    plot(x, y, ax, title, y_label)

fig, (ax1, ax2) = plt.subplots(ncols=2)
title = 'Increase in mean and std Fortune 500 company %s from 1955 to 2005'
stds1 = group_by_year.std().profit.as_matrix()
stds2 = group_by_year.std().revenue.as_matrix()
plot_with_std(x, y1.as_matrix(), stds1, ax1, title % 'profits', 'Profit (millions)')
plot_with_std(x, y2.as_matrix(), stds2, ax2, title % 'revenues', 'Revenue (millions)')
fig.set_size_inches(14, 4)
fig.tight_layout()

```

![jupyter-notebook-tutorial_48_0](../Images/963bf0012aa11cc05a286b3550d46f63.png)

这令人吃惊，标准差非常大。一些财富500强公司赚取数十亿，而其他公司则亏损数十亿，多年来风险随着利润的增长而增加。也许有些公司表现优于其他公司；排名前10%的公司利润是否比排名后10%的公司更稳定？

接下来我们还有许多问题可以探讨，而且很容易看出在笔记本中工作的流程如何与个人的思维过程相匹配，所以现在是时候结束这个例子了。这种流程帮助我们在一个地方轻松调查数据集，而无需在应用程序之间切换上下文，并且我们的工作可以立即共享和复现。如果我们希望为特定的受众创建更简洁的报告，我们可以通过合并单元格和删除中间代码来快速重构我们的工作。

### 共享你的笔记本

当人们谈论共享他们的笔记本时，他们通常考虑两种模式。大多数情况下，个人分享的是他们工作的最终结果，就像这篇文章本身一样，这意味着分享非交互式的、预渲染版本的笔记本；然而，也可以借助版本控制系统如[Git](https://git-scm.com/)进行协作。

也就是说，网上出现了一些新兴的[公司](https://kyso.io/)，提供在云端运行交互式Jupyter笔记本的能力。

### 在分享之前

共享笔记本将以你导出或保存时的状态出现，包括任何代码单元格的输出。因此，为了确保你的笔记本在共享时准备好，你应该在分享之前采取一些步骤：

1.  点击“单元格 > 全部输出 > 清除”

1.  点击“内核 > 重新启动并运行全部”

1.  等待你的代码单元格执行完毕，并检查它们是否按预期完成

这将确保你的笔记本不包含中间输出，状态不过时，并且在分享时按照顺序执行。

### 导出你的笔记本

Jupyter内置了导出到HTML和PDF以及其他几种格式的支持，你可以从“文件 > 另存为”菜单中找到这些选项。如果你希望将笔记本分享给一个小的私人组，这个功能可能已经足够。事实上，由于许多学术机构的研究人员会有一些公共或内部的网页空间，并且因为你可以将笔记本导出为HTML文件，Jupyter笔记本可以是他们与同行分享研究结果的特别便捷的方式。

但如果分享导出的文件对你来说不够直接，还有一些非常流行的方法可以更直接地在网上分享`.ipynb`文件。

### GitHub

到2018年初，GitHub上的[公开笔记本数量](https://github.com/parente/nbestimate)超过180万，这无疑是分享Jupyter项目给全世界的最受欢迎的独立平台。GitHub已经集成了对`.ipynb`文件的直接渲染支持，无论是在其网站上的代码库还是gists。如果你还不知情，[GitHub](https://github.com/)是一个用于版本控制和协作的代码托管平台，支持使用[Git](https://git-scm.com/)创建的代码库。你需要一个账户来使用他们的服务，但标准账户是免费的。

一旦你有了GitHub账户，最简单的在GitHub上分享笔记本的方式其实根本不需要Git。自2008年以来，GitHub提供了其Gist服务，用于托管和分享代码片段，每个片段都有自己的仓库。要使用Gists分享笔记本：

1.  登录并浏览到[gist.github.com](https://gist.github.com/)。

1.  在文本编辑器中打开你的`.ipynb`文件，选择全部并复制其中的JSON内容。

1.  将笔记本的JSON粘贴到gist中。

1.  给你的Gist起一个文件名，记得添加`.iypnb`，否则无法使用。

1.  点击“创建秘密gist”或“创建公开gist”。

这应该看起来像下面这样：

![创建Gist](../Images/d52a64eb4a582f70fdebcd1036e8850b.png)

如果你创建了一个公开的Gist，你现在可以与任何人分享其URL，其他人也可以[fork和克隆](https://help.github.com/articles/forking-and-cloning-gists/)你的工作。

创建你自己的Git仓库并在GitHub上分享超出了本教程的范围，但[GitHub提供了大量的指南](https://guides.github.com/)来帮助你自行入门。

对于使用git的用户，一个额外的提示是为那些Jupyter创建的隐藏`.ipynb_checkpoints`目录在你的`.gitignore`中[添加一个例外](https://stackoverflow.com/q/35916658/604687)，以避免不必要地将检查点文件提交到你的代码库。

### Nbviewer

到2015年，NBViewer已经每天渲染[成千上万](https://blog.jupyter.org/rendering-notebooks-on-github-f7ac8736d686)的笔记本，成为网络上最受欢迎的笔记本渲染器。如果你已经有地方来在线托管你的Jupyter笔记本，无论是GitHub还是其他地方，NBViewer会渲染你的笔记本，并提供一个可分享的URL。作为Project Jupyter的一部分，它作为免费服务提供，网址是[nbviewer.jupyter.org](https://nbviewer.jupyter.org/)。

最初在GitHub的Jupyter Notebook集成之前开发的，NBViewer允许任何人输入URL、Gist ID或GitHub用户名/仓库/文件，它将把笔记本渲染为网页。Gist的ID是其URL末尾的唯一数字；例如，在`https://gist.github.com/username/50896401c23e0bf417e89cd57e89e1de`中最后一个反斜杠后的字符。如果你输入GitHub用户名或用户名/仓库，你将看到一个最简文件浏览器，让你浏览用户的仓库及其内容。

NBViewer在显示笔记本时显示的URL是一个基于正在渲染的笔记本的URL的常量，因此你可以与任何人分享，只要原始文件保持在线，链接就会有效——NBViewer不会长时间缓存文件。

### 终结思考

从基础知识入手，我们已经掌握了Jupyter笔记本的自然工作流程，深入探讨了IPython的高级功能，最后学会了如何与朋友、同事和全世界分享我们的工作。所有这些都是在笔记本本身完成的！

应该清楚笔记本如何通过减少上下文切换和模拟项目中的自然思维发展来促进高效工作体验。Jupyter笔记本的强大功能也应该显而易见，我们提供了许多线索，以便你开始探索在自己的项目中更高级的功能。

如果你希望获取更多关于自己笔记本的灵感，Jupyter已经整理了[a gallery of interesting Jupyter Notebooks](https://github.com/jupyter/jupyter/wiki/A-gallery-of-interesting-Jupyter-Notebooks)，你可能会发现有帮助，此外，[Nbviewer主页](https://nbviewer.jupyter.org/)链接到一些真正精美的高质量笔记本示例。也查看一下我们的[Jupyter Notebook技巧](https://www.dataquest.io/blog/jupyter-notebook-tips-tricks-shortcuts/)列表。

> *想了解更多关于Jupyter笔记本的内容？我们有一个[a guided project](https://www.dataquest.io/m/207/guided-project%3A-using-jupyter-notebook)你可能会感兴趣。*

**个人简介: [Benjamin Pryke](https://twitter.com/BenjaminPryke)** 是一名Python和网页开发人员，拥有计算机科学和机器学习背景。FinTech公司Machina Capital的联合创始人。兼职体操运动员和数字波希米亚。

[原文](https://www.dataquest.io/blog/jupyter-notebook-tutorial/?utm_source=kdnuggets&utm_medium=blog)。转载已获许可。

**相关:**

+   [前 5 名最佳 Jupyter Notebook 扩展](/2018/03/top-5-best-jupyter-notebook-extensions.html)

+   [了解机器学习的 5 件事](/2018/03/5-things-know-about-machine-learning.html)

+   [Fast.ai 第 1 课在 Google Colab 上（免费 GPU）](/2018/02/fast-ai-lesson-1-google-colab-free-gpu.html)

* * *

## 我们的 3 个顶级课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

### 更多相关主题

+   [如何在 Jupyter Notebook 中设置 Julia](https://www.kdnuggets.com/2022/11/setup-julia-jupyter-notebook.html)

+   [数据科学家必备的 10 个 Jupyter Notebook 小贴士和技巧](https://www.kdnuggets.com/2023/06/10-jupyter-notebook-tips-tricks-data-scientists.html)

+   [Jupyter Notebook 魔法方法速查表](https://www.kdnuggets.com/jupyter-notebook-magic-methods-cheat-sheet)

+   [金融中的 Python：Jupyter Notebook 中的实时数据流](https://www.kdnuggets.com/python-in-finance-real-time-data-streaming-within-jupyter-notebook)

+   [5 个免费的 Jupyter Notebook 数据科学项目模板](https://www.kdnuggets.com/5-free-templates-for-data-science-projects-on-jupyter-notebook)

+   [如何编写高效的 Python 代码：初学者教程](https://www.kdnuggets.com/how-to-write-efficient-python-code-a-tutorial-for-beginners)
