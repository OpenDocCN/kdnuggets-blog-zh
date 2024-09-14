# 5 个你不能再忽视的机器学习项目

> 原文：[https://www.kdnuggets.com/2016/05/five-machine-learning-projects-cant-overlook.html](https://www.kdnuggets.com/2016/05/five-machine-learning-projects-cant-overlook.html)

一般来说，**流行的**机器学习项目之所以流行，是因为它们提供了广泛的所需服务，或者它们是第一个（或可能是最好的）为用户提供某个特定专业服务的项目。这些流行的项目包括 Scikit-learn、TensorFlow、Theano、MXNet（也许？）、Weka（以前）等等。根据你所工作的特定生态系统和你的机器学习目标，你认为流行的项目可能会有所不同；然而，它们都有一个共同点，那就是它们为大量用户提供服务。

不过，确实存在各种规模较小的[机器学习项目](https://www.kdnuggets.com/2020/03/20-machine-learning-datasets-project-ideas.html)正在被人们构建和使用：管道、包装器、高级 API、清理工具等等。它们提供了专业和灵活的服务，通常面向较少的用户，因各种原因而异。这篇文章将介绍 5 个这样的较小项目，以便读者熟悉。

我并不是建议你去尝试所有（或任何）这些项目；如果你有某些特定需求，而恰好在这个列表中找到一个对应的工具，那么尽管试试。真正的价值在于查看这些项目提供了什么、它们是如何实现的，以及其他人是如何构建以适应他们的生态系统的。你可能会得到一些关于自己项目的好主意。但最好的情况是：这里的某些项目完美地满足了需求，感谢这些开发者的工作，你解决了一个问题。

对于这些项目没有真实的标准，我很抱歉地报告。这些仅仅是我在过去几个月中注意到的一些有趣项目的集合，觉得足够有前景而分享给读者。此外，请注意，我对 Python 生态系统非常投入，因此这些工具也相应地被发现。我对 R、C++ 或任何其他特定环境所提供的项目没有任何偏见（未来可能会发现并分享这些项目）；不过，这个列表是基于我在互联网漫游时寻找有用工具的结果，比较泛泛地汇总而成。

所以，这里是：5 个你绝对应该查看的机器学习项目，顺序不重要（但按顺序编号，因为我喜欢编号）：

**1\. [Deepy](https://github.com/zomux/deepy)**

Deepy 是一个基于 Theano 的可扩展深度学习框架。它为 LSTM、批量归一化和自编码器等组件提供了一个干净的高层接口。Deepy 显然旨在简洁，其[文档](http://deepy.readthedocs.io/en/latest/)和示例也旨在如此。它还有一个姊妹项目，使用 Deepy 实现了[深度递归注意力作家](http://arxiv.org/pdf/1502.04623.pdf)（DRAW）生成模型。

作为 Deepy 简洁性和干净性的一个例子，这里是项目 GitHub 上一个包含 dropout 的多层模型示例：

```py
# A multi-layer model with dropout for MNIST task.
from deepy import *

model = NeuralClassifier(input_dim=28*28)
model.stack(Dense(256, 'relu'),
            Dropout(0.2),
            Dense(256, 'relu'),
            Dropout(0.2),
            Dense(10, 'linear'),
            Softmax())

trainer = MomentumTrainer(model)

annealer = LearningRateAnnealer(trainer)

mnist = MiniBatches(MnistDataset(), batch_size=20)

trainer.run(mnist, controllers=[annealer])

```

</br class="blank" />

你可能已经听说过 Deepy；截至撰写本文时，它的 GitHub 仓库已有 305 个星标，被分叉了 51 次。该项目是高层次深度学习 API 和包装器的一个不错示例，这些 API 和包装器正变得越来越普及（或似乎如此）。Deepy 的作者是 [Raphael Shu](http://raphael.uaca.com/)。

**2\. [MLxtend](https://github.com/rasbt/mlxtend)**

[Sebastian Raschka](https://twitter.com/rasbt) 汇集了 MLxtend，他迅速指出这是一项正在进行中的工作，但也试图覆盖许多不同的功能。MLxtend 是一个用于机器学习任务的有用工具和扩展的集合。

![MLxtend](../Images/a1803f039bb3a49863e5f0c86669d426.png)Sebastian 共享了以下内容，关于项目的起源及其目标：

> 本质上，这只是与机器学习和数据科学相关的有用工具和参考实现的集合。我为什么会想到这个？有几个原因：
> 
> 1.  实现了一些我在其他地方找不到的算法（例如，序列特征选择算法，多数投票分类器，堆叠估计器，绘制决策区域，...）
> 1.  
> 1.  教学目的的实现（逻辑回归，softmax 回归，多层感知机，PCA，核 PCA...）；这些实现侧重于代码可读性，而非纯粹效率
> 1.  
> 1.  便捷的包装器：tensorflow softmax 回归和多层感知机，pandas 数据框的列标准化

这基本上是一个常用的通用机器学习函数库，Sebastian 编写并频繁使用。此外，Sebastian 非常喜欢编码，并认为如果他将这个“动物园”（如他所称）提供给他人，他可能会保持代码比平时“整洁”。

许多实现的函数与 scikit-learn 的 API 相似，但未来添加的功能不一定会受此限制。主要信息：Sebastian 承诺还有更多的内容即将推出……所以请继续关注。Sebastian 可能玩弄的任何特性或新算法都有很大机会被打包到 MLxtend 中。

**3\. [datacleaner](https://github.com/rhiever/datacleaner)**

datacleaner是研究员[Randal Olson](http://www.randalolson.com/blog/)的作品，他还负责了出色的[TPOT机器学习管道](/2016/05/tpot-python-automating-data-science.html)项目。Olson将Data Cleaner称为“一个自动清理数据集并为分析做好准备的Python工具。”他迅速声明这不是魔法，但也指出了它*能*做的事情：

> datacleaner的功能是节省你在数据已经以pandas DataFrames能够处理的格式中进行编码和清理的时间。

datacleaner仍在开发中，但目前能够处理以下常规（且耗时的）数据清理操作：可选地删除具有缺失值的行；按列将缺失值替换为众数或中位数；用数值等效物编码非数值变量。Randal告诉我们他正在寻找贡献者，特别是那些对datacleaner可以以自动化方式执行的数据清理操作有更多想法的人。

Randal对细节的关注是任何阅读过他的博客或Github仓库的人都知道的，这个项目的简明文档也不例外。我最近一直在使用datacleaner，到目前为止它兑现了承诺。

**4\. [auto-sklearn](https://github.com/automl/auto-sklearn)**

auto-sklearn是为Scikit-learn环境提供的自动化机器学习工具。

> auto-sklearn使机器学习用户无需选择算法和调整超参数。它利用了贝叶斯优化、元学习和集成构建的最新优势。通过阅读这篇在[NIPS 2015](http://papers.nips.cc/paper/5872-efficient-and-robust-automated-machine-learning.pdf)上发布的论文，了解更多关于auto-sklearn背后的技术。

![auto-sklearn](../Images/4aafb75cd73f71f8b37f7d7bc31f4e70.png)

其[文档](http://automl.github.io/auto-sklearn/stable/)相当详尽，并且该仓库包含了[几个简洁的示例](https://github.com/automl/auto-sklearn/tree/master/example)。我承认我还没有使用过，但许多人已经使用过；它在Github上收获了近400个星标。鉴于我对Scikit-learn的偏爱，我想我将在不久的将来尝试这个。

auto-sklearn主要由[弗莱堡大学的自动化算法设计机器学习](http://aad.informatik.uni-freiburg.de/)小组开发。

**5\. [深度挖掘](https://github.com/HDI-Project/DeepMining)**

Deep Mining是一个机器学习管道自动调整器，由MIT CSAIL实验室的[Sebastien Dubois](http://sds-dubois.github.io/)带来。来自仓库的说明：

> 这个软件会迭代而智能地测试一些超参数集，以尽可能快地找到最佳的超参数组合，从而实现管道能提供的最佳分类准确性。

![深度挖掘](../Images/320d6937c084938979886b791a91e8dd.png)

深度挖掘（Deep Mining）似乎不是一个广为人知的项目，鉴于其相对少量的仓库星标；然而，鉴于它源于CSAIL，并且在过去一个月内有开发活动，值得将其与其他类似的自动化管道工具进行基准测试。它提供了[一些示例](https://github.com/HDI-Project/DeepMining/tree/master/gcp_hpo/examples)，使用起来似乎也很直接。

关于使用的方法：

> 文件夹 GCP-HPO 包含了所有实现高斯 Copula 过程（GCP）和基于此的超参数优化（HPO）技术的代码。高斯 Copula 过程可以看作是高斯过程的改进版本，它不对边际分布假设高斯先验，而是基于更复杂的先验。这项新技术被证明在超参数优化中优于基于 GP 的方法，而 GP 本身已经远远优于随机搜索。

一篇关于 GCP 方法的论文即将发布。

**相关**：

+   [GitHub 上的十大机器学习项目](/2015/12/top-10-machine-learning-github.html)

+   [TPOT：一个用于自动化数据科学的 Python 工具](/2016/05/tpot-python-automating-data-science.html)

+   [十大 IPython Notebook 数据科学和机器学习教程](/2016/04/top-10-ipython-nb-tutorials.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 更多相关话题

+   [可以帮助你解决实际问题的数据科学项目](https://www.kdnuggets.com/2022/11/data-science-projects-help-solve-real-world-problems.html)

+   [9 个可以让你获得学位的职业证书… 如果…](https://www.kdnuggets.com/9-professional-certificates-that-can-take-you-onto-a-degree-if-you-really-want-to)

+   [如何利用机器学习自动标记数据](https://www.kdnuggets.com/2022/02/machine-learning-automatically-label-data.html)

+   [7 个你不能错过的机器学习算法](https://www.kdnuggets.com/7-machine-learning-algorithms-you-cant-miss)

+   [2024 年你可以参加的五大顶级机器学习课程](https://www.kdnuggets.com/5-top-machine-learning-courses-you-can-take-in-2024)

+   [2024 年你不能错过的前 5 个 AI 播客](https://www.kdnuggets.com/top-5-ai-podcasts-you-cant-miss-in-2024)
