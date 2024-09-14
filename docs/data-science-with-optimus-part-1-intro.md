# 数据科学与 Optimus 第一部分：简介

> 原文：[`www.kdnuggets.com/2019/04/data-science-with-optimus-part-1-intro.html`](https://www.kdnuggets.com/2019/04/data-science-with-optimus-part-1-intro.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论![图](img/59f9452924682fb85e640ca8dfcedcc3.png)

如果你不认识这些图标也不用担心，我会在下一篇文章中解释它们 :)

数据科学已经达到了新的复杂性和当然也很棒的水平。我已经做了很多年，我希望人们能有一条清晰易懂的路径来完成他们的工作。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 需求

* * *

我已经讨论了数据科学及其他内容有一段时间了，现在是时候动手一起编程了。

这是关于使用 Optimus、Spark 和 Python 进行数据科学的系列文章的开始。

### 什么是 Optimus？

![图](img/2c867f5da10630609ff04d7e848cff42.png)

[`github.com/ironmussa/Optimus`](https://github.com/ironmussa/Optimus)

如果你一直关注我，你会知道我和[Iron-AI](https://iron-ai.com/)团队创建了一个名为 Optimus 的库。

Optimus V2 的创建旨在使数据清理变得轻而易举。API 的设计非常适合新手，同时对使用过 pandas 的人也非常熟悉。Optimus 扩展了 Spark DataFrame 的功能，添加了`.rows`和`.cols`属性。

使用 Optimus，你可以清理数据、准备数据、分析数据、创建分析器和图表，并进行机器学习和深度学习，所有这些都是分布式的，因为在后台我们有 Spark、TensorFlow、Sparkling Water 和 Keras。

它非常容易使用。它就像 pandas 的进化版，加上了一些 dplyr，并结合了 Keras 和 Spark。你用 Optimus 创建的代码可以在本地机器上运行，只需简单更改主节点，它也可以在本地集群或云端运行。

你会看到许多有趣的功能被创建来帮助数据科学周期的每一步。

Optimus 非常适合作为敏捷数据科学方法论的辅助工具，因为它可以帮助你完成几乎所有的步骤，并且可以轻松连接到其他库和工具。

如果你想了解更多关于敏捷数据科学方法论的内容，请查看这个：

[**创建以 ROI 为驱动的数据科学实践的敏捷框架**]

*数据科学是一个令人惊叹的研究领域，正受到学术界和工业界的积极发展……*www.business-science.io](http://www.business-science.io/business/2018/08/21/agile-business-science-problem-framework.html)

### 安装 Optimus

安装过程非常简单。只需运行命令：

```py
pip install optimuspyspark
```

然后你就准备好开始了。

### 运行 Optimus（以及 Spark、Python 等）

![](img/5085caf830450a6f046f4abca2ae1687.png)

创建一个数据科学环境应该是简单的。无论是尝试新东西还是用于生产。当我开始思考这些系列文章时，我惊讶于发现准备一个可重复的数据科学环境有多么困难，尤其是使用免费的工具。

一些参赛者包括：

[**Google Colaboratory**

colab.research.google.com](https://colab.research.google.com/)

[**微软 Azure 笔记本 - 在线 Jupyter 笔记本**

*提供在微软 Azure 云端运行的 Jupyter 笔记本的免费在线访问。* notebooks.azure.com](https://notebooks.azure.com/)

[**首页 - cnvrg**

cnvrg.io](https://cnvrg.io/)

**但毫无疑问对我来说最优秀的是 MatrixDS：**

[**由数据科学家为数据科学家建立的社区**

matrixds.com](https://matrixds.com/)

使用这个工具，你可以获得免费的 Python 环境（带 JupyterLab）和 R 环境（带 R Studio），还有 Shiny 和 Bokeh 等展示工具，甚至更多功能。而且全免费。你将能够运行仓库中的所有内容：

[**FavioVazquez/ds-optimus**

*如何使用 Optimus、Spark 和 Python 进行数据科学。 - FavioVazquez/ds-optimus* github.com](https://github.com/FavioVazquez/ds-optimus)

在 MatrixDS 内部，通过简单的项目分叉：

[**MatrixDS | 数据项目工作台**

*MatrixDS 是一个在任何规模上构建、共享和管理数据项目的地方。* community.platform.matrixds.com](https://community.platform.matrixds.com/community/project/5c5907039298c0508b9589d2/files)

只需创建一个帐户，你就可以开始使用。

### 旅程

![图像](img/59f9452924682fb85e640ca8dfcedcc3.png)

如果你不知道这些标志是什么，不用担心，我会在下一篇文章中解释 :)

上述路径是我将如何构建不同博客、教程和系列文章的结构。我现在必须告诉你，我正在准备一个全面的课程，涵盖一些商业科学的工具，详情请见：

[**商业科学大学**

*从虚拟研讨会中学习，带你了解整个数据科学业务过程中的问题解决……* university.business-science.io](https://university.business-science.io/)

第一部分在 MatrixDS 和 GitHub 上，因此你可以在 GitHub 上 Fork 并在 MatrixDS 上 Forklift。

[**MatrixDS | 数据项目工作台**

*MatrixDS 是一个在任何规模上构建、共享和管理数据项目的地方。* community.platform.matrixds.com](https://community.platform.matrixds.com/community/project/5c5907039298c0508b9589d2/files)

在 MatrixDS 上点击 Forklift：

![](img/8915f5feb6f431b9fc2645b0499f54b7.png)

[**FavioVazquez/ds-optimus**](https://github.com/FavioVazquez/ds-optimus)

*如何使用 Optimus、Spark 和 Python 进行数据科学。 - FavioVazquez/ds-optimus*github.com](https://github.com/FavioVazquez/ds-optimus)

![](img/254a4b88671349290e65117446cd5d5f.png)

这是笔记本，但请在平台上查看:)

有更新请在 Twitter 和 LinkedIn 关注我:).

**简介：[Favio Vazquez](https://www.linkedin.com/in/faviovazquez/)** 是一位物理学家和计算机工程师，专注于数据科学和计算宇宙学。他对科学、哲学、编程和音乐充满热情。他是西班牙语数据科学出版物 Ciencia y Datos 的创始人。他喜欢新挑战、与优秀团队合作和解决有趣的问题。他参与了 Apache Spark 的合作，帮助开发 MLlib、Core 和文档。他热衷于应用其在科学、数据分析、可视化和自动学习方面的知识和专业技能，帮助世界变得更美好。

[原文](https://towardsdatascience.com/data-science-with-optimus-part-1-intro-1f3e2392b02a)。经许可转载。

**相关：**

+   10 分钟实用 Apache Spark

+   数据管道、Luigi、Airflow：你需要知道的一切

+   4 个原因说明你的机器学习代码可能很糟糕

### 更多相关内容

+   [KDnuggets 新闻 3 月 30 日：最受欢迎的编程入门课程…](https://www.kdnuggets.com/2022/n13.html)

+   [哈佛最受欢迎的编程入门课程免费提供！](https://www.kdnuggets.com/2022/03/popular-intro-programming-course-harvard-free.html)

+   [KDnuggets 新闻，4 月 6 日：8 个免费的 MIT 数据科学课程…](https://www.kdnuggets.com/2022/n14.html)

+   [数据科学备忘单完整合集 - 第一部分](https://www.kdnuggets.com/2022/02/complete-collection-data-science-cheat-sheets-part-1.html)

+   [数据科学备忘单完整合集 - 第二部分](https://www.kdnuggets.com/2022/02/complete-collection-data-science-cheat-sheets-part-2.html)

+   [数据科学面试指南 - 第二部分：面试资源](https://www.kdnuggets.com/2022/04/data-science-interview-guide-part-2-interview-resources.html)
