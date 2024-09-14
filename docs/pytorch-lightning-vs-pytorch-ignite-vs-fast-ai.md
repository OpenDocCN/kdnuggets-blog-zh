# Pytorch Lightning 与 PyTorch Ignite 与 Fast.ai

> 原文：[`www.kdnuggets.com/2019/08/pytorch-lightning-vs-pytorch-ignite-vs-fast-ai.html`](https://www.kdnuggets.com/2019/08/pytorch-lightning-vs-pytorch-ignite-vs-fast-ai.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**由[威廉·法尔孔](https://www.linkedin.com/in/wfalcon/)，AI 研究员**

![图示](img/631434f0400fb0206fffbdd18564e11f.png)

显然，狮子、熊和老虎是朋友

* * *

## 我们的前三名课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT

* * *

[PyTorch-lightning](https://github.com/williamFalcon/pytorch-lightning) 是一个最近发布的库，是一个类似 Keras 的 PyTorch 机器学习库。它将核心训练和验证逻辑留给你，并自动化其他部分。（顺便提一下，这里的 Keras 意味着没有样板代码，而不是过于简化）。

作为 Lightning 的核心作者，我曾多次被问及 Lightning 与 fast.ai、[PyTorch ignite](https://github.com/pytorch/ignite)之间的核心差异。

在这里，我将**尝试**对这三种框架进行客观比较。这一比较基于在所有三个框架的教程和文档中客观发现的相似性和差异性。

### 动机

Fast.ai 最初是为了方便教学[fast.ai 课程](https://www.fast.ai/2018/10/02/fastai-ai/)而创建的。最近，它还转变成了一个实现常见方法（如 GANs、RL 和迁移学习）的库。

[PyTorch Ignite](https://github.com/pytorch/ignite#why-ignite)和[Pytorch Lightning](https://github.com/williamFalcon/pytorch-lightning#why-do-i-want-to-use-lightning)都是为了给予研究人员尽可能多的灵活性，要求他们定义训练循环和验证循环中的操作。

Lightning 有两个额外的、更雄心勃勃的目标：可重复性和普及最佳实践，只有 PyTorch 高级用户会实现（分布式训练、16 位精度等）。我将在后续章节中详细讨论这些目标。

因此，从基本层面上讲，目标用户是明确的：对于 fast.ai，目标用户是**对深度学习感兴趣的人**，而另外两个框架则专注于**活跃的研究人员**，无论是从事机器学习还是使用机器学习（例如：生物学家、神经科学家等）。

### 学习曲线

Lightning 和 Ignite 都具有非常简单的接口，因为大部分工作仍然由用户在纯 PyTorch 中完成。主要的工作发生在 [Engine](https://pytorch.org/ignite/engine.html) 和 [Trainer](https://williamfalcon.github.io/pytorch-lightning/Trainer/) 对象中。

然而，Fast.ai 确实需要在 PyTorch 之上学习另一个库。API 大部分时间并不直接操作纯 PyTorch 代码（也有一些地方会），但它需要像 [DataBunches](https://docs.fast.ai/basic_data.html#DataBunch)、 [DataBlocs](https://docs.fast.ai/data_block.html#The-data-block-API) 等抽象。当“最佳”方法不明显时，这些 API 非常有用。

对于研究人员来说，关键在于不需要再学习另一个库，直接控制研究的关键部分，如数据处理，而不需要其他抽象来操作这些部分。

在这种情况下，fast.ai 库有更高的学习曲线，但如果你不一定知道“最佳”方法是什么，并且只是想把好的方法作为黑箱来使用，这一点是值得的。

### Lightning vs Ignite

![图](img/44b0a765c5c71a65d171a63c0a797a65.png)

更像是共享

从上述内容可以明显看出，将 fast.ai 与这两个框架进行比较是不公平的，因为使用案例和用户群体不同（然而，我仍会在本文末尾的比较表中添加 fast.ai）。

Lightning 和 Ignite 之间的第一个主要区别是它们操作的接口。

在 Lightning 中，有一个标准接口（见 [LightningModule](https://williamfalcon.github.io/pytorch-lightning/LightningModule/RequiredTrainerInterface/)），每个模型都必须遵循这 9 个必需的方法。

这种灵活的格式允许在训练和验证中获得最大的自由。这个接口应该被视为一个 ***系统***，而不是一个模型。系统可能有多个模型（GANs、seq-2-seq 等），或者它可能是一个模型，比如这个简单的 MNIST 示例。

因此，研究人员可以自由尝试各种疯狂的东西，并且只需要担心这 9 个方法。

Ignite 需要非常类似的设置，但没有一个 ***标准*** 接口，每个模型都需要遵循。

请注意，**run** 函数可能会有不同的定义，即：可能有许多不同的事件添加到训练器中，或者它甚至可能被命名为其他名称，如 ***main, train, etc…***

在一个复杂的系统中，比如训练可能以奇怪的方式进行（看着你们，GAN 和 RL），代码的运行情况可能不容易被直观理解。而在 Lightning 中，你可以查看 training_step 来弄清楚发生了什么。

### 可重复性

![图](img/04f9431f398d5f0d0a8b1e65afbd8c1b.png)

当你尝试重现工作时

如我所提到的，Lightning 是在第二个更雄心勃勃的广泛动机下创建的：**可重复性**。

如果你尝试阅读某人的论文实现，往往很难弄清楚发生了什么。我们早已不再只是设计不同的神经网络架构。

现代的 SOTA 模型实际上是***系统***，**这些系统使用了多种模型或训练技术来实现特定的结果**。

正如我之前所说，LightningModule 是一个***系统***，而不是一个模型。因此，如果你想知道所有那些复杂的技巧和超级复杂的训练发生在哪里，你可以查看 training_step 和 validation_step。

如果每个研究项目和论文都使用 LightningModule 模板进行实现，那么**了解发生了什么将变得非常**容易（但可能不容易理解，哈哈）。

在 AI 社区中进行这种标准化将允许一个生态系统蓬勃发展，该生态系统可以使用 LightningModule 接口来执行一些很酷的功能，比如自动化部署、审计系统中的偏差，甚至支持将权重哈希到区块链后端以重建用于关键预测的模型，这些模型可能需要进行审计。

### 开箱即用的功能

Ignite 和 Lightning 之间的另一个主要区别在于 Lightning 开箱即用的功能。开箱即用意味着你**无需**额外编写代码。

举例来说，让我们尝试在同一台机器上的多个 GPU 上训练一个模型。

**Ignite** (**[**demo**](https://github.com/pytorch/ignite/blob/master/examples/mnist/mnist_dist.py)**)**

**Lightning** (**[**demo**](https://github.com/williamFalcon/pytorch-lightning/blob/master/examples/new_project_templates/single_gpu_node_ddp_template.py)**)**

好的，没有哪个是坏的……但如果我们想在多台机器上使用多个 GPU 呢？让我们在 200 个 GPU 上进行训练。

**Ignite**

…

好吧，这里没有内置支持……你需要扩展 [这个示例](https://github.com/pytorch/ignite/blob/master/examples/mnist/mnist_dist.py) 并添加一种提交脚本的简便方法。然后，你还需要处理加载/保存、确保不会覆盖权重/日志与所有进程等……你明白了。

**Lightning**

使用 Lightning，你只需设置节点数量并提交适当的作业。 [这里有一个关于如何正确配置作业的详细教程](https://medium.com/@_willfalcon/trivial-multi-node-training-with-pytorch-lightning-ff75dfb809bd)。

开箱即用的功能是指那些你***无需做任何事即可使用的功能***。这意味着你可能现在不需要它们，但当你需要例如...累积梯度、梯度裁剪或 16 位训练时，你不会花费数天/数周的时间阅读教程来使其工作。

只需设置适当的 Lightning 标志，然后继续进行你的研究。

Lightning 预构建了这些功能，以便用户花更多时间进行研究，而不是进行工程开发。这对于非计算机科学/数据科学研究人员，如物理学家、生物学家等尤其有用，他们可能在编程方面不那么精通。

这些功能使 PyTorch 的特性变得普及，只有高级用户可能会花时间去实现。

这里有表格比较了所有三个框架在不同功能集上的特性。

*如果我遗漏了任何重要内容，请留言，我会更新表格！*

### 高性能计算

### 调试工具

### 可用性

### 结束语

在这里，我们对三个框架进行了多层次的深入比较。每个框架都有其自身的优点。

如果你刚刚学习或没有跟上所有最新的最佳实践，不需要超高级的训练技巧，并且有时间学习新的库，那么选择 fast.ai。

如果你需要最大的灵活性，可以选择 Ignite 或 Lightning。

如果你不需要超高级的功能，并且可以接受添加 Tensorboard 支持、累积梯度、分布式训练等，那么选择 Ignite。

如果你需要更高级的功能、分布式训练、最新的深度学习训练技巧，并且希望看到一个标准化的实现世界，那就使用 Lightning。

**简介： [威廉·法尔肯](https://www.linkedin.com/in/wfalcon/)** 是 AI 研究员、初创公司创始人、CTO、Google Deepmind 学者以及 Facebook AI 的现任博士研究实习生。

[原文](https://towardsdatascience.com/pytorch-lightning-vs-pytorch-ignite-vs-fast-ai-61dc7480ad8a)。经许可转载。

**相关内容：**

+   初学者和 Udacity 深度学习纳米学位的 PyTorch 备忘单

+   使用 PyTorch 框架入门 NLP

+   XLNet 在多个 NLP 任务中超越 BERT

### 更多相关主题

+   [入门 PyTorch Lightning](https://www.kdnuggets.com/2022/12/getting-started-pytorch-lightning.html)

+   [深度学习库介绍：PyTorch 和 Lightning AI](https://www.kdnuggets.com/introduction-to-deep-learning-libraries-pytorch-and-lightning-ai)

+   [免费使用 Lightning AI Studio](https://www.kdnuggets.com/using-lightning-ai-studio-for-free)

+   [通过快速克里金（FKR）加速机器学习](https://www.kdnuggets.com/2022/06/vmc-speed-machine-learning-fast-kriging.html)

+   [BERT 在稀疏性下能有多快？](https://www.kdnuggets.com/2022/04/fast-bert-go-sparsity.html)

+   [机器学习项目的简单快速数据流](https://www.kdnuggets.com/2022/11/simple-fast-data-streaming-machine-learning-projects.html)
