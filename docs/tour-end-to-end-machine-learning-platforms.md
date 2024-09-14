# 《端到端机器学习平台概览》

> 原文：[https://www.kdnuggets.com/2020/07/tour-end-to-end-machine-learning-platforms.html](https://www.kdnuggets.com/2020/07/tour-end-to-end-machine-learning-platforms.html)

[评论](#comments)

**作者：[Ian Hellström](https://databaseline.tech/)，机器学习工程师**

机器学习 (ML) 被称为 [技术债务的高利贷](https://research.google/pubs/pub43146/)。虽然启动一个足够好的模型以解决特定业务问题相对容易，但要使该模型在生产环境中正常工作，能够处理杂乱的、不断变化的数据语义和关系，以及演变的架构，并且做到自动化和可靠，这是完全不同的挑战。如果你有兴趣了解一些知名的机器学习平台，你来对地方了！

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 领域

* * *

实际上，机器学习生产系统中的代码中只有 5% 是模型本身。将一组机器学习解决方案转变为端到端机器学习平台的是一种架构，它拥抱了旨在加快建模、自动化部署，并确保生产中的可扩展性和可靠性的技术。我之前谈过 [精益 D/MLOps](https://databaseline.tech/lean-dml-operations/)，数据和机器学习操作，因为没有数据的机器学习操作毫无意义，所以端到端机器学习平台需要一种整体方法。

CI/CD 基金会推出了一个 [MLOps 特殊兴趣小组 (SIG)](https://cd.foundation/blog/2020/02/11/announcing-the-cd-foundation-mlops-sig/)。他们为端到端机器学习平台确定的步骤如下图所示：

[![CI/CD 基金会 MLOps](../Images/7aa2ad97dc6bb2a1e398566a2c13e7b1.png)](https://databaseline.tech/images/2020-02-21-ml-cicd-mlops-sig.png)

它掩盖了一些不容忽视的细节。例如，服务可能需要不同的技术，具体取决于是否实时进行。可扩展的解决方案通常将模型放在一个容器中，该容器在许多机器的服务集群中运行，并由负载均衡器托管。因此，上述图中的一个框并不意味着实际平台的单一步骤、容器或组件。这不是对图像的批评，而是警告：看似简单的事情在实践中可能并不那么容易。

图表中缺少模型（配置）管理。你可以考虑版本控制、实验管理、运行时统计信息、训练、测试和验证数据集的数据来源跟踪、重训模型的能力（无论是从头开始还是从模型快照增量训练）、超参数值、准确性指标等。

另一个未列出的关键方面是能够检查模型是否存在偏差，例如，通过不同维度切片模型的关键性能指标。许多公司也需要能够热交换模型或并行运行多个模型。前者很重要，以免用户的请求在模型在后台更新时进入空白状态。后者对A/B测试或模型验证至关重要。

从CI/CD的另一个角度可以在[这里](https://martinfowler.com/articles/cd4ml.html)找到。它提到了版本控制数据和代码的必要性，这常常被忽视。

### Google: TFX

Google 开发[TensorFlow eXtended (TFX)](https://databaseline.tech/a-tour-of-end-to-end-ml-platforms/(https://www.kdd.org/kdd2017/papers/view/tfx-a-tensorflow-based-production-scale-machine-learning-platform))的主要动机是将机器学习模型从几个月的生产周期缩短到几周。他们的工程师和科学家面临挑战，因为“当机器学习需要在生产中部署时，实际工作流变得更加复杂。”

[![TensorFlow eXtended (TFX)](../Images/384435a75222fc795560a084c10898e8.png)](https://databaseline.tech/images/2020-02-21-ml-tfx.png)

TensorFlow 和[TFX](https://www.tensorflow.org/tfx/)是免费提供的，尽管后者不如前者成熟，因为它是在2019年发布的，晚于Google推出其ML基础设施两年。

模型性能指标用于部署安全可服务的模型。因此，如果较新的模型表现不如现有模型，它不会被推送到生产环境。在 TFX 术语中，模型不会获得“祝福”。在 TFX 中，这整个过程是自动化的。

这是开源 TFX 组件的快速概览：

+   [ExampleGen](https://github.com/tensorflow/tfx/blob/master/docs/guide/examplegen.md) 处理并拆分输入数据集。

+   [StatisticsGen](https://github.com/tensorflow/tfx/blob/master/docs/guide/statsgen.md) 计算数据集的统计信息。

+   [SchemaGen](https://github.com/tensorflow/tfx/blob/master/docs/guide/schemagen.md)检查统计信息并创建数据模式。

+   [ExampleValidator](https://github.com/tensorflow/tfx/blob/master/docs/guide/exampleval.md)用于检测数据集中的异常和缺失值。

+   [Transform](https://github.com/tensorflow/tfx/blob/master/docs/guide/transform.md)对数据集进行特征工程处理。

+   [Trainer](https://github.com/tensorflow/tfx/blob/master/docs/guide/trainer.md)使用TensorFlow训练模型。

+   [Evaluator](https://github.com/tensorflow/tfx/blob/master/docs/guide/evaluator.md)分析训练结果。

+   [ModelValidator](https://github.com/tensorflow/tfx/blob/master/docs/guide/modelval.md)确保模型可以安全地进行服务。

+   [Pusher](https://github.com/tensorflow/tfx/blob/master/docs/guide/pusher.md)将模型部署到服务基础设施中。

+   [TensorFlow Serving](https://www.tensorflow.org/serving)是一个C++后台，用于服务TensorFlow的[SavedModel](https://www.tensorflow.org/guide/saved_model#save_and_restore_models)文件。

为了最小化训练/服务的偏差，TensorFlow Transform在计算图中“冻结”值，使得训练期间发现的相同值在服务时也会被使用。训练时的多个操作在服务时将变成一个固定值。

### Uber: Michelangelo

大约在2015年，Uber的机器学习工程师发现了[机器学习系统中的隐性技术债务](https://papers.nips.cc/paper/5656-hidden-technical-debt-in-machine-learning-systems)，或者说是机器学习领域中的“但它在我的机器上有效……” Uber建立了与ML模型集成的定制化系统，但在大型工程组织中，这种做法的扩展性并不好。[用他们自己的话说](https://eng.uber.com/michelangelo/)，

> 在创建和管理大规模训练和预测数据的过程中，没有系统来构建可靠、统一且可重复的管道。

这就是他们构建Michelangelo的原因。它依赖于Uber的事务性和日志数据数据湖，支持离线（批处理）和在线（流式）预测。对于离线预测，容器化的Spark作业生成批量预测；对于在线部署，模型在预测服务集群中进行服务，该集群通常由数百台机器组成，这些机器通过负载均衡器进行连接，客户将单独或批量的预测请求发送到这些机器。

与模型管理相关的元数据（例如，训练器的运行时统计信息、模型配置、源流、特征的分布和相对重要性、模型评估指标、标准评估图表、学习的参数值以及总结统计信息）都为每次实验存储。

Michelangelo可以在同一个服务容器中部署多个模型，这使得从旧版本到新版本的安全过渡以及模型的并行A/B测试成为可能。

[![Uber 的 Michelangelo: 在线 vs 离线](../Images/dc66920d446d06b5a08ba07f79c78628.png)](https://databaseline.tech/images/2020-02-21-ml-michelangelo-v1.png)

Michelangelo 的原始版本不支持深度学习在 GPU 上训练的需求，但团队在这段时间里解决了这个问题。当前平台 ([current platform](https://eng.uber.com/michelangelo-model-representation/)) 使用 Spark 的 ML 管道序列化，但增加了一个用于在线服务的附加接口，该接口提供了一个轻量级的单示例（在线）评分方法，能够处理严格的服务级别协议，例如欺诈检测和预防。它通过绕过 Spark SQL 的 Catalyst 优化器的开销来实现这一点。

[![Uber 的 Michelangelo: 训练 vs 服务](../Images/f5d95859749ca800973a084799abdbaf.png)](https://databaseline.tech/images/2020-02-21-ml-michelangelo-v2.png)

值得注意的是，Google 和 Uber 都构建了内部协议缓冲解析器和服务表示，避免了默认实现中的瓶颈。

### Airbnb: Bighead

Airbnb 在 2016/2017 年建立了自己的 ML 基础设施团队，原因类似。首先，他们在生产中只有少数几个模型，但构建每个模型可能需要长达三个月。其次，模型之间没有一致性。第三，在线预测和离线预测之间存在很大差异。[Bighead](https://www.slideshare.net/databricks/bighead-airbnbs-endtoend-machine-learning-platform-with-krishna-puttaswamy-and-andrew-hoh) 是他们努力的成果：

[![Airbnb 的 Bighead](../Images/7ab8003ae8a7ebcfe1ca27697597d3ae.png)](https://databaseline.tech/images/2020-02-21-ml-bighead.png)

数据管理由内部工具 Zipline 处理。Redspot 是一个托管的、容器化的、多租户 Jupyter notebook 服务。Bighead 库用于数据转换和管道抽象，并包含常见模型框架的封装器。它通过转换保留元数据，因此用于追踪数据源。

Deep Thought 是一个用于在线预测的 REST API。Kubernetes 协调这些服务。对于离线预测，Airbnb 使用他们自己的 Automator。

### Netflix: Metaflow

Netflix 遇到了与前述公司类似的问题，这并不令人意外。他们的解决方案是[Metaflow](https://netflixtechblog.com/open-sourcing-metaflow-a-human-centric-framework-for-data-science-fa72e04a5d9)，这是一个面向数据科学家的 Python 库，处理[数据管理和模型训练，而非预测服务](https://blog.valohai.com/three-ways-to-categorize-machine-learning-platforms)。因此，它*不是*一个端到端的机器学习平台，可能更适合公司内部使用而非面向用户的场景。当然，它可以通过[Kubernetes](https://www.seldon.io/) 或[AWS SageMaker](https://aws.amazon.com/sagemaker/) 转变为一个完全成熟的解决方案。进一步的服务工具列表可以在[这里](https://github.com/EthicalML/awesome-production-machine-learning#model-deployment-and-orchestration-frameworks)找到。

[![Netflix' Metaflow](../Images/596f35c7ad288860e2252c986a5fa308.png)](https://databaseline.tech/images/2020-02-21-ml-metaflow.png)

数据科学家将他们的工作流编写为 DAG 步骤，类似于数据工程师在使用 Airflow 时的做法。与 Airflow 类似，你可以使用任何数据科学库，因为对 Metaflow 来说，只要是 Python 代码就会被执行。Metaflow 在后台分发处理和训练。所有代码和数据都会自动快照到 S3，以确保每个模型和实验都有版本历史。Pickle 是默认的模型序列化格式。

[开源版](https://docs.metaflow.org/)尚未内置[scheduler](https://docs.metaflow.org/introduction/what-is-metaflow)。它还鼓励用户‘主要依赖垂直扩展’，尽管他们可以使用 AWS SageMaker 实现水平扩展。它与 AWS 紧密耦合。

### Lyft: Flyte

Lyft 开源了他们的云原生平台[Flyte](https://flyte.org/)，其中数据和机器学习操作[汇聚](https://static.sched.com/hosted_files/kccncna19/7f/Flyte%20Kubecon.pdf)。这与我的[D/MLOps 哲学](https://databaseline.tech/lean-dml-operations/)一致——Data(Ops) 对 MLOps 的意义就像燃料对火箭的重要性：没有它，一切都无从谈起。

它建立在 Kubernetes 之上。由于 Lyft 在内部使用它，它可以扩展到至少 7,000 个独特工作流，每月超过 100,000 次执行，1 百万任务和 1,000 万个容器。

Flyte 中的所有实体都是不可变的，因此可以跟踪数据血缘、重现实验和回滚部署。重复任务可以利用任务缓存节省时间和成本。目前支持的任务包括[Python、Hive、Presto 和 Spark](https://lyft.github.io/flyte/user/tasktypes/index.html)以及[sidecars](https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/)。从源码来看，它似乎使用了 EKS。

[![Lyft's Flyte](../Images/1c7dd71ceb912bc09c0e0ef0565b8d13.png)](https://databaseline.tech/images/2020-02-21-ml-flyte.png)

他们还有一个名为[Amundsen](https://github.com/lyft/amundsen)的数据目录，这与Spotify的[Lexikon](https://labs.spotify.com/2020/02/27/how-we-improved-data-discovery-for-data-scientists-at-spotify/)相似。

### AWS、Azure、GCP等。

所有主要的[公有云](https://cloud.google.com/gartner-cloud-infrastructure-as-a-service/)供应商都有自己的机器学习平台解决方案，除了Oracle只为某些用例和行业提供[预制的基于ML的模型](https://www.oracle.com/artificial-intelligence/products.html)。

**AWS [SageMaker](https://aws.amazon.com/sagemaker/)** 是一个支持TensorFlow、Keras、PyTorch和MXNet的完整解决方案。借助[SageMaker Neo](https://aws.amazon.com/sagemaker/neo/)，可以将模型部署到云端或边缘设备上。它还内置了通过Amazon Mechanical Turk对存储在S3中的数据进行标注的功能。

[![AWS SageMaker](../Images/b1311ad9d1502bf934a5199251ab3728.png)](https://databaseline.tech/images/2020-02-21-ml-sagemaker.png)

Google没有托管平台，但借助[TFX、Kubeflow和**AI Platform**](https://cloud.google.com/ai-platform/)，可以将运行模型所需的所有组件连接在一起，包括CPU、GPU和TPU的支持，调整超参数，以及自动部署到生产环境。即使[Spotify](https://labs.spotify.com/2019/12/13/the-winding-road-to-better-machine-learning-infrastructure-through-tensorflow-extended-and-kubeflow/)也选择了TFX/Kubeflow-on-GCP选项。

除了TensorFlow，还有对[scikit-learn和XGBoost](https://cloud.google.com/ai-platform/training/docs?hl=en)的支持。自定义容器允许你使用任何框架，如[PyTorch](https://cloud.google.com/ai-platform/training/docs/custom-containers-training?hl=en)。目前还有一个类似SageMaker Ground Truth的[标注服务](https://cloud.google.com/ai-platform/data-labeling/docs/)处于测试阶段。

[![GCP AI Platform](../Images/93293af80aba9458cd660633ed54da87.png)](https://databaseline.tech/images/2020-02-21-ml-gcp.svg)

**[Azure 机器学习](https://azure.microsoft.com/en-us/services/machine-learning/)** 支持相当多的 [框架](https://azure.microsoft.com/en-us/services/machine-learning/#product-overview)，如 scikit-learn、Keras、PyTorch、XGBoost、TensorFlow 和 MXNet。它拥有自己的 [D/MLOps](https://azure.microsoft.com/en-us/services/machine-learning/mlops/#key-phases) 套件，配有大量图表。对那些更喜欢图形界面的人，提供了拖放式模型开发界面，但这伴随有各种 [隐患](https://databaseline.tech/the-problems-with-visual-programming-languages-in-data-engineering/)。模型和实验管理按照微软的预期，通过注册表完成。对于生产部署，使用 [Azure Kubernetes 服务](https://docs.microsoft.com/en-gb/azure/machine-learning/how-to-deploy-and-where#deploy-to-target)。控制滚动更新也是 [可能的](https://docs.microsoft.com/en-gb/azure/machine-learning/how-to-deploy-azure-kubernetes-service#deploy-models-to-aks-using-controlled-rollout-preview)。

**[IBM Watson 机器学习](https://www.ibm.com/cloud/machine-learning)** 提供了点击式机器学习选项 (SPSS) 和对一系列常见 [框架](https://dataplatform.cloud.ibm.com/docs/content/wsj/analyze-data/pm_service_supported_frameworks.html) 的支持。与其他主要玩家一样，模型可以在 CPU 或 GPU 上训练。 [超参数调优](https://dataplatform.cloud.ibm.com/docs/content/wsj/analyze-data/ml_dlaas_hpo.html?linkInPage=true) 也包含在内。该平台对数据和模型验证的细节不多，因为这些内容在其他 IBM 产品中可用。

尽管 **[阿里巴巴的 AI 机器学习平台](https://www.alibabacloud.com/product/machine-learning)** 在一个名称中展示了两个流行词，但并没有改善文档；关于 [最佳实践](https://www.alibabacloud.com/help/doc-detail/67395.htm?spm=a2c63.p38356.b99.39.62bb4809Vl31Cw) 的部分是用例而非推荐。

无论如何，它在 [拖放操作](https://www.alibabacloud.com/help/doc-detail/126312.htm) 上花费较多，特别是在数据管理和建模方面，这可能不利于自动化的端到端机器学习平台。该平台支持 [TensorFlow、MXNet 和 Caffe](https://www.alibabacloud.com/help/doc-detail/75093.htm) 等框架，但它也拥有大量的 [传统算法](https://www.alibabacloud.com/help/doc-detail/69688.htm)。它包括一个超参数调优器，这也是可以预期的。

模型序列化可以通过[PMML、TensorFlow 的 SavedModel 格式或 Caffe 格式](https://www.alibabacloud.com/help/doc-detail/126313.htm)完成。请注意，使用[PMML、ONNX](https://medium.com/analytics-and-data/overview-of-the-different-approaches-to-putting-machinelearning-ml-models-in-production-c699b34abf86)或[PFA 文件](http://dmg.org/pfa/)的评分引擎可以实现快速部署，但有可能引入训练/服务偏差，因为服务的模型是从不同格式加载的。

### 荣誉提及

**[H2O](https://www.h2o.ai/products/h2o/#overview)** 提供一个平台，包含数据处理、各种算法、交叉验证、超参数调优的网格搜索、特征排名和使用[POJO 或 MOJO](http://docs.h2o.ai/h2o/latest-stable/h2o-docs/productionizing.html#about-pojo-mojo)的模型序列化。

[![H2O.ai](../Images/e40354fba75e394968b47280930201b3.png)](https://databaseline.tech/images/2020-02-21-ml-h2o.png)

**[Valohai](https://valohai.com/product/)**—芬兰语为“光鲨”，真的！—是一个托管的机器学习平台。它可以运行在私有、公有、混合或多云环境中。

[![Valohai](../Images/1a813d3633a3373e19b7e920386b3a65.png)](https://databaseline.tech/images/2020-02-21-ml-valohai.png)

每个操作（或[执行](https://docs.valohai.com/core-concepts/executions/)）会对 Docker 镜像运行命令，因此与[Kubeflow](https://www.kubeflow.org/)非常相似。主要区别在于，Valohai 为你管理 Kubernetes 部署集群，而 Kubeflow 需要你自己完成。不过，Kubeflow 和 TFX 都带有一些 TensorFlow 相关的工具。使用 Valohai，你需要重用现有的 Docker 镜像或自制镜像，这意味着你可以使用任何机器学习框架，但这种自由必须权衡维护问题。

因此，可以依靠[Spark](https://spark.apache.org/docs/latest/ml-guide.html)、[Horovod](https://eng.uber.com/horovod/)、[TensorFlow](https://www.tensorflow.org/guide/distributed_training)等工具分发训练，或使用最适合你需求和基础设施的工具，但需要你自己填补空白。这也意味着你要负责确保数据转换的兼容性，以避免训练/服务偏差。请注意，它目前仅支持[对象存储](https://docs.valohai.com/core-concepts/data-stores/)。

**[Iguazio](https://www.iguazio.com/platform/)** 提到可以从笔记本或 IDE [秒级部署](https://www.iguazio.com/platform/)，尽管这似乎遗漏了最常见的场景：CI/CD 流水线或像 TFX 的[Pusher](https://www.tensorflow.org/tfx/guide/pusher)组件那样的平台。它使用 Kubeflow 进行工作流编排。

[![Iguazio](../Images/77a3e52aa3681a5ce0a14d3a1ac05315.png)](https://databaseline.tech/images/2020-02-21-ml-iguazio.png)

Iguazio 确实提供了一个具有统一 API 的特征存储，用于键值对和时间序列。许多现有的产品没有自己的特征存储，尽管大多数大型科技公司都有。特征存储是一个集中管理的地方，具有可重复使用的特征，可以跨模型共享，以加速模型开发。它可以在企业规模上自动化特征工程。例如，从时间戳中，你可以提取许多特征：年份、季节、月份、星期几、一天中的时间、是否是当地假期、自上次相关事件以来经过的时间（新颖性）、在固定窗口内某事件发生的频率等。

**[SwiftStack AI](https://www.swiftstack.com/solutions/ai)** 针对使用 NVIDIA GPU 进行高吞吐量深度学习，与 [RAPIDS](https://www.developer.nvidia.com/rapids) 套件配合使用。RAPIDS 提供了库，如 [cuML](https://github.com/rapidsai/cuml)，允许用户使用熟悉的 scikit-learn API，但从 GPU 加速中获益，支持的算法并且还有 [cuGraph](https://github.com/rapidsai/cugraph) 用于 GPU 驱动的图分析。

[![SwiftStack AI](../Images/d1e2f10cef9d510fbd15830b0cef3f06.png)](https://databaseline.tech/images/2020-02-21-ml-swiftstack.png)

**[AI Layer](https://algorithmia.com/serverless-ai-layer)** 是一个 [用于 D/MLOps 的 API](https://www.linkedin.com/pulse/ai-layer-diego-oppenheimer/)。它内置了对多种数据源、编程语言和机器学习框架的支持。

[![Algorithmia's AI Layer](../Images/d043542d81bd1ef6ee760721a157c0ee.png)](https://databaseline.tech/images/2020-02-21-ml-ai-layer.jpg)

**[MLflow](https://mlflow.org/)** 由 Databricks 支持，这解释了其与 Spark 的紧密集成。它提供了一个 [有限的部署选项集合](https://mlflow.org/docs/latest/models.html#built-in-deployment-tools)。例如，将模型导出为 PySpark 中的 [矢量化 UDF](https://spark.apache.org/docs/2.4.0/sql-pyspark-pandas-with-arrow.html) 对于实时系统并不最为理想，因为 Python UDF 存在 Python 运行时环境与 JVM 之间的通信开销。虽然由于使用了 Apache Arrow（一种内存中的列格式），这种开销不像标准 PySpark UDF 那样大，但它仍然是 [不可忽视的](https://medium.com/@QuantumBlack/spark-udf-deep-insights-in-performance-f0a95a4d8c62)。由于 Spark Streaming 是默认的数据摄取解决方案，使用 Spark 的微批处理模型可能仍然难以实现亚秒级延迟。

日志记录支持对于D/MLOps至关重要， [仍处于实验阶段](https://mlflow.org/docs/latest/tracking.html#automatic-logging)。根据文档，MLflow的重点并不是数据和模型验证，至少在平台本身中不是标准部分。提供托管版本的MLflow（在AWS和Azure上），提供 [更多功能](https://databricks.com/product/managed-mlflow)。

**D2iQ的 [KUDO for Kubeflow](https://d2iq.com/solutions/ksphere/kudo-kubeflow)** 是一个基于Kubeflow的面向企业客户的平台。与开源Kubeflow不同，它配备了Spark和 [Horovod](https://github.com/horovod/horovod)，以及为主要框架（TensorFlow、PyTorch和MXNet）预构建和全面测试的CPU/GPU镜像。数据科学家可以在笔记本中进行部署，无需切换上下文。默认情况下，它支持多租户。 [Istio](https://istio.io/) 和 [Dex](https://github.com/dexidp/dex) 集成以提供额外的安全性和认证。KUDO for Kubeflow 基于Konvoy，D2iQ的托管Kubernetes平台。它可以在云端、本地、混合环境或边缘运行。也支持气隙集群。

在Kubernetes术语中，KUDO for Kubeflow 是一个使用 [KUDO](https://kudo.dev/) 定义的操作符集合，KUDO是一个声明式工具包，可以使用YAML而不是Go来创建Kubernetes操作符。Cassandra、Elastic、Flink、Kafka、Redis等的Kubernetes统一声明操作符（KUDOs）都是 [开源的](https://github.com/kudobuilder/operators) ，可以与平台集成。更多细节请参阅 [我写的介绍文章](https://d2iq.com/blog/kudo-for-kubeflow-the-enterprise-machine-learning-platform)。

如果你想了解更多选项，包括可视化工作台，可以查看 [这里](https://heartbeat.fritz.ai/ai-and-machine-learning-landscape-part-4-end-to-end-ml-platforms-5adeac675d13) 或者查看 [Gartner的机器学习和数据科学平台魔力象限](https://www.analyticsvidhya.com/blog/2020/02/gartners-2020-magic-quadrant-for-data-science-and-machine-learning-tools-check-out-the-new-leaders/)。Facebook还发布了他们的平台 [FBLearner Flow](https://engineering.fb.com/core-data/introducing-fblearner-flow-facebook-s-ai-backbone/)（2016年），以及 [LinkedIn](https://engineering.linkedin.com/blog/2018/10/an-introduction-to-ai-at-linkedin)（2018年）和 [eBay](https://tech.ebayinc.com/engineering/ebays-transformation-to-a-modern-ai-platform/)（2019年）的详细信息。

**简介: [Ian Hellström](https://databaseline.tech/)** 曾在包括D2iQ、Spotify、Bosch和Sievo在内的多个公司担任数据和机器学习工程师。他是D2iQ企业机器学习平台KUDO for Kubeflow的产品经理。他目前居住在德国汉堡。

[原文](https://databaseline.tech/a-tour-of-end-to-end-ml-platforms/)。经许可转载。

**相关:**

+   [如何扩展 Scikit-learn 并使你的机器学习工作流程更加合理](/2019/10/extend-scikit-learn-bring-sanity-machine-learning-workflow.html)

+   [外行人数据科学指南 第3部分：数据科学工作流程](/2020/07/laymans-guide-data-science-workflow.html)

+   [将机器学习管道部署到云端，使用 Docker 容器](/2020/06/deploy-machine-learning-pipeline-cloud-docker.html)

### 更多相关内容

+   [2024年你必须尝试的7个端到端 MLOps 平台](https://www.kdnuggets.com/7-end-to-end-mlops-platforms-you-must-try-in-2024)

+   [初学者的端到端机器学习指南](https://www.kdnuggets.com/2021/12/beginner-guide-end-end-machine-learning.html)

+   [机器学习算法的全面端到端部署到生产环境中]（https://www.kdnuggets.com/2021/12/deployment-machine-learning-algorithm-live-production-environment.html）

+   [Python NLP 库的概览](https://www.kdnuggets.com/a-tour-of-python-nlp-libraries)

+   [5 个最佳端到端开源 MLOps 工具](https://www.kdnuggets.com/5-best-end-to-end-open-source-mlops-tools)

+   [一个简单的 HuggingFace 端到端项目实现指南](https://www.kdnuggets.com/a-simple-to-implement-end-to-end-project-with-huggingface)
