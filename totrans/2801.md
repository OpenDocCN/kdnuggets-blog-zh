# 使用元学习的少样本图像分类

> 原文：[https://www.kdnuggets.com/2020/03/few-shot-image-classification-meta-learning.html](https://www.kdnuggets.com/2020/03/few-shot-image-classification-meta-learning.html)

[评论](#comments)

**作者：[Etienne Bennequin](https://www.linkedin.com/in/etienne-bennequin-55931a101)，Sicara 数据科学家**

<picture>![博物馆观赏](../Images/acc77e3d1333edac9f3905fa77651695.png)</picture> 你并不总是有足够的图像来训练深度神经网络。这里是如何教你的模型从少量例子中快速学习的方法。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT

* * *

### 为什么我们关心少样本学习？

1980年，[福岛邦彦](https://en.wikipedia.org/wiki/Kunihiko_Fukushima) 开发了第一个卷积神经网络。从那时起，随着计算能力的提升和机器学习社区的巨大努力，深度学习算法在计算机视觉相关任务上的表现不断提高。2015年，微软的Kaiming He及其团队报告称，他们的模型[在ImageNet图像分类上表现优于人类](https://arxiv.org/abs/1512.03385)。此时，可以说计算机在利用数十亿张图像解决特定任务方面已经超越了我们。太棒了！

然而，如果你不是Google或Facebook，你通常无法构建一个包含那么多图像的数据集。当你从事计算机视觉工作时，有时需要用每个标签仅有的一到两个例子来分类图像。在这方面，人类依然无可匹敌。给一个婴儿看一张大象的照片，他们以后将永远不会错过识别大象。如果你用你的Resnet50做同样的事情，你可能会对结果感到失望。这种从少量例子中学习的问题被称为*少样本学习*。

近年来，少样本学习问题在研究界引起了广泛关注，许多优雅的解决方案也应运而生。目前最受欢迎的解决方案使用的是**元学习**，或者用三句话总结：学习如何学习。如果你想了解它是什么以及它如何应用于少样本图像分类，请继续阅读。

### 少样本图像分类任务

首先，我们需要定义N-way K-shot图像分类任务。给定：

1.  一个由N个标签组成的支持集，对于每个标签，有K个标记图像；

1.  一个由Q个查询图像组成的查询集；

任务是将查询图像分类到N个类别中的一个，给定N×K张支持集中的图像。当K很小（通常K<10）时，我们称之为“少样本图像分类”（在K=1的情况下称为“一样本分类”）。

![图示](../Images/e314a451ac6016fe2eddc7aa18002186.png)

少样本分类任务示例：给定支持集中每个N=3类别的K=2个实例，我们希望将查询集中的Q=4只狗标记为拉布拉多犬、圣伯纳犬或斗牛犬。即使你从未见过任何斗牛犬、圣伯纳犬或拉布拉多犬，这对你来说也会很简单。但要用AI解决这个问题，我们将需要一些元学习。

### 元学习范式

在1998年，[Thrun & Pratt](https://link.springer.com/chapter/10.1007/978-1-4615-5529-2_1)指出，给定一个任务，算法在任务上表现出“如果其性能随着经验的积累而提高”，则该算法正在“学习”；而对于一组任务，算法在“每个任务的性能随着经验和任务数量的增加而提高”时，则称该算法在“学习学习”。我们将最后一种称为元学习算法。它不是学习如何解决特定的任务，而是不断学习解决许多任务。每当它学习一个新任务时，它在学习新任务的能力上会变得更强：它学会了学习。

从形式上来说，如果我们想要解决一个任务T，元学习算法是在一批训练任务{Tᵢ}上进行训练的。算法从解决这些任务的尝试中获得的训练经验被用来解决最终任务T。

例如，考虑上图所示的任务T。该任务包括使用来自3x2=6张标记图像的信息，将图像标记为拉布拉多犬、圣伯纳犬或斗牛犬。一个训练任务Tᵢ可能是将图像标记为拳师犬、拉布拉多贵宾犬或罗威纳犬，使用来自3x2=6张标记图像的信息。元训练过程是一系列这些任务Tᵢ，每次使用不同的犬种。我们期望元学习模型“随着经验和任务数量的增加”变得更好。最后，我们在任务T上评估模型。

![图示](../Images/83c6596cb41c28dade3e99b3bcb60384.png)

我们在拉布拉多犬、圣伯纳犬和斗牛犬上评估元学习模型，但我们只在其他犬种上进行训练。

那么我们怎么做呢？假设你想解决任务T（涉及拉布拉多犬、圣伯纳犬和斗牛犬）。那么你将需要一个包含大量不同品种犬的元训练数据集。例如，你可以使用[斯坦福犬数据集](http://vision.stanford.edu/aditya86/ImageNetDogs/)，该数据集包含从ImageNet中提取的超过2万只犬。我们将这个数据集称为D。请注意，D不需要包含任何拉布拉多犬、圣伯纳犬或斗牛犬，过程仍然能够进行。

从D中我们抽取批次的集（见下文）。每一集对应一个N-way K-shot分类任务Tᵢ，这类似于T（通常我们使用相同的N和K）。在模型解决批次中的每一集后（即标记了每个查询集的图像），它的参数会被更新。这通常通过反向传播在查询集上的分类不准确性得到的损失来完成。

这样，模型可以在任务之间学习，以准确解决新的、未见过的少样本分类任务。标准的学习分类算法将学习一个映射`image→label`，而元学习算法通常学习一个映射`support-set→c(.)`，其中`c`是一个映射`query→label`。

### 元学习算法

现在我们知道算法进行元训练是什么意思，仍然有一个谜团：元学习模型如何解决少样本分类任务？当然，解决方案不止一个。在这里，作为真正的酷孩子，我们将专注于最流行的解决方案。

### 度量学习

度量学习的基本思想是学习数据点（如图像）之间的距离函数。它在解决少样本分类任务中已被证明非常有用：度量学习算法通过将查询图像与标记图像进行比较来分类查询图像，而不是在支持集（少量标记图像）上进行微调。

![图](../Images/8eaa575eb2f3c3562b3f7f40d954037a.png)

查询（右侧）与每个支持集图像进行比较。其标签取决于哪些图像最接近。

当然，你不能逐像素比较图像，所以你要做的是在相关的特征空间中比较图像。为了明确，让我们详细说明度量学习算法如何解决少样本分类任务（定义为标记示例的支持集和我们想要分类的图像的查询集）：

1.  我们从所有支持集和查询集图像中提取嵌入（通常使用卷积神经网络）。现在，我们在少样本分类任务中需要考虑的每个图像都由一个1维向量表示。

1.  每个查询的分类取决于它与支持集图像的距离。距离函数和分类策略都有许多可能的设计选择。一个例子是欧几里得距离和k-最近邻。

1.  在元训练期间，在每一集的末尾，CNN的参数通过反向传播从查询集（通常是交叉熵损失）分类错误中得到的损失进行更新。

每年发布几个度量学习算法来解决少样本图像分类的两个原因是：

1.  它们在经验上效果很好；

1.  唯一的限制是你的想象力。有许多方法可以提取特征，还有更多的方法可以比较这些特征。我们现在将介绍一些现有的解决方案。

![图](../Images/40bcca540659f5fb86a12bf2cba8aa63.png)

匹配网络算法。特征提取器对于支持集图像（左）和查询图像（底部）是不同的。查询的嵌入与支持集中的每个图像进行余弦相似度比较。然后通过 softmax 进行分类。图来自 [Vinyals et al.](http://papers.nips.cc/paper/6385-matching-networks-for-one-shot-learning)

[匹配网络](http://papers.nips.cc/paper/6385-matching-networks-for-one-shot-learning)（见上文）是首个使用元学习的度量学习算法。在这种方法中，我们不会以相同的方式提取支持图像和查询图像的特征。Oriol Vinyals及其来自 [Google DeepMind](https://deepmind.com/) 的团队想到使用 [LSTM 网络](https://www.mitpressjournals.org/doi/abs/10.1162/neco.1997.9.8.1735) 使所有图像在特征提取过程中相互作用。他们称之为 全上下文嵌入，因为你允许网络在知道不仅是要嵌入的图像，还包括支持集中的所有其他图像的情况下找到最合适的嵌入。这使得他们的模型比所有图像通过简单的 CNN 时表现更好，但也需要更多时间和更大的 GPU。

在更近期的工作中，我们不再将查询图像与支持集中的每个图像进行比较。来自多伦多大学的研究人员提出了 [原型网络](http://papers.nips.cc/paper/6996-prototypical-networks-for-few-shot-learning)。在他们的度量学习算法中，在从图像中提取特征后，我们为每个类别计算一个 prototype。为此，他们使用了该类别中每个图像的嵌入的均值。（但你可以想象有成千上万种方法来计算这些嵌入。这个函数只需要是可微的，以便进行反向传播。）一旦计算出原型，就使用欧几里得距离对查询进行分类（见下文）。

![图](../Images/d0e2d3af6391cac5e2626e2b3e8169a9.png)

在原型网络中，我们将查询 X 标记为最接近的 prototype 的标签。

尽管其简单性，原型网络仍然取得了最先进的成果。后来开发了更复杂的度量学习架构，如 [一种表示距离函数的神经网络](http://openaccess.thecvf.com/content_cvpr_2018/html/Sung_Learning_to_Compare_CVPR_2018_paper.html) （而不是欧几里得距离）。这稍微提高了准确性，但我认为直到今天，原型思想仍然是少样本图像分类领域度量学习算法中最有价值的思想（如果你不同意，请留下愤怒的评论）。

### 模型无关元学习

我们将以 [模型无关元学习](http://proceedings.mlr.press/v70/finn17a/finn17a.pdf) （MAML）结束这次回顾，目前这是最优雅和最有前途的元学习算法之一。它基本上是最纯粹形式的元学习，具有 两个层次的神经网络反向传播。

该算法的核心思想是训练一个神经网络，使其能够快速适应新的分类任务，只需少量样本。我在下面提供了MAML如何在一次元训练（即在从D中采样的少量样本分类任务Tᵢ上）中工作的可视化。假设你有一个用????参数化的神经网络M：

![图](../Images/9f41f2c9465714d621d57e98b0dae9ac.png)

MAML模型的元训练步骤，用????参数化。

1.  创建M的副本（这里称为f）并用????初始化它（图中，????₀=????）。

1.  快速在支持集上微调f（仅需少量梯度下降）。

1.  在查询集上应用微调后的f。

1.  将分类错误所产生的损失反向传播整个过程，并更新????。

然后，在下一个阶段，我们创建更新后的模型M的副本，运行新的少样本分类任务，依此类推。

在元训练过程中，MAML学习初始化参数，使模型能够快速高效地适应新少样本任务及新的未见过的类别。

公平地说，MAML目前在流行的少样本图像分类基准测试中表现不如度量学习算法。由于存在两个训练层次，因此训练起来非常困难，超参数搜索更加复杂。此外，元反向传播需要计算梯度的梯度，因此你必须使用近似方法才能在标准GPU上训练。由于这些原因，你可能会更愿意在家庭或工作项目中使用度量学习算法。

但Model Agnostic Meta-Learning之所以如此令人兴奋，是因为它是**模型无关**的。这意味着它可以虚拟地应用于任何神经网络，适用于任何任务。掌握MAML意味着能够训练任何神经网络，使其能够快速适应新任务，并且只需少量样本。MAML的作者Chelsea Finn和Sergey Levine将其应用于监督式少样本分类、监督式回归和强化学习。但凭借想象力和努力工作，你可以用它将任何神经网络转变为少样本高效神经网络！

这就是对元学习激动人心世界的简要介绍。最近，少样本学习在计算机视觉研究中引起了广泛关注，因此该领域发展非常迅速（如果你在2020年阅读这篇文章，我建议你寻找更近期的信息来源）。谁知道在接下来的几年里，神经网络在从一次瞥见中学习视觉概念方面会有多强大？

感谢Antoine Toubhans、Emna Kamoun、Raphaël Meudec、Hugo Lime、Bastien Ponchon、Nicolas Jean和Laurent Montier。

**个人简介：[Etienne Bennequin](https://www.linkedin.com/in/etienne-bennequin-55931a101)**（[@bennequin](https://twitter.com/bennequin)）是Sicara的数据科学家。

[原文](https://www.sicara.ai/blog/2019-07-30-image-classification-few-shot-meta-learning)。经许可转载。

**相关：**

+   [TensorFlow 2.0教程：优化训练时间性能](/2020/03/tensorflow-optimizing-training-time-performance.html)

+   [Keras Tuner的超参数调优实践](/2020/02/hyperparameter-tuning-keras-tuner.html)

+   [元学习的少样本图像分类](/2020/02/amazon-uses-self-learning-teach-alexa-correct-mistakes.html)

### 相关主题

+   [使用卷积神经网络（CNNs）进行图像分类](https://www.kdnuggets.com/2022/05/image-classification-convolutional-neural-networks-cnns.html)

+   [使用Tensorflow训练图像分类模型指南](https://www.kdnuggets.com/2022/12/guide-train-image-classification-model-tensorflow.html)

+   [更多分类问题的性能评估指标](https://www.kdnuggets.com/2020/04/performance-evaluation-metrics-classification.html)

+   [PyCaret二分类介绍](https://www.kdnuggets.com/2021/12/introduction-binary-classification-pycaret.html)

+   [使用HuggingFace对BERT进行推特分类微调](https://www.kdnuggets.com/2022/01/finetuning-bert-tweets-classification-ft-hugging-face.html)

+   [分类的机器学习算法](https://www.kdnuggets.com/2022/03/machine-learning-algorithms-classification.html)
