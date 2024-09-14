# 一小时入门机器学习！

> 原文：[`www.kdnuggets.com/2017/11/getting-started-machine-learning-one-hour.html`](https://www.kdnuggets.com/2017/11/getting-started-machine-learning-one-hour.html)

**由 [Abhijit Annaldas](https://www.linkedin.com/in/avannaldas/)，微软。**

我在为我的一小时讲座规划议程。传达学习路径、设置环境并解释重要的机器学习概念，在经过深思熟虑后，终于成为了议程的一部分。我最初考虑了各种讲座方式，包括 - 使用线性回归的 Python 实践、详细解释线性回归，或者仅仅分享我过去 18 个月的学习历程。但我想要开始一个能让观众获得大量新信息和问题的讲座，激发他们的好奇心和兴趣。我认为我在这个方面做得还不错。基本上，就是为了让他们入门机器学习。这就是为什么这个指南最终被称为*一小时入门机器学习*的原因。

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

![机器学习](img/842ee46e1c91e7d662a1281cc6534b3c.png)

这次讲座的笔记非常适合初学者，但仅仅是为了帮助我自己准备讲座而编排的。因此，我从中写出了一个机器学习入门指南，现已完成。我非常高兴最终形成的样子，并且很兴奋能与大家分享！

学习机器学习有两种主要的方法：理论机器学习方法和应用机器学习方法。我在之前的 [博客文章](http://abhijitannaldas.com/applied-vs-theoretical-machine-learning.html)中写过关于这方面的内容。

### 理论机器学习

以下是你可以开始学习的主题（按我认为的合适顺序排列）。对于理论性的机器学习学习，下面的主题应当被认真和深入地学习。

1.  线性代数 - [MIT](https://ocw.mit.edu/courses/mathematics/18-06-linear-algebra-spring-2010/)，[印度科学研究所](http://nptel.ac.in/courses/111108066)

1.  微积分 - [基础课程，Coursera](https://www.coursera.org/learn/calculus1)，[高级课程，Coursera](https://www.coursera.org/learn/advanced-calculus)

1.  概率和统计 - [MIT](https://ocw.mit.edu/courses/mathematics/18-05-introduction-to-probability-and-statistics-spring-2014/)

1.  统计学习理论 - [MIT](https://ocw.mit.edu/courses/brain-and-cognitive-sciences/9-520-statistical-learning-theory-and-applications-spring-2003/)，[Stanford](https://lagunita.stanford.edu/courses/HumanitiesSciences/StatLearning/Winter2016/about)

1.  机器学习 - [Coursera](https://www.coursera.org/learn/machine-learning)，[Caltech](https://work.caltech.edu/telecourse)

1.  实现机器学习研究想法的编程语言。

前进的道路可能是阅读研究论文，实施研究工作/新算法，发展专业知识，并在研究路径上进一步选择一个专业化方向。

### 应用机器学习

1.  对上述基础知识（1 到 4）的良好理解。

1.  机器学习（下文解释的重要概念）：[Coursera](https://www.coursera.org/learn/machine-learning)，[Caltech](https://work.caltech.edu/telecourse)

1.  [Python](https://www.datacamp.com/courses/intro-to-python-for-data-science) 或 R 编程语言，根据你的偏好。

1.  学习使用选择的编程语言中的流行机器学习、数据处理和可视化库。我个人使用 Python 编程语言，因此我将在下面详细说明。

1.  必须了解的 Python 库：[numpy](https://www.datacamp.com/community/tutorials/python-numpy-tutorial)，[pandas](https://www.datacamp.com/community/tutorials/pandas-tutorial-dataframe-python)，[scikit-learn](https://www.datacamp.com/community/tutorials/pandas-tutorial-dataframe-python)，[matplotlib](https://www.datacamp.com/community/tutorials/matplotlib-tutorial-python)

1.  其他流行的 Python 库：[LightGBM](https://github.com/Microsoft/LightGBM)，[XGBoost](https://github.com/dmlc/xgboost)，[CatBoost](https://catboost.yandex/)

### 快速开始选项

如果你想了解机器学习是什么以及它可能是什么样的，可以尝试这种方法进行实验，快速上手。如果你想长期认真从事数据科学，这不是一个理想的方法。

1.  了解机器学习概念概述（下文）

1.  学习 Python 或 R

1.  理解并学习使用你选择的语言中的流行库

### Python 环境设置

1.  Python

    +   Python.org [下载](https://www.python.org/downloads/)，[学习](https://www.datacamp.com/courses/intro-to-python-for-data-science) 或

    +   Anaconda [下载](https://www.continuum.io/downloads)，[学习](https://docs.continuum.io/anaconda/)

1.  代码编辑器 / IDE

    +   Visual Studio Code（搜索并安装 Python 扩展，选择下载量最多的一个）

    +   Notepad++

    +   [Jupyter](http://jupyter.readthedocs.io/en/latest/install.html)（与 Anaconda 一起安装）

1.  安装 Python 包

    +   [使用 pip 管理包，Python 的本地工具](https://docs.python.org/3/installing/index.html)：`pip install <package-name>`

    +   [使用 anaconda 管理包](https://conda.io/docs/using/pkgs.html)：`conda install <package-name>`

1.  管理 Python（本地）虚拟环境（如果需要多个环境）

    +   创建虚拟环境：`python -m venv c:\path\to\env\folder`

    +   命令帮助：`python -m venv -h`

    +   切换环境：`activate.bat` 脚本位于虚拟环境文件夹中

    +   Python（本地）虚拟环境 [文档](https://docs.python.org/3/library/venv.html)

1.  管理 Anaconda 虚拟环境（如果需要多个环境）

    +   默认 conda 环境 - `root`

    +   列出可用环境 - `conda env list`

    +   创建新环境 - `conda create --name environment_name`

    +   切换到环境 - `activate environment_name` 或 `source activate environment_name`

    +   Anaconda 虚拟环境 [文档](https://conda.io/docs/using/envs.html)

### 机器学习概念概述

+   **机器学习**：是一种通过函数 *f(x)* 从大量数据集中寻找模式的方法，该函数有效地推广到未见的 *x*，以发现未见数据中的学习模式，并进行机器学习模型训练所需的推断。

+   **数据集**：用于应用机器学习并从中发现模式的数据。对于监督类型的机器学习应用，数据集包含 *x*（输入/属性/独立变量）和 *y*（目标/标签/依赖变量）数据。对于无监督数据，它只是 *x*，输入数据的输出是某种学习到的模式（如聚类、组等）

+   **训练集**：*数据集*的一个子集，提供给（训练）机器学习算法以学习模式

+   **评估/验证/交叉验证集**：*数据集* 的一个子集，不在 *训练集* 中，用于评估机器学习算法的表现。

+   **测试集**：用于预测学习到的见解的 *数据集*。对于监督问题，像 *训练集* 中的目标/标签 *y* 需要预测，因此它不是 *训练集* 的一部分。对于无监督，*训练* 和 *测试* 集可以是相同的。

+   **类型**：

    +   **监督**：在监督问题中，历史数据包括需要为未来/未见数据预测的标签（目标属性、结果）。例如，对于房价预测，我们有房屋（面积、卧室数量、位置等）和价格的数据。在这里，通过用给定的数据（X - 数据）和价格（Y - 标签）训练机器学习模型后，将来将对新的/未见数据（X）预测价格（Y）。

    +   **无监督**：在无监督学习中，没有标签或目标属性。一个典型的例子是根据学习到的模式对数据进行聚类。例如，对于包含房屋详情（面积、位置、价格、卧室数量、楼层数量、建造日期等）的数据集，算法需要找出是否存在隐藏的模式。例如，有些房屋非常昂贵，而其他房屋价格则比较常见。有些房屋非常大，而有些房屋则是普通大小。根据这些模式，记录/数据被聚类为奢侈住宅、非奢侈住宅、平房、公寓等组。

    +   **强化学习**：在强化学习中，一个‘代理’在‘环境’中进行操作，并接收正反馈或负反馈。正反馈告诉代理它做得很好，代理会继续执行类似的计划/动作。负反馈则告诉代理它做错了什么，并应该改变行动路线。代理和环境都是软件/编程实现的。强化学习的核心是构建一个代理（或以某种方式构建代理的行为），使其能够在环境中成功完成特定任务。

+   **流行算法**：[线性回归](https://en.wikipedia.org/wiki/Linear_regression)，[逻辑回归](https://en.wikipedia.org/wiki/Logistic_regression)，[支持向量机](https://en.wikipedia.org/wiki/Support_vector_machine)，[K 最近邻](https://en.wikipedia.org/wiki/K-nearest_neighbors_algorithm)，[决策树](https://en.wikipedia.org/wiki/Decision_tree)，[随机森林](https://en.wikipedia.org/wiki/Random_forest)，[梯度提升](https://en.wikipedia.org/wiki/Gradient_boosting)，[集成学习](https://en.wikipedia.org/wiki/Ensemble_learning)

+   **预处理**：在现实世界中，数据很少以清晰整洁的状态存在，可以直接应用于机器学习算法。预处理是清理数据以供机器学习算法使用的过程。一些常见的预处理步骤包括……

    +   **缺失值**：当某些值缺失时，通常通过添加中位数/均值，删除相应的行，或使用前一行的值等方法处理。这些处理方法有很多，具体需要做什么取决于数据的种类、解决的问题和业务目标。

    +   **分类变量**：离散的有限值集合。例如‘汽车类型’，‘部门’，等。这些值被转换为数字或向量。转换为向量称为[独热编码](https://en.wikipedia.org/wiki/One-hot)。在 Python 中有多种方法可以实现这一点。一些机器学习算法/库本身通过内部编码处理分类列。一种编码方法是使用[sklearn.preprocessing.OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html)。

    +   **缩放**：将列中的值按比例缩放到一个公共范围，例如 0 到 1。将所有列中的值都调整到一个共同范围，可能在一定程度上提高准确性和训练速度。

    +   **文本**：文本需要使用自然语言处理技术（本指南不涉及）进行处理。如果没有预处理，通常会从供机器学习算法训练的数据中排除。

    +   **不平衡数据集**：数据不应该有偏差或偏斜。例如，考虑一个分类任务，其中一个算法将数据分类为 A、B 和 C 三种不同的类别。如果数据集中某一类的记录相对于其他类别非常少/多，则称之为偏倚/不平衡。通常，在这种情况下，通过从现有数据中合成生成更多随机数据来对数据进行过采样。一些机器学习算法/库允许提供权重或某些参数，以在内部平衡数据集，而无需我们进行修正。例如，[SVM：不平衡类别的分离超平面](http://scikit-learn.org/stable/auto_examples/svm/plot_separating_hyperplane_unbalanced.html) 在 scikit-learn 中。

    +   **异常值**：异常值需要根据问题和业务情况逐个处理。

+   **数据转换**：当数据集中的一列/属性没有固有模式时，它会被转换为如 log(值)、sqrt(值) 等形式，其中转换后的值可能会展现出有趣的模式/均匀性。这显然是根据具体情况而定的，需要数据探索来找到合适的转换方式。

+   **特征工程**：特征工程是从现有数据中提取隐藏见解的过程。考虑一个住房价格预测数据集，其中包含“地块宽度”、“地块长度”、“卧室数量”和“价格”列。在这里，我们发现缺少房子的一个关键属性“面积”，但可以根据“地块宽度”和“地块长度”计算得出。因此，数据集中添加了一个计算列“面积”。这就是特征工程。特征工程的难度可能不同，有时一个衍生属性就在眼前，如此处的例子，有时它确实隐藏得很深，需要大量思考。

+   **训练**：这是一个主要步骤，其中机器学习算法在给定数据上进行训练，以寻找可以应用于未见数据的通用模式。以下是这一阶段的一些重要细节……

    +   **特征选择**：并非所有特征/列都对学习有贡献。这些是数据不会影响结果的列。这些特征会从数据集中删除。决定训练哪些特征和排除哪些特征是根据应用的机器学习算法提供的特征重要性来决定的。大多数现代算法都会提供特征重要性。如果算法不提供，scikit-learn 具有 [特征选择能力](http://scikit-learn.org/stable/modules/feature_selection.html) 可以帮助特征选择。同时，相关特征也会被删除。

    +   **降维**：[降维](https://en.wikipedia.org/wiki/Dimensionality_reduction)也旨在找出所有特征中最重要的特征，目的是减少数据的[维度](https://en.wikipedia.org/wiki/Curse_of_dimensionality)。与基于特征重要性的特征选择的主要区别在于，降维中选择的是特征的子集和/或派生特征。换句话说，我们可能无法将提取的特征映射回原始特征。你可以在 scikit-learn 中找到更多关于降维的信息[这里](http://scikit-learn.org/stable/modules/unsupervised_reduction.html#)。

    +   **特征选择与降维**：在我看来，解决目标的两种方法中应该选择其中之一。如果我们既做基于特征重要性的特征选择，又做降维，应该首先进行基于特征重要性的特征选择，然后再引入降维。显而易见，我们应在每一步评估性能，以了解哪些有效，哪些无效。基于特征重要性的特征选择容易解释，因为所选特征是所有特征的子集，而降维则不是这样。

    +   **评估指标**：评估指标是用于评估预测正确性的指标。机器学习算法在训练时使用评估指标来评估、计算成本并在成本凸函数上进行优化。虽然每个算法都有默认的评估指标，但建议根据业务案例/问题指定确切的评估指标。例如，有些问题可以容忍假阳性，但不能容忍假阴性。通过指定评估指标，可以控制模型的这些细节。

    +   **参数调优**：尽管如今大多数最先进的算法都有合理的默认参数值，但调整参数总是有帮助的，可以控制模型的准确性并改善整体预测。参数调优可以通过反复更改和评估准确性来进行尝试和错误。或者，可以提供一组参数值，尝试这些参数的所有/不同排列，以找到最佳参数组合。这可以使用一些称为[scikit-learn 中的超参数优化器](http://scikit-learn.org/stable/modules/classes.html#hyper-parameter-optimizers)的辅助函数来完成。

    +   **过拟合（偏差）**：过拟合是指机器学习模型几乎记住了所有的训练数据，并且在训练集中的数据上预测几乎非常准确。这是一种模型无法泛化并预测未见过的数据的状态。这也被称为模型具有高偏差。过拟合可以通过使用正则化、调整不适当配置的超参数、保留部分数据集以使用正确的交叉验证策略来处理。

    +   **欠拟合（方差）**：欠拟合是指机器学习模型即使在对训练数据集中的数据进行预测时，预测效果也不好。这也被称为模型具有高方差。可以通过添加更多数据、增加/删除特征、尝试不同的机器学习算法等方式来处理欠拟合。

    +   **偏差与方差权衡（甜点）**：模型训练的目标是找到一个甜点，使模型的交叉验证误差最小。最初，交叉验证误差和训练误差都很高（欠拟合/高方差）。随着模型的训练，误差会下降到一个点，在该点交叉验证误差最小且接近训练误差（甜点）。这是最优点。在这个点之后，如果模型继续降低误差（在训练集上），它几乎会记住训练集，最终导致过拟合，这意味着在未见过的数据上误差较高。

    +   **正则化**：当模型尝试进一步学习（降低误差，趋向过拟合）时，正则化有助于抵消过拟合的效果。正则化通常是一个在成本/误差计算期间添加的参数。机器学习算法可能不会总是明确提供正则化参数。在这种情况下，通常有其他参数可以调整，以引入所需的正则化程度。

+   **预测**：要使用训练好的机器学习模型进行预测，需要调用模型的预测方法，并将测试数据集作为参数传递。测试数据集应该按照对训练数据集进行的预处理方式进行处理。换句话说，格式应与喂给机器学习模型进行训练的训练数据相同。

+   **其他术语**：

    +   **模型堆叠**：当单一的机器学习算法效果不好时，会使用多个机器学习算法进行预测，并以不同的方式将这些预测结果组合在一起。最简单的方式是加权预测。有时，会在第一层模型的预测结果上使用其他机器学习模型（元模型）。这可以复杂到任何程度，并且可以有不同的流程。

### 深度学习

有趣的是，今天解决的大多数（我猜超过 90%）机器学习问题都是通过仅使用随机森林、梯度提升决策树、SVM、KNN、线性回归、逻辑回归来解决的。

但是，有一些问题无法通过上述技术解决。像图像分类、图像识别、自然语言处理、音频处理等问题使用了一种叫做深度学习的技术。开始深度学习之前，我相信掌握上述所有概念是至关重要的。

优质的深度学习资源…

+   [Fast.ai](http://www.fast.ai/) – 感谢[Pranay Tiwari](https://www.linkedin.com/in/pranay-tiwari-47a49048/)的建议！

+   [neuralnetworksanddeeplearning.com - 在线书籍，重点介绍理论和基础知识](http://neuralnetworksanddeeplearning.com/)

+   [Coursera 上的深度学习专项课程，由 Andrew Ng 提供](https://www.coursera.org/specializations/deep-learning)

+   [deeplearningbook.org - 在线书籍](http://www.deeplearningbook.org/)

如果你了解深度学习概念并希望动手实践，一些流行的深度学习库包括：[Keras](https://keras.io/)、[CNTK](https://github.com/Microsoft/CNTK)、[Tensorflow](https://github.com/tensorflow/tensorflow)、[tflearn](https://github.com/tflearn/tflearn)、[sonnet](https://github.com/deepmind/sonnet)、[pytorch](https://github.com/pytorch/pytorch)、[caffe](https://github.com/BVLC/caffe)、[Theano](https://github.com/Theano/Theano)

### 练习

是的，实践是最重要的，如果不提到练习机器学习，这个指南将是不完整的。为了进一步练习和掌握你的技能，以下是你可以做的事情…

1.  从各种在线数据源获取数据集。其中一个受欢迎的数据源是 [UCI 机器学习库](http://abhijitannaldas.com/getting-started-with-machine-learning-in-one-hour/archive.ics.uci.edu/ml/)。此外，你还可以 [搜索“机器学习数据集”](https://google.com/search?q=datasets+for+machine+learning)。

1.  参加在线机器学习/数据科学黑客马拉松。一些受欢迎的赛事包括 - [Kaggle](https://kaggle.com/)、[HackerEarth](http://www.hackerearth.com/)，等等。如果你开始时遇到非常困难的任务，尝试坚持一会儿。如果仍然感觉困难，可以先放一放，寻找其他任务。不必灰心。通常在线黑客马拉松中的问题具有一定的难度，这可能不适合初学者。

1.  写博客记录你学到的东西！这将帮助你巩固对该主题的理解和思考。

1.  在 Quora 上关注数据科学、机器学习相关话题，有很多很棒的建议以及问题/答案可以学习。

1.  开始听播客（可通过以下链接获取）

1.  查看我 [数据科学学习资源](http://abhijitannaldas.com/useful-stuff/) 页面上的一些有用链接。

### 结束语

如果你认真考虑机器学习/数据科学领域，并且考虑转行，请思考一下你的动机以及你为什么想做这个。

如果你确定，我有一个建议给你。***绝不要放弃或怀疑这一切是否值得***。这绝对是值得的，我可以这样说，因为我在过去 18 个月里几乎每天、每个周末以及每一个空闲时间都走在这条路上（除了旅行或被日常工作压得喘不过气来）。掌握数据科学的道路并不容易。正如他们所说，*“罗马不是一天建成的！”*。你需要学习很多科目。不同的学习优先级之间需要权衡。即使学到了很多，你仍会发现一些你之前从未想到/听说过的新事物。你不断发现的新概念/技术可能会让你觉得自己仍然知道的很少，还有很多内容需要掌握。这很正常。坚持下去。设定大目标，规划小任务，并专注于手头的任务。如果有新的东西出现，记下它，稍后再处理。

### 谢谢你！

如果你一直阅读到这里，我感谢你的努力和时间。我希望这份指南对你有用，并使你开始自己的学习冒险变得稍微容易一些。如果在未来某个时候，你认为这份指南对你的学习冒险有所帮助，请回来在这里留下评论。或者通过 avannaldas .at. hotmail .dot. com 联系我。我很乐意听到你的反馈。知道这对你有帮助，并且我为此付出的努力是值得的，会让我感到极大的满足。

这是我写的最大的一篇文章。我花了很多小时来写作、编辑和审阅。如果你发现任何错误或可以改进的地方，请在评论中或通过电子邮件告诉我。我会尽快修正，并将其归功于你。这将帮助每一个阅读此文的人。

再次感谢！

祝一切顺利！

**个人简介：[Abhijit Annaldas](https://www.linkedin.com/in/avannaldas/)** 是一名软件工程师，同时也是一位饱含热情的学习者，他在机器学习方面拥有相当的知识和专长。他通过不断学习新事物和不懈的实践，每天都在提升自己的专业技能，自 2012 年 6 月以来，作为微软印度的软件工程师，他在不同的微软和开源技术中积累了丰富的企业级应用开发经验。

[原文](http://abhijitannaldas.com/getting-started-with-machine-learning-in-one-hour/)。经许可转载。

**相关：**

+   如何在 10 天内学习机器学习

+   Python 机器学习游击指南

+   基于密度的空间聚类与噪声（DBSCAN）

### 更多相关主题

+   [云存储的普及是商业的当务之急](https://www.kdnuggets.com/2022/02/cloud-storage-adoption-need-hour-business.html)

+   [Scikit-learn 入门：机器学习中的分类](https://www.kdnuggets.com/getting-started-with-scikit-learn-for-classification-in-machine-learning)

+   [突破数据障碍：零样本、单样本和少样本学习如何变革机器学习](https://www.kdnuggets.com/2023/08/breaking-data-barrier-zeroshot-oneshot-fewshot-learning-transforming-machine-learning.html)

+   [机器学习与大脑的不同 第一次：神经元反应缓慢](https://www.kdnuggets.com/2022/04/machine-learning-like-brain-part-one-neurons-slow-slow-slow.html)

+   [SQL 入门备忘单](https://www.kdnuggets.com/2022/08/getting-started-sql-cheatsheet.html)

+   [spaCy 入门指南：自然语言处理](https://www.kdnuggets.com/2022/11/getting-started-spacy-nlp.html)
