# 从零开始构建图像搜索服务

> 原文：[https://www.kdnuggets.com/2019/01/building-image-search-service-from-scratch.html/2](https://www.kdnuggets.com/2019/01/building-image-search-service-from-scratch.html/2)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](/2019/01/building-image-search-service-from-scratch.html?page=2#comments)

### 文本 -> 文本

*毕竟没那么不同*

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

**文本的嵌入**

转到自然语言处理（NLP）的领域，我们可以使用类似的方法**索引和搜索词汇**。

我们加载了一组来自[GloVe](https://nlp.stanford.edu/projects/glove/)的预训练向量，这些向量是通过爬取整个维基百科并学习数据集中词汇之间的语义关系获得的。

就像之前一样，我们将创建一个索引，这次包含所有的 GloVe 向量。然后，我们可以**搜索我们的嵌入**以查找相似的词汇。

例如，搜索`said`返回这个[`word`, `distance`]的列表：

+   `['said', 0.0]`

+   `['told', 0.688713550567627]`

+   `['spokesman', 0.7859575152397156]`

+   `['asked', 0.872875452041626]`

+   `['noting', 0.9151610732078552]`

+   `['warned', 0.915908694267273]`

+   `['referring', 0.9276227951049805]`

+   `['reporters', 0.9325974583625793]`

+   `['stressed', 0.9445104002952576]`

+   `['tuesday', 0.9446316957473755]`

这似乎很合理，大多数词汇与我们原始词汇的含义相似，或者代表了一个合适的概念。最后的结果（`tuesday`）也显示这个模型远非完美，但它能让我们开始。现在，让我们尝试将词汇和图像都融入我们的模型中。

**一个相当大的问题**

使用嵌入之间的距离作为搜索的方法是相当通用的，但我们对词汇和图像的表示似乎**不兼容**。图像的嵌入大小为 4096，而词汇的嵌入大小为 300——我们如何用一种来搜索另一种呢？此外，即使两个嵌入的大小相同，它们的训练方式也完全不同，因此图像和相关词汇随机具有相同的嵌入的可能性极低。我们需要训练一个**联合模型**。

### 图像 <-> 文本

*世界碰撞*

现在让我们创建一个**混合**模型，可以在词汇和图像之间相互转换。

在本教程中，我们将首次实际训练自己的模型，灵感来自一篇名为[DeViSE](https://papers.nips.cc/paper/5204-devise-a-deep-visual-semantic-embedding-model)的优秀论文。我们不会完全重新实现它，但会在其主要思想上大量借鉴。（有关该论文的另一种略有不同的看法，请查看fast.ai在其[lesson 11](http://course.fast.ai/lessons/lesson11.html)中的实现。）

这个想法是通过重新训练我们的图像模型并**改变标签类型**来结合这两种表示。

通常，图像分类器被训练从许多类别中选择一个（Imagenet有1000个）。这意味着——以Imagenet为例——最后一层是一个大小为1000的向量，表示**每个类别的概率**。这意味着我们的模型对哪些类别彼此相似没有语义理解：将`cat`的图像分类为`dog`和将其分类为`airplane`的错误程度相同。

对于我们的混合模型，我们用**类别的词向量**替换模型的最后一层。这使得我们的模型可以学习将图像的语义映射到词语的语义，并且意味着相似的类别会彼此更接近（因为`cat`的词向量比`airplane`更接近`dog`）。我们将预测一个**语义丰富的词向量**，其大小为300，而不是一个大小为1000的全零目标。

我们通过添加两个密集层来实现这一点：

+   一个大小为2000的中间层

+   一个大小为300的输出层（GloVe词向量的大小）。

这是模型在Imagenet上训练时的样子：

![](../Images/cf1d7ca19487c4ecfd529a923d805dca.png)

现在它的样子是这样的：

![](../Images/550d43127fbaec99e1423125966513ea.png)

**训练模型**

然后，我们在数据集的训练分割上重新训练模型，以学习**预测与图像标签相关的词向量**。例如，对于类别为猫的图像，我们试图预测与猫相关的300长度的向量。

这个训练需要一些时间，但仍然比在Imagenet上的速度快很多。作为参考，在我没有**GPU**的笔记本上大约花了6-7小时。

需要注意的是，这种方法的雄心壮志。我们这里使用的训练数据（数据集的80%，即800张图像）相对于通常的数据集来说微不足道（Imagenet有**一百万**张图像，**多三个数量级**）。如果我们使用传统的类别训练方法，我们不期望模型在测试集上表现很好，更不用说在完全新的示例上表现了。

一旦我们的模型训练完成，我们将获得上述的GloVe词索引，并通过将数据集中所有图像通过它来构建一个新的快速图像特征索引，保存到磁盘。

**标记**

我们现在可以通过将图像输入到我们训练的网络中，轻松提取任何图像的标签，保存出来的大小为300的向量，并在GloVe的英文单词索引中找到最接近的词。让我们试试这张图像——它属于`bottle`类别，但包含各种物品。

![](../Images/f6f739f1fad0973eb883c4aea63e303c.png)

这是生成的标签：

+   `[6676, 'bottle', 0.3879561722278595]`

+   `[7494, 'bottles', 0.7513495683670044]`

+   `[12780, 'cans', 0.9817070364952087]`

+   `[16883, 'vodka', 0.9828150272369385]`

+   `[16720, 'jar', 1.0084964036941528]`

+   `[12714, 'soda', 1.0182772874832153]`

+   `[23279, 'jars', 1.0454961061477661]`

+   `[3754, 'plastic', 1.0530102252960205]`

+   `[19045, 'whiskey', 1.061428427696228]`

+   `[4769, 'bag', 1.0815287828445435]`

这是一个相当惊人的结果，因为大多数标签**非常相关**。这种方法仍有成长空间，但它相当好地识别了图像中的大部分项目。模型学会了提取**许多相关标签**，即使是从它未经过训练的类别中！

**使用文本搜索图像**

最重要的是，我们可以使用我们的联合嵌入通过**任何词**在我们的图像数据库中进行搜索。我们只需从GloVe获取我们的预训练词嵌入，然后找到与之最相似的图像（通过我们的模型运行它们来获得）。

**用最少的数据进行图像搜索的泛化**。

首先从一个实际存在于我们训练集中的词开始，搜索`dog:`。

![](../Images/ae17e0fcb1686cfae30e42b933d5d579.png)

搜索词“`dog`”的结果

好的，结果相当不错——但这可以从任何仅仅在标签上训练的分类器中得到！让我们尝试更困难的任务，通过搜索关键字`ocean`，这是我们数据集中没有的。

![](../Images/40608dc265c7e9ee4ed46bd9450c703f.png)

搜索词“`ocean`”的结果

这太棒了——我们的模型理解`ocean`类似于`water`，并返回了许多来自`boat`类别的项。

那么搜索`street`怎么样？

![](../Images/48f0297f5b9471a38fc2ed2726c267df.png)

搜索词“`street`”的结果

在这里，我们返回的图像来自许多类别（`car`、`dog`、`bicycles`、`bus`、`person`），尽管我们从未在训练模型时使用过这个概念，但大多数图像包含或接近街道。因为我们通过预训练的词向量**利用外部知识**来学习从图像到向量的映射，这种映射比简单的类别更具**语义丰富性**，所以我们的模型能够很好地泛化到外部概念。

**超越词汇**

-   英语虽然进步很大，但还不足以为每个事物发明一个词。例如，在发布这篇文章时，英语中没有“猫躺在沙发上”的词汇，而这是一个完全有效的搜索引擎查询。如果我们想同时搜索多个单词，我们可以使用一种非常简单的方法，利用词向量的算术性质。结果表明，两个词向量的相加通常效果很好。因此，如果我们只是使用`cat`和`sofa`的平均词向量来搜索图像，我们可以期望得到非常像猫的、非常像沙发的图像，或者是猫躺在沙发上的图像。

![](../Images/ddebc9fcad6f827cc7f9ff0e25a62c6d.png)

-   获取多个单词的联合嵌入

-   让我们试试使用这种混合嵌入进行搜索吧！

![](../Images/f35d3fc3b4a2a49984204cdbc0ac0e38.png)

-   搜索词“`cat"+"sofa"`的结果

-   这是一个令人惊叹的结果，因为大多数这些图像包含了某种版本的毛茸茸的动物和沙发（我特别喜欢第二行最左侧的图像，看起来像是一堆毛茸茸的东西靠在沙发旁边）！我们的模型虽然只在单词上进行过训练，但可以处理两个单词的组合。我们还没有建立 Google 图像搜索，但对于相对简单的架构来说，这确实令人印象深刻。

-   这种方法实际上可以自然地扩展到各种领域（参见这个 [示例](https://towardsdatascience.com/semantic-code-search-3cd6d244a39c) 以了解联合代码-英语嵌入），所以我们非常想知道你将其应用于什么。

### 结论

-   我希望你觉得这篇文章有用，并且对基于内容的推荐系统和语义搜索有了更多了解。如果你有任何问题或评论，或者想分享你使用这个教程构建的东西，请通过 [Twitter](https://twitter.com/EmmanuelAmeisen)联系我！

***想从硅谷或纽约的顶级专业人士那里学习应用人工智能吗？*** 了解更多关于 [人工智能](http://insightdata.ai/?utm_source=representations&utm_medium=blog&utm_content=top) 计划的信息。

***你是一家从事人工智能的公司，想参与 Insight AI Fellows 计划吗？*** 随时 *[*联系我们*](http://insightdatascience.com/partnerships?utm_source=representations&utm_medium=blog&utm_content=top)*。 

-   感谢 [Stephanie Mari](https://medium.com/@stephaniemari?source=post_page)， [Bastian Haase](https://medium.com/@bastianhaase?source=post_page)， [Adrien Treuille](https://medium.com/@adrien_37234?source=post_page) 和 [Matthew Rubashkin](https://medium.com/@mrubash1?source=post_page)。

-   **简介：[Emmanuel Ameisen](https://www.linkedin.com/in/ameisen/) ([@EmmanuelAmeisen](https://twitter.com/EmmanuelAmeisen))** 是 Insight Data Science 的 AI 负责人。

-   [原文](https://blog.insightdatascience.com/the-unreasonable-effectiveness-of-deep-learning-representations-4ce83fc663cf)。经许可转载。

-   **相关：**

+   [解决 90% 自然语言处理问题的步骤指南](/2019/01/solve-90-nlp-problems-step-by-step-guide.html)

+   [使用 MXNET Gluon 和 Comet.ml 实现 ResNet 进行图像分类](/2018/12/implementing-resnet-mxnet-gluon-comet-ml-image-classification.html)

+   [快速轻松解决任何图像分类问题](/2018/12/solve-image-classification-problem-quickly-easily.html)

### 相关阅读

+   [构建视觉搜索引擎 - 第 2 部分：搜索引擎](https://www.kdnuggets.com/2022/02/building-visual-search-engine-part-2.html)

+   [在 Python 中使用网格搜索和随机搜索进行超参数调优](https://www.kdnuggets.com/2022/10/hyperparameter-tuning-grid-search-random-search-python.html)

+   [通过 Uplimit 的机器学习搜索课程提升您的搜索引擎技能！](https://www.kdnuggets.com/2023/10/uplimit-elevate-your-search-engine-skills-search-with-ml-course)

+   [构建视觉搜索引擎 - 第 1 部分：数据探索](https://www.kdnuggets.com/2022/02/building-visual-search-engine-part-1.html)

+   [311 呼叫中心性能：服务水平评级](https://www.kdnuggets.com/2023/03/boxplot-outlier-311-call-center-performance.html)

+   [从零开始的机器学习：决策树](https://www.kdnuggets.com/2022/11/machine-learning-scratch-decision-trees.html)
