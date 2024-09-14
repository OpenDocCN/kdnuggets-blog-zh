# 5 个最佳端到端开源 MLOps 工具

> 原文：[https://www.kdnuggets.com/5-best-end-to-end-open-source-mlops-tools](https://www.kdnuggets.com/5-best-end-to-end-open-source-mlops-tools)

![5 个最佳端到端开源 MLOps 工具封面图](../Images/ce3df6adb8e60f010f764bba4cb7a4e3.png)

图片作者

由于 [2024年你必须尝试的 7 个端到端 MLOps 平台](/7-end-to-end-mlops-platforms-you-must-try-in-2024) 博客的受欢迎程度，我正在编写另一份开源端到端 MLOPs 工具的列表。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT

* * *

开源工具提供了对数据和模型的隐私保护和更多控制。然而，你需要自己管理这些工具，部署它们，然后雇佣更多人来维护它们。此外，你还将负责安全性和任何服务中断。

简而言之，付费的 MLOps 平台和开源工具都有优缺点；你只需选择适合你的即可。

在这篇博客中，我们将了解 5 种端到端开源 MLOps 工具，用于训练、跟踪、部署和监控生产中的模型。

## 1\. Kubeflow

[kubeflow/kubeflow](https://github.com/kubeflow/kubeflow) 使所有机器学习操作在 Kubernetes 上变得简单、可移植和可扩展。它是一个云原生框架，允许你创建机器学习管道，并在生产环境中训练和部署模型。

![Kubeflow 仪表板 UI](../Images/b928c9d22dc772ae9adcd65bae4e3a1c.png)

图片来自 [Kubeflow](https://www.kubeflow.org/docs/components/central-dash/overview/)

Kubeflow 兼容于云服务（AWS、GCP、Azure）和自托管服务。它允许机器学习工程师集成各种 AI 框架用于训练、微调、调度和部署模型。此外，它提供了一个集中式仪表板，用于监控和管理管道、使用 Jupyter Notebook 编辑代码、实验跟踪、模型注册和工件存储。

## 2\. MLflow

[mlflow/mlflow](https://github.com/mlflow/mlflow) 通常用于实验跟踪和日志记录。然而，随着时间的推移，它已成为一个端到端的 MLOps 工具，适用于各种机器学习模型，包括 LLMs（大型语言模型）。

![MLflow 工作流图](../Images/5e9806e11f25fe5366467281e6ab93c6.png)

图片来自 [MLflow](https://mlflow.org/docs/latest/introduction/index.html)

MLFlow 有 6 个核心组件：

1.  **跟踪**：版本控制和存储参数、代码、指标和输出文件。它还提供交互式指标和参数可视化。

1.  **项目**：将数据科学源代码打包以便于重用和可重复性。

1.  **模型**：以标准格式存储机器学习模型和元数据，供下游工具后续使用。它还提供模型服务和部署选项。

1.  **模型注册表**：用于管理 MLflow 模型生命周期的集中式模型存储库。它提供版本控制、模型谱系、模型别名、模型标记和注释。

1.  **配方（管道）**：机器学习管道，让你快速训练高质量模型并将其部署到生产中。

1.  **大语言模型（LLMs）**：支持 LLMs 评估、提示工程、跟踪和部署。

你可以使用 CLI、Python、R、Java 和 REST API 管理整个机器学习生态系统。

## 3\. Metaflow

[Netflix/metaflow](https://github.com/Netflix/metaflow) 允许数据科学家和机器学习工程师快速构建和管理机器学习/AI 项目。

Metaflow 最初在 Netflix 开发，以提高数据科学家的生产力。它现在已经开源，所有人都可以受益。

![Metaflow Python 代码](../Images/6267bda44d033aca088d158f1c4f0fec.png)

图像来自 [Metaflow Docs](https://docs.metaflow.org/introduction/what-is-metaflow)

Metaflow 提供了一个统一的 API，用于数据管理、版本控制、编排、模型训练和部署，以及计算。它兼容主要的云服务提供商和机器学习框架。

## 4\. Seldon Core V2

[SeldonIO/seldon-core](https://github.com/SeldonIO/seldon-core) 是另一个流行的端到端 MLOps 工具，它让你打包、训练、部署和监控生产中的数千个机器学习模型。

![Seldon Core 工作流图](../Images/958a564e5334c773dbf5fc1f203824dd.png)

图像来自 [seldon-core](https://github.com/SeldonIO/seldon-core/)

**Seldon Core 的关键特性：**

1.  使用 Docker 在本地或在 Kubernetes 集群中部署模型。

1.  跟踪模型和系统指标。

1.  部署漂移和异常检测器与模型一起。

1.  支持大多数机器学习框架，如 TensorFlow、PyTorch、Scikit-Learn、ONNX。

1.  数据中心的 MLOps 方法。

1.  CLI 用于管理工作流、推理和调试。

1.  通过透明地部署多个模型来节省成本。

Seldon Core 将你的机器学习模型转换为 REST/GRPC 微服务。我可以轻松扩展和管理数千个机器学习模型，并提供额外的功能，如指标跟踪、请求日志、解释器、异常检测器、A/B 测试、金丝雀测试等。

## 5\. MLRun

[mlrun/mlrun](https://github.com/mlrun/mlrun) 框架允许轻松构建和管理生产中的机器学习应用程序。它简化了生产数据摄取、机器学习管道和在线应用程序，显著减少了工程工作量、生产时间和计算资源。

![MLRun 工作流程图](../Images/3ccfce9087762755c216820ea4df9121.png)

图片来源于 [MLRun](https://www.mlrun.org/)

MLRun 的核心组件：

1.  **项目管理**：一个集中管理各种项目资产的中心，如数据、函数、作业、工作流、机密等。

1.  **数据和工件**：连接各种数据源，管理元数据、目录，并对工件进行版本控制。

1.  **特征存储**：存储、准备、目录化和提供模型特征，以用于训练和部署。

1.  **批处理和工作流**：运行一个或多个函数，并收集、跟踪和比较它们的所有结果和工件。

1.  **实时服务管道**：快速部署可扩展的数据和机器学习管道。

1.  **实时监控**：监控数据、模型、资源和生产组件。

## 结论

与其为 MLOps 管道中的每个步骤使用不同的工具，不如只使用一个工具完成所有步骤。只需一个端到端的 MLOps 工具，你就可以训练、跟踪、存储、版本控制、部署和监控机器学习模型。你只需通过 Docker 或在云上本地部署它们即可。

使用开源工具适合需要更多控制和隐私的情况，但它带来了管理、更新、安全问题和停机时间的挑战。如果你是 MLOps 工程师的新手，我建议你先专注于开源工具，然后再转向如 Databricks、AWS、Iguazio 等托管服务。

希望你喜欢我关于 MLOps 的内容。如果你想阅读更多内容，请在评论中提及或通过 LinkedIn 联系我。

[](https://www.polywork.com/kingabzpro)****[Abid Ali Awan](https://www.polywork.com/kingabzpro)**** ([@1abidaliawan](https://www.linkedin.com/in/1abidaliawan)) 是一位认证的数据科学专业人士，热爱构建机器学习模型。目前，他专注于内容创作，并撰写有关机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是利用图神经网络构建一款 AI 产品，帮助那些面临心理健康问题的学生。

### 更多相关内容

+   [2024 年你必须尝试的 7 个端到端 MLOps 平台](https://www.kdnuggets.com/7-end-to-end-mlops-platforms-you-must-try-in-2024)

+   [闭源与开源图像注释](https://www.kdnuggets.com/closed-source-vs-open-source-image-annotation)

+   [端到端机器学习初学者指南](https://www.kdnuggets.com/2021/12/beginner-guide-end-end-machine-learning.html)

+   [将机器学习算法完整地部署到…](https://www.kdnuggets.com/2021/12/deployment-machine-learning-algorithm-live-production-environment.html)

+   [一个简单的 HuggingFace 端到端项目实现](https://www.kdnuggets.com/a-simple-to-implement-end-to-end-project-with-huggingface)

+   [使用开源工具生成合成时间序列数据](https://www.kdnuggets.com/2022/06/generate-synthetic-timeseries-data-opensource-tools.html)
