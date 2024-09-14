# GitHub Repo Raider与机器学习的自动化

> 原文：[https://www.kdnuggets.com/2019/11/github-repo-raider-automated-machine-learning.html](https://www.kdnuggets.com/2019/11/github-repo-raider-automated-machine-learning.html)

[评论](#comments)![GitHub Repo Raider](../Images/ce3bc0458e94c77da3352d704b2bd785.png)

[GitHub](https://github.com/)是各种开源项目的集散地，包括机器学习项目，无论是自动化的还是其他类型的。

什么是自动化机器学习？当然是[自动化自动化](https://www.kdnuggets.com/2019/11/github-repo-raider-automated-machine-learning.html)！更具体地说，自动化机器学习是使用自动化技术，无论是学习的方法还是简单的启发式规则，用于算法选择、超参数调优、架构设计或任何其他机器学习实现的部分。

切换话题，[印第安纳·琼斯](https://en.wikipedia.org/wiki/Indiana_Jones)是银幕上最伟大的角色之一。[夺宝奇兵](https://en.wikipedia.org/wiki/Raiders_of_the_Lost_Ark)，这个角色首次出现的电影，是个人最爱，受到数百万人的喜爱。其余的（目前的）四部曲电影质量参差不齐，但即便是最差的印第安纳·琼斯电影也比95%的现有影院作品要好。

印第安纳·琼斯与机器学习或GitHub有什么关系？其实没多少。但我们将借用他第一部电影的概念——寻找有价值的东西（在这种情况下是古老的遗物）——并将其应用于在GitHub上寻找有价值的开源代码库。

我们为什么要做这个？

> “财富与荣耀，小子。财富与荣耀。”
> 
> —亨利（印第安纳）琼斯博士

因此，在一篇略带印第安纳·琼斯风格的文章中，让我们看看GitHub上一些最受欢迎的自动化机器学习资源——无论是项目还是论文库。

> “小伙子，启动引擎！”
> 
> —亨利·琼斯博士，杰出

### 项目

这些项目涵盖了从帮助算法选择到协助超参数调优、架构设计及其他方面的内容。它们设计用于线性机器学习算法、集成方法和/或神经网络。它们都相互关联，因为它们都是Python项目，或者至少具有Python API。

> “蛇。为什么一定要蛇？”
> 
> —还有亨利·琼斯博士，杰出

这些项目没有特定的顺序，除了我试图按照它们支持的能力和算法的复杂程度来安排。虽然不完美，但也算是一种尝试。

**[auto-sklearn](https://github.com/automl/auto-sklearn)**

> auto-sklearn是一个自动化机器学习工具包，是scikit-learn估计器的即插即用替代品。
> 
> auto-sklearn 使机器学习用户无需进行算法选择和超参数调优。它利用了贝叶斯优化、元学习和集成构建的最新优势。通过阅读我们在 NIPS 2015 上发表的论文，了解 auto-sklearn 背后的技术。

**[TPOT: 基于树的管道优化工具](https://github.com/EpistasisLab/tpot)**

> TPOT 代表基于树的管道优化工具。将 TPOT 视为你的数据科学助手。TPOT 是一个 Python 自动化机器学习工具，通过遗传编程优化机器学习管道。
> 
> TPOT 将通过智能地探索成千上万的可能管道来自动化机器学习中最繁琐的部分，从而找到最适合你数据的管道。
> 
> 一旦 TPOT 完成搜索（或者你厌倦了等待），它会提供最佳管道的 Python 代码，以便你可以从那里进行调整。
> 
> TPOT 构建于 scikit-learn 之上，因此它生成的所有代码看起来都应该很熟悉……如果你对 scikit-learn 熟悉的话。

**[Featuretools](https://github.com/FeatureLabs/featuretools)**

> Featuretools 是一个执行自动化特征工程的框架。它擅长将时间序列和关系型数据集转换为适用于机器学习的特征矩阵。

**[SMAC3: 基于序列的模型算法配置](https://github.com/automl/SMAC3)**

> SMAC 是一个用于算法配置的工具，用于优化一组实例中任意算法的参数。这也包括机器学习算法的超参数优化。其核心包括贝叶斯优化和一种积极的竞赛机制，以高效决定两个配置中的哪一个表现更好。

**[AlphaPy](https://github.com/ScottfreeLLC/AlphaPy)**

> AlphaPy 是一个面向投机者和数据科学家的机器学习框架。它使用 Python 编写，并结合了 scikit-learn 和 pandas 库，以及许多其他有用的特征工程和可视化库。

**[MLBox](https://github.com/AxeldeRomblay/MLBox)**

> MLBox 是一个强大的自动化机器学习 Python 库。它提供以下功能：
> 
> +   快速读取和分布式数据预处理/清洗/格式化
> +   
> +   高度稳健的特征选择和泄露检测
> +   
> +   高维空间中的精确超参数优化
> +   
> +   最先进的分类和回归预测模型（深度学习、堆叠、LightGBM 等）
> +   
> +   带有模型解释的预测

**[AutoKeras](https://github.com/keras-team/autokeras)**

> Auto-Keras是一个开源软件库，用于自动化机器学习（AutoML）。它由德克萨斯A&M大学的数据实验室和社区贡献者开发。AutoML的**终极目标**是为具有有限数据科学或机器学习背景的领域专家提供易于访问的深度学习工具。Auto-Keras提供了自动搜索深度学习模型架构和超参数的功能。

**[Keras Tuner](https://github.com/keras-team/keras-tuner)**

> 一个用于Keras的超参数调优器，特别适用于TensorFlow 2.0的tf.keras。
> 
> 下面是如何使用随机搜索进行单层密集神经网络的超参数调优。
> 
> 首先，我们定义一个模型构建函数。它接受一个参数hp，你可以从中采样超参数，比如hp.Int('units', min_value=32, max_value=512, step=32)（一个从特定范围内的整数）。

**[NNI: Neural Network Intelligence](https://github.com/microsoft/nni)**

> NNI（Neural Network Intelligence）是一个工具包，帮助用户运行自动化机器学习（AutoML）实验。该工具调度并运行由调优算法生成的试验任务，以搜索最佳的神经网络架构和/或超参数，支持在本地机器、远程服务器和云环境中进行。

**[Auto-PyTorch](https://github.com/automl/Auto-PyTorch)**

> 自动化架构搜索和PyTorch的超参数优化。
> 
> 这是我们即将推出的Auto-PyTorch的一个非常早期的预Alpha版本。到目前为止，Auto-PyTorch支持特征化数据（分类、回归）和图像数据（分类）。

**[AdaNet](https://github.com/tensorflow/adanet)**

> AdaNet是一个轻量级的基于TensorFlow的框架，用于自动学习高质量模型，尽可能减少专家干预。AdaNet建立在最新的AutoML工作之上，既快速又灵活，同时提供学习保障。重要的是，AdaNet不仅提供了一个通用框架来学习神经网络架构，还能学习集成方法，以获得更好的模型。

**[Ludwig](https://github.com/uber/ludwig)**

> Ludwig是一个建立在TensorFlow之上的工具箱，允许训练和测试深度学习模型，而无需编写代码。
> 
> 它可以被从业者用来快速训练和测试深度学习模型，也可以被研究人员用来获取强有力的基准进行比较，并提供一个实验设置，确保通过执行标准数据预处理和可视化来保证可比性。
> 
> Ludwig提供了一组模型架构，这些架构可以组合在一起，为特定用例创建一个端到端的模型。作为类比，如果深度学习库提供了建造你的建筑物的构件，那么Ludwig提供了建造你城市的建筑物，你可以从可用的建筑物中选择，或将自己的建筑物添加到可用建筑物的集合中。

### 论文

当涉及自动化机器学习时，这个评论是否与你类似？

> “你正在干预你无法理解的力量。”
> 
> —马库斯·布罗迪

这不应该如此，你可以通过阅读一些最重要、最具影响力和解释性的研究论文来改变这种状况。

下面这对仓库可以用来筛选这些论文。

**[超赞的 AutoML 论文](https://github.com/hibayesian/awesome-automl-papers)**

> Awesome-AutoML-Papers 是一个精选的自动化机器学习论文、文章、教程、幻灯片和项目的列表。给这个仓库加星标，你就能跟上这一蓬勃研究领域的最新进展。感谢所有为这个项目做出贡献的人。加入我们，你也可以成为贡献者。

**[超赞的 AutoML](https://github.com/windmaple/awesome-AutoML)**

> 筛选与 AutoML 相关的研究、工具、项目和其他资源。

这总结了我们略显《夺宝奇兵》风格的自动化机器学习 GitHub 仓库探索。是否会有后续章节？就像即将到来的（???）[印第安纳·琼斯 5](https://en.wikipedia.org/wiki/Indiana_Jones_(franchise)#Untitled_fifth_film_(2021))一样，我们只能拭目以待。

**相关**：

+   [为你的模型自动调整超参数](/2019/09/automate-hyperparameter-tuning-models.html)

+   [如何通过机器学习在 GitHub 上自动化任务以获得乐趣和利润](/2019/05/automate-tasks-github-machine-learning-fun-profit.html)

+   [神经网络架构搜索研究指南](/2019/10/research-guide-neural-architecture-search.html)

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT 需求

* * *

### 更多相关话题

+   [免费 Python 自动化课程](https://www.kdnuggets.com/2022/07/free-automate-python-course.html)

+   [3 个有用的 Python 自动化脚本](https://www.kdnuggets.com/2022/11/3-useful-python-automation-scripts.html)

+   [数据科学工作流中的自动化](https://www.kdnuggets.com/2023/03/automation-data-science-workflows.html)

+   [10 个 GitHub 仓库掌握机器学习](https://www.kdnuggets.com/10-github-repositories-to-master-machine-learning)

+   [GitHub Actions 入门教程](https://www.kdnuggets.com/github-actions-for-machine-learning-beginners)

+   [从这些 GitHub 仓库中学习机器学习](https://www.kdnuggets.com/2023/01/learn-machine-learning-github-repositories.html)
