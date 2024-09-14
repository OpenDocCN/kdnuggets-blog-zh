# 制作你的第一个数据科学应用

> 原文：[`www.kdnuggets.com/2021/02/build-first-data-science-application.html`](https://www.kdnuggets.com/2021/02/build-first-data-science-application.html)

评论

**由 [Naser Tamimi](https://www.linkedin.com/in/nasertamimi/)，壳牌的数据科学家**

![图片](img/19cd12f42e1819c095846cfdb6db31ec.png)

由 [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/app?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

我需要学习什么才能制作我的第一个数据科学应用？关于网页部署呢？我是否需要学习 Flask 或 Django 来开发网页应用？我是否需要学习 TensorFlow 来制作深度学习应用？我应该如何设计我的用户界面？我还需要学习 HTML、CSS 和 JS 吗？

当我开始学习数据科学时，这些问题总是萦绕在我脑海中。我学习数据科学的目的不仅是为了开发模型或清理数据。我想要制作人们可以使用的应用。我在寻找一种快速制作 MVP（最小可行产品）以测试我的想法的方法。

如果你是一名数据科学家，想要制作你的第一个数据科学应用，在这篇文章中，我会展示你需要学习的 7 个 Python 库。我相信你已经知道其中的一些，但我在这里提到它们是为了那些不熟悉的人。

![图片](img/3758020ee6a5388afa4f20bc06bc0ea6.png)

由 [Med Badr Chemmaoui](https://unsplash.com/@medbadrc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/design?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

### Pandas

数据科学和机器学习应用都涉及数据。大多数数据集都不干净，需要进行某种清理和处理。Pandas 是一个可以加载、清理和处理数据的库。你可以使用像 SQL 这样的替代方案来进行数据处理和数据库管理，但 Pandas 更加简单，对希望成为开发者（或至少是 MVP 开发者）的数据科学家来说更为实用。

安装并了解更多关于 Pandas 的信息 [这里](https://pandas.pydata.org/)。

### Numpy

在许多数据科学项目中，包括计算机视觉，数组是最重要的数据类型。Numpy 是一个强大的 Python 库，允许你处理数组、操作它们并高效地应用算法。学习 Numpy 是使用我稍后提到的其他库的必要条件。

安装并了解更多关于 Numpy [这里](https://numpy.org/) 的信息。

### SciKitLearn

这个库是许多类型的机器学习模型和预处理工具的工具包。如果你在做机器学习项目，几乎没有可能不需要 SciKitLearn。

安装并了解更多关于 SciKit-Learn [这里](https://scikit-learn.org/stable/) 的信息。

### Keras 或 PyTorch

神经网络，特别是深度神经网络模型，是数据科学和机器学习中非常流行的模型。许多计算机视觉和自然语言处理方法都依赖于这些方法。一些 Python 库提供了访问神经网络工具的功能。TensorFlow 是最著名的，但我认为初学者使用 TensorFlow 较为困难。相反，我建议你学习 Keras，它是 TensorFlow 的一个接口（API）。Keras 使你作为人类能够轻松测试不同的神经网络架构，甚至构建自己的网络。另一个最近变得流行的选择是 PyTorch。

安装并了解更多关于 [Keras](https://keras.io/) 和 [PyTorch](https://pytorch.org/) 的信息。

### 请求

如今，许多数据科学应用程序都在使用 APIs（应用程序编程接口）。简单来说，通过 API，你可以请求服务器应用程序给你访问数据库或为你执行特定任务。例如，Google Map API 可以获取你提供的两个位置，并返回它们之间的旅行时间。没有 API，你必须重新发明轮子。Requests 是一个与 API 交互的库。现在，成为一名数据科学家几乎离不开使用 API。

安装并了解更多关于 Requests [这里](https://requests.readthedocs.io/en/master/) 的信息。

### Plotly

绘制不同类型的图表是数据科学项目的一个重要部分。虽然 Python 中最受欢迎的绘图库是 matplotlib，但我发现 Plotly 更专业、易于使用且灵活。Plotly 中的图表类型和绘图工具种类繁多。Plotly 的另一个优点是其设计。与具有科学外观的 matplotlib 图表相比，Plotly 看起来更友好。

安装并了解更多关于 Plotly [这里](https://plotly.com/) 的信息。

### ipywidgets

在用户界面方面，你必须在传统外观的用户界面和基于网页的用户界面之间进行选择。你可以使用像 PyQT 或 TkInter 这样的库来构建传统外观的用户界面。但我的建议是，尽可能制作能够在浏览器上运行的网页应用程序。为了实现这一点，你需要使用一个提供浏览器小部件的库。ipywidgets 提供了一组丰富的 Jupyter Notebook 小部件。

安装并了解更多关于 ipywidgets 的信息，请点击 [这里](https://ipywidgets.readthedocs.io/en/stable/)。

### Jupyter Notebook 和 Voila

你需要学习的最后几个工具是最简单的。首先，ipywidgets 在 Jupyter Notebook 中工作，你需要使用 Jupyter 来制作你的应用。我相信很多人已经在模型构建和探索性分析中使用 Jupyter Notebook。现在，将 Jupyter Notebook 视为前端开发工具。此外，你需要使用 Voila，一个第三方工具，它可以隐藏 Jupyter Notebook 中的所有代码部分。当你通过 Voila 启动 Jupyter Notebook 应用时，它就像一个网页应用。你甚至可以在 AWS EC2 机器上运行 Voila 和 Jupyter Notebook，并通过互联网访问你的简单应用。

安装并了解更多关于 Voila 的信息，请点击 [这里](https://github.com/voila-dashboards/voila)。

### 摘要

使用我在本文中提到的 7 个库，你可以构建出人们使用的数据科学应用。通过精通这些工具，你可以在几小时内构建 MVP，并用真实用户测试你的想法。之后，如果你决定扩展应用程序，你可以使用 Flask 和 Django 等更专业的工具，除了 HTML、CSS 和 JS 代码。

在 [Medium](https://tamimi-naser.medium.com/) 和 [Twitter](https://twitter.com/TamimiNas) 上关注我，获取最新故事。

**个人简介： [Naser Tamimi](https://www.linkedin.com/in/nasertamimi/)** 是一名为 Shell 工作的数据科学家。他的使命是教会读者他通过艰难的方式学到的东西。

[原文](https://towardsdatascience.com/build-your-first-data-science-application-9f1b816a5d67)。经许可转载。

**相关内容：**

+   使用 Pipes 进行更清晰的数据分析

+   使用 5 个必备的自然语言处理库入门

+   机器学习项目失败的五大原因

### 更多相关话题

+   [停止学习数据科学以寻找目标，并以目标来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功的数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [每位数据科学家都应该了解的三大 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [一个 90 亿美元的 AI 失败，剖析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [是什么让 Python 成为初创企业理想的编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)
