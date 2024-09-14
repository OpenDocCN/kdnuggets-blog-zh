# 17 个必知数据科学面试问题及答案，第 2 部分

> 原文：[https://www.kdnuggets.com/2017/02/17-data-science-interview-questions-answers-part-2.html](https://www.kdnuggets.com/2017/02/17-data-science-interview-questions-answers-part-2.html)
> 
> **编辑注：** 另见[**17 个必知数据科学面试问题及答案**](/2017/02/17-data-science-interview-questions-answers.html) 第 1 部分。这是第 2 部分。这里是 [第 3 部分](/2017/03/17-data-science-interview-questions-answers-part-3.html)。

本文回答了以下问题：

+   Q7\. 过拟合是什么？如何避免？

+   Q8\. 维度诅咒是什么？

+   Q9\. 如何确定模型中哪些特征最重要？

+   Q10\. 并行处理何时可以加速算法运行？何时可能使算法运行变慢？

+   Q11\. 集成学习的基本思想是什么？

+   Q12\. 在无监督学习中，如果数据集的真实情况未知，我们如何确定最有用的聚类数量？

* * *

### Q7\. 过拟合是什么？如何避免？

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT

* * *

[**Gregory Piatetsky**](/author/gregory-piatetsky) **回答：**

*(注意：这是对* [*21 个必知数据科学面试问题及答案，第 2 部分*](/2016/02/21-data-science-interview-questions-answers-part2.html)的修订版*)*

[过拟合](/tag/overfitting) 是指你构建了一个“过于紧密”地拟合数据的预测模型，以至于它捕捉到了数据中的随机噪声，而不是实际模式。因此，当应用于新数据时，模型预测将是错误的。

我们经常听到报道异常结果的研究（特别是如果你听《等待等待别告诉我》），或看到像“[橙色的二手车最不可能是柠檬](/2017/01/siegel-data-science-avoiding-prediction-pitfall.html)”这样的发现，或者得知研究推翻了以前确立的发现（鸡蛋现在对你没有坏处）。

许多此类研究产生了无法重复的可疑结果。

这是一个大问题，特别是在社会科学或医学领域，当研究人员经常犯数据科学的致命罪——**[过拟合数据](/tag/overfitting)。**

研究人员在没有适当统计控制的情况下测试太多的假设，直到他们偶然发现一些有趣的东西，然后就报告出来。不出所料，下次这种效果（部分由于偶然性）会大大减小或消失。

这些研究实践中的缺陷由John P. A. Ioannidis在其开创性论文[*为什么大多数已发表的研究结果是错误的*](http://www.plosmedicine.org/article/info%3Adoi%2F10.1371%2Fjournal.pmed.0020124)（PLoS Medicine，2005年）中进行了识别和报告。Ioannidis发现，结果往往被夸大或无法复制。他在论文中提供了统计证据，表明大多数声称的研究结果确实是错误的！

Ioannidis指出，为了使研究结果可靠，应该具备以下条件：

+   大样本量和显著效果

+   测试关系的数量更多且选择更少

+   设计、定义、结果和分析模式的更大灵活性

+   最小化由于财务和其他因素（包括该科学领域的受欢迎程度）造成的偏差

不幸的是，这些规则往往被违反，产生虚假结果，例如S&P 500指数与[孟加拉国黄油生产](https://2016/02/21-data-science-interview-questions-answers-part2.html)的强相关，或美国在科学、空间和技术上的支出与悬挂、窒息和勒死的自杀率相关（来自http://tylervigen.com/spurious-correlations）

![虚假相关](../Images/6ff3e462b9916e8ad30efb4541b53330.png)

(来源：[Tylervigen.com](http://tylervigen.com/chart-pngs/1.png))

在[虚假相关](http://www.tylervigen.com/discover)中查看更多奇怪和虚假的发现，或使用[Google correlate](https://www.google.com/trends/correlate/)等工具自行发现。

可以使用几种方法来避免“过拟合”数据：

+   尝试寻找最简单的假设

+   [正则化](https://en.wikipedia.org/wiki/Regularization_%28mathematics%29)（对复杂性进行惩罚）

+   [随机化测试](/2014/02/3-ways-to-test-accuracy-your-predictive-models.html)（随机化类别变量，尝试在此数据上应用你的方法——如果找到相同的强结果，说明存在问题）

+   嵌套交叉验证（在一个层级上进行特征选择，然后在外层级上进行整个方法的交叉验证）

+   调整[假发现率](https://en.wikipedia.org/wiki/False_discovery_rate)

+   使用[可重用的保留方法](/2015/08/feldman-avoid-overfitting-holdout-adaptive-data-analysis.html)——2015年提出的突破性方法

优秀的数据科学处于对世界科学理解的前沿，数据科学家有责任避免数据过拟合，并向公众和媒体普及糟糕数据分析的危险。

另见：

+   [你的机器学习模型为何错误的4个原因（以及如何修正）](/2016/12/4-reasons-machine-learning-model-wrong.html)

+   [当好的建议变坏](/2016/03/when-good-advice-goes-bad.html)

+   [数据挖掘和数据科学的根本罪过：过拟合](/2014/06/cardinal-sin-data-mining-data-science.html)

+   [避免过拟合的大主意：可重用的保留集以保持适应性数据分析中的有效性](/2015/08/feldman-avoid-overfitting-holdout-adaptive-data-analysis.html)

+   [通过可重用保留集克服过拟合：保持适应性数据分析中的有效性](/2015/08/reusable-holdout-preserving-validity-adaptive-data-analysis.html)

+   [11种聪明的过拟合方法及其避免方法](/2015/01/clever-methods-overfitting-avoid.html)

* * *

### Q8\. 什么是维度灾难？

[**普拉萨德·波雷**](/author/prasad-pore) **回答：**

> "随着特征或维度数量的增加，我们需要准确泛化的数据量呈指数增长。"
> 
> - [查尔斯·伊斯贝尔](http://ccsubs.com/video/yt%3AQZ0DtNFdDko/curse-of-dimensionality-georgia-tech-machine-learning/subtitles?lang=en)，乔治亚理工学院互动计算学院教授及高级副院长

下面是一个例子。图1（a）显示了一个维度中的10个数据点，即数据集中只有一个特征。它可以用只有10个值的线轻松表示，x=1, 2, 3... 10。

但如果我们再添加一个特征，相同的数据将在2维中表示（图1（b）），导致维度空间增加到10*10 =100。再添加第3个特征，维度空间将增加到10*10*10 = 1000。随着维度的增加，维度空间呈指数增长。

```py

   10^1 = 10

   10^2 = 100

   10^3 = 1000 and so on...
```

![n维空间比较](../Images/42858fce17f95d0c85abcb15027e0faa.png)

数据的指数增长导致数据集的高度稀疏，并不必要地增加了特定建模算法的存储空间和处理时间。想象一下高分辨率图像的图像识别问题，1280 × 720 = 921,600 像素，即921600维。天哪。这就是为什么它被称为**[维度灾难](/?s=curse+of+dimensionality)**。额外维度带来的价值比其对算法增加的开销要小得多。

总而言之，原本可以用10个空间单位表示的数据，在添加了2个维度后，需要1000个空间单位，仅仅因为我们在实验中观察到了这些维度。真正的维度是指准确泛化数据的维度，观察到的维度是指我们在数据集中考虑的其他维度，这些维度可能对准确泛化数据有或没有贡献。

* * *

### Q9\. 如何确定模型中哪些特征最为重要？

[**翠玉·范**](/author/thuy-pham) **回答：**

在应用机器学习中，成功在很大程度上依赖于数据表示（特征）的质量。高度相关的特征可以使分类模块中的学习/排序步骤变得容易。相反，如果标签类别是特征的非常复杂的函数，可能很难建立一个良好的模型 [Dom 2012]。因此，通常需要所谓的**[特征工程](/tags/feature-engineering)**，即将数据转化为与问题最相关的特征的过程。

一个[**特征选择**](/tag/feature-selection)方案通常涉及自动从大规模探索性特征池中选择显著特征的技术。冗余和无关的特征已知会导致准确性较差，因此丢弃这些特征应当是首要任务。相关性通常通过互信息计算进行评分。此外，输入特征应提供较高的类别区分度。特征的可分离性可以通过类别之间的距离或方差比来测量。最近的工作 [Pham 2016] 提出了基于系统投票的特征选择方法，这是一种结合上述标准的数据驱动方法。这可以作为广泛类别问题的通用框架。

![特征选择方法](../Images/52b9b5c9eb46050c3e235ecf3d98c8f6.png)

一种数据驱动的特征选择方法，结合了几种显著性标准 [Pham 2016]。

另一种方法是对那些不太重要的特征（例如，导致高错误度量的特征）进行惩罚，这些方法包括 Lasso 或 Ridge 等正则化方法。

参考文献：

+   [Dom 2012] P. Domingos. 关于机器学习的一些有用知识。*ACM 通讯*，55(10):78–87, 2012。 2.4

+   [Pham 2016] T. T. Pham, C. Thamrin, P. D. Robinson, 和 P. H. W. Leong. 强迫振荡测量中的呼吸伪影去除：一种机器学习方法。*生物医学工程，IEEE 汇刊*，已接受，2016。

* * *

### Q10\. 何时并行化能使你的算法运行得更快？何时可能使你的算法运行得更慢？

[**Anmol Rajpurohit**](https://twitter.com/hey_anmol) **回答：**

当任务可以分解为可以独立执行而无需通信或共享资源的子任务时，并行计算是一个好主意。即便如此，效率的实现是实现并行化好处的关键。在实际应用中，大多数程序有些部分需要以串行方式执行，并且并行可执行的子任务需要某种同步或数据传输。因此，很难预测并行化是否真的能使[**算法**](/tag/algorithms)比串行方法运行得更快。

相比于顺序完成任务所需的计算周期，并行计算总是会有开销。至少，这些开销包括将任务分解为子任务和汇总子任务结果。

**并行计算相对于顺序计算的性能主要取决于这些开销所消耗的时间与并行化带来的时间节省的比较。**

注意：并行化带来的开销不仅限于代码的运行时间，还包括编码和调试所需的额外时间（并行化与顺序代码）。

评估并行化收益的一个广为人知的理论方法是阿姆达尔定律，它提供了以下公式来测量并行运行子任务（在不同处理器上）与顺序运行（在单一处理器上）相比的加速：

![](../Images/9ffab2e095120202db6dca9392f1d551.png)

其中：

+   *S**延迟*是整个任务执行的理论加速；

+   *s* 是受益于改进系统资源的任务部分的加速；

+   *p* 是受益于改进资源的部分在原始执行时间中所占的比例。

要理解阿姆达尔定律的含义，请查看以下图示，该图展示了针对不同水平的可并行化任务，理论上随着处理器核心数量的增加而加速的情况：

![核心数量带来的加速](../Images/cc5d074b8a3261a73a6272f99642143c.png)

重要的是要注意，并不是每个程序都可以有效地并行化。实际上，由于顺序部分、通讯成本等限制，很少有程序能实现完美的加速。通常，大数据集构成了并行化的一个有力理由。然而，不应假设并行化会带来性能上的好处。应该在投入并行化努力之前，将并行化和顺序执行的性能在问题的子集上进行比较。

* * *

### Q11. 集成学习背后的思想是什么？

[**Prasad Pore**](/author/prasad-pore) **回答：**

> “在[统计学](https://en.wikipedia.org/wiki/Statistics)和[机器学习](https://en.wikipedia.org/wiki/Machine_learning)中，**集成方法**使用多种学习算法来获得比任何单一学习算法更好的[预测性能](https://en.wikipedia.org/wiki/Predictive_inference)。”
> 
> – [维基百科](https://en.wikipedia.org/wiki/Ensemble_learning)。

想象你在玩“谁想成为百万富翁？”并且达到了最后一个百万美元的问题。你对这个问题毫无头绪，但你还有观众投票和电话朋友的救生圈。谢天谢地。在这个阶段，你不想冒任何风险，那么你会怎么做以确保得到正确答案，成为百万富翁呢？

你会使用两个生命线，对吗？假设70%的观众说正确答案是D，而你的朋友也以90%的信心说正确答案是D，因为他是该问题领域的专家。使用两个生命线可以给你80%的平均信心，认为D是正确的，并使你更接近成为百万富翁。

这就是 [**集成方法**](/tag/ensemble-methods) 的方法。

著名的 [Netflix 奖](https://en.wikipedia.org/wiki/Netflix_Prize) 竞赛花费了近3年时间才实现10%的改进 [目标](/news/2009/n14/1i.html)。获胜者使用了梯度提升决策树来结合超过 [500 个模型](http://blog.echen.me/2011/10/24/winning-the-netflix-prize-a-summary/)。

在集成方法中，使用的模型越多样，最终结果的稳健性就会越高。

集成中使用的不同模型通过人口差异、生成的假设差异、使用的算法差异以及参数化差异来改善整体方差。主要有3种广泛使用的集成技术：

1.  装袋

1.  [提升](/tag/boosting)

1.  堆叠

如果你为相同的数据和相同的响应变量构建了不同的模型，你可以使用上述方法之一来构建集成模型。由于集成中使用的每个模型都有其自身的性能指标，因此一些模型的表现可能会优于最终的集成模型，而一些模型的表现可能会差于或与集成模型相当。但总体而言，集成方法将提高模型的整体准确性和稳定性，尽管这会以模型可理解性为代价。

更多关于集成方法的信息请参见：

+   [集成方法：优雅的技巧以提高机器学习结果](/2016/02/ensemble-methods-techniques-produce-improved-machine-learning.html)

+   [数据科学基础：集成学习者简介](/2016/11/data-science-basics-intro-ensemble-learners.html)

* * *

### Q12. 在无监督学习中，如果数据集的真实情况未知，我们如何确定最有用的聚类数量？

[**马修·梅奥**](/author/matt-mayo) **回答：**

在 **有监督** 学习中，数据集中类的数量是明确知道的，因为每个数据实例都标记为某个现存类别的成员。在最坏的情况下，我们可以扫描类别属性并计算存在的唯一条目数量。

在**[无监督学习](/tag/unsupervised-learning)**中，类属性和明确的类成员关系并不存在；事实上，无监督学习的一种主要形式——数据聚类——旨在通过最小化类间实例相似性和最大化类内相似性来近似类成员关系。聚类的一个主要缺点是需要在开始时以某种形式提供未标记数据集中存在的类数。如果幸运的话，我们可能会提前知道数据的**真实情况**——实际的类数。然而，这并不总是如此，原因有很多，其中之一是数据中可能根本没有定义的类数（从而没有定义的簇），无监督学习任务的整个重点就是调查数据并尝试强加某些有意义的最优簇和类数结构。

在不知道数据集的真实情况的情况下，我们如何知道数据簇的最优数量？正如预期的那样，实际上有许多方法可以回答这个问题。我们将查看两种特别流行的方法来尝试回答这个问题：肘部法则和轮廓法。

**肘部法则**

肘部法则通常是一个最佳的起点，尤其因为它易于解释和通过可视化进行验证。肘部法则旨在解释方差作为簇数（*k* 在 *k*-均值中的那个 *k*）的函数。通过绘制解释的方差百分比与 *k* 的关系，前 *N* 个簇应该提供显著的信息，解释方差；然而，某个最终的 *k* 值将导致信息增益显著减少，此时图表将呈现出明显的角度。这个角度将是肘部法则视角下的最优簇数。

显然，为了将方差绘制与不同簇数的关系，必须测试不同数量的簇。必须进行连续的完整聚类方法迭代，然后可以绘制和比较结果。

![肘部法则](../Images/3edaf0c9413aad166acbf351aa84e46f.png)

[图片来源](http://elf11.github.io/2015/08/18/Kmeans-analysis.html)。

**轮廓法**

轮廓法衡量对象与其自身簇的相似性——称为凝聚度——与其他簇的相似性——称为分离度。轮廓值是这种比较的手段，其范围为[-1, 1]；值接近1表示与自身簇中的对象关系紧密，而值接近-1则表示相反。在模型中产生大多数高轮廓值的聚类数据集可能是一个可接受且合适的模型。

![轮廓法](../Images/6e0efa5ccc574fd54efbf33c7defd138.png)

[图片来源](http://scikit-learn.org/stable/auto_examples/cluster/plot_kmeans_silhouette_analysis.html)。

了解轮廓法的更多信息请[点击这里](http://scikit-learn.org/stable/auto_examples/cluster/plot_kmeans_silhouette_analysis.html)。关于计算轮廓值的具体内容请见[这里](https://en.wikipedia.org/wiki/Silhouette_(clustering))。

**相关内容：**

+   [17个更多必须知道的数据科学面试问题及答案](/2017/02/17-data-science-interview-questions-answers.html)

+   [17个更多必须知道的数据科学面试问题及答案，第3部分](/2017/03/17-data-science-interview-questions-answers-part3.html)

+   [21个必须知道的数据科学面试问题及答案](/2016/02/21-data-science-interview-questions-answers.html)

+   [21个必须知道的数据科学面试问题及答案，第2部分](/2016/02/21-data-science-interview-questions-answers-part2.html)

### 更多相关话题

+   [20个问题（带答案）检测虚假数据科学家：ChatGPT…](https://www.kdnuggets.com/2023/01/20-questions-detect-fake-data-scientists-chatgpt-1.html)

+   [20个问题（带答案）检测虚假数据科学家：ChatGPT…](https://www.kdnuggets.com/2023/02/20-questions-detect-fake-data-scientists-chatgpt-2.html)

+   [7个数据分析面试问题与答案](https://www.kdnuggets.com/2022/09/7-data-analytics-interview-questions-answers.html)

+   [5个Python面试问题与答案](https://www.kdnuggets.com/2022/09/5-python-interview-questions-answers.html)

+   [3个更多SQL聚合函数数据科学面试问题](https://www.kdnuggets.com/2023/01/3-sql-aggregate-function-interview-questions-data-science.html)

+   [数据科学面试指南 - 第2部分：面试资源](https://www.kdnuggets.com/2022/04/data-science-interview-guide-part-2-interview-resources.html)
