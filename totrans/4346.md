# 生产机器学习监控：异常值、漂移、解释器与统计性能

> 原文：[https://www.kdnuggets.com/2020/12/production-machine-learning-monitoring-outliers-drift-explainers-statistical-performance.html](https://www.kdnuggets.com/2020/12/production-machine-learning-monitoring-outliers-drift-explainers-statistical-performance.html)

[评论](#comments)

**由[亚历杭德罗·索塞多](https://www.linkedin.com/in/axsaucedo/)，Seldon的工程总监**

![图](../Images/5adde1d0c003e4c846241add464f5b86.png)

作者提供的图像

> “机器学习模型的生命周期只有在生产环境中开始”

在本文中，我们展示了一个端到端的示例，展示了生产中机器学习模型监控的最佳实践、原则、模式和技术。我们将展示如何将标准的微服务监控技术适应于已部署的机器学习模型，以及更高级的范式，包括概念漂移、异常检测和人工智能解释。

我们将从头开始训练一个图像分类机器学习模型，将其作为微服务部署在Kubernetes中，并引入广泛的高级监控组件。这些监控组件将包括异常检测器、漂移检测器、AI解释器和指标服务器——我们将涵盖每个组件的底层架构模式，这些模式考虑了规模的需求，并设计为在数百或数千个异构机器学习模型中高效工作。

你还可以以视频形式查看这篇博客文章，该视频作为PyCon香港2020的主题演讲——主要的区别在于演讲使用了Iris Sklearn模型作为端到端示例，而不是CIFAR10 Tensorflow模型。

### 端到端机器学习监控示例

在本文中，我们展示了一个端到端的实际示例，涵盖了下面部分中概述的每个高级概念。

1.  复杂机器学习系统监控介绍

1.  CIFAR10 Tensorflow Renset32模型训练

1.  模型打包与部署

1.  性能监控

1.  事件基础设施监控

1.  统计监控

1.  异常检测监控

1.  概念漂移监控

1.  解释性监控

在本教程中，我们将使用以下开源框架：

+   [**Tensorflow**](https://github.com/tensorflow/tensorflow) — 广泛使用的机器学习框架。

+   [**Alibi Explain**](https://github.com/SeldonIO/alibi) — 白盒和黑盒机器学习模型解释库。

+   [**Albi Detect**](https://github.com/SeldonIO/alibi-detect) — 先进的机器学习监控算法，用于概念漂移、异常检测和对抗性检测。

+   [**Seldon Core**](https://github.com/SeldonIO/seldon-core/) — 机器学习模型的部署与编排及监控组件。

你可以在 [提供的 jupyter notebook](https://github.com/axsaucedo/seldon_experiments/blob/master/monitoring-talk/cifar10_example.ipynb) 中找到本文的完整代码，这将允许你运行模型监控生命周期中的所有相关步骤。

让我们开始吧。

### 1\. 复杂机器学习系统监控简介

生产机器学习的监控很困难，而且随着模型数量和高级监控组件的增加，复杂性呈指数级增长。这部分是因为生产机器学习系统与传统的软件微服务系统的差异——以下概述了一些关键差异。

![图像](../Images/0c0298d101c1be5bc129ca939d2ccf2c.png)

作者提供的图像

+   **专用硬件** — 优化的机器学习算法实现通常需要访问 GPU、更大的 RAM、专用的 TPU/FPGAs 以及其他动态变化的需求。这导致需要特定的配置，以确保这些专用硬件可以产生准确的使用指标，更重要的是，这些指标可以与相应的底层算法关联。

+   **复杂的依赖图** — 工具和基础数据涉及复杂的依赖关系，这些关系可能跨越复杂的图结构。这意味着处理单个数据点可能需要在多个跳跃之间进行状态评估，这可能会引入额外的领域特定抽象层，需要在可靠解读监控状态时考虑。

+   **合规要求** — 生产系统，特别是在高度监管的环境中，可能涉及复杂的审计政策、数据要求以及在每个执行阶段收集资源和文档的要求。有时，显示和分析的指标必须根据指定的政策限制给相关人员，这些政策的复杂性可能因用例而异。

+   **可重复性** — 除了这些复杂的技术要求外，还需要组件的可重复性，确保运行的组件可以在另一个时间点以相同的结果执行。对于监控系统，重要的是系统在设计时考虑到这一点，以便能够重新运行特定的机器学习执行，以重现特定的指标，无论是用于监控还是审计目的。

生产机器学习的构造涉及多阶段模型生命周期中的广泛复杂性。这包括实验、评分、超参数调整、服务、离线批处理、流处理等。每个阶段可能涉及不同的系统和各种异质工具。因此，确保我们不仅学习如何引入特定于模型的指标进行监控，而且要识别可以用于在规模上有效监控部署模型的更高级别的架构模式，这一点至关重要。这是我们在下面每个部分中将要覆盖的内容。

### 2\. CIFAR10 Tensorflow Resnet32模型训练

![Figure](../Images/e0fc7f608ee02bb318b6d800cfffeb2a.png)

来自开源的[CIFAR10数据集](https://www.cs.toronto.edu/~kriz/cifar.html)的图像

我们将使用直观的[**CIFAR10数据集**](https://www.cs.toronto.edu/~kriz/cifar.html)。这个数据集包含的图像可以被分类为10个类别之一。模型将以形状为32x32x3的数组作为输入，并以一个包含10个概率的数组作为输出，表示该图像属于哪个类别。

我们能够从Tensorflow数据集中加载数据——即：

10个类别包括：`cifar_classes = [“airplane”, “automobile”, “bird”, “cat”, “deer”, “dog”, “frog”, “horse”, “ship”, “truck”]`。

为了训练和部署我们的机器学习模型，我们将遵循下图所示的传统机器学习工作流程。我们将训练一个模型，然后可以将其导出和部署。

![Image for post](../Images/dc4efbfe55db9179d41c9b3cd37bb2ba.png)

我们将使用Tensorflow来训练这个模型，利用[Residual Network](https://towardsdatascience.com/an-overview-of-resnet-and-its-variants-5281e2f56035)，这无疑是最具突破性的架构之一，因为它使得训练多达数百甚至上千层的网络成为可能，并且性能良好。在这个教程中，我们将使用Resnet32实现，幸运的是，我们可以通过Alibi Detect包提供的工具使用它。

使用我的GPU，这个模型训练了大约5小时，幸运的是，我们可以使用通过Alibi Detect `fetch_tf_model`工具检索的预训练模型。

如果你仍然想训练CIFAR10 resnet32 tensorflow模型，你可以使用Alibi Detect包提供的辅助工具，如下所述，或者直接导入原始网络并自行训练。

我们现在可以在“未见数据”上测试训练好的模型。我们可以使用一个被分类为卡车的CIFAR10数据点来测试它。我们可以通过Matplotlib绘制数据点来查看。

![Image for post](../Images/c13d881447de9a1cb03b023f153b0927.png)

我们现在可以通过模型处理该数据点，你可以想象它应该被预测为“卡车”。

我们可以通过找到概率最高的索引来确定预测的类别，在这种情况下，它是`index 9`，概率为99%。从类别名称（例如`cifar_classes[ np.argmax( X_curr_pred )]`）可以看出，第9类是“卡车”。

### 3\. 打包与部署模型

我们将使用Seldon Core将模型部署到Kubernetes中，它提供了多种选项将我们的模型转换为一个完全成熟的微服务，暴露REST、GRPC和Kafka接口。

我们在使用Seldon Core部署模型时的选项包括 1) 使用[语言封装](https://docs.seldon.io/projects/seldon-core/en/latest/wrappers/language_wrappers.html)来部署我们的Python、Java、R等代码类，或 2) 使用[预打包模型服务器](https://docs.seldon.io/projects/seldon-core/en/latest/servers/overview.html)直接部署模型工件。在本教程中，我们将使用[Tensorflow预打包模型](https://docs.seldon.io/projects/seldon-core/en/latest/servers/tensorflow.html)服务器来部署我们之前使用的Resnet32模型。

这种方法将使我们能够利用Kubernetes的云原生架构，它通过水平可扩展的基础设施支持大规模微服务系统。在本教程中，我们将深入了解和利用应用于机器学习的云原生和微服务模式。

下面的图表总结了部署模型工件或代码本身的选项，以及我们可以选择部署单个模型或构建复杂的推理图的能力。

![帖子图片](../Images/a78e2d4a97e0799410efad8c75bdd0b3.png)

> 顺便提一下，您可以使用[KIND（Kubernetes in Docker）](https://github.com/kubernetes-sigs/kind)或[Minikube](https://github.com/kubernetes/minikube)等开发环境在Kubernetes上进行设置，然后按照[此示例的笔记本](https://github.com/axsaucedo/seldon_experiments/blob/master/monitoring-talk/cifar10_example.ipynb)或[Seldon文档](https://docs.seldon.io/projects/seldon-core/en/latest/workflow/install.html)中的说明进行操作。您需要确保安装Seldon，并配置相应的Ingress提供者，如Istio或Ambassador，以便发送REST请求。

为了简化教程，我们已经上传了训练好的Tensorflow Resnet32模型，可以在这个公共Google桶中找到：`gs://seldon-models/tfserving/cifar10/resnet32`。如果您已经训练了自己的模型，可以将其上传到您选择的桶中，可以是Google桶、Azure、S3或本地Minio。对于Google，您可以使用下面的`gsutil`命令行进行操作：

我们可以使用Seldon通过自定义资源定义配置文件部署模型。下面是将模型工件转换为完全成熟的微服务的脚本。

现在我们可以看到模型已经部署并正在运行。

```py
$ kubectl get pods | grep cifarcifar10-default-0-resnet32-6dc5f5777-sq765   2/2     Running   0          4m50s
```

现在我们可以通过发送相同的卡车图像来测试已部署的模型，看看我们是否仍然得到相同的预测。

![图像](../Images/c13d881447de9a1cb03b023f153b0927.png)

数据点通过`plt.imshow(X_curr[0])`显示

我们可以通过发送如下所述的REST请求来实现，然后打印结果。

上述代码的输出是对Seldon Core提供的URL的POST请求的JSON响应。我们可以看到预测正确地结果为“卡车”类别。

```py
{'predictions': [[1.26448288e-06, 4.88144e-09, 1.51532642e-09, 8.49054249e-09, 5.51306611e-10, 1.16171261e-09, 5.77286274e-10, 2.88394716e-07, 0.00061489339, 0.999383569]]}

Prediction: truck
```

### 4\. 性能监控

我们将涵盖的第一个监控支柱是传统的性能监控，这是你在微服务和基础设施领域中会找到的传统和标准监控功能。当然，在我们的案例中，我们将其应用于已部署的机器学习模型。

一些机器学习监控的高级原则包括：

+   **监控运行中的ML服务性能**

+   **识别潜在瓶颈或运行时警告**

+   **调试和诊断ML服务的意外性能**

为此，我们将介绍在生产系统中常用的前两个核心框架：

+   Elasticsearch用于日志——一个文档键值存储系统，通常用于存储容器的日志，这些日志可以用于通过堆栈跟踪或信息日志诊断错误。在机器学习的情况下，我们不仅用它来存储日志，还用来存储机器学习模型的预处理输入和输出，以便进一步处理。

+   Prometheus用于指标——一个时间序列存储系统，通常用于存储实时指标数据，然后可以利用像Grafana这样的工具进行可视化。

Seldon Core为任何已部署的模型提供了开箱即用的Prometheus和Elasticsearch集成。在本教程中，我们将参考Elasticsearch，但为了简化对几个高级监控概念的直观理解，我们将主要使用Prometheus进行指标和Grafana进行可视化。

在下图中，你可以看到导出的微服务如何使任何容器化的模型能够导出指标和日志。指标由Prometheus抓取，日志由模型转发到Elasticsearch（通过我们在下一部分中介绍的事件基础设施进行）。值得明确的是，Seldon Core还支持使用Jaeger的Open Tracing指标，显示了Seldon Core模型图中所有微服务跳跃的延迟。

![图像](../Images/c143b6a1cd7220fe395eb48ec970de53.png)

作者提供的图片

一些由Seldon Core模型暴露的性能监控指标示例，也可以通过进一步集成添加：

+   **每秒请求数**

+   **每个请求的延迟**

+   **CPU/内存/数据利用率**

+   **自定义应用指标**

在本教程中，你可以使用[Seldon Core Analytics包](https://docs.seldon.io/projects/seldon-core/en/latest/examples/metrics.html#Install-Seldon-Analytics)来设置Prometheus和Grafana，该包会设置一切，以便实时收集指标，然后在仪表板上进行可视化。

我们现在可以可视化部署模型相对于其特定基础设施的利用率指标。使用Seldon部署模型时，你需要考虑多个属性，以确保模型的最佳处理。这包括分配的CPU、内存和为应用程序保留的文件系统存储，还包括相应的配置，用于相对于分配的资源和预期请求运行进程和线程。

![图示](../Images/d0cc2829de8a477556017a2a4e626f43.png)

图片来源：作者

同样，我们也能够监控模型本身的使用情况——每个Seldon模型都暴露模型使用指标，如每秒请求数、每个请求的延迟、模型的成功/错误代码等。这些指标很重要，因为它们能够映射到底层机器学习模型的更高级/专用概念。大的延迟峰值可以根据模型的底层需求进行诊断和解释。模型显示的错误也被抽象成简单的HTTP错误代码，这有助于将先进的机器学习组件标准化为微服务模式，从而使DevOps / IT管理人员更容易大规模管理。

![图示](../Images/07c8b6943074a75ba76ace141563087f.png)

图片来源：作者

### 5\. 监控的事件基础设施

为了能够利用更高级的监控技术，我们将首先简要介绍事件基础设施，这使得Seldon可以使用先进的机器学习算法进行异步数据监控，并在可扩展的架构中运行。

Seldon Core利用[KNative Eventing](https://knative.dev/docs/eventing/)来使机器学习模型能够将模型的输入和输出转发到更高级的机器学习监控组件，如异常检测器、概念漂移检测器等。

![文章图像](../Images/a57c4af6face082e2133f577a4bef0a1.png)

我们不会详细介绍KNative引入的事件基础设施，但如果你感兴趣，Seldon Core文档中有多个实际示例，展示了如何利用KNative事件基础设施将有效负载[转发到进一步的组件](https://docs.seldon.io/projects/seldon-core/en/latest/analytics/log_level.html)如Elasticsearch，以及Seldon模型如何连接到[处理事件](https://docs.seldon.io/projects/seldon-core/en/latest/streaming/knative_eventing.html)。

对于本教程，我们需要使模型能够将所有处理过的负载输入和输出转发到KNative Eventing代理，这将使所有其他高级监控组件能够订阅这些事件。

下面的代码向部署配置中添加了一个“logger”属性，用于指定代理位置。

### 6. 统计监控

性能指标对于微服务的常规监控是有用的，然而对于机器学习的专业领域，有一些广为人知和广泛使用的指标在模型生命周期的训练阶段之外都至关重要。更常见的指标包括准确率、精确度、召回率，但也包括像[均方根误差](https://en.wikipedia.org/wiki/Root-mean-square_deviation)、[KL散度](https://en.wikipedia.org/wiki/Relative_entropy)等更多指标。

本文的核心主题不仅仅是指定如何计算这些指标，因为通过一些Flask包装魔法使单个微服务暴露这些指标并不是一项艰巨的任务。关键在于识别可以在数百或数千个模型中引入的可扩展架构模式。这意味着我们需要对接口和模式进行一定程度的标准化，以便将模型映射到相关基础设施中。

一些高级原则围绕更专业的机器学习指标，如下所示：

+   **针对统计机器学习性能的监控**

+   **基准测试多个不同模型或不同版本**

+   **针对数据类型和输入/输出格式的专业化**

+   **有状态的异步“反馈”提供（如“注释”或“修正”）**

鉴于这些需求，Seldon Core引入了一套架构模式，允许我们引入“可扩展指标服务器”的概念。这些指标服务器包含现成的处理模型处理数据的方法，通过订阅相应的事件主题，最终暴露出如下一些指标：

+   **原始指标：真正例、假负例、假正例、真正负例**

+   **基本指标：准确率、精确度、召回率、特异性**

+   **专业指标：KL散度、均方根误差等**

+   **按类别、特征和其他元数据的细分**

![图示](../Images/30cca19ad85dee2babd4ed75c402ce5f.png)

作者提供的图像

从架构的角度来看，这可以通过上面的图示更加直观地展示。这展示了如何通过模型提交单个数据点，然后由任何相应的指标服务器处理。指标服务器还可以在提供“正确/注释”标签后处理这些标签，这些标签可以与Seldon Core在每个请求中添加的唯一预测ID关联。通过从Elasticsearch存储中提取相关数据来计算和暴露专业指标。

目前，Seldon Core提供了一套开箱即用的Metrics Servers：

+   BinaryClassification — 处理以二元分类形式出现的数据（例如0或1），以暴露原始指标以显示基本统计指标（准确率、精确度、召回率和特异性）。

+   MultiClassOneHot — 处理以one hot预测形式出现的数据用于分类任务（例如[0, 0, 1]或[0, 0.2, 0.8]），然后可以暴露原始指标以显示基本统计指标。

+   MultiClassNumeric — 处理分类任务中以数值数据点形式出现的数据（例如1，或[1]），然后可以暴露原始指标以显示基本统计指标。

在这个示例中，我们将能够部署一个类型为“MulticlassOneHot”的Metric Server——你可以在下面总结的代码中看到使用的参数，但完整的YAML文件可以在jupyter notebook中找到。

一旦我们部署了我们的指标服务器，现在我们只需通过相同的微服务端点向我们的CIFAR10模型发送请求和反馈。为了简化工作流程，我们将不发送异步反馈（这会与elasticsearch数据进行比较），而是发送“自包含”的反馈请求，其中包含推断“响应”和推断“真实值”。

以下函数为我们提供了一种发送大量反馈请求的方法，以获得我们用例的大致准确率（正确预测与错误预测的数量）。

现在我们可以首先发送反馈以获得90%的准确率，然后为了确保我们的图表看起来漂亮，我们可以发送另一批请求，这将导致40%的准确率。

这基本上赋予我们实时可视化MetricsServer计算的指标的能力。

![图像](../Images/331608e4580c21ee4dc09de8b432720f.png)

作者提供的图片

从上面的仪表板中，我们可以对通过这种架构模式能够获得的指标类型有一个高层次的直觉。上述状态统计指标特别需要异步提供额外的元数据，然而，即使指标本身的计算方法可能非常不同，我们可以看到基础设施和架构要求可以被抽象和标准化，以便这些指标能够以更可扩展的方式处理。

随着我们深入研究更高级的统计监控技术，我们将继续看到这种模式。

### 7\. 异常检测监控

对于更高级的监控技术，我们将利用Alibi Detect库，特别是它提供的一些高级异常检测算法。Seldon Core为我们提供了一种将异常检测器作为架构模式进行部署的方法，还为我们提供了一个经过优化的预打包服务器，以服务Alibi Detect异常检测模型。

异常检测的一些关键原则包括：

+   检测数据实例中的异常

+   当出现异常值时进行标记/警报

+   识别可能有助于诊断异常值的潜在元数据

+   启用对已识别异常值的深入分析

+   启用检测器的持续/自动化再训练

对于异常值检测器，尤其重要的是允许计算在模型之外进行，因为这些计算通常比较重，并且可能需要更多专业化的组件。已部署的异常值检测器可能会带来与机器学习模型类似的复杂性，因此在这些高级组件中覆盖相同的合规性、治理和血统概念是很重要的。

下图展示了模型如何通过事件基础设施转发请求。异常值检测器随后处理数据点，以计算其是否为异常值。该组件能够将异常值数据异步存储在相应的请求条目中，或者它能够将指标暴露给 Prometheus，这也是我们在本节中将要可视化的内容。

![Figure](../Images/c8cff05e54c48ecdbb2c4093776237fd.png)

作者提供的图像

在本示例中，我们将使用[Alibi Detect 变分自编码器](https://docs.seldon.io/projects/alibi-detect/en/stable/examples/od_vae_cifar10.html)异常值检测器技术。异常值检测器在一批未标记但正常（内点）数据上进行训练。VAE 检测器尝试重建其接收到的输入数据，如果输入数据无法被很好地重建，则会被标记为异常值。

Alibi Detect 提供了允许我们从零开始导出异常值检测器的工具。我们可以使用`fetch_detector`函数来获取它。

如果你想训练异常值检测器，可以通过简单地利用`OutlierVAE`类及其相应的编码器和解码器来实现。

为了测试异常值检测器，我们可以拍摄相同的卡车图像，并观察如果在图像中逐渐添加噪声，异常值检测器的表现如何。我们还可以使用 Alibi Detect 可视化函数`plot_feature_outlier_image`来绘制这些结果。

我们可以创建一组修改过的图像，并通过以下代码将其传递给异常值检测器。

我们现在在变量`all_X_mask`中拥有一组修改过的样本，每个样本都带有逐渐增加的噪声。我们现在可以将这 10 个样本全部通过异常值检测器进行检测。

在查看结果时，我们可以看到前 3 个数据点没有被标记为异常值，而其余的数据点被标记为异常值——我们可以通过打印值`print(od_preds[“data”][“is_outlier”])`来查看这一点。该命令会显示如下数组，其中 0 表示非异常值，1 表示异常值。

```py
array([0, 0, 0, 1, 1, 1, 1, 1, 1, 1])
```

我们现在可以可视化异常值实例级别的评分如何与阈值映射，这反映了上述数组中的结果。

![Image for post](../Images/e223f2ae51a8eef73b4d5f7c5d778253.png)

同样，我们可以深入了解异常值检测器评分通道的直观感受，以及重建的图像，这应能清晰地展示其内部操作方式。

![帖子中的图片](../Images/2354074d9c4f8cd0c9d6d6b73aa0dc2e.png)

我们现在可以将异常值检测器投入生产。我们将利用与指标服务器类似的架构模式，即 Alibi Detect Seldon Core 服务器，它将监听数据的推理输入/输出。每个通过模型的数据点，相关的异常值检测器将能够处理它。

主要步骤是首先确保我们上面训练的异常值检测器已上传到 Google 桶等对象存储中。我们已经将其上传到 `gs://seldon-models/alibi-detect/od/OutlierVAE/cifar10`，但如果你愿意，可以上传并使用自己的模型。

一旦我们部署了异常值检测器，我们将能够发送大量请求，其中许多将是异常值，其他的则不是。

我们现在可以在仪表盘上可视化一些异常值——每个数据点都会有一个新的入口，并包括它是否为异常值。

![图](../Images/22fab564c5d242a7368693f631b1c0d1.png)

图片由作者提供

### 8. 漂移监测

随着时间的推移，实际生产环境中的数据可能会发生变化。虽然这种变化不是剧烈的，但可以通过数据分布的漂移来识别，尤其是在模型预测输出方面。

漂移检测的关键原则包括：

+   识别数据分布中的漂移，以及模型输入和输出数据之间关系的漂移。

+   标记出找到的漂移以及发现漂移时的相关数据点

+   允许深入查看用于计算漂移的数据

在漂移检测的概念中，与异常值检测用例相比，我们面临进一步的复杂性。主要是需要对每个漂移预测进行批量输入，而不是单个数据点。下图展示了与异常值检测器模式类似的工作流程，主要区别在于它保持一个滚动窗口或滑动窗口来进行处理。

![图](../Images/ff4154ac5d02e9d857b37971c4855fb7.png)

图片由作者提供

在这个例子中，我们将再次使用 Alibi Detect 库，它为我们提供了 [Kolmogorov-Smirnov 数据漂移检测器用于 CIFAR-10](https://docs.seldon.io/projects/alibi-detect/en/stable/examples/cd_ks_cifar10.html)。

对于这种技术，将能够使用 `KSDrift` 类来创建和训练漂移检测器，这还需要一个使用“未训练自编码器 (UAE)”的预处理步骤。

为了测试异常检测器，我们将生成一组带有损坏数据的检测器。Alibi Detect 提供了一套很好的工具，我们可以用来以逐渐增加的方式生成图像的损坏/噪声。在这种情况下，我们将使用以下噪声：`[‘gaussian_noise’, ‘motion_blur’, ‘brightness’, ‘pixelate’]`。这些将使用下面的代码生成。

以下是一个来自创建的损坏数据集的数据点，其中包含上述不同类型的逐渐增加的损坏图像。

![帖子图像](../Images/c2995ccd6c4acf96339a6c0835fbe4ac.png)

我们现在可以尝试运行一些数据点以计算是否检测到漂移。初始批次将由来自原始数据集的数据点组成。

这会如预期输出：`漂移？没有！`

类似地，我们可以在损坏的数据集上运行它。

我们可以看到所有请求都被标记为漂移，如预期的那样：

```py
Corruption type: gaussian_noise
Drift? Yes!

Corruption type: motion_blur
Drift? Yes!

Corruption type: brightness
Drift? Yes!

Corruption type: pixelate
Drift? Yes!
```

### 部署漂移检测器

现在我们可以按照上述架构模式部署我们的漂移检测器。与异常检测器类似，我们首先必须确保我们训练的漂移检测器可以上传到对象存储。我们目前可以使用我们准备好的 Google 存储桶 `gs://seldon-models/alibi-detect/cd/ks/drift` 进行部署。

这将具有类似的结构，主要区别是我们还将指定 Alibi Detect 服务器使用的所需批量大小，以便在运行模型之前作为缓冲。在这种情况下，我们选择批量大小为 1000。

现在我们已经部署了异常检测器，首先尝试从正常数据集中发送 1000 个请求。

接下来我们可以发送损坏的数据，这将在发送 10k 数据点后导致检测到漂移。

我们可以在 Grafana 仪表盘上可视化检测到的不同漂移点。

![图示](../Images/895a3bbbb20901101dfc4bd59edbb935.png)

作者提供的图像

### 9. 可解释性监控

人工智能可解释性技术对于理解复杂的黑箱机器学习模型的行为至关重要。当前的重点是提供对可解释组件可以大规模部署的架构模式的直观理解和实际示例。模型可解释性的关键原则包括：

+   **对模型行为进行人类可解释的洞察**

+   **引入特定用例的可解释性功能**

+   **识别关键指标，例如信任评分或统计性能阈值**

+   **启用使用一些更复杂的机器学习技术**

关于可解释性有广泛的不同技术，但理解不同类型的解释器的高层主题很重要。这些包括：

+   **范围（本地 vs 全局）**

+   **模型类型（黑箱 vs 白箱）**

+   **任务（分类、回归等）**

+   **数据类型（表格、图像、文本等）**

+   **洞察（特征归因、对比事实、影响训练实例等）**

作为接口的解释器在数据流模式上有相似之处。即许多解释器需要与模型处理的数据进行互动，并且能够与模型本身进行互动——对于黑箱技术，这包括输入/输出，而对于白箱技术，则包括模型本身的内部。

![Figure](../Images/81d115b32de0b5399e611f38e790c06d.png)

图片由作者提供

从架构的角度来看，这主要涉及一个独立的微服务，该服务不仅接收推断请求，还能够与相关模型互动，通过发送相关数据“反向工程”模型。这在上面的图示中有所展示，但一旦我们深入到示例中，这将变得更加直观。

对于这个示例，我们将使用 Alibi Explain 框架，并使用[Anchor Explanation](https://docs.seldon.io/projects/alibi/en/latest/examples/anchor_image_imagenet.html)技术。该本地解释技术告诉我们在特定数据点中具有最高预测能力的特征。

我们可以简单地通过指定数据集的结构以及一个允许解释器与模型的预测功能互动的`lambda`来创建我们的 Anchor 解释器。

我们能够识别模型中在此案例中预测图像的锚点。

![Image for post](../Images/c13d881447de9a1cb03b023f153b0927.png)

我们可以通过显示解释本身的输出锚点来可视化锚点。

我们可以看到图像的锚点包括卡车的挡风玻璃和轮子。

![Image for post](../Images/902de571fd0d5b33d84174f68044924c.png)

在这里，您可以看到解释器与我们部署的模型互动。在部署解释器时，我们将遵循相同的原则，但与使用在本地运行模型的`lambda`不同，这将是一个调用远程模型微服务的函数。

我们将采用类似的方法，只需将上面的图像上传到对象存储桶。与之前的示例类似，我们提供了一个存储桶，路径为`gs://seldon-models/tfserving/cifar10/explainer-py36–0.5.2`。我们现在可以部署一个解释器，该解释器可以作为 Seldon 部署的 CRD 部分进行部署。

我们可以通过运行`kubectl get pods | grep cifar`来检查解释器是否正在运行，这应当会输出两个正在运行的 pod：

```py
cifar10-default-0-resnet32-6dc5f5777-sq765   2/2     Running   0          18m
cifar10-default-explainer-56cd6c76cd-mwjcp   1/1     Running   0          5m3s
```

类似于我们向模型发送请求的方式，我们也能够向解释器路径发送请求。在这里，解释器将与模型本身互动，并打印出反向工程的解释。

我们可以看到解释的输出与我们上面看到的一样。

![Image for post](../Images/902de571fd0d5b33d84174f68044924c.png)

最后，我们还可以看到一些来自解释器本身的与指标相关的组件，这些组件可以通过仪表板进行可视化。

```py
Coverage: 0.2475
Precision: 1.0
```

与其他基于微服务的机器学习组件类似，解释器也可以暴露这些和其他更专业的性能或高级监控指标。

### 结束语

在总结之前，需要指出的是将这些高级机器学习概念抽象为标准化架构模式的重要性。这样做的原因主要是为了使机器学习系统具备规模化的能力，同时也便于在整个技术栈中进行高级组件的集成。

上述提到的所有高级架构不仅适用于各个高级组件，而且还可以启用我们可以称之为“集成模式”的功能——即将高级组件连接到其他高级组件的输出上。

![图示](../Images/0ea18befddfeb5c06aa892b9de841118.png)

作者提供的图片

还需要确保结构化和标准化的架构模式，使开发人员能够提供监控组件，这些组件也是高级机器学习模型，具备与之管理风险所需的治理、合规性和追溯相同的水平。

这些模式通过 Seldon Core 项目不断被优化和发展，前沿的异常检测、概念漂移、可解释性等算法也在不断改进——如果你对进一步讨论感兴趣，请随时联系。此教程中的所有示例都是开源的，欢迎提出建议。

如果你对机器学习模型的可扩展部署策略的更多实际示例感兴趣，你可以查看：

+   [使用 Argo 工作流的批处理](https://docs.seldon.io/projects/seldon-core/en/latest/examples/argo_workflows_batch.html)

+   [使用 Knative 的无服务器事件处理](https://docs.seldon.io/projects/seldon-core/en/latest/streaming/knative_eventing.html)

+   [使用 Alibi 的 AI 解释模式](https://docs.seldon.io/projects/seldon-core/en/latest/analytics/explainers.html)

+   [Seldon 模型容器化笔记本](https://docs.seldon.io/projects/seldon-core/en/latest/examples/sklearn_spacy_text_classifier_example.html)

+   [Kafka Seldon Core 流处理部署笔记本](https://github.com/SeldonIO/seldon-core/blob/master/examples/kafka/sklearn_spacy/README.ipynb)

**个人简介：[Alejandro Saucedo](https://www.linkedin.com/in/axsaucedo/)** 是 Seldon 的工程总监、The Institute for Ethical AI 的首席科学家，以及 ACM 委员会成员。

[原文](https://towardsdatascience.com/production-machine-learning-monitoring-outliers-drift-explainers-statistical-performance-d9b1d02ac158)。经许可转载。

**相关：**

+   [可解释性、可说明性与机器学习——数据科学家需要了解的内容](/2020/11/interpretability-explainability-machine-learning.html)

+   [使用 TensorFlow Serving 部署训练好的模型到生产环境](/2020/11/serving-tensorflow-models.html)

+   [AI 不仅仅是一个模型：实现完整工作流成功的四个步骤](/2020/11/mathworks-ai-four-steps-workflow.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 需求

* * *

### 相关主题

+   [使用 Eurybia 检测数据漂移以确保生产 ML 模型质量](https://www.kdnuggets.com/2022/07/detecting-data-drift-ensuring-production-ml-model-quality-eurybia.html)

+   [使用 MLOps 管理生产中的模型漂移](https://www.kdnuggets.com/2023/05/managing-model-drift-production-mlops.html)

+   [用 AI 对抗 AI：深度伪造应用的欺诈监测](https://www.kdnuggets.com/2023/05/fighting-ai-ai-fraud-monitoring-deepfake-applications.html)

+   [使用标准差在 Python 中去除异常值](https://www.kdnuggets.com/2017/02/removing-outliers-standard-deviation-python.html)

+   [如何使用 Pandas 处理数据集中的异常值](https://www.kdnuggets.com/how-to-handle-outliers-in-dataset-with-pandas)

+   [将机器学习算法完整部署到生产环境](https://www.kdnuggets.com/2021/12/deployment-machine-learning-algorithm-live-production-environment.html)
