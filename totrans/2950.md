# 如何（不）使用机器学习进行时间序列预测：避免陷阱

> 原文：[https://www.kdnuggets.com/2019/05/machine-learning-time-series-forecasting.html](https://www.kdnuggets.com/2019/05/machine-learning-time-series-forecasting.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者 [Vegard Flovik](https://www.linkedin.com/in/vegard-flovik/)**

在我的其他文章中，我已经涉及了诸如：[如何结合机器学习和物理学](https://towardsdatascience.com/how-do-you-combine-machine-learning-and-physics-based-modeling-3a3545d58ab9)以及[机器学习如何用于生产优化](https://towardsdatascience.com/machine-learning-for-production-optimization-e460a0b82237)和[异常检测与状态监测](https://www.linkedin.com/pulse/how-use-machine-learning-anomaly-detection-condition-flovik-phd/)。但在这篇文章中，我将讨论机器学习在时间序列预测中的一些常见陷阱。

![](../Images/376ee98c1f8813944396a42ed0c44481.png)

时间序列预测是机器学习的一个重要领域。这一点非常重要，因为有许多预测问题涉及时间因素。然而，虽然时间因素增加了额外的信息，但也使得时间序列问题比许多其他预测任务更难处理。

本文将探讨使用机器学习进行[时间序列预测](https://en.wikipedia.org/wiki/Time_series)的任务，以及如何避免一些常见的陷阱。通过一个具体的例子，我将展示如何看似拥有一个良好的模型并决定将其投入生产，而实际上，该模型可能根本没有预测能力。更具体地说，我将重点关注如何评估模型的准确性，并展示如何仅依赖常见的错误指标，如[平均百分比误差](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)、[R2分数](https://en.wikipedia.org/wiki/Coefficient_of_determination)等，如果不加以小心应用，这些指标可能会非常误导。

### 时间序列预测的机器学习模型

有几种类型的模型可以用于时间序列预测。在这个具体的例子中，我使用了长短期记忆网络，简称[LSTM 网络](https://en.wikipedia.org/wiki/Long_short-term_memory)，这是一种特殊类型的神经网络，根据之前的时间数据进行预测。它在语言识别、时间序列分析等方面非常流行。然而，根据我的经验，简单类型的模型在许多情况下实际上能提供同样准确的预测。使用例如[random forest](https://en.wikipedia.org/wiki/Random_forest)、[gradient boosting regressor](https://en.wikipedia.org/wiki/Gradient_boosting)和[time delay neural networks](https://en.wikipedia.org/wiki/Time_delay_neural_network)这样的模型，可以通过一系列延迟添加到输入中来包含时间信息，使得数据在不同的时间点上被表示。由于其序列特性，TDNN被实现为一个[前馈神经网络](https://en.wikipedia.org/wiki/Feedforward_neural_network)而非[递归神经网络](https://en.wikipedia.org/wiki/Recurrent_neural_network)。

### 如何使用开源软件库实现这些模型

我通常使用[Keras](https://keras.io/)来定义我的神经网络类型模型，它是一个高级神经网络API，使用Python编写，并且可以在[TensorFlow](https://github.com/tensorflow/tensorflow)、[CNTK](https://github.com/Microsoft/cntk)或[Theano](https://github.com/Theano/Theano)上运行。对于其他类型的模型，我通常使用[Scikit-Learn](http://scikit-learn.org/stable/)，这是一个免费的机器学习库，提供各种[classification](https://en.wikipedia.org/wiki/Statistical_classification)、[regression](https://en.wikipedia.org/wiki/Regression_analysis)和[clustering](https://en.wikipedia.org/wiki/Cluster_analysis)算法，包括[support vector machines](https://en.wikipedia.org/wiki/Support_vector_machine)、[random forests](https://en.wikipedia.org/wiki/Random_forests)、[gradient boosting](https://en.wikipedia.org/wiki/Gradient_boosting)、[*k*-means](https://en.wikipedia.org/wiki/K-means_clustering)和[DBSCAN](https://en.wikipedia.org/wiki/DBSCAN)，并设计为与Python的数值和科学库[NumPy](https://en.wikipedia.org/wiki/NumPy)和[SciPy](https://en.wikipedia.org/wiki/SciPy)兼容。

然而，本文的主要主题不是如何实现时间序列预测模型，而是如何评估模型预测。因此，我不会详细讨论模型构建等内容，因为有很多其他博客文章和文章涵盖了这些主题。

### 示例案例：时间序列数据的预测

本案例中使用的示例数据如下面的图所示。我稍后会更详细地介绍这些数据，但目前，**让我们假设这些数据代表了例如股票指数的年度演变**。数据被分为训练集和测试集，其中前250天的数据用于模型训练，然后我们尝试预测数据集的最后部分的股票指数。

![](../Images/3e4a7802a8b91fa90b5bb7fc9b8100a3.png)

由于本文不关注模型实现，**让我们直接进入模型准确性评估的过程**。仅通过视觉检查上述图形，模型预测似乎与实际指数紧密跟随，表明准确性较高。然而，为了更精确一些，我们可以通过绘制实际值与预测值的散点图来评估模型准确性，如下所示，同时计算常见的误差指标 [R2 score](https://en.wikipedia.org/wiki/Coefficient_of_determination)。

![](../Images/04f8f11a51f487fbbca068edf32b4582.png)

从模型预测中，我们获得了0.89的R2分数，实际值与预测值之间似乎匹配良好。然而，**正如我将详细讨论的那样**，这个指标和模型评估可能非常具有误导性。

### 这完全是**错误的**……

从上述图形和计算的误差指标来看，模型显然给出了准确的预测。然而，事实并非如此，这只是一个如何选择错误的准确性指标可能在评估模型性能时非常具有误导性的例子。在这个示例中，为了说明问题，数据被明确选择为实际上无法预测的数据。**更具体地说，我所称的“股票指数”数据，实际上是使用 [random walk process](https://en.wikipedia.org/wiki/Random_walk)进行建模的**。正如名称所示，随机游走是一个完全的 [stochastic process](https://en.wikipedia.org/wiki/Stochastic_process)。因此，使用历史数据作为训练集以学习行为和预测未来结果的想法根本不可行。鉴于此，模型为何看似给出了如此准确的预测？**正如我将详细回顾的，这都归结于（错误的）准确性指标选择。**

### 时间延迟预测和自相关

时间序列数据，顾名思义，与其他类型的数据不同，因为时间方面是重要的。**积极的一面是，这为我们提供了在构建机器学习模型时可以使用的额外信息，不仅输入特征包含有用的信息，而且输入/输出随时间的变化也是有用的**。然而，尽管时间因素增加了额外的信息，它也使时间序列问题比许多其他预测任务更难处理。

在这个具体的例子中，我使用了一个[LSTM 网络](https://en.wikipedia.org/wiki/Long_short-term_memory)，它根据之前的数据进行预测。然而，当稍微放大模型预测的细节时，如下图所示，我们开始看到模型实际上在做什么。

![](../Images/e1f7528d4a17c18cb8388b9b950c1f33.png)

时间序列数据往往在时间上具有相关性，并显示出显著的[自相关](https://en.wikipedia.org/wiki/Autocorrelation)。在这种情况下，这意味着时间“*t*+1”的指数很可能接近时间“*t*”的指数。如右侧的图示所示，模型实际在做的是，当预测时间“*t*+1”的值时，它简单地使用时间“*t*”的值作为预测（通常称为[persistence model](https://machinelearningmastery.com/persistence-time-series-forecasting-with-python/)）。绘制预测值和真实值之间的[cross-correlation](https://en.wikipedia.org/wiki/Cross-correlation)（下图），我们看到在1天的时间滞后处有一个明显的峰值，表明模型只是将前一个值作为未来的预测。

![](../Images/07a5851d1035004b6b0b27f207386217.png)

### 当不正确使用时，准确度指标可能会非常具有误导性。

这意味着在评估模型直接预测值的能力时，常见的误差指标，如[均值百分比误差](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)和[R2 分数](https://en.wikipedia.org/wiki/Coefficient_of_determination)，都显示出高预测准确性。然而，由于示例数据是通过随机游走过程生成的，模型不可能预测未来的结果。这突显了一个重要事实，即仅通过直接计算常见误差指标来评估模型的预测能力可能非常具有误导性，人们很容易对模型的准确性感到过于自信。

### 平稳性和差分时间序列数据

一个[平稳时间序列](https://www.otexts.org/fpp/8/1)是指其统计属性，如[均值](https://en.wikipedia.org/wiki/Mean)、[方差](https://en.wikipedia.org/wiki/Variance)、[自相关](https://en.wikipedia.org/wiki/Autocorrelation)等，都在时间上保持不变。大多数统计预测方法基于这样一个假设：时间序列可以通过数学变换大致“平稳化”。一种基本的变换是[时间差分数据](https://www.otexts.org/fpp/8/1)，如下面的图所示。

![](../Images/bb0f437a4dd3a7ca78f33a07b8862536.png)

这种变换的作用是，我们不直接考虑指数，而是计算连续时间步之间的*difference*。

将模型定义为预测时间步之间的*差异*而不是值本身，是对模型预测能力的更强测试。在这种情况下，它不能简单地使用数据具有强自相关性，并将时间“*t*”的值作为“*t+*1”的预测。因此，它提供了对模型的更好测试，检验其是否从训练阶段学到有用的东西，以及分析历史数据是否确实能帮助模型预测未来变化。

### 时间差分数据的预测模型

由于能够预测时间差分数据，而不是直接预测数据，能更强地指示模型的预测能力，因此我们用我们的模型进行尝试。该测试的结果在下图中展示，显示了实际值与预测值的散点图。

![](../Images/25c19f3d6d1f44fc053d5cce69a0b1cb.png)

这个图表表明模型**不能**根据历史事件预测未来变化，这在这种情况下是预期结果，因为数据是通过完全随机的 [随机游走过程](https://en.wikipedia.org/wiki/Random_walk)生成的。根据定义，预测 [随机过程](https://en.wikipedia.org/wiki/Stochastic_process) 的未来结果是不可能的，如果有人声称可以做到这一点，应该有点怀疑……

### 你的时间序列是随机游走吗？

你的时间序列实际上可能是随机游走，以下是一些检查方法：

+   时间序列显示出强烈的时间依赖性（自相关性），其衰减呈线性或类似模式。

+   时间序列是非平稳的，将其平稳化后显示数据中没有明显的可学习结构。

+   持久性模型（使用上一个时间步的观察值作为下一个时间步的预测）提供了最可靠的预测来源。

这一点对于时间序列预测至关重要。基线预测使用 [持久性模型](https://machinelearningmastery.com/persistence-time-series-forecasting-with-python/) 可以迅速指示你是否能够显著改进。如果不能，你可能在处理随机游走（或接近随机游走）。人类的大脑被固有地编程去寻找模式，我们必须警惕不要自欺欺人，浪费时间开发复杂的模型用于随机游走过程。

> *作者注：在发布文章后，我意识到Jason Brownlee在相同主题上有一篇很好的文章，处理了随机游走和时间序列： [使用Python进行时间序列预测的随机游走温和介绍](https://machinelearningmastery.com/gentle-introduction-random-walk-times-series-forecasting-python/)。*

### 摘要

我希望通过这篇文章强调的主要观点是，在评估模型预测准确性时要**非常小心**。正如上述例子所示，即使是完全随机的过程，其中预测未来结果在定义上是不可能的，也很容易被欺骗。通过简单地定义一个模型，进行一些预测，并计算常见的准确性指标，似乎可以有一个好的模型并决定将其投入生产。然而，实际上，该模型可能根本没有预测能力。

如果你正在进行时间序列预测，且可能认为自己是数据科学家，我建议你同样重视[*科学家*](https://en.wikipedia.org/wiki/Scientist)的角色。始终对数据告诉你的内容持怀疑态度，提出关键问题，绝不要匆忙下结论。[科学方法](https://en.wikipedia.org/wiki/Scientific_method)应在数据科学中应用，就像在任何其他科学领域一样。

[原文](https://towardsdatascience.com/how-not-to-use-machine-learning-for-time-series-forecasting-avoiding-the-pitfalls-19f9d7adf424). 经授权转载。

**简介**：[Vegard Flovik](https://www.linkedin.com/in/vegard-flovik/) 是一名首席数据科学家，负责Axbit AS的机器学习和高级分析工作。

**资源：**

+   [在线和基于网络的：分析、数据挖掘、数据科学、机器学习教育](https://www.kdnuggets.com/education/online.html)

+   [分析、数据科学、数据挖掘和机器学习的软件](https://www.kdnuggets.com/software/index.html)

**相关：**

+   [使用简单神经网络和LSTM进行时间序列预测简介](https://www.kdnuggets.com/2019/04/introduction-time-series-forecasting-simple-neural-networks-lstm.html)

+   [通过自动化机器学习加速时间序列分析](https://www.kdnuggets.com/2019/02/datarobot-accelerating-time-series-analysis-automated-machine-learning.html)

+   [如何微调机器学习模型以提高预测准确性](https://www.kdnuggets.com/2019/01/fine-tune-machine-learning-models-forecasting.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的IT工作

* * *

### 更多相关话题

+   [成为优秀数据科学家所需的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家都应该掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [每个数据科学家都应该知道的三个R库（即使你使用Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [使用Ploomber、Arima、Python和Slurm进行时间序列预测](https://www.kdnuggets.com/2022/03/time-series-forecasting-ploomber-arima-python-slurm.html)

+   [使用statsmodels和Prophet进行时间序列预测](https://www.kdnuggets.com/2023/03/time-series-forecasting-statsmodels-prophet.html)
