# 比赛第二名：自动化数据科学

> 原文：[https://www.kdnuggets.com/2016/08/automating-data-science.html](https://www.kdnuggets.com/2016/08/automating-data-science.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者：Ankit Sharma，DataRPM**。

> **编辑注**：这篇博客文章曾参与最近的KDnuggets自动化数据科学与机器学习[博客竞赛](/2016/06/kdnuggets-blog-contest-automated-data-science.html)，并获得了第二名。

数据科学家是21世纪最性感的职业。但即使是数据科学家也不得不亲自动手完成工作。如果一些手动劳动密集型任务能够被自动化，而我们仅需专注于逻辑和研究，是否可以让这份工作重新变得有趣？

任何通用的数据科学管道都将包含以下组件。要自动化数据科学管道，我们需要分别自动化每个单独的组件。

![Automation pathway](../Images/a06b52be0f0a17540bb8e5678d1d9b9f.png)

在自动化**数据清洗与特征生成与选择**方面，已经进行了大量的研究，因为这些步骤几乎占据了数据科学项目时间的70%。

在这篇文章中，我们将关注那些鲜有讨论的话题：**算法选择**和**参数调优**。这两个组件都被认为是手动的。数据科学家花费数小时应用多个机器学习算法，然后调优参数。对于每个新问题，他们都会遵循相同的过程，并在一段时间后形成对哪个算法适合哪种类型的数据集以及如何最好地调优参数的直觉。如果你仔细观察，这本身就是一个学习问题。最近，许多研究和开发开始自动化这些组件，以在最短的时间和最少的努力下获得最佳准确性。

自动化算法选择可以通过一个叫做**元学习**的概念来实现，而自动化参数调优则有不同的技术，例如网格搜索、贝叶斯优化等，我们将在文章后面讨论这些技术。

**使用元学习自动化算法选择**

元学习有两个重要部分——元存储和元模型算法。元存储用于捕捉数据集的元数据和经过超参数调优后获得最佳结果的算法的元数据。数据集一旦加载到环境中，其元数据会生成并存储在集中式的MetaStore中。在运行任何机器学习算法（尤其是分类和回归模型）之前，元模型算法会分析该数据集的元数据，并运行算法排名逻辑，以预测在10-20种不同模型中哪个模型可能在该数据集上表现更好。

![Meta-model algorithm](../Images/00c72639d3a627a2243f2d50693fdfa4.png)

一旦选择了算法，下一步是运行超参数调整算法来调整这个模型。元模型算法将主要使用 MetaStore 和机器学习算法。元模型算法还将有助于减少超参数空间，从而快速收敛以提供最佳结果。

要设置 MetaStore，我们需要捕获数据集元数据和最佳算法运行元数据。对于两者，可以存储许多特征。以下是一些有用的特征列表：

**数据集元数据**

1.  观察数

1.  观察数与属性数的比率

1.  类别特征的数量

1.  数值特征的数量

1.  类别数

1.  类别不平衡比率

1.  缺失值百分比

1.  多变量正态性

1.  特征的偏度

1.  特征选择后的属性百分比

1.  异常值的百分比

1.  决策树模型特性，如每个特征的节点数、最大树深度、形状、树的不平衡等

1.  最大费舍尔判别率

1.  最大（个体）特征效率

1.  集体特征效率

1.  类边界上的点的比例

1.  平均类内/类间最近邻距离的比率

1.  一对一最近邻分类器的留一误差率

1.  一对一最近邻分类器的非线性

1.  每维度的平均点数

1.  最优聚类数

1.  聚类的 WSSE

1.  数据集的领域

**算法元数据**

1.  数据集 ID

1.  算法 ID

1.  超参数

1.  运行时间

**元模型算法**

这是元学习的核心。它本身是一个机器学习算法，基本上从不同算法在大量数据集上的各种运行中学习。我们从特定数据集上的最佳算法运行中得出的算法元数据并将其存储在 MetaStore 中，数据集元数据与之结合形成一个新的数据集，该数据集被输入机器学习算法中，算法将为特定数据集排名 ML 算法的选择。

![算法元学习](../Images/6ad36314803ac0e1dd5a9bf08c411e99.png)

*元学习以获取算法选择的元知识，来源：Soares, C., Giraud-Carrier, C., Brazdil, P., Vilalta, R.: Metalearning: Applications to Data Mining. Springer (2009)*

**超参数调整**

几乎所有的机器学习算法都有超参数。SVM 具有正则化参数（C），k-means 具有聚类数（k），神经网络具有隐藏层数量、dropout 等作为超参数。

高效地找到这些超参数的最佳解决方案是任何 ML 解决方案的关键。基本上有三种著名的技术来调整任何机器学习算法的超参数：

![超参数搜索技术](../Images/1e1067df10d4d2318cda740e263e475d.png)

+   **手动搜索**：数据科学家尝试不同的值，并且可能通过运气找到最佳解决方案。这在数据科学社区中非常常见。

+   **网格搜索**：机器通过评估每个参数组合并检查最佳结果来完成所有工作。它仅适用于较少的特征；随着特征集的增加，组合数量也会增加，从而增加计算时间。

+   **随机搜索**：适用于大量特征

近期在该领域有一些进展，数据科学家们正在使用**贝叶斯方法来优化超参数**。这种方法适用于特征数量非常大的情况，但对于数据较少和特征较少的数据集可能过于复杂。目标是最大化某个真正未知的函数 f。通过观察（即在特定超参数值下评估函数）获得关于该函数的信息。这些观察结果用于推断函数值的后验分布，表示可能函数的分布。该方法需要较少的迭代次数来优化超参数值。它可能不会总是找到最佳解决方案，但会在惊人的少量迭代中提供相对接近最佳的解决方案。

最近这个领域非常热闹。有一些[商业](/software/automated-data-science.html)和开源工具正在努力让数据科学家的工作更轻松。

需要查看的开源工具：

+   [Spearmint](https://github.com/JasperSnoek/spearmint/)

+   [TPOT](https://github.com/rhiever/tpot)

+   [Hyperopt](https://github.com/hyperopt/hyperopt)

进一步阅读和参考资料：

+   [数据挖掘和 KDD 的元学习](http://www.fit.vutbr.cz/study/courses/VPD/public/1213VPD-Striz.pdf)

+   MLA Bergstra, James S., 等. "[超参数优化算法](http://papers.nips.cc/paper/4443-algorithms-for-hyper-parameter-optimization.pdf)"。《神经信息处理系统进展》。2011年。

+   [TPOT：用于自动化数据科学的 Python 工具](http://www.randalolson.com/2016/05/08/tpot-a-python-tool-for-automating-data-science/)

+   [贝叶斯优化用于超参数调整](https://arimo.com/data-science/2016/bayesian-optimization-hyperparameter-tuning/)

**简介：[Ankit Sharma](https://www.linkedin.com/in/anktsh)** 是 DataRPM 的数据科学家。

**相关：**

+   [数据科学自动化：揭穿误解](/2016/08/data-science-automation-debunking-misconceptions.html)

+   [获胜者是……逐步回归](/2016/08/winner-stepwise-regression.html)

+   [TPOT：用于自动化数据科学的 Python 工具](/2016/05/tpot-python-automating-data-science.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织在 IT 领域

* * *

### 更多相关主题

+   [神经网络与深度学习：教科书（第 2 版）](https://www.kdnuggets.com/2023/07/aggarwal-neural-networks-deep-learning-textbook-2nd-edition.html)

+   [宣布博客写作比赛，获胜者将获得 NVIDIA GPU！](https://www.kdnuggets.com/2022/11/blog-writing-contest-nvidia-gpu.html)

+   [5 种工具用于自动化数据清洗过程](https://www.kdnuggets.com/5-tools-for-automating-data-cleaning-processes)

+   [自动化思维链：AI 如何自我提示以进行推理](https://www.kdnuggets.com/2023/07/automating-chain-of-thought-ai-prompt-itself-reason.html)

+   [停止学习数据科学来寻找目标，找到目标再…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [数据科学最低要求：您需要了解的 10 项基本技能…](https://www.kdnuggets.com/2020/10/data-science-minimum-10-essential-skills.html)
