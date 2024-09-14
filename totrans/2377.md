# 如何使用 MLFlow 打包和分发机器学习模型

> 原文：[https://www.kdnuggets.com/2022/08/package-distribute-machine-learning-models-mlflow.html](https://www.kdnuggets.com/2022/08/package-distribute-machine-learning-models-mlflow.html)

在机器学习模型生命周期开发的每个阶段，协作是一个基本活动。将机器学习模型从构思到部署的过程需要不同角色之间的参与和互动。此外，机器学习模型开发的性质涉及实验、工件和指标的跟踪、模型版本等，这要求有效的组织来正确维护机器学习模型生命周期。

幸运的是，有一些工具用于开发和维护模型生命周期，比如 [MLflow](https://mlflow.org/)。在本文中，我们将解析 MLflow、它的主要组件及其特性。我们还将提供示例，展示 MLflow 在实际中的工作方式。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

# 什么是 MLflow？

MLflow 是一个开源工具，旨在支持机器学习模型生命周期的每个阶段的开发、维护和协作。此外，MLflow 是一个框架无关的工具，因此任何机器学习/深度学习框架都可以迅速适应 MLflow 提出的生态系统。

MLflow 是一个提供跟踪指标、工件和元数据工具的平台。它还提供了用于打包、分发和部署模型及项目的标准格式。

MLflow 还提供了管理模型版本的工具。这些工具被封装在其四个主要组件中：

+   MLflow 跟踪，

+   MLflow 项目，

+   MLflow 模型和

+   MLflow 注册表。

## MLflow 跟踪

[MLflow 跟踪](https://mlflow.org/docs/latest/tracking.html) 是一个基于 API 的工具，用于记录指标、参数、模型版本、代码版本和文件。MLflow 跟踪集成了一个用户界面，用于可视化和管理工件、模型、文件等。

每个 MLflow 跟踪会话都在运行的概念下组织和管理。运行指的是代码的执行，其中工件日志被显式记录。

MLflow Tracking 允许你通过 MLflow 的 [Python](https://www.mlflow.org/docs/latest/python_api/index.html)、[R](https://www.mlflow.org/docs/latest/R-api.html)、[Java](https://www.mlflow.org/docs/latest/java_api/index.html) 和 [REST APIs](https://www.mlflow.org/docs/latest/rest-api.html) 生成运行记录。默认情况下，这些运行记录会存储在代码会话执行的目录中。不过，MLflow 也允许将工件存储在本地或远程服务器上。

## MLflow 模型

[MLflow Models](https://mlflow.org/docs/latest/models.html) 允许将机器学习模型打包为标准格式，以便通过 REST API、Microsoft Azure ML、Amazon SageMaker 或 Apache Spark 等不同服务直接使用。MLflow Models 规范的一个优点是打包支持多语言或多风味。

对于打包，MLflow 会生成一个包含两个文件的目录，一个是模型文件，另一个是指定模型打包和加载细节的文件。例如，下面的代码片段展示了一个 MLmodel 文件，其中指定了风味加载器以及定义环境的 `conda.yaml` 文件。

```py
artifact_path: model
flavors:
  python_function:
    env: conda.yaml
    loader_module: MLflow.sklearn
    model_path: model.pkl
    python_version: 3.8.2
  sklearn:
    pickled_model: model.pkl
    serialization_format: cloudpickle
    sklearn_version: 0.24.2
run_id: 39c46969dc7b4154b8408a8f5d0a97e9
utc_time_created: '2021-05-29 23:24:21.753565'
```

## MLflow 项目

[MLflow Projects](https://www.mlflow.org/docs/latest/projects.html) 提供了一个用于打包、共享和重用机器学习项目的标准格式。每个项目可以是一个远程仓库或本地目录。与 MLflow Models 不同，MLflow Projects 旨在实现机器学习项目的可移植性和分发。

一个 MLflow 项目由一个名为 `MLProject` 的 YAML 清单定义，其中公开了项目的规格。

模型实现的关键特性在 MLProject 文件中指定。这些特性包括：

+   模型接收的输入参数，

+   参数的数据类型，

+   执行模型的命令，以及

+   项目运行的环境。

下面的代码片段展示了一个 MLProject 文件的示例，其中实现的模型是一个决策树，其唯一参数是树的深度，默认值为 2。

```py
name: example-decision-tree
conda_env: conda.yaml
entry_points:
  main:
    parameters:
      tree_depth: {type: int, default: 2}
    command: "python main.py {tree_depth}"
```

同样，MLflow 提供了一个 CLI 来运行位于本地服务器或远程仓库中的项目。下面的代码片段展示了如何从本地服务器或远程仓库运行项目的示例：

```py
$ mlflow run model/example-decision-tree -P tree_depth=3
$ mlflow run git@github.com:FernandoLpz/MLflow-example.git -P tree_depth=3
```

在这两个示例中，环境将根据 `MLProject file` 规格生成。触发模型的命令将在命令行上传递的参数下执行。由于模型允许输入参数，这些参数通过 `-P` 标志进行分配。在这两个示例中，模型参数指的是决策树的最大深度。

默认情况下，如示例所示的运行将把工件存储在 `.mlruns` 目录中。

# 如何在 MLflow 服务器中存储工件？

实施 MLflow 时最常见的用例之一是使用 MLflow 服务器来记录指标和工件。MLflow 服务器负责管理由 MLflow 客户端生成的工件和文件。这些工件可以存储在不同的方案中，从文件目录到远程数据库。例如，要在本地运行 MLflow 服务器，我们输入：

```py
$ mlflow server
```

上述命令将通过 IP 地址 http://127.0.0.1:5000/ 启动一个 MLflow 服务。为了存储工件和指标，服务器的跟踪 URI 在客户端会话中定义。

在下面的代码片段中，我们将看到在 MLflow 服务器中实施工件存储的基本实现：

```py
import MLflow 
remote_server_uri = "http://127.0.0.1:5000"
MLflow.set_tracking_uri(remote_server_uri)
with MLflow.start_run():
   MLflow.log_param("test-param", 1)
   MLflow.log_metric("test-metric", 2)
```

`MLflow.set_tracking_uri ()` 命令设置服务器的位置。

# 如何在 MLflow 服务器中运行身份验证？

暴露一个没有身份验证的服务器可能是有风险的。因此，添加身份验证是很方便的。身份验证将取决于你部署服务器的生态系统：

+   在本地服务器上，只需添加基于 *用户* 和 *密码* 的基本身份验证即可，

+   在远程服务器上，凭据必须与相应的代理一起调整。

为了说明，我们来看一个应用基本身份验证（*用户名* 和 *密码*）的 MLflow 服务器示例。我们还将看到如何配置客户端以使用该服务器。

## 示例：MLflow 服务器身份验证

在这个例子中，我们通过 Nginx 反向代理对 MLflow 服务器应用基本的用户名和密码身份验证。

让我们从安装 Nginx 开始，我们可以通过以下方式进行：

```py
# For darwin based OS
$ brew install nginx

# For debian based OS
$ apt-get install nginx

# For redhat based OS
$ yum install nginx

```

对于 Windows 操作系统，你需要使用本机 Win32 API。请按照 [这里](https://nginx.org/en/docs/windows.html) 的详细说明操作。

安装完成后，我们将使用 `htpasswd` 命令生成一个具有相应密码的用户，如下所示：

```py
sudo htpasswd -c /usr/local/etc/nginx/.htpasswd MLflow-user
```

上述命令为 `.htpasswd` 文件中的用户 `mlflow-user` 生成凭据。之后，为了在创建的用户凭据下定义代理，将修改配置文件 `/usr/local/etc/nginx/nginx.conf`，该文件默认内容如下：

```py
server {
       listen       8080;
       server_name  localhost;
       # charset koi8-r;
       # access_log  logs/host.access.log  main;
       location / {
           root   html;
           index  index.html index.htm;
       }

```

生成的凭据应如下所示：

```py

server {
       # listen       8080;
       # server_name  localhost;

       # charset koi8-r;

       # access_log  logs/host.access.log  main;

       location / {
           proxy_pass http://localhost:5000;
           auth_basic "Restricted Content";
           auth_basic_user_file /usr/local/etc/nginx/.htpasswd;
       }

```

我们通过端口 5000 为 localhost 定义了一个身份验证代理。这是 MLflow 服务器默认部署的 IP 地址和端口号。当使用云提供商时，你必须配置实现所需的凭据和代理。现在按照以下代码片段初始化 MLflow 服务器：

```py
$ MLflow server --host localhost
```

当尝试在浏览器中访问 http://localhost 时，将会要求通过创建的用户名和密码进行身份验证。

![登录](../Images/75f7e933ab09a0f7fb49dc4d8a1bcbf1.png)

图 1\. 登录

输入凭据后，你将被引导到 MLflow 服务器 UI。

![MLflow 服务器 UI](../Images/ee8980fc7b14bbdc2fb10610bfc24070.png)

图 2\. MLflow 服务器 UI

要从客户端将数据存储到 MLflow 服务器，你需要：

+   定义将包含访问服务器凭证的环境变量，并

+   设置将存储工件的URI。

因此，对于凭证，我们将导出以下环境变量：

```py
$ export MLflow_TRACKING_USERNAME=MLflow-user
$ export MLflow_TRACKING_PASSWORD=MLflow-password
```

一旦定义了环境变量，你只需定义工件存储的服务器URI。

```py
import MLflow

# Define MLflow Server URI
remote_server_uri = "http://localhost"
MLflow.set_tracking_uri(remote_server_uri)

with MLflow.start_run():
   MLflow.log_param("test-param", 2332)
   MLflow.log_metric("test-metric", 1144)
```

执行上述代码片段时，我们可以看到测试指标和参数在服务器上的反映。

![从带有服务器身份验证的客户端服务中存储的指标和参数。](../Images/27b97fd9e33290425f2e3d54daec50bc.png)

图3\. 从带有服务器身份验证的客户端服务中存储的指标和参数。

# 如何注册一个MLflow模型？

在开发机器学习模型时，一个日常需求是保持模型版本的有序性。为此，MLflow提供了[MLflow注册表](https://mlflow.org/docs/latest/model-registry.html)。

MLflow注册表是一个扩展，有助于：

+   管理每个MLModel的版本并

+   记录每个模型在三个不同阶段的演变：归档、*暂存*和*生产。它是* 与git版本系统非常相似。

有四种注册模型的替代方案：

+   通过UI，

+   作为`MLflow.<flavor>.log_model()`的参数，

+   使用`MLflow.register_model()`方法或

+   使用`create_registered_model()`客户端API。

在以下示例中，使用`MLflow.<flavor>.log_model()`方法注册模型：

```py
with MLflow.start_run():

   model = DecisionTreeModel(max_depth=max_depth)
   model.load_data()
   model.train()
   model.evaluate()

   MLflow.log_param("tree_depth", max_depth)
   MLflow.log_metric("precision", model.precision)
   MLflow.log_metric("recall", model.recall)
   MLflow.log_metric("accuracy", model.accuracy)

   # Register the model
   MLflow.sklearn.log_model(model.tree, "MyModel-dt",      registered_model_name="Decision Tree")
```

如果这是一个新模型，MLFlow会将其初始化为*版本1*。如果模型已经有版本，它将被初始化为*版本2*（或后续版本）。

默认情况下，注册模型时分配的状态为无。要为注册的模型分配状态，我们可以按以下方式进行：

```py
client = MLflowClient()
client.transition_model_version_stage(
    name="Decision Tree",
    version=2,
    stage="Staging"
)
```

在上述代码片段中，*版本2*的*决策树*模型被分配到*暂存*状态。在服务器UI中，我们可以看到状态，如图4所示：

![注册的模型](../Images/a520e6d37fe1a626f8761a15d3e6ee51.png)

图4\. 注册的模型

为了服务模型，我们将使用MLflow CLI，为此我们只需服务器URI、模型名称和模型状态，如下所示：

```py
$ export MLflow_TRACKING_URI=http://localhost
$ mlflow models serve -m "models:/MyModel-dt/Production"
```

# 模型的服务和POST请求

```py
$ curl http://localhost/invocations -H 'Content-Type: application/json' -d '{"inputs": [[0.39797844703998664, 0.6739875109527594, 0.9455601866618499, 0.8668404460733665, 0.1589125298570211]}'
[1]%

```

在之前的代码片段中，向模型服务的地址发送了一个POST请求。请求中传递了一个包含五个元素的数组，这正是模型期望的推断输入数据。此情况下的预测结果为1。

然而，需要提到的是，MLFlow允许通过[签名](https://www.mlflow.org/docs/latest/_modules/mlflow/models/signature.html)的实现定义`MLmodel`文件的推断数据结构。同样，请求中传递的数据可以是不同类型的，这些类型可以在[这里](https://www.mlflow.org/docs/latest/_modules/mlflow/models/signature.html)查看。

上述示例的完整实现可以在此找到：[https://github.com/FernandoLpz/MLFlow-example](https://github.com/FernandoLpz/MLFlow-example)

# MLflow 插件

由于 MLflow 的框架无关特性，[MLflow 插件](https://mlflow.org/docs/latest/plugins.html#writing-your-own-mlflow-plugins) 应运而生。其主要功能是以适应不同框架的方式扩展 MLflow 的功能。

MLflow 插件允许对特定平台的部署和工件存储进行自定义和调整。

例如，针对平台特定部署有插件：

+   [MLflow-redisai](https://github.com/RedisAI/mlflow-redisai)：允许将从 MLFlow 创建和管理的模型部署到 [RedisAI](https://oss.redislabs.com/redisai/)。

+   [MLflow-torchserve](https://github.com/mlflow/mlflow-torchserve)：使 [PyTorch](https://pytorch.org/) 模型能够直接部署到 [TorchServe](https://github.com/pytorch/serve)。

+   [MLflow-algorithmia](https://github.com/algorithmiaio/mlflow-algorithmia)：允许将使用 MLFlow 创建和管理的模型部署到 [Algorithmia](https://algorithmia.com/) 基础设施中。

+   [MLflow-ray-serve](https://github.com/ray-project/mlflow-ray-serve)：支持将 MLFlow 模型部署到 [Ray](https://docs.ray.io/en/master/serve/) 基础设施中。

另一方面，对于 MLflow 项目的管理，我们有 [MLflow-yarn](https://github.com/criteo/mlflow-yarn)，一个用于在 [Hadoop](https://hadoop.apache.org/) / [Yarn](https://yarnpkg.com/) 支持下管理 MLProjects 的插件。对于 MLflow Tracking 的自定义，我们有 [MLflow-elasticsearchstore](https://github.com/criteo/mlflow-elasticsearchstore)，它允许在 [Elasticsearch](https://www.elastic.co/) 环境下管理 MLFlow Tracking 扩展。

同样，针对 *AWS* 和 *Azure* 的特定插件也可用。

+   [*MLflow.sagemaker*](https://www.mlflow.org/docs/latest/python_api/mlflow.sagemaker.html) 和

+   [*MLflow.azureml*](https://docs.microsoft.com/en-us/azure/databricks/applications/mlflow/)。

需要提到的是，MLflow 提供了根据需求创建和 [自定义插件](https://www.mlflow.org/docs/latest/plugins.html#writing-your-own-mlflow-plugins) 的功能。

# MLflow 与 Kubeflow

由于对开发和维护机器学习模型生命周期工具的需求不断增加，像 MLflow 和 [KubeFlow](https://www.kubeflow.org/) 这样的不同替代方案应运而生。

正如我们在本文中所见，MLflow 是一个允许在开发机器学习模型生命周期中协作的工具，主要集中在工件跟踪（MLflow Tracking）、协作、维护和项目版本控制上。

另一方面，还有 KubeFlow，它与 MLflow 类似，是一个用于开发机器学习模型的工具，但有一些具体的区别。

Kubeflow 是一个运行在 [Kubernetes](https://kubernetes.io/) 集群上的平台；也就是说，KubeFlow 利用 Kubernetes 的容器化特性。此外，KubeFlow 提供了如 [KubeFlow Pipelines](https://www.kubeflow.org/docs/components/pipelines/overview/pipelines-overview/) 等工具，旨在通过 SDK 扩展生成和自动化管道（DAGs）。

KubeFlow 还提供了 [Katib](https://www.kubeflow.org/docs/components/katib/overview/)，这是一个用于大规模优化超参数的工具，并提供来自 Jupyter notebooks 的管理和协作服务。

SEO链接：Kubernetes 和 Kubeflow 指南

具体来说，MLflow 是一个专注于机器学习项目开发管理和协作的工具。另一方面，Kubeflow 是一个专注于通过 Kubernetes 集群和容器使用开发、训练和部署模型的平台。

两个平台都提供了显著的优势，是开发、维护和部署机器学习模型的替代方案。然而，考虑到这些技术在开发团队中的使用、实施和集成的门槛是至关重要的。

由于 Kubeflow 与 Kubernetes 集群相关联用于实施和集成，因此建议有专家来管理这项技术。同样，开发和配置管道自动化也是一个挑战，可能在特定情况下对公司不利。

总结来说，MLflow 和 Kubeflow 是专注于机器学习模型生命周期特定阶段的平台。MLflow 是一个具有协作导向的工具，而 Kubeflow 更侧重于利用 Kubernetes 集群生成机器学习任务。然而，Kubeflow 需要 MLOps 部分的经验。需要了解 Kubernetes 中服务的部署，这可能是在尝试接触 Kubeflow 时需要考虑的问题。

**[Fernando López](https://www.linkedin.com/in/ferneutron/)** ([GitHub](https://github.com/FernandoLpz)) 是 Hitch 的数据科学负责人，领导一个数据科学团队，负责整个组织中人工智能模型的开发和部署，用于视频面试评估、候选人画像和评估管道。

### 更多相关话题

+   [用 llamafile 以 5 个简单步骤分发和运行 LLMs](https://www.kdnuggets.com/distribute-and-run-llms-with-llamafile-in-5-simple-steps)

+   [为什么机器学习模型会在沉默中消亡？](https://www.kdnuggets.com/2022/01/machine-learning-models-die-silence.html)

+   [在机器学习模型中处理稀疏特征](https://www.kdnuggets.com/2021/01/sparse-features-machine-learning-models.html)

+   [模型很少被部署：机器学习行业的普遍失败](https://www.kdnuggets.com/2022/01/models-rarely-deployed-industrywide-failure-machine-learning-leadership.html)

+   [机器学习模型的高级特征选择技术](https://www.kdnuggets.com/2023/06/advanced-feature-selection-techniques-machine-learning-models.html)

+   [5分钟解释的机器学习模型](https://www.kdnuggets.com/5-machine-learning-models-explained-in-5-minutes)
