# 深度学习中的激活函数如何工作

> 原文：[`www.kdnuggets.com/2022/06/activation-functions-work-deep-learning.html`](https://www.kdnuggets.com/2022/06/activation-functions-work-deep-learning.html)

让我们从**激活函数**的定义开始：

> * * *
> 
> ## 我们的前 3 个课程推荐
> ## 
> ![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯
> 
> ![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能
> 
> ![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT
> 
> * * *
> 
> **“在人工神经网络中，每个神经元对其输入形成加权总和，并通过被称为激活函数的函数传递结果的标量值。”**
> 
> —[维基百科定义](https://en.wikipedia.org/wiki/Activation_function)

听起来有点复杂？别担心！阅读本文后，你将对激活函数有更好的理解。

![深度学习中的激活函数如何工作](img/8d4304aeeb1d39b7ac754f055981f1e6.png)

对于人类而言，我们的大脑接收来自外部世界的信息，对接收输入的神经元进行处理，并激活神经元尾部以生成所需的决策。同样，在神经网络中，我们提供图像、声音、数字等作为输入，对人工神经元进行处理，通过算法激活正确的最终神经元层以生成结果。

## 我们为什么需要激活函数？

激活函数决定了一个神经元是否应该**激活或不激活**。这意味着它将使用一些简单的数学运算来确定神经元对网络的输入在预测过程中是否相关。

激活函数的目的是向人工神经网络引入**非线性**，并从输入值集合中生成输出。

## 激活函数的类型

激活函数可以分为三种类型：

1.  线性激活函数

1.  二值阶跃函数

1.  非线性激活函数

### 线性激活函数

线性激活函数，通常称为**恒等激活函数**，与输入成正比。线性激活函数的范围为（-∞到∞）。线性激活函数只是将输入的加权总和相加并返回结果。

![深度学习中的激活函数如何工作](img/15a61eb4276c282836fc2e706c9442b6.png)

线性激活函数——图示

从数学角度来看，它可以表示为：

![深度学习中的激活函数如何工作](img/151bf2885a57ef72a7ac4c54ba18e9c8.png)

线性激活函数 — 方程

**优缺点**

+   这不是一个二元激活函数，因为线性激活函数仅提供一系列激活值。我们可以将几个神经元连接在一起，如果有多个激活值，我们可以基于此计算最大值（或软最大值）。

+   该激活函数的导数是一个常数。也就是说，梯度与 x（输入）无关。

### 二元阶跃激活函数

一个**阈值**决定了在二元阶跃激活函数中神经元是否应该被激活。

激活函数将输入值与阈值进行比较。如果输入值大于阈值，则激活神经元。如果输入值小于阈值，则禁用神经元，这意味着其输出不会传递到下一层或隐藏层。

![激活函数在深度学习中的工作原理](img/10e4ae23fa8708e44ca18e6a0e10b212.png)

二元阶跃函数 — 图

从数学上讲，二元激活函数可以表示为：

![激活函数在深度学习中的工作原理](img/47bd118991d87fcd6c5dfe70168733df.png)

二元阶跃激活函数 — 方程

**优缺点**

+   它不能提供多值输出——例如，它不能用于多类分类问题。

+   步进函数的梯度为零，这使得反向传播过程变得困难。

### 非线性激活函数

非线性激活函数是最常用的激活函数。它们使得人工神经网络模型能够适应各种数据并区分输出。

非线性激活函数允许堆叠多个神经元层，因为输出将是通过多个层的输入的非线性组合。任何输出都可以表示为神经网络中的功能计算输出。

这些激活函数主要根据其范围和曲线进行分类。本文余下部分将概述神经网络中使用的主要非线性激活函数。

# 1. Sigmoid

Sigmoid 接受一个数字作为输入，并返回一个介于 0 和 1 之间的数字。它简单易用，具备激活函数的所有理想特性：非线性、连续可微性、单调性和固定输出范围。

这主要用于**二元分类问题**。这个 Sigmoid 函数给出了特定类别存在的概率。

![激活函数在深度学习中的工作原理](img/1dd0239de89470cbb6c12ddd05c294c2.png)

Sigmoid 激活函数 — 图

从数学上讲，它可以表示为：

![激活函数在深度学习中的工作原理](img/a354aae8799bdd4770a600624273cb60.png)

Sigmoid 激活函数 — 方程

**优缺点**

+   它本质上是非线性的。该函数的组合也具有非线性特性，并且它将提供类比激活，不像二进制步进激活函数。它也有平滑的梯度，对于分类问题来说效果很好。

+   激活函数的输出始终会在 (0,1) 范围内，而线性激活函数的范围是 (-∞, ∞)。因此，我们为激活定义了一个范围。

+   Sigmoid 函数引发了**“梯度消失”**的问题，Sigmoid 会饱和并杀死梯度。

+   它的输出**不是零中心的**，这会导致梯度更新在不同方向上过度移动。输出值在零和一之间，使得优化变得更加困难。

+   网络要么拒绝学习更多内容，要么变得非常慢。

# 2. TanH（双曲正切）

TanH 将一个实值数压缩到范围**[-1, 1]**。它是非线性的，但与 Sigmoid 不同，其输出是**零中心的**。其主要优点在于，负输入将被强烈映射到负值，而零输入在 TanH 图中将被映射到接近零的位置。

![深度学习中的激活函数如何工作](img/95bba17f69f7a53fd4b21393d23585e5.png)

TanH 激活函数 — 图

从数学上讲，TanH 函数可以表示为：

![深度学习中的激活函数如何工作](img/fb603f8d328bf76b6ef20935478456c5.png)

TanH 激活函数 — 方程

**优点与缺点**

+   TanH 也存在梯度消失问题，但 TanH 的梯度比 sigmoid 更强（导数更陡）。

+   TanH 是零中心的，梯度不必朝着特定方向移动。

# 3. ReLU（修正线性单元）

ReLU 代表修正线性单元，是应用中最常用的激活函数之一。它解决了梯度消失的问题，因为 ReLU 函数的梯度最大值为一。它也解决了神经元饱和的问题，因为 ReLU 函数的斜率从未为零。ReLU 的范围在**0 和无限之间**。

![深度学习中的激活函数如何工作](img/0369c70833265a0315e80122dfe52e5f.png)

ReLU 激活函数 — 图

从数学上讲，它可以表示为：

![深度学习中的激活函数如何工作](img/0e329f7594f219053f638c84a2d05baa.png)

ReLU 激活函数 — 方程

**优点与缺点**

+   由于只有一定数量的神经元被激活，ReLU 函数在计算上比 sigmoid 和 TanH 函数更高效。

+   由于其线性、非饱和的特性，ReLU 加速了梯度下降向损失函数全局最小值的收敛。

+   其一个局限性是**它应该仅在人工神经网络模型的隐藏层中使用**。

+   在训练过程中，一些梯度可能会很脆弱。

+   换句话说，对于 ReLU 的 (x<0) 区域的激活，梯度将为 0，因此权重在下降过程中不会调整。这意味着那些进入该状态的神经元将停止响应输入的变化（仅仅因为梯度为 0，没有变化）。这就是**死 ReLU 问题**。

# 4. Leaky ReLU

Leaky ReLU 是 ReLU 激活函数的升级版，用于解决 ReLU 死亡问题，因为它在负区域有一个小的正斜率。但是，目前不同任务上的好处一致性尚不明确。

![深度学习中激活函数的工作原理](img/d9ad66067d83564315ce55cf1946de49.png)

Leaky ReLU 激活函数 — 图

从数学上表示为，

![深度学习中激活函数的工作原理](img/ad8ffedd0c6939c1bc789804bbfa5910.png)

Leaky ReLU 激活函数 — 方程

**优缺点**

+   Leaky ReLU 的优点与 ReLU 相同，此外它还允许反向传播，即使对于负输入值也是如此。

+   通过对负输入值进行微小修改，图形左侧的梯度将变成真实（非零）值。因此，该区域内不会再有死神经元。

+   对于负输入值，预测可能不稳定。

# 5. ELU（指数线性单元）

ELU 也是 ReLU 的一种变体，它也解决了死 ReLU 问题。ELU 就像 Leaky ReLU 一样，通过引入一个新的 alpha 参数并与另一个方程相乘来考虑负值。

ELU 在计算上比 Leaky ReLU 略贵，而且它与 ReLU 非常相似，只是在负输入值方面有所不同。对于正输入值，它们的形状都是单位函数。

![深度学习中激活函数的工作原理](img/b2f9279c8d4987ceab71751a00d71528.png)

ELU 激活函数-图

从数学上表示为：

![深度学习中激活函数的工作原理](img/a71669c3b75ad8457e02a84b81f41dfc.png)

ELU 激活函数 — 方程

**优缺点**

+   ELU 是 ReLU 的一个强有力的替代方案。与 ReLU 不同，ELU 可以产生负输出。

+   ELU 中有指数运算，因此增加了计算时间。

+   不涉及对 'a' 值的学习，并且存在梯度爆炸问题。

# 6. Softmax

多个 sigmoid 的组合称为 Softmax 函数。它确定相对概率。类似于 sigmoid 激活函数，Softmax 函数返回每个类别/标签的概率。**在多分类问题中，softmax 激活函数最常用于神经网络的最后一层。**

Softmax 函数给出了当前类别相对于其他类别的概率。这意味着它还考虑了其他类别的可能性。

![深度学习中激活函数的工作原理](img/ca087644f0ff080de42f7998ff705f24.png)

Softmax 激活函数 — 图

从数学角度看，它可以表示为：

![深度学习中激活函数的工作原理](img/778fe4f04a08fdf86f75f65b951e180e.png)

Softmax 激活函数 — 方程

**优缺点**

+   它比绝对值更好地模拟了编码标签。

+   如果我们使用绝对值（模值），信息会丢失，但指数函数会自动处理这一点。

+   **softmax 函数也应用于多标签分类**和回归任务。

# 7\. Swish

Swish 允许少量负权重的传播，而 ReLU 将所有非正权重设置为零。这是决定非单调平滑激活函数（如 Swish）在逐层深度神经网络中成功的关键特性。

它是由谷歌研究人员创建的自门控激活函数。

![深度学习中激活函数的工作原理](img/10c2ae768b3b654d151f1fdc1293d310.png)

Swish 激活函数 — 图

从数学角度看，它可以表示为：

![深度学习中激活函数的工作原理](img/4cf850d754d377b9c455429f5aef4209.png)

Swish 激活函数 — 方程

**优缺点**

+   Swish 是一个平滑的激活函数，这意味着它不像 ReLU 在 x 等于零附近那样突然改变方向。相反，它平滑地从 0 向小于 0 的值弯曲，然后再次向上。

+   在 ReLU 激活函数中，非正值被置为零。而负数可能对检测数据中的模式有价值。由于稀疏性，大的负数被消除，从而形成双赢局面。

+   Swish 激活函数由于非单调性，增强了输入数据和权重的学习。

+   计算上略显昂贵，并且随着时间的推移，算法可能会出现更多问题。

# 重要考虑事项

在选择合适的激活函数时，必须考虑以下问题和事项：

**梯度消失**是神经网络训练过程中常见的问题。像 sigmoid 激活函数一样，一些激活函数的输出范围很小（0 到 1）。因此，sigmoid 激活函数的输入发生巨大变化时，输出的修改很小。因此，导数也变得很小。这些激活函数仅用于只有几层的浅层网络。当这些激活函数应用于多层网络时，梯度可能变得过小，无法完成预期训练。

**梯度爆炸**是训练过程中产生大规模错误梯度的情况，导致神经网络模型权重的巨大更新。当发生梯度爆炸时，网络可能变得不稳定，训练无法完成。由于梯度爆炸，权重值可能增长到溢出的程度，从而导致**NaN**值的丢失。

# 最终总结

+   所有隐藏层通常使用相同的激活函数。**ReLU** 激活函数 **应仅** 在隐藏层中使用，以获得更好的结果。

+   由于梯度消失问题，**不应在** 隐藏层中使用 sigmoid 和 TanH 激活函数，因为它们使模型在训练过程中更容易出现问题。

+   Swish 函数用于深度 **超过 40 层** 的人工神经网络。

+   回归问题应使用线性激活函数

+   二分类问题应使用 sigmoid 激活函数

+   多分类问题应使用 softmax 激活函数

神经网络架构及其可用的激活函数，

+   卷积神经网络 (CNN)：ReLU 激活函数

+   循环神经网络 (RNN)：TanH 或 sigmoid 激活函数

你可以找到无数的文章评估激活函数的比较。我建议你亲自动手实践。

如果你有任何问题，可以在 [LinkedIn](https://www.linkedin.com/in/parthibanmarimuthu/) 或 [Twitter](https://twitter.com/parthibharathiv) 上随时问我。

**[Parthiban Marimuthu](https://www.linkedin.com/in/parthibanmarimuthu/)** ([@parthibharathiv](https://twitter.com/parthibharathiv)) 生活在印度钦奈，在 Spritle Software 工作。Parthiban 研究并转化数据科学原型，设计机器学习系统，并研究和实现适当的 ML 算法和工具。

### 更多相关话题

+   [揭开神经魔法的面纱：深入了解激活函数](https://www.kdnuggets.com/unveiling-neural-magic-a-dive-into-activation-functions)

+   [KDnuggets™ 新闻 22:n03，1 月 19 日：深入了解 13 个数据…](https://www.kdnuggets.com/2022/n03.html)

+   [KDnuggets 新闻，8 月 3 日：10 个最常用的 Tableau 函数 • 是…](https://www.kdnuggets.com/2022/n31.html)

+   [逻辑回归是如何工作的？](https://www.kdnuggets.com/2022/07/logistic-regression-work.html)

+   [了解不同的数据可视化如何工作](https://www.kdnuggets.com/2022/09/datacamp-learn-different-data-visualizations-work.html)

+   [大型语言模型是什么？它们是如何工作的？](https://www.kdnuggets.com/2023/05/large-language-models-work.html)
