# 图形机器学习能否识别在线社交网络中的仇恨言论？

> 原文：[https://www.kdnuggets.com/2019/09/graph-machine-learning-hate-speech-social-networks.html](https://www.kdnuggets.com/2019/09/graph-machine-learning-hate-speech-social-networks.html)

[评论](#comments)

**由 Pantelis Elinas、Anna Leontjeva 和 Yuriy Tyshetskiy 撰写**。

![](../Images/ddd24baf552f68f88b2b391bd337ce2b.png)

三十多年来，互联网从一个用于研究科学家交流和交换数据的小型计算机网络，发展成了一种渗透到我们日常生活几乎每个方面的技术。今天，很难想象没有在线访问的生活，无论是为了商业、购物还是社交。

一种以从未有过的规模连接人类的技术，同时也放大了我们一些最糟糕的特质。在线仇恨言论以病毒式的方式在全球传播，对个人和社会产生了短期和长期的影响。这些后果往往难以衡量和预测。在线社交媒体网站和移动应用程序无意中成为了仇恨言论传播和扩散的平台。

### 什么是在线仇恨言论？

> “仇恨言论是一种在线（例如，互联网、在线社交媒体平台）发生的言论，其目的是基于种族、宗教、民族背景、性取向、残疾或性别等特征攻击某个人或群体。” [[来源](https://en.wikipedia.org/wiki/Online_hate_speech)]

许多国际机构，包括 [联合国人权理事会](https://www.ohchr.org/en/hrbodies/hrc/pages/home.aspx) 和 [在线仇恨预防机构](https://ohpi.org.au/)，致力于理解在线仇恨言论的性质、传播和预防。近期机器学习的进展为这些努力提供了有希望的结果，特别是作为一种可扩展的自动化系统，用于早期检测和预防。学术研究人员不断改进用于仇恨言论分类的机器学习系统。同时，所有主要的社交媒体网络都在部署并不断优化类似的工具和系统。

在线仇恨言论是一个复杂的主题。在这篇文章中，我们考虑使用机器学习来检测基于其在 Twitter 社交网络上的活动的仇恨用户。这个问题和数据集首次发布在 [1]。数据可以从 Kaggle 上 [这里](https://www.kaggle.com/manoelribeiro/hateful-users-on-twitter) 自由下载。

在接下来的内容中，我们开发并比较了两种机器学习方法，以将 Twitter 用户的小子集分类为仇恨用户或正常用户（非仇恨）。首先，我们采用传统的机器学习方法，根据用户的词汇和社交资料训练分类器。接下来，我们应用最先进的图神经网络（GNN）机器学习算法解决同样的问题，但现在还考虑用户之间的关系。

> *如果你希望跟随，Jupyter Notebook中的Python代码可以在* [*这里*](https://github.com/stellargraph/stellargraph/tree/develop/demos/use-cases)*找到。*

**数据集**

我们展示了如何使用机器学习进行在线仇恨言论检测，使用的数据集包括Twitter用户及其在社交媒体网络上的活动。该数据集最初由巴西米纳斯吉拉斯联邦大学的研究人员发布[1]，我们使用时未做修改。

数据涵盖了100,368个Twitter用户。每个用户都有多个与活动相关的特征，如发推频率、粉丝数量、收藏数量和标签数量。此外，通过分析每个用户最近200条推文中的词汇，我们得到了大量关于语言内容的特征。斯坦福大学的[Empath](http://empath.stanford.edu/)工具[2]被用来分析每个用户在爱、暴力、社区、温暖、嘲笑、独立、嫉妒和政治等类别中的词汇，并分配数值以表示用户与每个类别的关系。

总的来说，我们使用204个特征来描述数据集中的每个用户。对于每个用户，我们收集这些特征形成一个204维的*特征向量*，用于作为机器学习模型的输入，以分类用户为仇恨或正常。

数据集还包括用户之间的关系。如果一个用户转发了另一个用户的推文，那么这两个用户被认为是相连的。这种关系结构形成了一个网络，与Twitter的关注者和被关注者网络不同。关注者对我们是隐蔽的，因为用户可以选择将他们的网络设置为私密，而转发网络保持公开，只要原始推文是公开的。

![](../Images/9229077a5a84c30f9595fa27eae80ae3.png)

*数据集中标注用户的相对比例，其中红色表示**仇恨**，绿色表示**正常**，蓝色表示**其他**。少数用户被知晓是仇恨的或非仇恨的。*

最后，用户被标记为三种类别之一：**仇恨**、**正常**或**其他**。在数据集中的约10万用户中，仅约5千个用户被手动标注为**仇恨**或**正常**；其余约9.5万用户属于**其他**类别，即尚未被标注。数据集中标注用户的相对比例如左侧所示，其中红色是**仇恨**，绿色是**正常**，蓝色是**其他**。少数用户被知晓为仇恨的或非仇恨的。文献[1]中详细描述了数据标注过程的协议。

图1显示了数据集的图形表示。我们用红色圆圈表示标注为**仇恨**的用户，用绿色圆圈表示标注为**正常**的用户。标注为**其他**（未标注）的用户则留空。

![](../Images/734b5a2c75c48b6c5d43eb5ce33b4a45.png)

*图1：仇恨Twitter数据集结构和基本统计数据。*

### 使用机器学习进行仇恨用户分类

我们的目标是训练一个二分类模型，用于将用户分类为仇恨或正常。然而，用于训练模型的数据集面临两个挑战。

首先，只有一小部分用户被标注为**仇恨**或**正常**，大多数用户的标签未知（**其他**类别）。其次，标注数据在标签分布上高度不平衡：在约5000个标注用户中，仅约500个（约10%）被标注为**仇恨**，其余的标注为**正常**。

[半监督机器学习](https://en.wikipedia.org/wiki/Semi-supervised_learning)方法可以帮助我们通过利用标注数据和未标注数据来缓解标注样本不足的问题。我们将在本文后续部分讨论这些方法，尤其是在GNNs的背景下。

为了处理标注训练集中的类别不平衡，我们计算并使用类别权重；这些权重用于模型的损失函数（在模型训练过程中优化），以使模型在将用户分类为“少数”类别（即示例较少的类别，或在本例中为**仇恨**用户类别）的错误受到比分类“多数”类别（即**正常**用户类别）的错误更多的惩罚。

**将数据划分为训练集和测试集**

我们将数据划分为训练集和测试集，使用分层抽样方法，使15%的标注用户数据用于训练，其余85%用于测试训练后的分类模型。

我们的训练集和测试集的统计数据如下：

**训练正常: 664，仇恨: 81**

**测试正常: 3,763，仇恨: 463**

训练数据集表现出高度的类别不平衡。我们可以通过使用类别权重来补偿这种不平衡，从而在模型训练过程中评估损失函数时，给予代表性较少的类别更多的权重。我们计算了类别权重**正常: 0.56 和仇恨: 4.60**；也就是说，正类在计算损失函数时将获得**约8倍**的权重。

**评估指标**

为了评估性能并比较训练后的分类模型，我们将考虑以下三项指标（有关二分类器评估指标的描述请见[这里](https://en.wikipedia.org/wiki/Evaluation_of_binary_classifiers)），这些指标是在持出测试集的标注用户上进行评估的：

1.  准确率

1.  接收器操作特征（ROC）曲线

1.  ROC曲线下面积（AU-ROC）

**逻辑回归模型**

我们首先训练一个[逻辑回归](https://en.wikipedia.org/wiki/Logistic_regression)（LR）模型，以预测用户是**正常**还是**仇恨**。

在训练和评估这个模型时，我们将忽略那些未被标注为**正常**或**恶意**的用户以及用户之间的关系，因为LR模型不直接支持这些信息。

训练和测试数据以表格格式呈现，如图2所示。训练集中标注的用户用红色和绿色圆圈表示。每个用户的特征向量垂直堆叠，形成输入LR模型的设计矩阵。在训练LR模型后，我们可以对保留的测试集中的用户进行预测，以测量训练模型的泛化性能。

![](../Images/378420260464df07c36d2e90b039f399.png)

*图2：用于训练和评估逻辑回归模型的设置，用于在线仇恨言论分类。*

训练模型后，我们可以用它对测试集中的每个用户进行预测，并计算准确率和AU-ROC指标，同时绘制ROC曲线。测试集上的准确率为**85.9%**，AU-ROC为**0.81**。下面的图中可以看到ROC曲线的绘制。

![](../Images/bb2bd9a950bec8f272459c2e60af965b.png)

*图3：使用测试数据计算的逻辑回归分类器的ROC曲线。ROC曲线下面积为0.81。*

**图神经网络**

在LR模型的规范和训练中，我们忽略了约95k个未被标注为**恶意**或**正常**的用户。此外，我们忽略了用户之间的关系。

可以想象，一个恶意用户可能会采取措施避免被轻易识别，例如小心不使用明显的恶意词汇。然而，同一个用户可能会乐于转发其他用户的恶意推文。这些信息隐藏在用户之间的关系中。我们上述使用的LR模型没有利用这些关系。

> *这使我们提出了一个问题：我们能否利用用户之间的关系以及关于未标注用户的数据来提高机器学习模型的预测性能？如果可以，我们可以使用什么样的机器学习模型，以及如何使用？*

使用关系数据的一种方法是进行手动[特征工程](https://en.wikipedia.org/wiki/Feature_engineering)，将与网络相关的特征引入LR模型。此类特征的例子包括各种[中心性度量](https://en.wikipedia.org/wiki/Centrality)，这些度量量化了图中节点的位置重要性。事实上，[1]中发布的数据集包括了这些工程化的网络相关特征，但我们故意从数据中删除了这些特征，以展示现代机器学习的核心思想之一。这一思想由深度学习方法普及，即可以让机器学习算法*自动*学习适合的特征以最大化模型性能，从而避免了繁琐的手动特征工程过程。（然而，这种特征工程的自动化带来了模型解释性的代价——这是另一个讨论的话题。）

在上述思想的指导下，我们放弃了特征工程，使用最先进的GNN算法来处理在线仇恨言论分类。GNN模型共同利用用户特征和数据集中所有用户之间的关系，包括那些没有标注的用户。我们期望使用这些额外信息的GNN模型会优于基线LR模型。

> *这篇文章* [*了解你的邻居：图上的机器学习*](https://medium.com/stellargraph/knowing-your-neighbours-machine-learning-on-graphs-9b7c3d0d5896) *提供了图机器学习的入门性但全面的概述。*

我们在这里使用的特定GNN算法已在[3]中发布。它被称为图采样与聚合（GraphSAGE），并基于这样的洞察：对一个节点的预测应该基于该节点的特征向量，但也应考虑其邻居的特征向量，甚至其邻居的邻居，等等。以分类仇恨用户为例，我们的工作假设是仇恨用户可能与其他仇恨用户相连。这种连接的强度将取决于两个用户之间的图距离（两个用户节点之间的[图距离](https://en.wikipedia.org/wiki/Distance_%28graph_theory%29)）以及它们的特征向量。

GraphSAGE 引入了一种新的图卷积神经网络层，该层在训练分类器时传播节点邻域的信息。这种新层在图4中进行了总结。正如[3]中所描述的，这种层“通过从节点的局部邻域中采样和聚合特征来生成嵌入。”嵌入是节点的潜在表示，可用作分类模型的输入，通常是一个全连接神经网络，从而可以以端到端的方式训练所有模型参数。我们可以将多个这样的层按顺序堆叠起来，以构建一个更深的网络，该网络融合了来自更大网络邻域的信息。使用多少 GraphSAGE 层是特定于问题的，应作为模型超参数进行适当调整。

![](../Images/0df8c1b7670b5ef2edebf19dd1a79110.png)

*图4：GraphSAGE 神经网络层的描述。它使用聚合信息来形成节点的邻域，以学习如何做出更好的预测。蓝色箭头表示在聚合步骤中考虑的节点邻居。*

一般而言，对于具有高节点度的大图，图神经网络模型可能变得计算上难以处理。为避免这种情况，GraphSAGE 使用一种采样方案来限制将特征信息传递给中心节点的邻居数量，如图4中的“AGGREGATE”步骤所示。此外，GraphSAGE 模型学习的函数可以用于生成在训练期间网络中不存在的节点的潜在表示。因此，GraphSAGE 可以在仅有部分图在训练时可用的归纳设置中进行预测（尽管这不是我们工作的示例中情况，你可以在 Jupyter Notebook [这里](https://github.com/stellargraph/stellargraph/blob/develop/demos/node-classification/graphsage/graphsage-pubmed-inductive-node-classification-example.ipynb) 找到这样的演示）。

开源的 [StellarGraph Python 库](https://github.com/stellargraph/stellargraph) 提供了一个易于使用的 GraphSAGE 算法实现。在本文中，我们将使用 StellarGraph 来构建和训练一个 GraphSAGE 模型，以预测仇恨的 Twitter 用户。请参见 Jupyter notebook [这里](https://github.com/stellargraph/stellargraph/tree/develop/demos/use-cases) 了解如何操作。

我们可以可视化注释用户的节点潜在表示。我们将第一个 GraphSAGE 层的输出激活作为节点表示。这些在图5中以二维形式显示，其中 **仇恨** 用户用红色显示，**普通** 用户用蓝色显示。

![](../Images/f6b789d1de743112302a276b377d0c27.png)

*图5：注释用户节点嵌入的可视化。仇恨用户用红色显示，普通用户用蓝色显示。*

图6中显示的节点潜在表示表明，大多数仇恨用户倾向于聚集在一起。然而，一些正常用户也在同一区域，这些用户将难以与仇恨用户区分开来。同样，有少量仇恨用户分散在正常用户中，这些也将很难正确分类。

GraphSAGE用户分类模型在测试数据上的准确率为**88.9%**，AU-ROC得分为**0.88**。

**模型之间的比较**

现在让我们比较一下GraphSAGE和逻辑回归模型，看看使用有关未标记用户和用户之间关系的额外信息是否真的有助于提高用户分类器的性能。

图6中同时绘制了两个模型的ROC曲线。LR和GraphSAGE模型的AU-ROC分别为**0.81**和**0.88**（较大的数字表示更好的性能）。通过这一测量，我们可以看到，利用关系信息的机器学习模型提高了整体预测性能。

![](../Images/cf98f511b9f19c8e6571fcba01a32580.png)

*图6：逻辑回归（橙色）和GraphSAGE（蓝色）模型的ROC曲线图。还显示了曲线下的面积。曲线是使用测试集的数据绘制的。*

在将用户分类为**仇恨**（正类）或**正常**（负类）时，重要的是要最小化假阳性的数量，即被错误分类为仇恨的正常用户数量。同时，我们也希望能正确分类尽可能多的仇恨用户。我们可以通过设置由ROC曲线指导的决策阈值来实现这两个目标。

假设我们愿意容忍约2%的假阳性率，则GraphSAGE和LR模型分别达到了**0.378**和**0.253**的真实正例率。因此，我们可以看到，对于固定的2%假阳性率，GraphSAGE模型的真实正例率比LR模型高出**12%**。也就是说，我们可以在相同的低误分类正常用户的情况下，正确识别更多的仇恨用户。我们可以得出结论，通过利用数据中可用的关系信息以及未标记用户信息，机器学习模型在具有潜在网络结构的稀疏标记数据集上的性能得到了极大提升。

### 结论

在本文中，我们考虑了互联网增长推动的在线仇恨言论的兴起，并提出了一个问题；*“图机器学习能否识别在线社交网络中的仇恨言论？”*

我们的技术分析明确回答了这个问题，*“是的，但仍有很大的改进空间。”* 我们展示了现代GNNs能够以比传统机器学习方法更高的准确度识别在线仇恨言论。

[对外关系委员会](https://www.cfr.org/) 最近发表了 [这篇文章](https://www.cfr.org/backgrounder/hate-speech-social-media-global-comparisons)，指出：“归因于在线仇恨言论的暴力在全球范围内增加。”我们已经表明，图形机器学习是对抗在线仇恨言论的强有力武器。我们的结果为使用更大网络数据集和更复杂的 GNN 方法进行在线仇恨言论分类的额外研究提供了鼓励。

如果你想了解更多关于如何使用最先进的 GNN 模型进行预测建模的信息，可以查看 [StellarGraph 图形机器学习库](https://github.com/stellargraph/stellargraph) 和众多配套的 [演示](https://github.com/stellargraph/stellargraph/tree/develop/demos)。

本工作得到 [CSIRO Data61](https://www.data61.csiro.au/) 的支持，这是澳大利亚领先的数字研究网络。

**参考文献**

1.  “[像狼群中的绵羊](https://arxiv.org/abs/1801.00317)”：对 Twitter 上仇恨用户的特征描述。M. H. Ribeiro, P. H. Calais, Y. A. Santos, V. A. F. Almeida 和 W. Meira Jr. 2018年。

1.  [Empath：理解大规模文本中的话题信号](https://arxiv.org/abs/1602.06979)。E. Fast, B. Chen, M. S. Bernstein，CHI 人机交互系统会议论文集，2016年。

1.  [大规模图上的归纳表示学习](http://papers.nips.cc/paper/6703-inductive-representation-learning-on-large-graphs)。W. L. Hamilton, R. Ying 和 J. Leskovec，NeurIPS，2017年

[原文](https://medium.com/stellargraph/can-graph-machine-learning-identify-hate-speech-in-online-social-networks-58e3b80c9f7e)。经许可转载。

**简历：**[Pantelis Elinas](https://au.linkedin.com/in/pantelis-elinas-370971119) 是在澳大利亚领先的数字研究网络 CSIRO Data61 工作的高级研究工程师。他喜欢解决有趣的问题，分享知识和开发有用的软件工具。

[Anna Leontjeva](https://au.linkedin.com/in/anna-leontjeva-datascientist) 是一位拥有超过 10 年经验的高级数据科学家，目前在 CSIRO Data61 从事 StellarGraph 的工作，这是一个图形机器学习库。

[Yuriy Tyshetskiy](https://au.linkedin.com/in/yuriy-tyshetskiy-b42bb216) 是在 CSIRO Data61 领导图形机器学习系统团队的高级研究工程师，开发 StellarGraph 库。他的经验涉及理论等离子体物理、计算机视觉和图形机器学习等多个学科。

**相关：**

+   [前30名社交网络分析与可视化工具](https://www.kdnuggets.com/2015/06/top-30-social-network-analysis-visualization-tools.html)

+   [了解你的邻居：图上的机器学习](https://www.kdnuggets.com/2019/08/neighbours-machine-learning-graphs.html)

+   [图是数据科学的下一个前沿](https://www.kdnuggets.com/2018/10/graphs-next-frontier-data-science.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速通道进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 需求

* * *

### 更多相关主题

+   [识别机器学习可解决问题的 4 个因素](https://www.kdnuggets.com/2022/04/4-factors-identify-machine-learning-solvable-problems.html)

+   [如何识别时间序列数据集中的缺失数据](https://www.kdnuggets.com/how-to-identify-missing-data-in-timeseries-datasets)

+   [关于值得信赖的图神经网络的全面调查：…](https://www.kdnuggets.com/2022/05/comprehensive-survey-trustworthy-graph-neural-networks-privacy-robustness-fairness-explainability.html)

+   [机器学习如何促进在线学习](https://www.kdnuggets.com/2022/12/machine-learning-benefit-online-learning.html)

+   [在 5 分钟内用 Python 构建文本转语音转换器](https://www.kdnuggets.com/2022/09/build-texttospeech-converter-python-5-minutes.html)

+   [语音识别指标的演变](https://www.kdnuggets.com/2022/10/evolution-speech-recognition-metrics.html)
