# Dask 中的机器学习

> 原文：[https://www.kdnuggets.com/2020/06/machine-learning-dask.html](https://www.kdnuggets.com/2020/06/machine-learning-dask.html)

[评论](#comments)

在个人电脑上处理几GB的数据通常是一个艰巨的任务，除非该电脑具有高 RAM 和大量计算能力。

尽管如此，数据科学家仍然需要寻找替代方案来处理这个问题。一些解决方法包括调整 Pandas 以使其能够处理大型数据集，购买 GPU 机器或在云端购买计算资源。在本文中，我们将看到如何使用[Dask](https://dask.org/)在本地机器上处理大数据集。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

### Dask 和 Python

Dask 是一个用于 Python 的灵活并行计算库。它被构建为能够很好地与其他开源项目（如 NumPy、Pandas 和 scikit-learn）集成。在 Dask 中，[Dask 数组](https://docs.dask.org/en/latest/array.html) 相当于 NumPy 数组，[Dask DataFrames](https://docs.dask.org/en/latest/dataframe.html) 相当于 Pandas DataFrames，而 [Dask-ML](https://ml.dask.org/) 相当于 scikit-learn。

这些相似性使得将 Dask 轻松地融入您的工作流程。使用 Dask 的优势在于您可以将计算扩展到计算机上的多个核心。这使得您能够处理内存无法容纳的大型数据集。它还帮助加快通常需要很长时间的计算。

![图示](../Images/e812ece84005237a2818fcfe66918ac8.png)

[来源](https://dask.org/)

### Dask DataFrames

当加载大量数据时，Dask 通常会读取数据的一个样本以推断数据类型。如果某一列的数据类型不同，这通常会导致问题。为了避免类型错误，通常的好做法是事先声明数据类型。Dask 通过将数据切分成块（由 `blocksize` 参数定义）来加载大型文件。

```py
data_types ={'column1': str,'column2': float}
df = dd.read_csv(“data,csv”,dtype = data_types,blocksize=64000000 )
```

![图示](../Images/5742054bfc128a1492d8afc31c6a87ec.png)

[来源](https://dask.org/)

Dask DataFrame 中的命令与 Pandas 中的命令大致相同。例如，获取 `head` 和 `tail` 是类似的：

```py
 df.head()
df.tail()
```

DataFrame 上的函数是懒惰执行的。这意味着它们不会被计算，直到调用 `compute` 函数。

```py
df.isnull().sum().compute()
```

由于数据是分区加载的，一些 Pandas 函数如 `sort_values()` 可能会失败。解决方法是使用 `nlargest()` 函数。

### Dask 集群

并行计算是 Dask 的关键，因为它允许在多个核心上运行计算。Dask 提供了一个在单台计算机上工作的机器调度器。它不具备扩展性。它还提供了一个可以扩展到多台计算机的分布式调度器。

使用 `dask.distributed` 需要你设置一个 `Client`。如果你打算在分析中使用 `dask.distributed`，这应该是你首先做的事情。它提供了低延迟、数据本地性、工作节点之间的数据共享，并且易于设置。

```py
from dask.distributed import Client
client = Client()
```

![](../Images/5c70485c72f6d940cbc7583df741811a.png)

即使在单台机器上使用 `dask.distributed` 也是有利的，因为它通过仪表板提供了一些诊断功能。

如果未声明 `Client`，默认将使用单机调度器。它通过使用进程或线程在单台计算机上提供并行性。

### Dask ML

Dask 还使你能够以并行方式进行机器学习训练和预测。`dask-ml` 的目标是提供可扩展的机器学习。当你在 scikit-learn 中声明 `n_jobs = -1` 时，你可以并行运行计算。Dask 利用这一能力来使你能够在集群中分配计算。这是通过 [joblib](https://joblib.readthedocs.io/en/latest/) 实现的，该包允许 Python 中的并行处理和管道化。使用 Dask ML，你可以实现 scikit-learn 模型以及其他库，如 XGboost。

这就是简单实现的样子。

首先像往常一样导入 `train_test_split`，用于将数据分成训练集和测试集。

```py
from dask_ml.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)
```

接下来，导入你想使用的模型并实例化它。

```py
from sklearn.ensemble import RandomForestClassifier
model = RandomForestClassifier(verbose=1)
```

然后你需要导入 `joblib` 以启用并行计算。

```py
import joblib
```

接下来，使用并行后端运行你的训练和预测。

```py
from sklearn.externals.joblib import parallel_backend
with parallel_backend(‘dask’):
 model.fit(X_train,y_train)
 predictions = model.predict(X_test)
```

> 机器学习正迅速接近数据采集地点——边缘设备。[订阅 Fritz AI 新闻通讯以了解更多关于这一过渡以及如何帮助扩展你的业务。](https://www.fritz.ai/newsletter?utm_campaign=fritzai-newsletter-scale6&utm_source=heartbeat)

### 限制和内存使用

Dask 中的单个任务不能并行运行。工作节点是继承了 Python 计算的所有优缺点的 Python 进程。在分布式环境中工作时，还应注意确保数据安全和隐私。

Dask 有一个中央调度器，用于跟踪工作节点和集群上的数据。这个调度器还控制从集群中释放数据。一旦任务完成，它会从内存中清除数据，以释放内存供其他任务使用。如果某个 `client` 需要某些数据，或者它对正在进行的计算很重要，它会保留在内存中。

另一个 Dask 的限制是它没有实现 Pandas 中的所有函数。Pandas 接口很庞大，因此 Dask 并未完全实现它。这意味着在 Dask 上尝试一些操作可能会比较困难。此外，Pandas 上慢的操作在 Dask 上也同样慢。

### 当你不需要 Dask DataFrame 时

在以下情况下，你可能不需要 Dask:

+   当有一些 Pandas 函数尚未在 Dask 中实现时。

+   当你的数据完全适合计算机的内存时。

+   当你的数据不是表格形式时。在这种情况下，可以尝试 [dask.bag](https://docs.dask.org/en/latest/bag.html) 或 [dask.array](https://docs.dask.org/en/latest/array.html)。

### 最后的思考

在这篇文章中，我们看到我们如何使用 Dask 在本地机器或以分布式方式处理巨大数据集。我们了解到，由于 Dask 的熟悉语法和扩展能力，我们可以使用 Dask。它能够扩展到数千个核心。

我们还看到我们可以在机器学习中使用它来训练和运行预测。你可以通过查看这些官方文档中的演示文稿来了解更多信息：

[**Dask 的演示文稿 - Dask 2.15.0 文档**](https://docs.dask.org/en/latest/presentations.html)

**简历: [德里克·穆伊提](https://derrickmwiti.com/)** 是一位数据分析师、作家和导师。他致力于在每项任务中取得卓越成果，并且是 Lapid Leaders Africa 的导师。

[原文](https://heartbeat.fritz.ai/machine-learning-in-dask-e234c98ef07)。已获许可转载。

**相关:**

+   [为什么以及如何在大数据中使用 Dask](/2020/04/dask-big-data.html)

+   [五个有趣的数据工程项目](/2020/03/data-engineering-projects.html)

+   [Python 中的自动化机器学习](/2019/01/automated-machine-learning-python.html)

### 相关阅读

+   [KDnuggets 新闻，12月14日: 3 门免费的机器学习课程…](https://www.kdnuggets.com/2022/n48.html)

+   [每位机器学习工程师都应该掌握的 5 项机器学习技能…](https://www.kdnuggets.com/2023/03/5-machine-learning-skills-every-machine-learning-engineer-know-2023.html)

+   [学习数据科学、机器学习和深度学习的稳固计划](https://www.kdnuggets.com/2023/01/mwiti-solid-plan-learning-data-science-machine-learning-deep-learning.html)

+   [人工智能、分析、机器学习、数据科学、深度学习…](https://www.kdnuggets.com/2021/12/developments-predictions-ai-machine-learning-data-science-research.html)

+   [打破数据障碍: 如何通过零样本、单样本和少样本…](https://www.kdnuggets.com/2023/08/breaking-data-barrier-zeroshot-oneshot-fewshot-learning-transforming-machine-learning.html)

+   [联邦学习: 与教程一起的协作机器学习…](https://www.kdnuggets.com/2021/12/federated-learning-collaborative-machine-learning-tutorial-get-started.html)
