# 从快速机器学习算法基准测试中学到的经验

> 原文：[https://www.kdnuggets.com/2017/08/lessons-benchmarking-fast-machine-learning-algorithms.html](https://www.kdnuggets.com/2017/08/lessons-benchmarking-fast-machine-learning-algorithms.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png)[评论](#comments)

**由微软的Miguel Fierro、Mathew Salvaris、Guolin Ke和Tao Wu撰写**。

根据[KDnuggets](/2016/03/xgboost-implementing-winningest-kaggle-algorithm-spark-flink.html)，提升决策树在Kaggle主办的机器学习挑战中占据了超过一半的获胜解决方案。除了卓越的性能，这些算法在实际应用中也具有吸引力，因为它们只需最少的调整。在这篇文章中，我们评估了两个流行的树提升软件包： [XGBoost](https://github.com/dmlc/xgboost) 和 [LightGBM](https://github.com/Microsoft/LightGBM)，包括它们的GPU实现。我们基于六个数据集的测试结果总结如下：

1.  XGBoost和LightGBM实现了类似的准确率指标。

1.  在所有测试数据集上，无论是CPU还是GPU实现，LightGBM的训练时间都低于XGBoost及其基于直方图的变体XGBoost hist。两个库之间的训练时间差异取决于数据集，可能大到25倍。

1.  XGBoost GPU实现对大数据集的扩展性较差，在一半的测试中内存不足。

1.  当特征维度较高时，XGBoost hist可能显著慢于原始的XGBoost。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的IT

* * *

我们的所有代码都是开源的，可以在[this repo](https://github.com/Azure/fast_retraining/)中找到。我们将解释这些库背后的算法，并在不同的数据集上进行评估。你喜欢你的机器学习快速吗？那就继续阅读吧。

### 提升决策树的基础

[梯度提升](https://en.wikipedia.org/wiki/Gradient_boosting)是一种机器学习技术，它生成一个以弱分类器的集合形式存在的预测模型，优化可微分的损失函数。最受欢迎的梯度提升类型之一是提升决策树，它内部由一组弱[决策树](https://en.wikipedia.org/wiki/Decision_tree_learning)组成。计算树的方法有两种不同策略：按层次和按叶节点。按层次策略逐层生长树。在这种策略中，每个节点优先分裂接近树根的节点。按叶节点策略通过在具有最高损失变化的节点处分裂数据来生长树。按层次生长通常适用于较小的数据集，而按叶节点生长则容易过拟合。按叶节点生长在[大数据集上往往表现优异](http://researchcommons.waikato.ac.nz/handle/10289/2317)，其速度远快于按层次生长。

![](../Images/12f36bb713456993f64393f64e51e30e.png)

训练提升决策树的一个关键挑战是每个叶节点的[寻找最佳分裂的计算成本](https://arxiv.org/abs/1706.08359)。传统技术找到每个叶节点的[准确分裂](https://arxiv.org/abs/1603.02754)，并需要在每次迭代中扫描所有数据。另一种方法通过构建特征的直方图来[近似分裂](https://arxiv.org/abs/1611.01276)。这样，算法无需评估每个特征的所有值来计算分裂，而只需评估直方图的区间，这些区间是有限的。这种方法对大数据集效率更高，同时不影响准确性。

[XGBoost 于2014年开始](http://homes.cs.washington.edu/~tqchen/2016/03/10/story-and-lessons-behind-the-evolution-of-xgboost.html)，由于在许多[获胜的 Kaggle 竞赛中](https://github.com/dmlc/xgboost/blob/master/demo/README.md)的应用而变得流行。最初，XGBoost 基于按层次生长算法，但最近增加了一个叶节点生长选项，利用直方图实现分裂近似。我们称这一版本为 XGBoost hist。LightGBM 是一个较新的工具，始于2016年3月，并在2016年8月开源。它基于叶节点算法和直方图近似，由于其速度引起了广泛关注（免责声明：这篇博客文章的合著者 Guolin Ke 是 LightGBM 的主要贡献者）。除了多线程 CPU 实现外，GPU 加速现在也在[XGBoost](https://github.com/dmlc/xgboost/tree/master/plugin/updater_gpu)和[LightGBM](https://github.com/Microsoft/LightGBM/blob/master/docs/GPU-Tutorial.md)上可用。

### 评估 XGBoost 和 LightGBM

我们在六个不同的数据集上进行了[machine learning experiments](https://github.com/Azure/fast_retraining/)。这些实验的代码在我们的[github repo](https://github.com/Azure/fast_retraining/)中的python notebooks里。我们还展示了我们使用的具体[XGBoost和LightGBM的编译版本](https://github.com/Azure/fast_retraining/blob/master/INSTALL.md)，并提供了[安装它们和设置实验的步骤](https://github.com/Azure/fast_retraining/blob/master/INSTALL.md)。我们尝试了使用CPU和GPU的分类和回归问题。所有实验都在Azure NV24虚拟机上运行，该虚拟机配备了24个核心、224 GB内存和NVIDIA M60 GPU。操作系统为Ubuntu 16.04。在所有实验中，我们发现XGBoost和LightGBM的准确率指标相似（此处显示F1分数），因此我们在这篇博客中重点关注了训练时间。下表显示了两种库在CPU和GPU实现中的训练时间以及训练时间比率。

![](../Images/7669a13d7f013ffb9c8711007bf4bfa1.png)

### 从基准测试中的学习

正如常说的，没有任何基准是绝对真实的，但有些基准是有用的。从我们的实验中，我们发现叶节点实现通常比层级实现更快。然而，BCI和Planet Kaggle数据集的CPU结果以及BCI的GPU结果显示，XGBoost hist的时间比标准XGBoost长得多。这是由于数据集的巨大规模以及大量的特征，这给XGBoost hist带来了相当大的内存开销。

我们还发现XGBoost的GPU实现扩展性不好，对于三个较大的数据集出现了内存溢出错误。此外，我们不得不在航空公司数据集的XGBoost训练运行了5小时后终止。

最后，在LightGBM和XGBoost之间，我们发现LightGBM在所有XGBoost和XGBoost hist完成的测试中都更快，其中XGBoost的最大差异为25倍，XGBoost hist为15倍。

我们想要研究不同数据大小和轮数对CPU与GPU性能的影响。在下表中，我们展示了使用航空公司数据集子样本的结果。比较CPU和GPU训练时间时，我们发现当数据集较大且轮数较多时，LightGBM的GPU版本优于CPU版本。正如预期的那样，对于小数据集，将数据在RAM和GPU内存之间复制的额外IO开销掩盖了在GPU上运行计算的速度优势。在这里，我们没有观察到使用XGBoost hist在GPU上有任何性能提升。值得一提的是，XGBoost的标准实现（基于精确分裂而非直方图）与多核CPU相比也未能从GPU中获益，参见这篇[最近的论文](https://arxiv.org/abs/1706.08359)。

![](../Images/1fed194178d3ccb9ca962b4b9dc785ba.png)

总的来说，我们发现 LightGBM 在 CPU 和 GPU 实现中都比 XGBoost 更快。此外，如果使用 XGBoost，我们建议密切关注特征维度和内存消耗。LightGBM 显著的速度优势意味着可以进行更多迭代和/或更快的超参数搜索，如果你在优化模型时有时间限制，或希望尝试不同的特征工程思路，这将非常有用。

编程愉快！

米格尔、马修、郭林和陶。

*致谢：我们感谢微软的斯蒂夫·邓、戴维·史密斯和胡塞因·伊尔迪兹对本文的帮助和反馈。*

[原文](https://blogs.technet.microsoft.com/machinelearning/2017/07/25/lessons-learned-benchmarking-fast-machine-learning-algorithms/)。经授权转载。

**简介：** 米格尔·费罗，数据科学家；马修·萨尔瓦里斯，数据科学家；郭林·科，副研究员；陶吴，首席数据科学经理，均在微软工作。

**相关内容：**

+   [使用鸢尾花数据集的简单 XGBoost 教程](/2017/03/simple-xgboost-tutorial-iris-dataset.html)

+   [Dask 和 Pandas 与 XGBoost：在分布式系统中良好协作](/2017/04/dask-pandas-xgboost-playing-nicely-distributed-systems.html)

+   [决策树分类器：简明技术概述](/2016/10/decision-trees-concise-technical-overview.html)

### 更多相关内容

+   [文本分类任务的最佳架构：基准测试…](https://www.kdnuggets.com/2023/04/best-architecture-text-classification-task-benchmarking-options.html)

+   [KDnuggets 新闻，9月28日：免费的 Python 算法课程 •…](https://www.kdnuggets.com/2022/n38.html)

+   [使用快速克里金法（FKR）加速机器学习](https://www.kdnuggets.com/2022/06/vmc-speed-machine-learning-fast-kriging.html)

+   [机器学习项目的简单快速数据流处理](https://www.kdnuggets.com/2022/11/simple-fast-data-streaming-machine-learning-projects.html)

+   [实用深度学习课程来自 fast.ai 重磅回归！](https://www.kdnuggets.com/2022/07/practical-deep-learning-fastai-2022.html)

+   [我从使用 ChatGPT 进行数据科学中学到的](https://www.kdnuggets.com/what-i-learned-from-using-chatgpt-for-data-science)
