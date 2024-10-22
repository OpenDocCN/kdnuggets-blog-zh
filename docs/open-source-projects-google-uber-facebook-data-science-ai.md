# 谷歌、优步和 Facebook 的数据科学与人工智能开源项目

> 原文：[`www.kdnuggets.com/2019/11/open-source-projects-google-uber-facebook-data-science-ai.html`](https://www.kdnuggets.com/2019/11/open-source-projects-google-uber-facebook-data-science-ai.html)

comments ![figure-name](img/7011785e3dbfb93751de718ebf81c7e5.png)

开源正成为分享和改进技术的标准。一些世界上最大的组织，如谷歌、Facebook 和优步，正在将它们在工作流程中使用的技术开源给公众。这使得普通人能够利用在全球最大公司中使用的技术。可能最知名的开源项目是 PyTorch 和 Tensorflow（这两个项目恰巧都是深度学习的事实标准）。

### [Facebook 开源项目](https://opensource.facebook.com/#artificial-intelligence)

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

![figure-name](img/a56724021f25208884e75e1700ac642b.png)[来源](https://opensource.facebook.com/)

1.  [**PyTorch**](https://pytorch.org/)

![figure-name](img/7f09c8ba15be2949855d568a9e41fa0a.png)[来源](https://pytorch.org/)

+   PyTorch 基本上是数据科学社区中最著名的深度学习库。它拥有一个丰富的[生态系统](https://pytorch.org/ecosystem/)，数据科学家可以利用这些工具进行各种任务。可用的一些工具包括用于贝叶斯优化的[BoTorch](https://botorch.org/)、用于设计和使用自然语言处理深度学习模型的[AllenNLP](https://allennlp.org/)、用于轻松构建和评估神经网络的[fastai](https://docs.fast.ai/)以及提供完整 scikit-learn 兼容性的高级接口[skorch](https://github.com/skorch-dev/skorch)。

1.  [**Prophet**](https://facebook.github.io/prophet/)

![figure-name](img/4376a84bac22cb14046be26c1b14fe16.png)[来源](https://facebook.github.io/prophet/)

+   Prophet 是一个开源时间序列预测库，提供**[Python](https://facebook.github.io/prophet/docs/quick_start.html#python-api) 和 [R](https://facebook.github.io/prophet/docs/quick_start.html#r-api)**的 API。它能够在具有高季节性的数据上表现良好，并能够考虑假期效应。它可以处理数据中的缺失值和异常值。时间序列中的一个大问题是缺失数据，因为数据应是顺序的，常见的做法是用均值或中位数填补缺失值（大多数情况下这不是时间序列中的最佳选择）。

### [Uber 的开源项目](https://uber.github.io/#/projects)

![figure-name](img/92c1115942c4c7c1081d73ef017f9771.png)[来源](https://uber.github.io/#/)

1.  [**CausalML**](https://github.com/uber/causalml)

![figure-name](img/29ff17f8a876ad080e842c76e46e9bc8.png)[来源](https://github.com/uber/causalml)

+   CausalML 是 Uber 提供的开源工具，用于提升建模和因果推断方法，采用机器学习技术。它允许用户从实验或观察数据中估计条件平均处理效应（CATE）或个体处理效应（ITE）。

1.  [**Ludwig**](https://uber.github.io/ludwig/index.html)

![figure-name](img/ae98d857007ec1ebfeaabfac3f93ffdd.png)[来源](https://uber.github.io/ludwig/)

+   Ludwig 可能是 Uber 最著名的开源项目。[Ludwig 允许用户训练和测试深度学习模型，而无需编写代码，只需指定 YAML](https://uber.github.io/ludwig/getting_started/)。它构建在 Tensorflow 之上。对于有偏好的用户，还提供了 Python API。

1.  [**Pyro**](https://github.com/pyro-ppl/pyro)

![figure-name](img/cb2924b08b2af1806b6f5e3331f5a5f1.png)[来源](https://github.com/pyro-ppl/pyro)

+   Pyro 由 Uber AI Labs 维护，并建立在 PyTorch 之上，用于深度概率编程。它建立在**通用、可扩展、最小和灵活**的原则上。正在开发一个名为 [NumPyro](https://github.com/pyro-ppl/numpyro) 的 Pyro 概率编程库的测试版，具有 NumPy 后端，以实现更快的处理速度。

1.  **[kepler.gl](https://kepler.gl/)**

![figure-name](img/4c9cd67c668f311bee482dd84e9c3974.png)[来源](https://kepler.gl/)

+   [Kepler.gl](https://kepler.gl/demo) 是 Uber 提供的开源地理空间分析工具箱，用于大规模数据集的处理。它旨在帮助数据科学家使用交互式和数据驱动的方法在位置数据上产生影响。它构建在 Mapbox GL 和 Deck.gl 之上。

### 谷歌开源项目

![figure-name](img/cf855eae922f45d29669a813490adc94.png)[来源](https://opensource.google/projects/explore/featured)

1.  **[Google Cloud Data Lab](https://cloud.google.com/datalab/)**

![figure-name](img/4decbea33292564c44dd9683b291adc9.png)[来源](https://opensource.google/projects/datalab)

+   Google Datalab 是一个交互式可视化探索工具，具有 IPython 后端，这意味着它拥有一个熟悉的 Jupyter 环境，因此经常使用 Jupyter 的用户会感到很熟悉。Cloud Datalab 使得可以使用 Python、SQL 和 JavaScript（用于 BigQuery 用户自定义函数）在 BigQuery、Cloud Machine Learning Engine、Compute Engine 和 Cloud Storage 上分析数据。***然而，如果决定使用云资源（如虚拟机和 Cloud Storage），则需要付费***。

1.  **[Tensorflow](https://opensource.google/projects/tensorflow)**

![figure-name](img/3bd98be21982f3bfc716210e707d0d28.png)[来源](https://opensource.google/projects/tensorflow)

+   这个框架无需介绍。Tensorflow 与 PyTorch 一起被视为数据科学社区的事实标准深度学习框架。Tensorflow 激发了许多扩展，以更好地利用库，从可视化到直接从命令库的生产 API。

1.  **[CausalImpact](https://opensource.google/projects/causalimpact)**

![figure-name](img/b4d473abfc86998c38b52d1ae34720a6.png)[来源](https://opensource.google/projects/causalimpact)

+   [CausalImpact](https://github.com/google/CausalImpact) 库是一个 R 库，用于估计设计干预对时间序列的因果效应。该库使用贝叶斯时间序列模型来估计事件发生时的效果，即使没有现实世界的证据。这在随机实验不可用或不可行时特别有用。

**相关：**

+   2018 年回顾：机器学习开源项目与框架

+   Facebook 一直在悄悄开源一些令人惊叹的 PyTorch 深度学习功能

+   LinkedIn、Uber、Lyft、Airbnb 和 Netflix 如何解决机器学习解决方案的数据管理和发现问题

### 更多相关话题

+   [成为一名优秀数据科学家所需的 5 个关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应该掌握的 6 种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021 年最佳 ETL 工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [停止学习数据科学以寻找目标，改为寻找目标…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [建立一个坚实的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)
