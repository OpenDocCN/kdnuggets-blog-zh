# Python 中的轻松 AutoML

> 原文：[https://www.kdnuggets.com/2021/04/easy-automl-python.html](https://www.kdnuggets.com/2021/04/easy-automl-python.html)

[评论](#comments)

**作者：[迪伦·谢里](https://www.linkedin.com/in/dylansherry/)，EvalML 团队负责人**

![使用 EvalML 在 Python 中轻松实现开源 AutoML](../Images/bc5d64a495185d869d33f09fede4dc34.png)

Alteryx 托管两个开源建模项目。

**[Featuretools](https://featuretools.alteryx.com/en/stable/)** 是一个执行自动特征工程的框架。它擅长将时间序列和关系数据集转换为机器学习的特征矩阵。

**[Compose](https://github.com/alteryx/compose)** 是一个用于自动化预测工程的工具。它允许你结构化预测问题并生成监督学习的标签。

我们看到 Featuretools 和 Compose 使用户能够轻松将多个表组合成转换和汇总的特征用于机器学习，并定义时间序列的监督学习用例。

我们接着问的问题是：接下来会发生什么？Featuretools 和 Compose 的用户如何以简单且灵活的方式构建机器学习模型？

我们很高兴宣布一个新的开源项目加入了 Alteryx 开源生态系统。**[EvalML](https://github.com/alteryx/evalml)** 是一个用于自动化机器学习（AutoML）和模型理解的库，使用 Python 编写。

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

![图](../Images/1319ed30bec469e3b669a661f0796aa2.png)

EvalML 的 AutoML 搜索实操

EvalML 提供了一个简单、统一的界面，用于构建机器学习模型，使用这些模型生成洞察并进行准确预测。EvalML 提供了一个统一 API 访问多个建模库。EvalML 支持各种监督学习问题类型，包括回归、二分类和多分类。自定义目标函数让用户能够直接按他们重视的标准来搜索模型。最重要的是，我们的目标是使 EvalML 稳定且高效，每个版本都有机器学习性能测试。

### EvalML 的亮点

**1. 简单统一的建模 API**

EvalML 减少了获得准确模型所需的工作量，从而节省时间和复杂性。

EvalML 由 AutoML 生成的管道包括开箱即用的预处理和特征工程步骤。一旦用户确定了他们希望建模的数据的目标列，EvalML 的 AutoML 将运行搜索算法来训练和评分一组模型，使用户能够从中选择一个或多个模型，并使用这些模型进行洞察驱动的分析或生成预测。

EvalML 设计时考虑了与 [Featuretools](https://featuretools.com/?__hstc=142826602.43730bd3179999cf11c14fbc47b01062.1613430843886.1613430843886.1613430843886.1&__hssc=142826602.1.1613430843886&__hsfp=264117289) 的良好兼容性，Featuretools 能够集成来自多个表的数据并生成特征，从而提升 ML 模型的性能，还兼容 [Compose](https://compose.alteryx.com/)，这是一个用于标签工程和时间序列聚合的工具。EvalML 用户可以轻松控制 EvalML 如何处理每个输入的特征，如数值特征、分类特征、文本、日期时间等。

![图](../Images/3dda01534486e96ea72c50211ccece51.png)

你可以将 Compose 和 Featuretools 与 EvalML 一起使用来构建机器学习模型。

EvalML 模型通过管道数据结构表示，管道由组件图组成。AutoML 对数据应用的每个操作都记录在管道中。这使得从选择模型到部署模型变得简单。同时，定义自定义组件、管道和目标在 EvalML 中也很容易，无论是用于 AutoML 还是作为独立元素。

**2\. 特定领域的目标函数**

EvalML 支持定义自定义目标函数，可以根据你的数据和领域进行调整。这使你能够明确模型在你的领域中有价值的标准，然后使用 AutoML 找到提供这些价值的模型。

自定义目标用于在 AutoML 排行榜上对模型进行排名，帮助引导 AutoML 搜索出影响最大的模型。自定义目标还将被 AutoML 用于调整二分类模型的分类阈值。

EvalML 文档提供了 [自定义目标示例](https://evalml.alteryx.com/en/v0.18.1/demos/lead_scoring.html) 及其有效使用方法。

**3\. 模型理解**

EvalML 提供了多种模型和工具用于模型理解。目前支持的功能包括特征重要性和置换重要性、部分依赖、精确率-召回率、混淆矩阵、ROC 曲线、预测解释和二分类器阈值优化。

![图](../Images/f690d8e7a632fbd3dc20f0e0028d4814.png)

来自 [EvalML 文档](https://evalml.alteryx.com/en/v0.18.1/user_guide/model_understanding.html#Partial-Dependence-Plots) 的部分依赖示例

**4\. 数据检查**

EvalML 的数据检查可以在建模之前捕捉到数据中的常见问题，防止它们导致模型质量问题或神秘的错误和堆栈跟踪。当前的数据检查包括检测 [目标泄漏](https://en.wikipedia.org/wiki/Leakage_(machine_learning)) 的简单方法，其中模型在训练期间获得了在预测时不可用的信息，检测无效数据类型、高类别不平衡、高空值列、常量列以及可能是 ID 且对建模无用的列。

![](../Images/fa9ebf5142aba2215d86fd04304a7515.png)

### 开始使用 EvalML

你可以通过访问 [我们的文档页面](http://evalml.alteryx.com/)来开始使用 EvalML，我们提供了 [安装说明](https://evalml.alteryx.com/en/stable/install.html)以及 [教程](https://evalml.alteryx.com/en/stable/tutorials.html)，这些教程展示了如何使用 EvalML 的示例，还有 [用户指南](https://evalml.alteryx.com/en/stable/user_guide.html)介绍 EvalML 的组件和核心概念， [API 参考](https://evalml.alteryx.com/en/stable/api_reference.html)等。EvalML 代码库位于 [https://github.com/alteryx/evalml](https://github.com/alteryx/evalml)。如需与团队联系，请查看我们的 [开源 Slack](https://join.slack.com/t/alteryx-oss/shared_invite/zt-6inxevps-RSbpr9lsACE1kObXz4rIuA)。我们正在积极维护代码库，并会对你提交的问题作出回应。

### 接下来是什么？

EvalML 有一个活跃的功能发展路线图，包括时间序列建模、AutoML 期间管道的并行评估、AutoML 算法的升级、新的模型类型和预处理步骤、模型调试和模型部署工具、异常检测支持等等。

想了解更多？如果你有兴趣在项目继续更新时获得通知，请花点时间关注这个博客，给 [我们的 GitHub 仓库](https://github.com/alteryx/evalml)点个星，敬请期待更多即将发布的功能和内容！

**个人简介: [Dylan Sherry](https://www.linkedin.com/in/dylansherry/)** 是一名软件工程师，领导团队开发 EvalML AutoML 包。Dylan 在自动建模技术方面拥有十年的经验。

[原文](https://innovation.alteryx.com/introducing-evalml/)。经允许转载。

**相关内容:**

+   [Uber 开源 Ludwig 的第三个版本，它是一个无代码机器学习平台](/2020/10/uber-open-source-ludwig-code-free-machine-learning-platform.html)

+   [高级超参数优化/调整算法](/2020/11/algorithms-for-advanced-hyper-parameter-optimization-tuning.html)

+   [使用 PyCaret 2.0 构建你自己的 AutoML](/2020/08/build-automl-pycaret.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 维护

* * *

### 更多相关主题

+   [无脑 AutoML 与 AutoXGB](https://www.kdnuggets.com/2022/02/no-brainer-automl-autoxgb.html)

+   [Nota AI 发布了 NetPresso 模型搜索的测试版，他们的…](https://www.kdnuggets.com/2022/04/nota-ai-releases-beta-version-netpresso-model-search-hardwareaware-automl-tool.html)

+   [2023 年你应该考虑的顶级 AutoML 框架](https://www.kdnuggets.com/2023/05/best-automl-frameworks-2023.html)

+   [Python 数据预处理的简单指南](https://www.kdnuggets.com/2020/07/easy-guide-data-preprocessing-python.html)

+   [用 Python 构建 AI 应用的 10 个简单步骤](https://www.kdnuggets.com/build-an-ai-application-with-python-in-10-easy-steps)

+   [用 Python 构建命令行应用的 7 个简单步骤](https://www.kdnuggets.com/build-a-command-line-app-with-python-in-7-easy-steps)
