# 2018 年数据科学的 20 大 Python 库

> 原文：[`www.kdnuggets.com/2018/06/top-20-python-libraries-data-science-2018.html/2`](https://www.kdnuggets.com/2018/06/top-20-python-libraries-data-science-2018.html/2)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

### 机器学习

**10. [Scikit-learn](http://scikit-learn.org/stable/) （提交次数：22753，贡献者：1084）**

这个基于 NumPy 和 SciPy 的 Python 模块是处理数据的最佳库之一。它提供了许多标准机器学习和数据挖掘任务的算法，如聚类、回归、分类、降维和模型选择。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

对库进行了多项增强。交叉验证已被修改，提供了使用多个指标的能力。几种训练方法如最近邻和逻辑回归也进行了小幅改进。最后，一个主要更新是完成了 [常见术语和 API 元素词汇表](http://scikit-learn.org/dev/glossary.html#glossary)，它介绍了 Scikit-learn 中使用的术语和惯例。

**11. [XGBoost](http://xgboost.readthedocs.io/en/latest/) / [LightGBM](http://lightgbm.readthedocs.io/en/latest/Python-Intro.html) / [CatBoost](https://github.com/catboost/catboost) （提交次数：3277 / 1083 / 1509，贡献者：280 / 79 / 61）**

梯度提升是最受欢迎的机器学习算法之一，它通过构建一组逐步改进的基本模型，即决策树。因此，有专门的库设计用于快速便捷地实现这种方法。我们认为 XGBoost、LightGBM 和 CatBoost 值得特别关注。它们都是解决相同问题的竞争者，并且几乎以相同的方式使用。这些库提供了高度优化、可扩展和快速的梯度提升实现，这使得它们在数据科学家和 Kaggle 竞赛者中极为受欢迎，因为许多比赛都借助这些算法赢得了胜利。

**12. [Eli5](https://eli5.readthedocs.io/en/latest/) （提交次数：922，贡献者：6）**

机器学习模型预测的结果通常不是完全清晰的，这正是 eli5 库帮助解决的挑战。它是一个用于可视化和调试机器学习模型的包，并逐步跟踪算法的工作。它支持 scikit-learn、XGBoost、LightGBM、lightning 和 sklearn-crfsuite 库，并为每个库执行不同的任务。

### 深度学习

**13. [TensorFlow](https://www.tensorflow.org/) (提交: 33339, 贡献者: 1469)**

TensorFlow 是一个流行的深度学习和机器学习框架，由 Google Brain 开发。它提供了处理多数据集的人工神经网络的能力。最受欢迎的 TensorFlow 应用包括目标识别、语音识别等。在常规 TensorFlow 之上，还有不同的层辅助工具，如 tflearn、tf-slim、skflow 等。

这个库在新版本发布方面非常迅速，引入了越来越多的新功能。最新的更新包括修复潜在的安全漏洞和改进 TensorFlow 与 GPU 的集成，例如，你可以在一台机器上的多个 GPU 上运行 Estimator 模型。

**14. [PyTorch](https://pytorch.org/)(提交: 11306, 贡献者: 635)**

PyTorch 是一个大型框架，允许你执行带有 GPU 加速的张量计算，创建动态计算图并自动计算梯度。除此之外，PyTorch 提供了丰富的 API 来解决与神经网络相关的应用。

该库基于 Torch，这是一个用 C 实现并在 Lua 中封装的开源深度学习库。Python API 于 2017 年引入，从那时起，框架逐渐流行，吸引了越来越多的数据科学家。

**15. [Keras](https://keras.io/) (提交: 4539, 贡献者: 671)**

Keras 是一个高级库，用于处理神经网络，运行在 TensorFlow、Theano 之上，现在由于新版本的发布，也可以使用 CNTK 和 MxNet 作为后端。它简化了许多特定任务，并大大减少了单调的代码。然而，它可能不适合一些复杂的任务。

该库经历了性能、可用性、文档和 API 的改进。一些新特性包括 Conv3DTranspose 层、新的 MobileNet 应用和自我归一化网络。

### 分布式深度学习

**16. [Dist-keras](http://joerihermans.com/work/distributed-keras/) / [elephas](https://pypi.org/project/elephas/) / [spark-deep-learning](https://databricks.github.io/spark-deep-learning/site/index.html) (提交: 1125 / 170 / 67, 贡献者: 5 / 13 / 11)**

深度学习问题变得至关重要，因为越来越多的应用场景需要相当大的努力和时间。然而，使用像 Apache Spark 这样的分布式计算系统来处理如此大量的数据要容易得多，这也再次拓展了深度学习的可能性。因此，dist-keras、elephas 和 spark-deep-learning 正逐渐受到关注并迅速发展，很难单独挑出其中一个库，因为它们都是为了解决一个共同的任务而设计的。这些包允许你通过 Apache Spark 直接基于 Keras 库训练神经网络。Spark-deep-learning 还提供了创建与 Python 神经网络的管道的工具。

### 自然语言处理

**17. [NLTK](https://www.nltk.org/) (提交次数: 13041, 贡献者: 236)**

NLTK 是一套库，一个用于自然语言处理的完整平台。借助 NLTK，你可以以多种方式处理和分析文本，对其进行标记和标签，提取信息等。NLTK 也用于原型设计和研究系统的构建。

对该库的改进包括 API 和兼容性的细微变化以及对 CoreNLP 的新接口。

**18. [SpaCy](https://spacy.io/) (提交次数: 8623, 贡献者: 215)**

SpaCy 是一个自然语言处理库，提供了出色的示例、API 文档和演示应用程序。该库使用 Cython 语言编写，Cython 是 Python 的 C 扩展。它支持几乎 30 种语言，提供简单的深度学习集成，并承诺强大的稳定性和高准确性。另一个优秀的特点是 spaCy 具有处理整个文档的架构，而无需将文档拆分成短语。

**19. [Gensim](https://radimrehurek.com/gensim/) (提交次数: 3603, 贡献者: 273)**

Gensim 是一个用于强大语义分析、主题建模和向量空间建模的 Python 库，基于 Numpy 和 Scipy 构建。它提供了流行的 NLP 算法的实现，例如 word2vec。虽然 gensim 有自己的 models.wrappers.fasttext 实现，但 fasttext 库也可以用于高效学习词汇表示。

### 数据抓取

**20. [Scrapy](https://scrapy.org/) (提交次数: 6625, 贡献者: 281)**

Scrapy 是一个用于创建爬虫机器人以扫描网页并收集结构化数据的库。此外，Scrapy 还可以从 API 中提取数据。由于其扩展性和可移植性，该库非常方便。

年内取得的进展包括对代理服务器的若干升级以及改进的错误通知和问题识别系统。还有使用*scrapy parse*的新元数据设置可能性。

### 结论

这是我们在 2018 年为数据科学提供的丰富的 Python 库集合。与前一年相比，一些新的现代库正在获得越来越多的关注，而那些已经成为数据科学任务经典的库也在不断改进。

再次说明，这里有一张表格显示了 GitHub 活动的详细统计数据。

![](https://activewizards.com/content/blog/Top_20_Python_libraries_for_data_science_-_2018/github-table01-by-click.jpg)

尽管我们今年扩展了我们的列表，但它仍可能没有涵盖一些其他值得关注的优秀和有用的库。因此，请在下方评论区分享您的最爱，以及我们提到的包的任何想法。

感谢您的关注！

**[ActiveWizards](https://activewizards.com/)** 是一个专注于数据项目（大数据、数据科学、机器学习、数据可视化）的数据科学家和工程师团队。核心专长领域包括数据科学（研究、机器学习算法、可视化和工程）、数据可视化（d3.js、Tableau 等）、大数据工程（Hadoop、Spark、Kafka、Cassandra、HBase、MongoDB 等）以及数据密集型 Web 应用开发（RESTful APIs、Flask、Django、Meteor）。

[原文](https://activewizards.com/blog/top-20-python-libraries-for-data-science-in-2018/)。经授权转载。

**相关：**

+   2018 年数据科学领域前 20 个 R 库

+   2018 年数据科学领域前 15 个 Scala 库

+   2017 年数据科学领域前 15 个 Python 库

### 更多相关内容

+   [每个数据科学家都应了解的三种 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [数据科学学习统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [停止学习数据科学以寻找目标，并为寻找目标而学习数据科学](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [是什么使得 Python 成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [9 亿美元人工智能失败分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)
