# MLDB：机器学习数据库

> 原文：[https://www.kdnuggets.com/2016/10/mldb-machine-learning-database.html](https://www.kdnuggets.com/2016/10/mldb-machine-learning-database.html)

**由François Maillet，MLDB.ai**。

在这篇文章中，我们将展示使用MLDB构建自己的实时图像分类服务有多么简单。我们将在这个示例中使用不同品牌的汽车，但你可以根据我们展示的内容来训练任何图像数据集上的模型。我们将使用一个TensorFlow深度卷积神经网络、迁移学习，所有这些都将由MLDB运行。

### 使用Inception模型的迁移学习

从高层次来看，迁移学习允许我们使用在一个任务上训练的模型，将其学到的知识应用到另一个任务上。我们使用了[Inception-v3模型](http://arxiv.org/abs/1512.00567)，这是一个深度卷积神经网络，已经在[ImageNet](http://image-net.org/)大规模视觉识别挑战数据集上进行了训练。该挑战的任务是将图像分类为多达1000个类别，如*獾、货车*或*汉堡包*。

Inception模型作为训练好的TensorFlow图形公开发布。[TensorFlow](https://www.tensorflow.org/)是Google去年开源的深度学习库，MLDB有一个内置的集成。正如你将看到的，MLDB使得直接在SQL中运行TensorFlow模型变得极其简单。

在解决任何机器学习问题时，一个关键步骤是选择和设计特征提取器。它们用于将我们想要分类的对象——无论是图像、歌曲还是新闻文章——转换为可以提供给分类器的数值表示，称为特征向量。传统上，特征提取器的选择是手动完成的。深度神经网络一个真正令人兴奋的方面是它们能够自我学习特征提取器。

以下是Inception模型的架构，图像从左侧输入，预测从右侧输出。最后一层的大小将是1000，并为每个类别提供一个概率。然而，之前的层是网络对原始图像的变换，因为这些层在解决图像分类任务时最有用。例如，一些层是边缘检测器。

[![MLDB CNN](../Images/a4a00cd0af2e77a5b216fcbc7af524b3.png)](https://4.bp.blogspot.com/-TMOLlkJBxms/Vt3HQXpE2cI/AAAAAAAAA8E/7X7XRFOY6Xo/s1600/image03.png)

所以我们的想法是将图像传递到网络中，但不是获取最后一层的输出，而是获取倒数第二层，这将给我们一个图像的概念性数值表示。我们可以将该表示作为特征，提供给一个新的分类器，并在我们自己的任务上进行训练。因此，你可以将Inception模型视为从图像到特征向量的方式，新分类器可以高效地对其进行操作。我们利用了数百小时的GPU计算时间来训练Inception模型，但将其应用于完全新的任务。

### MLDB上的Inception

让我们开始吧！下面的代码使用了我们的pymldb库。你可以在[MLDB文档](https://docs.mldb.ai/ipy/notebooks/_tutorials/_latest/Using%20pymldb%20Tutorial.html)中了解更多信息。

我们在这里做了什么？我们使用pymldb进行了一个简单的PUT调用，创建了类型为*[tensorflow.graph](https://docs.mldb.ai/doc/#/v1/plugins/tensorflow/doc/TensorflowGraph.md.html)*的​`inception`函数。它使用JSON blob进行参数化。该函数加载了一个训练好的Inception模型实例（请注意，MLDB可以透明地加载远程资源，以及压缩档案中的文件；[更多内容在这里](https://docs.mldb.ai/ipy/notebooks/_tutorials/_latest/Loading%20Data%20From%20An%20HTTP%20Server%20Tutorial.html)）。我们指定模型的输入将是位于*url*的远程资源，输出将是模型的​*pool_3* 层，即倒数第二层。使用*pool_3* 层将提供高级特征，而最后一层*softmax*是专门用于ImageNet任务的。

现在创建了​ inception 函数后，它可以在SQL中使用，也可以作为REST端点。然后，我们可以通过简单的SQL查询将图像传递到网络中。这里我们将Inception应用于KDNuggets徽标，得到的是该图像的数值表示。这2048个数字是我们可以用作特征向量的内容。

### 使用SQL准备训练数据集

现在我们可以导入训练数据。我们有一个包含大约200个汽车图像链接的CSV文件，这些图像来自3个流行品牌：奥迪、宝马和特斯拉。重要的是要记住，尽管我们使用的是汽车数据集，但你可以用任何你想要的图像替代它。

我们可以通过运行一个​[*import.text* 程序](https://docs.mldb.ai/doc/#builtin/procedures/importtextprocedure.md.html)来将CSV文件导入数据集。

我们可以使用SQL生成一些快速统计数据：

![SQL结果](../Images/40d084d116b38b185d2f5807714f7388.png)

我们现在可以使用一种类型为[transform](https://docs.mldb.ai/doc/#builtin/procedures/TransformDataset.md.html)的过程来对所有图像应用*Inception*模型，并将结果存储在另一个数据集中。转换过程只是执行一个SQL查询，并将结果保存到一个新数据集中。运行下面的代码本质上就是进行特征提取。

对我们的图像数据集进行处理。

### 训练一个专用模型

既然我们已经为所有图像准备了特征，我们就使用一种类型为*[classifier.experiment](https://docs.mldb.ai/doc/#builtin/procedures/ExperimentProcedure.md.html)*的过程来训练和测试随机森林分类器。默认情况下，数据集会在训练和测试之间50/50地拆分。

注意`inputData`键的内容，它指定了用于训练和测试的数据是SQL格式。`{* EXCLUDING(label)}`是MLDB行表达式语法的一个好例子，旨在处理具有数百万列的稀疏数据集。

从测试集的表现来看，这个模型的表现相当不错：

![MLDB Out[10]](../Images/abd4814eadc1bdcae9850bd76368b649.png)

![MLDB Out[11]](../Images/859c33d6a895df6860ea564c1e4483e5.png)

### 实时预测

现在我们有了一个训练好的模型，我们如何用它来对新图像进行评分呢？我们需要做两件事：从图像中提取特征，然后将这些特征输入到我们新训练的分类器中。这本质上就是我们的评分管道。

我们所做的是创建一个名为*brand_predictor*的函数，类型为[sql.expression](https://docs.mldb.ai/doc/#builtin/functions/SqlQueryFunction.md.html)。这允许我们将SQL表达式持久化为一个可以多次调用的函数。当我们上面训练分类器时，训练过程会自动创建一个*car_brand_cls_scorer_0*，可以在常规SQL/Rest中使用，该函数会运行模型。它会期望一个名为*features*的输入列。

就这样，我们现在已经准备好从互联网中对新图像进行评分了：

```py` ``` {    "output": {      "scores": [        [          "\"audi\"",           [            -8,             "2016-05-05T04:18:03Z"          ]        ],         [          "\"bmw\"",           [            -7.333333492279053,             "2016-05-05T04:18:03Z"          ]        ],         [          "\"tesla\"",           [            0.2666666805744171,             "2016-05-05T04:18:03Z"          ]        ]      ]    }  } ```py    The image we gave it represented a Tesla, and that is the label that got the highest score.    ### Conclusion      The Machine Learning Database solves machine learning problems end­-to-­end, from data collection to production deployment, and offers world­-class performance yielding potentially dramatic increases in ROI when compared to other machine learning platforms.    In this post, we only scratched the surface of what you can do with MLDB. We have a [white-­paper](http://blog.mldb.ai/files/whitepaper_big2016.pdf) that goes over all of our design decisions in details.    If we’ve peaked your interest, here are a few links that may interest you:    *   try MLDB for free in 5 minutes by [launching a hosted instance](https://mldb.ai/#signup) *   run a trial version of MLDB on your own hardware using [Docker or Virtualbox](https://docs.mldb.ai/doc/#builtin/Running.md.html) *   Check out our [demos and tutorials](https://docs.mldb.ai/doc/#builtin/Demos.md.html), especially [DeepTeach](https://github.com/mldbai/deepteach) which uses the same techniques as shown in this post, and [MLPaint](http://blog.mldb.ai/blog/posts/2016/09/mlpaint/), a white­box real­time handwritten digit recogniser *   [MLDB Github repository](https://github.com/mldbai/mldb)    Don’t hesitate to get in touch! You can find us on [Gitter](https://gitter.im/mldbai/mldb), or follow us on [Twitter](https://twitter.com/mldbai).    All the code from this article is available in the MLDB repository as a [Jupyter notebook](https://docs.mldb.ai/ipy/notebooks/_other/_latest/KDNuggets%20Transfer%20Learning%20Blog%20Post.html), and is also shipped with MLDB. Boot up an instance, go the the demos folder and you can run a live version.    Happy MLDBing!    **Bio: [François Maillet](http://blog.francoismaillet.com/)** is a computer scientist specialising in machine learning and data science. He leads the machine learning team at MLDB.ai, a Montréal startup building the [Machine Learning Database](http://mldb.ai/)​ (MLDB). François has been applying machine learning for almost 10 years to solve varied problems, like real­-time bidding algorithms and behavioral modelling for the adtech industry, automatic bully detection on web forums, audio similarity and fingerprinting, steerable music recommendation and playlist generation.    **Related:**    *   [Recycling Deep Learning Models with Transfer Learning](/2015/08/recycling-deep-learning-representations-transfer-ml.html) *   [Spark for Scale: Machine Learning for Big Data](/2016/09/spark-scale-machine-learning-big-data.html) *   [The Deception of Supervised Learning](/2016/09/deception-of-supervised-learning.html)     * * *      ## Our Top 3 Course Recommendations      ![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - Get on the fast track to a career in cybersecurity.    ![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - Up your data analytics game    ![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - Support your organization in IT    * * *      ### More On This Topic    *   [KDnuggets News, September 21: 7 Machine Learning Portfolio Projects…](https://www.kdnuggets.com/2022/n37.html) *   [Vector Database for LLMs, Generative AI, and Deep Learning](https://www.kdnuggets.com/vector-database-for-llms-generative-ai-and-deep-learning) *   [Database Key Terms, Explained](https://www.kdnuggets.com/2016/07/database-key-terms-explained.html) *   [Free SQL and Database Course](https://www.kdnuggets.com/2022/09/free-sql-database-course.html) *   [In-Database Analytics: Leveraging SQL's Analytic Functions](https://www.kdnuggets.com/2023/07/indatabase-analytics-leveraging-sql-analytic-functions.html) *   [Database Optimization: Exploring Indexes in SQL](https://www.kdnuggets.com/2023/07/database-optimization-exploring-indexes-sql.html) ````
