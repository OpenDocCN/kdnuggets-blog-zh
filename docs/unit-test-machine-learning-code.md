# 如何对机器学习代码进行单元测试

> 原文：[https://www.kdnuggets.com/2017/11/unit-test-machine-learning-code.html](https://www.kdnuggets.com/2017/11/unit-test-machine-learning-code.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者：Chase Roberts，机器学习工程师**

![AI](../Images/7c1f9253aaea96785bcd1cea1033e67f.png)

资料来源：[provalisresearch.com/blog/machine-learning](https://provalisresearch.com/blog/machine-learning/)

在过去的一年里，我的大部分工作时间都用在了深度学习研究和实习上。那一年中有很大一部分时间是在犯非常大的错误，这帮助我不仅学到了机器学习的知识，还学会了如何正确和合理地设计这些系统。我在 Google Brain 期间学到的一个主要原则是，单元测试可以决定你的算法的成败，并且可以节省你几周的调试和训练时间。

然而，似乎在线上并没有一个可靠的教程来教你如何*编写*神经网络代码的单元测试。即使像 OpenAI 这样的地方，也只是通过[盯着他们的每一行代码，试图思考为何会导致错误](https://blog.openai.com/openai-baselines-dqn/)。显然，我们中的大多数人没有那种时间或自我厌恶，因此希望这个教程能帮助你理智地开始测试你的系统！

让我们从一个简单的例子开始。试着找出这个代码中的错误。

你看到了吗？网络实际上并没有堆叠。当我写这段代码时，我复制并粘贴了 slim.conv2d(…) 这一行，只修改了内核大小，而没有修改实际的输入。

我很尴尬地说，这实际上发生在我大约一周前……但这是一个重要的教训！这些错误很难捕捉，有几个原因。

1.  这段代码从未崩溃、抛出错误，甚至没有变慢。

1.  这个网络仍然可以训练，损失值仍然会下降。

1.  经过几个小时的训练，值会收敛，但结果非常差，让你不知所措，不知道需要修复什么。

当你唯一的反馈是最终的验证误差时，你唯一需要搜索的地方就是整个网络架构。不用说，你需要一个更好的系统。

那么我们如何在进行完整的多日训练之前*捕捉*到这个问题呢？最容易注意到的是，这些层的值实际上从未到达函数外的任何其他张量。因此，假设我们有某种损失和优化器，这些张量永远不会被优化，因此它们总是会保持默认值。

我们可以通过简单地进行一次训练步骤并比较其前后状态来检测它。

哇。在不到 15 行代码的情况下，我们现在验证了至少我们创建的所有变量都得到了训练。

这个测试非常简单且非常有用。假设我们解决了之前的问题，现在我们想开始添加一些批量归一化。看看你能否发现错误。

你看到了吗？这个问题非常微妙。你看，在tensorflow中，batch_norm的`is_training`默认值是*False*，所以添加这一行代码实际上不会在训练期间对输入进行归一化！值得庆幸的是，我们写的最后一个单元测试将立即捕捉到这个问题！（我知道，因为这发生在我三天前。）

让我们再做一个例子。这实际上来自于我一天看到的一个[reddit帖子](https://www.reddit.com/r/MachineLearning/comments/6qyvvg/p_tensorflow_response_is_making_no_sense/)。我不会深入细节，但基本上这个人想创建一个在(0, 1)范围内输出的分类器。看看你是否能找到问题所在。

发现了错误吗？这个问题在事前很难发现，而且可能导致非常混淆的结果。基本上，发生的情况是预测只有一个输出，当你对其应用[softmax交叉熵](https://en.wikipedia.org/wiki/Softmax_function)时，会导致损失总是为0。

测试这一点的一个简单方法是，嗯……确保损失从不为0。

另一个好的测试是类似于我们的第一个测试，但反向的。你可以确保只有你想训练的变量实际上被训练。以GAN为例。一个常见的错误是意外忘记设置在何种优化期间训练哪些变量。这种代码时常发生。

最大的问题是优化器的默认设置是优化所有变量。在像GAN这样的高级架构中，这对你所有的训练时间来说是致命的。然而，你可以通过编写像这样的测试轻松检测到这些错误：

一个非常类似的测试可以写在判别器上。而且这个相同的测试也可以用于很多强化学习算法。许多actor-critic模型有需要通过不同损失优化的独立网络。

这里是我建议你在测试中遵循的一些模式。

1.  保持它们的确定性。如果一个测试以奇怪的方式失败，而你永远无法重现它，那真的很糟糕。如果你*真的*想要随机输入，请确保给随机数设定种子，以便你可以轻松地重新运行测试。

1.  保持测试简短。不要有一个训练到收敛并且与验证集进行检查的单元测试。如果你这样做，你是在浪费自己的时间。

1.  确保在每个测试之间重置图。

总结来说，这些黑箱算法仍然有很多测试的方法！花一个小时编写测试可以节省你数天的重新训练，并大大提高你的研究成果。如果因为我们的实现有问题而不得不舍弃完美的想法，那不是很糟糕吗？

这个列表显然不是全面的，但它是一个很好的开始！如果你有额外的建议或发现的有用的特定测试，请在[twitter](https://twitter.com/TheNerdStation)上给我留言！我很想做这方面的第二部分。

本文中的所有观点均为我的个人经验反映，并未得到 Google 的赞助或支持。

**简介: [Chase Roberts](http://thenerdstation.github.io/)** 是前 Google Brain 研究实习生。曾参与机器学习机器人团队的工作，专注于渐进神经网络在迁移学习中的实现及深入分析，同时也参与了强化学习架构的实现。

[原文](https://medium.com/@keeper6928/how-to-unit-test-machine-learning-code-57cf6fd81765)。经授权转载。

**相关：**

+   [TensorFlow: 需要优化哪些参数？](/2017/11/tensorflow-parameters-optimize.html)

+   [软件工程与机器学习概念](/2017/03/software-engineering-vs-machine-learning-concepts.html)

+   [使用 TensorFlow 进行线性回归的预测分析](/2017/11/tensorflow-predictive-analytics-linear-regression.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

### 了解更多主题

+   [如何在 Python 中进行单元测试？](https://www.kdnuggets.com/2023/01/perform-unit-testing-python.html)

+   [如何通过使用自动 EDA 工具来赢得数据科学评估测试](https://www.kdnuggets.com/2022/04/ace-data-science-assessment-test-automatic-eda-tools.html)

+   [在 Python 中执行 T 测试](https://www.kdnuggets.com/2023/01/performing-ttest-python.html)

+   [超越准确性：使用 NLP 测试库评估和改进模型](https://www.kdnuggets.com/2023/04/john-snow-beyond-accuracy-nlp-test-library.html)

+   [宣布 PyCaret 3.0：开源、低代码 Python 机器学习](https://www.kdnuggets.com/2023/03/announcing-pycaret-30-opensource-lowcode-machine-learning-python.html)

+   [KDnuggets 新闻，4月27日：关于 Papers With Code 的简要介绍；…](https://www.kdnuggets.com/2022/n17.html)
