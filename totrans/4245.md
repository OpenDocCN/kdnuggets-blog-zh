# Pandas 不够用？这里有几个处理更大、更快数据的 Python 替代方案。

> 原文：[https://www.kdnuggets.com/2021/07/pandas-alternatives-processing-larger-faster-data-python.html](https://www.kdnuggets.com/2021/07/pandas-alternatives-processing-larger-faster-data-python.html)

[评论](#comments)

**由 [DaurEd](https://daureducation.medium.com/) 提供优质且负担得起的教育**。

![](../Images/d24ce366a64c6254e5b2a7776109a476.png)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能。

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作。

* * *

数据以各种格式存在于各处，如 CSV、平面文件、JSON 等。当数据量较大时，很难读取到内存中，并且进行探索性数据分析（EDA）耗时较长。此博客围绕以 *CSV 格式* 处理表格数据，并使用 *Pandas 以及 cuDF、dask、modin 和 datatable 等一些替代方案*。

> **问题：** 导入（读取）大型 CSV 文件会导致 *内存不足* 错误。RAM 不足以一次读取整个 CSV 文件，经常导致计算机崩溃。而且，使用单个 CPU 核心处理时速度较慢。

关于本次探索中使用的数据：一个包含 500 万行和 14 列的销售数据记录，如下所示。此数据集以 zip 格式提供，下载地址为：

+   [大型随机生成的 CSV 格式销售数据集](http://eforexcel.com/wp/downloads-18-sample-csv-files-data-sets-for-testing-sales/)

数据的前 5 行：

![](../Images/4f2b4bb786e1b0ded727a4008a1fd6be.png)

Pandas 设计为 *仅在单个核心上运行*，因此无法利用系统上可用的多核。

然而，[**cuDF**](https://docs.rapids.ai/api/cudf/stable/) 库旨在在 GPU 上实现 Pandas API。*[**Modin**](https://modin.readthedocs.io/en/latest/)* 和 [**Dask Dataframe**](https://docs.dask.org/en/latest/dataframe.html) 库提供了围绕 Pandas API 的 *并行算法*。

Modin 旨在对整个 pandas API 进行并行化，没有例外。这意味着所有 pandas 函数都可以在 Modin 上使用，只需更改一行代码：*import modin.pandas as pd*（查看下面的 Git 链接了解实现细节）。

Dask 目前缺少 Modin 已实现的多个 pandas API。

要了解如何通过参数 *num_cpus* 控制 Modin 使用的处理器数量，请查看：

[https://github.com/Piyush-Kulkarni/ByeByePandas.git](https://github.com/Piyush-Kulkarni/ByeByePandas.git)

在 Windows 平台上，您可以在任务管理器 > 性能中检查系统的核心数。

![](../Images/246f50035976fc036dc9310ea87cb0fa.png)

使用*num_cpu*超出系统可用核心数不会提升性能。实际上，它可能会降低性能。在我的案例中，超出系统提供的6个核心后，性能略有下降，如上述图表所示。

[原文](https://daureducation.medium.com/bye-bye-pandas-3808348c48f1)。已获许可转载。

**相关：**

+   [Pandas 与 SQL：数据科学家应如何选择工具](https://www.kdnuggets.com/2021/06/pandas-vs-sql.html)

+   [使用 PyPolars 使 Pandas 快速 3 倍](https://www.kdnuggets.com/2021/05/pandas-faster-pypolars.html)

+   [Vaex：Pandas 的 1000 倍速度](https://www.kdnuggets.com/2021/05/vaex-pandas-1000x-faster.html)

### 更多相关话题

+   [使用管道编写干净的 Python 代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [建立一个稳固的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)

+   [打破数据障碍：零样本、一样本和少样本…](https://www.kdnuggets.com/2023/08/breaking-data-barrier-zeroshot-oneshot-fewshot-learning-transforming-machine-learning.html)

+   [了解如何在您的设备上仅需几步运行 Alpaca-LoRA](https://www.kdnuggets.com/2023/05/learn-run-alpacalora-device-steps.html)

+   [确保 LLMs 的可靠少样本提示选择](https://www.kdnuggets.com/2023/07/ensuring-reliable-fewshot-prompt-selection-llms.html)

+   [人工智能并非来取代我们](https://www.kdnuggets.com/2023/02/ai-replace-us.html)
