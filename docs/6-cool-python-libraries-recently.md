# 我最近发现的6个酷炫的Python库

> 原文：[https://www.kdnuggets.com/2021/09/6-cool-python-libraries-recently.html](https://www.kdnuggets.com/2021/09/6-cool-python-libraries-recently.html)

[评论](#comments)

**作者 [Dhilip Subramanian](https://medium.com/@sdhilip)，数据科学家和AI爱好者**

![图片](../Images/6ad8a106a0ec5f6b286bdefd412cffd6.png)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你在IT方面的组织

* * *

Python是机器学习的核心组成部分，库使我们的生活更简单。最近，我在进行ML项目时发现了6个很棒的库。它们帮助我节省了大量时间，我将在这篇博客中讨论它们。

## 1. clean-text

一个真正不可思议的库，clean-text应该是你处理抓取或社交媒体数据时的首选。最酷的地方在于，它不需要任何长的复杂代码或正则表达式来清理我们的数据。让我们看看一些示例：

**安装**

```py
!pip install cleantext
```

**示例**

```py
#Importing the clean text library
from cleantext import clean# Sample texttext = """ Zürich, largest city of Switzerland and capital of the canton of 633Zürich. Located in an Al\u017eupine. ([https://google.com](https://google.com/)). Currency is not ₹"""# Cleaning the "text" with clean textclean(text, 
      fix_unicode=True, 
      to_ascii=True, 
      lower=True, 
      no_urls=True, 
      no_numbers=True, 
      no_digits=True, 
      no_currency_symbols=True, 
      no_punct=True, 
      replace_with_punct=" ", 
      replace_with_url="", 
      replace_with_number="", 
      replace_with_digit=" ", 
      replace_with_currency_symbol="Rupees")
```

**输出**

![](../Images/ebefdb8e66b61b222d258ee32dc8e389.png)

从上面可以看到，它包含了“Zurich”中的Unicode（字母‘u’已编码）、ASCII字符（在Al\u017eupine.）、货币符号（卢比）、HTML链接和标点符号。

你只需要在clean函数中提及所需的ASCII、Unicode、URLs、数字、货币和标点符号。或者，它们可以在上述函数中用replace参数替换。例如，我将卢比符号更改为Rupees。

完全没有必要使用正则表达式或长代码。这个库非常方便，尤其是当你需要清理抓取或社交媒体数据的文本时。根据你的需求，你也可以单独传递参数，而不是将它们全部组合在一起。

欲了解更多详细信息，请查看这个[GitHub仓库](https://github.com/jfilter/clean-text)。

## 2\. drawdata

Drawdata是我发现的另一个酷炫的Python库。你有多少次遇到需要向团队解释ML概念的情况？这肯定很常见，因为数据科学完全是团队合作的事。这个库可以帮助你在Jupyter Notebook中绘制数据集。

个人来说，当我向我的团队解释ML概念时，我非常喜欢使用这个库。对创建这个库的开发者致以敬意！

Drawdata仅适用于具有四个类别的分类问题。

**安装**

```py
!pip install drawdata
```

**示例**

```py
# Importing the drawdata 
from drawdata import draw_scatterdraw_scatter()
```

**输出**

![](../Images/1df5aa80c014f829a732e10e6f7d10ad.png)

图片来源：作者

执行 draw_Scatter() 后，上述绘图窗口将打开。显然，有四个类别，即 A、B、C 和 D。你可以点击任何类别并绘制你想要的点。每个类别在绘图中代表不同的颜色。你还可以选择将数据下载为 CSV 或 JSON 文件。此外，数据可以复制到剪贴板，并从以下代码中读取。

```py
#Reading the clipboardimport pandas as pd 
df = pd.read_clipboard(sep=",")
df
```

这个库的一个局限性是它只提供两个数据点和四个类别。但除此之外，它绝对值得尝试。有关更多细节，请查看这个[GitHub 链接](https://github.com/koaning/drawdata)。

## 3\. Autoviz

我永远不会忘记我使用 matplotlib 进行探索性数据分析的时光。有很多简单的可视化库。然而，我最近发现了 Autoviz，它可以用一行代码自动可视化任何数据集。

**安装**

```py
!pip install autoviz
```

**示例**

我在这个示例中使用了 IRIS 数据集。

```py
# Importing Autoviz class from the autoviz library
from autoviz.AutoViz_Class import AutoViz_Class#Initialize the Autoviz class in a object called df
df = AutoViz_Class()# Using Iris Dataset and passing to the default parametersfilename = "Iris.csv"
sep = ","graph = df.AutoViz(
    filename,
    sep=",",
    depVar="",
    dfte=None,
    header=0,
    verbose=0,
    lowess=False,
    chart_format="svg",
    max_rows_analyzed=150000,
    max_cols_analyzed=30,
)
```

上述参数是默认的。有关更多信息，请查看[这里](https://pypi.org/project/autoviz/https://pypi.org/project/autoviz/)。

**输出**

![](../Images/e3a0101664fea9105c03c5c8e7071896.png)

图片来源：作者

我们可以通过一行代码查看所有的可视化并完成我们的 EDA。虽然有很多自动可视化库，但我真的很喜欢这个。

## 4\. Mito

大家都喜欢 Excel，对吧？这是初步探索数据集最简单的方法之一。我几个月前遇到了 Mito，但直到最近才尝试，它真的让我爱不释手！

这是一个带有图形用户界面的 Jupyter-lab 扩展 Python 库，添加了电子表格功能。你可以加载你的 CSV 数据，并将数据集作为电子表格进行编辑，它会自动生成 Pandas 代码。非常酷。

Mito 确实值得写一整篇博客。然而，今天我不会详细介绍。相反，我会给你一个简单的任务演示。有关更多详细信息，请查看[这里](https://trymito.io/)。

**安装**

```py
**#First install mitoinstaller in the command prompt** pip install mitoinstaller**# Then, run the installer in the command prompt**
python -m mitoinstaller install**# Then, launch Jupyter lab or jupyter notebook from the command prompt** python -m jupyter lab
```

有关安装的更多信息，请查看[这里](https://docs.trymito.io/getting-started/installing-mito)。

```py
**# Importing mitosheet and ruuning this in Jupyter lab**import mitosheet
mitosheet.sheet()
```

执行上述代码后，mitosheet 将在 jupyter lab 中打开。我使用的是 IRIS 数据集。首先，我创建了两列新数据。一列是平均萼片长度，另一列是总萼片宽度。其次，我更改了平均萼片长度的列名。最后，我为平均萼片长度列创建了一个直方图。

按照上述步骤操作后，代码会自动生成。

**输出**

![](../Images/fe424946548073a9edb746ca6654cd98.png)

图片来源：作者

以下代码是为上述步骤生成的：

```py
from mitosheet import * # Import necessary functions from Mito
register_analysis('UUID-119387c0-fc9b-4b04-9053-802c0d428285') # Let Mito know which analysis is being run# Imported C:\Users\Dhilip\Downloads\archive (29)\Iris.csv
import pandas as pd
Iris_csv = pd.read_csv('C:\Users\Dhilip\Downloads\archive (29)\Iris.csv')# Added column G to Iris_csv
Iris_csv.insert(6, 'G', 0)# Set G in Iris_csv to =AVG(SepalLengthCm)
Iris_csv['G'] = AVG(Iris_csv['SepalLengthCm'])# Renamed G to Avg_Sepal in Iris_csv
Iris_csv.rename(columns={"G": "Avg_Sepal"}, inplace=True)
```

## 5\. Gramformer

另一个令人印象深刻的库，Gramformer 基于生成模型，帮助我们纠正句子的语法。这个库有三个模型，分别是*检测器、高亮器和纠正器*。检测器识别文本是否有不正确的语法。高亮器标记出有问题的词性，而纠正器则修复错误。Gramformer 完全开源，并处于早期阶段，但它不适合处理长段落，因为它只在句子级别工作，并且已经训练了 64 长度的句子。

目前，纠正器和高亮器模型正在工作。我们来看一些示例。

**安装**

```py
!pip3 install -U git+https://github.com/PrithivirajDamodaran/Gramformer.git
```

**实例化 Gramformer**

```py
gf = Gramformer(models = 1, use_gpu = False) # 1=corrector, 2=detector (presently model 1 is working, 2 has not implemented)
```

![](../Images/56008268f4c4ff6e2edb99e1b3c99f06.png)

**示例**

```py
#Giving sample text for correction under gf.correctgf.correct(""" New Zealand is island countrys in southwestern Paciific Ocaen. Country population was 5 million """)
```

**输出**

![](../Images/3060be6c6cbf4aeeaa85e4ec15b2c1ad.png)

作者提供的图片

从以上输出中，我们可以看到它纠正了语法甚至拼写错误。这是一个非常了不起的库，功能也很好。我还没有尝试高亮器，你可以尝试一下，并查看 [GitHub 文档](https://github.com/PrithivirajDamodaran/Gramformer)以了解更多*细节*。

## 6\. Styleformer

我对 Gramformer 的积极体验促使我寻找更多独特的库。这就是我发现 Styleformer 的原因，它是另一个极具吸引力的 Python 库。Gramformer 和 Styleformer 都由 Prithiviraj Damodaran 创建，并且都基于生成模型。向创作者致以敬意，感谢开源。

Styleformer 帮助将休闲句子转换为正式句子，将正式句子转换为休闲句子，将主动句转换为被动句，将被动句转换为主动句。

来看看一些示例

**安装**

```py
!pip install git+https://github.com/PrithivirajDamodaran/Styleformer.git
```

**实例化 Styleformer**

```py
sf = Styleformer(style = 0)# style = [0=Casual to Formal, 1=Formal to Casual, 2=Active to Passive, 3=Passive to Active etc..]
```

**示例**

```py
# Converting casual to formal sf.transfer("I gotta go")
```

![](../Images/6c88c8eba994152d82f6ce53ad74667e.png)

```py
# Formal to casual 
sf = Styleformer(style = 1)     # 1 -> Formal to casual# Converting formal to casual
sf.transfer("Please leave this place")
```

![](../Images/1c5303fc21075a83c42987c39f5d0163.png)

```py
# Active to Passive 
sf = Styleformer(style = 2)     # 2-> Active to Passive# Converting active to passive
sf.transfer("We are going to watch a movie tonight.")
```

![](../Images/a32241558085e6baf66af2f03a047c01.png)

```py
# passive to active
sf = Styleformer(style = 2)     # 2-> Active to Passive# Converting passive to active
sf.transfer("Tenants are protected by leases")
```

![](../Images/c896b10bea33758ba328bd4f7de456f1.png)

查看上述输出，它的转换非常准确。我在一次分析中使用了这个库，将休闲语言转换为正式语言，特别是用于社交媒体帖子。有关更多详细信息，请查看 [GitHub](https://github.com/PrithivirajDamodaran/Styleformer)。

你可能对一些之前提到的库比较熟悉，但像 Gramformer 和 Styleformer 这样的库则是新兴的玩家。它们被极度低估，毫无疑问值得被了解，因为它们节省了我大量时间，并且我在 NLP 项目中大量使用了它们。

感谢阅读。如果你有任何补充意见，请随时留下评论！

你可能还喜欢我之前的文章 [*五个酷炫的 Python 数据科学库*](https://pub.towardsai.net/five-cool-python-libraries-for-data-science-7f1fce402b90)

**个人简介： [Dhilip Subramanian](https://medium.com/@sdhilip)** 是一名机械工程师，已获得分析硕士学位。他拥有9年的经验，专注于与数据相关的多个领域，包括IT、营销、银行、电力和制造业。他对自然语言处理和机器学习充满热情。他是[SAS社区](https://communities.sas.com/t5/user/viewprofilepage/user-id/271305)的贡献者，并喜欢在Medium平台上撰写有关数据科学的各种技术文章。

[原文](https://towardsdatascience.com/6-cool-python-libraries-that-i-came-across-recently-72e05dadd295)。经许可转载。

**相关内容：**

+   [五个有趣的Python库用于数据科学](/2020/04/five-cool-python-libraries-data-science.html)

+   [Python中的简单语音转文本](/2020/06/easy-speech-text-python.html)

+   [学习数据科学和机器学习：第一步](/2021/08/learn-data-science-machine-learning.html)

### 相关阅读

+   [数据科学、数据可视化与…的38个顶级Python库](https://www.kdnuggets.com/2020/11/top-python-libraries-data-science-data-visualization-machine-learning.html)

+   [2022年数据科学家应该知道的Python库](https://www.kdnuggets.com/2022/04/python-libraries-data-scientists-know-2022.html)

+   [可解释AI：揭示模型决策的10个Python库](https://www.kdnuggets.com/2023/01/explainable-ai-10-python-libraries-demystifying-decisions.html)

+   [数据清洗的Python库介绍](https://www.kdnuggets.com/2023/03/introduction-python-libraries-data-cleaning.html)

+   [超越Numpy和Pandas：挖掘鲜为人知的…](https://www.kdnuggets.com/2023/08/beyond-numpy-pandas-unlocking-potential-lesserknown-python-libraries.html)

+   [50级数据科学家：需要了解的Python库](https://www.kdnuggets.com/level-50-data-scientist-python-libraries-to-know)
