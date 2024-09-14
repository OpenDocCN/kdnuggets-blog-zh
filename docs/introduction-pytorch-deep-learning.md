# PyTorch 深度学习简介

> 原文：[`www.kdnuggets.com/2018/11/introduction-pytorch-deep-learning.html`](https://www.kdnuggets.com/2018/11/introduction-pytorch-deep-learning.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

![Image](img/601b7698df3c9c20b0c40518801cdfa0.png)

在本教程中，你将了解**使用 PyTorch 框架的深度学习**，并在完成后，你将能够舒适地将其应用于你的深度学习模型。Facebook 今年早些时候推出了 PyTorch 1.0，并与[Google Cloud](https://cloud.google.com/)、[AWS](http://aws.amazon.com/)和[Azure 机器学习](https://azure.microsoft.com/en-us/services/machine-learning-studio/)集成。在本教程中，我假设你已经熟悉[Scikit-learn](http://scikit-learn.org/)、[Pandas](https://pandas.pydata.org/)、[NumPy](http://www.numpy.org/)和[SciPy](https://www.scipy.org/)。这些包是本教程的重要前提条件。

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

### 攻击计划

1.  什么是[深度学习](https://heartbeat.fritz.ai/introduction-to-deep-learning-with-keras-c7c3d14e1527)？

1.  PyTorch 简介

1.  为什么你会选择 PyTorch 而非其他 Python 深度学习库

1.  PyTorch 张量

1.  PyTorch 自动求导

1.  PyTorch nn 模块

1.  PyTorch 优化包

1.  自定义 PyTorch nn 模块

1.  综合概述及进一步阅读

### 什么是深度学习？

深度学习是机器学习的一个子领域，其算法受到人脑工作方式的启发。这些算法被称为人工神经网络。这些神经网络的例子包括[卷积神经网络](https://heartbeat.fritz.ai/a-beginners-guide-to-convolutional-neural-networks-cnn-cf26c5ee17ed)，用于图像分类，人工神经网络和递归神经网络。

### PyTorch 简介

PyTorch 是一个基于[Torch](https://en.wikipedia.org/wiki/Torch_%28machine_learning%29)的 Python 机器学习包，而 Torch 是一个基于编程语言[Lua](https://en.wikipedia.org/wiki/Lua_%28programming_language%29)的开源机器学习包。PyTorch 具有两个主要特性：

+   [Tensor](https://www.kdnuggets.com/2018/05/wtf-tensor.html) 计算（类似于 NumPy）具有强大的 GPU 加速

+   自动微分用于构建和训练神经网络

### 为什么你可能会倾向于使用 PyTorch 而非其他 Python 深度学习库

你可能会倾向于使用 PyTorch 而非其他深度学习库有几个原因：

1.  与其他库如 TensorFlow 不同，你必须首先定义整个计算图才能运行模型，PyTorch 允许你动态定义计算图。

1.  PyTorch 对于深度学习研究也非常出色，提供了最大的灵活性和速度。

### PyTorch 张量

**PyTorch 张量**与 NumPy 数组非常相似，区别在于它们可以在 GPU 上运行。这一点很重要，因为它有助于加速数值计算，这可以将神经网络的速度提高 50 倍或更多。要使用 PyTorch，你需要访问[`PyTorch.org/`](https://pytorch.org/)并安装 PyTorch。如果你使用 Conda，可以通过运行以下简单命令安装 PyTorch：

要定义一个 PyTorch 张量，首先需要导入 `torch` 包。PyTorch 允许你定义两种类型的[张量](https://pytorch.org/docs/0.3.1/tensors.html)——CPU 张量和 GPU 张量。在本教程中，我会假设你正在使用 CPU 机器，但我也会展示如何在 GPU 上定义张量：

PyTorch 中的默认张量类型是一个浮点张量，定义为 `**torch.FloatTensor**`。作为示例，你将从一个 Python 列表中创建一个张量：

如果你使用的是支持 GPU 的机器，你将按照下面的方式定义张量：

你也可以使用 PyTorch 张量进行数学计算，如加法和减法：

你还可以定义矩阵并进行矩阵操作。让我们看看如何定义一个矩阵并进行转置：

### PyTorch 自动微分

PyTorch 使用一种称为[**自动微分**](https://en.wikipedia.org/wiki/Automatic_differentiation)的技术来数值评估函数的导数。自动微分在神经网络中计算反向传播。在训练神经网络时，权重被随机初始化为接近零但不为零的数字。**反向传播**是调整这些权重的过程，从右到左，而前向传播则是其逆过程（从左到右）。

`torch.autograd` 是支持 PyTorch 中自动微分的库。该包的核心类是 `torch.Tensor`。要跟踪所有操作，将 `.requires_grad` 设置为 `True`。要计算所有梯度，调用 `.backward()`。该张量的梯度将累积在 `**.**grad` 属性中。

如果你想将张量从计算历史中分离，可以调用 `**.**detach()` 函数。这也将防止对张量的未来计算进行跟踪。另一种防止历史跟踪的方法是使用 `torch.no_grad():` 包装你的代码。

`Tensor` 和 `Function` 类是相互连接的，以构建一个无环图，该图编码了计算的完整历史。张量的 `.grad_fn` 属性引用创建该张量的 `Function`。要计算导数，请在 `Tensor` 上调用 `.backward()`。如果 `Tensor` 只包含一个元素，你无需为 `backward()` 函数指定任何参数。如果 `Tensor` 包含多个元素，则需要指定一个形状匹配的梯度张量。

作为示例，你将创建两个张量，一个的 `requires_grad` 为 `True`，另一个为 `False`。然后你将使用这两个张量进行加法和求和操作。之后，你将计算其中一个张量的梯度。

调用 `**.**grad` 在 `b` 上将不会返回任何内容，因为你没有将 `requires_grad` 设置为 `True`。

### PyTorch nn 模块

这是用于在 PyTorch 中构建神经网络的模块。`nn` 依赖于 `autograd` 来定义模型和对其进行微分。让我们从定义 **训练神经网络** 的过程开始：

1.  定义一个具有可学习参数的神经网络，这些参数称为权重。

1.  遍历输入数据集。

1.  通过网络处理输入。

1.  将预测结果与实际值进行比较，并测量误差。

1.  将梯度传播回网络的参数中。

1.  使用简单的更新规则更新网络的权重：

`weight = weight — learning_rate * gradient`

现在你将使用 `nn` 包来创建一个两层的神经网络：

让我们解释一下上述使用的一些参数：

+   `N` 是批量大小。批量大小是权重将在更新后更新的观察数量。

+   `D_in` 是输入维度

+   `H` 是隐藏维度

+   `D_out` 是输出维度

+   `torch.randn` 定义了一个具有指定维度的矩阵

+   `torch.nn.Sequential` 初始化一个线性层堆栈

+   `torch.nn.Linear` 对输入数据应用线性变换

+   `torch.nn.ReLU` 元素级地应用修正线性单元函数

+   `torch.nn.MSELoss` 创建一个度量输入 x 和目标 y 之间的均方误差的标准

### PyTorch optim 包

接下来，你将使用 `optim` 包来 **定义一个优化器**，它将为你更新权重。`optim` 包抽象了优化算法的概念，并提供了常用优化算法的实现，如 AdaGrad、RMSProp 和 Adam。我们将使用 Adam 优化器，它是更受欢迎的优化器之一。

该优化器接受的第一个参数是张量，它应该更新。在前向传递中，你将通过将 x 传递给模型来计算预测的 y。之后，计算并打印损失。在运行反向传递之前，将所有梯度归零，以便更新优化器中的变量。这是因为默认情况下，`.backward()` 被调用时梯度不会被覆盖。之后，调用优化器的 step 函数，这将更新其参数。如何实现如下所示，

### PyTorch 中的自定义 nn 模块

有时你需要构建自己的自定义模块。在这些情况下，你将子类化 `nn.Module`**。**然后你需要定义一个 `forward`，它将接收输入张量并产生输出张量。如何使用 `nn.Module` 实现一个两层网络如下所示*。*该模型与上面的模型非常相似，但不同之处在于你将使用 `torch.nn.Module` 来创建神经网络。另一个不同之处是使用 `随机梯度下降优化器` 而不是 Adam。你可以按下面所示实现一个自定义 nn 模块：

### 综合所有内容与进一步阅读

PyTorch 允许你实现不同类型的层，如 [卷积层，](https://heartbeat.fritz.ai/a-beginners-guide-to-convolutional-neural-networks-cnn-cf26c5ee17ed) [递归层](https://pytorch.org/docs/stable/index.html)，以及 [线性层，](https://pytorch.org/docs/stable/nn.html) 等等。你可以通过其官方 [文档](https://pytorch.org/docs/stable/) 了解更多有关 PyTorch 的信息。

**简介: [德里克·穆伊提](https://derrickmwiti.com/)** 是一位数据分析师、作家和导师。他致力于在每个任务中取得优异的成果，并且是 Lapid Leaders Africa 的导师。

[原文](https://heartbeat.fritz.ai/introduction-to-pytorch-for-deep-learning-5b437cea90ac)。转载经许可。

**相关内容：**

+   Keras 深度学习入门

+   使用 PyTorch 的简单导数

+   PyTorch 张量基础

### 更多相关话题

+   [深度学习库简介：PyTorch 和 Lightning AI](https://www.kdnuggets.com/introduction-to-deep-learning-libraries-pytorch-and-lightning-ai)

+   [完整的免费 PyTorch 深度学习课程](https://www.kdnuggets.com/2022/10/complete-free-pytorch-course-deep-learning.html)

+   [使用 PyTorch 的迁移学习实用指南](https://www.kdnuggets.com/2023/06/practical-guide-transfer-learning-pytorch.html)

+   [PyTorch 还是 TensorFlow？对比流行的机器学习框架](https://www.kdnuggets.com/2022/02/packt-pytorch-tensorflow-comparing-popular-machine-learning-frameworks.html)

+   [可解释的 PyTorch 神经网络](https://www.kdnuggets.com/2022/01/interpretable-neural-networks-pytorch.html)

+   [如何开始使用 PyTorch 进行自然语言处理](https://www.kdnuggets.com/2022/04/start-natural-language-processing-pytorch.html)
