# 机器学习分类：基于数据集的图解

> 原文：[`www.kdnuggets.com/2018/11/machine-learning-classification-dataset-based-pictorial.html`](https://www.kdnuggets.com/2018/11/machine-learning-classification-dataset-based-pictorial.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

机器学习中的分类概念涉及构建一个将数据分为不同类别的模型。通过输入一组预标记类别的训练数据来构建该模型，以便算法从中学习。然后，通过输入一个类别未标记的不同数据集来使用该模型，让模型基于从训练集中学到的知识来预测其类别。著名的分类方案包括决策树和支持向量机等。由于这种类型的算法需要明确的类别标记，因此分类是一种有监督学习形式。

从概念上讲，这非常直观且易于理解。但初学者可能会问这在实际生活中是如何运作的。为了将机器学习分类与实际情况联系起来，让我们一步步地看一下这个概念如何展开，具体涉及一个数据集，从一个常见的以逗号分隔值（CSV）文件——一个常用的存储和输入数据到机器学习系统的方式——到一个可以用来进行预测的模型。

在我们的练习中，我们将做以下假设：

+   我们将使用经过时间考验的[**成人数据集**](https://archive.ics.uci.edu/ml/datasets/adult)作为我们的示例。

+   我们将省略讨论数据集的任何预处理细节。

+   因此，我们将忽略数据集中类别特征的存在，这些特征在实际中需要转换为数值表示。

+   我们还将忽略空值的存在，这在实际中也需要处理。

+   最后，我们假设我们对数据集的常规训练/测试拆分感兴趣（而不是某些保留方法如交叉验证）。

我们先来看一下原始数据集，你可以在图 1 中找到。这包括完成机器学习任务所需的所有数据。我们只取了 CSV 文件的前 25 行作为示例。

请注意，你可以点击所有图片以放大查看。

[](https://image.ibb.co/g1LsJL/supervised-ds-illustrated-1.png)

![图](https://image.ibb.co/g1LsJL/supervised-ds-illustrated-1.png)

**图 1：原始成人数据集。**

我们有一组特征变量，或称为**特征**，用于描述某个观察实例。行是观察实例，而列是特征。这适用于所有列，除了最右侧的列，这就是我们的**目标**，一个类别集合，我们将尝试通过其相关特征值进行预测——这就是分类的本质。监督学习的“其他”类别，回归，在概念上几乎相同；唯一的区别是，尽管预测旨在学习如何预测有限的分类值集合，但回归旨在预测连续的数值。

图 2 显示了我们的数据集被分为特征在红线的左侧，目标在右侧。

[](https://image.ibb.co/h5NTsf/supervised-ds-illustrated-2.png)

![图](https://image.ibb.co/h5NTsf/supervised-ds-illustrated-2.png)

**图 2：特征在红线的左侧，目标在右侧。**

特定实例的特征被组合成一个**特征向量**，其示例在**图 3**中展示。

[](https://image.ibb.co/nsQgCf/supervised-ds-illustrated-3.png)

![图](https://image.ibb.co/nsQgCf/supervised-ds-illustrated-3.png)

**图 3：在完整数据集背景下显示的特征向量。**

一个**实例**由特征向量和对应的目标组成，如**图 4**所示。

[](https://image.ibb.co/c0p8sf/supervised-ds-illustrated-4.png)

![图](https://image.ibb.co/c0p8sf/supervised-ds-illustrated-4.png)

**图 4：一个实例（或观察）包括一个特征向量和一个对应的目标。**

现在我们已经定义了完整数据集的构成，我们需要做一些决定。由于我们最终希望将数据用于建模，并利用学到的知识对其他数据进行分类（而不是用于构建模型的相同数据），我们需要将数据集分为**训练**数据集和**测试**数据集。这通常通过在某个点拆分数据集来完成，拆分点由希望用于训练的数据集的百分比表示。在这个例子中，我们将使用 20 个数据实例进行训练（80%，这种拆分通常是合理的范围），剩余的 5 个数据实例用于测试我们所学到的知识。这个拆分可以在**图 5**中看到。

[](https://image.ibb.co/jifRdL/supervised-ds-illustrated-5.png)

![图](https://image.ibb.co/jifRdL/supervised-ds-illustrated-5.png)

**图 5：将我们的数据集拆分为训练集（黄线以上）和测试集（黄线以下）。**

此时，我们对单一实体数据集的了解足够多，可以将其切分为对我们的机器学习算法有用的部分。我们需要在训练和测试集中分离特征和目标（我们将忽略确保实例**随机**分割的细节）。

假设我们使用的是 Python 生态系统（我对此很熟悉），这样的分割可以通过如[Scikit-learn 的`train_test_split`函数](http://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html)等工具轻松实现，下面展示了一个示例：

```py
X_train, X_test, y_train, y_test = train_test_split(
    full_data[:,:-1],
    full_data[:,-1:],
    test_size=0.2,
    random_state=42)
```

调用后，我们得到：

+   训练特征矩阵 (**X_train**)，左上角（如图 6 所示）

+   训练目标向量 (**y_train**)，右上角

+   测试特征矩阵 (**X_test**)，左下角

+   测试目标向量 (**y_test**)，右下角

[](https://image.ibb.co/nx76dL/supervised-ds-illustrated-6.png)

![图](https://image.ibb.co/nx76dL/supervised-ds-illustrated-6.png)

**图 6：我们的训练和测试分割，以及特征和目标的分离。**

现在我们需要学习训练集中特征到目标的映射，以便将这个映射应用于我们的测试数据，以查看我们的模型的准确性。这一学习过程的结果，如图 7 所示，是一个可以随后使用的函数。这就是监督机器学习的本质：输入标记数据实例，学习这样一个**函数**，然后将该函数应用于未标记的数据（或故意隐瞒标签的数据）。

[](https://image.ibb.co/bJEmdL/supervised-ds-illustrated-7.png)

![图](https://image.ibb.co/bJEmdL/supervised-ds-illustrated-7.png)

**图 7：从原始数据到有用的预测函数，建模过程中。**

训练后，函数可以应用于我们的测试集，并且可以根据测试实例中的特征做出**预测**（如图 8 所示）。

[](https://image.ibb.co/gxY1Cf/supervised-ds-illustrated-8.png)

![图](https://image.ibb.co/gxY1Cf/supervised-ds-illustrated-8.png)

**图 8：使用学习到的函数对测试集进行预测。**

最后一个主要步骤是通过准确性等指标来衡量我们模型的有效性（请注意，我们没有讨论测试集与验证集的区别）。在测试阶段，我们的预测目标将与测试集的真实（实际）目标进行比较，并记录下来。实际操作中，这将把新向量的元素（例如，**y_pred**）与现有**y_test**向量的元素进行比较。这显示了我们的模型的有效性，并为我们提供了一个基准，我们可以用它来比较未来使用其他算法学习的分类模型（函数），甚至使用相同算法但不同超参数设置的模型。

**相关：**

+   使用 Mitchell 范式的学习算法简明解释

+   数据科学难题，重温

+   机器学习算法：简明技术概述

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业生涯

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT 需求

* * *

### 更多相关主题

+   [分类的机器学习算法](https://www.kdnuggets.com/2022/03/machine-learning-algorithms-classification.html)

+   [使用 Scikit-learn 进行机器学习分类入门](https://www.kdnuggets.com/getting-started-with-scikit-learn-for-classification-in-machine-learning)

+   [分类问题的更多性能评估指标你……](https://www.kdnuggets.com/2020/04/performance-evaluation-metrics-classification.html)

+   [使用 PyCaret 进行二分类介绍](https://www.kdnuggets.com/2021/12/introduction-binary-classification-pycaret.html)

+   [用 HuggingFace 微调 BERT 进行推文分类](https://www.kdnuggets.com/2022/01/finetuning-bert-tweets-classification-ft-hugging-face.html)

+   [分类的逻辑回归](https://www.kdnuggets.com/2022/04/logistic-regression-classification.html)
