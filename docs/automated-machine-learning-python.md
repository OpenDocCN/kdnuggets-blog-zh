# Python 中的自动化机器学习

> 原文：[`www.kdnuggets.com/2019/01/automated-machine-learning-python.html`](https://www.kdnuggets.com/2019/01/automated-machine-learning-python.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

![](img/a06801a2d61f74023099372964a67cc4.png)

正如我们已经知道的，机器学习是一种自动化复杂问题解决的方法。但机器学习本身能否被自动化？这就是我们在本文中将要探讨的问题。到文章结束时，我们将回答这个问题，并展示如何实际实现它。

### 自动化机器学习（AutoML）

在应用机器学习模型时，我们通常会进行[数据预处理](https://heartbeat.fritz.ai/data-preprocessing-and-visualization-implications-for-your-machine-learning-model-8dfbaaa51423)、[特征工程](https://heartbeat.fritz.ai/introduction-to-automated-feature-engineering-using-deep-feature-synthesis-dfs-3feb69a7c00b)、[特征提取](https://en.wikipedia.org/wiki/Feature_extraction)和[特征选择](https://en.wikipedia.org/wiki/Feature_selection)。之后，我们会选择最佳算法并[调整我们的参数](https://en.wikipedia.org/wiki/Hyperparameter_optimization)以获得最佳结果。AutoML 是一系列用于自动化这些过程的概念和技术。

**AutoML 的好处**

将机器学习模型应用于我们的实际问题通常需要计算机科学技能、领域专业知识和数学专业知识。找到具备所有这些技能的专家并不总是一件容易的事。

AutoML 还减少了在设计机器学习模型时可能出现的偏差和错误。组织还可以通过在其数据管道中应用 AutoML 来降低雇佣多名专家的成本。AutoML 还减少了开发和测试机器学习模型所需的时间。

**AutoML 的缺点**

AutoML 在机器学习领域是一个相对较新的概念。因此，在应用一些当前的 AutoML 解决方案时，重要的是要谨慎。这是因为这些技术中的一些仍在开发中。

另一个主要挑战是运行 AutoML 模型所需的时间。这将真正取决于我们运行的机器的计算能力。正如我们很快将看到的，有些 AutoML 解决方案在我们的本地机器上运行良好，但有些则需要像[Google Colab](https://www.inc.com/jeff-haden/the-worlds-most-successful-people-dont-actually-start-work-at-4-am-they-wake-work-whenever-heck-they-decide.html)这样的加速解决方案。

### AutoML 概念

关于 AutoML，有两个主要概念需要掌握：神经架构搜索和迁移学习。

**神经架构搜索**

神经架构搜索是自动化设计神经网络的过程。通常， [强化学习](https://en.wikipedia.org/wiki/Reinforcement_learning) 或 [进化算法](https://en.wikipedia.org/wiki/Evolutionary_algorithm) 被用于这些网络的设计。在 [强化学习](https://en.wikipedia.org/wiki/Reinforcement_learning) 中，模型因低准确度而受到惩罚，因高准确度而获得奖励。使用这种技术，模型会始终努力获得更高的准确度。

已经发表了几篇神经架构搜索的论文，例如 [Learning Transferable Architectures for Scalable Image Recognition](https://arxiv.org/abs/1707.07012)、[Efficient Neural Architecture Search (ENAS)](https://arxiv.org/pdf/1802.03268.pdf)**、**和 [Regularized Evolution for Image Classifier Architecture Search](https://arxiv.org/abs/1802.01548) 等。

**转移学习**

转移学习顾名思义是一种技术，其中使用预训练模型来转移在应用模型到新但类似的数据集时学到的内容。这使我们能够在使用较少的计算时间和功率的情况下获得高准确率。神经架构搜索适用于需要发现新架构的问题，而转移学习则最适合数据集与预训练模型所用的数据集类似的问题。

**AutoML 解决方案**

现在让我们来看看一些可用的自动化机器学习解决方案。

***Auto-Keras***

根据 [官方站点](https://autokeras.com/) 的说法：

> Auto-Keras 是一个开源的自动化机器学习（AutoML）软件库。它由 [DATA Lab](http://faculty.cs.tamu.edu/xiahu/index.html) 开发，来自德克萨斯农工大学和社区贡献者。AutoML 的终极目标是为具有有限数据科学或机器学习背景的领域专家提供易于访问的深度学习工具。Auto-Keras 提供了自动搜索深度学习模型的架构和超参数的功能。

可以使用简单的 pip 命令进行安装：

```py
pip install auto-keras
```

Auto-Keras 仍在进行最终测试，尚未发布最终版本。官方站点警告说，他们对因使用站点上的库而产生的任何损失不承担责任。此包基于 [Keras](https://heartbeat.fritz.ai/introduction-to-deep-learning-with-keras-c7c3d14e1527) 深度学习包。

***Auto-Sklearn***

[Auto-Sklearn](https://automl.github.io/auto-sklearn/stable/) 是一个基于 S[cikit-learn](https://scikit-learn.org/) 的自动化机器学习包。它可以替代 Scikit-learn 估计器。其安装也通过一个简单的 pip 命令：

```py
pip install auto-sklearn
```

在 Ubuntu 中，需要 C++11 构建环境和 SWIG：

```py
sudo apt-get install build-essential swig
```

通过 Anaconda 的安装步骤如下：

```py
conda install gxx_linux-64 gcc_linux-64 swig
```

在 Windows 上无法运行 Auto-Sklearn。然而，可以尝试一些技巧，如 Docker 镜像或通过虚拟机运行。

***基于树的管道优化工具 (TPOT)***

根据其官方网站 [site](https://automl.info/tpot/)：

> TPOT 的目标是通过将灵活的 [表达式树](https://en.wikipedia.org/wiki/Binary_expression_tree) 表示与 [遗传编程](https://en.wikipedia.org/wiki/Genetic_programming) 等随机搜索算法结合起来，自动化构建机器学习管道。TPOT 使用基于 Python 的 [scikit-learn](http://scikit-learn.org/) 库作为其机器学习菜单。

该软件是开源的，并可在 [GitHub](https://github.com/EpistasisLab/tpot) 上获取。

***Google 的 AutoML***

其官方网站声明：

> Cloud AutoML 是一套机器学习产品，使有限机器学习专业知识的开发人员能够利用 Google 的先进迁移学习和神经网络结构搜索技术，训练出符合其业务需求的高质量模型。

Google 的 AutoML 解决方案不是开源的。其定价可在 [这里](https://cloud.google.com/pricing/) 查看。

***H20***

[H2O](https://www.h2o.ai/products/h2o/) 是一个开源的分布式内存机器学习平台。它同时提供 R 和 Python 版本。此软件包支持统计学和机器学习算法。

### 将 AutoML 应用于现实世界问题

现在让我们看看如何使用 Auto-Keras 和 Auto-Sklearn 解决实际问题。

**Auto-Keras 实现**

我强烈建议在 [Google Colab](https://colab.research.google.com/) 上运行以下示例，除非我们使用高计算能力的机器。同样，确保我们在 Google Colab 上使用 GPU 运行时也很重要。这里的第一步是安装 Auto-Keras 到我们的 Colab 运行时中。

```py
!pip install autokeras
```

![](img/da06b51df5fff22d7fa251bb92167fc2.png)

我们将对 MNIST 数据集进行图像分类。第一步是导入该数据集和图像分类器。数据集从 Keras 导入，而图像分类器从 Auto-Keras 导入。由于我们正在构建一个基于预训练模型来识别手写数字的模型，因此我们将其归类为监督学习问题。然后，我们在未见过的数字图像上测试模型的准确性。

在这个示例中，图像和标签已经被格式化为 numpy 数组，这在机器学习中是必需的。下一步是将我们刚刚加载的数据分为训练集和测试集，如下所示。

一旦数据被分为训练集和测试集，下一步是拟合图像分类器。

1.  将 `Verbose` 指定为 True 意味着搜索过程会显示在屏幕上供我们查看。

1.  在 fit 方法中，`time_limit` 指的是搜索的时间限制（以秒为单位）。

1.  `final_fit` 是在模型找到最佳架构后进行的最后一次训练。将 `retrain` 指定为 true 意味着模型的权重将被重新初始化。

1.  打印`y`将显示我们在测试集上评估模型后的准确度。

![](img/2f3146b171f2586d4542ac79921d35b5.png)

这就是我们使用 Auto-Keras 进行图像分类所需的一切。代码行数很少，Auto-Keras 会为我们完成所有繁重的工作。

**Auto-Sklearn 实现**

Auto-Sklearn 的实现与上述 Auto-Keras 实现非常相似。我们将使用数字数据集运行类似的分类。首先，我们需要进行一些导入：

1.  `autosklearn.classification`，我们稍后将用来加载分类器

1.  `sklearn.model_selection` 用于模型选择

1.  `sklearn.datasets` 用于加载数据集

1.  `import sklearn.metrics` 用于测量模型性能

像往常一样，我们加载数据集并将其拆分为训练集和测试集。然后，我们从 `autosklearn.classification` 导入 `AutoSklearnClassifier`。完成这些后，我们将分类器拟合到数据集上，进行预测并检查准确度。这就是你需要做的全部。

### 接下来是什么？

自动化机器学习包仍在积极开发中。我们预计 2019 年将在这一领域看到更多进展。可以通过关注官方文档网站来跟踪这些包的进展。也可以通过在 GitHub 上提交拉取请求来为这些包做贡献。有关 [Auto-Keras](https://autokeras.com/) 和 [Auto-Sklearn](https://automl.github.io/auto-sklearn/stable/) 的更多信息和示例可以在其官网上找到。

**在**[**Hacker News**](https://news.ycombinator.com/item?id=18903668)**和**[**Reddit**](https://www.reddit.com/r/learnmachinelearning/comments/afww4m/automated_machine_learning_in_python/)**讨论此帖。**

**个人简介：[Derrick Mwiti](https://derrickmwiti.com/)** 是一位数据分析师、作家和导师。他致力于在每个任务中交付优异的结果，并在 Lapid Leaders Africa 担任导师。

[原文](https://heartbeat.fritz.ai/automated-machine-learning-in-python-5d7ddcf6bb9e)。经授权转载。

**相关：**

+   使用开源工具实现自动化机器学习系统

+   使用 Keras 深度学习介绍

+   在 Google Colab 中使用 Hyperas 调整 Keras 超参数

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT

* * *

### 更多相关话题

+   [使用 Python 进行自动化机器学习：不同方法的比较](https://www.kdnuggets.com/2023/03/automated-machine-learning-python-comparison-different-approaches.html)

+   [使用 Python 进行自动化机器学习：案例研究](https://www.kdnuggets.com/2023/04/automated-machine-learning-python-case-study.html)

+   [使用 Streamlit DIY 自动化机器学习](https://www.kdnuggets.com/2021/11/diy-automated-machine-learning-app.html)

+   [自动化文本摘要入门](https://www.kdnuggets.com/2019/11/getting-started-automated-text-summarization.html)

+   [利用 ChatGPT 进行自动化数据清理和预处理](https://www.kdnuggets.com/2023/08/harnessing-chatgpt-automated-data-cleaning-preprocessing.html)

+   [AI 自动化网络安全：应该自动化哪些部分？](https://www.kdnuggets.com/ai-automated-cybersecurity-what-to-automate)
