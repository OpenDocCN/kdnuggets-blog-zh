# 使用 PyPolars 使 Pandas 的速度提升 3 倍

> 原文：[https://www.kdnuggets.com/2021/05/pandas-faster-pypolars.html](https://www.kdnuggets.com/2021/05/pandas-faster-pypolars.html)

[评论](#comments)

**由 [Satyam Kumar](https://www.linkedin.com/in/satkr/)，机器学习爱好者和程序员**

![](../Images/d7e6cfa674476abb5778d3a3291c484b.png)

照片由 [Tim Gouw](https://unsplash.com/@punttim?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Pandas 是数据科学家处理数据时最重要的 Python 包之一。Pandas 库主要用于数据探索和可视化，因为它提供了大量内置函数。由于 Pandas 无法处理大规模数据集，因为它无法扩展或将处理过程分配到 CPU 的所有核心。

为了加快计算速度，可以利用 CPU 的所有核心来提升工作流程的速度。包括 Dask、Vaex、Modin、Pandarallel、PyPolars 等在内的各种开源库可以将计算并行化到 CPU 的多个核心。在这篇文章中，我们将讨论 PyPolars 库的实现和使用，并将其性能与 Pandas 库进行比较。

### PyPolars 是什么？

PyPolars 是一个开源的 Python 数据框架库，类似于 Pandas。PyPolars 利用 CPU 的所有可用核心，因此比 Pandas 更快地执行计算。PyPolars 的 API 与 Pandas 相似。它是用 Rust 编写的，并具有 Python 包装器。

> 理想情况下，当数据过大以至于无法使用 Pandas 处理，但又小到不需要使用 Spark 时，应使用 PyPolars。

### PyPolars 如何工作？

PyPolars 库有两个 API，一个是 Eager API，另一个是 Lazy API。Eager API 非常类似于 Pandas，结果在执行完成后立即产生。Lazy API 则类似于 Spark，当执行查询时会形成一个映射或计划，然后在 CPU 的所有核心上并行执行。

![](../Images/0230c45ae7afe40f0abe487235ee4ba1.png)

（图片由作者提供），PyPolars API

PyPolars 基本上是 Polars 库的 Python 绑定。PyPolars 库的最佳部分是其与 Pandas 的 API 相似性，这使得开发者更容易上手。

### 安装：

可以使用以下命令从 PyPl 安装 PyPolars：

```py
**pip install py-polars**
```

并使用以下命令导入库

```py
**import pypolars as pl**
```

### 基准时间限制：

> 在演示中，我使用了一个大规模数据集（约 6.4GB），包含 2500 万个实例。

![](../Images/016819891b0567cb7b42212697294dd6.png)

（图片由作者提供），Pandas 和 Py-Polars 基本操作的基准时间数量

对于使用 Pandas 和 PyPolars 库的一些基本操作的基准时间数据，我们可以观察到 PyPolars 的速度几乎比 Pandas 快 2 到 3 倍。

现在我们知道 PyPolars 的 API 非常类似于 Pandas，但仍然未覆盖 Pandas 的所有功能。例如，PyPolars 中没有 `**.describe()**` 函数，而是可以使用 `**df_pypolars.to_pandas().describe()**`

### 用法：

(作者提供的代码)

### 结论：

在本文中，我们简要介绍了 PyPolars 库，包括其实现、用法，并将其基准时间与 Pandas 进行了一些基本操作的比较。请注意，PyPolars 的工作方式与 Pandas 非常相似，且 PyPolars 是一个内存高效的库，因为它的内存是不可变的。

可以查看 [文档](https://github.com/ritchie46/polars) 以详细了解该库。还有各种其他开源库可以并行化 Pandas 操作，加速过程。阅读 [下文提到的文章](https://towardsdatascience.com/4-libraries-that-can-parallelize-the-existing-pandas-ecosystem-f46e5a257809) 以了解 4 个这样的库：

[**4 个可以并行化现有 Pandas 生态系统的库**](http://towardsdatascience.com)

通过使用这些框架来进行并行处理以分配 Python 工作负载

**参考文献：**

[1] Polars 文档和 GitHub 存储库：[https://github.com/ritchie46/polars](https://github.com/ritchie46/polars)

**感谢阅读**

**简介： [Satyam Kumar](https://www.linkedin.com/in/satkr/)** 是一位机器学习爱好者和程序员。Satyam [撰写](https://satyam-kumar.medium.com/) 关于数据科学的文章，并且是 AI 的顶级写手。他正在寻求一个具有挑战性的职业机会，以发挥他的技术技能和能力。

[原文](https://towardsdatascience.com/3x-times-faster-pandas-with-pypolars-7550e605805e)。经授权转载。

**相关：**

+   [Vaex: Pandas 但快 1000 倍](/2021/05/vaex-pandas-1000x-faster.html)

+   [如何用 Modin 加速 Pandas](/2021/03/speed-up-pandas-modin.html)

+   [如何处理机器学习中的分类数据](/2021/05/deal-with-categorical-data-machine-learning.html)

* * *

## 我们的 3 大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT

* * *

### 更多相关话题

+   [如何优化 Dockerfile 指令以加快构建时间](https://www.kdnuggets.com/how-to-optimize-dockerfile-instructions-for-faster-build-times)

+   [最简单的方法使用 Pandas 制作美观的交互式可视化](https://www.kdnuggets.com/2021/12/easiest-way-make-beautiful-interactive-visualizations-pandas.html)

+   [通过参与竞赛将机器学习的速度提高 4 倍](https://www.kdnuggets.com/2022/01/learn-machine-learning-4x-faster-participating-competitions.html)

+   [oBERT：复合稀疏化实现更快、更准确的 NLP 模型](https://www.kdnuggets.com/2022/05/obert-compound-sparsification-delivers-faster-accurate-models-nlp.html)

+   [使用 AI 和分析引擎更快速地准备时间序列数据](https://www.kdnuggets.com/2021/12/piexchange-faster-way-prepare-timeseries-data-ai-analytics-engine.html)

+   [通过 DataCamp 的分析师接管更快成为数据驱动](https://www.kdnuggets.com/2022/10/datacamp-data-driven-faster-analyst-takeover.html)
