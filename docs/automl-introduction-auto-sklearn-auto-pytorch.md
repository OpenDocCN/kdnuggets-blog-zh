# AutoML：使用 Auto-Sklearn 和 Auto-PyTorch 的介绍

> 原文：[https://www.kdnuggets.com/2021/10/automl-introduction-auto-sklearn-auto-pytorch.html](https://www.kdnuggets.com/2021/10/automl-introduction-auto-sklearn-auto-pytorch.html)

[评论](#comments)

![autoML.png](../Images/62f872662eee3e99a9dd99ba62bc7d95.png)

### 让计算机自动完成你想要的任务

[机器学习 (ML)](https://www.exxactcorp.com/blog/Deep-Learning/a-breakdown-of-deep-learning-frameworks)现在影响着广泛的商业、工程和研究领域，以至于很难找到一个完全未涉及机器学习的领域。机器学习的进展伴随着软件和自动化的广泛趋势：只要人类活动依赖于可以以计算机可以处理的方式描述的重复任务，通常编写一个计算机可以遵循的配方（或程序）是很有用的。

现在使用机器学习意味着对于许多有用的任务，已经不再需要手动编写程序，甚至不需要准确知道如何解决问题。相反，我们可以通过定义一个搜索空间和一个学习算法来解决许多问题，然后让机器来解决。

现代机器学习有时被称为“软件 2.0”，这一趋势受到深度学习效果以及随之而来的研究和开发兴趣的推动。显而易见，这种方法非常适合一些应用：例如，拟合统计模型到数据上；但也有一些更深奥和令人印象深刻的例子，在统计学或旧式机器学习时代并不明显。

在过去几年中，我们看到机器学习比其他任何方法更好地预测蛋白质折叠，在围棋、Dota II、星际争霸 II 等游戏中击败了顶级人类玩家，并创造出相对连贯的文本和语音回应（尽管最后的成就有时可能会有偏差）。

尽管如此，这些项目几乎总是需要大量世界级工程和研究人才的应用。这并不完全令人惊讶，因为即使是配备了先进最尖端机器学习算法的计算机程序，要实现全新的目标仍然需要人类的创新。这种情况可能会在未来发生变化，当人工智能研究人员创造出突破人类专家级别的人工智能研究者门槛的新机器学习代理时。

目前，尽管突破性的人工智能科学仍然难以实现自动化，但有越来越多的机器学习应用场景中，*不一定需要人工工程师来优化模型*以完成特定任务。实际上，在某些任务中，将选择具体模型和调整学习超参数的任务交给人工判断，实际上可能会拖慢进度或导致结果不佳。人类可能在探索超参数空间时表现不佳，可能因为错误的原因偏向自己喜欢的模型类型，或者可能比实际训练模型所需的更频繁地开始和停止训练（这对他们的心理状态也不好）。

相反，一个好的机器学习从业者应该充分利用所有可用的工具，现在这些工具包括开源现成工具和应用机器学习的最佳实践。换句话说，**AutoML**。

![automl.png](../Images/928bab5e5c050b1c0d88ea047058bf05.png)

### 什么是AutoML？

AutoML是一个广泛的技术和工具类别，用于将自动化搜索应用于您的自动化搜索，并将学习应用于您的学习。这些技术从对统计学习算法的超参数应用贝叶斯优化，到对深度学习模型进行神经网络结构搜索。

这个领域非常活跃且多样化，拥有健康的竞赛生态系统，其中许多竞赛都在[automl.ai](https://automl.ai/competitions/)上进行了记录。事实上，最著名的AutoML包之一，[Auto-SciKit-Learn](https://github.com/automl/auto-sklearn)（Auto-Sklearn），起初是2014年至2016年ChaLearn AutoML挑战赛的[获胜者](/2016/08/winning-automl-challenge-auto-sklearn.html)。

Auto-Sklearn由德国著名的[AutoML超级组织](https://www.automl.org/automl/)中追求自动化机器学习的最著名研究小组之一开发。这个[合作组织](https://www.automl.org/)由弗赖堡大学和汉诺威大学的实验室组成。其他在这一领域有显著贡献的研究者包括[Auto-WEKA](https://www.automl.org/automl/autoweka/)背后的科学家，Auto-WEKA是最早的流行AutoML工具包之一，以及其继任者Auto-WEKA 2.0。这些研究人员主要分布在北美，但以加拿大的不列颠哥伦比亚大学为中心。而Auto-WEKA与开源WEKA软件和Java一起工作，Auto-Sklearn是一个Python包，并且旨在紧密遵循SciKit-Learn的使用模式（因此得名“Auto-SciKit-Learn”）。

除了Auto-Sklearn，弗赖堡-汉诺威AutoML小组还开发了一个[Auto-PyTorch](https://github.com/automl/Auto-PyTorch)库。我们将在接下来的简单教程中使用这两个库作为我们进入AutoML的起点。

### AutoML教程演示

首先，我们将设置所需的包和依赖项。我们使用 Python3 的 virtualenv 来管理项目的虚拟环境，但如果你更喜欢 Anaconda（特别是如果你在 Anaconda 中使用 pip），你会发现类似的说明应该也能奏效。

以下是从基于 Unix 的系统（如 Ubuntu）或类似 Anaconda 提示符的 Windows 系统的命令行设置环境的命令。Auto-Sklearn 文档建议首先从其 requirements.txt 依赖文件安装，但我们发现对于本教程中使用的代码没有必要这样做。

```py
# create and activate a new virtual environment
virtualenv automl --python=python3
source automl/bin/activate
# install auto-sklearn
pip install auto-sklearn
```

如果你在同一环境中同时使用 AutoML 库，可能会遇到冲突，因此请为 Auto-PyTorch 创建第二个环境。请注意，此环境需要使用版本大于或等于 3.7 的 Python。

```py
deactivate
virtualenv autopt –-python=python3.7
source autopt/bin/activate
# install auto-pytorch from the github repo
git clone https://github.com/automl/Auto-PyTorch.git
cd Auto-PyTorch
pip install -e .
pip install numpy==1.20.0
pip install ipython
```

请注意，在 `pip install -e .` 后还有额外的两个安装语句。在我们的操作中，将 NumPy 版本升级到 1.20.0 解决了一个奇怪的错误，下面重现了该错误。

```py
ValueError: numpy.ndarray size changed, may indicate binary incompatibility. Expected 88 from C header, got 80 from PyObject
```

如果你想为项目做贡献，或只是想查看最新的进行中的代码，请查看开发分支。

```py
# (optional)
git checkout development
# make sure to switch back to the primary branch for the tutorial
git checkout master
```

本教程的其余代码是 Python 代码，因此请启动你的 Python 提示符、Jupyter notebook 或文本编辑器。

本教程将包含使用标准 SciKit-Learn、Auto-Sklearn 和 Auto-PyTorch 分类器进行分类的最少演示。我们将为每种情况使用 SciKit-Learn 提供的内置数据集，每个演示都共享一些代码以导入公共依赖项、加载和拆分数据集。

```py
import time
import sklearn
import sklearn.datasets

#** load and split data **
data, target = sklearn.datasets.load_iris(return_X_y=True)

# split
n = int(data.shape[0] * 0.8)

train_x = data[:n]
train_y = target[:n]
test_x = data[n:]
test_y = target[n:]
```

上述设置数据集的代码将用于本教程中的每个演示。

我们使用了小型的“iris”数据集（150 个样本，4 个特征和 3 个标签类别），以节省时间，但在完成示例后，你可能会想尝试更复杂的数据集。

其他可以尝试的 sklearn.datasets 中的分类数据集包括糖尿病 (**load_diabetes**) 数据集和手写数字数据集 (**load_digits**)。糖尿病数据集有 569 个样本，每个样本有 30 个特征和 2 个标签类别，而手写数字数据集有 1797 个样本，每个样本有 64 个特征（对应 8x8 的图像）和 10 个标签类别。

在开始使用 sklearn 的 AutoML 分类器之前，让我们先使用 vanilla sklearn 的默认设置训练几个标准分类器。可以选择很多，但我们将坚持使用 k-最近邻分类器、支持向量机分类器和多层感知器。

```py
# import classifiers
from sklearn.svm import SVC
from sklearn.neural_network import MLPClassifier
from sklearn.neighbors import KNeighborsClassifier

# instantiate with default parameters
knn = KNeighborsClassifier()
mlp = MLPClassifier()
svm = SVC()
```

SciKit-Learn 使用友好的 fit/predict API，使得训练模型变得非常简单，Auto-Sklearn 和 Auto-PyTorch 也保留了相同的 API。这是易用性的一个重要因素，因为在这三个包中训练模型的体验非常相似。

```py
t0 = time.time()
knn.fit(train_x, train_y)
mlp.fit(train_x, train_y)
svm.fit(train_x, train_y)
t1 = time.time()
```

同样，评估你的模型也很简单。SciKit-Learn 分类模型有一个 predict 方法，该方法接收输入数据并预测标签，然后可以将结果传递给 **sklearn.metrics.accuracy_score** 以计算准确率。

以下代码可以用来计算预测和在保留的测试集上的预测准确率，使用的是在上一段代码中训练的k最近邻、支持向量机和多层感知器分类器。

```py
knn_predict = knn.predict(test_x)
train_knn_predict = knn.predict(train_x)

svm_predict = svm.predict(test_x)
train_svm_predict = svm.predict(train_x)

mlp_predict = mlp.predict(test_x)
train_mlp_predict = mlp.predict(train_x)

knn_accuracy = sklearn.metrics.accuracy_score(test_y, knn_predict)
train_knn_accuracy = sklearn.metrics.accuracy_score(train_y,train_knn_predict)

svm_accuracy = sklearn.metrics.accuracy_score(test_y, svm_predict)
train_svm_accuracy = sklearn.metrics.accuracy_score(train_y,train_svm_predict)

mlp_accuracy = sklearn.metrics.accuracy_score(test_y, mlp_predict)
train_mlp_accuracy = sklearn.metrics.accuracy_score(train_y,train_mlp_predict)

print(f"svm, knn, mlp test accuracy: {svm_accuracy:.4f}," \
           f"{knn_accuracy:.4}, {mlp_accuracy:.4}")
print(f"svm, knn, mlp train accuracy: {train_svm_accuracy:.4f}," \
           f"{train_knn_accuracy:.4}, {train_mlp_accuracy:.4}")
print(f"time to fit: {t1-t0}")
```

![图示](../Images/49b5f26a303ba61bd253033cfca7c7d9.png)

Sklearn 分类器在鸢尾花数据集上的图像

这些模型在鸢尾花训练数据集上相当有效，但训练集和测试集之间存在显著差距。

接下来，让我们使用这个类

```py
AutoSKlearnClassifier 
```

来源于

```py
autosklearn.classification 
```

使用这个基本 AutoML 类的代码看起来和上面示例中训练单个模型的代码完全相同，但实际上它会对多种机器学习模型进行超参数搜索，并保留最佳模型作为集成。

在引入常用库并设置好训练和测试数据集分割后，我们需要导入并实例化 AutoML 分类器。

```py
import autosklearn
from autosklearn.classification import AutoSklearnClassifier as ASC

classifier = ASC()
classifier.time_left_for_this_task = 300

t0 = time.time()
classifier.fit(train_x, train_y)
t1 = time.time()

autosk_predict = classifier.predict(test_x)
train_autosk_predict = classifier.predict(train_x)

autosk_accuracy = sklearn.metrics.accuracy_score( \
           test_y, autosk_predict \
           )
train_autosk_accuracy = sklearn.metrics.accuracy_score( \
           Train_y,train_autosk_predict \
           )

print(f"test accuracy {autosk_2_accuracy:.4f}")
print(f"train accuracy {train_autosk_2_accuracy:.4f}")
print(f"time to fit: {t1-t0}")
```

![图示](../Images/f43d91f167d3b49fd419adad11f63c6f.png)

Auto-Sklearn 分类器集成在鸢尾花数据集上的图示

如果你不重置`time_left_for_this_task`，使用**AutoSklearnClassifier**运行`fit`方法将会花费大量时间，因为默认值是3600秒（一小时）。对于我们简单的鸢尾花数据集来说，这是过度的。从包文档来看，时间限制应该可以作为初始化分类器对象时的输入参数设置，但在我们的经验（版本 0.13.0）中并非如此。

你还可以尝试启用交叉验证来运行 `fit` 方法，如果选择这样做，你需要再次使用该方法进行训练。

```py
refit
```

用最佳模型和超参数训练整个训练数据集。我们发现，当使用交叉验证和重训练相比于默认设置时，测试集准确率从80%提高到了86.67%。

请记住，当你在使用`predict`方法对**AutoSklearnClassifier**对象进行推断时，你实际上是在利用在 AutoML 超参数搜索过程中找到的最佳模型的集成。

| **方法** | **训练准确率** | **测试准确率** | **运行时间** |
| --- | --- | --- | --- |
| 默认 KNN | 0.9833 | 0.8000 | **0.6 ms 总计** |
| 默认 SVM | 0.9667 | 0.7000 | **0.6 ms 总计** |
| 默认 MLP | 0.9833 | 0.7667 | **0.6 ms 总计** |
| Auto-Sklearn | 1.000 | 0.8000 | 291.390 s |
| Auto-Sklearn (含 cv + refit) | 1.000 | 0.8667 | 918.658 s |
| Auto-PyTorch | **0.9917** | **0.9667** | 302.236 s |

最后，让我们尝试一个适合深度学习爱好者的 AutoML 包。

与 Auto-Sklearn 一样，Auto-PyTorch 也旨在极其简单易用。要运行下一段代码，请记得切换到你的 Auto-PyTorch 环境，以确保正确的依赖项可用。在导入常用库并拆分数据之后：

```py
import autoPyTorch
from autoPyTorch import AutoNetClassification as ANC

model = ANC(max_runtime=300, min_budget=30, max_budget=90, cuda=False)

t0 = time.time()
model.fit(train_x, train_y, validation_split=0.1)
t1 = time.time()

auto_predict = model.predict(test_x)
train_auto_predict = model.predict(train_x)

auto_accuracy = sklearn.metrics.accuracy_score(test_y, auto_predict)
train_auto_accuracy = sklearn.metrics.accuracy_score(train_y, train_auto_predict)

print(f"auto-pytorch test accuracy {auto_accuracy:.4}")
print(f"auto-pytorch train accuracy {train_auto_accuracy:.4}")
```

![图示](../Images/bb6f6cfaecf6d13a62ae45c38c3558ac.png)

Auto-PyTorch 分类器在鸢尾花数据集上的图示

正如你在结果表中看到的，Auto-PyTorch在拟合鸢尾花数据集时非常高效和有效，训练和测试准确率达到了90多%。这比我们之前训练的自动SciKit-Learn分类器稍微好一点，比使用默认参数的标准sklearn分类器要好得多。

### AutoML会取代数据科学家吗？

不，未必。AutoML承诺改善典型数据科学和机器学习工作流的实用性、性能和效率。额外的抽象层和自动化的最佳实践超参数搜索，如果使用得当，确实可以带来显著的不同。对于我们在今天教程中实验的这些软件包，我们将其准备程度描述为正在进行的研究原型。

我们需要进行许多小修小补才能使一切正常工作，例如将NumPy升级到1.20.0以修复模糊的错误消息，无法将运行时间限制设置为输入参数（如Auto-Sklearn文档所建议），以及由于一些模糊的冲突而无法使用单个虚拟环境同时支持两个软件包。此外，AutoML的价值在于自动化超参数搜索，但自动分类器本身有许多参数，这些参数对最终结果有很大影响，这也让人忍俊不禁。

尽管如此，我们认为AutoML是任何机器学习或数据科学从业者工具箱中有价值的补充，无论他们使用Auto-Sklearn/Auto-PyTorch、Auto-WEKA、其他软件包，还是自行开发解决方案。当AutoML工具适合时，它不仅能提升项目的性能，还能减少经济和能源（包括环境）成本，从而避免长时间的超参数和架构搜索。

即使一些工具仍有许多粗糙的地方，这也只是一个很好的理由和动力，让你自己参与到这些项目中。Auto-PyTorch在Apache 2.0许可证下提供，而Auto-Sklearn使用BSD 3-Clause许可证。

**简介：[Kevin Vu](https://www.kdnuggets.com/author/kevin-vu)** 负责管理Exxact Corp博客，并与许多才华横溢的作者合作，他们撰写有关深度学习不同方面的文章。

[原文](https://www.exxactcorp.com/blog/Deep-Learning/automl-an-introduction-using-auto-sklearn-and-auto-pytorch)。经授权转载。

**相关内容：**

+   [自动化机器学习简介](/2021/09/introduction-automated-machine-learning.html)

+   [如何创建AutoML管道优化沙盒](/2021/09/automl-pipeline-optimization-sandbox.html)

+   [使用TPOT进行机器学习管道优化](/2021/05/machine-learning-pipeline-optimization-tpot.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 需求

* * *

### 更多相关内容

+   [使用 AutoXGB 的无脑 AutoML](https://www.kdnuggets.com/2022/02/no-brainer-automl-autoxgb.html)

+   [Nota AI 发布 NetPresso 模型搜索的测试版，他们的…](https://www.kdnuggets.com/2022/04/nota-ai-releases-beta-version-netpresso-model-search-hardwareaware-automl-tool.html)

+   [2023 年你应该考虑的顶级 AutoML 框架](https://www.kdnuggets.com/2023/05/best-automl-frameworks-2023.html)

+   [使用 Matplotlib 进行数据可视化简介](https://www.kdnuggets.com/2022/12/introduction-data-visualization-matplotlib.html)

+   [Numpy 和 Pandas 简介](https://www.kdnuggets.com/introduction-to-numpy-and-pandas)

+   [深度学习库简介：PyTorch 和 Lightning AI](https://www.kdnuggets.com/introduction-to-deep-learning-libraries-pytorch-and-lightning-ai)
