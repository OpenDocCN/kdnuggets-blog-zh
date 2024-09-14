# 5 个你不能再忽视的机器学习项目，四月

> 原文：[https://www.kdnuggets.com/2017/04/five-machine-learning-projects-cant-overlook-april.html](https://www.kdnuggets.com/2017/04/five-machine-learning-projects-cant-overlook-april.html)

是时候再次呈现 "[5 个你不能再忽视的机器学习项目](/tag/overlook)" —— 这个谦虚的任务旨在将一些强大的鲜为人知的机器学习项目展示给更多人 —— 这次是针对 2017 年四月。之前的列表包括了通用的和专门的机器学习与深度学习库，以及辅助支持、数据清洗和自动化工具。

本次我们展示了 5 个你可能还未听说的机器学习相关项目，包括来自不同生态系统和编程语言的项目。你可能会发现，即使你对这些特定工具没有需求，检查它们的广泛实现细节或特定代码也可能帮助你生成一些自己的想法。与上一次一样，除了那些在我花时间在线时引起我注意的项目外，没有正式的纳入标准，项目必须有 Github 仓库。是的，这很主观，但没有任何定性的方法能使这个任务变得有意义。

不再赘述，以下是：你应该考虑查看的另外 5 个机器学习项目。它们没有特定顺序，但编号是为了让它们看起来有序，这主要是为了缓解我对未编号列表的焦虑。

**1\. [Scikit-plot](https://github.com/reiinakano/scikit-plot)**

> Scikit-plot 是一个非艺术性数据科学家深刻认识到可视化是数据科学过程中的关键组成部分之一的结果，而不仅仅是一个附加的想法。

![Scikit-plot](../Images/c444efff0a3ffc8028a21c27ae57d431.png)

我第一次通过作者在 Reddit 上的帖子遇到 Scikit-plot，并几乎立即开始使用它。该项目旨在为 Scikit-learn 用户提供一系列标准且有用的图表，毫无麻烦，因为我们都对使用这些图表感兴趣。库中包含的图表示例包括：

+   肘部图

+   特征重要性图

+   PCA 投影图

+   ROC 曲线

+   轮廓图

该库有 2 个 API，其中一个与 Scikit-learn 的集成非常紧密，以便控制对其 API 的调用（[工厂 API](http://scikit-plot.readthedocs.io/en/stable/apidocs.html)）。另一个 API 更为传统（[函数 API](http://scikit-plot.readthedocs.io/en/stable/Quickstart.html#the-functions-api)），但根据你的需求任选其一即可。

在这里找到[快速入门指南](http://scikit-plot.readthedocs.io/en/stable/Quickstart.html)，了解你需要的一切以开始使用这个有前景的小库。

**2\. [scikit-feature](https://github.com/jundongl/scikit-feature)**

> scikit-feature 是一个由亚利桑那州立大学数据挖掘与机器学习实验室开发的开源特征选择库，使用 Python 语言。它建立在广泛使用的机器学习包 scikit-learn 和两个科学计算包 Numpy 及 Scipy 的基础上。scikit-feature 包含大约 40 种流行的特征选择算法，包括传统特征选择算法以及一些结构性和流式特征选择算法。

尽管所有特征选择方法都有一个共同的目标，即识别冗余和无关的特征，但存在许多算法来处理这些相关问题——这是一个活跃的研究领域。在这方面，scikit-feature 既适用于实际的特征选择，也适用于特征选择算法的研究。该项目有一个由 ASU 托管的 [网站](http://featureselection.asu.edu/)，最初是在 MATLAB 中构思的，但后来移植到了 Python 生态系统。可以在 [这里](http://featureselection.asu.edu/algorithms.php) 找到 scikit-feature 支持的算法列表。

数据科学家 [Rubens Zimbres](https://www.linkedin.com/in/rubens-zimbres/) 最近如此生动地 [表达了](https://www.linkedin.com/feed/update/urn:li:activity:6257288156043890688/)（强调部分）：

> 经过一些经验，使用堆叠神经网络、并行神经网络、不对称配置、简单神经网络、多层、丢弃、激活函数等，有一个结论：**没有什么能比好的特征选择更重要**。

**3\. [Smile](https://github.com/haifengl/smile)**

> Smile（统计机器智能与学习引擎）是一个快速且全面的机器学习系统。凭借先进的数据结构和算法，Smile 提供了最先进的性能。
> 
> Smile 涵盖了机器学习的各个方面，包括分类、回归、聚类、关联规则挖掘、特征选择、流形学习、多维缩放、遗传算法、缺失值填补、高效最近邻搜索等。

![Smile](../Images/fdf585393b98bd76e5f5a8482cc31598.png)

现在，Smile 似乎成为了在 Java 和 Scala 世界中工作的人们的首选通用机器学习库——可以说是 JVM 上的 Scikit-learn。该项目拥有一个非常全面的 [教程网站](http://haifengl.github.io/smile/)，不仅涵盖了 Smile 的操作，还作为机器学习算法的一种优质介绍。

如果你在 JVM 上进行机器学习，Smile 绝对值得一看。我实际上很难相信你在那个生态系统中工作却不知道这个项目。

**4\. [Gensim](https://github.com/RaRe-Technologies/gensim)**

> Gensim 是一个用于主题建模、文档索引和大规模语料库相似性检索的 Python 库。目标用户是自然语言处理（NLP）和信息检索（IR）社区。

Gensim 具有多功能且旨在完备性，它实现了“[e]fficient multicore implementations of popular algorithms, such as online Latent Semantic Analysis (LSA/LSI/SVD), Latent Dirichlet Allocation (LDA), Random Projections (RP), Hierarchical Dirichlet Process (HDP) or word2vec deep learning.”

Gensim 的文档可以在 [这里](http://radimrehurek.com/gensim/install.html) 找到。你可以在 KDnuggets 上找到去年的 Gensim 初学者主题建模教程（由 Gensim 的开发者撰写）[这里](/2016/07/americas-next-topic-model.html)。

**5\. [Sonnet](https://github.com/deepmind/sonnet)**

从 [Sonnet 开源的博客公告](https://deepmind.com/blog/open-sourcing-sonnet)中可以看到：

> 自2015年11月首次发布以来，围绕 TensorFlow 迅速发展了一个多样化的高级库生态系统，使得常见任务可以更快地完成。Sonnet 与这些现有的神经网络库有许多相似之处，但也有一些专门针对我们研究需求的特性。随我们的《学习学习》论文发布的代码包含了 Sonnet 的初步版本，其他即将发布的代码将基于我们今天发布的完整库。

![Sonnet](../Images/17ba3996b8ce1c8368e915adb9382027.png)

DeepMind 已经开源了它的高级 TensorFlow 库，尽管该组织承认它类似于其他类似的库，但它实现了他们所需的功能，特别是：

> 循环神经网络的状态通常最好表示为异构张量的集合，将这些表示为平面列表可能容易出错。Sonnet 提供了处理这些任意层次结构的工具[.]

希望这篇文章让你对一些你以前不知道的库或一些你没有意识到自己想要的功能有了新的认识。

**相关**：

+   [5 个你不能再忽视的机器学习项目，1月](/2017/01/five-machine-learning-projects-cant-overlook-january.html)

+   [5 个你不能再忽视的机器学习项目](/2016/06/five-more-machine-learning-projects-cant-overlook.html)

+   [5 个你不能再忽视的机器学习项目](/2016/05/five-machine-learning-projects-cant-overlook.html)

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT 部门

* * *

### 更多此主题的信息

+   [每个数据科学家都应该了解的三个 R 语言库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [停止学习数据科学以寻找目标，并通过寻找目标来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [一项 90 亿美元的 AI 失败，详细分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [构建一个坚实的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)

+   [使用管道编写干净的 Python 代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)
