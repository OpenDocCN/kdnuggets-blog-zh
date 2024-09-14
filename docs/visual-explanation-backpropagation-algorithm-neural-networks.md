# 神经网络反向传播算法的可视化解释

> 原文：[https://www.kdnuggets.com/2016/06/visual-explanation-backpropagation-algorithm-neural-networks.html](https://www.kdnuggets.com/2016/06/visual-explanation-backpropagation-algorithm-neural-networks.html)

假设我们真的喜欢登山，并为了增加一点额外的挑战，我们这次蒙上眼睛，以便看不到我们的位置，也不知道何时完成了我们的“目标”，即到达山顶。

由于我们看不到前方的路径，我们让直觉引导我们：假设山顶是山的“最高”点，我们认为最陡峭的路径最有效地将我们引向山顶。

我们通过迭代地“感知”周围并朝着最陡峭的上升方向迈出一步来解决这个挑战——我们称之为“梯度上升”。但如果我们到达一个无法进一步上升的点怎么办？即，每个方向都向下？此时，我们可能已经到达了山顶，但也可能只是到达了一个较小的高原……我们并不知道。从本质上讲，这只是梯度上升优化的一个类比（基本上是通过梯度下降最小化成本函数的对立面）。然而，这并非特指反向传播，而只是最小化一个凸成本函数（如果只有一个全局最小值）或非凸成本函数（具有局部最小值，如“高原”让我们以为达到了山顶）的其中一种方法。借助一点视觉辅助，我们可以将一个只有一个参数的非凸成本函数（蓝色球为我们当前位置）形象化如下：

[![非凸成本](../Images/b7cb1df71cfdd2e82bb55a3beeeb8c30.png)](https://github.com/rasbt/python-machine-learning-book/blob/master/faq/visual-backpropagation/nonconvex-cost.png)

现在，反向传播就是在多个“层级”上反向传播成本。例如，如果我们有一个多层感知器，我们可以将前向传播（将输入信号通过网络，并通过相应的权重计算输出）形象化如下：

[![前向传播](../Images/9c05363841f1cd37e9b476c669cb53fe.png)](https://github.com/rasbt/python-machine-learning-book/blob/master/faq/visual-backpropagation/forward-propagation.png)

在反向传播中，我们“简单地”反向传播误差（我们通过比较计算出的输出和已知的正确目标输出来计算的“成本”，然后用来更新模型参数）：

[![反向传播](../Images/026031521802271f37a0fdb30bc165f3.png)](https://github.com/rasbt/python-machine-learning-book/blob/master/faq/visual-backpropagation/backpropagation.png)

这可能是在预备微积分之后很久的事了，但它本质上都是基于我们用于嵌套函数的简单链式法则

[![链式法则](../Images/e05c3db54124e253d0e75c7cff434527.png)](https://github.com/rasbt/python-machine-learning-book/blob/master/faq/visual-backpropagation/chain_rule_1.png)

[![链式法则](../Images/f9c99e2855716c634e5701100e8ad8f9.png)](https://github.com/rasbt/python-machine-learning-book/blob/master/faq/visual-backpropagation/chain_rule_2.png)

与其“手动”完成这些工作，不如使用计算工具（称为“自动微分”），而反向传播基本上是这种自动微分的“反向”模式。为什么是反向而不是前向？因为计算上更便宜！如果我们使用前向方式，我们将逐层相乘大矩阵，直到将一个大矩阵与输出层中的一个向量相乘。然而，如果我们从反向开始，也就是从将一个矩阵与一个向量相乘开始，我们得到另一个向量，以此类推。所以，我认为反向传播的美在于我们进行的是更高效的矩阵-向量乘法，而不是矩阵-矩阵乘法。

**简介：[Sebastian Raschka](https://twitter.com/rasbt)** 是一名数据科学家和机器学习爱好者，对 Python 和开源有着极大的热情。《[Python 机器学习](https://www.packtpub.com/big-data-and-business-intelligence/python-machine-learning)》的作者。密歇根州立大学。

[原文](https://github.com/rasbt/python-machine-learning-book/blob/master/faq/visual-backpropagation.md)。经许可转载。

**相关内容：**

+   [深度学习何时比 SVM 或随机森林效果更好？](/2016/04/deep-learning-vs-svm-random-forest.html)

+   [分类发展作为一种学习机器](/2016/04/development-classification-learning-machine.html)

+   [为什么从头实现机器学习算法？](/2016/05/implement-machine-learning-algorithms-scratch.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

### 了解更多相关内容

+   [线性回归与逻辑回归：简明解释](https://www.kdnuggets.com/2022/03/linear-logistic-regression-succinct-explanation.html)

+   [KDnuggets 新闻 22:n12，3月23日：最佳数据科学书籍…](https://www.kdnuggets.com/2022/n12.html)

+   [协同过滤的直观解释](https://www.kdnuggets.com/2022/09/intuitive-explanation-collaborative-filtering.html)

+   [思想传播：一种复杂推理的类比方法……](https://www.kdnuggets.com/thought-propagation-an-analogical-approach-to-complex-reasoning-with-large-language-models)

+   [在使用神经网络之前尝试的10个简单方法](https://www.kdnuggets.com/2021/12/10-simple-things-try-neural-networks.html)

+   [可解释的 PyTorch 神经网络](https://www.kdnuggets.com/2022/01/interpretable-neural-networks-pytorch.html)
