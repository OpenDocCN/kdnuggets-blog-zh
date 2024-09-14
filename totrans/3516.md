# 拓扑分析与机器学习：朋友还是敌人？

> 原文：[https://www.kdnuggets.com/2015/09/ayasdi-topological-analysis-machine-learning.html](https://www.kdnuggets.com/2015/09/ayasdi-topological-analysis-machine-learning.html)

**作者：哈兰·塞克斯顿（Ayasdi）**。

对拓扑数据分析（TDA）不太熟悉的人常常问我类似的问题：“机器学习和 TDA 之间有什么区别？”这是一个很难回答的问题，部分原因在于这取决于你对机器学习（ML）的定义。

维基百科对机器学习（ML）的描述如下：

*“机器学习探索能够从数据中学习并做出预测的算法的研究与构建。这些算法通过从示例输入中构建模型来做出数据驱动的预测或决策，而不是严格遵循静态程序指令。”*

在这种广泛性层面上，人们可以争辩说 TDA 是一种机器学习（ML），但我认为大多数在这两个领域工作的人会不同意。

机器学习的具体实例彼此之间的相似性要大于与任何 TDA 示例的相似性。TDA 也是如此，其示例更像 TDA 而不是 ML。

为了说明 TDA 和 ML 之间的区别，也许更重要的是演示 TDA 和 ML 如何及为何能够很好地结合，我将给出两个过于简单的定义，然后通过一个实际例子来说明这一点。

一个简单的定义机器学习（ML）的方法是任何假设了一个参数化数据模型并从数据中学习这些参数的方法。

同样，我们可以将 TDA 定义为任何只使用“相似性”概念作为数据模型的方法。

从这个角度来看，机器学习（ML）模型要具体得多，详细得多，模型的成功取决于它如何与基础数据相契合。优势在于，当数据与模型相符时，结果可能会非常显著——明显的混乱被几乎完美的理解所取代。

TDA 的优势在于其广泛性。

使用拓扑数据分析（TDA）时，任何相似性的概念都足以使用该方法论。相比之下，机器学习（ML）则需要对相似性有强烈的理解（以及更多），才能对任何方法取得进展。

例如，从一个很长的名字列表中，可靠地预测身高和体重是不可能的。你需要更多的信息。

这里的一个关键要素是，源自拓扑学的算法通常对小错误非常宽容——即使你的相似性概念有些缺陷，只要它“基本正确”，TDA 方法通常也能产生有用的结果。

TDA 方法的广泛性比 ML 技术具有额外的优势，即使当 ML 方法很适用时，ML 方法通常会创建详细的内部状态，这些状态生成相似性概念，从而使得将 TDA 与 ML 结合起来，可以对数据集获得更多的洞见。

这一切听起来很棒，但它足够笼统（或者如果你不太宽容的话，足够模糊），可能意味着任何事情。

所以，让我们具体谈谈。

随机森林^(TM) 分类器是一种集成学习方法，通过在训练过程中构建大量决策树，并对这些“森林”（决策树的集合）应用“多数规则”来对非训练数据进行分类。

尽管构建的细节非常有趣和巧妙，但目前并不相关。你只需要记住随机森林的操作方式是：对每个数据点应用一组决策树，并返回一系列“叶节点”（输入“落入”的决策树中的叶子）。

在正常操作中，每棵树中的每个叶子都有一个与之相关联的类别C，这被解释为“当一个数据点到达树中的这个节点时，我知道它极有可能属于类别C。”随机森林通过计算每棵树的“叶节点类别投票”并选择赢家来进行分类。虽然这种方法在广泛的数据类型上非常有效，但也丢失了大量的信息。

如果你关心的是数据点类别的最佳猜测，那么你不需要看到那些额外的信息，但有时你需要更多。这些“额外”的信息可以通过定义两个数据点之间的距离为其各自“叶节点”不同的次数来转化为距离函数。

两个数据点之间的距离函数是一个完全有效的度量（实际上，它是数据集变换上的汉明度量），因此我们可以将TDA应用于它。

例如，我们来看一个随机抽取的5000点样本：

[https://archive.ics.uci.edu/ml/datasets/Dataset+for+Sensorless+Drive+Diagnosis](https://archive.ics.uci.edu/ml/datasets/Dataset+for+Sensorless+Drive+Diagnosis)

数据集相对复杂，包含48个连续特征，这些特征似乎是硬盘电流信号的未解释测量值。数据还包括一个分类列，有11个可能的值，描述了磁盘驱动组件的不同状态（可能是故障模式？）。一个显而易见的尝试是对特征列应用欧几里得度量，然后通过类别对图表进行着色。由于我们对特征列一无所知，首先要尝试的是邻域透镜。结果是一个没有特征的斑点：

![无特征的斑点](../Images/0011e0d722ed99cad66ab7783451ee92.png)

这令人失望。

利用一些内部调试能力，我查看了邻域透镜的散点图，明白了为什么效果如此糟糕——它看起来像一棵圣诞树：

![圣诞树](../Images/f661256f6b0e80f3d605511d7499ab16.png)

显然，在欧几里得度量下，类别没有局部化。

然而，如果你在数据集上构建一个随机森林，分类器的袋外误差非常小，这强烈表明分类器非常可靠。

所以我尝试使用随机森林赫明度量和该度量的邻域镜头来绘制一个图表：

![带邻域镜头的赫明度量](../Images/1583ae2543637f20acc5c9c6988feb3e.png)

这看起来非常好。为了确认，我们还查看了邻域镜头的散点图，结果与上图所示一致：

![邻域镜头散点图](../Images/ecef40df087632cf35281e83f9219ba6.png)

从图表和散点图中明显可以看出，随机森林在分类水平以下“看到”的强结构被 TDA 揭示出来了。原因是随机森林未能有效使用“额外”数据，而 TDA 使用了这些数据并从中受益颇丰。

然而，有人可能会问，这种结构是否是虚幻的——或许是我们在系统中使用的算法的某种伪影？在这个数据集的情况下，我们无法确定，因为我们对这些组没有更多了解。

尽管如此，我们仍然使用随机森林度量对客户数据进行了分析，以识别数千种复杂设备可能的故障模式，这些数据是在设备烧机期间收集的。类别是基于对设备进行返厂后的尸检（并非所有都涉及故障）。

我们发现，在这种情况下，随机森林度量在分类故障方面做得很好，而且我们获得的图像特征与上述类似。更重要的是，我们发现特定故障模式下的不同组有时具有不同的原因。

在这两个案例中的要点是，这些原因如果没有使用 TDA 与随机森林进一步分解空间，就会更难找到。

我们刚才看到的例子展示了 TDA 如何与机器学习方法结合，以获得比单独使用任一技术更好的结果。

这就是我们所说的 [机器学习与 TDA：更好地结合](http://www.ayasdi.com/resources/whitepaper/tda-and-machine-learning/) 的意思。

[原文](http://www.ayasdi.com/blog/bigdata/how-tda-and-machine-learning-enhance-each-other/)。

**相关：**

+   [世界经济论坛技术先锋与分析获奖者](/2015/08/wef-tech-pioneers-analytics-winners.html)

+   [机器学习课程。学习构建解决方案，荷兰代尔夫特，11月16-20日](/2015/09/perclass-machine-learning-course-delft-november.html)

+   [大师算法 – 顶级机器学习研究员 Pedro Domingos 的新书](/2015/09/book-master-algorithm-pedro-domingos.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT工作

* * *

### 更多相关话题

+   [机器学习的甜蜜点：NLP和文档分析中的纯粹方法](https://www.kdnuggets.com/2022/05/machine-learning-sweet-spot-pure-approaches-nlp-document-analysis.html)

+   [学习数据科学、机器学习和深度学习的坚实计划](https://www.kdnuggets.com/2023/01/mwiti-solid-plan-learning-data-science-machine-learning-deep-learning.html)

+   [市场数据与新闻：时间序列分析](https://www.kdnuggets.com/2022/06/market-data-news-time-series-analysis.html)

+   [学习数据分析和数据科学的最佳免费资源](https://www.kdnuggets.com/2024/03/365datascience-best-free-resources-learn-data-analysis-data-science)

+   [LangChain与LlamaIndex的比较分析](https://www.kdnuggets.com/comparative-analysis-of-langchain-and-llamaindex)

+   [KDnuggets 新闻，6月29日：20个数据科学基础Linux命令…](https://www.kdnuggets.com/2022/n26.html)
