# 10个简单技巧加速你在Python中的数据分析

> 原文：[https://www.kdnuggets.com/2019/07/10-simple-hacks-speed-data-analysis-python.html](https://www.kdnuggets.com/2019/07/10-simple-hacks-speed-data-analysis-python.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由[Parul Pandey](https://www.linkedin.com/in/parul-pandey-a5498975/)，数据科学爱好者**

![图示](../Images/fd6d112577937ab23fd45ef60708771e.png)

[来源](https://pixabay.com/images/id-2123970/)

提示和技巧，尤其是在编程世界中，非常有用。有时一个小技巧既能节省时间又能拯救生命。一个小小的快捷方式或附加功能有时可以成为上天的恩赐，并能显著提升生产力。所以，这里有一些我用过并汇编在这篇文章中的我最喜欢的提示和技巧。有些可能比较常见，有些可能较新，但我相信下次你做数据分析项目时，它们会非常有用。

### 1\. 对pandas数据框进行分析

**Profiling** 是一个帮助我们理解数据的过程，[**Pandas Profiling**](https://github.com/pandas-profiling/pandas-profiling) 是一个完全实现这一点的Python包。它是进行Pandas数据框探索性数据分析的简单而快速的方法。pandas的`df.describe()`和`df.info()functions`通常作为EDA过程的第一步。然而，它仅提供数据的非常基本概述，在大型数据集的情况下帮助不大。另一方面，Pandas Profiling函数通过`df.profile_report()`扩展了pandas DataFrame，实现了快速数据分析。它用一行代码显示大量信息，并且是一个交互式HTML报告。

对于给定的数据集，pandas profiling包计算以下统计数据：

![图示](../Images/ecad56550efebb2dc30e3909e2bb1bb0.png)

Pandas Profiling包计算的统计数据。

**安装**

```py
pip install pandas-profiling
or
conda install -c anaconda pandas-profiling
```

**使用**

让我们使用经典的泰坦尼克号数据集来演示这个多功能的Python分析器的能力。

```py
#importing the necessary packages
import pandas as pd
import pandas_profiling

# Depreciated: pre 2.0.0 version
df = pd.read_csv('titanic/train.csv')
pandas_profiling.ProfileReport(df)
```

编辑：在这篇文章发布后一周，Pandas-Profiling推出了一个重大升级 - 版本2.0.0。语法有所变化，实际上功能已包含在pandas中，报告也变得更加全面。以下是最新的使用语法：

**使用**

要在Jupyter笔记本中显示报告，请运行：

```py
#Pandas-Profiling 2.0.0
df.profile_report()
```

这行代码就是你需要在Jupyter笔记本中显示数据分析报告的全部。报告非常详细，包括必要的图表。

![](../Images/888ec5d1b8d79beed629d4f3b31f4964.png)

报告还可以导出为**交互式HTML文件**，代码如下。

```py
profile = df.profile_report(title='Pandas Profiling Report')
profile.to_file(outputfile="Titanic data profiling.html")
```

![](../Images/77cd5dac107d830fa64b1e36488a7840.png)

请参阅[文档](https://pandas-profiling.github.io/pandas-profiling/docs/)以获取更多详细信息和示例。

### 2\. 为pandas图表添加交互性

**Pandas**有一个内置的`.plot()`函数作为DataFrame类的一部分。然而，使用该函数生成的可视化图表不具备交互性，这使得它们的吸引力降低。相反，`pandas.DataFrame.plot()`函数绘制图表的便利性也不可忽视。如果我们能在不对代码进行重大修改的情况下，使用pandas绘制类似于plotly的交互式图表，那会怎么样呢？实际上，你可以借助[**Cufflinks**](https://github.com/santosjorge/cufflinks)库来实现这一点**。**

Cufflinks库将[**plotly**](https://www.plot.ly/)的强大功能与[pandas](http://pandas.pydata.org/)的灵活性结合起来，方便绘图。现在，让我们看看如何安装这个库并在pandas中使用它。

**安装**

```py
pip install plotly # Plotly is a pre-requisite before installing cufflinks
pip install cufflinks
```

**用法**

```py
#importing Pandas 
import pandas as pd
#importing plotly and cufflinks in offline mode
import cufflinks as cf

import plotly.offline
cf.go_offline()
cf.set_config_file(offline=False, world_readable=True)
```

现在是时候让魔法在Titanic数据集中展开了。

```py
df.iplot()
```

![图示](../Images/35469750ef51f65125c84e1eb17fbde7.png)![图示](../Images/6fb0c52d10aa70787737d7598b62dd47.png)

XXX

**df.iplot() 与 df.plot()**

右侧的可视化展示了静态图表，而左侧的图表则是交互式的，并且更加详细，所有这些都没有对语法进行任何重大更改。

[**点击这里**](https://github.com/santosjorge/cufflinks/blob/master/Cufflinks%20Tutorial%20-%20Pandas%20Like.ipynb)获取更多示例。

### 3\. 一点魔法

**魔法命令**是Jupyter Notebooks中的一组便利函数，旨在解决标准数据分析中的一些常见问题。你可以使用`%lsmagic`查看所有可用的魔法命令。

![图示](../Images/bb157a69505a727331e7366042d0f033.png)

所有可用魔法函数的列表

魔法命令有两种类型：***行魔法***，由单个`%`字符前缀，并在单行输入上操作；和***单元格魔法***，由双`%%`前缀标识，并在多行输入上操作。如果设置为1，魔法函数可以在不输入初始%符号的情况下调用。

让我们看看一些可能在常见数据分析任务中有用的功能：

+   **% pastebin**

%pastebin 将代码上传到[Pastebin](https://en.wikipedia.org/wiki/Pastebin)并返回网址。Pastebin是一个在线内容托管服务，我们可以在这里存储纯文本如源代码片段，然后将网址分享给他人。实际上，Github gist也类似于**pastebin**，尽管有版本控制。

考虑一个名为`file.py`的Python脚本，内容如下：

```py
#file.py
def foo(x):
    return x
```

在Jupyter Notebook中使用**%pastebin**会生成一个pastebin网址。

![](../Images/0bca3617a36ba8b44974f606609fe544.png)

+   **%matplotlib notebook**

` %matplotlib inline` 函数用于在Jupyter notebook中渲染静态matplotlib图表。尝试将`inline`部分替换为`notebook`，以便获得可缩放和可调整大小的图表。确保在导入matplotlib库之前调用该函数。

![](../Images/e30c44c157cb07da81eb12798ac203e4.png)

**%matplotlib inline 与 %matplotlib notebook**

+   **%run**

` %run` 函数在笔记本中运行 Python 脚本。

```py
%run file.py
```

+   **%%writefile**

`%%writefile` 将单元格的内容写入文件。这里的代码将被写入名为 **foo.py** 的文件，并保存在当前目录中。

![](../Images/ab331460839793d1854a8a282d532412.png)

+   **%%latex**

`%%latex` 函数将单元格内容渲染为 LaTeX。这对于在单元格中书写数学公式和方程非常有用。

![](../Images/9523100bf882a6a7340a0a3dbbc4c4df.png)

### 4\. 查找和消除错误

**交互式调试器** 也是一个魔法函数，但我将其分为一个独立的类别。如果在运行代码单元时遇到异常，请在新行中输入 ` %debug` 并运行。这将打开一个交互式调试环境，将你带到发生异常的位置。你还可以检查程序中分配的变量的值，并在这里执行操作。要退出调试器，请按 `q`。

![](../Images/24de249f6095d5a6ecbcbc1f052e60c1.png)

### 5\. 打印也可以很漂亮

如果你想生成美观的数据结构表示， [**pprint**](https://docs.python.org/2/library/pprint.html) 是首选模块。它在打印字典或 JSON 数据时特别有用。让我们来看一个使用 `print` 和 `pprint` 显示输出的示例。

![](../Images/c36d818c9468452dd36ae1d2361ac7c5.png)

![](../Images/581e71757069ecf7f135bb2ee7d558f3.png)

### 6\. 使提示框突出显示

我们可以在 Jupyter Notebooks 中使用警告/提示框来突出显示重要内容或需要引起注意的事项。提示框的颜色取决于指定的警告类型。只需在需要突出显示的单元格中添加以下任何或所有代码。

+   **蓝色警告框：信息**

```py
<div class="alert alert-block alert-info">
<b>Tip:</b> Use blue boxes (alert-info) for tips and notes. 
If it’s a note, you don’t have to include the word “Note”.
</div>
```

![](../Images/d080781aae3d7cf705c4071a03469d8a.png)

+   **黄色警告框：警告**

```py
<div class="alert alert-block alert-warning">
<b>Example:</b> Yellow Boxes are generally used to include additional examples or mathematical formulas.
</div>
```

![](../Images/eefcb6bfb4fbbb7da2ae310aa31054a5.png)

+   **绿色警告框：成功**

```py
<div class="alert alert-block alert-success">
Use green box only when necessary like to display links to related content.
</div>
```

![](../Images/153e8056a7699e171ab93196f8b03755.png)

+   **红色警告框：危险**

```py
<div class="alert alert-block alert-danger">
It is good to avoid red boxes but can be used to alert users to not delete some important part of code etc. 
</div>
```

![](../Images/b5b69b493eebb5ea48fc5abacbf6d805.png)

### 7\. 打印单元格的所有输出

设想一个包含以下代码行的 Jupyter Notebook 单元格：

```py
In  [1]: 10+5          
         11+6
```

```py
Out [1]: 17
```

这是一个单元格的正常属性，只有最后一个输出会被打印，而对于其他输出，我们需要添加 `print()` 函数。实际上，我们只需在笔记本顶部添加以下代码片段，就可以打印所有输出。

```py
from IPython.core.interactiveshell import InteractiveShell  InteractiveShell.ast_node_interactivity = "all"
```

现在所有输出会一个接一个地打印出来。

```py
In  [1]: 10+5          
         11+6
         12+7
```

```py
Out [1]: 15
Out [1]: 17
Out [1]: 19
```

要恢复到原始设置：

```py
InteractiveShell.ast_node_interactivity = "last_expr"
```

### 8\. 使用 'i' 选项运行 Python 脚本

从命令行运行 Python 脚本的典型方法是：`python hello.py`。然而，如果在运行相同脚本时添加 ` -i `，例如 `python -i hello.py`，则会提供更多的优势。让我们看看如何。

+   首先，一旦程序结束，Python 不会退出解释器。因此，我们可以检查变量的值和程序中定义的函数的正确性。

![](../Images/33e2cac27f652736f64b43036e92ab3e.png)

+   其次，我们可以轻松调用 Python 调试器，因为我们仍然在解释器中：

```py
import pdb
pdb.pm()
```

这将把我们带到发生异常的位置，然后我们可以进一步处理代码。

![图片](../Images/82537e7c997adffd53bbede48ac08569.png)

*原始的*[*来源*](http://www.bnikolic.co.uk/blog/python-running-cline.html)*。

### 9\. 自动注释代码

`Ctrl/Cmd + /` 会自动将所选行注释掉。再次按下组合键将取消注释该行代码。

![](../Images/33a4882d98f017683aa1128f5fcd85b2.png)

### 10\. 删除是人的本能，恢复是神圣的

你是否曾在 Jupyter Notebook 中不小心删除了一个单元格？如果是，那么这里有一个快捷键可以撤销该删除操作。

+   如果你删除了一个单元格的内容，可以通过按 `CTRL/CMD+Z` 容易地恢复它。

+   如果需要恢复整个删除的单元格，可以按 `ESC+Z` 或 `EDIT > Undo Delete Cells`

![](../Images/121ab7f8c18f4fdea8ee2f6afc370c82.png)

### 结论

在这篇文章中，我列出了在使用 Python 和 Jupyter Notebooks 时总结的主要技巧。我相信它们会对你有帮助，你也会从中获得一些收获。祝编程愉快！

**个人简介: [Parul Pandey](https://www.linkedin.com/in/parul-pandey-a5498975/)** 是一位数据科学爱好者，常为数据科学相关出版物如 Towards Data Science 撰写文章。

[原文](https://towardsdatascience.com/10-simple-hacks-to-speed-up-your-data-analysis-in-python-ec18c6396e6b)。经许可转载。

**相关内容：**

+   [使用‘What-If Tool’调查机器学习模型](/2019/06/using-what-if-tool-investigate-machine-learning-models.html)

+   [使用 Matplotlib 制作动画](/2019/05/animations-with-matplotlib.html)

+   [PyViz：简化 Python 中的数据可视化过程](/2019/06/pyviz-data-visualisation-python.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

### 更多相关话题

+   [加速 Python 代码的 3 种简单方法](https://www.kdnuggets.com/2022/10/3-simple-ways-speed-python-code.html)

+   [RAPIDS cuDF 加速您的下一个数据科学工作流程](https://www.kdnuggets.com/2023/04/rapids-cudf-speed-next-data-science-workflow.html)

+   [提高数据效率和速度的5个Python技巧](https://www.kdnuggets.com/5-python-tips-for-data-efficiency-and-speed)

+   [如何使用索引加速SQL查询【Python版】](https://www.kdnuggets.com/2023/08/speed-sql-queries-indexes-python-edition.html)

+   [如何通过缓存加速Python代码](https://www.kdnuggets.com/how-to-speed-up-python-code-with-caching)

+   [如何将Python Pandas的速度提高超过300倍](https://www.kdnuggets.com/how-to-speed-up-python-pandas-by-over-300x)
