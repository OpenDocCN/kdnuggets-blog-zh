# 文本聚类：从非结构化数据中快速获得洞察，第2部分

> 原文：[https://www.kdnuggets.com/2017/07/text-clustering-unstructured-data-part2.html](https://www.kdnuggets.com/2017/07/text-clustering-unstructured-data-part2.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png)[评论](#comments)

**由 Vivek Kalyanarangan 提供**。

![文本聚类](../Images/67c6b391782158a75264cfc1b6faf4f4.png)

在这两部分系列中，我们将探讨文本聚类以及如何从非结构化数据中获得洞察。它将非常强大且具备工业级别的力量。第一部分将关注动机，第二部分将讨论实施。

本文是关于如何使用文本聚类从非结构化数据中获得洞察的两部分系列的第二部分。我们将以非常模块化的方式构建它，以便可以应用于任何数据集。此外，我们还将专注于**将功能暴露为 API**，以便它可以作为即插即用的模型，而不会对现有系统造成干扰。

+   [文本聚类：如何从非结构化数据中快速获得洞察——第1部分：动机](/2017/06/text-clustering-unstructured-data.html)

+   文本聚类：如何从非结构化数据中快速获得洞察——第2部分：实施

如果你很急，你可以在我的 [Github 页面](https://github.com/vivekkalyanarangan30/Text-Clustering-API/) 找到完整的项目代码

这只是最终输出效果的预览——

![文本聚类 API](../Images/7e0179269fc5076a5b471604f0c09141.png)

### 安装

+   Anaconda Python 2.7 发行版——[从这里下载](https://www.continuum.io/downloads)

+   flask API python 包——安装 anaconda 后，打开命令提示符并输入

    ```py
    pip install flask
    ```

+   flasgger python 包——安装 anaconda 和 flask 后，打开命令提示符并输入

    ```py
    pip install flasgger
    ```

现在你已经准备好工具了。点击这里下载代码以开始设置。

### 运行中

解压缩内容，打开命令提示符并输入

```py
python CLAAS_public.py

```

服务器将启动，你现在可以在此位置访问工具——[http://localhost:8180/apidocs/index.html](http://localhost:8180/apidocs/index.html)

### 工作流程

#### 无指导聚类

这就是实际进行 KMeans 聚类的地方。

1.  它需要一个 CSV 文件作为输入。此外，你还需要输入包含非结构化文本的列名和集群数量

1.  一旦你点击“尝试一下”按钮，输入将由 API 使用

1.  API 进行文本清理、Tfidf 向量化和聚类

1.  一旦完成，它会提供一个下载链接，附加一个包含集群编号的额外列

#### 有指导的聚类

就这项技术而言，它要简单一些。

1.  它需要两个文件作为输入，一个是要聚类的数据文件，另一个是**预定义的关键词**

1.  此外，它需要与无指导聚类等效的列名称

1.  输出会为每个给定的关键词添加额外的列

1.  如果文档包含该词，则为 TRUE，否则为 FALSE

这可以感知文档中关键字的存在/缺失，指示哪些文档包含关键字信号，哪些文档不包含。

#### [结论](https://machinelearningblogs.com/2017/06/23/text-clustering-get-quick-insights-unstructured-data-2/#)

这就是关于文本聚类的多系列内容的全部。这样就足够开始了，对吧？撰写这一系列内容是一次很棒的体验。下一部分见。玩得开心！

[原文](https://machinelearningblogs.com/2017/06/23/text-clustering-get-quick-insights-unstructured-data-2/)。经许可转载。

**简介：** [Vivek Kalyanarangan](https://machinelearningblogs.com/about/) 是一名数据科学家，关注医疗领域的问题。

**相关：**

+   [文本聚类：从非结构化数据中快速获得见解](/2017/06/text-clustering-unstructured-data.html)

+   [K-means 聚类与 Tableau – 呼叫详细记录示例](/2017/06/kmeans-clustering-tableau-call-detail-records.html)

+   [从零开始的 Python 机器学习工作流程 第 2 部分：k-means 聚类](/2017/06/machine-learning-workflows-python-scratch-part-2.html)

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT

* * *

### 更多相关主题

+   [释放聚类的威力：理解 K-Means 聚类](https://www.kdnuggets.com/2023/07/clustering-unleashed-understanding-kmeans-clustering.html)

+   [5 种将非结构化数据转换为结构化见解的方法（使用 LLMs）](https://www.kdnuggets.com/5-ways-of-converting-unstructured-data-into-structured-insights-with-llms)

+   [快速数据科学技巧和窍门：学习 SAS](https://www.kdnuggets.com/2022/05/sas-quick-data-science-tips-tricks-learn.html)

+   [7 个 Pandas 绘图函数用于快速数据可视化](https://www.kdnuggets.com/7-pandas-plotting-functions-for-quick-data-visualization)

+   [快速指南：如何找到合适的标注人员](https://www.kdnuggets.com/2022/04/quick-guide-find-right-minds-annotation.html)

+   [Voronoi 图的快速概述](https://www.kdnuggets.com/2022/11/quick-overview-voronoi-diagrams.html)
