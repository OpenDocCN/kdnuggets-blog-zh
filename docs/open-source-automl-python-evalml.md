# 使用 EvalML 实现简单的开源 Python AutoML

> 原文：[`www.kdnuggets.com/2021/02/open-source-automl-python-evalml.html`](https://www.kdnuggets.com/2021/02/open-source-automl-python-evalml.html)

评论

**由 [Dylan Sherry](https://www.linkedin.com/in/dylansherry/)，EvalML 团队负责人**

![使用 EvalML 实现简单的开源 Python AutoML](img/bc5d64a495185d869d33f09fede4dc34.png)

Alteryx 托管两个开源建模项目。

**[Featuretools](https://featuretools.alteryx.com/en/stable/)** 是一个用于执行自动化特征工程的框架。它擅长将时间序列和关系数据集转化为机器学习的特征矩阵。

**[Compose](https://github.com/alteryx/compose)** 是一个用于自动预测工程的工具。它允许你构建预测问题并生成监督学习的标签。

我们已经看到 Featuretools 和 Compose 使用户能够轻松地将多个表结合成用于机器学习的转化和聚合特征，并定义时间序列监督机器学习用例。

我们接着问了一个问题：接下来会发生什么？Featuretools 和 Compose 的用户如何以简单灵活的方式构建机器学习模型？

我们很高兴地宣布一个新的开源项目加入了 Alteryx 开源生态系统。**[EvalML](https://github.com/alteryx/evalml)** 是一个用于自动化机器学习（AutoML）和模型理解的库，使用 Python 编写。

```py
import evalml

# obtain features, a target and a problem type for that target
X, y = evalml.demos.load_breast_cancer()
problem_type = 'binary'
X_train, X_test, y_train, y_test = evalml.preprocessing.split_data(
    X, y, problem_type=problem_type, test_size=.2)

# perform a search across multiple pipelines and hyperparameters
automl = AutoMLSearch(X=x, y=y, problem_type=problem_type)
automl.search()

# the best pipeline is already refitted on the entire training data
best_pipeline = automl.best_pipeline
best_pipeline.predict(X_test)
```

![图](img/1319ed30bec469e3b669a661f0796aa2.png)

EvalML 的 AutoML 搜索演示

EvalML 提供了一个简单的统一界面，用于构建机器学习模型，使用这些模型生成洞察并进行准确预测。EvalML 在同一个 API 下提供对多个建模库的访问。EvalML 支持多种监督机器学习问题类型，包括回归、二元分类和多类别分类。自定义目标函数让用户可以直接根据他们重视的内容来寻找模型。最重要的是，我们旨在使 EvalML 稳定高效，并在每次发布时进行机器学习性能测试。

### EvalML 的亮点

**1\. 简单统一的建模 API**

EvalML 减少了获得准确模型所需的努力，节省了时间和复杂性。

AutoML 生成的 EvalML 流水线包括开箱即用的预处理和特征工程步骤。一旦用户确定了他们想要建模的数据的目标列，EvalML 的 AutoML 将运行搜索算法来训练和评估一组模型，允许用户从这些模型中选择一个或多个模型，然后使用这些模型进行洞察驱动的分析或生成预测。

EvalML 设计为与 [Featuretools](https://featuretools.com/?__hstc=142826602.43730bd3179999cf11c14fbc47b01062.1613430843886.1613430843886.1613430843886.1&__hssc=142826602.1.1613430843886&__hsfp=264117289)良好兼容，它可以从多个表中集成数据并生成特征以增强 ML 模型，以及与 [Compose](https://compose.alteryx.com/)兼容，这是一个用于标签工程和时间序列汇总的工具。EvalML 用户可以轻松控制 EvalML 如何处理每个输入的特征，例如数值特征、分类特征、文本、日期时间等。

![图示](img/3dda01534486e96ea72c50211ccece51.png)

你可以在 EvalML 中使用 Compose 和 Featuretools 来构建机器学习模型。

EvalML 模型使用管道数据结构表示，由组件图组成。AutoML 对数据应用的每个操作都会记录在管道中。这使得从选择模型到部署模型变得容易。定义自定义组件、管道和目标在 EvalML 中也很简单，无论是用于 AutoML 还是作为独立元素。

**2\. 特定领域的目标函数**

EvalML 支持定义自定义目标函数，你可以根据你的数据和领域进行定制。这使你能够阐明在你的领域中使模型有价值的因素，然后使用 AutoML 找到能够提供这些价值的模型。

自定义目标用于在 AutoML 排行榜上对模型进行排名，无论是在搜索过程中还是之后。使用自定义目标将帮助引导 AutoML 搜索到具有最高影响力的模型。自定义目标还将被 AutoML 用于调整二分类模型的分类阈值。

EvalML 文档提供了[自定义目标的示例](https://evalml.alteryx.com/en/v0.18.1/demos/lead_scoring.html)以及如何有效使用它们。

**3\. 模型理解**

EvalML 提供了多种模型和工具用于模型理解。目前支持特征重要性和置换重要性、部分依赖、精确率-召回率、混淆矩阵、ROC 曲线、预测解释以及二分类器阈值优化。

![图示](img/f690d8e7a632fbd3dc20f0e0028d4814.png)

部分依赖的示例来自于 [EvalML 文档](https://evalml.alteryx.com/en/v0.18.1/user_guide/model_understanding.html#Partial-Dependence-Plots)。

**4\. 数据检查**

EvalML 的数据检查可以在建模之前捕捉数据中的常见问题，避免这些问题导致模型质量问题或神秘的错误和堆栈跟踪。当前的数据检查包括检测 [target leakage](https://en.wikipedia.org/wiki/Leakage_(machine_learning))（模型在训练期间接触到的在预测时不可用的信息）、无效数据类型、高类别不平衡、高空值列、常量列，以及可能是 ID 而对建模无用的列。

![](img/fa9ebf5142aba2215d86fd04304a7515.png)

### 使用 EvalML 入门

你可以通过访问 [我们的文档页面](http://evalml.alteryx.com/) 来开始使用 EvalML，我们提供了[安装说明](https://evalml.alteryx.com/en/stable/install.html)以及[tutorials](https://evalml.alteryx.com/en/stable/tutorials.html)，展示了如何使用 EvalML 的示例，[用户指南](https://evalml.alteryx.com/en/stable/user_guide.html) 介绍了 EvalML 的组件和核心概念，[API 参考](https://evalml.alteryx.com/en/stable/api_reference.html) 等更多内容。EvalML 的代码库在[`github.com/alteryx/evalml`](https://github.com/alteryx/evalml)。要与团队联系，请查看我们的[开源 Slack](https://join.slack.com/t/alteryx-oss/shared_invite/zt-6inxevps-RSbpr9lsACE1kObXz4rIuA)。我们积极参与代码库，并将回应你提出的任何问题。

### 下一步是什么？

EvalML 有一个活跃的功能路线图，包括时间序列建模、AutoML 期间的管道并行评估、AutoML 算法的升级、新模型类型和预处理步骤、模型调试和部署工具、异常检测支持等更多功能。

想了解更多？如果你对项目的更新感兴趣，请花一点时间关注这个博客，给[我们的 GitHub 仓库](https://github.com/alteryx/evalml)加个星，并关注更多即将推出的功能和内容！

**个人简介： [Dylan Sherry](https://www.linkedin.com/in/dylansherry/)** 是一位软件工程师，领导着开发 EvalML AutoML 包的团队。Dylan 拥有十年的自动建模技术经验。

[原文](https://innovation.alteryx.com/introducing-evalml/)。经授权转载。

**相关内容：**

+   Uber 开源了 Ludwig 的第三版，它是无代码机器学习平台

+   高级超参数优化/调整的算法

+   使用 PyCaret 2.0 构建自己的 AutoML

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在的 IT 组织

* * *

### 相关话题

+   [AutoXGB 的简单 AutoML](https://www.kdnuggets.com/2022/02/no-brainer-automl-autoxgb.html)

+   [Nota AI 发布了 NetPresso 模型搜索的测试版，他们的…](https://www.kdnuggets.com/2022/04/nota-ai-releases-beta-version-netpresso-model-search-hardwareaware-automl-tool.html)

+   [2023 年你应该考虑的顶级 AutoML 框架](https://www.kdnuggets.com/2023/05/best-automl-frameworks-2023.html)

+   [Python 数据预处理简易指南](https://www.kdnuggets.com/2020/07/easy-guide-data-preprocessing-python.html)

+   [用 Python 在 10 个简单步骤中构建 AI 应用程序](https://www.kdnuggets.com/build-an-ai-application-with-python-in-10-easy-steps)

+   [用 Python 在 7 个简单步骤中构建命令行应用程序](https://www.kdnuggets.com/build-a-command-line-app-with-python-in-7-easy-steps)
