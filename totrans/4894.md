# 学习率在人工神经网络中有用吗？

> 原文：[https://www.kdnuggets.com/2018/01/learning-rate-useful-neural-network.html](https://www.kdnuggets.com/2018/01/learning-rate-useful-neural-network.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

这篇文章将帮助你理解我们为什么需要学习率以及它是否对训练人工神经网络有用。通过使用一个非常简单的单层感知器Python代码，我们将改变学习率值以理解其概念。

**介绍**

人工神经网络的初学者面临的一个障碍是学习率。我被问过很多次关于学习率在训练人工神经网络（ANNs）中的作用。我们为什么要使用学习率？学习率的最佳值是什么？在这篇文章中，我将通过提供一个示例来简化问题，展示学习率在训练ANN时的作用。我将从解释我们的Python代码示例开始，然后再讨论学习率。

**示例**

一个非常简单的示例被用来摆脱复杂性，让我们专注于学习率。一个数值输入将应用于一个单层感知器。如果输入为250或更小，则其值将作为网络的输出返回。如果输入大于250，则将被裁剪为250。下表展示了用于训练的6个样本。

![](../Images/bdcb17c10de3c2ec4e956f201ac6d85a.png)

**ANN架构**

使用的ANN架构如下一图所示。该架构仅包含输入层和输出层。输入层只有一个神经元用于我们的单一输入。输出层也只有一个神经元用于生成输出。输出层的神经元负责将输入映射到正确的输出。同时，输出层的神经元上还应用了一个偏置，其权重为***b***，值为***+1***。输入层还有一个权重***W***。

![](../Images/94a9409e811a7869b85ea922f5cc67ea.png)

**激活函数**

本示例中使用的激活函数的方程和图形如下图所示。当输入小于或等于250时，输出将与输入相同。否则，输出将被裁剪为250。

![](../Images/97066b4183b4e99264208931ddd63d88.png)

**使用Python的实现**

实现整个网络的Python代码如下所示。我们将讨论代码的所有部分，尽量将其简化，然后专注于改变学习率以了解其对网络训练的影响。

```py
1.  import numpy  
2.    
3.  def activation_function(inpt):  
4.      if(inpt > 250):  
5.          return 250 # clip the result to 250  
6.      else:  
7.          return inpt # just return the input  
8.    
9.  def prediction_error(desired, expected):  
10\.     return numpy.abs(numpy.mean(desired-expected)) # absolute error  
11\.   
12\. def update_weights(weights, predicted, idx):  
13\.     weights = weights + .00001*(desired_output[idx] - predicted)*inputs[idx] # updating weights  
14\.     return weights # new updated weights  
15\.   
16\. weights = numpy.array([0.05, .1]) #bias & weight of input  
17\. inputs = numpy.array([60, 40, 100, 300, -50, 310]) # training inputs  
18\. desired_output = numpy.array([60, 40, 150, 250, -50, 250]) # training outputs  
19\.   
20\. def training_loop(inpt, weights):  
21\.     error = 1  
22\.     idx = 0 # start by the first training sample  
23\.     iteration = 0 #loop iteration variable  
24\.     while(iteration < 2000 or error >= 0.01): #while(error >= 0.1):  
25\.         predicted = activation_function(weights[0]*1+weights[1]*inputs[idx])  
26\.         error = prediction_error(desired_output[idx], predicted)  
27\.         weights = update_weights(weights, predicted, idx)  
28\.         idx = idx + 1 # go to the next sample  
29\.         idx = idx % inputs.shape[0] # restricts the index to the range of our samples  
30\.         iteration = iteration + 1 # next iteration  
31\.     return error, weights  
32\.   
33\. error, new_weights = training_loop(inputs, weights)  
34\. print('--------------Final Results----------------')  
35\. print('Learned Weights : ', new_weights)  
36\. new_inputs = numpy.array([10, 240, 550, -160])  
37\. new_outputs = numpy.array([10, 240, 250, -160])  
38\. for i in range(new_inputs.shape[0]):  
39\.     print('Sample ', i+1, '. Expected = ', new_outputs[i], ' , 
          Predicted = ', activation_function(new_weights[0]*1+new_weights[1]*new_inputs[i]))  

```

第17行和第18行负责创建两个数组（inputs和desired_output），用于存储前面表格中展示的训练输入和输出数据。每个输入将根据所使用的激活函数产生相应的输出。

第16行创建了一个网络权重的数组。只有两个权重：一个用于输入，另一个用于偏置。它们被随机初始化为偏置0.05和输入0.1。

激活函数本身是通过第3到7行定义的 activation_function(inpt) 方法实现的。它接受一个参数，即输入，并返回一个值，即期望输出。

由于预测中可能存在误差，我们需要测量该误差以了解我们离正确预测的距离。因此，有一个从第9到10行实现的方法，称为 prediction_error(desired, expected)，它接受两个输入：期望输出和实际输出。这个方法只是计算每个期望输出和实际输出之间的绝对差异。任何误差的最佳值当然是0。这是最终值。

如果出现预测误差怎么办？在这种情况下，我们必须对网络进行更改。但是具体要更改什么呢？是网络权重。为了更新网络权重，有一个方法叫做 update_weights(weights, predicted, idx)，定义在第13到14行。它接受三个输入：旧权重、预测输出和具有错误预测的输入的索引。这个方法应用以下方程来更新权重：

![](../Images/dc8b0e0c701ece674dbc601279191261.png)

这个方程使用当前步骤（n）的权重生成下一步骤（n+1）的权重。这个方程是我们用来了解学习率如何影响学习过程的。

最后，我们需要将所有这些拼接在一起以使网络学习。这是通过从第20行到31行定义的 training_loop(inpt, weights) 方法完成的。它进入一个训练循环。这个循环用于将输入映射到其输出，同时使预测误差最小化。

循环执行三个操作：

1.      输出预测。

2.      误差计算。

3.      更新权重。

在了解了示例及其 Python 代码后，让我们开始展示学习率在获得最佳结果方面的作用。

**学习率**

在前面讨论的示例中，第13行有学习率被使用的权重更新方程。首先，让我们假设我们没有完全使用学习率。方程将如下：

```py
weights = weights + (desired_output[idx] - predicted)*inputs[idx]

```

让我们看看去除学习率的效果。在训练循环的迭代中，网络有以下输入（b=0.05 和 W=0.1，输入 = 60，期望输出=60）。

期望输出，即第25行激活函数的结果，将是 activation_function(0.05(+1) + 0.1(60))。预测输出将是6.05。

在第26行，预测误差将通过计算期望输出和预测输出之间的差异来计算。误差将是 abs(60-6.05)=53.95。

然后在第27行，权重将根据上述方程进行更新。新的权重将是[0.05, 0.1] + (53.95)*60 = [0.05, 0.1] + 3237 = [3237.05, 3237.1]。

看起来新的权重与之前的权重差异太大。每个权重增加了3,237，这实在太大了。但让我们继续进行下一次预测。

在下一次迭代中，网络将应用以下输入：（b=3237.05 和 W=3237.1，输入=40，期望输出=40）。预期输出将是activation_function((3237.05 + 3237.1(40)) = 250。预测误差将是abs(40 - 250) = 210。误差非常大，比之前的误差还要大。因此，我们必须再次更新权重。根据上述方程，新的权重将是[3237.05, 3237.1] + (-210)*40 = [3237.05, 3237.1] + -8400 = [-5162.95, -5162.9]。

下一张表总结了前三次迭代的结果：

![](../Images/f274b272653f8d81d3df3858f7954883.png)

随着迭代次数的增加，结果变得更差。权重的大小迅速变化，有时甚至改变了符号。它们从非常大的正值变为非常大的负值。我们如何停止权重的这种大幅度和突变？如何缩小更新权重的值？

如果我们查看上一张表中权重变化的值，似乎这个值非常大。这意味着网络以较大的速度更改其权重。这就像一个人在短时间内做出大幅度的移动。某个时刻，这个人在远东，而在很短的时间内，这个人会在远西。我们只需要让它变得更慢。

如果我们能够将这个值缩小到更小的程度，那么一切都会变得顺利。但是怎么做呢？

回到生成这个值的代码部分，看来更新方程就是生成它的原因。具体来说，就是这一部分：

```py
(desired_output[idx] - predicted)*inputs[idx]

```

我们可以通过将这部分乘以一个小值（例如0.1）来缩放它。因此，第一次迭代中生成的3237.0的更新值将减少到323.7。我们甚至可以将这个值缩小到更小的值，例如0.001。使用0.001时，值将仅为3.327。

我们现在可以理解了。这一缩放值就是学习率。选择较小的学习率使权重更新的速度较小，并避免突变。随着值变大，变化速度加快，从而导致不良结果。

**但学习率的最佳值是什么？**

我们不能说哪个值是学习率的最佳值。学习率是一个超参数。超参数的值通过实验来确定。我们尝试不同的值，并使用那些给出最佳结果的值。有一些方法可以帮助你选择超参数的值。

**测试网络**

对于我们的问题，我推测值为 .00001 是合适的。在使用这个学习率训练网络后，我们可以进行测试。下表显示了对 4 个新测试样本的预测结果。似乎在使用学习率后结果有了很大改善。

![](../Images/2face38a2a1266e29bdb60faf7af8beb.png)

**简介： [艾哈迈德·贾德](https://www.linkedin.com/in/ahmedfgad/)** 于 2015 年 7 月从埃及梅努费亚大学计算机与信息学院获得信息技术学士学位，成绩优异并获得荣誉。由于在学院中排名第一，他被推荐在 2015 年于埃及的一所学院担任助教，随后在 2016 年继续担任助教和研究员。他当前的研究兴趣包括深度学习、机器学习、人工智能、数字信号处理和计算机视觉。

[原文](https://www.linkedin.com/pulse/learning-rate-useful-artificial-neural-networks-ahmed-gad/)。经许可转载。

**相关：**

+   [TensorFlow：一步一步构建前馈神经网络](/2017/10/tensorflow-building-feed-forward-neural-networks-step-by-step.html)

+   [3 个受欢迎的深度学习课程概述](/2017/10/3-popular-courses-deep-learning.html)

+   [5 个免费资源帮助你进一步理解深度学习](/2017/10/5-free-resources-furthering-understanding-deep-learning.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 更多相关内容

+   [基础率谬误及其对数据科学的影响](https://www.kdnuggets.com/2023/04/base-rate-fallacy-impact-data-science.html)

+   [通过 LinkedIn 个人资料提高你的回访率](https://www.kdnuggets.com/increase-your-callback-rate-with-a-linkedin-profile)

+   [神经网络与深度学习：教材（第 2 版）](https://www.kdnuggets.com/2023/07/aggarwal-neural-networks-deep-learning-textbook-2nd-edition.html)

+   [神经网络前尝试的 10 个简单步骤](https://www.kdnuggets.com/2021/12/10-simple-things-try-neural-networks.html)

+   [可解释的 PyTorch 神经网络](https://www.kdnuggets.com/2022/01/interpretable-neural-networks-pytorch.html)

+   [深度神经网络不会引导我们走向 AGI](https://www.kdnuggets.com/2021/12/deep-neural-networks-not-toward-agi.html)
