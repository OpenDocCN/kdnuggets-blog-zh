# 让你的深度学习训练更快：FreezeOut

> 原文：[https://www.kdnuggets.com/2017/08/train-deep-learning-faster-freezeout.html](https://www.kdnuggets.com/2017/08/train-deep-learning-faster-freezeout.html)

深度神经网络有许多可学习的参数用于进行推断。这通常在两个方面构成问题：有时，模型的预测不够准确，同时训练所需时间也很长。

在之前的文章中，我们介绍了 [让你的深度学习模型训练更快更精准：Snapshot Ensembling — 用1的成本实现M个模型](/2017/08/train-deep-learning-faster-snapshot-ensembling.html)。

本文讨论了使用一种新颖的方法减少训练时间，对准确性几乎没有影响。

### [FreezeOut — 通过逐步冻结层加速训练](https://arxiv.org/pdf/1706.04983.pdf)

本文作者提出了一种通过冻结层来提高训练速度的方法。他们尝试了几种不同的冻结层的方法，并展示了训练速度的提高对准确性影响很小（或没有影响）。

#### 冻结一层是什么意思？

冻结一层可以防止其权重被修改。这种技术常用于**迁移学习**，其中基础模型（在其他数据集上训练过的）被冻结。

#### **冻结如何影响模型的速度？**

如果你不想修改某一层的权重，可以**完全避免**对该层的**反向传播**，从而显著**提高速度**。例如，如果你冻结了模型的一半，训练模型将只需比完全可训练模型少一半的时间。

另一方面，你仍然需要训练模型，因此如果冻结得太**早**，将会得到**不准确**的预测。

#### 什么是‘新颖’的方法？

作者展示了一种**逐层冻结**的方式，尽可能快地冻结每一层，从而减少反向传播的次数，从而降低训练时间。

起初，整个模型是可训练的（和普通模型完全一样）。经过几次迭代后，第一层被冻结，其余模型继续训练。再经过几次迭代，下一层被冻结，以此类推。

#### **学习率退火**

作者们使用了学习率退火来控制模型的学习率。他们使用的显著不同的技术是**逐层**而不是整个模型来**改变**学习率。他们使用了以下方程：

![学习率逐层调整](../Images/3ee3d24d4a8960a68c1ed13706be64d5.png)

方程 2.0：**α**是学习率。**t**是迭代次数。***i***表示模型的第i层。

#### 方程 2.0 解释

子*i*表示第i层。因此*α*子*i*表示第i层的学习率。类似地，*t[i]*表示第i层训练的迭代次数。*t*表示整个模型的总迭代次数。

![alpha](../Images/7b2da0a9a0e9eb8a88a5b558cfe03f31.png)

这表示第 i 层的初始学习率。

作者们尝试了方程 2.1 的不同值

#### 方程式 2.1 的初始学习率

作者们尝试了缩放初始学习率，使每层被训练的时间相等。

记住，由于模型的第一层会首先停止训练，因此它会被训练最少的时间。为了解决这个问题，他们对每层的学习率进行了缩放。

![线性计划](../Images/b36a7082533ab3cf8168df5b0e18f6e9.png)

进行缩放是为了确保所有层的权重在权重空间中均匀移动，即被训练时间最长的层（后面的层）具有较低的学习率。

作者们还尝试了**立方缩放**，即将 t 子 i 的值替换为其立方。

![DenseNet 上的性能与误差](../Images/eee55845ce5249ed089173cdc233e562.png)

图 2.1：DenseNet 上的性能与误差

作者们增加了更多的基准测试，他们的方法在仅**3%**的准确率下降下提高了约**20%**的训练速度，而在**无下降**的情况下提高了**15%**。

对于不利用跳跃连接的模型（例如 VGG-16），他们的方法效果不是很好。在这些网络中，准确率和加速并没有明显差异。

* * *

### 我的额外技巧

作者们正在逐步停止每层的训练，并且不再计算反向传递。他们似乎遗漏了利用**预计算层激活**。通过这样做，你甚至可以防止计算**前向传递**。

#### **什么是预计算**

这是转移学习中使用的技巧。这是一般工作流程。

1.  冻结你不想修改的层

1.  计算从冻结层到最后一层的激活（对你的整个数据集）

1.  将这些激活保存到磁盘

1.  使用这些激活作为可训练层的输入

由于层是逐步冻结的，新的模型现在可以视为一个独立模型（一个更小的模型），它只接收最后一层的输出作为输入。每冻结一层，可以重复这个过程。

这样做加上**FreezeOut**将会大幅减少训练时间，同时不会以任何方式影响其他指标（如准确率）。

### 结论

我展示了 2 种（以及我自己的一半）非常近期和新颖的技术，通过微调学习率来提高准确率和降低训练时间。同时，通过尽可能添加预计算，使用我自己提出的方法可以显著加快速度。

[原文](https://hackernoon.com/training-your-deep-model-faster-and-sharper-e85076c3b047)。经授权转载。

**简介：** Harshvardhan Gupta 在 [HackerNoon](https://hackernoon.com/) 上撰写文章。

**相关：**

+   [让你的深度学习模型更快更精准：Snapshot Ensembling — 用1的成本获得M个模型](/2017/08/train-deep-learning-faster-snapshot-ensembling.html)

+   [DeepSense：用于时间序列移动传感数据处理的统一深度学习框架](/2017/08/deepsense-unified-deep-learning-framework-time-series-mobile.html)

+   [如何从训练数据中挤出更多价值](/2017/07/squeeze-most-from-training-data.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 需求

* * *

### 主题更多

+   [通过参与比赛，4倍速学习机器学习](https://www.kdnuggets.com/2022/01/learn-machine-learning-4x-faster-participating-competitions.html)

+   [为什么我们总是需要人类来训练 AI——有时是实时的](https://www.kdnuggets.com/2021/12/why-we-need-humans-training-ai.html)

+   [使用 Tensorflow 训练图像分类模型的指南](https://www.kdnuggets.com/2022/12/guide-train-image-classification-model-tensorflow.html)

+   [如何从头开始构建和训练 Transformer 模型…](https://www.kdnuggets.com/how-to-build-and-train-a-transformer-model-from-scratch-with-hugging-face-transformers)

+   [利用 AI 和分析引擎更快地准备时间序列数据](https://www.kdnuggets.com/2021/12/piexchange-faster-way-prepare-timeseries-data-ai-analytics-engine.html)

+   [oBERT：复合稀疏化提供更快、更准确的 NLP 模型](https://www.kdnuggets.com/2022/05/obert-compound-sparsification-delivers-faster-accurate-models-nlp.html)
