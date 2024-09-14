# 以下是你在模型服务器中构建 ML 驱动服务时需要关注的要素

> 原文：[`www.kdnuggets.com/2020/09/model-server-build-ml-powered-services.html`](https://www.kdnuggets.com/2020/09/model-server-build-ml-powered-services.html)

评论

**由[Ben Lorica](https://twitter.com/bigdata)（协助组织[Ray Summit](https://events.linuxfoundation.org/ray-summit/)）和[Ion Stoica](https://people.eecs.berkeley.edu/~istoica/)（伯克利，Anyscale）** 

![](img/06fc71caf07273db0c45fb9d4799718d.png)

机器学习正被嵌入到涉及多种数据类型和数据源的应用程序中。这意味着来自不同背景的软件开发者需要在涉及机器学习的项目上进行合作。在我们[上一篇文章](https://anyscale.com/blog/five-key-features-for-a-machine-learning-platform/)中，我们列出了机器学习平台需要具备的关键特性，以满足当前和未来的工作负载。我们还描述了 MLOps，这是一套专注于机器学习生命周期生产化的实践。

![](img/05f7f399b092a645e55bd552010d4d55.png)

在这篇文章中，我们专注于模型服务器，这些软件是实时或离线机器学习服务的核心。用于服务机器学习模型有两种常见的方法。第一种方法是将模型评估嵌入到一个网页服务器（例如，Flask）中，作为一个专门用于预测服务的 API 服务端点。

第二种方法将模型评估任务转移到一个独立的服务上。这是初创公司活跃的领域，相关选项越来越多。提供的服务包括来自*云服务提供商*的服务（[SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/how-it-works-hosting.html)，[Azure](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-deploy-and-where)，[Google Cloud](https://cloud.google.com/ai-platform/prediction/docs/deploying-models)），*模型服务的开源项目*（[Ray Serve](https://docs.ray.io/en/master/serve/index.html)，Seldon，TorchServe，TensorFlow Serving 等），*专有软件*（SAS，Datatron，ModelOp 等），以及通常用一些通用框架编写的定制解决方案。

虽然机器学习可以用于一次性项目，但大多数开发者希望将机器学习嵌入到他们的产品和服务中。模型服务器是生产化机器学习的软件基础设施的重要组成部分，因此，公司需要仔细评估他们的选项。本文重点介绍了公司在选择模型服务器时应该关注的关键特性。

### 支持流行工具包

您的模型服务器可能与模型训练系统是分开的。选择一个能够使用多种流行工具生成的训练模型工件的模型服务器。开发者和机器学习工程师使用许多不同的库来构建模型，包括深度学习库（PyTorch、TensorFlow）、机器学习和统计学库（scikit-learn、XGBoost、SAS、[statsmodel](https://www.statsmodels.org/stable/index.html)）。模型构建者还继续使用各种编程语言。虽然 Python 已成为机器学习的主流语言，但其他语言如 R、Java、Scala、Julia、SAS 也仍然有许多用户。最近，许多公司实现了像 Databricks、Cloudera、Dataiku、Domino Data Lab 等的数据科学工作台。

### 模型部署及更多功能的图形用户界面

开发者可能会使用命令行界面，但企业用户将需要一个图形用户界面，该界面引导他们完成模型部署过程，并突出显示机器学习生命周期的不同阶段。随着部署过程的成熟，它们可能更多地转向脚本和自动化。具有用户界面的模型服务器包括[Seldon Deploy](https://www.youtube.com/watch?v=iTVY4GI1bhs)、[SAS Model Manager](https://www.sas.com/en_us/software/model-manager.html)、[Datatron](https://www.datatron.com/)等，针对企业用户。

### 操作简便，部署快捷，但性能高且可扩展性强

随着机器学习被嵌入到关键应用程序中，公司将需要低延迟的模型服务器来支持大规模的预测服务。像 Facebook 和 Google 这样的公司拥有提供实时响应的机器学习服务，[每天数十亿次](https://engineering.fb.com/ml-applications/transitioning-entirely-to-neural-machine-translation/)。虽然这些可能是极端案例，但许多公司也部署了像[推荐和个性化系统](https://www.sigarch.org/deep-learning-its-not-all-about-recognizing-cats-and-dogs/)这样的应用程序，每天与许多用户互动。借助 Ray Serve 等开源软件，公司现在可以访问可扩展到多台机器的低延迟模型服务器。

大多数模型服务器使用微服务架构，并可以通过 REST 或 gRPC API 访问。这使得将机器学习（“推荐系统”）与其他服务（“购物车”）集成更为容易。根据您的设置，您可能需要一个允许您在云端、本地或两者兼有的模型服务器。您的模型服务器必须参与自动扩展、资源管理和硬件配置等基础设施功能。

一些模型服务器增加了最近的创新，这些创新减少了复杂性，提高了性能，并提供了与其他服务集成的灵活选项。随着新 Tensor 数据类型的引入，RedisAI 支持*数据本地性*——这一功能使用户能够从他们喜欢的客户端获取和设置 Tensors，并“在数据所在的位置运行他们的 AI 模型。”Ray Serve [将模型评估逻辑更接近业务逻辑](https://www.youtube.com/watch?v=fABgQ5hA4qI&feature=youtu.be&t=1361)，通过给开发人员从 API 端点到模型评估再到 API 端点的端到端控制。此外，Ray Serve 易于操作，部署起来就像简单的 web 服务器一样简单。

### 包括测试、部署和推出的工具

一旦模型训练完成，它必须经过审查和测试，然后才能部署。Seldon Deploy、Datatron 和其他模型服务器具有一些有趣的功能，允许你通过单次预测或负载测试来测试模型。为了便于错误识别和测试，这些模型服务器还允许你上传测试数据并可视化测试预测。

一旦你的模型经过审查和测试，你的模型服务器应该能让你安全地提升或降级模型。其他流行的推出模式包括：

+   [Canary](https://rollout.io/blog/canary-deployment/)：一小部分请求发送到新的模型，而大部分请求则路由到现有的模型。

+   [Shadowing](https://www.getambassador.io/docs/latest/topics/using/shadowing/)：生产流量被复制到非生产环境服务中，以在实际运行前测试模型。

理想情况下，推出工具应该是完全自动化的，这样你的部署工具就可以与 CI/CD 或 MLOps 过程集成。

### 支持复杂的部署模式

随着你对机器学习的使用增加，你的模型服务器应该能够支持许多生产中的模型。你的模型服务器还应支持复杂的部署模式，包括同时部署多个模型。它应该支持各种模式，包括：

+   **A/B 测试**：一部分预测使用一个模型，其余的则使用另一个模型。

+   **集成模型**：将多个模型结合起来形成一个更强大的预测模型。

+   **级联**：如果基线模型产生低置信度的预测，流量将被路由到替代模型。另一个用例是细化：检测图片中是否有车，如果有，则将图片发送到一个读取车牌的模型。

+   **多臂老虎机**：一种强化学习形式，老虎机将流量分配到多个竞争的模型中。

### 开箱即用的度量和监控

机器学习模型可能会随着时间的推移而退化，因此重要的是要有系统来指示模型何时变得不准确或开始表现出偏差和其他意外行为。你的模型服务器应该生成性能、使用情况和其他自定义指标，这些指标可以被可视化和实时监控工具使用。一些模型服务器开始提供高级功能，包括异常检测和警报。甚至有初创公司（[Superwise](https://superwise.ai/)，[Arize](https://techcrunch.com/2020/02/18/tubemogul-execs-launch-arize-ai-for-ai-troublehsooting/)）专注于使用“机器学习来监控机器学习”。虽然这些目前是与模型服务器分开的专用工具，需要与模型服务器集成，但很可能一些模型服务器将把高级监控和可观测能力内置到他们的产品中。

### 与模型管理工具集成

随着你将更多模型部署到生产环境，你的模型服务器将需要与模型管理工具进行集成。这些工具有许多不同的标签——访问控制、模型目录、模型注册、模型治理仪表板——但本质上，它们为你提供了对过去和当前模型的 360 度全景视图。

因为模型需要定期检查，你的模型服务器应该与审计和重现模型的服务对接。*模型版本控制*现在已经成为标准，并且大多数我们检查过的模型服务器都具备这个功能。Datatron 提供了一个模型治理仪表板，用于审计表现不佳的模型。许多模型服务器提供*数据血缘*服务，记录请求的发送时间及模型输入和输出。调试和审计模型还需要对其关键驱动因素有深入的理解。Seldon Deploy 集成了一个用于模型检查和解释的[开源工具](https://github.com/SeldonIO/alibi)。

### 统一批量和在线评分

假设你更新了模型，或者你收到了一大量的新记录。在这两种情况下，你可能需要将模型应用于大型数据集。你将需要一个能够有效地以小批量处理大型数据集的模型服务器，并且提供低延迟的在线评分（例如，Ray Serve 支持批量和在线评分）。

### 总结

随着机器学习嵌入更多的软件应用程序，公司需要仔细选择他们的模型服务器。虽然[Ray Serve](https://docs.ray.io/en/master/serve/index.html) 是一个相对较新的开源模型服务器，但它已经具备了我们在这篇文章中列出的许多功能。Ray Serve 是一个可扩展、简单且灵活的工具，用于部署、操作和监控机器学习模型。正如我们在[上一篇文章](https://anyscale.com/blog/five-key-features-for-a-machine-learning-platform/)中提到的，我们相信 Ray 和 Ray Serve 将成为未来许多 ML 平台的基础。

[原文](https://anyscale.com/blog/heres-what-you-need-to-look-for-in-a-model-server-to-build-ml-powered-services/)。转载许可。

**简介：** [Ben Lorica](https://twitter.com/bigdata) 组织了 #SparkAISummit 和 #raysummit，并曾担任 Strataconf 和 OReillyAI 的程序主席。

**相关：**

+   [揭开 AI 基础设施堆栈的神秘面纱](https://www.kdnuggets.com/2020/05/demystifying-ai-infrastructure-stack.html)

+   [优化生产环境中机器学习 API 的响应时间](https://www.kdnuggets.com/2020/05/optimize-response-time-machine-learning-api-production.html)

+   [机器学习部署的软件接口](https://www.kdnuggets.com/2020/03/software-interfaces-machine-learning-deployment.html)

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

### 更多相关内容

+   [想利用数据技能解决全球问题吗？这里是…](https://www.kdnuggets.com/2022/04/jhu-want-data-skills-solve-global-problems.html)

+   [Kubernetes 中的高可用 SQL Server Docker 容器](https://www.kdnuggets.com/2022/04/high-availability-sql-server-docker-containers-kubernetes.html)

+   [如何通过 ML 模型可解释性加速 AI 采纳进程…](https://www.kdnuggets.com/2022/07/ml-model-explainability-accelerates-ai-adoption-journey-financial-services.html)

+   [KDnuggets™ 新闻 22:n03, 1 月 19 日：深入了解 13 个数据…](https://www.kdnuggets.com/2022/n03.html)

+   [合成数据社区的到来及其必要性](https://www.kdnuggets.com/2022/04/community-synthetic-data-need.html)

+   [构建供应链管道所需的 6 项数据科学技术](https://www.kdnuggets.com/2022/01/6-data-science-technologies-need-build-supply-chain-pipeline.html)
