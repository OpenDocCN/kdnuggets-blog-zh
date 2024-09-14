# 机器学习部署的软件接口

> 原文：[`www.kdnuggets.com/2020/03/software-interfaces-machine-learning-deployment.html`](https://www.kdnuggets.com/2020/03/software-interfaces-machine-learning-deployment.html)

评论

**由 [Luigi Patruno](https://mlinproduction.com/about/) 提供，数据科学家及 ML in Production 的创始人**。

![](img/741c813aec2c11d4c0beb00d70c39134.png)

在我们 [上一篇文章](https://www.kdnuggets.com/2020/02/deploy-machine-learning-model.html) 关于机器学习部署的文章中，我们介绍了什么是部署机器学习模型。我们了解到，为了使训练好的模型的预测结果可供用户和其他软件系统使用，我们需要考虑许多因素，包括预测生成的频率以及是否一次生成单个样本的预测或批量样本的预测。在这篇文章中，我们将开始研究 **如何实现** 部署过程。

虽然许多博客文章直接跳到实现 Flask API 或使用工作流调度程序，我们将从更基础的层面开始。我们将首先讨论 *软件接口*，它可以被认为是软件之间的边界。一个类比是，软件是拼图的一块，整个软件系统是完整的拼图。设计得当时，接口允许你连接许多不同的软件组件，从而形成大型而复杂的项目。

> 在 ML 部署方面，良好构建的接口有助于实现可重复、自动化的即插即用部署。一个好的接口让你轻松推出模型更新，对你部署的模型进行版本控制，等等。

开始吧！

### 什么是接口？

想象一个经理分配给员工创建报告的任务。一位好的经理可能会说：“我需要你生成一个包含以下图表和数据的报告。为生成该报告，请使用客户交易数据。”经理明确了期望的结果（报告），并暗示了一种方法（使用客户交易数据）。

相比之下，一位不好的经理可能会做以下任何事情：

+   **不指定输入**——要求报告但未指定使用哪些数据或暗示员工应与谁联系以发现合适的数据集。

+   **不明确交付物**——给员工一堆数据，但不告诉他们应该生产什么。

+   **微观管理**——告诉员工使用哪些工具生成报告，遵循哪些步骤，并承诺任何偏离此计划的行为都将受到迅速且严厉的惩罚。

[软件接口](https://blog.robertelder.org/interfaces-most-important-software-engineering-concept/)就像管理者。一个好的接口明确说明了必要的输入和它产生的输出。例如，作为函数实现的接口将列出所有必需的参数和函数返回的内容。接口可以被看作是定义不同软件如何相互通信的“边界”。当接口构建良好时，不同的软件，甚至是由不同团队或公司开发的软件，可以相互通信并协同工作。

软件工程师被教导要关注他们开发的接口，而不是函数的实现。实现固然重要，但总是可以更新。但更新接口尤其是外部接口在发布后要困难得多。因此，定义接口所投入的时间是值得的。

### 机器学习模型的基本接口

软件工程师如何看待机器学习模型实际做了什么？从抽象的角度来看，模型接受数据，对这些数据进行某种处理，然后返回结果。就是这么简单。模型如何处理数据可能非常复杂，比如卷积神经网络对图像数据张量进行卷积的前向传播，但这些都是实现细节。

机器学习模型的边界由模型的输入，即特征，以及模型预测的输出组成。因此，一个构建良好的接口必须同时考虑输入特征和预测输出。为了说明这一点，让我们用一个简单的函数来定义这个接口：

```py
def predict(model, input_features):
    '''
    Function that accepts a model and input data and returns a prediction.

    Args:
    ---
    model: a machine learning model.
    input_features: Features required by the model to generate a 
    prediction. Numpy array of shape (1, n) where n is the dimension
    of the feature vector.

    Returns:
    --------
    prediction: Prediction of the model. Numpy array of shape (1,).
    ''' 

```

这个函数的输入是一个*model*和一组*input_features*，并返回一个预测。注意我们还没有*实现*这个函数，即我们还没有编写*如何*将模型和特征结合以生成预测。我们只是创建了一个契约或承诺——我们保证如果调用者提供了一个*model*和*input_features*，函数将返回一个预测。

### 机器学习模型的多个接口

我们定义的*predict()*方法接受一个特征向量并返回一个预测。我们怎么知道这一点？文档说明*input_features*是一个形状为*(1, n)*的 numpy 数组，其中*n*是特征向量的维度。如果你的模型期望一次预测一个实例，这很好，但如果模型还需要对样本批次进行预测，就不太理想了。你可以通过编写 for 循环来解决这个问题，但循环的效率可能不高。相反，我们应该定义另一个直接处理批量情况的方法。我们称之为*predict_batch*：

```py
def predict_batch(model, batch_input_features):
    '''
    Function that predicts a batch of samples.

    Args:
    ---
    model: a machine learning model.
    batch_input_features: A batch of features required by the model to
    generate predictions. Numpy array of shape (m, n) where m is the
    number of instances and n is the dimension of the feature vector.

    Returns:
    --------
    predictions: Predictions of the model. Numpy array of shape (m,).
    '''

```

这个方法定义了一个契约，承诺在提供模型和输入特征批次时返回一批预测。再次强调，我们尚未实现这个方法——这留给方法的开发者。开发者可以选择使用循环并重复调用*predict*。或者开发者可以做其他事情。这对于部署的目的并不重要。*重要的是*我们有两个接口：一个用于预测单个样本，另一个用于预测一批样本。

### 机器学习面向对象编程 – MLOOP

到目前为止，我们忽略了*model*参数，这对于*predict*和*predict_batch*方法都是必需的。让我来解释一下这对机器学习来说为什么是个问题。

目前，大多数开发机器学习模型的工程师都希望使用最佳工具。如果工程师正在构建经典模型，如逻辑回归或随机森林，工程师可能会选择使用[scikit-learn](https://scikit-learn.org/stable/)。但对于深度学习，该工程师可能会选择使用[Tensorflow](https://www.tensorflow.org/)或[PyTorch](https://pytorch.org/)。即使在经典机器学习中，工程师也可能选择[xgboost](https://xgboost.readthedocs.io/en/latest/)的梯度提升树实现。每个库中的模型对象具有略微不同的 API。我们无法预测未来的 ML 库将实现哪些 API。这将使我们接口的实现变得非常混乱。例如，我们不希望我们的实现看起来像这样：

```py
def predict(model, input_features):
    ...
    if isinstance(model, sklearn.base.BaseEstimator)
        ...
    elif isinstance(model, xgboost.core.Booster): 
        ...
    elif isinstance(model, tensorflow.keras.Model): 
        ...
    elif isinstance(model, torch.nn.module):
        ...
    ... 

```

这种实现方式将很难维护，也会使调试运行时错误变得困难。此外，想象一下，如果我们希望在使用一个模型时传递额外的参数而不是另一个模型会发生什么。例如，如果我们希望仅在预测 sklearn 模型时传递额外的参数。函数的参数数量将会增加，但这些参数对于非 sklearn 模型是无用的。我们将如何在文档中描述这一点？这些只是为什么面向对象编程、创建类和对象是更受欢迎的几个原因。

我们的接口由两个方法组成：*predict*和*predict_batch*。让我们定义一个包含这两个方法的基类：

```py
class Model:
    def __init__(self, model):
        self.model = model

    def predict(self, input_features):
        '''
        Function that accepts input data and returns a prediction.

        Args:
        ---
        input_features: Features required by the model to generate prediction. Numpy
        array of shape (1, n) where n is the dimension of the feature vector.

        Returns:
        --------
        prediction: Prediction of the model. Numpy array of shape (1,).
        '''
        raise NotImplementedError

    def predict_batch(self, batch_input_features):
        '''
        Function that predicts a batch of samples.

        Args:
        ---
        batch_input_features: A batch of features required by the model to generate
            predictions. Numpy array of shape (m, n) where m is the number of
            instances and n is the dimension of the feature vector.

        Returns:
        --------
        prediction: Predictions of the model. Numpy array of shape (m, 1).
        '''
        raise NotImplementedError

```

这个基类充当了我们数据科学团队的*模板*。如果数据科学家想使用 scikit-learn 模型，他只需要继承 Model 类并实现必要的方法。如果另一个数据科学家想使用 Tensorflow，没问题，只需创建一个 Tensorflow 子类！为了说明这一点，让我们创建一个 sklearn 子类：

```py
class SklearnModel(Model):
    def __init__(self, model):
        super().__init__(model) 

    def predict(self, input_features):
        y = self.model.predict(input_features.reshape(1, -1))
        return y

    def predict_batch(self, batch_input_features):
        ys = self.model.predict(batch_input_features)
        return ys

```

由于 sklearn Predictors 期望 2D 输入，我们在 predict 方法中重新调整了 input_features 参数。这是面向对象方法的一个关键好处。我们可以定义适用于我们解决的问题类型的接口*并且*利用[优秀的第三方机器学习库！](https://github.com/EthicalML/awesome-production-machine-learning)

好处不仅仅于此。我们可以添加更多简化 ML 工作流的方法。例如，一旦模型被训练，我们通常需要一种方式来序列化模型，然后在推理时进行反序列化。因此，我们可以在接口中添加两个方法，`serialize()` 和 `deserialize()`。我们甚至可以在基类 `Model` 中提供这些方法的默认实现，并在子类中创建库特定的实现。

其他有用的接口方法示例包括将序列化模型从本地文件系统移动到某个模型存储或像 S3 这样的远程文件系统。你可以添加的方法没有限制。

> 提前创建良好的接口将为你的机器学习团队节省大量时间，使得在进行额外项目时，部署变得自动化和可重复。

[原始](https://mlinproduction.com/software-interfaces-for-machine-learning-deployment-deployment-series-02/)。已获许可转载。

**简介：** [Luigi Patruno](https://twitter.com/MLinProduction) 是数据科学家和机器学习顾问。他目前是 2U 的数据科学总监，负责领导一个数据科学团队，专注于构建机器学习模型和基础设施。作为顾问，Luigi 帮助公司通过应用现代数据科学方法来实现战略业务和产品目标。他创立了 [MLinProduction.com](http://mlinproduction.com/) 以收集和分享机器学习操作化的最佳实践，并且教授过统计学、数据分析和大数据工程的研究生课程。

**相关：**

+   [部署机器学习模型是什么意思？](https://www.kdnuggets.com/2020/02/deploy-machine-learning-model.html)

+   [用于生产级机器学习的 MLOps](https://www.kdnuggets.com/2019/11/cnvrg-mlops-production-machine-learning.html)

+   [使用 Flask 部署机器学习模型](https://www.kdnuggets.com/2019/12/excelr-deployment-machine-learning-flask.html)

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

### 更多相关话题

+   [将机器学习算法完全部署到...](https://www.kdnuggets.com/2021/12/deployment-machine-learning-algorithm-live-production-environment.html)

+   [从数据收集到模型部署：数据科学项目的 6 个阶段](https://www.kdnuggets.com/2023/01/data-collection-model-deployment-6-stages-data-science-project.html)

+   [回归基础第 4 周：高级话题和部署](https://www.kdnuggets.com/back-to-basics-week-4-advanced-topics-and-deployment)

+   [顶级 7 款模型部署和服务工具](https://www.kdnuggets.com/top-7-model-deployment-and-serving-tools)

+   [软件开发人员与软件工程师](https://www.kdnuggets.com/2022/05/software-developer-software-engineer.html)

+   [软件错误与权衡：Tomasz Lelek 的新书](https://www.kdnuggets.com/2021/12/manning-software-mistakes-tradeoffs-book.html)
