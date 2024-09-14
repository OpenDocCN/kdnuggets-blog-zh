# Apache Spark : Python 与 Scala

> 原文：[https://www.kdnuggets.com/2018/05/apache-spark-python-scala.html](https://www.kdnuggets.com/2018/05/apache-spark-python-scala.html)

[评论](#comments)

**由 [Preet Gandhi](https://www.linkedin.com/in/preetgandhi/), NYU 数据科学中心**

![Apache Spark: Python vs Scala](../Images/49d2f2526df2a55685cc8518299e0706.png)

Apache Spark 是最流行的大数据分析框架之一。Spark 用 Scala 编写，因为它的静态类型和已知的编译方式使其速度较快。虽然 Spark 为 Scala、Python、Java 和 R 提供了 API，但最常用的语言是前两者。Java 不支持 Read-Evaluate-Print-Loop，而 R 不是通用语言。数据科学社区分为两个阵营；一个偏爱 Scala，而另一个则偏爱 Python。每种语言都有其优缺点，最终的选择应取决于应用的结果。

![Apache Spark Python Scala](../Images/91ac3f646835cc64a1a8bae368d775a5.png)

### 性能

Scala 通常比 Python 快 10 倍以上。Scala 在运行时使用 Java 虚拟机（JVM），这使得它在大多数情况下比 Python 更快。Python 是动态类型的，这降低了速度。编译语言通常比解释语言更快。在 Python 的情况下，会调用 Spark 库，这需要大量的代码处理，因此性能较慢。在这种情况下，Scala 在有限的核心上表现良好。此外，Scala 原生支持 Hadoop，因为它基于 JVM。Hadoop 很重要，因为 Spark 是在 Hadoop 的文件系统 HDFS 之上构建的。Python 与 Hadoop 服务的交互非常糟糕，因此开发人员必须使用第三方库（如 hadoopy）。而 Scala 通过 Java 原生 Hadoop 的 API 与 Hadoop 交互。这就是为什么用 Scala 编写原生 Hadoop 应用程序非常简单的原因。

### 学习曲线

两者都是函数式和面向对象的语言，语法相似，并且都有繁荣的支持社区。与 Python 相比，Scala 可能学习起来更复杂，因为它具有高级函数特性。Python 更适合简单直观的逻辑，而 Scala 更适用于复杂的工作流。Python 具有简单的语法和良好的标准库。

### 并发

Scala 具有多个标准库和核心，允许在大数据生态系统中快速集成数据库。Scala 允许使用多个并发原语编写代码，而 Python 不支持并发或多线程。由于其并发特性，Scala 允许更好的内存管理和数据处理。然而，Python 确实支持重量级进程分叉。在这种情况下，只有一个线程处于活动状态。因此，每当新代码部署时，必须重新启动更多进程，这增加了内存开销。

### 可用性

两者都很具表现力，我们可以通过它们实现高功能水平。Python 更加用户友好且简洁。Scala 在框架、库、隐式、宏等方面通常更强大。由于 Scala 的函数式特性，它在 MapReduce 框架中表现良好。许多 Scala 数据框架遵循类似的抽象数据类型，与 Scala 的 API 集合一致。开发者只需学习基本的标准集合，这让他们能够轻松地熟悉其他库。Spark 是用 Scala 编写的，因此了解 Scala 会让你理解并修改 Spark 的内部实现。此外，许多即将推出的功能首先会在 Scala 和 Java 中发布 API，而 Python API 会在后续版本中演进。但是对于 NLP，Python 更受欢迎，因为 Scala 在机器学习或 NLP 工具方面不多。此外，使用 GraphX、GraphFrames 和 MLLib 时，Python 更受青睐。Python 的可视化库补充了 Pyspark，因为 Spark 和 Scala 都没有类似的功能。

### 代码恢复和安全

Scala 是一种静态类型语言，可以帮助我们发现编译时错误，而 Python 是动态类型语言。Python 语言在每次修改现有代码时都容易出现错误。因此，相比于 Python，Scala 的代码重构更容易。

### 结论

Python 运行较慢但非常易于使用，而 Scala 速度最快且使用起来相对简单。Scala 提供了对 Spark 最新特性的访问，因为 Apache Spark 是用 Scala 编写的。编程语言的选择取决于最适合项目需求的特性，因为每种语言都有其优缺点。Python 更倾向于分析，而 Scala 更倾向于工程，但两者都是构建数据科学应用程序的优秀语言。总体而言，为了充分利用 Spark 的全部潜力，Scala 会更有利。如果你真的想在 Spark 上进行创新的机器学习，那么学习那种神秘的语法是值得的。

**简历：[Preet Gandhi](https://www.linkedin.com/in/preetgandhi/)** 是纽约大学数据科学中心的数据科学硕士生。她是一个狂热的大数据和数据科学爱好者。你可以通过 [pg1690@nyu.edu](mailto:pg1690@nyu.edu) 联系她。

**相关：**

+   [使用 Apache Spark 的深度学习：第 1 部分](https://www.kdnuggets.com/2018/04/deep-learning-apache-spark-part-1.html)

+   [实践：Python 数据分析入门](https://www.kdnuggets.com/2018/05/tdwi-intro-python-data-analysis.html)

+   [2018 年数据科学中的前 15 个 Scala 库](https://www.kdnuggets.com/2018/02/top-15-scala-libraries-data-science-2018.html)

### 更多相关内容

+   [每个数据科学家都应该知道的三种 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [是什么让Python成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [停止学习数据科学以寻找目标，并找到目标来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [一个90亿美元的人工智能失败案例分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的5个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)
