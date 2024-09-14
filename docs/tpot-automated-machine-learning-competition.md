# TPOT 自动化机器学习竞赛：AutoML 能否击败 Kaggle 上的人类？

> 原文：[https://www.kdnuggets.com/2017/06/tpot-automated-machine-learning-competition.html](https://www.kdnuggets.com/2017/06/tpot-automated-machine-learning-competition.html)

**作者：兰迪·奥尔森，宾夕法尼亚大学**。

[自动化机器学习（AutoML)](/2017/01/current-state-automated-machine-learning.html)预计将在 2017 年对数据科学产生深远的影响。在宾夕法尼亚大学，我们一直在努力开发 [TPOT](https://github.com/rhiever/tpot)，这是一个最先进的开源 AutoML 工具，可以优化监督学习问题的机器学习管道。

现在我们想看看你能用 TPOT 做些什么。

![TPOT](../Images/40cac51d39530f5abf52161ff929709f.png)

在接下来的几个月里，我们将挑战你将 TPOT 应用到你在 [Kaggle](https://www.kaggle.com/) 上找到的任何有趣的数据科学问题。如果你的作品在 Kaggle 问题的排行榜中排名前 25%，我们希望看到 TPOT 如何帮助你实现这一点。

比赛结束时，TPOT 团队将审查所有参赛作品，对其进行排名，并向排名前 3 的参赛作品颁发（奖金！）奖品：$500、$250、$100。比赛结束后，我们还会发布一篇文章，突出展示最佳作品。

参赛作品将根据在 Kaggle 问题上的排名以及提供的技术说明进行评判。

将你的作品通过电子邮件发送到 [olsonran@upenn.edu](mailto:olsonran@upenn.edu)

参赛作品提交截止日期为**2017年8月7日**，获奖者将在接下来的一周内公布。

### 入门 TPOT

如果你对 TPOT 和 AutoML 不熟悉，你很幸运！我们已经编写了 [详细文档](http://rhiever.github.io/tpot/)，描述了如何使用 TPOT，还有 [用户指南](http://rhiever.github.io/tpot/using/) 和 [API 文档](http://rhiever.github.io/tpot/api/)。为了给你一个起点，下面是一个将 TPOT 应用到 scikit-learn 的 MNIST 数据集的基本示例。

这段 Python 代码将会在训练数据上拟合 TPOT，对测试数据进行评分，然后将优化后的管道以 Python 代码的形式导出到文件“tpot_mnist_pipeline.py”。

重要的是，TPOT 使用的机器学习算法可以进行高度定制。实际上，**TPOT 可以优化任何遵循 [scikit-learn 接口](http://scikit-learn.org/stable/developers/contributing.html#apis-of-scikit-learn-objects)的机器学习算法的管道**。你可以在用户指南 [这里](http://rhiever.github.io/tpot/using/#customizing-tpots-operators-and-parameters) 阅读有关定制 TPOT 配置的更多信息。

### 入门 Kaggle

如果您是 Kaggle 新手，他们提供了一个 [广泛的竞赛列表](https://www.kaggle.com/competitions)，您可以在他们的网站上查看。Kaggle 的经典问题之一是 [Titanic 挑战](https://www.kaggle.com/c/titanic)，您需要使用机器学习预测 Titanic 灾难的幸存者。

我们有一个 [Jupyter Notebook 教程](https://github.com/rhiever/tpot/blob/master/tutorials/Titanic_Kaggle.ipynb)，指导您使用 TPOT 解决 Kaggle 的 Titanic 挑战的基本方法。

### 附加比赛说明

+   每个条目必须具有开源许可证，以便我们可以分享和讨论每个人的条目。

+   我们建议使用 Jupyter Notebooks 提交条目，但只要包含代码和写作，任何格式都是可以接受的。

+   条目必须在 8 月 7 日前排名前 25% 才能计入比赛。

+   Kaggle 教程问题，例如 Titanic 问题，将不计入比赛。

+   我们创建了一个 [公开的 GitHub 仓库](https://github.com/EpistasisLab/tpot-competition)，供任何希望合作提交比赛条目的人使用。

**简介：[兰迪·奥尔森博士](http://www.randalolson.com/author/rhiever/)** 是宾夕法尼亚大学的博士后研究员。作为 Jason H. Moore 教授研究实验室的成员，他研究生物启发的 AI 及其在生物医学问题上的应用。

[原始](http://www.randalolson.com/2017/06/02/tpot-automated-machine-learning-competition/)。已获得许可转载。

**相关：**

+   [自动化机器学习：与 Randy Olson 的采访，TPOT 主要开发者](/2016/11/autoamted-machine-learning-interview-randy-olson-tpot.html)

+   [TPOT：一个用于自动化数据科学的 Python 工具](/2016/05/tpot-python-automating-data-science.html)

+   [自动化机器学习的现状](/2017/01/current-state-automated-machine-learning.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT

* * *

### 更多相关主题

+   [使用 TPOT 的机器学习管道优化](https://www.kdnuggets.com/2021/05/machine-learning-pipeline-optimization-tpot.html)

+   [为什么我们永远需要人类来训练 AI——有时是实时的](https://www.kdnuggets.com/2021/12/why-we-need-humans-training-ai.html)

+   [使用 Streamlit 的 DIY 自动化机器学习](https://www.kdnuggets.com/2021/11/diy-automated-machine-learning-app.html)

+   [使用 Python 进行自动化机器学习：案例研究](https://www.kdnuggets.com/2023/04/automated-machine-learning-python-case-study.html)

+   [使用 Python 进行自动化机器学习：不同方法的比较…](https://www.kdnuggets.com/2023/03/automated-machine-learning-python-comparison-different-approaches.html)

+   [Nota AI 发布了 NetPresso Model Search 的测试版，它们的…](https://www.kdnuggets.com/2022/04/nota-ai-releases-beta-version-netpresso-model-search-hardwareaware-automl-tool.html)
