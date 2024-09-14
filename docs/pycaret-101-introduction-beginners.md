# PyCaret 101: 初学者入门指南

> 原文：[https://www.kdnuggets.com/2021/06/pycaret-101-introduction-beginners.html](https://www.kdnuggets.com/2021/06/pycaret-101-introduction-beginners.html)

[评论](#comments)

**由 [Moez Ali](https://www.linkedin.com/in/profile-moez/)，PyCaret 的创始人兼作者**

![](../Images/2b4e095038b7e40091fb46813a4c773b.png)

PyCaret — 一个开源的低代码 Python 机器学习库

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在的组织的 IT

* * *

### PyCaret

[PyCaret](https://www.pycaret.org/) 是一个开源的低代码机器学习库和端到端模型管理工具，内置于 Python 中，用于自动化机器学习工作流程。它的易用性、简单性以及能够快速高效地构建和部署端到端机器学习管道的能力将令你惊叹。

PyCaret 是一个低代码库，可以用几行代码替代数百行代码。这使得实验周期变得极其快速高效。

PyCaret 是 **简单易用的**。PyCaret 中执行的所有操作都被顺序存储在一个 **Pipeline** 中，完全自动化以 **部署**。无论是填补缺失值、进行独热编码、转换分类数据、特征工程，还是超参数调优，PyCaret 都会自动完成。要了解更多关于 PyCaret 的信息，请观看这段 1 分钟的视频。

PyCaret — 一个开源的低代码 Python 机器学习库

### PyCaret 的特点

![](../Images/eadab4d647dcf4f77f48fa4df54f1c5a.png)

作者提供的图像

### PyCaret 的模块

PyCaret 是一个模块化库，按模块排列，每个模块代表一个机器学习用例。截止到本文撰写时，支持以下模块：

![](../Images/25cf0f7d01befb77986c3b39587cd96e.png)

作者提供的图像 — PyCaret 支持的机器学习用例

**时间序列模块正在开发中，将在下一个主要版本中推出。**

### 安装 PyCaret

安装 PyCaret 非常简单，仅需几分钟。我们强烈建议使用虚拟环境，以避免与其他库的潜在冲突。

PyCaret 的默认安装是一个精简版的 pycaret，只安装了硬性依赖项，[列在这里](https://github.com/pycaret/pycaret/blob/master/requirements.txt)。

```py
**# install slim version (default)** pip install pycaret**# install the full version**
pip install pycaret[full]
```

当你安装 PyCaret 的完整版时，所有的可选依赖项也会被安装，详细信息见 [这里](https://github.com/pycaret/pycaret/blob/master/requirements-optional.txt)。

![](../Images/61927545f7f850ddb656ad96326dd159.png)

PyCaret 数字化 — 作者提供的图片

### ???? 开始吧

在我向你展示如何用 PyCaret 轻松做机器学习之前，让我们先从高层次上谈谈机器学习生命周期：

![](../Images/a9a1254f49bf8637e4b40ea779055c7a.png)

机器学习生命周期 — 作者提供的图片（从左到右阅读）

+   **业务问题 —** 这是机器学习工作流的第一步。根据用例和问题的复杂性，这一步可能需要几天到几周的时间才能完成。在这一阶段，数据科学家会与主题专家（SME）会面，以了解问题，采访关键利益相关者，收集信息，并设定项目的总体期望。

+   **数据来源与 ETL —** 一旦理解了问题，就可以利用访谈中获得的信息从企业数据库中获取数据。

+   **探索性数据分析（EDA） —** 模型尚未开始。EDA 是你分析原始数据的阶段。你的目标是探索数据，评估数据的质量、缺失值、特征分布、相关性等。

+   **数据准备 —** 现在是准备数据模型训练的时候了。这包括将数据划分为训练集和测试集、填补缺失值、独热编码、目标编码、特征工程、特征选择等。

+   **模型训练与选择 —** 这是大家都兴奋的步骤。这包括训练一堆模型、调整超参数、模型集成、评估性能指标、模型分析如 AUC、混淆矩阵、残差等，最后选择一个最佳模型用于生产环境中的业务应用。

+   **部署与监控 —** 这是最后一步，主要涉及 MLOps。这包括打包最终模型、创建 Docker 镜像、编写评分脚本，然后将所有这些整合在一起，最终将其发布为一个 API，用于对通过管道传入的新数据进行预测。

传统的方法相当繁琐、耗时，并且需要大量的技术知识，我可能无法在一个教程中涵盖所有内容。然而，在这个教程中，我将使用 PyCaret 来演示数据科学家如何变得如此高效地完成这些任务。

### ???? 业务问题

在本教程中，我将使用达顿商学院的一个非常流行的案例研究，该案例研究发表在[哈佛商业评论](https://hbsp.harvard.edu/product/UV0869-PDF-ENG)上。案例涉及两个未来要结婚的人。名叫*Greg*的男子想买一个戒指向名叫*Sarah*的女孩求婚。问题是找到Sarah会喜欢的戒指，但在朋友的建议下，Greg决定买一个钻石石头，以便Sarah可以决定她的选择。Greg随后收集了6000颗钻石的数据，包括价格和切工、颜色、形状等属性。

### ???? 数据

在本教程中，我将使用达顿商学院的一个非常流行的案例研究的数据集，该案例研究发表在[哈佛商业评论](https://hbsp.harvard.edu/product/UV0869-PDF-ENG)上。本教程的目标是根据钻石的属性（如克拉重量、切工、颜色等）预测钻石价格。你可以从[PyCaret的仓库](https://github.com/pycaret/pycaret/tree/master/datasets)下载数据集。

```py
**# load the dataset from pycaret** from pycaret.datasets import get_data
data = get_data('diamond')
```

![](../Images/fb95a6b487372fcc0798bf241f463754.png)

数据的样本行

### ???? 探索性数据分析

让我们做一些快速的可视化，以评估独立特征（重量、切工、颜色、清晰度等）与目标变量`Price`的关系。

```py
**# plot scatter carat_weight and Price**
import plotly.express as px
fig = px.scatter(x=data['Carat Weight'], y=data['Price'], 
                 facet_col = data['Cut'], opacity = 0.25, template = 'plotly_dark', trendline='ols',
                 trendline_color_override = 'red', title = 'SARAH GETS A DIAMOND - A CASE STUDY')
fig.show()
```

![](../Images/5e3fa68a8d11c0a17130264aa4d9f53d.png)

让我们检查目标变量的分布。

```py
**# plot histogram**
fig = px.histogram(data, x=["Price"], template = 'plotly_dark', title = 'Histogram of Price')
fig.show()
```

![](../Images/06f59e9b6448c861aa90716081884715.png)

注意到`Price`的分布是右偏的，我们可以快速检查对数变换是否能使`Price`大致正态分布，从而给假设正态分布的算法提供机会。

```py
import numpy as np**# create a copy of data**
data_copy = data.copy()**# create a new feature Log_Price**
data_copy['Log_Price'] = np.log(data['Price'])**# plot histogram**
fig = px.histogram(data_copy, x=["Log_Price"], title = 'Histgram of Log Price', template = 'plotly_dark')
fig.show()
```

![](../Images/a4bba39265f978622d2702ae97bf7620.png)

这确认了我们的假设。变换将帮助我们摆脱偏斜，使目标变量大致符合正态分布。基于此，我们将在训练模型之前对`Price`变量进行变换。

### ???? 数据准备

在PyCaret的所有模块中，`setup`是任何使用PyCaret的机器学习实验中的第一个也是唯一的强制步骤。该函数负责在训练模型之前所需的所有数据准备工作。除了执行一些基本的默认处理任务外，PyCaret还提供了广泛的预处理功能。要了解PyCaret中所有预处理功能的更多信息，请参见这个[链接](https://pycaret.org/preprocessing/)。

```py
**# initialize setup**
from pycaret.regression import *
s = setup(data, target = 'Price', transform_target = True, log_experiment = True, experiment_name = 'diamond')
```

![](../Images/f92622b3666c7bc68fe61fb6b46ddf5c.png)

pycaret.regression模块中的setup函数

当你初始化PyCaret中的`setup`函数时，它会分析数据集并推断所有输入特征的数据类型。如果所有数据类型都被正确推断，你可以按回车继续。

注意：

+   我已传递`log_experiment = True`和`experiment_name = 'diamond'`，这将告诉PyCaret自动记录所有的指标、超参数和模型工件，在建模阶段进行时，这一切都在后台进行。这是由于与[MLflow](https://www.mlflow.org/)的集成实现的。

+   此外，我在`setup`中使用了`transform_target = True`。PyCaret将使用box-cox变换在后台转换`Price`变量。这影响了数据的分布，类似于对数变换（*技术上有所不同*）。如果你想了解更多关于box-cox变换的信息，你可以参考这个[链接](https://onlinestatbook.com/2/transformations/box-cox.html)。

![](../Images/ac6d418cca0eb62de9b2bf43575dc318.png)

设置输出 — 为显示目的已截断

### ???? 模型训练与选择

现在数据已经准备好进行建模，让我们使用`compare_models`函数开始训练过程。它将训练模型库中的所有算法，并使用k折交叉验证评估多个性能指标。

```py
**# compare all models**
best = compare_models()
```

![](../Images/07fecb193d63f93d20f3fea859771fe5.png)

`compare_models`的输出

```py
**# check the residuals of trained model**
plot_model(best, plot = 'residuals_interactive')
```

![](../Images/eb43d501d3b721c528f968ef18076086.png)

最佳模型的残差和QQ图

```py
**# check feature importance**
plot_model(best, plot = 'feature')
```

![](../Images/0317c1fe59b3d80b2e167e051eab4208.png)

### 完成并保存Pipeline

现在让我们最终确定最佳模型，即在整个数据集（包括测试集）上训练最佳模型，然后将Pipeline保存为pickle文件。

```py
**# finalize the model**
final_best = finalize_model(best)**# save model to disk** save_model(final_best, 'diamond-pipeline')
```

`save_model`函数将把整个Pipeline（包括模型）保存为本地磁盘上的pickle文件。默认情况下，它将把文件保存到与Notebook或脚本所在的文件夹相同的位置，但如果需要，您也可以传递完整路径：

```py
save_model(final_best, 'c:/users/moez/models/diamond-pipeline'
```

### ???? 部署

记住我们在设置函数中传递了`log_experiment = True`以及`experiment_name = 'diamond'`。让我们看看PyCaret在MLflow的帮助下在后台做了什么神奇的事情。要查看这些魔法，让我们启动MLflow服务器：

```py
**# within notebook (notice ! sign infront)** !mlflow ui**# on command line in the same folder** mlflow ui
```

现在打开你的浏览器，输入“https://localhost:5000”。它将打开一个类似这样的用户界面：

![](../Images/b6170680042f3b2a36cc3b3356deb876.png)

[https://localhost:5000](https://localhost:5000/)

上表中的每一项代表一个训练运行，产生一个训练好的Pipeline和一堆元数据，如运行的日期时间、性能指标、模型超参数、标签等。让我们点击其中一个模型：

![](../Images/99991e20fe48b1af29c79fe0a5967e7a.png)

第一部分 — CatBoost 回归器

![](../Images/d48bae6d168a6898337ee1394c12df66.png)

第二部分 — CatBoost 回归器（续）

![](../Images/901c1286a5f3784d006ef3fc25b7f9d2.png)

第三部分 — CatBoost 回归器

注意你有一个`logged_model`的地址路径。这是使用Catboost回归器训练的Pipeline。你可以使用`load_model`函数读取这个Pipeline。

```py
**# load model**
from pycaret.regression import load_model
pipeline = load_model('C:/Users/moezs/mlruns/1/b8c10d259b294b28a3e233a9d2c209c0/artifacts/model/model')**# print pipeline** print(pipeline)
```

![](../Images/bc9604047be618af559db94d8a86c428.png)

print(pipeline)的输出

现在让我们使用这个Pipeline对新数据进行预测

```py
**# create a copy of data and drop Price** data2 = data.copy()
data2.drop('Price', axis=1, inplace=True)**# generate predictions** from pycaret.regression import predict_model
predictions = predict_model(pipeline, data=data2)
predictions.head()
```

![](../Images/1c1bee5b356af4b2b77b4227a490bd60.png)

从管道生成的预测

哇哦！我们现在从训练好的管道中获得了推断。如果这是你的第一次，恭喜你。请注意，所有的转换，如目标转换、独热编码、缺失值填充等，都是在后台自动完成的。你将得到一个包含实际规模预测的数据框，这才是你关心的。

![](../Images/83f3b7bb83ff24f6897be4a9f2a07d9b.png)

作者提供的图像

![](../Images/b36b63110de0ece1dd0a6283b02637a3.png)

作者提供的图像

使用这个轻量级的工作流自动化库在 Python 中没有什么限制。如果你觉得有用，请不要忘记在我们的 GitHub 仓库中给我们 ⭐️。

想了解更多关于 PyCaret 的信息，请关注我们的 [LinkedIn](https://www.linkedin.com/company/pycaret/) 和 [Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI_B3g)。

加入我们的 Slack 频道。邀请链接 [在这里](https://join.slack.com/t/pycaret/shared_invite/zt-p7aaexnl-EqdTfZ9U~mF0CwNcltffHg)。

### 你可能还会感兴趣：

[在 Power BI 中使用 PyCaret 2.0 构建自己的 AutoML](https://towardsdatascience.com/build-your-own-automl-in-power-bi-using-pycaret-8291b64181d)

[使用 Docker 在 Azure 上部署机器学习管道](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-cloud-using-docker-container-bec64458dc01)

[在 Google Kubernetes Engine 上部署机器学习管道](https://towardsdatascience.com/deploy-machine-learning-model-on-google-kubernetes-engine-94daac85108b)

[在 AWS Fargate 上部署机器学习管道](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-aws-fargate-eb6e1c50507)

[构建并部署你的第一个机器学习网页应用](https://towardsdatascience.com/build-and-deploy-your-first-machine-learning-web-app-e020db344a99)

[使用 AWS Fargate 无服务器部署 PyCaret 和 Streamlit 应用](https://towardsdatascience.com/deploy-pycaret-and-streamlit-app-using-aws-fargate-serverless-infrastructure-8b7d7c0584c2)

[使用 PyCaret 和 Streamlit 构建并部署机器学习网页应用](https://towardsdatascience.com/build-and-deploy-machine-learning-web-app-using-pycaret-and-streamlit-28883a569104)

[在 GKE 上部署使用 Streamlit 和 PyCaret 构建的机器学习应用](https://towardsdatascience.com/deploy-machine-learning-app-built-using-streamlit-and-pycaret-on-google-kubernetes-engine-fd7e393d99cb)

### 重要链接

[文档](https://pycaret.readthedocs.io/en/latest/installation.html)

[博客](https://medium.com/@moez_62905)

[GitHub](https://www.github.com/pycaret/pycaret)

[StackOverflow](https://stackoverflow.com/questions/tagged/pycaret)

[安装 PyCaret](https://pycaret.readthedocs.io/en/latest/installation.html) [Notebook 教程](https://pycaret.readthedocs.io/en/latest/tutorials.html) [为 PyCaret 做贡献](https://pycaret.readthedocs.io/en/latest/contribute.html)

### 想了解某个特定模块？

点击下面的链接查看文档和工作示例。

[分类](https://pycaret.readthedocs.io/en/latest/api/classification.html) [回归](https://pycaret.readthedocs.io/en/latest/api/regression.html)

[聚类](https://pycaret.readthedocs.io/en/latest/api/clustering.html)

[异常检测](https://pycaret.readthedocs.io/en/latest/api/anomaly.html)

[自然语言处理](https://pycaret.readthedocs.io/en/latest/api/nlp.html) [关联规则挖掘](https://pycaret.readthedocs.io/en/latest/api/arules.html)

**结束**

**简介： [Moez Ali](https://www.linkedin.com/in/profile-moez/)** 是数据科学家，也是 PyCaret 的创始人和作者。

[原文](https://medium.com/analytics-vidhya/pycaret-101-for-beginners-27d9aefd34c5). 经许可转载。

**相关内容：**

+   [使用 PyCaret 和 MLflow 的简单 MLOps](/2021/05/easy-mlops-pycaret-mlflow.html)

+   [使用 PyCaret 编写和训练自定义机器学习模型](/2021/05/pycaret-write-train-custom-machine-learning-models.html)

+   [你不知道的 PyCaret 的 5 件事](/2020/07/5-things-pycaret.html)

### 更多相关内容

+   [使用 PyCaret 进行二分类介绍](https://www.kdnuggets.com/2021/12/introduction-binary-classification-pycaret.html)

+   [使用 PyCaret 进行 Python 中的聚类介绍](https://www.kdnuggets.com/2021/12/introduction-clustering-python-pycaret.html)

+   [宣布 PyCaret 3.0：开源、低代码的 Python 机器学习](https://www.kdnuggets.com/2023/03/announcing-pycaret-30-opensource-lowcode-machine-learning-python.html)

+   [开始使用 PyCaret](https://www.kdnuggets.com/2022/11/getting-started-pycaret.html)

+   [数据科学家的线性规划 101](https://www.kdnuggets.com/2023/02/linear-programming-101-data-scientists.html)

+   [LangChain 101：构建您自己的 GPT 驱动应用程序](https://www.kdnuggets.com/2023/04/langchain-101-build-gptpowered-applications.html)
