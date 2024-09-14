# 极简且干净的机器学习算法实现的优秀合集。

> 原文：[https://www.kdnuggets.com/2017/01/great-collection-clean-machine-learning-algorithms.html](https://www.kdnuggets.com/2017/01/great-collection-clean-machine-learning-algorithms.html)

想从头实现机器学习算法吗？

最近的 KDnuggets 投票询问了 "[你在过去 12 个月中用于实际数据科学应用的方法/算法？](/2016/08/new-poll-data-science-methods-algorithms-used.html)"，结果可以在 [这里找到](/2016/09/poll-algorithms-used-data-scientists.html)。结果按行业就业部门和地区进行了分析，但对新手来说，主要收获是涵盖了各种算法。

![前 10 大算法](../Images/f40f1e7b92b0e89bcbbd0b5980c159b2.png)明确一点：这不是可用的 [机器学习算法](https://www.kdnuggets.com/2016/08/10-algorithms-machine-learning-engineers.html) 的完整展示，而是最常用算法的一个子集（根据我们的读者）。**今天存在很多机器学习算法。** 但既然有这么多机器学习 [库、项目、程序和其他项目](/2016/11/top-20-python-machine-learning-open-source-updated.html) 涵盖了这些算法，涉及各种编程语言、不同平台和环境，为什么你还想从头实现一个算法呢？

[Sebastian Raschka](https://twitter.com/rasbt)，著名的机器学习爱好者、“数据科学家”（他的引号，注意不是我的），博士生以及 [《Python 机器学习》](https://www.amazon.com/Python-Machine-Learning-Sebastian-Raschka/dp/1783555130) 的作者，对于 [从头实现机器学习算法](/2016/05/implement-machine-learning-algorithms-scratch.html) 有如下看法：

> 从头实现算法有几个不同的原因可能是有用的：
> 
> 1.  这可以帮助我们理解算法的内部工作。
> 1.  
> 1.  我们可以尝试更高效地实现一个算法。
> 1.  
> 1.  我们可以向算法添加新功能或尝试不同的核心思想变体。
> 1.  
> 1.  我们规避许可问题（例如，Linux 与 Unix）或平台限制。
> 1.  
> 1.  我们希望发明新算法或实现尚未实现/分享的算法。
> 1.  
> 1.  我们对 API 不满意和/或我们希望将其更“自然”地集成到现有的软件库中。

所以，假设你有一个从头实现机器学习算法的理由。或者，假设你只是想更好地理解机器学习算法的工作原理（#1），或者想为算法添加新功能（#3），或完全是其他的事情。找到一个独立实现的算法集合，最好是来自同一个作者，以便于代码从一个算法到下一个算法更容易理解，并且（特别是）实现了最小化的干净代码，这可能是一个具有挑战性的任务。

进入 [MLAlgorithms Github 仓库](https://github.com/rushter/MLAlgorithms)，由 [rushter](https://github.com/rushter) 描述为：

> 一系列最小化和干净实现的机器学习算法。

这实际上对所包含的代码是一个很好的描述。作为一个从头实现了众多机器学习算法的人——需要查阅教科书、论文、博客帖子和代码以获取指导——并且研究了算法代码以了解特定实现的工作原理以便进行修改的人，我可以根据经验说，这段代码非常容易跟随。

更多来自 README 的内容：

> 本项目面向那些想要学习机器学习算法内部工作原理或从头实现这些算法的人。代码比优化后的库更容易理解，也更易于操作。所有算法都用 Python 实现，使用了 numpy、scipy 和 autograd。

所以，重要点：算法没有优化，因此可能不适合生产环境；算法使用 Python 实现，仅使用了少数常见的科学计算库。只要你对这些点感到满意，并且有兴趣学习如何实现算法或只是想更好地理解它们，这段代码就适合你。

截至撰写时，rushter 已经实现了各种任务类型的算法，包括聚类、分类、回归、降维以及各种深度神经网络算法。为了帮助你通过实现来协调学习，以下是实现算法链接的部分列表以及一些辅助材料。请注意，所有这些实现算法的荣誉归于仓库所有者 [rushter](https://github.com/rushter)，他的 Github 个人资料显示他可以接受聘用。我怀疑这个仓库会是一个很受欢迎的简历。

**深度学习（MLP、CNN、RNN、LSTM）**

![深度学习](../Images/35cca5c97cb4d4033c23fdadcb5820b8.png)从这里开始：[理解深度学习的 7 步](/2016/01/seven-steps-deep-learning.html)

*从对深度神经网络的模糊理解到在 7 步中成为知识渊博的实践者！*

代码：[rushter 的 neuralnet 模块](https://github.com/rushter/MLAlgorithms/tree/master/mla/neuralnet)

**线性回归/逻辑回归**

![线性回归](../Images/9ad77abc2d73c45236832b6f4edf7c6c.png) 从这里开始： [线性回归、最小二乘法与矩阵乘法：简明技术概述](/2016/11/linear-regression-least-squares-matrix-multiplication-concise-technical-overview.html)

*线性回归是一个简单的代数工具，试图找到最“优”的线来拟合两个或更多属性。*

代码： [rushter 的 linear_models.py](https://github.com/rushter/MLAlgorithms/blob/master/mla/linear_models.py)

**随机森林**

![随机森林](../Images/d7df51ea9647f66d47d5ac3397320dda.png) 从这里开始： [随机森林：犯罪教程](/2016/09/reandom-forest-criminal-tutorial.html)

*在这里了解随机森林的概况，根据最近的调查，这是 KDnuggets 读者使用最广泛的算法之一。*

代码： [rushter 的 linear_models.py](https://github.com/rushter/MLAlgorithms/blob/master/mla/ensemble/random_forest.py)

**支持向量机**

![支持向量机](../Images/d5f9a91b5d6ed87326db5bc95500daf4.png) 从这里开始： [支持向量机：简明技术概述](/2016/09/support-vector-machines-concise-technical-overview.html)

*支持向量机仍然是一个流行且经过时间考验的分类算法。本文提供了它们功能的高层次简明技术概述。*

代码： [rushter 的 svm.py](https://github.com/rushter/MLAlgorithms/blob/master/mla/svm/svm.py) 和 [rushter 的 kernels.py](https://github.com/rushter/MLAlgorithms/blob/master/mla/svm/kernerls.py)

**K 最近邻**

![KNN](../Images/4322b5f9ae66be35375b65bec3b6c05f.png) 从这里开始： [使用 Python 实现自己的 k 最近邻算法](/2016/01/implementing-your-own-knn-using-python.html)

*对最常用的机器学习算法之一，k 最近邻，进行详细解释。*

代码： [rushter 的 knn.py](https://github.com/rushter/MLAlgorithms/blob/master/mla/knn.py)

**朴素贝叶斯**

![贝叶斯](../Images/4bbaac39ca24078ff088ace99cc9b745.png) 从这里开始： [贝叶斯机器学习解释](/2016/07/bayesian-machine-learning-explained.html)

*在这里获取精彩的贝叶斯机器学习介绍解释，以及进一步学习的建议。*

代码： [rushter 的 naive_bayes.py](https://github.com/rushter/MLAlgorithms/blob/master/mla/naive_bayes.py)

**K 均值聚类**

![聚类](../Images/a4235ade8eec2481aba75af7b5189ef6.png) 从这里开始： [比较聚类技术：简明技术概述](/2016/09/comparing-clustering-techniques-concise-technical-overview.html)

*今天使用了多种聚类技术。鉴于聚类在日常数据挖掘中的广泛应用，本文提供了两种典型技术的简明技术概述。*

代码： [rushter 的 kmeans.py](https://github.com/rushter/MLAlgorithms/blob/master/mla/kmeans.py)

所有实现的算法都附带示例代码，这对理解 API 很有帮助。一个示例如下：

上述算法实现仅是仓库内容的一部分，因此请务必查看以下实现：

+   [**高斯混合模型**](https://github.com/rushter/MLAlgorithms/blob/master/mla/gaussian_mixture.py)

+   [**主成分分析 (PCA)**](https://github.com/rushter/MLAlgorithms/blob/master/mla/pca.py)

+   [**因式分解机**](https://github.com/rushter/MLAlgorithms/blob/master/mla/fm.py)

+   [**限制玻尔兹曼机 (RBM)**](https://github.com/rushter/MLAlgorithms/blob/master/mla/rbm.py)

+   [**t-分布随机邻域嵌入 (t-SNE)**](https://github.com/rushter/MLAlgorithms/blob/master/mla/tsne.py)

+   [**梯度提升机**](https://github.com/rushter/MLAlgorithms/blob/master/mla/ensemble/gbm.py)

如果你发现这些代码示例对你有帮助，我相信[作者](https://github.com/rushter)会很高兴听到你的反馈。

**相关**：

+   [为什么从零开始实现机器学习算法？](/2016/05/implement-machine-learning-algorithms-scratch.html)

+   [伟大的算法教程汇总](/2016/09/great-algorithm-tutorial-roundup.html)

+   [机器学习工程师需要了解的 10 个算法](/2016/08/10-algorithms-machine-learning-engineers.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT 工作

* * *

### 更多相关主题

+   [使用管道编写干净的 Python 代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [利用数据科学使清洁能源更加公平](https://www.kdnuggets.com/2022/03/data-science-make-clean-energy-equitable.html)

+   [如何为 Python 应用程序创建最小化的 Docker 镜像](https://www.kdnuggets.com/how-to-create-minimal-docker-images-for-python-applications)

+   [免费数据科学、数据工程等课程合集](https://www.kdnuggets.com/collection-of-free-courses-to-learn-data-science-data-engineering-machine-learning-mlops-and-llmops)

+   [KDnuggets 新闻，5月25日：每个 Python 机器学习工程师都需要了解的 6 个工具…](https://www.kdnuggets.com/2022/n21.html)

+   [KDnuggets™ 新闻 22:n06，2月9日：数据科学编程…](https://www.kdnuggets.com/2022/n06.html)
