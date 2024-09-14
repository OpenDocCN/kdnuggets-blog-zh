# 文本挖掘 101：从简历中挖掘信息

> 原文：[https://www.kdnuggets.com/2017/05/text-mining-information-resume.html](https://www.kdnuggets.com/2017/05/text-mining-information-resume.html)

**作者：Yogesh H. Kulkarni**

![](../Images/1e46e710e41a4018807625084a80b8e8.png)

### 摘要

本文展示了一个从文本简历中挖掘相关实体的框架。它展示了如何实现解析逻辑与实体规范的分离。虽然这里只考虑了一个简历样本，但该框架可以进一步增强，用于不同的简历格式，以及判决书、合同、专利、医学论文等文档。

### 介绍

世界上大多数非结构化数据以文本形式存在。为了理解这些数据，必须要么细致地逐一阅读，要么使用某些自动化技术来提取相关信息。考虑到这些文本数据的体量、种类和速度，必须采用文本挖掘技术来提取相关信息，将非结构化数据转化为结构化形式，从而使进一步的洞察、处理、分析和可视化成为可能。

本文处理的是特定领域，即申请人简历。众所周知，简历不仅有不同的文件格式（txt、doc、pdf 等），还具有不同的内容和布局。这种异质性使得提取相关信息成为一项挑战。尽管可能无法从所有类型的格式中完全提取所有相关信息，但可以从一些已知格式中开始简单的步骤，至少提取可能的信息。

大体上有两种方法：基于语言的方法和基于机器学习的方法。在“语言”方法中，通过模式搜索来找到关键信息，而在“机器学习”方法中，则使用监督和非监督方法来提取信息。这里使用的“正则表达式”（RegEx）是“语言”基础的模式匹配方法之一。

### 框架

在简历中实现实体提取的一种原始方法是为每个实体在代码程序中编写模式匹配逻辑。如果模式发生任何变化，或者引入新的实体/模式，则需要更改代码程序。这使得维护变得繁琐，因为复杂性增加。为了解决这个问题，提出了一种框架，将解析逻辑与实体规范分开，如下所示。实体及其 RegEx 模式在配置文件中指定。该文件还指定了用于每种实体类型的提取方法。解析器使用这些模式按指定方法提取实体。这种分离的优点不仅是可维护性，还包括其在法律/合同、医学等其他领域的潜在使用。

### 实体规范

配置文件指定了要提取的实体及其模式和提取方法。它还指定了在其中查找给定实体的部分。下方文本框中显示的规范描述了元数据实体，如 Name、Phone、Email 等。提取它们的方法是“univalue_extractor”。这些实体要搜索的部分被命名为“”，这是一个非标记的部分，如简历的前几行。像 Email 或 Phone 这样的实体可以有多个正则表达式模式。如果第一个失败，则尝试第二个，依此类推。这里是所用模式的简要描述：![](../Images/937d2cfcc0a91f8d5f04f640d9c95e1b.png)

+   姓名：假定简历的第一行包含姓名，前面可有“Name:”前缀。

+   邮箱：是一个单词（中间可有点），然后是“@”，接着是一个单词、点，再一个单词。

+   电话：括号中的国际代码（可选），然后是3-3-4的数字模式，前3位数字可选用括号包围。对于印度号码，它可以硬编码为“+91”，如下一条所示。

+   Python 的‘etree’ ElementTree 库用于将配置 xml 解析为内部字典。

+   解析器读取这些规范的字典，并使用它从文本简历中查找实体。

+   一旦匹配到实体，它会作为节点标签存储，如 Email、Phone 等。

![](../Images/bc7b59b8a7aa1e02cce29b29f2e87b89.png)

像上面描述的 Metadata，教育资格可以用下面的配置进行搜索：

+   解析器的方法“section_value_extractor”应当在“EducationSection”部分中使用。它通过匹配给定单词来找到部分中的值。

+   如果解析器发现任何单词，如“10^(th)”或“X”或“SSC”，那么这些就是提取的“Secondary”实体值，描述了中学教育水平。

+   如果解析器发现任何单词，如“12^(th)”或“XII”或“HSC”，那么这些就是提取的“HigherSecondary”实体值，描述了高级中学教育水平。

### 分段

上述代码片段中提到的部分是文本块，标记为 SummarySection、EducationSection 等。这些在配置文件的顶部进行指定。

+   方法“section_extractor”逐行解析文档并查找节标题。

+   通过用于标题的关键字来识别部分。例如，对于 SummarySection，关键字包括“Summary”、“Aim”、“Objective”等。

+   一旦找到匹配，设置“SummarySection”的状态，并将进一步的行合并到其中，直到找到下一个部分。

+   一旦新的标题匹配，下一节的新状态开始，如此继续。

![](../Images/13d7c4500c3acdddd9d67d91bd747c0d.png)

**结果**

以下是一个样本简历：

![](../Images/dee8d6e8b16ef536e15589413a0e257f.png)

![](../Images/13c4cc7f89cc34b302f3c39ffbb8a36c.png)

提取的实体如下所示：

解析器的实现、配置文件及样本简历可以在[github](https://github.com/yogeshhk/MiningResume)找到。

### 结束语

本文展示了如何从非结构化数据（如简历）中挖掘结构化信息。由于实施示例仅适用于一个样本，它可能不适用于其他格式。需要进行增强和定制，以适应其他类型的简历。除了简历，解析规范分离框架还可以用于不同领域的其他类型文档，通过指定特定领域的配置文件来实现。

**![](../Images/9f10d69469cf984ba9ce95b6a4ab4ec7.png)简介：** [**Yogesh H. Kulkarni**](https://www.linkedin.com/in/yogeshkulkarni/)，在几何建模领域工作超过16年后，最近完成了博士学位。在进行博士研究期间，他进入了数据科学领域，并希望进一步从事这一领域的职业。他对文本挖掘、机器学习/深度学习充满兴趣，主要使用Python技术栈进行实现。他很乐意听到您对本文以及相关话题、项目、任务、机会等的看法。

**相关：**

+   [利用深度学习从职位描述中提取知识](/2017/05/deep-learning-extract-knowledge-job-descriptions.html)

+   [理解文本分析](/2017/02/sas-text-analytics-nyc.html)

+   [文本挖掘亚马逊手机评论：有趣的见解](/2017/01/data-mining-amazon-mobile-phone-reviews-interesting-insights.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您组织的IT

* * *

### 更多相关话题

+   [停止学习数据科学，寻找目的，再找到目的……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [成为优秀数据科学家需要的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [一个90亿美元的AI失败案例分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [建立稳固的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)

+   [使用管道编写干净的Python代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)
