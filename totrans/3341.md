# 改善预测的模型堆叠

> 原文：[https://www.kdnuggets.com/2017/02/stacking-models-imropved-predictions.html](https://www.kdnuggets.com/2017/02/stacking-models-imropved-predictions.html)

**作者：Burak Himmetoglu，加州大学圣巴巴拉分校。**

如果你曾经参加过 Kaggle 比赛，你可能会对结合不同预测模型以提高准确性并提升排行榜上的分数有所了解。尽管这种方法被广泛使用，但我知道的只有少数资源提供了清晰的描述（其中之一可以在[这里](http://mlwave.com/kaggle-ensembling-guide/)找到，还有一个[caret 包扩展](https://cran.r-project.org/web/packages/caretEnsemble/vignettes/caretEnsemble-intro.html)）。因此，我将尝试在这里提供一个简单的示例，以说明如何结合不同的模型。我选择的示例是 Kaggle 上的[房价预测](https://www.kaggle.com/c/house-prices-advanced-regression-techniques)竞赛。这是一个回归问题，给定关于房子的许多特征，需要在测试集上预测其价格。我将使用三种不同的回归方法（XGBoost、神经网络和支持向量回归）来创建预测，并将它们堆叠起来以生成最终预测。我假设读者对 R、Xgboost 和 caret 包以及支持向量回归和神经网络有所了解。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能。

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 在 IT 领域支持你的组织。

* * *

通过结合不同模型来构建预测模型的主要思想可以用下图示意：

![workflow](../Images/3af5043b368b3b859dfdfb58d330a072.png)

让我描述图中的关键点：

+   初始训练数据 (**X**) 具有 *m* 个观测值和 *n* 个特征（所以它是 *m x n*）。

+   有 M 个不同的模型，这些模型在 X 上进行训练（通过某种训练方法，如交叉验证）。

+   每个模型提供结果（y）的预测，然后将这些预测转化为第二级训练数据（Xl2），现在是 *m x M*。即，M 个预测成为这个第二级数据的特征。

+   然后，可以在这些数据上训练一个第二级模型（或多个模型），以生成最终结果，这些结果将用于预测。

第二层数据（Xl2）的构建有几种方式。在这里，我将讨论**堆叠**，它对于小型或中型数据集效果很好。堆叠使用类似于k折交叉验证的想法来创建**样本外**预测。

这里的关键词是**样本外**，因为如果我们使用所有训练数据拟合的M模型的预测，那么第二层模型将会偏向于M模型中表现最好的模型。这是没有用的。

作为这一点的说明，假设**模型1**在训练数据上的训练准确率低于**模型2**。然而，可能有些数据点**模型1**表现更好，但由于某些原因在其他数据点上表现极差（见下图）。相反，**模型2**在所有数据点上的总体表现可能更好，但在**模型1**表现较好的数据点上表现更差。这个想法是结合这两个模型在它们表现最好的地方。这就是为什么创建样本外预测更有可能捕捉到每个模型表现最好的不同区域。

![模型](../Images/5176f9842aa4d78e0ac86d823a58a174.png)

首先，让我描述一下我所说的堆叠。这个想法是将训练集分成几个部分，就像在k折交叉验证中一样。对于每个折叠，使用其余的折叠来获取所有模型1…M的预测。下面的图表最能解释这一点：

![堆叠](../Images/b485907a53455f6ecfe2584bd46e091b.png)

在这里，我们将训练数据分成N个折叠，并将第N个折叠保留用于验证（即保留折叠）。假设我们有M个模型（稍后我们将使用M=3）。如图所示，每个折叠（Fj）的预测是通过使用其余折叠进行拟合得到的，并收集在样本外预测矩阵（Xoos）中。即，第二层训练数据Xl2就是Xoos。这对于每个模型都重复进行。样本外预测矩阵（Xoos）将用于第二层训练（通过某种选择的方法）以获得所有数据点的最终预测。需要注意几个要点：

+   我们没有简单地将所有训练数据中M模型的预测逐列堆叠以创建第二层训练数据，这是由于上述问题（即第二层训练将简单地选择M模型中表现最好的模型）。

+   通过使用样本外预测，我们仍然有大量的数据来训练第二层模型。我们只需在Xoos上进行训练，并在保留折叠（Nth）上进行预测。这与模型集成不同。

现在，每个模型（1...M）可以在（N-1）个折叠上进行训练，并对保留折叠（第N折）进行预测。这没有什么新鲜的。但我们所做的是，使用在Xoos上训练的第二层模型，我们将获得对保留数据的预测。我们希望**第二层训练的预测比原始模型中的每个M预测都要好**。如果没有，我们将不得不重新调整模型组合的方式。

让我用一个具体的例子来说明我刚才写的内容。以房价数据为例，我使用了10折的训练数据分割。前9折用于构建Xoos，第10折作为验证的保留数据。我训练了三个一级模型：XGBoost、神经网络、支持向量回归。对于第二层，我使用了线性弹性网模型（即LASSO + 岭回归）。以下是每个模型在保留折叠上的均方根误差（RMSE）：

```py
 XGBoost:  0.10380062

  Neural Network:  0.10147352

  Support Vector Regression:  0.10726746

  Stacked:  0.10005465 
```

从这些数据中可以清楚地看到，堆叠模型的RMSE略低于其他模型。这看起来可能变化很小，但当涉及到Kaggle的领导层时，这样的小差异非常重要！

![堆叠图](../Images/25c59f2fc6d8329b603e2437a4418cf3.png)

从图形上看，可以看到被圈出的数据点在XGBoost中预测效果较差（当在所有训练数据上训练时，它是最好的模型），但神经网络和支持向量回归对这个特定点表现得更好。在堆叠模型中，该数据点被放置在接近神经网络和支持向量回归的位置。当然，你也可以看到一些情况下仅使用XGBoost的效果比堆叠要好（比如一些较低的点）。然而，堆叠模型的整体预测准确度更高。

一个最终的复杂情况，将进一步提升你的得分：如果你有额外的计算时间，你可以创建重复的堆叠。这将进一步减少你预测的方差（这有点像袋装法）。

例如，让我们创建一个10折的堆叠，不仅仅进行一次，而是10次！（通过caret的`createMultiFolds`函数）。这将为我们提供多个第二层预测，然后可以对这些预测进行平均。例如，以下是20个不同的随机10折创建的保留数据上的RMSE值（rmse1: XGBoost，rmse2: 神经网络，rmse3: 支持向量回归）。对这些Xoos（即堆叠1的Xoos、堆叠2的Xoos，…，堆叠10的Xoos）的第二层预测的最终预测进行平均，将进一步提高你的得分。

![表格](../Images/1c5f65587d7163510e8cb27031ccdfec.png)

一旦我们验证了堆叠模型的预测效果比每个单独的模型要好，那么我们将重新运行整个过程，而不保留第N折作为保留数据。我们从所有折叠中创建Xoos，然后第二层训练使用Xoos来预测Kaggle提供的测试集。希望这会让你的分数在排行榜上有所提升！

最后：你可以在我的 [Github 仓库](https://github.com/bhimmetoglu/kaggle_101/tree/master/HousePrices)中找到脚本。

注意：如果你有分类问题，你仍然可以使用相同的程序来堆叠类别概率。

**简介：[Burak Himmetoglu](https://burakhimmetoglu.com/)** 是数据科学家和高性能计算（HPC）专家，拥有物理学博士学位。他拥有扎实的数学建模、数据分析和编程背景，并热衷于将学术技能应用于解决复杂的业务问题和开发数据产品。

[原文](https://burakhimmetoglu.com/2016/12/01/stacking-models-for-improved-predictions/)。经许可转载。

**相关：**

+   [数据科学基础：集成学习者简介](/2016/11/data-science-basics-intro-ensemble-learners.html)

+   [Python 中的随机森林](/2016/12/random-forests-python.html)

+   [机器学习的顶级 R 包](/2017/02/top-r-packages-machine-learning.html)

### 更多相关内容

+   [神经网络预测中排列的重要性](https://www.kdnuggets.com/2022/12/importance-permutation-neural-network-predictions.html)

+   [预测制作：Python 中线性回归的初学者指南](https://www.kdnuggets.com/2023/06/making-predictions-beginner-guide-linear-regression-python.html)

+   [与未来对话：未来十年的 AI 预测](https://www.kdnuggets.com/2023/04/chatting-future-predictions-ai-next-decade.html)

+   [每位初学者数据科学家应该掌握的 6 种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [为什么机器学习模型在沉默中消亡？](https://www.kdnuggets.com/2022/01/machine-learning-models-die-silence.html)

+   [用 LIME 解释 NLP 模型](https://www.kdnuggets.com/2022/01/explain-nlp-models-lime.html)
