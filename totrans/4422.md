# 使用DALEX和Neptune进行可解释且可复现的机器学习模型开发

> 原文：[https://www.kdnuggets.com/2020/08/explainable-reproducible-machine-learning-model-development-dalex-neptune.html](https://www.kdnuggets.com/2020/08/explainable-reproducible-machine-learning-model-development-dalex-neptune.html)

[评论](#comments)

**由[Jakub Czakon](https://www.linkedin.com/in/jakub-czakon-2b797b69/)、neptune.ai的高级数据科学家，[Przemysław Biecek](https://www.linkedin.com/in/pbiecek/)、MI2DataLab创始人以及MI2DataLab的研究工程师Adam Rydelek**

![](../Images/f6bf1686db1df1c4f1f5dfd97c02ac5f.png)

机器学习模型开发是困难的，尤其是在现实世界中。

通常，你需要：

+   理解业务问题，

+   收集数据，

+   探索它，

+   设置合适的验证方案，

+   实现模型并调整参数，

+   以对业务有意义的方式部署它们，

+   检查模型结果只会发现你必须处理的新问题。

这还不是全部。

你应该对你运行的**实验**和训练的**模型**进行**版本控制**，以便你或其他人需要检查它们或在未来复现结果时使用。从我的经验来看，这一刻总是出乎意料，“我希望我早些考虑到这一点”的感觉非常真实（且痛苦）。

但还有更多内容。

随着机器学习模型服务于真实的人，误分类案例（这是使用机器学习的自然结果）影响了人们的生活，有时还非常不公平。这使得**解释模型预测的能力**成为一种必要，而不仅仅是一个附加功能。

那么你可以做些什么呢？

幸运的是，如今有工具可以解决这两个问题。

最棒的是，你可以将它们结合起来**使你的模型具有版本控制、可复现和可解释性**。

**继续阅读以了解如何：**

+   使用**DALEX**解释器解释机器学习模型

+   使用**Neptune**使你的模型有版本控制并使实验可复现

+   使用**Neptune + DALEX集成**自动保存每次训练运行的模型解释器和交互式解释图表

+   使用**版本控制的解释器**比较、调试和审计你构建的每个模型

让我们深入了解一下。

### 使用DALEX进行可解释的机器学习

如今，仅仅在测试集上得分高的模型通常是不够的。这就是为什么对可解释人工智能（**XAI**）的兴趣日益增长，XAI是一套让你理解模型行为的方法和技术。

有许多可解释人工智能（XAI）方法在多种编程语言中可用。在机器学习中一些最常用的方法是*LIME*、*SHAP*或*PDP*，但还有许多其他方法。

在众多技术中很容易迷失方向，这时**可解释人工智能金字塔**派上用场。它将与模型探索相关的需求汇聚成一个可扩展的逐层地图。左侧关于单个实例的需求，右侧关于整个模型的需求。连续的层次深入探讨有关模型行为的更多详细问题（局部或全局）。

![图](../Images/0a85199219440aaa9d112da40313744e.png)

XAI pyramide | 了解更多请参见[解释模型分析电子书](https://pbiecek.github.io/ema/)

DALEX（适用于R和Python）是一个**帮助你理解**复杂模型工作原理的工具。它目前仅适用于表格数据（但未来将支持文本和视觉）。

它与用于构建机器学习模型的最流行框架集成，如*keras, sklearn, xgboost, lightgbm, H2O*等！

**DALEX**的核心对象是**解释器**。它连接训练或评估数据和训练好的模型，并提取你需要解释的所有信息。

一旦拥有它，你可以创建可视化，展示模型参数，并深入挖掘其他与模型相关的信息。你可以与团队分享，或保存以备后用。

为任何模型创建解释器非常简单，如在这个使用*sklearn*的示例中所示！

```py
import dalex as dx
import pandas as pd
from sklearn.ensemble import RandomForestClassifier
from sklearn.preprocessing import LabelEncoder

data = dx.datasets.load_titanic()
le = preprocessing.LabelEncoder()
for feature in ['gender', 'class', 'embarked']:
	data[feature] = le.fit_transform(data[feature])

X = data.drop(columns='survived')
y = data.survived

classifier = RandomForestClassifier()
classifier.fit(X, y)

exp = dx.Explainer(classifier, X, y, label = "Titanic Random Forest")
```

### **模型解释（局部解释）**

当你想理解**为何你的模型做出特定预测**时，局部解释是你最好的朋友。

一切从预测开始，向下移动到上面金字塔的左半部分，你可以探索和理解发生了什么。

DALEX提供了一系列方法，展示每个变量的局部影响：

+   [SHAP](https://github.com/slundberg/shap): 使用经典的Shapley值计算特征对模型预测的贡献

+   [Break Down](https://pbiecek.github.io/breakDown/): 将预测分解成可以归因于每个变量的部分，使用所谓的“贪婪解释”

+   [Break Down with interactions](https://pbiecek.github.io/breakDown/reference/break_down.html): 扩展“贪婪解释”以考虑特征交互

向下移动金字塔，局部解释的下一个关键部分是**理解模型对特征值变化的敏感性**。

在DALEX中有一种简单的方法来绘制这些信息：

+   [Ceteris Paribus](https://github.com/pbiecek/ceterisParibus): 显示模型预测的变化，仅允许单个变量的差异，同时保持其他变量不变

继续我们在泰坦尼克数据集上创建的随机森林模型示例，我们可以轻松创建上述提到的图。

```py
observation = pd.DataFrame({'gender': ['male'],
                   	    'age': [25],
                   	    'class': ['1st'],
                   	    'embarked': ['Southampton'],
                       	    'fare': [72],
                   	    'sibsp': [0],
                   	    'parch': 0},
                  	    index = ['John'])

# Variable influence plots - Break Down & SHAP
bd = exp.predict_parts(observation , type='break_down')
bd_inter = exp.predict_parts(observation, type='break_down_interactions')
bd.plot(bd_inter)

shap = exp.predict_parts(observation, type = 'shap', B = 10)
shap.plot(max_vars=5)

# Ceteris Paribus plots
cp = exp.predict_profile(observation)
cp.plot(variable_type = "numerical")
cp.plot(variable_type = "categorical")
```

![本地解释 dalex](../Images/af80b569c95394071aa04cf71a65a0de.png)

### **模型理解（全局解释）**

当你想了解**在模型做出决策时哪些特征通常重要**时，你应查看全局解释。

为了从全局层面理解模型，DALEX 提供了变量重要性图。变量重要性图，特别是[置换特征重要性](https://christophm.github.io/interpretable-ml-book/feature-importance.html)，使用户能够了解每个变量对整体模型的影响，并区分出最重要的变量。

这些可视化可以被看作是 SHAP 和 Break Down 图的全局等效物，它们描绘了单个观察的类似信息。

在数据集层面，下降到金字塔下，有一些技术，如部分依赖图和累计局部依赖图，让你**可视化模型对选定变量反应的方式**。

现在让我们为示例创建一些全局解释。

```py
# Variable importance

vi = exp.model_parts()
vi.plot(max_vars=5)

# Partial and Accumulated Dependence Profiles

pdp_num = exp.model_profile(type = 'partial')
ale_num = exp.model_profile(type = 'accumulated')

pdp_num.plot(ale_num)

pdp_cat = exp.model_profile(type = 'partial', 
variable_type='categorical',
variables = ["gender","class"])
ale_cat = exp.model_profile(type = 'accumulated',
          variable_type='categorical',
          variables = ["gender","class"])

ale_cat.plot(pdp_cat)
```

![全局解释 dalex](../Images/af19a0391f6e9c8dde1f156b86505d31.png)

### **可重复使用且有组织的解释对象**

一个干净、结构化且易于使用的 XAI 可视化集合很好，但 DALEX 还有更多功能。

将你的模型打包在**DALEX 解释器**中，提供了一个**可重复使用且有组织的存储和版本控制**任何你进行的机器学习**模型**工作的方式。

使用 DALEX 创建的解释对象包含：

+   需要解释的模型，

+   模型名称和类别，

+   任务类型，

+   用于计算解释的数据，

+   针对这些数据的模型预测，

+   预测函数，

+   模型残差，

+   对观察的采样权重，

+   额外的模型信息（包、版本等）

将所有这些信息存储在一个对象中，使得创建本地和全局解释变得容易（正如我们之前所见）。

它还使得在模型开发的每个阶段，审查、共享和比较模型及解释成为可能。

### 使用 Neptune 进行实验和模型版本控制

在理想情况下，你的所有机器学习模型和实验都应以与版本化软件项目相同的方式进行版本控制。

不幸的是，要跟踪你的 ML 项目，你需要的远远不止于将代码提交到 Github。

简而言之，**正确版本化机器学习模型** 你应当跟踪：

+   代码、笔记本和配置文件

+   环境

+   参数

+   数据集

+   模型文件

+   结果如评估指标、性能图表或预测

有些内容与 .git 很匹配（代码、环境配置），但其他的则不太合适。

Neptune 通过让你记录所有你认为重要的内容，使得跟踪这些内容变得简单。

你只需在脚本中添加几行：

```py
import neptune
from neptunecontrib.api import *
from neptunecontrib.versioning.data import *

neptune.init('YOU/YOUR_PROJECT')

neptune.create_experiment(
          params={'lr': 0.01, 'depth': 30, 'epoch_nr': 10}, # parameters
          upload_source_files=['**/*.py', # scripts
                               'requirements.yaml']) # environment
log_data_version('/path/to/dataset') # data version
#
# your training logic
#
neptune.log_metric('test_auc', 0.82) # metrics
log_chart('ROC curve', fig) # performance charts
log_pickle('model.pkl', clf) # model file
```

每次你运行的实验或模型训练都会被版本控制，并在 Neptune 应用程序（和数据库 ????）中等待你。

![图](../Images/5c1627d06e0e3712b005925b22acce45.png)

[在 Neptune 中查看](https://ui.neptune.ai/o/shared/org/dalex-integration/e/DAL-79/details)

你的团队可以访问所有实验和模型，比对结果，快速找到信息。

你可能在想：“好的，很棒，我的模型已经版本化了，但”：

+   如果我想在模型训练后的几周或几个月后调试它怎么办？

+   如果我想查看每次实验运行的预测解释或变量重要性怎么办？

+   如果有人让我检查这个模型是否存在不公平偏见，而我没有训练它的代码或数据怎么办？

我听到了，这就是 DALEX 集成发挥作用的地方！

### DALEX + Neptune = 版本化和可解释的模型

为什么不让你的 DALEX **解释器在每个实验中都被记录和版本控制**，并使用交互式解释图表在一个友好的用户界面中呈现，方便与任何你想分享的人共享呢？

确实，为什么不呢！

使用 Neptune-DALEX 集成，你可以以额外 3 行代码的成本获得所有这些。

此外，还有一些非常实际的好处：

+   你可以 **审查** 其他人创建的模型，并轻松分享你的模型

+   你可以 **比较** 任何创建的模型的行为

+   你可以 **追踪和审计每个模型** 以发现不希望有的偏见和其他问题

+   你可以 **调试** 和比较那些缺少训练数据、代码或参数的模型

好的，这听起来很酷，但它实际上是如何工作的呢？

让我们现在深入了解一下。

### **本地解释的版本控制**

要记录本地模型解释，你只需：

+   创建一个观察向量

+   创建你的 DALEX 解释器对象

+   将它们传递给 `log_local_explanations` 函数来自 `neptunecontrib`

```py
from neptunecontrib.api import log_local_explanations

observation = pd.DataFrame({'gender': ['male'],
                   	    'age': [25],
                   	    'class': ['1st'],
                   	    'embarked': ['Southampton'],
                       	    'fare': [72],
                   	    'sibsp': [0],
                   	    'parch': 0},
                  	    index = ['John'])

log_local_explanations(expl, observation)
```

交互式解释图表将在 Neptune 应用的“Artifacts”部分等待你：

![图](../Images/354524bc9f4a79a229dbf7c8eb896a22.png)

[在 Neptune 中查看](https://ui.neptune.ai/shared/dalex-integration/e/DAL-78/artifacts?path=charts%2F&file=SHAP.html)

以下图表被创建：

+   变量重要性

+   部分依赖（如果指定了数值特征）

+   累积依赖（如果指定了类别特征）

### **全局解释的版本控制**

对于全局模型解释，更简单：

+   创建你的 DALEX 解释器对象

+   将其传递给 `log_global_explanations` 函数来自 `neptunecontrib`

+   （可选）指定你希望绘制的类别特征

```py
from neptunecontrib.api import log_global_explanations

log_global_explanations(expl, categorical_features=["gender", "class"])
```

就是这样。现在你可以前往“Artifacts”部分，找到你的本地解释图表：

![图](../Images/8a1ad29846aabf8b31bcc54d6e62aaa6.png)

[在 Neptune 中查看](https://ui.neptune.ai/o/shared/org/dalex-integration/e/DAL-78/artifacts?path=charts%2F&file=Variable%20Importance.html)

以下图表被创建：

+   细分，

+   通过交互进行细分，

+   shap

+   数值变量的 ceteris paribus，

+   类别变量的 ceteris paribus

### **版本化解释器对象**

但如果你真的想对解释进行版本控制，你应该 **对解释器对象本身进行版本控制**。

保存它的好处是什么？：

+   你总是可以在之后创建它的可视化表示

+   你可以在表格格式中深入了解细节

+   你可以随意使用它（即使你目前不知道怎么做????）

并且这非常简单：

```py
from neptunecontrib.api import log_explainer

log_explainer('explainer.pkl', expl)
```

你可能会想：“我还可以如何使用解释器对象？”

让我在接下来的部分展示给你。

### **获取并分析训练模型的解释**

首先，如果你将解释器记录到 Neptune，你可以直接将其提取到你的脚本或笔记本中：

```py
import neptune
from neptunecontrib.api import get_pickle

project = neptune.init(api_token='ANONYMOUS',
                       project_qualified_name='shared/dalex-integration')
experiment = project.get_experiments(id='DAL-68')[0]
explainer = get_pickle(filename='explainer.pkl', experiment=experiment)
```

现在你有了模型解释，你可以调试你的模型。

一个可能的场景是，你有一个观察点，但你的模型却失败得很惨。

你想弄清楚原因。

如果你保存了 DALEX 解释器对象，你可以：

+   创建本地解释并查看发生了什么。

+   检查特征变化如何影响结果。

![图示](../Images/8b46514b05aa3005b55ae67533d11601.png)

[在 Neptune 中查看](https://ui.neptune.ai/shared/dalex-integration/n/6b9d8213-9d1c-4a7d-a448-d2d9e29f7878/66389c83-b397-4ec6-a1fc-8b5847980fe5)

当然，你可以做更多的事情，特别是如果你想比较模型和解释的话。

让我们现在来深入了解一下！

### **比较模型和解释**

如果你想：

+   将当前模型想法与在生产中运行的模型进行比较？

+   查看去年的实验性想法是否在新收集的数据上表现更好？

拥有清晰的实验和模型结构以及存储它们的单一位置使得这变得非常简单。

你可以根据参数、数据版本或指标在 Neptune UI 中比较实验：

![图示](../Images/264e24ab0f479d0ae3aea82e2c3eec12.png)

[在 Neptune 中查看](https://ui.neptune.ai/o/shared/org/dalex-integration/compare?shortId=%5B%22DAL-78%22%2C%22DAL-77%22%2C%22DAL-76%22%2C%22DAL-75%22%2C%22DAL-72%22%5D&viewId=495b4a41-3424-4d01-9064-70be82716196)

你**只需两次点击即可查看差异**，并且可以通过一到两次点击深入查看所需的信息。

好吧，它在比较超参数和指标时确实非常有用，但解释器呢？

你可以进入每个实验并[查看交互式解释图表](https://ui.neptune.ai/o/shared/org/dalex-integration/e/DAL-78/artifacts?path=charts%2F&file=Break%20Down%20Interactions.html)以查看模型是否存在异常情况。

更好的是，Neptune 让你以编程方式访问所有记录的信息，包括模型解释器。

你可以**获取每个实验的解释器对象并进行比较**。只需使用来自 `neptunecontrib` 的 `get_pickle` 函数，然后使用 DALEX `.plot` 可视化多个解释器：

```py
experiments =project.get_experiments(id=['DAL-68','DAL-69','DAL-70','DAL-71'])

shaps = []
for exp in experiments:
	auc_score = exp.get_numeric_channels_values('auc')['auc'].tolist()[0]
	label = f'{exp.id} | AUC: {auc_score:.3f}'

	explainer_ = get_pickle(filename='explainer.pkl', experiment=exp)

	sh = explainer_.predict_parts(new_observation, type='shap', B = 10)
	sh.result.label = label
	shaps.append(sh)

shaps[0].plot(shaps[1:])
```

![图示](../Images/6169f3b94a4c2d57e5fa4cc80356ca92.png)

[在 Neptune 中查看](https://ui.neptune.ai/o/shared/org/dalex-integration/n/comparison-6b9d8213-9d1c-4a7d-a448-d2d9e29f7878/4bf30571-1ef1-4c63-9241-6d3b2cab65a7)

这就是 DALEX 图表的魅力。你可以传递多个解释器，它们将发挥魔力。

当然，你可以将以前训练的模型与当前正在工作的模型进行比较，以查看你是否在正确的方向上。只需将其附加到解释器列表中并传递给 `.plot` 方法。

### **最后思考**

好的，总结一下。

在这篇文章中，你了解了：

+   各种模型解释技术以及如何将这些解释与 DALEX 解释器打包

+   如何使用 Neptune 对机器学习模型和实验进行版本控制

+   如何为每次训练版本化模型解释器和交互式解释图表，并与 Neptune + DALEX 集成

+   如何比较和调试你训练的模型与解释器

希望有了这些信息，你的模型开发过程现在会更加有组织、可重复和可解释。

祝训练愉快！

[![](../Images/c89ec4635ace581269125d4065e3d508.png)](https://docs.neptune.ai/integrations/dalex.html)

[**Jakub Czakon**](https://www.linkedin.com/in/jakub-czakon-2b797b69/) 是 neptune.ai 的高级数据科学家。

[**Przemysław Biecek**](https://www.linkedin.com/in/pbiecek/) 是 MI2DataLab 的创始人，三星研发中心波兰的首席数据科学家。

**Adam Rydelek** 是 MI2DataLab 的研究工程师，华沙理工大学数据科学专业的学生。

[原文](https://neptune.ai/blog/explainable-and-reproducible-machine-learning-with-dalex-and-neptune)。已获许可转载。

**相关：**

+   [一种简单且可解释的二分类器性能度量](/2020/03/interpretable-performance-measure-binary-classifier.html)

+   [解释“黑箱”机器学习模型：SHAP 的实际应用](/2020/05/explaining-blackbox-machine-learning-models-practical-application-shap.html)

+   [可解释性第 3 部分：通过 LIME 和 SHAP 打开黑箱](/2019/12/interpretability-part-3-lime-shap.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 领域

* * *

### 更多相关话题

+   [构建一个可重复和可维护的数据科学项目：一本免费书…](https://www.kdnuggets.com/2022/08/free-book-build-reproducible-maintainable-data-science-project.html)

+   [可解释的预测和现在预测，采用最先进的深度学习技术…](https://www.kdnuggets.com/2021/12/sota-explainable-forecasting-and-nowcasting.html)

+   [可解释的 AI：揭示模型决策的 10 个 Python 库](https://www.kdnuggets.com/2023/01/explainable-ai-10-python-libraries-demystifying-decisions.html)

+   [弥合人类理解与机器学习之间的差距：……](https://www.kdnuggets.com/2023/06/closing-gap-human-understanding-machine-learning-explainable-ai-solution.html)

+   [开放助手：探索开放和协作的可能性……](https://www.kdnuggets.com/2023/04/open-assistant-explore-possibilities-open-collaborative-chatbot-development.html)

+   [12个VSCode技巧和窍门用于Python开发](https://www.kdnuggets.com/2023/05/12-vscode-tips-tricks-python-development.html)
