# 10 个 Python 技能，训练营中不会教你

> 原文：[`www.kdnuggets.com/2020/12/10-python-skills-dont-teach-bootcamp.html`](https://www.kdnuggets.com/2020/12/10-python-skills-dont-teach-bootcamp.html)

评论![图像](img/3d7f5b23b52139faf90e6068dd2fd7cb.png)

图片由*[Sandy Torchon*](https://www.pexels.com/@sandy-torchon-2229511?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 在*[Pexels*](https://www.pexels.com/photo/people-riding-the-roller-coaster-3973555/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)上拍摄

数据科学训练营非常有趣，但他们没有时间教你所有的内容。

编程训练营的经历就像是去游乐园（虽然那里的一些陌生人可能会成为你的好朋友）。当过山车启动时，它要求你全神贯注。在一阵阵的紧张间隙中，你将有机会喘口气——交流故事、推荐和想法。

通过这 10 个 Python 技能，重新体验学习新事物的兴奋感，这些技能在训练营中不会教你。

### #10 — 设置 DataFrame 显示选项

在 Jupyter Notebook 中更改 pandas DataFrames 的显示方式非常简单。我通常将这段代码包含在与我的导入语句相同的单元格中：

使用这些设置，我可以完全读取可能包含大量文本的单元格。我不必担心过长的数据框，但可以随心所欲地左右滚动。

玩转这些选项，找到适合你的设置。更多信息，你可以查看 pandas 文档中的这一部分[这里](https://pandas.pydata.org/pandas-docs/stable/user_guide/options.html)。

### #9 — 更改 pandas 显示数字的方式

如果你想更改 DataFrames 中数字的显示方式，可以使用这些方便的选项来舍入尾随的小数。

```py
pd.set_option(‘precision’, 2) # Round to two decimal points
```

第二个选项还提供了在较大的数字之间使用逗号分隔符的功能：

```py
pd.options.display.float_format = ‘{:,.2f}’.format 
```

### #8 — 导入 Excel 工作簿并附加表单名称

如果你正在读取一个包含多个表单的工作簿，可以使用以下方法将它们全部导入一个数据框：

```py
df = pd.concat(pd.read_excel('Ticket_Sales_Total.xlsx', sheet_name=**None**), ignore_index=**True**)
```

[这个技巧有效](https://pbpython.com/pandas-excel-tabs.html)，当你的数据使用相同的表头且没有其他信息可以从表单名称中获得时。

另外，如果你想读取表单并保留一些来自表单名称的信息，可以使用下面的函数。

在第 15 行，pandas 创建了一个新列（‘sheet’），其值为表单名称的最后一个单词。如果 Ticket_Sales_Total.xlsx 中的表单名称为 *Ticket Sales 2017*、*Ticket Sales 2018* 和 *Ticket Sales 2019*，那么 read_excel_sheets() 函数将为每一行附加来自表单名称的相关年份。

*感谢*[*Colin Copland*](https://www.caktusgroup.com/blog/2019/08/13/import-multiple-excel-sheets-pandas/)*的这个小提示！*

### #7 — 检查 pandas 行的随机选择

与其仅查看 dataframe 的 `.head()` 或 `.tail()`，你可以通过以下方式查看随机行的选择：

```py
df.sample(n)
```

这很有用，因为在一个排序的数据框中，异常记录可能会出现在头部或尾部，从而导致在 [进行探索性数据分析（EDA）](https://towardsdatascience.com/10-underrated-python-skills-dfdff5741fdf) 时产生扭曲的视角。

关于 pandas 的示例项目，请查看：

[**临床文本中的命名实体识别**](https://medium.com/atlas-research/ner-for-clinical-text-7c73caddd180)

使用 pandas 将 2011 年的 i2b2 数据集重新格式化为 CoNLL 格式，以用于自然语言处理（NLP）。

### 第 6 步 — 利用预测力评分代替相关性

[预测力评分](https://github.com/8080labs/ppscore) 是由 [弗洛里安·维茨霍雷克](https://medium.com/u/6ed760f28120?source=post_page-----419e5e4c4d66--------------------------------) 和 [8080 Labs](https://8080labs.com/) 团队开发的，目的是改进相关性度量。

**相关性有限**，因为它会忽略 *非线性* 关系（例如，绘制每日温度和主题公园门票销售的二次关系图，或表示游乐设施票价与排队人数的阶跃函数，或者在“猜测你的体重”嘉年华游戏中使用的高斯函数）。任何与 *分类* 变量相关的关系也会被相关性矩阵遗漏。

此外，相关性缺乏提供关系 *不对称性* 信息的能力。例如，知道一个顾客最喜欢的公园部分可能不能预测他们最喜欢的游乐设施，但知道他们最喜欢的游乐设施会更强地预测他们最喜欢的公园部分。

相比之下，预测力评分可以检测非线性效应，自动编码分类变量，并量化不对称性。它计算列对之间的预测关系，并提供从 0 到 1 的评分。

使用方法，只需 `import ppscore as pps` 并调用 `pps.matrix(df)`。

[**你从未听说过的最佳数据科学认证**](https://towardsdatascience.com/best-data-science-certification-4f221ac3dbe3)

关于数据战略中最有价值的培训的实用指南。

### # 第 5 步 — 创建一个包

模块有助于将可重用的代码，如 Python 函数、变量和类进行模块化。以这种方式进行组织可以使代码更易于理解和使用。

> 对我来说，这是数据科学家提高生产力的最大助力。它使你能够更快地工作，减少错误。而且，通过编写包，你还可以提升你的编码技能。— [亚当·沃塔瓦](https://medium.com/u/4953633c3102?source=post_page-----419e5e4c4d66--------------------------------)

一个包将包含一个或多个相关模块。我们可以创建一个名为 mythemepark 的包，步骤如下：

第一步 — 创建一个名为 MyThemePark 的新文件夹。

第 2 步——在 MyThemepark 内部创建一个名为 mythemepark 的子文件夹。

第 3 步——使用像[atom](https://atom.io/)这样的 Python IDE，创建模块 greet_visitors.py（用于提供欢迎游客进入公园的代码）、functions.py（提供操作各种游乐设施和游戏的代码）和 classes.py（提供可以实例化新对象（如娱乐设施、商店、促销等）的模板）。

备注：

+   确保你使用这些[PEP8 包和模块命名约定](https://www.python.org/dev/peps/pep-0008/#package-and-module-names)。

+   包曾经需要有一个 __init__.py 文件，但随着[命名空间包](https://www.python.org/dev/peps/pep-0420/#rationale)的引入，现在不再需要这样做。

### #4— 检查包的大小

在 pip 安装了运行主题公园所需的所有依赖项后，你的 SSD 可能会有些混乱。检查已安装包的大小将帮助你了解哪些包占用最多空间。然后，你可以决定哪些包“带来快乐”，并适当进行[KonMari](https://konmari.com/)整理。

要找到 Linux 机器上已安装包的路径，请输入：

```py
pip3 show "some_package" | grep "Location:"
```

这将返回 path/to/all/packages。类似于：/Users/yourname/opt/anaconda3/lib/python3.7/site-packages

将该文件路径插入到以下命令中：

```py
du  -h  path/to/all/packages
```

其中`du`报告文件系统的磁盘空间使用情况。

这段代码将输出每个包的大小。最后一行输出将包含所有包的总大小。

[**10 个被低估的 Python 技能**](https://towardsdatascience.com/10-underrated-python-skills-dfdff5741fdf)

使用这些技巧提升你的数据科学技能，以改善 Python 编码以进行更好的 EDA、目标分析、特征…

### #3 — 检查内存使用

如同优化工作空间一样，检查[代码组件的内存使用](https://stackoverflow.com/questions/40993626/list-memory-usage-in-ipython-and-jupyter)也可能很有用。你可以使用 Python 的[sys.getsizeof](https://docs.python.org/3/library/sys.html#sys.getsizeof)方法，通过实现以下代码来做到这一点：

### #2— 提升你的命令行工具

[Click](https://click.palletsprojects.com/en/7.x/)是一个 Python 命令行工具，使你能够为 bash shell 创建直观的程序和接口。Click 支持选项对话框、用户提示、确认请求、环境变量值等。

这是一个示例脚本，用于向游乐设施操作员请求密码：

将输出：

```py
$ encrypt
Password: 
Repeat for confirmation:
```

### #1 — 检查所有内容是否符合 PEP8 规范

[nblint 包](https://github.com/alexandercbooth/nblint)允许你在 Jupyter Notebook 中运行 pycodestyle 引擎。这将检查你的代码（即代码规范检查）使用 pycodestyle 引擎。

Linting 突出显示你 Python 代码中的任何语法或风格问题，使其更不容易出错，并且对你的同事更具可读性。Linting 工具最早由 1978 年的沮丧调试者引入，这个做法确实得名于从干衣机中取出的衣物上去除小块杂布的行为。

### 附加：清理 conda 缓存

首先，简要说明一下`pip`和`conda`之间的区别。 [pip](https://pip.pypa.io/en/stable/) 是 Python 包装权威机构推荐的用于从 Python 包索引 [PyPI](https://pypi.org/) 安装软件包的工具。 [conda](https://conda.io/docs/) 是来自 [Anaconda](https://repo.anaconda.com/) 的跨平台包和环境管理器。

一般来说，混合使用 pip 和 conda 包管理器是不明智的。这是因为这两个管理器之间没有沟通——这可能会导致软件包冲突。考虑在虚拟环境中专门使用 pip [除非你准备好使用 conda](https://towardsdatascience.com/10-underrated-python-skills-dfdff5741fdf)。

我们已经介绍了如何清理你用 pip 安装的软件包——这里是移除 conda 安装的软件包的说明。如果你一直在使用 conda 包管理器，你可以通过使用以下代码来释放空间：

```py
conda clean --all
```

### 总结

再次回顾一下我们在本文中涵盖的十个提示：

+   [设置 DataFrame 显示选项 (#10)](https://towardsdatascience.com/10-python-skills-419e5e4c4d66#9907)

+   [更改 pandas 显示数字的方式 (#9)](https://towardsdatascience.com/10-python-skills-419e5e4c4d66#c5b0)

+   [导入 Excel 工作簿并附加工作表名称 (#8)](https://towardsdatascience.com/10-python-skills-419e5e4c4d66#1282)

+   [检查熊猫行的随机选择 (#7)](https://towardsdatascience.com/10-python-skills-419e5e4c4d66#3346)

+   [利用预测能力分数代替相关性 (#6)](https://towardsdatascience.com/10-python-skills-419e5e4c4d66#c2df)

+   [创建一个软件包 (#5)](https://towardsdatascience.com/10-python-skills-419e5e4c4d66#fb56)

+   [检查软件包的大小 (#4)](https://towardsdatascience.com/10-python-skills-419e5e4c4d66#531b)

+   [检查内存使用 (#3)](https://towardsdatascience.com/10-python-skills-419e5e4c4d66#5407)

+   [提升你的命令行工具 (#2)](https://towardsdatascience.com/10-python-skills-419e5e4c4d66#35ba)

+   [检查所有内容是否符合 PEP8 (#1)](https://towardsdatascience.com/10-python-skills-419e5e4c4d66#4ec5)

+   [清理 conda 缓存](https://towardsdatascience.com/10-python-skills-419e5e4c4d66#9d53)

**如果你喜欢这篇文章**，请在 [Medium](https://medium.com/@nicolejaneway)、 [LinkedIn](http://www.linkedin.com/in/nicole-janeway-bills)、 [YouTube](https://www.youtube.com/channel/UCO6JE24WY82TKabcGI8mA0Q?view_as=subscriber) 和 [Twitter](https://twitter.com/Nicole_Janeway) 上关注我，以获取更多提升数据科学技能的想法。

### 继续提升你的技能

[**10 个适合初学者的 Python 技能**](https://towardsdatascience.com/10-python-skills-beginners-3066305f0d3c)

Python 是增长最快、最受喜爱的编程语言。通过这些数据科学技巧入门。

[**10 个被低估的 Python 技能**](https://towardsdatascience.com/10-underrated-python-skills-dfdff5741fdf)

通过这些技巧提升你的数据科学技能，改进你的 Python 编码以优化 EDA、目标分析、特征…

[**如何以最小的努力通过 AWS Cloud Practitioner 认证**](https://nicolejaneway.medium.com/how-to-ace-the-aws-cloud-practitioner-certification-with-minimal-effort-39f10f43146)

预测：阴天，100% 可能第一次尝试就通过。

[**如何使你的数据科学项目具有未来适应性**](https://towardsdatascience.com/model-selection-and-deployment-cf754459f7ca)

5 个关键的 ML 模型选择和部署元素

[**逐步指南：在 Python 中映射 GIS 数据**](https://towardsdatascience.com/walkthrough-mapping-gis-data-in-python-92c77cd2b87a)

通过 GeoPandas DataFrames 和 Google Colab 提高你对地理空间信息的理解

**简介： [尼科尔·詹纳威·比尔斯](http://www.linkedin.com/in/nicole-janeway-bills)** 是一位拥有商业和联邦咨询经验的机器学习工程师。精通 Python、SQL 和 Tableau，尼科尔在自然语言处理（NLP）、云计算、统计测试、定价分析和 ETL 过程方面具有商业经验，并旨在利用这些背景将数据与业务成果联系起来，并继续发展技术技能。

[原始](https://towardsdatascience.com/10-python-skills-419e5e4c4d66)。已获转载许可。

**相关：**

+   10 个适合初学者的 Python 技能

+   10 个被低估的 Python 技能

+   你从未听说过的最佳数据科学认证

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 更多相关内容

+   [大型语言模型是什么？它们如何工作？](https://www.kdnuggets.com/2023/05/large-language-models-work.html)

+   [基础模型是什么？它们如何工作？](https://www.kdnuggets.com/2023/05/foundation-models-work.html)

+   [向量数据库是什么？它们为何对 LLMs 重要？](https://www.kdnuggets.com/2023/06/vector-databases-important-llms.html)

+   [你的特性重要吗？这并不意味着它们是好的](https://www.kdnuggets.com/your-features-are-important-it-doesnt-mean-they-are-good)

+   [哪个更好：数据科学训练营 vs 学位 vs 在线课程](https://www.kdnuggets.com/2022/09/best-data-science-bootcamp-degree-online-course.html)

+   [免费全栈 LLM 训练营](https://www.kdnuggets.com/2023/06/free-full-stack-llm-bootcamp.html)
