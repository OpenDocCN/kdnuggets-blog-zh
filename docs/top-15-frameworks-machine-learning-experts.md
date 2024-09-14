# 机器学习专家的前 15 个框架

> 原文：[https://www.kdnuggets.com/2016/04/top-15-frameworks-machine-learning-experts.html](https://www.kdnuggets.com/2016/04/top-15-frameworks-machine-learning-experts.html)

[评论](#comments)

![机器学习框架](../Images/7ffbfa274264b04505bf3141af100194.png)

机器学习工程师是工程团队的一部分，负责构建产品和算法，确保其可靠、快速并能够大规模运作。他们与数据科学家紧密合作，以了解理论和业务方面的内容。在机器学习的背景下，主要的区别总结如下：

+   机器学习工程师构建、实施和维护生产环境中的机器学习系统。

+   数据科学家进行研究以生成关于机器学习项目的想法，并进行分析以理解机器学习系统的指标影响。

以下是机器学习工程师框架的列表：

+   [Apache Singa](http://singa.apache.org/docs/overview.html) 是一个通用的分布式深度学习平台，用于在大型数据集上训练大型深度学习模型。它设计了一个基于层抽象的直观编程模型。支持各种流行的深度学习模型，包括前馈模型（如卷积神经网络 CNN）、能量模型（如限制玻尔兹曼机 RBM）和递归神经网络 RNN。提供了许多内置的层供用户使用。

+   [Amazon Machine Learning](https://aws.amazon.com/machine-learning/) 是一项使各技能水平的开发者能够轻松使用机器学习技术的服务。Amazon Machine Learning 提供可视化工具和向导，指导你创建机器学习 (ML) 模型，而无需学习复杂的 ML 算法和技术。它连接到存储在 Amazon S3、Redshift 或 RDS 中的数据，并可以在这些数据上运行二元分类、多类分类或回归，以创建模型。

+   [Azure ML Studio](https://studio.azureml.net/) 允许 Microsoft Azure 用户创建和训练模型，然后将其转化为可以被其他服务使用的 API。用户每个账户可以获得最多 10GB 的存储空间用于模型数据，不过你也可以将自己的 Azure 存储连接到该服务以支持更大的模型。提供了各种算法，既包括 Microsoft 自家的，也包括第三方的。你甚至不需要账户就可以试用该服务；你可以匿名登录，并使用 Azure ML Studio 达到八小时。

+   [Caffe](http://caffe.berkeleyvision.org/) 是一个以表达、速度和模块化为目标的深度学习框架。它由伯克利视觉与学习中心（[BVLC](http://bvlc.eecs.berkeley.edu/)）和社区贡献者开发。[杨庆佳](http://daggerfs.com/) 在 UC Berkeley 博士期间创建了该项目。Caffe 在 [BSD 2-Clause license](https://github.com/BVLC/caffe/blob/master/LICENSE) 许可下发布。模型和优化通过配置定义，无需硬编码，用户可以在 CPU 和 GPU 之间切换。速度使得 Caffe 适合研究实验和工业部署。Caffe 可以在单个 NVIDIA K40 GPU 上处理超过 60M 图像每天。

+   [H2O](http://www.h2o.ai/) 使任何人都能轻松应用数学和预测分析来解决当今最具挑战性的业务问题。它智能地结合了其他机器学习平台中目前没有的独特功能，包括：最佳开源技术、易于使用的 WebUI 和熟悉的界面、对所有常见数据库和文件类型的数据无关支持。使用 H2O，你可以使用现有的语言和工具进行工作。此外，你可以将平台无缝扩展到你的 Hadoop 环境中。

+   [大规模在线分析 (MOA)](http://moa.cms.waikato.ac.nz/) 是最受欢迎的开源数据流挖掘框架，拥有一个非常活跃的成长社区。它包括一系列机器学习算法（[分类，回归](http://moa.cms.waikato.ac.nz/details/classification/)，[聚类](http://moa.cms.waikato.ac.nz/details/stream-clustering/)，[异常检测](http://moa.cms.waikato.ac.nz/details/outlier-detection/)，概念漂移检测和[推荐系统](http://moa.cms.waikato.ac.nz/details/recommender-systems/)）和评估工具。MOA 与 WEKA 项目相关，也用 Java 编写，同时能够扩展到更具挑战性的问题上。

+   [MLlib (Spark)](http://spark.apache.org/mllib/) 是 Apache Spark 的机器学习库。其目标是使实际的机器学习可扩展且简单。它包括常见的学习算法和工具，包括分类、回归、聚类、协同过滤、降维，以及较低级别的优化原语和较高级别的管道 API。

+   [mlpack](http://mlpack.org/) 是一个基于 C++ 的机器学习库，最初于 2011 年推出，旨在“可扩展性、速度和易用性”，根据库的创建者。实现 mlpack 可以通过一组命令行可执行文件进行快速“黑箱”操作，也可以通过 C++ API 进行更复杂的工作。Mlpack 提供这些算法作为简单的命令行程序和 C++ 类，然后可以集成到更大规模的机器学习解决方案中。

+   [Pattern](http://www.clips.ua.ac.be/pattern) 是一个用于 Python 编程语言的网络挖掘模块。它提供了数据挖掘工具（Google、Twitter 和 Wikipedia API、网页爬虫、HTML DOM 解析器）、自然语言处理工具（词性标注、n-gram 搜索、情感分析、WordNet）、机器学习工具（向量空间模型、聚类、SVM）、网络分析和 <canvas> 可视化工具。

+   [Scikit-Learn](http://scikit-learn.org/stable/) 通过在现有 Python 包——NumPy、SciPy 和 matplotlib——之上构建，充分发挥了 Python 的广度。生成的库可以用于交互式的“工作台”应用，也可以嵌入到其他软件中并重复使用。该工具包在 BSD 许可证下提供，因此完全开放和可重用。Scikit-learn 包含许多标准 [机器学习 *任务*](http://strata.oreilly.com/2013/09/gaining-access-to-the-best-machine-learning-methods.html)（如聚类、分类、回归等）所需的工具。由于 scikit-learn 由 [大量开发者和机器学习专家的社区](https://github.com/scikit-learn/scikit-learn/graphs/contributors) 开发，有前途的新技术通常会在相当短的时间内被纳入。

+   [Shogun](http://www.shogun-toolbox.org/) 是最古老、最受尊敬的机器学习库之一，Shogun 创建于 1999 年，用 C++ 编写，但不限于 C++。得益于 [SWIG 库](http://www.swig.org/)，Shogun 可以在 Java、Python、C#、Ruby、R、Lua、Octave 和 Matlab 等语言和环境中透明使用。Shogun 旨在统一大规模学习，适用于广泛的特征类型和学习设置，如分类、回归或探索性数据分析。

+   [TensorFlow](https://www.tensorflow.org/) 是一个用于数值计算的开源软件库，通过数据流图来实现。TensorFlow 实现了所谓的数据流图，其中一批数据（“张量”）可以通过图中描述的一系列算法进行处理。数据在系统中的流动称为“流”——因此得名。图可以使用 C++ 或 Python 组装，并可以在 CPU 或 GPU 上处理。

+   [Theano](http://deeplearning.net/software/theano/) 是一个 Python 库，允许你定义、优化和评估数学表达式，特别是多维数组（numpy.ndarray）的表达式。使用 Theano，可以达到与手工编写的 C 实现相媲美的速度，适用于涉及大量数据的问题。它是在 [LISA](http://www.iro.umontreal.ca/rubrique.php3?id_rubrique=27) 实验室编写的，以支持高效机器学习算法的快速开发。Theano 的名字源自 [希腊数学家](https://en.wikipedia.org/wiki/Theano_(mathematician))，她可能是毕达哥拉斯的妻子。Theano 在 BSD 许可证下发布。

+   [Torch](http://torch.ch/) 是一个科学计算框架，广泛支持以 GPU 为主的机器学习算法。由于其易用高效的脚本语言 LuaJIT 和底层的 C/CUDA 实现，Torch 易于使用且高效。Torch 的目标是最大限度地提高科学算法的灵活性和速度，同时使过程极其简单。Torch 拥有一个**[由社区驱动的大型生态系统](https://github.com/torch/torch7/wiki/Cheatsheet)**，涵盖机器学习、计算机视觉、信号处理、并行处理、图像、视频、音频和网络等领域，并建立在 Lua 社区之上。

+   [Veles](https://velesnet.ml/) 是一个用于深度学习应用的分布式平台，虽然它使用 Python 来执行节点之间的自动化和协调，但其核心代码是用 C++ 编写的。数据集可以在输入集群之前进行分析和自动归一化，而 REST API 使训练后的模型能够立即投入生产。它专注于性能和灵活性。它几乎没有硬编码实体，能够训练所有广泛认可的拓扑结构，如全连接网络、卷积网络、递归网络等。

在评论中告诉我们更多关于你喜欢的机器学习框架的事宜。

**简介: [Devendra Desale](https://www.linkedin.com/in/devendra-desale-91872847)([@DevendraDesale](https://twitter.com/DevendraDesale))** 是一名数据科学研究生，目前从事文本挖掘和大数据技术工作。他还对企业架构和数据驱动的业务感兴趣。在离开电脑时，他也喜欢参加聚会和探索未知领域。

**相关**:

+   [Github 上的前 10 个数据科学资源](/2016/03/top-10-data-science-github.html)

+   [Github 上的前 10 个数据可视化项目](/2016/02/top-10-data-visualization-github.html)

+   [数据科学工具 – 专有供应商仍然重要吗？](/2016/03/data-science-tools-proprietary-vendors-vs-open-source.html)

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 维护

* * *

### 更多相关内容

+   [专家推荐的 5 种最佳机器学习实践](https://www.kdnuggets.com/2022/09/top-5-machine-learning-practices-recommended-experts.html)

+   [接近机器学习过程的框架](https://www.kdnuggets.com/2018/05/general-approaches-machine-learning-process.html)

+   [PyTorch还是TensorFlow？比较流行的机器学习框架](https://www.kdnuggets.com/2022/02/packt-pytorch-tensorflow-comparing-popular-machine-learning-frameworks.html)

+   [2023年你应该考虑的顶级AutoML框架](https://www.kdnuggets.com/2023/05/best-automl-frameworks-2023.html)

+   [Chip Huyen分享了实施机器学习系统的框架和案例研究](https://www.kdnuggets.com/2023/02/sphere-chip-huyen-shares-frameworks-case-studies-implementing-ml-systems.html)

+   [关于开发安全、可靠和值得信赖的AI框架的专家见解](https://www.kdnuggets.com/expert-insights-on-developing-safe-secure-and-trustworthy-ai-frameworks)
