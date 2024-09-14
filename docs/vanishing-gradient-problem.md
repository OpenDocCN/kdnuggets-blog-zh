# 梯度消失问题：原因、后果及解决方案

> 原文：[https://www.kdnuggets.com/2022/02/vanishing-gradient-problem.html](https://www.kdnuggets.com/2022/02/vanishing-gradient-problem.html)

![梯度消失问题：原因、后果及解决方案](../Images/0a76a6fdac48e94ed85b1ee128af5a45.png)

图片由编辑提供

Sigmoid 函数是用于开发深度神经网络的最流行的激活函数之一。Sigmoid 函数的使用限制了深度神经网络的训练，因为它导致了梯度消失问题。这使得神经网络的学习速度变慢，甚至在某些情况下完全无法学习。这篇博客文章旨在描述梯度消失问题，并解释使用 Sigmoid 函数如何导致这个问题。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT

* * *

# Sigmoid 函数

Sigmoid 函数在神经网络中经常用于激活神经元。它是一个具有特征 S 形状的对数函数。函数的输出值介于 0 和 1 之间。Sigmoid 函数用于在二分类问题中激活输出层。它的计算公式如下：

![梯度消失问题解析](../Images/d8d2fa8be9674346f77dba068732404e.png)

在下面的图表中，你可以看到 Sigmoid 函数本身及其导数的比较。Sigmoid 函数的一阶导数是钟形曲线，其值范围从 0 到 0.25。

![梯度消失问题解析](../Images/c5aeb35ef714f979fd9fdbfb64261d9e.png)

我们对神经网络如何执行前向传播和反向传播的了解对于理解梯度消失问题至关重要。

![梯度消失问题解析](../Images/862e4f3d9e4cf38da6981eeb8a049e45.png)

# 前向传播

神经网络的基本结构包括输入层、一个或多个隐藏层和一个输出层。网络的权重在前向传播过程中被随机初始化。输入特征在每个隐藏层节点处与相应的权重相乘，并在每个节点处加上一个偏置。然后，这个值通过激活函数转换为节点的输出。为了生成神经网络的输出，隐藏层输出乘以权重加上偏置值，然后使用另一个激活函数进行转换。这将是神经网络对给定输入值的预测值。

![消失梯度问题解释](../Images/51d627e5d0acde8bababdde965dc010c.png)

# 反向传播

当网络生成输出时，损失函数(C)表示它预测输出的准确性。网络执行反向传播以最小化损失。反向传播方法通过调整神经网络的权重和偏置来最小化损失函数。在此方法中，损失函数的梯度相对于网络中的每个权重进行计算。

在反向传播中，节点的新权重(w[new])是通过旧权重(w[old])和学习率(ƞ)与损失函数梯度的乘积来计算的。![消失梯度问题解释](../Images/26acc3e4c6a799aac5744ac88dcdb036.png)

![消失梯度问题解释](../Images/be7ea7065f53e1ede2b5b33d9f3e3cf4.png)

利用偏导数链式法则，我们可以将损失函数的梯度表示为所有节点的激活函数相对于其权重的梯度的乘积。因此，网络中节点的更新权重依赖于每个节点激活函数的梯度。

对于具有sigmoid激活函数的节点，我们知道sigmoid函数的偏导数最大值为0.25。当网络中层数增加时，导数的乘积值会减少，直到某个点，损失函数的偏导数接近零，偏导数消失。我们称之为消失梯度问题。

在浅层网络中，sigmoid函数可以使用，因为梯度的小值不会造成问题。然而，对于深层网络，消失梯度可能对性能产生重大影响。由于导数消失，网络的权重保持不变。在反向传播过程中，神经网络通过更新其权重和偏置来减少损失函数。在具有消失梯度的网络中，权重无法更新，因此网络无法学习。结果是网络性能下降。

# 克服问题的方法

梯度消失问题是由用于创建神经网络的激活函数的导数引起的。解决这个问题的最简单方法是替换网络的激活函数。可以用 ReLU 等激活函数代替 sigmoid。

修正线性单元（ReLU）是激活函数，当它们应用于正输入值时会生成正线性输出。如果输入为负值，该函数将返回零。

![梯度消失问题解释](../Images/3955a28d80709d4d297fc54a5546ba89.png)

ReLU 函数的导数定义为对于大于零的输入为 1，对于负输入为 0。下面共享的图表显示了 ReLU 函数的导数。

![梯度消失问题解释](../Images/47848f7a08be7ca8124605fd7199f9c0.png)

如果在神经网络中用 ReLU 函数代替 sigmoid 函数进行激活，损失函数的偏导数值将为 0 或 1，这防止了梯度消失。因此，ReLU 函数可以防止梯度消失。使用 ReLU 的问题是，当梯度值为 0 时，节点被视为死节点，因为权重的旧值和新值保持不变。这种情况可以通过使用 leaky ReLU 函数来避免，该函数防止梯度降到零值。

避免梯度消失问题的另一种技术是权重初始化。这是将初始值分配给神经网络中的权重的过程，以便在反向传播过程中，权重不会消失。

总之，梯度消失问题源于用于创建神经网络的激活函数的偏导数的特性。使用 Sigmoid 激活函数的深度神经网络中，这个问题可能会更加严重。通过使用像 ReLU 和 leaky ReLU 这样的激活函数，可以显著减少这个问题。

**[蒂娜·雅各布](https://www.linkedin.com/in/tina-jacob-77068253/)** 对数据科学充满热情，认为生活就是不断学习和成长，无论遇到什么。

### 了解更多相关话题

+   [你应该了解的5个梯度下降和成本函数的概念](https://www.kdnuggets.com/2020/05/5-concepts-gradient-descent-cost-function.html)

+   [90%的今日代码是为了防止失败，这也是一个问题](https://www.kdnuggets.com/2022/07/90-today-code-written-prevent-failure-problem.html)

+   [回到基础，第二部分：梯度下降](https://www.kdnuggets.com/2023/03/back-basics-part-dos-gradient-descent.html)

+   [梯度下降：优化的登山者指南…](https://www.kdnuggets.com/gradient-descent-the-mountain-trekker-guide-to-optimization-with-mathematics)

+   [NLP应用的范围：一个不同的…](https://www.kdnuggets.com/2022/03/different-solution-problem-range-nlp-applications-real-world.html)

+   [用 Python 解决背包问题的遗传编程](https://www.kdnuggets.com/2023/01/knapsack-problem-genetic-programming-python.html)
