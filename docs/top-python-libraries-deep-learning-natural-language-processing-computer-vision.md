# 深度学习、自然语言处理与计算机视觉的顶级 Python 库

> 原文：[https://www.kdnuggets.com/2020/11/top-python-libraries-deep-learning-natural-language-processing-computer-vision.html](https://www.kdnuggets.com/2020/11/top-python-libraries-deep-learning-natural-language-processing-computer-vision.html)

[评论](#comments)

在之前的文章中，我们查看了 [数据科学、数据可视化和机器学习的顶级 Python 库](/2020/11/top-python-libraries-data-science-data-visualization-machine-learning.html)。这次，我们将关注深度学习、自然语言处理和计算机视觉的顶级库。这些类别实际上不需要进一步的说明。

这种分隔和分类是随意的，有时比其他情况更多，但我们尽力将工具按照预期的使用场景进行分组，希望这对读者最有帮助。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

显然，现如今并非所有的 NLP 和 CV 工作都采用深度学习技术，但随着趋势向这种技术转变以获得最先进的结果，我们坚持这种否则随意的分类逻辑。

我们的列表由我们团队共同决定的、代表常用且广泛使用的 Python 库组成。此外，要被列入其中，库必须有一个 Github 仓库。类别没有特定顺序，库在每个类别中也没有特定顺序。我们曾考虑按星数或其他指标进行排序，但为了避免对库的任何感知价值或重要性进行明确偏向，我们决定不这样做。因此，它们在这里的列出是纯粹随机的。库的描述直接来自于 Github 仓库，以某种形式展示。

再次感谢 [Ahmed Anis](https://www.kdnuggets.com/author/ahmad-anis) 对数据收集的贡献，以及 KDnuggets 团队其他成员的意见、见解和建议。

请注意，下图由 [Gregory Piatetsky](https://www.kdnuggets.com/author/gregory-piatetsky) 展示了每个库的类型，按星数和贡献者进行绘制，符号大小反映了库在 Github 上的提交次数，采用对数刻度。

![图](../Images/ea3153d3e93ca65fa6291b2f432b9e70.png)**图 1：顶级 Python 库，用于深度学习、自然语言处理和计算机视觉**

按星标数和贡献者数绘制；相对大小由提交日志数决定

那么，废话少说，这里是 KDnuggets 员工评定的 30 个顶级 Python 库，用于深度学习、自然语言处理和计算机视觉。

### 深度学习

**1\. [TensorFlow](https://github.com/tensorflow/tensorflow)**

星标：149000，提交：97741，贡献者：2754

> TensorFlow 是一个端到端的开源机器学习平台。它拥有全面且灵活的工具、库和社区资源，使研究人员能够推动机器学习的最新技术，并使开发人员能够轻松构建和部署机器学习驱动的应用程序。

**2\. [Keras](https://github.com/keras-team/keras)**

星标：50000，提交：5349，贡献者：864

> Keras 是一个用 Python 编写的深度学习 API，运行在机器学习平台 TensorFlow 之上。

**3\. [PyTorch](https://github.com/pytorch/pytorch)**

星标：43200，提交：30696，贡献者：1619

> Python 中的张量和动态神经网络，具有强大的 GPU 加速

**4\. [fastai](https://github.com/fastai/fastai)**

星标：19800，提交：1450，贡献者：607

> fastai 简化了使用现代最佳实践训练快速且准确的神经网络

**5\. [PyTorch Lightning](https://github.com/PyTorchLightning/pytorch-lightning)**

星标：9600，提交：3594，贡献者：317

> 用于高性能 AI 研究的轻量级 PyTorch 封装。扩展你的模型，而不是样板代码。

**6\. [JAX](https://github.com/google/jax)**

星标：10000，提交：5708，贡献者：221

> Python+NumPy 程序的可组合变换：微分、矢量化、JIT 到 GPU/TPU 等等

**7\. [MXNet](https://github.com/apache/incubator-mxnet)**

星标：19100，提交：11387，贡献者：839

> 轻量级、可移植、灵活的分布式/移动深度学习，支持动态、感知变异的数据流调度器；适用于 Python、R、Julia、Scala、Go、JavaScript 等更多语言

**8\. [Ignite](https://github.com/pytorch/ignite)**

星标：3100，提交：747，贡献者：112

> 高级库，帮助灵活且透明地训练和评估 PyTorch 中的神经网络。

### 自然语言处理

**9\. [FastText](https://github.com/facebookresearch/fastText)**

星标：21700，提交：379，贡献者：47

> fastText 是一个用于高效学习词表示和句子分类的库。

**10\. [spaCy](https://github.com/explosion/spaCy)**

星标：17400，提交：11628，贡献者：482

> 工业级自然语言处理（NLP），使用 Python 和 Cython

**11\. [gensim](https://github.com/RaRe-Technologies/gensim)**

星标：11200，提交：4024，贡献者：361

> Gensim 是一个 Python 库，用于主题建模、文档索引和大规模语料库的相似性检索。目标用户是自然语言处理（NLP）和信息检索（IR）社区。

**12\. [NLTK](https://github.com/nltk/nltk)**

Stars: 9300, Commits: 13990, Contributors: 319

> NLTK -- 自然语言工具包 -- 是一个开源 Python 模块、数据集和教程的套件，支持自然语言处理的研究和开发。

**13\. [Datasets (Huggingface)](https://github.com/huggingface/datasets)**

Stars: 4300, Commits: 568, Contributors: 64

> 快速、高效、开放访问的数据集和评估指标，适用于自然语言处理以及 PyTorch、TensorFlow、NumPy 和 Pandas。

**14\. [Tokenizers (Huggingface)](https://github.com/huggingface/tokenizers)**

Stars: 3800, Commits: 1252, Contributors: 30

> 快速的最先进的分词器，优化用于研究和生产。

**15\. [Transformers (Huggingface)](https://github.com/huggingface/transformers)**

Stars: 3500, Commits: 5480, Contributors: 585

> Transformers：最先进的自然语言处理，适用于 Pytorch 和 TensorFlow 2.0。

**16\. [Stanza](https://github.com/stanfordnlp/stanza/)**

Stars: 4800, Commits: 1514, Contributors: 19

> 官方斯坦福 NLP Python 库，支持多种人类语言。

**17\. [TextBlob](https://github.com/sloria/textblob)**

Stars: 7300, Commits: 542, Contributors: 24

> 简单、Pythonic 的文本处理--情感分析、词性标注、名词短语提取、翻译等。

**18\. [PyTorch-NLP](https://github.com/PetrochukM/PyTorch-NLP)**

Stars: 1800, Commits: 442, Contributors: 15

> PyTorch 自然语言处理（NLP）的基础实用工具。

**19\. [Textacy](https://github.com/chartbeat-labs/textacy)**

Stars: 1500, Commits: 1324, Contributors: 23

> 一个 Python 库，用于执行各种自然语言处理（NLP）任务，基于高性能的 spaCy 库。

**20\. [Finetune](https://github.com/IndicoDataSolutions/finetune)**

Stars: 626, Commits: 1405, Contributors: 13

> Finetune 是一个库，允许用户利用最先进的预训练 NLP 模型进行各种下游任务。

**21\. [TextHero](https://github.com/jbesomi/texthero)**

Stars: 1900, Commits: 266, Contributors: 17

> 从零到英雄的文本预处理、表示和可视化。

**22\. [Spark NLP](https://github.com/JohnSnowLabs/spark-nlp)**

Stars: 1700, Commits: 4363, Contributors: 50

> Spark NLP 是一个构建在 Apache Spark ML 之上的自然语言处理库。

**23\. [GluonNLP](https://github.com/dmlc/gluon-nlp)**

Stars: 2200, Commits: 712, Contributors: 72

> GluonNLP 是一个工具包，能够简化文本预处理、数据集加载和神经模型构建，帮助你加速自然语言处理（NLP）研究。

### 计算机视觉

**24\. [Pillow](https://github.com/python-pillow/Pillow)**

Stars: 7800, Commits: 10799, Contributors: 303

> Pillow 是友好的 PIL 分支。PIL 是 Python Imaging Library。

**25\. [OpenCV](https://github.com/opencv/opencv)**

Stars: 49600, Commits: 29453, Contributors: 1234

> 开源计算机视觉库

**26\. [scikit-image](https://github.com/scikit-image/scikit-image)**

Stars: 4000, Commits: 12352, Contributors: 403

> Python 中的图像处理

**27\. [Mahotas](https://github.com/luispedro/mahotas)**

Stars: 644, Commits: 1273, Contributors: 25

> Mahotas 是一个快速计算机视觉算法库（全部用 C++ 实现以提高速度），操作 numpy 数组。

**28\. [Simple-CV](https://github.com/sightmachine/simplecv)**

Stars: 2400, Commits: 2625, Contributors: 69

> SimpleCV 是一个开源机器视觉框架，使用 OpenCV 和 Python 编程语言。

**29\. [GluonCV](https://github.com/dmlc/gluon-cv)**

Stars: 4300, Commits: 774, Contributors: 101

> GluonCV 提供了计算机视觉领域的最先进（SOTA）深度学习模型的实现。

**30\. [Torchvision](https://github.com/pytorch/vision)**

Stars: 7500, Commits: 1286, Contributors: 334

> torchvision 包含流行的数据集、模型架构和计算机视觉的常见图像变换。

**相关**:

+   [数据科学、数据可视化与机器学习的顶级 Python 库](/2020/11/top-python-libraries-data-science-data-visualization-machine-learning.html)

+   [前 13 个 Python 深度学习库](/2018/11/top-python-deep-learning-libraries.html)

+   [前 8 个 Python 机器学习库](/2018/10/top-python-machine-learning-libraries.html)

### 更多相关内容

+   [每个数据科学家都应该了解的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [成为优秀数据科学家所需的 5 项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每位初学数据科学家都应该掌握的 6 个预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021 年最佳 ETL 工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [顶级自然语言处理库指南](https://www.kdnuggets.com/2023/04/guide-top-natural-language-processing-libraries.html)

+   [是什么让 Python 成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)
