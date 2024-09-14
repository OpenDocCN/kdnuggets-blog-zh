# 开始使用 PyTorch 第 1 部分：理解自动微分是如何工作的

> 原文：[https://www.kdnuggets.com/2018/04/getting-started-pytorch-understanding-automatic-differentiation.html/2](https://www.kdnuggets.com/2018/04/getting-started-pytorch-understanding-automatic-differentiation.html/2)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](/2018/04/getting-started-pytorch-understanding-automatic-differentiation.html?page=2#comments)

### 构建块 #3：变量和 Autograd

PyTorch 使用*Autograd*包实现了我们上面描述的功能。

现在，了解*Autograd*如何工作有三个重要的方面。

**构建块 #3.1：变量**

*变量*就像一个*张量*，是一个用于存储数据的类。然而，它在使用方式上有所不同。***变量***专门用于保存在神经网络训练过程中会变化的值，即我们网络的可学习参数。**张量**则用于存储不会被学习的值。例如，张量可能用于存储每个示例生成的损失值。

```py
from torch.autograd import Variable

var_ex = Variable(torch.randn((4,3))   #creating a Variable
```

*变量*类封装了一个张量。你可以通过调用变量的***.data***属性来访问这个张量。

*变量*还存储了标量量（比如损失）相对于它所持有的参数的梯度。可以通过调用***.grad***属性来访问这个梯度。这基本上是计算到当前节点的梯度，而每个后续节点的梯度可以通过将*边权重*与前一个节点计算出的梯度相乘来计算。

*变量*持有的第三个属性是***grad_fn***，这是创建该变量的*函数*对象。

![](../Images/db001b7058cc95475452d2b2f0c97638.png)

> **注意：** PyTorch 0.4 将变量和张量类合并为一个，张量可以通过切换而不是实例化新对象来变成“变量”。但由于我们在本教程中使用的是 v 0.3，我们将继续进行。

**构建块 #3.2：函数**

我刚才提到的*函数*基本上是对函数的抽象。它接受输入并返回输出。例如，如果我们有两个变量，*a*和*b*，那么如果，

*c = a + b*

然后*c*是一个新变量，它的*grad_fn*是一个叫做*AddBackward*的东西（PyTorch 用于将两个变量相加的内置函数），这个函数接受了*a*和*b*作为输入，并创建了*c*。

然后，你可能会问，为什么需要一个全新的类，而 Python 本身提供了定义函数的方法？

在训练神经网络时，有两个步骤：前向传播和反向传播。通常，如果你使用 Python 函数来实现，你将需要定义两个函数：一个用于计算前向传播过程中的输出，另一个用于计算需要传播的梯度。

**PyTorch 将需要编写两个单独的函数（用于前向传播和反向传播）的需求抽象成一个名为 *torch.autograd.Function* 的单一类中的两个成员函数。**

PyTorch 结合了 *Variables* 和 *Functions* 来创建计算图。

**构建块 #3.3 : 自动求导**

现在，让我们深入了解 PyTorch 如何创建计算图。首先，我们定义我们的变量。

上述代码行的结果是，

现在，让我们剖析一下刚才发生了什么。如果你查看源代码，事情是这样发展的。

+   **定义图的*叶子*变量（第 5–9 行）。** 我们开始定义一堆“变量”（普通的 Python 语言使用，而非 pytorch 的 *Variables*）。如果你注意到，我们定义的值是计算图中的叶子节点。因为这些节点不是任何计算的结果，所以我们必须定义它们。在这一点上，这些变量现在占据了我们 Python 命名空间中的内存。这意味着它们是真实存在的。我们**必须**将 ***requires_grad*** 属性设置为 True，否则这些 Variables 不会被包含在计算图中，也不会为它们（以及依赖于这些特定变量以进行梯度流动的其他变量）计算梯度。

+   **创建图（第 12–15 行）**。到目前为止，我们的内存中还没有计算图。只有叶子节点，但一旦你编写第 12–15 行，图就会**即时生成。非常重要的是要掌握这一细节。即时生成。** 当你写 *b =w1*a* 时，就是图创建开始的时刻，并持续到第 15 行。这正是我们模型的前向传播阶段，当输出从输入中计算出来时。每个变量的 *forward* 函数可能会缓存一些输入值以在反向传播时计算梯度。（例如，如果我们的前向函数计算 *W*x*，那么 *d(W*x)/d(W)* 就是 *x*，需要缓存的输入）

+   现在，我告诉你我之前绘制的图不完全准确的原因是什么？因为当 PyTorch 创建图时，图中的节点不是 *Variable* 对象。实际上，是每个 *Variable* 的 *grad_fn* 构成了图中的节点。所以，PyTorch 图的样子会是这样。

![](../Images/bd1ab5ae2e471579c37576784c3b1341.png)

每个 Function 是 PyTorch 计算图中的一个节点。

+   我已经通过它们的名称表示了叶子节点，但它们也有自己的 *grad_fn* （返回 None 值）。这很有意义，因为你不能在叶子节点之外进行反向传播。其余的节点现在被它们的 *grad_fn* 替换。我们看到单个节点 *d* 被三个 Functions、两个乘法和一个加法替换，而损失则被一个 *minus* Function 替换。

+   **计算梯度（第18行）。**我们现在通过在*L*上调用`*.backward()*`函数来计算梯度。这里到底发生了什么？首先，L的梯度是1（*dL / dL*）。**然后，我们调用它的*backward*函数，这个函数的任务基本上是计算*Function*对象的输出相对于*Function*对象输入的梯度。**在这里，L是10 - d的结果，这意味着，反向函数将计算梯度（*dL/dd*）为-1。

+   现在，这个计算出的梯度与累计梯度相乘（存储在与当前节点对应的*Variable*的*grad*属性中，在我们的例子中是*dL/dL = 1*），然后发送到输入节点，存储在与输入节点对应的*Variable*的***grad*属性中。**从技术上讲，我们所做的就是应用链式法则（*dL/dL*） × （*dL/dd*） = *dL/dd*。

+   现在，让我们理解梯度是如何在*Variable* *d*上进行传播的。*d*是从它的输入（w3, w4, b, c）计算出来的。在我们的图中，它包含3个节点，2次乘法和1次加法。

+   首先，函数*AddBackward*（代表我们图中节点*d*的加法操作）计算其输出（*w3*b + w4*c*）相对于其输入（*w3*b和w4*c*）的梯度（两者都是1）。现在，这些*local*梯度与累计梯度（*dL/dd* × *1* = -1）相乘，结果保存在各自输入节点的*grad*属性中。

+   然后，函数*MulBackward*（代表*w3*c*的乘法操作）计算其输入输出相对于其输入（*w3和c*）的梯度。局部梯度与累计梯度（*dL/d(w3*c)* = -1）相乘。结果值（*-1 × c* 和 *-1 × w3*）然后存储在*Variables*的*w3*和*c*的*grad*属性中。

+   所有节点的梯度都是以类似的方式计算的。

+   任何节点的*L*的梯度可以通过调用*grad*来访问，该节点对应的Variable，**前提是它是一个叶节点**（PyTorch的默认行为不允许访问非叶节点的梯度。稍后会详细说明）。现在我们得到了梯度，我们可以使用SGD或任何你喜欢的优化算法来更新权重。

```py
w1 = w1 — (learning_rate) * w1.grad    #update the wieghts using GD
```

如此等等。

### 自动微分的一些巧妙细节

所以，我不是告诉过你不能访问非叶节点的*grad*属性吗？是的，这是默认行为。你可以通过在定义后调用`*.retain_grad()*`来覆盖这一行为，然后你就能够访问它的*grad*属性。但真的，底下究竟发生了什么呢？

**动态计算图**

PyTorch 创建了一种叫做 **动态计算图** 的东西，这意味着图是在运行时动态生成的。**在调用 *Variable* 的 *forward* 函数之前，图中不存在 *Variable（它的 grad_fn）* 的节点。** 图是由于调用了多个 *Variables* 的 *forward* 函数而创建的。仅在此时，图和中间值（用于后续计算梯度）才会被分配缓冲区。当你调用 *backward()* 时，随着梯度的计算，这些缓冲区基本上会被释放，图也会被销毁。你可以尝试在图上多次调用 *backward()*，你会看到 PyTorch 会给你一个错误。这是因为图在第一次调用 *backward()* 时被销毁了，因此在第二次调用时没有图可供反向传播。

如果你再次调用 *forward*，将会生成一个全新的图，并为其分配新的内存。

**默认情况下，只有叶子节点的梯度（*grad* 属性）被保存，而非叶子节点的梯度则会被销毁。** 但这种行为可以根据上述描述进行更改。

这与 **静态计算图** 相对，静态计算图用于 TensorFlow，其中图是在运行程序 ***之前*** 声明的。动态图范式允许你在运行时对网络架构进行更改，因为图仅在代码片段运行时才会创建。这意味着图可能在程序的生命周期内被重新定义。然而，这在静态图中是不可能的，因为静态图是在运行程序之前创建的，仅在后续执行。动态图还使得调试变得更加容易，因为错误的源头更容易追踪。

### 一些行业技巧

**requires_grad**

这是 *Variable* 类的一个属性。默认情况下，它是 False。当你需要冻结某些层并在训练时阻止它们更新参数时，它非常方便。你可以简单地将 *requires_grad* 设置为 False，这些 *Variables* 将不会被包含在计算图中。因此，没有梯度会传播到它们，或者到那些依赖于这些层进行梯度流动的层。*requires_grad*，**当设置为 True 时是** **传染性的**，这意味着即使一个操作的一个操作数具有 *requires_grad* 设置为 True，结果也将是如此。

![](../Images/df9e5f91cec8821b5f0aff2c1a35d7ae.png)

**b** 不包括在图中。现在没有梯度通过 ***b*** 进行反向传播。**a** 现在仅从 **c** 获取梯度。即使 **w1** 的 requires_grad = True，也无法接收梯度。

**volatile**

这同样是 *Variable* 类的一个属性，它使得当它被设置为 True 时，*Variable* 被排除在计算图之外。它可能看起来与 *requires_grad* 非常相似，因为它也是 **当设置为 True 时是传染性的**。但它的优先级高于 *requires_grad*。一个 *requires_grad* 等于 True 和 *volatile* 等于 True 的变量，将不会被包含在计算图中。

你可能会想，为什么还需要另一个开关来覆盖*requires_grad*，而我们可以简单地将*requires_grad*设置为 False？让我稍微岔开一下。

在推断时不创建图形非常有用，因为我们不需要梯度。首先，消除了创建计算图形的开销，速度得到了提升。其次，如果我们创建了图形，并且没有在之后调用*backward*，则用于缓存值的缓冲区将永远不会释放，可能导致内存耗尽。

通常，我们在神经网络中有许多层，可能在训练时已将*requires_grad*设置为 True。为了防止在推断时创建图形，我们可以采取以下两种方法之一。将*requires_grad*设置为**False**（可能是152层？）。**或者，只在输入上设置*volatile*为 True，我们可以确保不会创建图形。**你可以选择。

![](../Images/91704abbdd0a52fd54d325cb261738ee.png)

对于**b或任何依赖于b的节点**，不会创建图形。

> **注意：** PyTorch 0.4 没有针对合并 Tensor/Variable 类的 volatile 参数。相反，推断代码应放在 torch.no_grad() 上下文管理器中。

```py
with torch.no_grad():
    -----  your inference code goes here ----
```

### 结论

这就是*Autograd*。了解*Autograd*的工作原理可以在你卡住或处理错误时节省很多麻烦。感谢你阅读到这里。我打算写更多关于 PyTorch 的教程，介绍如何使用内置函数快速创建复杂架构（或者，可能不是那么快，但比逐块编码更快）。敬请期待！

**进一步阅读**

1.  [理解反向传播](http://neuralnetworksanddeeplearning.com/chap2.html)

1.  [理解链式法则](https://www.youtube.com/watch?v=MKWBx78L7Qg)

1.  Python 类 [第 1 部分](https://www.hackerearth.com/practice/python/object-oriented-programming/classes-and-objects-i/tutorial/) 和 [第 2 部分](https://www.hackerearth.com/practice/python/object-oriented-programming/classes-and-objects-ii-inheritance-and-composition/tutorial/)

1.  [PyTorch 官方教程](http://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html)

**简历：[Ayoosh Kathuria](https://www.linkedin.com/in/ayoosh-kathuria-44a319132/)** 对计算机视觉充满热情，致力于教机器如何从环境中提取有意义的信息。他目前正在通过利用上下文来改进目标检测。

[原文](https://towardsdatascience.com/getting-started-with-pytorch-part-1-understanding-how-automatic-differentiation-works-5008282073ec)。经许可转载。

**相关：**

+   [构建神经网络的简单入门指南](/2018/02/simple-starter-guide-build-neural-network.html)

+   [比较深度学习框架：罗塞塔石碑方法](/2018/03/deep-learning-frameworks.html)

+   [排名前列的数据科学深度学习库](/2017/10/ranking-popular-deep-learning-libraries-data-science.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 工作

* * *

### 更多相关话题

+   [入门 PyTorch Lightning](https://www.kdnuggets.com/2022/12/getting-started-pytorch-lightning.html)

+   [用 5 个步骤入门 PyTorch](https://www.kdnuggets.com/5-steps-getting-started-pytorch)

+   [如何通过使用自动 EDA 工具来应对数据科学评估测试](https://www.kdnuggets.com/2022/04/ace-data-science-assessment-test-automatic-eda-tools.html)

+   [我如何使用 Grounding DINO 实现自动图像标注](https://www.kdnuggets.com/2023/05/automatic-image-labeling-grounding-dino.html)

+   [ChatGPT 如何工作：模型背后的秘密](https://www.kdnuggets.com/2023/04/chatgpt-works-model-behind-bot.html)

+   [Burtch Works 2023 数据科学与 AI 专业人士薪资报告…](https://www.kdnuggets.com/2023/08/burtch-works-2023-data-science-ai-professionals-salary-report.html)
