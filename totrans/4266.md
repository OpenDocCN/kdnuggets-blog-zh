# 用 PyCaret 和 Gradio 提升你的机器学习实验

> 原文：[https://www.kdnuggets.com/2021/05/supercharge-machine-learning-experiments-pycaret-gradio.html](https://www.kdnuggets.com/2021/05/supercharge-machine-learning-experiments-pycaret-gradio.html)

[评论](#comments)

**作者 [Moez Ali](https://www.linkedin.com/in/profile-moez/)，PyCaret 的创始人兼作者**

### ???? 介绍

本教程是一个逐步的、初学者友好的说明，展示了如何在几分钟内将两个强大的 Python 开源库[PyCaret](https://www.pycaret.org/)和[Gradio](https://www.gradio.app/)集成在一起，从而提升机器学习实验的效率。

本教程是一个“hello world”示例，我使用了来自 UCI 的[Iris 数据集](https://archive.ics.uci.edu/ml/datasets/iris)，这是一个多分类问题，目标是预测鸢尾花的类别。此示例中的代码可以在任何其他数据集上复现，无需重大修改。

### ???? PyCaret

PyCaret 是一个开源的低代码机器学习库和端到端模型管理工具，用 Python 构建，旨在自动化机器学习工作流。因其易用性、简单性以及快速高效地构建和部署端到端 ML 原型的能力而极受欢迎。

PyCaret 是一个低代码库，可以用少量代码替代数百行代码。这使得实验周期变得极其快速高效。

PyCaret **简单且** **易于使用**。在 PyCaret 中执行的所有操作都会顺序存储在一个**管道**中，该管道完全自动化以便于**部署**。无论是填补缺失值、一键编码、转换分类数据、特征工程还是超参数调整，PyCaret 都会自动化处理所有这些操作。

要了解更多关于 PyCaret 的信息，请查看他们的[GitHub](https://www.github.com/pycaret/pycaret)。

### ???? Gradio

Gradio 是一个开源 Python 库，用于在你的机器学习模型周围创建可自定义的 UI 组件。Gradio 使你可以在浏览器中“玩弄”你的模型，通过拖放自己的图像、粘贴自己的文本、录制自己的声音等方式，查看模型的输出。

Gradio 适用于：

+   在训练好的 ML 流水线周围创建快速演示

+   获取关于模型性能的实时反馈

+   在开发过程中交互式调试你的模型

要了解更多关于 Gradio 的信息，请查看他们的[GitHub](https://github.com/gradio-app/gradio)。

![](../Images/6b5b2bec4a3fa1212dcda34a60ffae00.png)

PyCaret 和 Gradio 的工作流程

### ???? 安装 PyCaret

安装 PyCaret 非常简单，仅需几分钟。我们强烈建议使用虚拟环境，以避免与其他库的潜在冲突。

PyCaret 的默认安装是一个精简版，仅安装了硬性依赖，详见[此处](https://github.com/pycaret/pycaret/blob/master/requirements.txt)。

```py
**# install slim version (default)** pip install pycaret**# install the full version**
pip install pycaret[full]
```

当你安装 pycaret 的完整版时，所有可选依赖项也会被安装，如 [此处列出](https://github.com/pycaret/pycaret/blob/master/requirements-optional.txt)。

### ???? 安装 Gradio

你可以从 pip 安装 gradio。

```py
pip install gradio
```

### ???? 开始使用

```py
**# load the iris dataset from pycaret repo**
from pycaret.datasets import get_data
data = get_data('iris')
```

![](../Images/68a4c217ba36c111fda107572bf06777.png)

从鸢尾花数据集中提取的样本行

### ???? 初始化设置

```py
**# initialize setup**
from pycaret.classification import *
s = setup(data, target = 'species', session_id = 123)
```

![](../Images/3991ce23b61d9448e9bf124772818497.png)

每当你初始化 PyCaret 中的 `setup` 函数时，它会对数据集进行分析，并推断所有输入特征的数据类型。在这种情况下，你可以看到所有四个特征（*sepal_length, sepal_width, petal_length, 和 petal_width*）都正确地识别为数值型数据。你可以按 Enter 继续。

![](../Images/dbd7bfd8b0d2c8cb64cb2bc73e52b569.png)

setup 输出 — 为显示而截断

对 PyCaret 中的所有模块而言，`setup` 函数是开始任何机器学习实验的第一个也是唯一的必需步骤。除了默认执行一些基本处理任务外，PyCaret 还提供了广泛的预处理功能，如 [缩放和转换](https://pycaret.org/normalization/)、[特征工程](https://pycaret.org/feature-interaction/)、[特征选择](https://pycaret.org/feature-importance/)，以及几个关键的数据准备步骤，如 [独热编码](https://pycaret.org/one-hot-encoding/)、[缺失值填补](https://pycaret.org/missing-values/)、[过采样/欠采样](https://pycaret.org/fix-imbalance/)，等等。要了解更多关于 PyCaret 中所有预处理功能的信息，你可以查看这个 [链接](https://pycaret.org/preprocessing/)。

![](../Images/0c3ca66dbbf45cd55fd9adab8c167655.png)

[https://pycaret.org/preprocessing/](https://pycaret.org/preprocessing/)

### ???? 比较模型

这是我们推荐的在 PyCaret 的 *任何* 监督实验工作流中的第一步。此函数使用默认超参数训练模型库中的所有可用模型，并使用交叉验证评估性能指标。

此函数的输出是一个表格，显示所有模型的均值交叉验证分数。折数可以使用 `fold` 参数定义（默认 = 10 折）。表格按选择的指标排序（从高到低），可以使用 `sort` 参数定义（默认 = ‘准确度’）。

```py
best = compare_models(n_select = 15)
compare_model_results = pull()
```

`n_select` 参数在 setup 函数中控制训练模型的返回。在这种情况下，我将其设置为 15，意味着返回前 15 个模型作为列表。`pull` 函数在第二行中将 `compare_models` 的输出存储为 `pd.DataFrame`。

![](../Images/a40c5b924d1a33c140f262e7d0e1c972.png)

compare_models 的输出

```py
len(best)
>>> 15print(best[:5])
```

![](../Images/c38744faa6461922f7018289e6bcfa6f.png)

print(best[:5]) 的输出

### ???? Gradio

现在我们完成了建模过程，让我们使用 Gradio 创建一个简单的 UI 来与我们的模型互动。我将分两部分进行，首先创建一个使用 PyCaret 的 `predict_model` 功能生成并返回预测的函数，然后将这个函数集成到 Gradio 中，设计一个简单的输入表单以实现互动。

### **第一部分 — 创建内部函数**

代码的前两行将输入特征转换为 pandas DataFrame。第 7 行创建了 `compare_models` 输出中显示的模型名称的唯一列表（这将作为 UI 中的下拉框）。第 8 行根据列表的索引值选择最佳模型（这个值将通过 UI 传递），第 9 行使用 PyCaret 的 `predict_model` 功能对数据集进行评分。

[https://gist.github.com/moezali1/2a383489a08757df93572676d20635e0](https://gist.github.com/moezali1/2a383489a08757df93572676d20635e0)

### 第二部分 — 使用 Gradio 创建 UI

下面代码的第 3 行创建了一个模型名称的下拉框，第 4–7 行为每个输入特征创建了一个滑块，我已将默认值设置为每个特征的均值。第 9 行启动了一个 UI（在 notebook 以及你的本地主机上，你可以在浏览器中查看）。

[https://gist.github.com/moezali1/a1d83fb61e0ce14adcf4dffa784b1643](https://gist.github.com/moezali1/a1d83fb61e0ce14adcf4dffa784b1643)

![](../Images/2be1a04ec464e208c77ed2a305057953.png)

Gradio 接口运行的输出

你可以在这里观看这个快速视频，了解如何轻松地与管道互动，并查询模型，而无需编写数百行代码或开发一个完整的前端。

使用 PyCaret 和 Gradio 为你的机器学习实验加速

我希望你能欣赏 PyCaret 和 Gradio 的易用性和简洁性。在不到 25 行代码和几分钟的实验中，我使用 PyCaret 训练和评估了多个模型，并开发了一个轻量级的 UI 来在 Notebook 中与模型互动。

### 敬请期待！

下周我将编写一个关于使用 [PyCaret 异常检测模块](https://pycaret.readthedocs.io/en/latest/api/anomaly.html) 对时间序列数据进行无监督异常检测的教程。请关注我的 [Medium](https://medium.com/@moez-62905)、[LinkedIn](https://www.linkedin.com/in/profile-moez/) 和 [Twitter](https://twitter.com/moezpycaretorg1) 以获取更多更新。

使用这个轻量级的 Python 工作流自动化库，你可以实现无限的可能。如果你觉得这个有用，请不要忘记在我们的 GitHub 仓库上给我们 ⭐️。

要了解更多关于 PyCaret 的信息，请关注我们的 [LinkedIn](https://www.linkedin.com/company/pycaret/) 和 [Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI_B3g)。

加入我们的 Slack 频道。邀请链接 [在这里](https://join.slack.com/t/pycaret/shared_invite/zt-p7aaexnl-EqdTfZ9U~mF0CwNcltffHg)。

### 你可能还会对以下内容感兴趣：

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

[安装 PyCaret](https://pycaret.readthedocs.io/en/latest/installation.html) [笔记本教程](https://pycaret.readthedocs.io/en/latest/tutorials.html) [参与 PyCaret 贡献](https://pycaret.readthedocs.io/en/latest/contribute.html)

### 想了解某个特定模块吗？

点击下面的链接查看文档和工作示例。

[分类](https://pycaret.readthedocs.io/en/latest/api/classification.html) [回归](https://pycaret.readthedocs.io/en/latest/api/regression.html)

[聚类](https://pycaret.readthedocs.io/en/latest/api/clustering.html)

[异常检测](https://pycaret.readthedocs.io/en/latest/api/anomaly.html)

[自然语言处理](https://pycaret.readthedocs.io/en/latest/api/nlp.html) [关联规则挖掘](https://pycaret.readthedocs.io/en/latest/api/arules.html)

**个人简介: [Moez Ali](https://www.linkedin.com/in/profile-moez/)** 是一位数据科学家，同时也是 PyCaret 的创始人兼作者。

[原创](https://towardsdatascience.com/supercharge-your-machine-learning-experiments-with-pycaret-and-gradio-5932c61f80d9)。已获许可转载。

**相关:**

+   [使用 PyCaret 编写并训练自定义机器学习模型](/2021/05/pycaret-write-train-custom-machine-learning-models.html)

+   [使用 PyCaret + MLflow 实现轻松的 MLOps](/2021/05/easy-mlops-pycaret-mlflow.html)

+   [使用Streamlit进行主题建模](/2021/05/topic-modeling-streamlit.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的IT工作

* * *

### 更多相关话题

+   [用Hugging Face和Gradio在5分钟内构建AI聊天机器人](https://www.kdnuggets.com/2023/06/build-ai-chatbot-5-minutes-hugging-face-gradio.html)

+   [加速你的AI之旅！加入Uplimit的免费AI构建课程…](https://www.kdnuggets.com/2024/01/uplimit-supercharge-your-ai-journey-openai-course)

+   [机器学习实验的版本控制与追踪](https://www.kdnuggets.com/2021/12/versioning-machine-learning-experiments-tracking.html)

+   [深度学习实验中的Hydra配置](https://www.kdnuggets.com/2023/03/hydra-configs-deep-learning-experiments.html)

+   [宣布PyCaret 3.0：开源、低代码的Python机器学习](https://www.kdnuggets.com/2023/03/announcing-pycaret-30-opensource-lowcode-machine-learning-python.html)

+   [如何设计数据收集实验](https://www.kdnuggets.com/2022/04/design-experiments-data-collection.html)
