# Python 在 Excel 中：这将永远改变数据科学

> 原文：[https://www.kdnuggets.com/python-in-excel-this-will-change-data-science-forever](https://www.kdnuggets.com/python-in-excel-this-will-change-data-science-forever)

![Python 在 Excel 中：这将永远改变数据科学](../Images/956af61c7faec8620bc5e98205e435ed.png)

图片由作者提供

作为一名从事行业工作的数据科学家，过去一年感觉像是一场新技术突破和 AI 创新的过山车。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 需求

* * *

像 ChatGPT、Notable、Pandas AI 和[代码解释器](https://youtu.be/pVPp4ldOzJU?si=smscYnJQpKTZjLzm)这样的工具为我节省了大量时间，在编写、研究、编程和数据分析等任务中表现尤为突出。

就在我以为事情已经不能再更好了的时候，微软和 Anaconda 宣布了[Python 与 Excel 的集成](https://techcommunity.microsoft.com/t5/excel-blog/announcing-python-in-excel-combining-the-power-of-python-and-the/ba-p/3893439)！

你现在可以在 Excel 电子表格中编写 Python 代码来分析数据、构建机器学习模型并创建可视化效果。

# 那么……为什么 Python 和 Excel 的集成会引起如此轰动呢？

在 Excel 中编写 Python 代码的能力将为数据科学家和分析师开启新的大门。

当我获得第一个数据科学工作时，我以为自己会大部分时间在 Jupyter Notebook 中工作。令我惊讶的是，我在工作的第一天就不得不学习使用 Excel，因为高层管理、利益相关者和客户更喜欢从电子表格中解释结果。

实际上，我过去甚至创建过 Tableau 仪表板来向客户展示结果，但最终因为他们对 Excel 更为熟悉，所以不得不在 Excel 中重新构建图表。

这并不是我所在组织的独特之处。[截至2023年](https://earthweb.com/excel-users/)，全球已有超过一百万家公司和15亿人使用 Excel。

许多数据从业者，如我自己，发现自己常常在 Python IDE 和 Excel 电子表格之间切换。我们使用前者构建机器学习模型和分析数据，使用后者来展示我们的发现。

Python 和 Excel 的集成将帮助数据科学家和分析师简化工作流程，让我们能够在一个平台上进行数据分析、建模和展示。

还不相信吗？

让我们探索一下这种组合的一些潜在使用案例。

# 数据科学家在 Excel 中使用 Python 的方法

以下是数据科学家可以将电子表格的功能与 Python 的广泛库结合使用的一些方法：

## 1\. 数据预处理

如果我可以将我工作中的某一部分外包出去，那就是数据准备。这是一项繁琐的任务，当使用本机 Excel 功能时变得极其耗时。

借助新的 Python-Excel 集成功能，用户现在可以直接将像 Pandas 这样的库导入到 Excel 中，并在 Excel 电子表格中直接进行高级筛选和数据聚合。

你只需在电子表格中的一个单元格中输入“=PY”，然后高亮显示你想用 Python 分析的数据，就会为你创建一个 Pandas dataframe。然后，你可以像在 Jupyter Notebook 中一样对这些数据进行分组和处理。

这是一个展示如何在 Excel 中创建 Pandas dataframe 的示例：

![Python in Excel: This Will Change Data Science Forever](../Images/a26f3b34e9f2e596ef992b3f9539e1d0.png)

来源：[Microsoft](https://techcommunity.microsoft.com/t5/excel-blog/announcing-python-in-excel-combining-the-power-of-python-and-the/ba-p/3893439)

## 2\. 机器学习

虽然 Excel 提供了诸如线性回归和图表趋势线拟合等基本工具，但大多数机器学习用例需要更复杂的建模技术，这超出了 Excel 的本机能力。

借助这种 Python-Excel 集成功能，用户现在可以在 Excel 中使用 Scikit-Learn 等库构建和训练高级统计模型。模型结果可以在 Excel 中可视化和展示，弥合建模与决策制定之间的差距。

这是一个展示如何使用 Python 在 Excel 中构建决策树分类器的图像：

![Python in Excel: This Will Change Data Science Forever](../Images/b97e8a5a936851522db0e6e517d565cd.png)

来源：[Microsoft](https://techcommunity.microsoft.com/t5/excel-blog/announcing-python-in-excel-combining-the-power-of-python-and-the/ba-p/3893439)

## 3\. 数据分析

在 Excel 中分析数据的过程可能非常繁琐——当同时处理多个文件时，用户需要手动复制和粘贴数据，拖动公式到单元格，并手动合并数据。

例如，如果我有五个这样的每月销售数据工作表：

![XXXXX](../Images/456f85e4ad35f785a15f673dfd0909f9.png)

如果我想找到一个月内销售量超过 100 个单位的产品，我首先需要手动将所有工作表的数据复制并粘贴到第一个工作表的数据下方。然后，我需要更改日期格式并创建数据透视表。

最后，我还需要添加一个筛选器来找到符合我标准的产品。

每次我在不同的文件或工作表中获得新的销售数据时，我都需要手动复制和粘贴。

随着数据量的增加，这个过程变得越来越困难且容易出错。

相反，整个分析可以通过以下代码行在 Python 中简化：

```py
# 1\. Merge the data
df_merged = pd.concat([df_jan, df_feb], ignore_index=True)

# 2\. Convert the date format
df_merged['Date'] = pd.to_datetime(df_merged['Date']).dt.strftime('%Y-%m-%d')

# 3\. Compute the total units sold for each product
grouped_data = df_merged.groupby('Product').agg({'Units Sold': 'sum'}).reset_index()

# 4\. Identify products that sold more than 100 units
products_over_100 = grouped_data[grouped_data['Units Sold'] > 100]

products_over_100
```

每次有新数据进来时，我只需更改一行代码并重新运行程序，就能获得所需的结果。通过Python与Excel的集成，我可以在一个平台上监督整个数据分析流程，从而最大化效率。

## 4. 数据可视化

尽管Excel本身提供了大量的可视化选项，但该工具在构建图表类型方面仍然有些限制。像小提琴图、热图和对角图这样的图表在Excel中并不容易获得，这使得数据科学家难以表示复杂的统计关系。

运行Python代码的能力将使Excel用户能够使用如Matplotlib和Seaborn等库来创建更复杂、更具自定义的图表。

![Excel中的Python：这将永远改变数据科学](../Images/83dac9414630748c3d78b3f213c4f242.png)

来源：[微软](https://techcommunity.microsoft.com/t5/excel-blog/announcing-python-in-excel-combining-the-power-of-python-and-the/ba-p/3893439)

# 如何在Excel中使用Python？

在撰写本文时，Python与Excel的功能仅通过[Microsoft 365 Insider计划](https://insider.microsoft365.com/en-us/join/windows)提供。你需要注册并选择Beta Channel Insider级别才能访问此功能，因为它尚未公开推出。

一旦你加入了365 Insider计划，你会在“公式”选项卡中找到一个Python部分。你只需点击“插入Python”即可开始编写自己的Python代码。

另外，你可以在任意单元格中输入`=PY`来开始使用。

![Excel中的Python：这将永远改变数据科学](../Images/e6a3b3cdb4edc1ea2f2594135f72ec08.png)

来源：Anaconda

# Python与Excel的集成将使数据科学民主化

随着ChatGPT的发布，以及诸如代码解释器和Notable等插件的出现，许多曾经需要强大技术专长的任务变得更容易执行。

对于数据科学家和分析师而言尤其如此——你现在可以将CSV文件上传到ChatGPT，它将清理、分析并构建你的数据集模型。

在我看来，Python与Excel的集成使我们离[数据科学和分析的民主化](/2023/06/chatgpt-replace-data-scientists.html)更近了一步。

在市场营销和金融等领域，那些完全依赖Excel的行业专家现在将能够执行Python代码来分析他们的数据，而无需下载编程IDE。

在一个熟悉的界面中处理数据，加上ChatGPT在编写代码方面的熟练，将使非程序员能够执行数据科学工作流程，并使用Python代码解决问题。

如果你是一个不懂编码的Excel用户，这对于你来说是一个在你已经熟悉的界面中学习Python编程的绝佳机会。

**[Natassha Selvaraj](https://linktr.ee/natasshaselvaraj)** 是一位自学成才的数据科学家，对写作充满热情。你可以在[LinkedIn](https://www.linkedin.com/in/natassha-selvaraj-33430717a/)上与她联系。

### 更多相关话题

+   [别再谈ChatGPT，这款新AI助手已遥遥领先，将彻底改变工作方式](https://www.kdnuggets.com/2023/08/forget-chatgpt-new-ai-assistant-leagues-ahead-change-way-work-forever.html)

+   [使用Python自动化微软Excel和Word](https://www.kdnuggets.com/2021/08/automate-microsoft-excel-word-python.html)

+   [我使用了ChatGPT（每天）5个月。这里有一些隐藏的宝藏…](https://www.kdnuggets.com/2023/07/used-chatgpt-every-day-5-months-hidden-gems-change-life.html)

+   [人工智能将如何改变移动应用程序](https://www.kdnuggets.com/2022/12/artificial-intelligence-change-mobile-apps.html)

+   [免费的微软Excel初学者课程](https://www.kdnuggets.com/2022/09/free-microsoft-excel-beginners-course.html)

+   [通过《快速Python数据科学》提升你的Python技能！](https://www.kdnuggets.com/2022/06/manning-step-python-game-fast-python-data-science.html)
