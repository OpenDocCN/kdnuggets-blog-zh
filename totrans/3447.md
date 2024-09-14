# 贝叶斯机器学习的解释

> 原文：[https://www.kdnuggets.com/2016/07/bayesian-machine-learning-explained.html](https://www.kdnuggets.com/2016/07/bayesian-machine-learning-explained.html)

**由Zygmunt Zając， [FastML](http://fastml.com/)**。

所以你知道贝叶斯规则了。它与机器学习有什么关系呢？很难掌握这些拼图如何组合在一起——我们知道这花费了我们一段时间。这篇文章是我们当时希望有的介绍。

*虽然我们对这个问题有一些了解，但我们不是专家，因此以下内容可能包含不准确之处甚至明显错误。如有发现，请在评论中或私下指出。*

### 贝叶斯主义者与频率主义者

从本质上讲，贝叶斯意味着概率性。这个特定的术语存在是因为概率有两种方法。贝叶斯主义者认为它是信念的度量，因此概率是主观的并且指向未来。

频率主义者有不同的看法：他们使用概率来指代过去的事件——这样它是客观的，不依赖于个人的信仰。这个名字来源于方法——例如：我们抛硬币100次，出现正面53次，所以正面的频率/概率是0.53。

要深入探讨此主题及更多内容，请参阅Jake VanderPlas的 [Frequentism and Bayesianism](https://jakevdp.github.io/blog/2015/08/07/frequentism-and-bayesianism-5-model-selection/) 系列文章。

### 先验、更新和后验

作为贝叶斯主义者，我们从一个称为先验的信念开始。然后我们获得一些数据并用它来更新我们的信念。结果称为后验。如果我们获得更多数据，旧的后验成为新的先验，循环重复。

这个过程使用了 **贝叶斯规则**：

```py

P( A | B ) = P( B | A ) * P( A ) / P( B )

```

`P( A | B )`，读作“在B的条件下A的概率”，表示条件概率：如果B发生，那么A的可能性有多大。

### 从数据中推断模型参数

在贝叶斯机器学习中，我们使用贝叶斯规则从数据（D）中推断模型参数（theta）：

```py

P( theta | D ) = P( D | theta ) * P( theta ) / P( data )

```

这些所有组件都是概率分布。

`P( data )` 是我们通常无法计算的，但由于它只是一个标准化常数，因此不是那么重要。在比较模型时，我们主要关心包含theta的表达式，因为 `P( data )` 对每个模型都是相同的。

`P( theta )` 是先验，或者说我们对模型参数可能是什么的信念。我们对此的意见通常比较模糊，如果我们有足够的数据，我们根本不在意。推断应趋于可能的theta，只要它在先验中不是零。先验是通过参数化的分布来指定的——请参见 [Where priors come from](http://zinkov.com/posts/2015-06-09-where-priors-come-from/)。

`P( D | theta )` 被称为给定模型参数的数据的似然。似然的公式是特定于模型的。人们经常使用似然来评估模型：一个对真实数据提供更高似然的模型更好。

最终，`P( theta | D )`，一个后验，是我们追求的目标。它是一个通过先验信念和数据获得的模型参数的概率分布。

当使用似然来获取模型参数的点估计时，这叫做[最大似然估计](https://en.wikipedia.org/wiki/Maximum_likelihood)，或 MLE。如果还考虑先验，那么就是[最大后验估计](https://en.wikipedia.org/wiki/Maximum_a_posteriori_estimation)（MAP）。如果先验是均匀的，MLE 和 MAP 是相同的。

注意，选择模型可以看作与选择模型（超）参数是分开的。然而，在实践中，它们通常通过验证一起进行。

### 模型与推断

推断指的是你如何学习模型的参数。模型与训练它的方式是分开的，尤其是在贝叶斯世界中。

考虑深度学习：你可以使用 Adam、RMSProp 或其他一些优化器来训练网络。然而，它们往往彼此相似，都是随机梯度下降的变体。相比之下，贝叶斯推断的方法之间的差异更为深刻。

两种最重要的方法是[蒙特卡罗采样](https://en.wikipedia.org/wiki/Monte_Carlo_method)和变分推断。采样是黄金标准，但速度较慢。[《大师算法》摘录](http://fastml.com/an-excerpt-from-the-master-algorithm/)中有更多关于 MCMC 的内容。

变分推断是一种明确设计用来在准确性和速度之间进行权衡的方法。它的缺点是模型特定，但隧道尽头有曙光——见下文的[软件部分](http://fastml.com/bayesian-machine-learning/#software)和[变分推断：统计学家的综述](http://arxiv.org/abs/1601.00670)。

### 统计建模

在贝叶斯方法的谱系中，有两种主要的风格。我们称第一种为*统计建模*，第二种为*概率机器学习*。后者包含了所谓的非参数方法。

建模发生在数据稀缺、珍贵且难以获得的情况下，例如在社会科学和其他难以进行大规模受控实验的环境中。想象一个统计学家用他所拥有的少量数据精心构建和调整模型。在这种情况下，你不遗余力地充分利用可用输入。

同时，在小数据情况下，量化不确定性非常重要，而这正是贝叶斯方法擅长的。

贝叶斯方法——特别是 MCMC——通常计算成本高。这与小数据密切相关。

为了体验一下，可以参考[示例](https://github.com/stan-dev/example-models/wiki/ARM-Models-Sorted-by-Type)来了解[回归分析与多级/层次模型](http://www.stat.columbia.edu/~gelman/arm/)一书。这是一本关于线性模型的书。它们从一个简单的线性模型开始，然后经过一个预测变量、两个预测变量、六个预测变量，直到十一种预测变量的多个线性模型。

这种劳动密集型的模式与当前机器学习的趋势相悖，后者使用数据让计算机自动从中学习。

### 概率机器学习

让我们尝试将“贝叶斯的”替换为“概率的”。从这个角度来看，它与其他方法的区别不大。就分类而言，大多数分类器能够输出概率预测。即使是支持向量机（SVM），它们在某种程度上是贝叶斯的对立面。

顺便说一下，这些概率只是分类器的信念声明。它们是否对应于真实的概率则是另一回事，这被称为[校准](http://fastml.com/classifier-calibration-with-platts-scaling-and-isotonic-regression/)。

### LDA，一种生成模型

[潜在狄利克雷分配](https://en.wikipedia.org/wiki/Latent_Dirichlet_allocation)是一种将数据投入并让它自动排序的方法（与手动建模相对）。它类似于矩阵因式分解模型，特别是非负矩阵分解。你从一个矩阵开始，其中行是文档，列是词，每个元素是给定文档中给定词的计数。LDA将大小为*n x d*的矩阵“因式分解”为两个矩阵，文档/主题（*n x k*）和主题/词（*k x d*）。

与因式分解的区别在于，你不能通过相乘这两个矩阵来得到原始矩阵，但由于适当的行/列之和为一，你可以“生成”一份文档。为了得到第一个词，你从一个主题中抽样，然后从这个主题中抽取一个词（第二个矩阵）。对你想要的词数重复这个过程。请注意，这是一种词袋表示，而不是词语的正确顺序。

上述是一个**生成**模型的例子，这意味着可以从中抽样或生成示例。与通常根据*x*区分类别的分类器不同，它们通常建模`P( y | x )`。生成模型关注的是*y*和*x*的联合分布`P( y, x )`。估计这个分布更困难，但它允许抽样，当然也可以从`P( y, x )`得到`P( y | x )`。

### 贝叶斯非参数方法

虽然没有确切的定义，但这个名称意味着模型中的参数数量可以随着数据的增加而增长。这类似于支持向量机（Support Vector Machines），例如，该算法从训练点中选择支持向量。非参数方法包括LDA的层次狄利克雷过程版本，其中主题的数量会自动选择，以及高斯过程。

#### 高斯过程

高斯过程与支持向量机有些相似——两者都使用核函数并具有类似的可扩展性（这一点通过使用近似方法在这些年里得到了极大改善）。GP的自然公式是回归，分类则是次要的。对于SVM则正好相反。

另一个区别是GP从根本上是概率性的（提供误差条），而SVM则不是。你可以在回归中观察到这一点。大多数“常规”方法只提供点估计。贝叶斯对应物，如高斯过程，也会输出不确定性估计。

![同方差噪声](../Images/4bbaac39ca24078ff088ace99cc9b745.png)

*致谢：Yarin Gal的[异方差丢弃不确定性](https://github.com/yaringal/HeteroscedasticDropoutUncertainty)和[我的深度模型不知道的东西](http://mlg.eng.cam.ac.uk/yarin/blog_3d801aa532c1ce.html)*

不幸的是，这并不是故事的结尾。即使像GP这样复杂的方法通常也基于同方差性假设，即均匀的噪声水平。实际上，噪声可能在输入空间中不同（具有异方差性）——见下图。

![异方差噪声](../Images/c3294dfa86d536a2d6e59cc885422ed8.png)

高斯过程的一个相对流行的应用是机器学习算法的超参数优化。数据量很小，无论是维度——通常只有几个需要调整的参数，还是样本数量。每个样本代表目标算法的一次运行，可能需要几个小时或几天。因此，我们希望以尽可能少的样本获取重要信息。

大多数关于GP的研究似乎发生在欧洲。英语国家在简化GP使用方面做了一些有趣的工作，最终形成了由Zoubin Ghahramani领导的[自动化统计学家](http://www.automaticstatistician.com/index/)项目。

观看这个[视频](https://www.youtube.com/watch?v=BS4Wd5rwNwE)的前10分钟，获得关于高斯过程的简明介绍。

### 软件

目前最显眼的贝叶斯软件可能是[Stan](http://mc-stan.org/)。Stan是一种概率编程语言，意味着它允许你指定和训练你想要的任何贝叶斯模型。它可以在Python、R等语言中运行。Stan有一个现代的采样器叫做[NUTS](http://andrewgelman.com/2011/11/30/stan-uses-nuts/)：

> [Stan](http://mc-stan.org/)中的大部分计算是通过哈密尔顿蒙特卡洛（HMC）完成的。HMC需要一些调整，因此Matt Hoffman编写了一种新的算法Nuts（“No-U-Turn Sampler”），它自适应地优化HMC。在许多情况下，Nuts实际上比最优的静态HMC更具计算效率！

Stan的一个特别有趣的功能是它具有[自动变分推断](http://arxiv.org/abs/1506.03431)：

> 变分推断是一种可扩展的近似贝叶斯推断技术。推导变分推断算法需要繁琐的特定模型计算；这使得自动化变得困难。我们提出了一种自动变分推断算法，即自动微分变分推断（ADVI）。用户只需提供一个贝叶斯模型和一个数据集；别无其他要求。

这种技术为将小规模建模应用于至少中等规模数据铺平了道路。

在 Python 中，最受欢迎的包是 [PyMC](https://pymc-devs.github.io/pymc3/)。它不像 Stan 那样先进或精致（开发者似乎在追赶 Stan），但仍然不错。PyMC 具有 NUTS 和 ADVI——这是一个带有 [minibatch ADVI 示例](https://gist.github.com/taku-y/b4da34be310718a6ea02) 的笔记本。该软件使用 Theano 作为后端，因此比纯 Python 更快。

[Infer.NET](http://research.microsoft.com/en-us/um/cambridge/projects/infernet/) 是微软的概率编程库。它主要从 C# 和 F# 等语言中获得，但显然也可以从 .NET 的 IronPython 中调用。Infer.net 默认使用 [期望传播](http://research.microsoft.com/en-us/um/cambridge/projects/infernet/docs/Working%20with%20different%20inference%20algorithms.aspx)。

除此之外，还有大量的包实现了各种贝叶斯计算的变体，从其他概率编程语言到专门的 LDA 实现。其中一个有趣的例子是 [CrossCat](https://github.com/probcomp/crosscat)：

> CrossCat 是一种通用的贝叶斯方法，用于分析高维数据表。CrossCat 通过在分层的非参数贝叶斯模型中进行近似推断，从数据中估计表中变量的完整联合分布，并为每个条件分布提供高效的采样器。CrossCat 结合了非参数混合建模和贝叶斯网络结构学习的优势：它可以通过假设潜在变量来建模任何联合分布，只要数据足够，但也能发现可观察变量之间的独立性。

还有 [BayesDB](http://probcomp.csail.mit.edu/bayesdb/)/[Bayeslite](https://github.com/probcomp/bayeslite) 来自同一团队。

### 资源

为了巩固你的理解，你可以阅读 Radford Neal 的 [Bayesian Methods for Machine Learning](http://www.cs.toronto.edu/~radford/ftp/bayes-tut.pdf) 教程。它与本文的主题一一对应。

我们发现 Kruschke 的 [Doing Bayesian Data Analysis](https://sites.google.com/site/doingbayesiandataanalysis/what-s-new-in-2nd-ed)（被称为小狗书）最为易读。作者详尽地解释了建模的所有细节。

[Statistical rethinking](http://xcelab.net/rm/statistical-rethinking/)似乎是类似的，但更新了。它包含了R + Stan的示例。作者Richard McElreath在YouTube上发布了一系列[讲座](https://www.youtube.com/playlist?list=PLDcUM9US4XdMdZOhJWJJD4mDBMnbTWw_z)。

在机器学习方面，这两本书都只涉及到线性模型。同样，Cam Davidson-Pilon的[Probabilistic Programming & Bayesian Methods for Hackers](http://camdavidsonpilon.github.io/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers/#contents)涵盖了*贝叶斯*部分，但没有涉及*机器学习*部分。

Alex Etz关于[理解贝叶斯](http://alexanderetz.com/understanding-bayes/)的系列文章也是如此。

对于那些数学爱好者，Kevin Murphy的[Machine Learning: a Probabilistic Perspective](https://www.cs.ubc.ca/~murphyk/MLbook/)可能是一本不错的书。如果你喜欢高难度的内容，那没问题，Bishop的[Pattern Recognition and Machine Learning](http://research.microsoft.com/en-us/um/people/cmbishop/prml/)可以满足你的需求。最近有一个[Reddit帖子](https://www.reddit.com/r/MachineLearning/comments/4c5gqk/is_pattern_recognition_and_machine_learning_still/)简要讨论了这两本书。

David Barber的[Bayesian Reasoning and Machine Learning](http://web4.cs.ucl.ac.uk/staff/D.Barber/pmwiki/pmwiki.php?n=Brml.HomePage)也很受欢迎，并且在网上免费提供，另外，[Gaussian Processes for Machine Learning](http://www.gaussianprocess.org/gpml/)这本经典书籍也是如此。

据我们所知，目前没有关于贝叶斯机器学习的MOOC课程，但*mathematicalmonk* 从贝叶斯的角度解释了[machine learning](https://www.youtube.com/playlist?list=PLD0F06AA0D2E8FFBA)。

Stan有一个详尽的[手册](http://mc-stan.org/documentation/)，PyMC有一个[教程](https://pymc-devs.github.io/pymc3/getting_started.html)以及相当多的示例。

**个人简介: [Zygmunt Zając](http://fastml.com/)** 喜欢新鲜空气、牵手以及海滩上的长途散步。他运营着[FastML.com](http://fastml.com)，这是全球最受欢迎的机器学习博客。除了各种有趣的文章，FastML现在还提供了一个[机器学习职位板块](http://fastml.com/jobs/)和一个[3D数据集可视化工具](http://cubert.fastml.com/)。

[原文](http://fastml.com/bayesian-machine-learning/)。经许可转载。

**相关内容：**

+   [机器学习关键术语解读](/2016/05/machine-learning-key-terms-explained.html)

+   [十大数据挖掘算法解读](/2015/05/top-10-data-mining-algorithms-explained.html)

+   [数据科学难题解读](/2016/03/data-science-puzzle-explained.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯的捷径。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的IT工作

* * *

### 更多相关主题

+   [建立一个稳固的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)

+   [使用管道编写干净的Python代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [停止学习数据科学以寻找目标，而是为了找到目标...](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的最佳资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [90亿美元AI失败，探讨](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [成功数据科学家的5个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)
