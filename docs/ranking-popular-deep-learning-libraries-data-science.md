# 排名受欢迎的深度学习库用于数据科学

> 原文：[https://www.kdnuggets.com/2017/10/ranking-popular-deep-learning-libraries-data-science.html](https://www.kdnuggets.com/2017/10/ranking-popular-deep-learning-libraries-data-science.html)

**作者：[Rachel Allen](https://github.com/raykallen/) 和 [Michael Li](https://github.com/tianhuil/)，The Data Incubator。**

*在[The Data Incubator](https://www.thedataincubator.com/)我们以拥有最先进的数据科学课程而自豪。我们的课程很大程度上基于来自企业和政府合作伙伴的反馈，关于他们使用和学习的技术。除了这些反馈，我们还希望开发一个数据驱动的方法来确定我们应该在我们的[数据科学企业培训](https://www.thedataincubator.com/training.html)和我们的[免费奖学金](https://www.thedataincubator.com/fellowship.html)中教授什么内容，面向希望进入数据科学行业的硕士和博士生。以下是结果。*

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速通道进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT工作

* * *

**排名**

以下是基于Github和Stack Overflow活动以及Google搜索结果的23个开源深度学习库的排名。这张表格显示了标准化分数，其中值为1表示高于平均水平一个标准差（平均值= 0分）。例如，Caffe在Github活动中高于平均水平一个标准差，而deeplearning4j接近平均水平。请参见下面的方法。

![](../Images/43d766d411a32c95b4acc73842d5379e.png)

**结果与讨论**

排名基于三个组件的等权重：Github（stars和forks）、Stack Overflow（tags和questions）以及Google结果（总数和季度增长率）。这些数据是通过可用的API获取的。制定一个全面的深度学习工具包列表是棘手的 - 最终，我们抓取了五个我们认为具有代表性的列表（详情请见下面的方法）。计算每个指标的标准化分数可以让我们看到哪些包在每个类别中脱颖而出。[完整排名在这里](https://github.com/thedataincubator/data-science-blogs/blob/master/output/DL_libraries_final_Rankings.csv)，而[原始数据在这里](https://github.com/thedataincubator/data-science-blogs/blob/master/output/deep_learning_data.csv)。

**TensorFlow** **以最大规模的活跃社区主导领域**

TensorFlow 在所有计算指标中至少比均值高出两个标准差。TensorFlow 的 Github fork 数量几乎是第二受欢迎框架 Caffe 的三倍，Stack Overflow 上的问题数量超过了 Caffe 的六倍。TensorFlow 于 2015 年由 Google Brain 团队首次开源，已经超越了 Theano (4) 和 Torch (8) 等更资深的库，成为我们列表中的顶尖位置。虽然 TensorFlow 是一个运行在 C++ 引擎上的 Python API，但我们列表中的几个库可以将 TensorFlow 用作后端，并提供它们自己的接口。这些库包括 Keras (2)，它将 [很快成为核心 TensorFlow 的一部分](https://twitter.com/fchollet/status/820746845068505088) 和 Sonnet (6)。TensorFlow 的受欢迎程度可能是由于其通用的深度学习框架、灵活的接口、漂亮的计算图可视化以及 Google 的大量开发者和社区资源。

**Caffe 尚未被 Caffe2 替代**

Caffe 在我们的列表中稳居第三位，其 Github 活动比所有竞争对手（不包括 TensorFlow）都要多。Caffe 通常被认为比 TensorFlow 更专业，主要开发用于图像处理、物体识别和预训练卷积神经网络。Facebook 于 2017 年 4 月发布了 Caffe2 (11)，它已经在深度学习库中排名前半。Caffe2 是 Caffe 的更轻量级、模块化和可扩展的版本，包含递归神经网络。Caffe 和 Caffe2 是独立的库，因此数据科学家可以继续使用原始的 Caffe。然而，像 [Caffe Translator](https://github.com/caffe2/caffe2/blob/master/caffe2/python/caffe_translator.py) 这样的迁移工具提供了使用 Caffe2 驱动现有 Caffe 模型的方式。

**Keras 是深度学习最受欢迎的前端**

Keras (2) 是排名最高的非框架库。Keras 可以作为 TensorFlow (1)、Theano (4)、MXNet (7)、CNTK (9) 或 deeplearning4j (14) 的前端。Keras 在所有三项度量指标中表现优于平均水平。Keras 的受欢迎程度可能归因于其简洁性和易用性。Keras 允许快速原型设计，但牺牲了一些直接使用框架时的灵活性和控制力。数据科学家在数据集上尝试深度学习时，常常偏爱 Keras。Keras 的发展和受欢迎程度持续增长，R Studio 最近发布了 [一个接口](https://keras.rstudio.com/) 供 Keras 在 R 中使用。

**Theano 即使没有大规模的行业支持仍继续保持顶尖位置**

在众多新的深度学习框架中，Theano（4）是我们排名中最古老的库。Theano开创了计算图的使用，并且在深度学习和机器学习的研究社区中仍然很受欢迎。Theano本质上是一个Python的数值计算库，但可以与像Lasagne（15）这样的高级深度学习封装库一起使用。虽然谷歌支持TensorFlow（1）和Keras（2），Facebook支持PyTorch（5）和Caffe2（11），MXNet（7）是Amazon Web Services的官方深度学习框架，而微软设计并维护CNTK（9），Theano依然在没有来自技术行业巨头的官方支持下保持受欢迎。

**Sonnet是增长最快的库**

2017年初，谷歌的DeepMind公开发布了Sonnet（6）的代码，这是一个建立在TensorFlow之上的高级面向对象库。与上一季度相比，Google搜索结果中Sonnet的页面数量增长了272%，是我们排名中的所有库中增长最多的。尽管谷歌在2014年收购了这家英国人工智能公司，DeepMind和Google Brain仍然保持着大致独立的团队。DeepMind专注于人工通用智能，而Sonnet可以帮助用户在其特定的人工智能想法和研究基础上进行构建。

**Python是深度学习接口的语言**

PyTorch（5），一个唯一接口在Python中的框架，是我们列表中增长第二快的库。与上季度相比，PyTorch的Google搜索结果增加了236%。在我们排名的23个开源深度学习框架和封装库中，只有三个没有Python接口：Dlib（10），MatConvNet（20），和OpenNN（23）。C++和R接口仅在23个库中的七个和六个中提供。尽管数据科学社区在使用Python方面基本达成了共识，但深度学习库仍然有许多选择。

**限制**

像任何分析一样，在过程中做出了一些决策。所有源代码和数据都在[我们的Github页面](https://github.com/thedataincubator/data-science-blogs)上。深度学习库的完整列表来自几个来源。

自然，使用时间较长的一些库将有更高的指标，因此排名也更高。唯一考虑到这一点的指标是Google搜索的季度增长率。

数据呈现出一些困难：

+   neural designer和Wolfram Mathematica是专有的，已被移除

+   cntk也称为微软认知工具包，但我们只使用了原始的ctnk名称。

+   neon已更改为nervana neon

+   paddle已更改为paddlepaddle

+   一些库显然是其他库的衍生品，如Caffe和Caffe2。我们决定如果库有独特的github存储库，则单独对待这些库。

**方法**

所有源代码和数据都在[我们的Github页面](https://github.com/thedataincubator/data-science-blogs)上。

我们首先生成了一个包含23个开源深度学习库的列表 [从](https://svds.com/understanding-ai-toolkits/) [每个](https://en.wikipedia.org/wiki/Comparison_of_deep_learning_software) [五个](https://www.packtpub.com/books/content/top-10-deep-learning-frameworks) [不同](https://twitter.com/fchollet/status/882995652233371648) [来源](https://svds.com/wp-content/uploads/2017/02/Deep_learning_ratings_final-1024x563.png)，然后收集了所有这些库的指标，以得出排名。Github数据基于星标和分叉，Stack Overflow数据基于标签和包含包名称的问题，Google结果基于过去五年的Google搜索结果总数和过去三个月相较于前后三个月的结果季度增长率。

一些其他说明：

+   由于几个库的名称是常见词（如caffe，chainer，lasagne），因此用来确定Google搜索结果数量的搜索词包括库名称和“深度学习”一词。

    ing"。

+   任何不可用的Stack Overflow计数被转换为零计数。

+   计数标准化为均值0和标准差1，然后平均得到Github和Stack Overflow评分，并结合搜索结果计算总体评分。

+   一些手动检查用于确认Github存储库位置。

所有数据均于2017年9月14日下载。

**资源**

源代码可在 [The Data Incubator](https://www.thedataincubator.com/)'s [GitHub](https://github.com/thedataincubator/data-science-blogs/) 上获取。如果你有兴趣了解更多，请考虑

+   [数据科学企业培训](https://www.thedataincubator.com/training.html)

+   [为希望进入行业的硕士和博士提供的免费八周奖学金](https://www.thedataincubator.com/fellowship.html)

+   [招聘数据科学家](https://www.thedataincubator.com/hiring.html)

**简介：[Michael Li](https://github.com/tianhuil/)** 曾在（Foursquare）担任数据科学家，在（D.E. Shaw，J.P. Morgan）担任量化分析师，以及在（NASA）担任火箭科学家。他在普林斯顿获得了赫兹奖学金博士学位，并作为马歇尔奖学金获得者在剑桥大学学习了数学三部分。在Foursquare，Michael 发现他最喜欢的工作部分是教授和指导聪明的人关于数据科学的知识。他决定创建一个初创公司，让他专注于自己真正喜欢的事情。

### 更多相关主题

+   [成为出色数据科学家需要的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家都应该掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [每个数据科学家都应该了解的三种R库（即使你使用Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [停止学习数据科学以寻找目的，寻找目的以…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [数据科学学习统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)
