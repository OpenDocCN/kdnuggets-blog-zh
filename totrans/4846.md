# 使用 PyTorch 计算简单导数

> 原文：[https://www.kdnuggets.com/2018/05/simple-derivatives-pytorch.html](https://www.kdnuggets.com/2018/05/simple-derivatives-pytorch.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

使用 PyTorch 求导数很简单。像许多其他神经网络库一样，PyTorch 包含一个自动微分包 `[autograd](https://pytorch.org/docs/master/autograd.html)`，它完成了繁重的工作。但在 PyTorch 中，导数的计算似乎尤其简单。

我希望在第一次学习如何将 [导数和神经网络的实际应用](https://www.kdnuggets.com/2017/10/neural-network-foundations-explained-gradient-descent.html)结合起来时，有一些具体的例子，展示如何使用这样的神经网络包来找出简单的导数并进行计算，这些计算与神经网络中的计算图分开。PyTorch 的架构使得这种教学示例变得容易。

我不太确定这是否对那些想了解 PyTorch 如何实现自动微分、如何实际计算导数，甚至是学习“寻找导数”是什么意思的人有用，但我们还是试试看吧。也许这对这些人都没有帮助。 :)

![PyTorch](../Images/05892c36023f36e716584053fc6b9e7a.png)

首先，我们需要一个函数来找出导数。我们随意使用这个：

![公式](../Images/f63c40b9d1ac08484e874bdc8ba90142.png)

我们应该记住，函数的导数可以被解释为切线的斜率，以及函数的变化率。

在我们使用 PyTorch 找出这个函数的导数之前，让我们先手动计算一下：

![公式](../Images/63f2148d099a0885d5fcf5aeb0161e92.png)

![公式](../Images/e9b358a522dc0f7f98dbb990318d9596.png)

上述是我们原始函数的 *一阶导数*。

现在让我们找出给定 *x* 值的导数函数值。我们随意使用 **2**：

![公式](../Images/c85e3f5373a0fb763a2f27eed1eb8bee.png)

![公式](../Images/f7e148a32128215ad6de52382a341e16.png)

![公式](../Images/90de01b6c4561049c41a251cdf100dc7.png)

![公式](../Images/69a4f46f44f8f44f74b2d1e673a6777b.png)

对 *x = 2* 求导数函数的结果是 **233**。这可以解释为在公式中，当 *x = 2* 时，*y* 对 *x* 的变化率是 233。

**使用 `autograd` 查找并求解导数**

我们如何使用 PyTorch 的 `autograd` 包做与上面相同的事情？

首先，我们需要将原始函数在 Python 中表示为如下形式：

```py

y = 5*x**4 + 3*x**3 + 7*x**2 + 9*x - 5
```

```py
import torch

x = torch.autograd.Variable(torch.Tensor([2]),requires_grad=True)
y = 5*x**4 + 3*x**3 + 7*x**2 + 9*x - 5

y.backward()
x.grad

```

上述代码逐行解释：

+   导入 torch 库

+   定义我们想计算导数的函数

+   将我们想要计算其导数的值（2）定义为PyTorch `Variable` 对象，并指定它应以能够跟踪计算图中连接位置的方式进行实例化，以便通过链式法则 (`requires_grad`) 执行微分。

+   使用 `autograd` 的 `[backward()](https://pytorch.org/docs/master/autograd.html#torch.autograd.backward)` 计算梯度的总和，使用链式法则

+   输出 *x* 张量的 `grad` 属性中存储的值，如下所示

```py

tensor([ 233.])
```

这个值233与我们上面手动计算的结果一致。

若要进一步使用PyTorch，包括使用 `autograd` 包和 [`Tensor`](https://www.kdnuggets.com/2018/05/pytorch-tensor-basics.html) 对象构建一些基础神经网络，我建议查看官方的 [PyTorch 60分钟快速教程](https://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html) 或 [这个教程](https://github.com/yunjey/pytorch-tutorial)。

**相关**：

+   [PyTorch张量基础](/2018/05/pytorch-tensor-basics.html)

+   [什么是张量?!?](/2018/05/wtf-tensor.html)

+   [提升你的数据科学技能。学习线性代数。](/2018/05/boost-data-science-skills-learn-linear-algebra.html)

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行IT管理

* * *

### 更多相关内容

+   [5个简单步骤系列：掌握Python、SQL、Scikit-learn、PyTorch及其他](https://www.kdnuggets.com/5-simple-steps-series-master-python-sql-scikit-learn-pytorch-google-cloud)

+   [可解释的PyTorch神经网络](https://www.kdnuggets.com/2022/01/interpretable-neural-networks-pytorch.html)

+   [完整免费PyTorch深度学习课程](https://www.kdnuggets.com/2022/10/complete-free-pytorch-course-deep-learning.html)

+   [PyTorch Lightning入门](https://www.kdnuggets.com/2022/12/getting-started-pytorch-lightning.html)

+   [调优PyTorch中的Adam优化器参数](https://www.kdnuggets.com/2022/12/tuning-adam-optimizer-parameters-pytorch.html)

+   [YOLOv5 PyTorch教程](https://www.kdnuggets.com/2022/12/yolov5-pytorch-tutorial.html)
