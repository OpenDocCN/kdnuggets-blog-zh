# 7 个最佳机器学习实验追踪工具

> 原文：[https://www.kdnuggets.com/2023/02/7-best-tools-machine-learning-experiment-tracking.html](https://www.kdnuggets.com/2023/02/7-best-tools-machine-learning-experiment-tracking.html)

![作者提供的图片](../Images/5ce6a07f0be2194017defafdb6289885.png)

作者提供的图片

5 年前，数据科学家和机器学习工程师通常将机器学习 (ML) 实验数据存储在电子表格、纸张或 Markdown 文件中。这些日子已经一去不复返了。如今，我们拥有高效、用户友好的实验跟踪平台。

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

除了轻量级实验跟踪外，这些平台还提供数据和模型版本控制、交互式仪表板、超参数优化、模型注册、机器学习管道，甚至模型服务。

在这篇文章中，我们将介绍 7 个用户友好、API 轻量级且具备交互式仪表板的顶级机器学习实验跟踪工具，以便查看和管理实验。

# 1\. MLflow Tracking

[MLflow Tracking](https://mlflow.org/docs/latest/tracking.html) 是开源库 MLflow 的一部分。该 API 用于记录实验、指标、参数、输出文件和代码版本。MLflow 跟踪还提供了基于 Web 的界面，让你可以可视化结果，并与参数和指标进行交互。

![作者提供的图片](../Images/9455f7b370d03537468311b6e3cd7e1e.png)

作者提供的图片

你可以使用 Python、R、Java 和 REST API 记录查询和实验。MLflow 还提供了与流行的机器学习框架（如 Scikit-learn、Keras、PyTorch、XGBoost 和 Spark）的集成。

# 2\. DVC

[数据版本控制 · DVC](https://dvc.org/) 是一个基于 Git 的开源工具，用于数据和模型版本控制、机器学习管道和机器学习实验跟踪。通过 [Studio · DVC](https://dvc.org/doc/studio)，你可以在提供 UI 的 Web 应用程序上记录实验，实现实时实验跟踪、可视化和协作。

![7 个最佳机器学习实验追踪工具](../Images/1c298af3e1945cc744bd877906fd6dff.png)

图片来源于 DVC

DVC 是一个终极工具，它自动化你的工作流程，存储和版本控制你的数据和模型，提供机器学习的 CI/CD，并简化你的机器学习模型部署。你可以使用 Python API、CLI、VSCode 插件和 Studio 访问和存储实验。

# 3\. ClearML

[ClearML 实验](https://clear.ml/clearml-experiment/) 跟踪并自动化与 ML 操作相关的所有事务。你可以用它记录和分享实验，版本化工件，并创建 ML 流水线。

![作者提供的图像](../Images/b0326ded147fe678e880ff5a9cd950e6.png)

图片来源于 ClearML

你可以可视化结果、比较、重现和管理各种实验。ClearML 实验与流行的 ML 库如 PyTorch、TensorFlow 和 XGBoost 集成。你可以获得一个免费的基础版本，涵盖了 MLOps 的所有核心流程。

# 4\. DAGsHub

[DAGsHub Logger](https://dagshub.com/docs/feature_guide/git_tracking/) 允许你使用 Python API 记录指标、超参数和输出。该平台还支持通过 MLflow 记录实验，这对于跟踪实时模型性能非常有用。

![作者提供的图像](../Images/1ef827818174f6567ea4e0d09fdaf3e3.png)

作者提供的图像

[DagsHub](https://dagshub.com/) 为你提供代码、数据和模型版本控制、实验跟踪、ML 流水线可视化、模型服务、模型监控和团队协作。它是一个完整的数据科学和机器学习项目工具。

# 5\. TensorBoard

[TensorBoard](https://www.tensorflow.org/tensorboard) 是我使用的第一个实验记录器。它简单且与 TensorFlow 包无缝集成。通过添加几行代码，你可以跟踪和可视化指标，如准确度、损失和 F1 分数。

![作者提供的图像](../Images/098f3503436745c0e9aa30c002e7d65b.png)

图片来源于文档 | TensorBoard

你可以通过 TensorBoard UI 可视化模型图、投影嵌入、查看图像、文本和音频数据，并管理你的实验。它是一个简单、快速而强大的工具。缺点是它仅与 TensorFlow 框架兼容。

# 6\. Comet ML

[Comet ML 实验跟踪](https://www.comet.com/site/products/ml-experiment-tracking/) 是一个免费的 ML 实验跟踪工具，服务于社区。你可以使用简单的 Python、Java 和 R API 管理实验，这些 API 适用于所有流行的机器学习框架，如 Keras、LightGBM、Transformers 和 Pytorch。

![作者提供的图像](../Images/9750b8390ff33219a3e47be0c011e605.png)

图片来源于 Comet ML

你可以在网页应用上记录实验并查看、比较和管理它们。网页用户界面包含项目、报告、代码、工件、模型和团队协作。此外，你还可以修改可视化，甚至使用 Python 可视化库创建自己的图表。

# 7\. Weights & Biases

[Weights & Biases](https://wandb.ai/site/experiment-tracking) 是一个以社区为中心的平台，用于跟踪实验、工件、交互式数据可视化、模型优化、模型注册和工作流程自动化。只需几行代码，你就可以开始跟踪、比较和可视化 ML 模型。

![作者提供的图像](../Images/8326e571920d5c318b2724584b75eca3.png)

图片来自 Weights & Biases

除了指标、参数和输出，你还可以监控CPU和GPU的利用率，实时调试性能，存储和版本控制高达100 GB的数据集，并分享你的报告。

# 结论

这些平台都很棒。它们各有优缺点。你可以使用它们展示你的投资组合、协作项目、跟踪实验，并简化流程以减少人工干预。

让我为你简化一下。

+   **MLflow:** 开源、免费使用，并且提供了所有基本的MLOps功能。

+   **DVC:** 适用于已经使用DVC进行数据和模型版本管理，并希望使用同一工具进行机器学习管道和实验跟踪的人群。

+   **CLearML:** 端到端可扩展的MLOps平台。

+   **DAGsHub:** 适用于团队协作、机器学习项目和实验跟踪。它也是一个端到端的机器学习平台。

+   **TensorBoard:** 如果你是TensorFlow的粉丝或长期用户，为了简便起见，使用它来跟踪实验。

+   **Comet ML:** 这是一种简单且互动的机器学习模型跟踪方式。

+   **Weights & Biases:** 以社区为中心，简单，推荐用于机器学习投资组合。

我希望你喜欢我的工作。如果你有任何问题或想给我关于MLOps工具的建议，请在评论中告诉我。我知道这个领域充满了机器学习跟踪工具，每个机器学习平台现在都提供了轻量级实验记录功能。

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan)) 是一位认证的数据科学专业人士，热衷于构建机器学习模型。目前，他专注于内容创作，并撰写关于机器学习和数据科学技术的技术博客。Abid拥有技术管理硕士学位和电信工程学士学位。他的愿景是利用图神经网络构建一个AI产品，帮助那些在心理健康方面挣扎的学生。

### 更多相关内容

+   [机器学习实验版本管理与跟踪](https://www.kdnuggets.com/2021/12/versioning-machine-learning-experiments-tracking.html)

+   [开发分析追踪的开放标准](https://www.kdnuggets.com/2022/07/developing-open-standard-analytics-tracking.html)

+   [数据科学中的实验设计重要性](https://www.kdnuggets.com/2022/08/importance-experiment-design-data-science.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [2023年最佳的8款Python图像处理工具](https://www.kdnuggets.com/2022/11/8-best-python-image-manipulation-tools.html)

+   [2023年7款最佳数据建模工具](https://www.kdnuggets.com/2023/03/list-7-best-data-modeling-tools-2023.html)
