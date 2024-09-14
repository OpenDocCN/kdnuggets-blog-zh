# TensorFlow 中的梯度提升与 XGBoost

> 原文：[`www.kdnuggets.com/2018/01/gradient-boosting-tensorflow-vs-xgboost.html`](https://www.kdnuggets.com/2018/01/gradient-boosting-tensorflow-vs-xgboost.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**由 [Nicolò Valigi](https://www.linkedin.com/in/nicolovaligi/)，AI Academy 创始人**

TensorFlow 1.4 最近几周发布了，带有梯度提升的实现，称为 **TensorFlow Boosted Trees (TFBT)**。不幸的是，[论文](https://arxiv.org/abs/1710.11555) 中没有任何基准测试，因此我对 XGBoost 进行了一些测试。

对于许多 Kaggle 风格的数据挖掘问题，自 2016 年发布以来，XGBoost 一直是首选解决方案。它今天可能是最接近即插即用的机器学习算法的，因为它优雅地处理未规范化或缺失的数据，同时准确且训练速度快。

本文中重现结果的代码可以在 [GitHub](https://github.com/nicolov/gradient_boosting_tensorflow_xgboost) 上找到。

### 实验

我想要一个足够大的数据集来测试这两种解决方案的可扩展性，因此我选择了 [这里](http://stat-computing.org/dataexpo/2009/) 提供的 **航空公司数据集**。该数据集包含大约 1.2 亿个数据点，涵盖了从 1987 年到 2008 年期间的所有美国商业航班。特征包括起始和目的地机场、起飞日期和时间、航空公司以及飞行距离。我设定了一个简单的二分类任务，试图预测航班是否会延误超过 15 分钟。

我从 2006 年抽取了 10 万个航班作为训练集，从 2007 年抽取了 10 万个航班作为测试集。遗憾的是，大约 20% 的航班延迟超过了 15 分钟，这一事实对航空公司行业并不好 :D。很容易看出，全天的起飞时间与延迟的可能性之间的相关性有多强：

![](img/5441b90ecfb58bced314c9b7eb110c9a.png)

我没有进行任何特征工程，因此特征列表非常基础：

```py
Month
DayOfWeek
Distance
CRSDepTime
UniqueCarrier
Origin
Dest

```

我使用了用于 XGBoost 的 scikit 风格的封装，它使得从 NumPy 数组中进行训练和预测变成了两行代码的事情（[代码](https://github.com/nicolov/gradient_boosting_tensorflow_xgboost/blob/master/do_xgboost.py)）。对于 TensorFlow，我使用了`tf.Experiment`、`tf.learn.runner`和 NumPy 输入函数，以节省一些样板代码（[代码](https://github.com/nicolov/gradient_boosting_tensorflow_xgboost/blob/master/do_tensorflow.py)）。TODO

### 结果

我从 XGBoost 开始，并对超参数做了合理的猜测，立刻得到了一个我满意的 [AUC 评分](https://stats.stackexchange.com/questions/132777/what-does-auc-stand-for-and-what-is-it)。当我尝试在 TensorFlow Boosted Trees 上使用相同的设置时，我甚至没有耐心等训练结束！

尽管我在两个模型中都保持了 `num_trees=50` 和 `learning_rate=0.1`，但最终我不得不使用持出集来调整 TF Boosted Trees 的 `examples_per_layer` 旋钮。这可能与 TFBT 论文中提出的新颖 *逐层* 学习算法有关，但我还没有深入研究。作为比较的起点，我选择了两个值（1k 和 5k），它们的训练时间和准确率与 XGBoost 相似。以下是结果：

![](img/edf8da04192ca1cbff995991242c0161.png)

![](img/7acb4573030b1a09dddcca330f037844.png)

准确率数据：

```py
                   Model  AUC score
-----------------------------------
                 XGBoost       67.6
TensorFlow (1k ex/layer)       62.1
TensorFlow (5k ex/layer)       66.1

```

训练运行时间：

```py
./do_xgboost.py --num_trees=50
42.06s user 1.82s system 1727% cpu 2.540 total

./do_tensorflow.py --num_trees=50 --examples_per_layer=1000
124.12s user 27.50s system 374% cpu 40.456 total

./do_tensorflow.py --num_trees=50 --examples_per_layer=5000
659.74s user 188.80s system 356% cpu 3:58.30 total

```

显示的两个 TensorFlow 设置都无法匹配 XGBoost 的训练时间/准确率。除了在 `user` 时间（总 CPU 使用时间）上的劣势外，TensorFlow 在多个核心上的并行效果也似乎不佳，导致 `total`（即墙）时间也存在巨大差距。XGBoost 可以顺利使用我机器上的 32 个核心中的 16 个（当使用更多树时表现更好），而 TensorFlow 使用的核心不到 4 个。我猜测整个“分布式 TF”工具箱可以用来让 TFBT 更好地扩展，但为了充分利用一个 *单一* 服务器，这似乎有些过头了。

### 结论

经过几小时的调整，我无法让 TensorFlow 的 Boosted Trees 实现达到 XGBoost 的结果，无论是在训练时间还是准确率上。这立即使它不适合我用 XGBoost 进行的许多快速粗糙项目。实现的有限并行性也意味着它不能扩展到大数据集。

TensorFlow Boosted Trees 可能在已经大量投资于 TensorFlow 工具的基础设施中是有意义的。TensorBoard 和数据加载管道也是两个在 Boosted Trees 上运行良好的特性，可以轻松迁移自其他基于 TensorFlow 的深度学习项目。然而，直到实现能够匹配 XGBoost 的性能（或者我学会如何调优），TF Boosted Trees 在大多数情况下不会非常有用。

要重现我的结果，请获取 [GitHub 上的训练代码](https://github.com/nicolov/gradient_boosting_tensorflow_xgboost)。

**个人简介：[Nicolò Valigi](https://www.linkedin.com/in/nicolovaligi/)** 对推动机器学习的代码和基础设施充满热情。他创办了 AI Academy，一家帮助公司了解 AI 的咨询公司，并且是 Cruise Automation 的高级软件工程师，负责自动驾驶汽车平台。

[原文](https://nicolovaligi.com/gradient-boosting-tensorflow-xgboost.html)。经许可转载。

**相关：**

+   XGBoost：简明技术概述

+   从基准测试快速机器学习算法中获得的经验教训

+   一个简单的使用 Iris 数据集的 XGBoost 教程

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

### 更多相关话题

+   [如何加速 XGBoost 模型训练](https://www.kdnuggets.com/2021/12/speed-xgboost-model-training.html)

+   [提升机器学习算法：概述](https://www.kdnuggets.com/2022/07/boosting-machine-learning-algorithms-overview.html)

+   [XGBoost 的假设是什么？](https://www.kdnuggets.com/2022/08/assumptions-xgboost.html)

+   [调优 XGBoost 超参数](https://www.kdnuggets.com/2022/08/tuning-xgboost-hyperparameters.html)

+   [利用 XGBoost 进行时间序列预测](https://www.kdnuggets.com/2023/08/leveraging-xgboost-timeseries-forecasting.html)

+   [掌握季节性和提升业务成果的终极指南](https://www.kdnuggets.com/2023/08/media-mix-modeling-ultimate-guide-mastering-seasonality-boosting-business-results.html)
