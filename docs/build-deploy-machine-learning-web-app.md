# 构建和部署你的第一个机器学习 web 应用

> 原文：[https://www.kdnuggets.com/2020/05/build-deploy-machine-learning-web-app.html](https://www.kdnuggets.com/2020/05/build-deploy-machine-learning-web-app.html)

[评论](#comments)

**由 [Moez Ali](https://www.linkedin.com/in/profile-moez/) 贡献，PyCaret的创始人和作者**

![图示](../Images/8213893cbe65e0b98f2f494411a5cb55.png)

在我们的 [上一篇文章](https://towardsdatascience.com/machine-learning-in-power-bi-using-pycaret-34307f09394a) 中，我们展示了如何使用 [PyCaret](https://www.pycaret.org/) 在 Power BI 中训练和部署机器学习模型。如果你之前没有听说过 PyCaret，请阅读我们的 [公告](https://towardsdatascience.com/announcing-pycaret-an-open-source-low-code-machine-learning-library-in-python-4a1f1aad8d46) 以快速入门。

在本教程中，我们将使用 PyCaret 开发一个 **机器学习管道**，包括预处理转换和回归模型，以根据年龄、BMI、吸烟状态等人口统计数据和基本患者健康风险指标预测住院费用。

### ???? 在本教程中你将学到什么

+   什么是部署，我们为什么要部署机器学习模型。

+   使用 PyCaret 开发机器学习管道并训练模型。

+   使用一个名为‘Flask’的 Python 框架构建一个简单的 web 应用。

+   在‘Heroku’上部署 web 应用，并查看你的模型运行效果。

### ???? 本教程中我们将使用哪些工具？

**PyCaret**

[PyCaret](https://www.pycaret.org/) 是一个开源、低代码的 Python 机器学习库，用于训练和部署生产中的机器学习管道和模型。PyCaret 可以通过 pip 轻松安装。

```py
# for Jupyter notebook on your local computer
pip install **pycaret**# for azure notebooks and google colab
!pip install **pycaret**
```

**Flask**

[Flask](https://flask.palletsprojects.com/en/1.1.x/) 是一个允许你构建 web 应用的框架。一个 web 应用可以是商业网站、博客、电子商务系统，或是一个使用训练模型从实时数据中生成预测的应用。如果你还没有安装 Flask，可以使用 pip 进行安装。

```py
# install flask
pip install **Flask**
```

**GitHub**

[GitHub](https://www.github.com/) 是一个基于云的服务，用于托管、管理和控制代码。想象一下你在一个大型团队中工作，多人（有时是数百人）在进行更改。PyCaret 本身就是一个开源项目的例子，数百名社区开发者持续为源代码做出贡献。如果你以前没有使用过 GitHub，你可以 [注册](https://github.com/join) 一个免费的账户。

**Heroku**

[Heroku](https://www.heroku.com/) 是一个平台即服务（PaaS），它基于托管容器系统提供 Web 应用程序的部署，集成了数据服务和强大的生态系统。简单来说，这将允许你将应用程序从本地计算机转移到云端，以便任何人都可以通过 Web URL 访问。在本教程中，我们选择 Heroku 进行部署，因为它提供免费资源小时，当你[注册](https://signup.heroku.com/)新账户时可以获得。

![图示](../Images/d7e260c18d29b72de9c8083c75ff54ef.png)

机器学习工作流程（从训练到 PaaS 部署）

### 为什么要部署机器学习模型？

机器学习模型的部署是将模型投入生产的过程，在此过程中，Web 应用程序、企业软件和 API 可以通过提供新数据点并生成预测来使用已训练的模型。

通常，机器学习模型的建立是为了预测结果（例如，[分类](https://www.pycaret.org/classification)中的二元值，即 1 或 0，[回归](https://www.pycaret.org/regression)中的连续值，[聚类](https://www.pycaret.org/clustering)中的标签等）。生成预测的方式有两种： (i) 批量预测；和 (ii) 实时预测。在我们的[上一教程](https://towardsdatascience.com/machine-learning-in-power-bi-using-pycaret-34307f09394a)中，我们展示了如何在 Power BI 中部署机器学习模型并进行批量预测。在本教程中，我们将学习如何部署机器学习模型以进行实时预测。

### 商业问题

一家保险公司希望通过在住院时使用人口统计和基本患者健康风险指标来更好地预测患者费用，从而改善其现金流预测。

![图示](../Images/55da7f0ae496ad44230dae57479ebf11.png)

*(*[*数据源*](https://www.kaggle.com/mirichoi0218/insurance#insurance.csv)*)*

### 目标

建立一个 Web 应用程序，在其中输入患者的人口统计和健康信息，以预测费用。

### 任务

+   训练和验证模型，并开发一个机器学习管道用于部署。

+   使用输入表单构建一个基本的 HTML 前端，输入独立变量（年龄、性别、BMI、子女数量、吸烟者、地区）。

+   使用 Flask 框架构建 Web 应用程序的后端。

+   在 Heroku 上部署 Web 应用程序。部署完成后，它将变得公开可用，并可以通过 Web URL 进行访问。

### ???? 任务 1 — 模型训练和验证

训练和模型验证在集成开发环境（IDE）或 Notebook 中进行，可以是在本地计算机上或在云端。在本教程中，我们将使用 Jupyter Notebook 中的 PyCaret 来开发机器学习管道并训练回归模型。如果你之前没有使用过 PyCaret，请[点击这里](https://towardsdatascience.com/announcing-pycaret-an-open-source-low-code-machine-learning-library-in-python-4a1f1aad8d46) 了解更多关于 PyCaret 的信息，或查看我们[网站](https://www.pycaret.org/)上的[入门教程](https://www.pycaret.org/tutorial)。

在本教程中，我们进行了两个实验。第一个实验使用 PyCaret 中的默认预处理设置（缺失值填补、分类编码等）。第二个实验有一些额外的预处理任务，如缩放和标准化、自动特征工程以及将连续数据分箱。请参阅第二个实验的设置示例：

```py
# Experiment No. 2from **pycaret.regression** import *****r2 = **setup**(data, target = 'charges', session_id = 123,
           normalize = True,
           polynomial_features = True, trigonometry_features = True,
           feature_interaction=True, 
           bin_numeric_features= ['age', 'bmi'])
```

![图](../Images/23c9b273943d56697487a97c23d17fc0.png)

两个实验的信息网格比较

只需几行代码，神奇就会发生。注意在**实验 2**中，变换后的数据集具有 62 个用于训练的特征，而这些特征仅源于原始数据集中的 7 个特征。所有的新特征都是 PyCaret 中变换和自动特征工程的结果。

![图](../Images/4efab6a3013cd24cbd02b82bb1877314.png)

数据集在变换后的列

模型训练和验证的示例代码在 PyCaret 中：

```py
# Model Training and Validation 
lr = **create_model**('lr')
```

![图](../Images/77b5d1d13d4a17347dcdb86135860829.png)

线性回归模型的 10 折交叉验证

注意变换和自动特征工程的影响。R2 增加了 10%，且付出了很小的努力。我们可以比较两个实验的**残差图**，观察变换和特征工程对模型**异方差性**的影响。

```py
# plot residuals of trained model **plot_model**(lr, plot = 'residuals')
```

![图](../Images/3b081a146b5974e560d42e3bfeb4447f.png)

线性回归模型的残差图

机器学习是一个*迭代*过程。迭代次数和使用的技术取决于任务的关键程度以及预测错误的影响。机器学习模型在医院 ICU 中实时预测患者结果的严重性和影响远大于预测客户流失的模型。

在本教程中，我们仅进行了两次迭代，第二次实验中的线性回归模型将用于部署。然而，在这一阶段，模型仍然只是 Notebook 中的一个对象。要将其保存为可以转移到其他应用程序并被其使用的文件，请运行以下代码：

```py
# save transformation pipeline and model 
**save_model**(lr, model_name = 'c:/*username*/ins/deployment_28042020')
```

当你在 PyCaret 中保存模型时，会创建基于 **setup()** 函数中定义的配置的整个转换管道。所有的相互依赖关系都会自动协调。请参见存储在 ‘deployment_28042020’ 变量中的管道和模型：

![图示](../Images/9e478a35206a20e549f19d30a9661636.png)

使用 PyCaret 创建的管道

我们已经完成了训练和选择用于部署的模型的第一个任务。最终的机器学习管道和线性回归模型现在保存在本地驱动器中，位置由 **save_model()** 函数定义。（在此示例中：c:/*username*/ins/deployment_28042020.pkl）。

### ???? 任务 2 — 构建网页应用程序

现在我们的机器学习管道和模型已经准备好，我们将开始构建一个可以连接到这些管道并实时生成新数据预测的网页应用程序。该应用程序有两个部分：

+   前端（使用 HTML 设计）

+   后端（使用 Flask 在 Python 中开发）

### 网页应用程序的前端

通常，网页应用程序的前端是使用 HTML 构建的，这不是本文的重点。我们使用了一个简单的 HTML 模板和一个 CSS 样式表来设计输入表单。下面是我们网页应用程序前端页面的 HTML 代码片段。

![图示](../Images/2b54ec5cc7b7325392fccda870773a6d.png)

home.html 文件中的代码片段

你不需要成为 HTML 专家来构建简单的应用程序。有许多免费的平台提供 HTML 和 CSS 模板，并且可以通过拖放界面快速构建美观的 HTML 页面。

**CSS 样式表**

CSS（也称为层叠样式表）描述了 HTML 元素如何在屏幕上显示。它是一种有效控制应用程序布局的方式。样式表包含背景颜色、字体大小和颜色、边距等信息。它们被保存在外部的 .css 文件中，并通过包含 1 行代码与 HTML 关联。

![图示](../Images/e6172cc6c23d83a4fd3683b3a341b880.png)

home.html 文件中的代码片段

### 网页应用程序的后端

网页应用程序的后端是使用 Flask 框架开发的。对于初学者来说，可以直观地将 Flask 视为一个可以像其他 Python 库一样导入的库。请参见我们用 Flask 框架在 Python 中编写的后端示例代码片段。

![图示](../Images/feb16c63a07224b37ffa90a4a955a984.png)

app.py 文件中的代码片段

如果你还记得上面第 1 步，我们已经完成了线性回归模型的确定，该模型在 62 个特征上进行了训练，这些特征由 PyCaret 自动工程化。然而，我们的网页应用程序的前端有一个输入表单，只收集六个特征，即年龄、性别、bmi、孩子数量、吸烟者、区域。

我们如何在实时中将新数据点的 6 个特征转换为模型训练时的 62 个特征？随着模型训练过程中应用的转换序列，编码变得越来越复杂且耗时。

在 PyCaret 中，所有的转换，如类别编码、缩放、缺失值填充、特征工程甚至特征选择，都会在生成预测之前实时自动执行。

> *想象一下你为了将所有转换严格按顺序应用到模型预测之前需要写多少代码。在实践中，当你想到机器学习时，你应该考虑整个 ML 管道，而不仅仅是模型。*

**Testing App**

在我们将应用程序发布到 Heroku 之前的最后一步是本地测试 Web 应用程序。打开 Anaconda Prompt，导航到你计算机上保存 **'app.py'** 的文件夹。使用以下代码运行 Python 文件：

```py
python **app.py**
```

![Figure](../Images/ca5a8bbb6144dfd4c02bc8262e3aff94.png)

执行 app.py 时 Anaconda Prompt 中的输出

执行后，将 URL 复制到浏览器中，它应该会打开一个托管在你本地机器 (127.0.0.1) 上的 Web 应用程序。尝试输入测试值，查看预测功能是否正常工作。在下面的示例中，预期的账单金额为 19 岁女性吸烟者且无子女的西南部地区为 $20,900。

![Figure](../Images/d5073bc26689c4ed2520b3788f1b4bdc.png)

在本地机器上打开的 Web 应用程序

恭喜！你现在已经构建了你的第一个机器学习应用程序。现在是时候将这个应用程序从本地机器迁移到云端，以便其他人可以通过 Web URL 使用它。

### ???? 任务 3 — 在 Heroku 上部署 Web 应用

现在模型已经训练完成，机器学习管道也准备好了，应用程序已经在本地机器上测试过，我们准备开始在 Heroku 上部署。将应用程序源代码上传到 Heroku 有几种方法。最简单的方法是将 GitHub 仓库链接到你的 Heroku 账户。

如果你想跟着操作，可以从 GitHub 上叉取这个 [仓库](https://github.com/pycaret/deployment-heroku)。如果你不知道如何叉取仓库，请 [阅读这个](https://help.github.com/en/github/getting-started-with-github/fork-a-repo) 官方 GitHub 教程。

![Figure](../Images/b2daa1c271a42ab84e0dcc83c5b4bb87.png)

[https://www.github.com/pycaret/deployment-heroku](https://www.github.com/pycaret/deployment-heroku)

现在你对上面展示的仓库中的所有文件都很熟悉了，除了两个文件，即 '**requirements.txt**' 和 '**Procfile**'。

![Figure](../Images/35693d5786a9d88caa018a90432f0d30.png)

requirements.txt

'**requirements.txt**' 文件是一个文本文件，包含执行应用程序所需的 Python 包的名称。如果这些包在应用程序运行的环境中没有安装，应用程序将会失败。

![Figure](../Images/17ea811b90d8d3bd37482f7e5a324eec.png)

Procfile

**Procfile** 只是提供启动指令的代码行，告诉 web 服务器在有人登录应用程序时应该首先执行哪个文件。在这个例子中，我们的应用程序文件名是 ‘**app.py**’，应用程序的名称也是 ‘**app**’。*（因此是 app:app）*

一旦所有文件上传到 GitHub 仓库，我们现在可以开始在 Heroku 上进行部署。请按照以下步骤操作：

**步骤 1 — 在 heroku.com 上注册并点击 ‘创建新应用程序’**

![图示](../Images/aa02c1b6a02bcf867d41f7b46df172b9.png)

Heroku 仪表盘

**步骤 2 — 输入应用程序名称和区域**

![图示](../Images/701e0be8acd243ba464ee5d7a833a8fd.png)

Heroku — 创建新应用程序

**步骤 3 — 连接到托管代码的 GitHub 仓库**

![图示](../Images/a494b41066c23b3f6fcaab7eb27b91ca.png)

Heroku — 连接到 GitHub

**步骤 4 — 部署分支**

![图示](../Images/3470a80faee1fd99b96f7dcbbb6a69e4.png)

Heroku — 部署分支

**步骤 5 — 等待 5–10 分钟，成功**

![图示](../Images/5f2d87a6bdbbef8693b83f79722b3d93.png)

Heroku — 成功部署

应用程序已发布到网址：[https://pycaret-insurance.herokuapp.com/](https://pycaret-insurance.herokuapp.com/)

![图示](../Images/e1bde245a86a323e2c9dcd6df0112113.png)

[https://pycaret-insurance.herokuapp.com/](https://pycaret-insurance.herokuapp.com/)

在结束教程之前还有最后一件事要查看。

到目前为止，我们已经构建并部署了一个与我们的机器学习管道配合使用的 web 应用程序。现在，假设你已经有了一个企业应用程序，你希望将模型的预测集成进去。你需要的是一个可以通过 API 调用输入数据点并返回预测的 web 服务。为此，我们在我们的 **‘app.py’** 文件中创建了 ***predict_api*** 函数。请查看代码片段：

![图示](../Images/9b9346b7d5a4a3870f9877bc5ed6e1f5.png)

app.py 文件中的代码片段（web 应用程序的后端）

以下是如何使用 requests 库在 Python 中使用此网络服务：

```py
import **requests**url = 'https://pycaret-insurance.herokuapp.com/predict_api'pred = **requests.post(**url,json={'age':55, 'sex':'male', 'bmi':59, 'children':1, 'smoker':'male', 'region':'northwest'})**print**(pred.json())
```

![图示](../Images/af12c785bd4cf0f7db433e8b8ce99546.png)

向发布的网络服务发送请求以在笔记本中生成预测

### 下一个教程

在下一个教程中，我们将深入探讨如何使用 Docker 容器部署机器学习管道。我们将演示如何在 Linux 上轻松部署和运行容器化的机器学习应用程序。

关注我们的 [LinkedIn](https://www.linkedin.com/company/pycaret/) 并订阅我们的 [YouTube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI_B3g) 频道以了解更多有关 PyCaret 的信息。

**重要链接**

+   [用户指南 / 文档](https://www.pycaret.org/guide)

+   [GitHub 仓库](https://www.github.com/pycaret/pycaret)

+   [安装 PyCaret](https://www.pycaret.org/install)

+   [笔记本教程](https://www.pycaret.org/tutorial)

+   [贡献 PyCaret](https://www.pycaret.org/contribute)

**想了解特定模块的内容？**

自1.0.0版首次发布以来，PyCaret提供了以下可用模块。点击下面的链接查看文档和Python中的工作示例。

+   [分类分析](https://www.pycaret.org/classification)

+   [回归分析](https://www.pycaret.org/regression)

+   [聚类分析](https://www.pycaret.org/clustering)

+   [异常检测](https://www.pycaret.org/anomaly-detection)

+   [自然语言处理](https://www.pycaret.org/nlp)

+   [关联规则挖掘](https://www.pycaret.org/association-rules)

**另见:**

PyCaret入门教程（Notebook版）:

+   [聚类分析](https://www.pycaret.org/clu101)

+   [异常检测](https://www.pycaret.org/anom101)

+   [自然语言处理](https://www.pycaret.org/nlp101)

+   [关联规则挖掘](https://www.pycaret.org/arul101)

+   [回归分析](https://www.pycaret.org/reg101)

+   [分类分析](https://www.pycaret.org/clf101)

**开发管道中有哪些？**

我们正在积极改进PyCaret。未来的发展计划包括一个新的**时间序列预测**模块，与**TensorFlow**的集成，以及PyCaret可扩展性的重大改进。如果您想分享反馈并帮助我们进一步改进，可以在网站上[填写此表单](https://www.pycaret.org/feedback)或在我们的[GitHub](https://www.github.com/pycaret/)或[LinkedIn](https://www.linkedin.com/company/pycaret/)页面留言。

**您想要贡献吗？**

PyCaret是一个开源项目。欢迎大家贡献。如果您想贡献，请随时处理[开放问题](https://github.com/pycaret/pycaret/issues)。我们接受带有单元测试的开发分支上的拉取请求。

如果您喜欢PyCaret，请在我们的[GitHub仓库](https://www.github.com/pycaret/pycaret)上给我们⭐️。

Medium: [https://medium.com/@moez_62905/](https://medium.com/@moez_62905/machine-learning-in-power-bi-using-pycaret-34307f09394a)

LinkedIn: [https://www.linkedin.com/in/profile-moez/](https://www.linkedin.com/in/profile-moez/)

Twitter: [https://twitter.com/moezpycaretorg1](https://twitter.com/moezpycaretorg1)

**个人简介: [Moez Ali](https://www.linkedin.com/in/profile-moez/)** 是数据科学家，也是PyCaret的创始人和作者。

[原文](https://towardsdatascience.com/build-and-deploy-your-first-machine-learning-web-app-e020db344a99)。经许可转载。

**相关:**

+   [宣布PyCaret 1.0.0](/2020/04/announcing-pycaret.html)

+   [在Power BI中使用PyCaret进行机器学习](/2020/05/machine-learning-power-bi-pycaret.html)

+   [使用pdpipe构建Pandas管道](/2019/12/build-pipelines-pandas-pdpipe.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织IT需求

* * *

### 更多相关话题

+   [使用Heroku部署机器学习Web应用](https://www.kdnuggets.com/2022/04/deploy-machine-learning-web-app-heroku.html)

+   [成为优秀数据科学家所需的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [在5分钟内构建机器学习Web应用](https://www.kdnuggets.com/2022/03/build-machine-learning-web-app-5-minutes.html)

+   [KDnuggets新闻2022年3月9日：在5分钟内构建机器学习Web应用…](https://www.kdnuggets.com/2022/n10.html)
