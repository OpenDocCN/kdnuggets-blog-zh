# Amazon Machine Learning：实用还是过于简单？

> 原文：[https://www.kdnuggets.com/2016/02/amazon-machine-learning-nice-easy-simple.html](https://www.kdnuggets.com/2016/02/amazon-machine-learning-nice-easy-simple.html)

**作者：Alex Perrier，[@alexip](https://twitter.com/alexip)**，（最初发布于[Open Data Science博客](http://www.opendatascience.com/blog/amazon-machine-learning-nice-and-easy-or-overly-simple/)）

**机器学习即服务（MLaaS）承诺将数据科学带入公司可及的范围。在这种背景下，Amazon Machine Learning是一种具有二分类/多分类和线性回归特性的预测分析服务。该服务提供了简单的工作流程，但缺乏模型选择功能，执行时间较慢。预测性能令人满意。**

![AmazonWebservices_Logo.svg](../Images/76dc4722bfa89125a95988a8f65108c7.png)

数据科学很热门且具有吸引力，但它也很复杂。构建和维护数据科学基础设施可能很昂贵。经验丰富的数据科学家稀缺，内部开发算法、构建预测分析应用程序和创建生产就绪的API需要特定的专业知识和资源。尽管公司可能预计数据科学服务的好处，但在进行必要投资之前，他们可能不愿意先行测试。

这就是机器学习即服务（Machine Learning-as-a-Service）介入的地方，它承诺简化和普及机器学习：在短时间内享受机器学习的好处，同时保持低成本。

一些关键参与者已经进入了这个领域：[Google Predictive Analytics](https://cloud.google.com/prediction/)、[Microsoft Azure Machine Learning](https://azure.microsoft.com/en-us/services/machine-learning/)、[IBM Watson](http://www.ibm.com/analytics/watson-analytics/)、[Big ML](https://bigml.com/)和[许多其他](http://www.butleranalytics.com/10-machine-learning-as-a-service-platforms/)。有些提供简化的预测分析服务，而有些则提供更专业的界面和超越预测的数据科学服务。

相对较新的参与者是AWS的[Amazon Machine Learning](https://aws.amazon.com/machine-learning/)服务。该服务于2015年4月在AWS 2015峰会上推出，距今不足一年。Amazon Machine Learning旨在通过关注数据工作流并将更复杂的技术细节隐藏在后台，简化预测分析。通过将技术细节的重要部分从用户视野中移除，Amazon Machine Learning将数据科学带给更广泛的受众。它显著降低了公司尝试预测分析的门槛，使强大的机器学习工具能够在很短的时间内可用和运作。

互联网的大部分已经在运行 AWS 的许多服务。AWS 将机器学习服务添加到其中将允许工程师将预测分析能力纳入现有应用程序。

亚马逊机器学习使公司能够在不投入大量资源和投资的情况下尝试数据科学并评估其业务价值。在这方面，亚马逊机器学习是希望搭乘数据科学列车的公司的 [预测分析入门](http://docs.aws.amazon.com/machine-learning/latest/dg/machine-learning-concepts.html)。

### 活塞、汽化器和滤清器：引擎下的秘密是什么？

亚马逊机器学习的一个重要特征是其简化的机器学习方法。它“[为我们简化了机器学习 [InfoWorld]](http://www.infoworld.com/article/2908498/amazon-web-services/amazon-machine-learning-for-the-rest-us.html)“；它“[使任何开发者都能触及机器学习 [Techcrunch]](http://techcrunch.com/2015/04/09/aws-wants-to-put-machine-learning-in-reach-of-any-developer/#.mdrfnlu:aFbV).”

但预测分析是一个复杂的领域。诸如数据处理、特征工程、参数调整和模型选择等任务需要时间，并遵循一套完善的协议、方法和技术。亚马逊机器学习的简化服务是否仍能在不牺牲复杂性的情况下提供性能？你是否还能通过简化的机器学习流程获得预测分析的好处？

**1 个模型，1 种算法，3 种任务，简单的管道设置，向导和智能默认值**

根据 [文档](https://aws.amazon.com/documentation/machine-learning/)，亚马逊机器学习基于通过 [随机梯度下降](https://en.wikipedia.org/wiki/Stochastic_gradient_descent)（简称 SGD）训练的线性模型。就是这样。没有随机森林或提升树，没有内核 SVM、贝叶斯分类器或聚类。这可能看起来是一个严峻的限制。然而，由 [Leon Bottou](http://leon.bottou.org/papers) 开发的随机梯度下降算法是一个非常稳定和鲁棒的算法。这个算法已经存在很长时间，并且多年来有许多改进版本。

这个简单的预测设置很可能足以解决大量现实世界的商业预测问题。正如我们将看到的，它也表现出不错的性能。

**任务**

亚马逊机器学习平台提供三种监督学习任务，每种任务都有其相关模型和损失函数。

+   **二分类** 使用逻辑回归（逻辑损失函数 + SGD）

+   **多分类** 使用多项逻辑回归（多项逻辑损失 + SGD）

+   以及 **回归** 使用线性回归（平方损失函数 + SGD）

对于二分类器，评分函数是[F1-measure](https://en.wikipedia.org/wiki/F1_score)；对于多类分类器，评分是宏平均 F1-measure，它是每个类别 F1-measure 的平均值；对于回归使用 RMSE 指标。F1-measure 通常用于信息检索，是精确率和召回率的调和平均数。这是一种稳健的分类度量，对多类不平衡不太敏感。

**使用 Recipes 进行特征工程**

在 Amazon 机器学习管道中，可以使用[Recipes](http://docs.aws.amazon.com/machine-learning/latest/dg/feature-processing.html)来转换你的变量。通过 JSON 格式的指令可以进行多种转换：替换缺失值、笛卡尔积、将分类变量分箱为数值型变量，或为文本数据形成 n-grams。

例如，这里是一个在处理[鸢尾花](https://archive.ics.uci.edu/ml/datasets/Iris)数据集时自动生成的将分类值转换为数值值的 Recipe。

```py
{
  "groups" : {
    "NUMERIC_VARS_QB_50" : "group('sepal_width')",
    "NUMERIC_VARS_QB_20" : "group('petal_width')",
    "NUMERIC_VARS_QB_10" : "group('petal_length','sepal_length')"
  },
  "assignments" : { },
  "outputs" : [ "ALL_CATEGORICAL",
  "quantile_bin(NUMERIC_VARS_QB_50,50)",
  "quantile_bin(NUMERIC_VARS_QB_20,20)",
  "quantile_bin(NUMERIC_VARS_QB_10,10)" ]
} 
```

**训练集与验证集**

默认情况下，Amazon 机器学习将你的训练数据集拆分为 70/30。Amazon 机器学习将丰富的技术简化为非常简单且有限的选择。数据拆分为训练集和验证集可以用多种方式进行，而 Amazon 机器学习将其简化为随机化样本或不随机化。你当然可以在 Amazon 机器学习之外自行拆分数据，创建一个新的数据源用于保留集，并在该保留集上评估模型的性能。

**SGD 参数调整**

可用于调整模型的参数数量较少：传递次数、正则化类型（无、L1、L2）和正则化参数。无法设置算法的学习率，也没有信息说明如何设置这个重要参数。

### 那么，你从哪里开始？

AWS 控制台主页有超过 50 种不同服务，名称如 Elastic Beanstalk、Kinesis、RedShift 或 Route 53，这可能会让人感到畏惧。然而，得益于良好的[文档](http://docs.aws.amazon.com/machine-learning/latest/dg/what-is-amazon-machine-learning.html)和一套设计良好的向导，创建你的第一个项目将是一个快速且愉快的体验。

一旦你的数据集以正确格式的 csv 文件存储在 S3 上，整个过程分为四个步骤：

+   创建一个**数据源**：告知 Amazon 机器学习你的数据所在位置及其遵循的模式。

+   创建一个**模型**：任务由目标的数据类型推断（数值型 => 回归，二分类 => 分类或分类数据用于多项分类），你可以为模型设置一些自定义参数。

+   **训练**和评估模型

+   执行批量**预测**

最佳的入门策略是遵循编写精良且详细的 [Amazon Machine Learning 的教程](http://docs.aws.amazon.com/machine-learning/latest/dg/tutorial.html)。

这些资源也可用：

+   [AWS Machine Learning 的 Cloud Academy 课程](https://cloudacademy.com/amazon-web-services/amazon-machine-learning-course/)

+   [Amazon Machine Learning：使用案例和 Python 中的实际示例](http://cloudacademy.com/blog/aws-machine-learning/)

+   以及这个优秀的 YouTube 视频 [你在 Amazon AWS 的第一周](https://www.youtube.com/watch?v=7CiHBcqw6zc)，由 Miles Ward 提供，用于 EC2 设置。

### 那实践中呢？

**交叉验证**

在 Amazon Machine Learning 中没有专门的交叉验证方法。建议的方式是按照 K 折交叉验证方案创建数据文件，为每个折创建数据源，并在每个数据源上训练模型。例如，为了执行四折交叉验证，你需要创建四个数据源、四个模型和四个评估。然后，你可以平均这四个评估分数，以获得模型的最终交叉验证分数。

**过拟合**

[过拟合](http://docs.aws.amazon.com/machine-learning/latest/dg/model-fit-underfitting-vs-overfitting.html)发生在你的模型过于紧密地贴合训练数据，以至于丧失预测新数据的能力。检测过拟合很重要，以确保模型具有预测能力。这可以通过 [学习曲线](https://class.coursera.org/ml-005/lecture/64) 来完成，通过比较训练集和验证集的 [误差曲线](http://scikit-learn.org/stable/modules/learning_curve.html) 来检测不同样本大小的表现。

Amazon Machine Learning 提供了两种经典的正则化方法（L1 Lasso 和 L2 Ridge）来减少过拟合，但没有过拟合检测方法。要检查你的模型是否对训练数据过拟合，你需要创建不同的数据集和模型，并对每个模型进行评估。

**成本**

特征工程和特征选择是一个反复进行的过程，需要创建和评估许多数据集。每次创建新的数据源时，Amazon Machine Learning 会对数据进行统计分析，这可能会显著增加项目的整体成本。在为本文进行研究时，95% 的成本是由于为我尝试的每个新数据源创建数据统计。而且 Amazon Machine Learning 处理大约 400,000 个样本花费了大约 15 小时。

**控制台的替代方案**

构建一个快速的测试/失败循环对任何数据科学项目都是至关重要的。数据文件、模型和验证之间的反复过程需要进行，以构建一个具有强大预测能力的鲁棒模型。

通过 UI 与 Amazon Machine Learning 交互会很快变得乏味，特别是如果你已经习惯了命令行的话。全新的数据-模型-评估流程涉及大约八到十页的页面、字段和点击。这些 UI 操作需要时间。此外，每个新实体可能需要几分钟才能可用。与基于脚本的流程（如命令行、R Studio、Jupyter notebooks 等）相比，这个过程非常缓慢。

使用配方、上传预定义的模式以及使用 AWS CLI 管理 S3 将有助于加快速度。

AWS 提供了多种语言的 SDK，包括用于 Amazon Machine Learning 的方法。你可以使用 Python、Java 或 Scala 来驱动你的 Amazon Machine Learning 项目。例如，可以查看这个[Amazon Machine Learning 代码示例](https://github.com/awslabs/machine-learning-samples)的 GitHub 仓库。

通过脚本与 Amazon Machine Learning 交互可能是与该服务交互的最有效方式。但如果你无论如何都会编写 Python 脚本，那么使用 Amazon Machine Learning 的优势就不那么明显了。你完全可以使用像 Scikit-learn 这样的专用数据科学工具包。

### 案例研究

由于限制在线性模型和随机梯度下降算法上，人们可能会对服务的性能产生疑问。在本文的其余部分，我将比较 Scikit-learn 和 Amazon Machine Learning 在二分类和多分类任务中的表现。

#### 鸢尾花数据集

让我们从一个简单且非常容易的多分类数据集——[鸢尾花数据集](http://scikit-learn.org/stable/modules/generated/sklearn.datasets.load_iris.html) 开始，并比较 Scikit-learn 的 [SGDClassifier](http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.SGDClassifier.html) 与 Amazon Machine Learning 的多分类任务的表现。

SGDClassifier 的设置与 Amazon Machine Learning 的 SGD 参数类似：

+   L2 正则化 (alpha = 10^(-6) )

+   最优学习率

+   对数损失函数

+   10 次迭代

训练集被随机选择为 70/30 以进行训练和评估。宏平均 F1 分数在 Scikit 和 Amazon Machine Learning 中都被使用。

在保留集上的最终评估分数在 Scikit-learn 和 Amazon Machine Learning 之间非常相似：

+   Scikit-learn: 0.93

+   Amazon Machine Learning: 0.94

到目前为止，一切顺利。现在来处理一个更复杂的数据集。

### Kaggle Airbnb 数据

最近的 [Airbnb 新用户预订](https://www.kaggle.com/c/airbnb-recruiting-new-user-bookings) Kaggle 竞赛旨在预测 Airbnb 用户的国家目的地，给定一系列数据集（国家、年龄、性别信息、用户和会话）。

我们将简化数据集，仅考虑由性别、年龄、附属、浏览器、注册日期等特征组成的用户训练数据。数据集在竞赛页面上免费提供，只需注册 Kaggle 即可。

在这个数据集中，大约40%的用户没有进行任何预订。我们将尝试预测用户是否进行了预订，而不是预测目的地国家（如果有的话），从而解决一个二分类问题。

使用10万行数据作为训练数据集，根据AUC分数，我们在训练数据集上获得了以下性能结果：

+   Amazon Machine Learning SGD : 0.71

+   scikit SGD : 0.61

+   scikit RandomForest: 0.70

+   XGboost: 0.74

*注意：这绝不是一个基准测试。以上结果仅用于说明。*

我们尝试了多种 SGD 设置，但未能更接近 Amazon Machine Learning 的分数。分数是基于 Amazon Machine Learning 创建的初始验证30k样本和另一组50k样本进行平均的。

XGBoost 分类器的随机森林没有使用网格搜索。我们在可能的情况下使用了默认设置。

这些结果表明，当使用 SGD 分类器时，Amazon Machine Learning 的性能达到了最佳水平。Amazon Machine Learning SGD 超越了 Scikit-learn 的 SGD。它与随机森林性能相近，但被 XGboost 超越。在这篇[博客文章](http://lenguyenthedat.com/minimal-data-science-2-avazu/#comment-2451032447)中也观察到了类似的表现。

### 结论

总之，Amazon Machine Learning 是一个让公司快速启动数据科学项目的绝佳方式。该服务性能出色且易于使用。但它缺少重要的模型选择功能，算法种类非常有限，执行时间较长。

Amazon Machine Learning 的简化方法使工程师能够快速实现预测分析服务。这反过来允许公司进行实验并评估数据科学的商业价值。

这也是一个很好的平台，可以学习和实践机器学习概念，而无需担心算法和模型。对于有志成为数据科学家的人员来说，这是体验真实（尽管简化过的）数据科学项目工作流程的好方法。

基于控制台的工作流程较慢。使用[SDKs](https://aws.amazon.com/sdk-for-python/)和[AWS CLI](http://docs.aws.amazon.com/cli/latest/reference/machinelearning/index.html)很快变得必要。

通过正则化可以调整模型以解决欠拟合和过拟合问题。然而，没有简单的方法来检测过拟合或欠拟合的存在。添加经典的可视化，如学习曲线，将极大地促进模型选择。

简介：[亚历克斯·佩里埃博士](https://twitter.com/alexip)，是数据科学家、Berklee Online的软件工程师以及ODSC的贡献者。你可以在他的[机器学习博客](http://alexperrier.github.io)上阅读更多内容。

[原文](http://www.opendatascience.com/blog/amazon-machine-learning-nice-and-easy-or-overly-simple/)。

**相关：**

+   [数据科学中5个最佳机器学习API](/2015/11/machine-learning-apis-data-science.html)

+   [标准化机器学习网络服务API的世界](/2015/07/psi-machine-learning-web-service-apis.html)

+   [云机器学习战争：亚马逊 vs IBM Watson vs 微软Azure](/2015/04/cloud-machine-learning-amazon-ibm-watson-microsoft-azure.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT需求

* * *

### 更多相关话题

+   [如何让大型语言模型与您的软件兼容……](https://www.kdnuggets.com/how-to-make-large-language-models-play-nice-with-your-software-using-langchain)

+   [选择合适的机器学习算法的简单指南](https://www.kdnuggets.com/2020/05/guide-choose-right-machine-learning-algorithm.html)

+   [学习如何设计、测量和实施可信的A/B测试……](https://www.kdnuggets.com/2023/01/sphere-design-measure-implement-trustworthy-ab-tests-ronny-kohavi.html)

+   [使用Python为亚马逊产品构建推荐系统](https://www.kdnuggets.com/2023/02/building-recommender-system-amazon-products-python.html)

+   [免费亚马逊课程学习生成式AI：适合所有级别](https://www.kdnuggets.com/free-amazon-courses-to-learn-generative-ai-for-all-levels)

+   [滴答：使用Pendulum进行Python中的日期和时间管理](https://www.kdnuggets.com/tick-tock-using-pendulum-for-easy-date-and-time-management-in-python)
