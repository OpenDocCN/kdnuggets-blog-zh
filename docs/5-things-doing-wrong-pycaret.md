# 你在 PyCaret 中做错的 5 件事

> 原文：[https://www.kdnuggets.com/2020/11/5-things-doing-wrong-pycaret.html](https://www.kdnuggets.com/2020/11/5-things-doing-wrong-pycaret.html)

[评论](#comments)

**由 [Moez Ali](https://www.linkedin.com/in/profile-moez/)，PyCaret 的创始人及作者**

![图](../Images/6fd3bbe1eed3087171efe5233e63e86b.png)

照片由 [Ben White](https://unsplash.com/@benwhitephotography?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

* * *

## 我们的前三课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

### PyCaret

PyCaret 是一个开源的低代码机器学习库，能够自动化机器学习工作流程。它是一个端到端的机器学习和模型管理工具，加速机器学习实验周期，提高生产力。

与其他开源机器学习库相比，PyCaret 是一个低代码的替代库，可以用几行代码替代数百行代码。这使得实验变得指数级地快速和高效。

官方网站: [https://www.pycaret.org](https://www.pycaret.org/)

文档: [https://pycaret.readthedocs.io/en/latest/](https://pycaret.readthedocs.io/en/latest/)

Git: [https://www.github.com/pycaret/pycaret](https://www.github.com/pycaret/pycaret)

### ???? compare_models 比你想象的要做得更多

当我们在 2020 年 4 月发布 PyCaret 1.0 版本时，**compare_models** 函数会比较库中的所有模型，并返回平均交叉验证性能指标。基于此，你可以使用**create_model**来训练表现最佳的模型，并获取可以用于预测的训练模型输出。

这个行为在版本 2.0 中有所改变。**compare_models** 现在返回基于**n_select**参数的最佳模型，该参数默认设置为 1，这意味着它会返回最佳模型（默认情况下）。

![图](../Images/2f64f64a52336071f10bd0aa449bc957.png)

compare_models(n_select = 1)

通过将默认的**n_select**参数更改为 3，你可以获得前 3 个模型的列表。例如：

![图](../Images/1e8aa7356f9bd812c66f347418a27077.png)

compare_models(n_select = 3)

返回的对象是训练好的模型，你实际上不需要再次调用**create_model**来训练它们。你可以使用这些模型来生成诊断图或进行预测，例如：

![图像](../Images/561308d029c10707f2b8796d963a5e44.png)

**predict_model**函数

### ???? 你认为你只能使用scikit-learn模型

我们收到很多请求，要求在模型库中包括非*scikit-learn*模型。许多人没有意识到，你不仅限于默认模型。**create_model**函数除了接受模型库中模型的ID外，还接受未训练的模型对象。只要你的对象与*scikit-learn* fit/predict API兼容，它就会正常工作。例如，在这里我们通过简单地导入未训练的NGBClassifier，训练并评估了来自[ngboost](https://github.com/stanfordmlgroup/ngboost)库的***NGBClassifier***：

![图像](../Images/f7e2bf067b1dd47cf059ecbc8a29f37f.png)

使用外部模型的create_model

你也可以将未训练的模型传递给**compare_models**的**include**参数，它将正常工作。

![图像](../Images/dd119e114d11c91320af98c831c196f2.png)

使用未训练对象的compare_models

请注意，包括的参数包括模型库中三个未训练的模型的ID，即逻辑回归、决策树和K邻近，以及ngboost库中的一个未训练对象。此外，请注意索引表示在include参数中输入的模型的位置。

### ???? 你不了解pull()函数

PyCaret中的所有训练函数（create_model、tune_model、ensemble_model等）显示一个分数网格，但不返回分数网格。因此，你无法将分数网格存储在像pandas.DataFrame这样的对象中。然而，有一个叫做**pull**的函数可以实现。例如：

![图像](../Images/8f33d9987a20793c5236c7ae391a2a72.png)

使用create_model的pull函数

这也适用于使用**predict_model**函数的holdout分数网格。

![图像](../Images/f976d7fe434f039d8400949177f29d1b.png)

使用**predict_model**函数拉取

现在你可以将指标作为pandas.DataFrame访问，你可以做很多事情。例如，你可以创建一个循环来训练不同参数的模型，并用这段简单的代码创建比较表：

![图像](../Images/d4cf0da30cc625e0751b2652c1326950.png)

create_model和pull函数

### ???? 你认为PyCaret是一个黑箱，但它不是。

另一个常见的误解是所有的预处理操作都在后台进行，用户无法访问。因此，你无法审计运行**setup**函数时发生了什么。这是不对的。

PyCaret中有两个函数**get_config**和**set_config**，它们允许你访问和更改后台的所有内容，从你的训练集到模型的随机状态。你可以通过调用**help(get_config)**来查看可以访问哪些变量：

![图像](../Images/0d548afe8b03e65615731b5a1f30fcb3.png)

help(get_config)

你可以通过在**get_config**函数内部调用变量来访问它。例如，要访问**X_train**转化后的数据集，你可以这样写：

![图示](../Images/7ed5d7408642944630557ab823afdb22.png)

get_config(‘X_train’)

你可以使用**set_config**函数来更改环境变量。根据你目前对**pull、get_config**和**set_config**函数的了解，你可以创建一些非常复杂的工作流。例如，你可以对保留集进行***N次***重采样，以评估平均性能指标，而不是依赖于一个保留集：

```py
import numpy as npXtest = get_config('X_test')
ytest = get_config('y_test')AUC = []for i in np.random.randint(0,1000,size=10):
    Xtest_sampled = Xtest.sample(n = 100, random_state = i)
    ytest_sampled = ytest[Xtest_sampled.index]
    set_config('X_test', Xtest_sampled)
    set_config('y_test', ytest_sampled)
    predict_model(dt);
    AUC.append(pull()['AUC'][0])>>> print(AUC)**[Output]:** [0.8182, 0.7483, 0.7812, 0.7887, 0.7799, 0.7967, 0.7812, 0.7209, 0.7958, 0.7404]>>> print(np.array(AUC).mean())**[Output]: 0.77513**
```

### 你还没有记录你的实验

如果你没有记录你的实验，现在就应该开始记录。无论你是否打算使用MLFlow后台服务器，你都应该记录所有的实验。当你进行任何实验时，你会生成大量的元数据，这些数据很难手动追踪。

PyCaret的日志记录功能在你使用**get_logs**函数时会生成一个漂亮、轻量、易于理解的Excel电子表格。例如：

```py
**# loading dataset**
from pycaret.datasets import get_data
data = get_data('juice')**# initializing setup**
from pycaret.classification import *s = setup(data, target = 'Purchase', silent = True, log_experiment = True, experiment_name = 'juice1')**# compare baseline models**
best = compare_models()**# generate logs**
get_logs()
```

![图示](../Images/7f9e5823e3551fa6be2cd756b1b4f324.png)

get_logs()

在这个非常简短的实验中，我们生成了3000多个元数据点（指标、超参数、运行时间等）。想象一下你将如何手动跟踪这些数据点？也许，这在实际操作中是不可能的。幸运的是，PyCaret提供了一种简单的方法来实现。只需在**setup**函数中将**log_experiment**设置为True即可。

使用Python中的轻量级工作流自动化库，你可以实现无限的可能性。如果你觉得这些内容有用，请不要忘记在我们的[GitHub 仓库](https://www.github.com/pycaret/pycaret/)上给我们⭐️。

想了解更多关于PyCaret的内容，请关注我们的[LinkedIn](https://www.linkedin.com/company/pycaret/)和[Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI_B3g)。

要了解更多关于PyCaret 2.2的更新，请查看[发行说明](https://github.com/pycaret/pycaret/releases)或阅读此[公告](https://towardsdatascience.com/pycaret-2-2-is-here-whats-new-ad7612ca63b)。

### 重要链接

[用户指南](https://www.pycaret.org/guide)

[文档](https://pycaret.readthedocs.io/en/latest/)

[官方教程](https://github.com/pycaret/pycaret/tree/master/tutorials) [示例笔记本](https://github.com/pycaret/pycaret/tree/master/examples)

[其他资源](https://github.com/pycaret/pycaret/tree/master/resources)

### 想了解某个特定模块吗？

点击下面的链接查看文档和示例。

[分类](https://pycaret.readthedocs.io/en/latest/api/classification.html)

[回归](https://pycaret.readthedocs.io/en/latest/api/regression.html)

[聚类](https://pycaret.readthedocs.io/en/latest/api/clustering.html)

[异常检测](https://pycaret.readthedocs.io/en/latest/api/anomaly.html)

[自然语言处理](https://pycaret.readthedocs.io/en/latest/api/nlp.html) [关联规则挖掘](https://pycaret.readthedocs.io/en/latest/api/arules.html)

**简介： [Moez Ali](https://www.linkedin.com/in/profile-moez/)** 是一名数据科学家，也是 PyCaret 的创始人和作者。

[原文](https://towardsdatascience.com/5-things-you-are-doing-wrong-in-pycaret-e01981575d2a)。经许可转载。

**相关内容：**

+   [你对 PyCaret 不了解的 5 件事](/2020/07/5-things-pycaret.html)

+   [使用 Docker 容器将机器学习管道部署到云端](/2020/06/deploy-machine-learning-pipeline-cloud-docker.html)

+   [GitHub 是你所需的最佳 AutoML 工具](/2020/08/github-best-automl-ever-need.html)

### 了解更多相关内容

+   [你不知道的低代码工具的 7 种用法](https://www.kdnuggets.com/2022/09/7-things-didnt-know-could-low-code-tool.html)

+   [Phi-2：小型语言模型的大作为](https://www.kdnuggets.com/phi-2-small-lms-that-are-doing-big-things)

+   [使用 PyCaret 进行二分类介绍](https://www.kdnuggets.com/2021/12/introduction-binary-classification-pycaret.html)

+   [使用 PyCaret 在 Python 中进行聚类简介](https://www.kdnuggets.com/2021/12/introduction-clustering-python-pycaret.html)

+   [宣布 PyCaret 3.0：开源、低代码的 Python 机器学习](https://www.kdnuggets.com/2023/03/announcing-pycaret-30-opensource-lowcode-machine-learning-python.html)

+   [PyCaret 入门指南](https://www.kdnuggets.com/2022/11/getting-started-pycaret.html)
