# 如何在深度学习中创建自定义实时图表

> 原文：[https://www.kdnuggets.com/2020/12/create-custom-real-time-plots-deep-learning.html](https://www.kdnuggets.com/2020/12/create-custom-real-time-plots-deep-learning.html)

[评论](#comments)![图像](../Images/09f302a5f8f4d5b2f8b8eda6d0830a4f.png)

图像来源：[**Pixabay**](https://pixabay.com/photos/garage-floors-parking-construction-1149542/)

### 我们所说的实时图表是什么意思？

训练一个复杂的深度学习模型和一个大数据集可能是非常耗时的。随着训练轮次的增加，大量数字在屏幕上闪烁。你的眼睛（和大脑）会感到疲倦。

那个令人兴奋的准确度图表在哪里，不断更新你的进展情况？你怎么知道模型是否在学习有用的东西？以及，学习的速度有多快？

一个***实时视觉更新***会非常棒，不是吗？

毕竟，人类是视觉动物。

[**人类是视觉动物**](https://www.seyens.com/humans-are-visual-creatures/)

在这里，我们收集了一些有趣的事实，以强调在科学交流中使用视觉辅助工具的重要性...

说到*视觉*，我并不是指在你打开模型的详细输出时屏幕上滚动的大量数字。

**不是这个。**

![文章图像](../Images/a09377ffebfe8e2b0f095871e98d674f.png)

**我们要这个。**

![文章图像](../Images/97fbf52e2cd7ee6dd47da8a45468611f.png)

让我们看看如何实现这个目标。

> 那个令人兴奋的准确度图表在哪里，不断更新你的进展情况？你怎么知道模型是否在学习有用的东西？以及，学习的速度有多快？

### 我们所说的自定义图表是什么意思？

对于常规任务，有很多现成的工具。但许多时候，我们需要定制化的输出。

### Tensorboard 很酷，但可能无法满足所有需求。

如果你在进行深度学习任务时使用 TensorFlow/Keras，可能你已经听说过或使用过 Tensorboard。它是一个非常棒的仪表板工具，你可以传递训练日志，并获得极佳的可视化更新。

[**开始使用 TensorBoard | TensorFlow**](https://www.tensorflow.org/tensorboard/get_started)

在机器学习中，要改进某些东西，你通常需要能够衡量它。TensorBoard 是一个提供...

![图像](../Images/3d58a74739c05eb95f99ced75497b5ee.png)

图像来源：[**Tensorboard**](https://www.tensorflow.org/tensorboard)

你可以非常轻松地使用 Tensorboard 获取标准的损失和准确度图表。如果你只想监控这些内容，而不关心其他内容，可以停止阅读本文，直接使用 Tensorboard。

但如果你有一个高度不平衡的数据集，并且你想绘制**精准度、召回率和 F1 分数**呢？或者，另一种不那么常见的分类指标，比如 [**马修斯系数**](https://en.wikipedia.org/wiki/Matthews_correlation_coefficient)？再或者，你只关心真正负样本和假负样本的比例，想创建你自己的指标？

如何在训练过程中看到这些非标准指标的实时更新？

### Keras 有内置的混淆矩阵计算功能

幸运的是，Keras 提供了与混淆矩阵相关的四个基本量的基本日志——真正的正例 (TP)、假正例 (FP)、真正的负例 (TN) 和假负例 (FN)。这些来自 Keras Metrics 模块。

[**模块: tf.keras.metrics | TensorFlow Core v2.3.0**](https://www.tensorflow.org/api_docs/python/tf/keras/metrics)

内置指标。

我们可以简单地在模型的训练日志中定义一个指标列表，并在编译模型时传递该列表。

```py
**metrics** = [
    tf.keras.metrics.TruePositives(name="tp"),
    tf.keras.metrics.TrueNegatives(name="tn"),
    tf.keras.metrics.FalseNegatives(name="fn"),
    tf.keras.metrics.FalsePositives(name="fp"),
]
```

然后，

```py
model.compile(
        optimizer=tf.keras.optimizers.Adam(lr=learning_rate),
        loss=tf.keras.losses.BinaryCrossentropy(),
       ** metrics=metrics**,
    )
```

所以，我们可以将这些度量（尽管它们是在训练数据集上计算的）作为训练日志的一部分。一旦得到这些度量，我们可以根据原理定义计算我们想要的任何自定义指标。例如，这里展示了一些非标准指标的公式，

![图](../Images/8de41f4b30ce8573040d305b52603e3e.png)

图片来源: [维基百科](https://en.wikipedia.org/wiki/Precision_and_recall)

但是，我们如何从这些计算值创建自定义实时图表呢？

**我们当然使用回调！**

> 如何在训练过程中看到这些非标准指标的实时更新？

### 用于实时可视化的自定义回调

回调是一种神奇的工具类，可以在训练的某些点（或者每个纪元）被调用。简而言之，它们可以在训练过程中**实时处理与模型性能或算法相关的数据**。

这是 TensorFlow 官方的 Keras 回调页面。但为了我们的目的，**我们必须编写一个自定义绘图类**，它派生自基础 Callback 类。

[**模块: tf.keras.callbacks | TensorFlow Core v2.3.0**](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks)

回调：在模型训练过程中在特定点调用的工具。

### 演示 Jupyter notebook

演示 Jupyter notebook 在我的 Github 仓库中 [**位于这里**](https://github.com/tirthajyoti/Deep-learning-with-Python/blob/master/Notebooks/Custom-real-time-plots-with-callbacks.ipynb)。[**这个仓库**](https://github.com/tirthajyoti/Deep-learning-with-Python) 包含许多其他有用的深度学习教程风格的笔记本。请随意加星或 fork。

### 一个不平衡的数据集

![图](../Images/47bc59c70b76f2bef4725c3d7edbd46f.png)

图片来源: [**Pixabay**](https://pixabay.com/illustrations/balance-swing-equality-measurement-2108024/)

我们为演示中的二分类任务创建了一个具有不平衡类别频率（负样本远多于正样本）的合成数据集。这种情况在实际数据分析项目中非常常见，它强调了需要一个用于自定义分类指标的可视化仪表盘的必要性，在这种情况下准确率不是一个好的指标。

以下代码创建了一个包含**90% 负样本和 10% 正样本**的数据集。

```py
from sklearn.datasets import make_classificationn_features = 15
n_informative = n_featuresd = make_classification(n_samples=10000,
                        n_features=n_features,
                        n_informative=n_informative,
                        n_redundant=0,
                        n_classes=2,
                        **weights=[0.9,0.1]**,
                        flip_y=0.05,
                        class_sep=0.7)
```

下图显示了两个类别的样本数据分布。注意核密度图的失衡。

![图像](../Images/b9d660279e2cc9116455c123a2a1f065.png)

**合成数据集按类别分布**

> 回调是一类很棒的工具，可以在训练的某些点（或者每个周期）调用。

### 自定义回调类

自定义回调类主要执行以下操作，

+   初始化一系列列表以存储值

+   从模型中提取每个周期结束时的指标

+   从这些提取中计算分类指标

+   并将其存储在这些列表中

+   创建多个图表

这里是**初始化**，

![帖子图像](../Images/3856b6e836edad3d0db2e702a284186f.png)

这里是**提取**，

![帖子图像](../Images/e99f274458bc9262b27380369b1ba64c.png)

这里是**计算**，

![帖子图像](../Images/c2458e1c6d89203ed6bb98ee11c5d469.png)

这里是**存储**，

![帖子图像](../Images/b7d4cec14b79606613bb18250242275f.png)

此外，我不会用标准的 Matplotlib 绘图代码来让你觉得枯燥，除了以下这部分，它**在每次迭代时刷新你的 Jupyter notebook 图表**。

```py
from IPython.display import clear_output# Clear the previous plot
 clear_output(wait=True)
```

此外，**你不必每个周期都绘图**，因为这可能会增加负担并减慢显示或机器的速度。你可以选择每隔 5 个周期绘制一次。只需将整个绘图代码放在一个条件下（这里`epoch`是你从训练日志中获得的周期数）

```py
# Plots every 5th epoch
 if epoch > 0 and epoch%5==0:
```

不用担心这些如何协同工作，因为[**演示笔记本仍然在这里**](https://github.com/tirthajyoti/Deep-learning-with-Python/blob/master/Notebooks/Custom-real-time-plots-with-callbacks.ipynb)供你使用。

### 结果

这里是一个典型的结果，显示了损失和精度/召回率/F1 分数的简单仪表板风格。注意，召回率开始时较高，但精度和 F1 分数对于这个不平衡的数据集较低。这些是你可以通过这种回调实时计算和监控的指标！

![帖子图像](../Images/a30fe77c85543471d9015055dbbd5a71.png)

### 更多结果 — 概率分布！

你可以在每个周期结束时对模型（在那个点训练好的）进行任何计算，并可视化结果。例如，我们可以预测输出概率，并绘制它们的分布。

```py
def on_epoch_end(self, epoch, logs={}):
    # Other stuff
    m = self.model
    preds = m.predict(X_train)
    plt.hist(preds, bins=50,edgecolor='k')
```

注意，开始时只有少数样本获得高概率，随着时间的推移，模型开始学习数据的真实分布。

![帖子图像](../Images/c56c2ff6cae1a1770cdaac86d7f82ec8.png)

### 摘要

我们展示了如何通过简单的代码片段创建一个实时深度学习模型性能的仪表板。按照这里概述的方法，你无需依赖 Tensorboard 或任何第三方软件。你可以创建自己的计算和图表，尽可能自定义。

请注意，上述方法仅适用于 Jupyter Notebook。对于独立的 Python 脚本，您需要进行不同的调整。

关于这个话题，还有另一篇精彩的文章，您可以在这里查看。

[**如何在 Keras 中绘制模型训练图 — 使用自定义回调函数和 TensorBoard**](https://medium.com/@kapilvarshney/how-to-plot-the-model-training-in-keras-using-custom-callback-function-and-using-tensorboard-41e4ce3cb401)

在处理犬种识别时，我开始探索不同的训练过程可视化方法……

### 您可能还喜欢……

如果您喜欢这篇文章，您可能还会喜欢以下我撰写的深度学习文章，

[**您是否在 Keras 深度学习模型中使用了“Scikit-learn 包装器”？**](https://towardsdatascience.com/are-you-using-the-scikit-learn-wrapper-in-your-keras-deep-learning-model-a3005696ff38)

如何使用 Keras 的特殊包装类进行超参数调优？

[**深度学习模型的激活图，几行代码搞定**](https://towardsdatascience.com/a-single-function-to-streamline-image-classification-with-keras-bd04f5cfe6df)

我们展示了如何用几行代码展示深度 CNN 模型中各层的激活图……

[**一个简化 Keras 图像分类的单函数**](https://towardsdatascience.com/a-single-function-to-streamline-image-classification-with-keras-bd04f5cfe6df)

我们展示了如何构建一个通用的工具函数来自动从目录中提取图像，并……

您可以查看作者的 [**GitHub**](https://github.com/tirthajyoti?tab=repositories)** 代码库**，以获取机器学习和数据科学的代码、想法和资源。如果您和我一样，对 AI/机器学习/数据科学充满热情，请随时 [在 LinkedIn 上添加我](https://www.linkedin.com/in/tirthajyoti-sarkar-2127aa7/) 或 [在 Twitter 上关注我](https://twitter.com/tirthajyotiS)。

[**Tirthajyoti Sarkar - 高级首席工程师 - 半导体、AI、机器学习 - ON…**](https://towardsdatascience.com/activation-maps-for-deep-learning-models-in-a-few-lines-of-code-ed9ced1e8d21)

通过写作让数据科学/机器学习概念易于理解：https://medium.com/@tirthajyoti 开源和……

[原文](https://towardsdatascience.com/how-to-create-custom-real-time-plots-in-deep-learning-ecbdb3e7922f)。经授权转载。

**相关内容：**

+   [用于比较、绘图和评估回归模型的简单 Python 包](/2020/11/simple-python-package-comparing-plotting-evaluating-regression-models.html)

+   [初学者的 20 个核心数据科学概念](/2020/12/20-core-data-science-concepts-beginners.html)

+   [通过 Yann LeCun 的免费课程学习深度学习](/2020/11/learn-deep-learning-free-course-yann-lecun.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织在 IT 领域

* * *

### 更多相关内容

+   [如何在 Python 中创建自定义上下文管理器](https://www.kdnuggets.com/how-to-create-custom-context-managers-in-python)

+   [介绍 OpenChat：一个免费且简单的构建平台…](https://www.kdnuggets.com/2023/06/introducing-openchat-free-simple-platform-building-custom-chatbots-minutes.html)

+   [使用自定义指令调整 ChatGPT 以满足您的需求](https://www.kdnuggets.com/2023/08/tailor-chatgpt-fit-needs-custom-instructions.html)

+   [如何为机器学习创建数据集](https://www.kdnuggets.com/2022/02/create-dataset-machine-learning.html)

+   [创建时间序列比率分析仪表板](https://www.kdnuggets.com/2023/06/wolfer-create-time-series-ratio-analysis-dashboard.html)

+   [从零到英雄：使用 PyTorch 创建您的第一个 ML 模型](https://www.kdnuggets.com/from-zero-to-hero-create-your-first-ml-model-with-pytorch)
