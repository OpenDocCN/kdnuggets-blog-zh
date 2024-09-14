# 使用 Apache Spark 与 PySpark 的好处与示例

> 原文：[https://www.kdnuggets.com/2020/04/benefits-apache-spark-pyspark.html](https://www.kdnuggets.com/2020/04/benefits-apache-spark-pyspark.html)

[评论](#comments)

### **什么是 Apache Spark？**

[Apache Spark](https://spark.apache.org/)是技术领域中最热门的新趋势之一。它可能是实现大数据与机器学习结合成果的最有潜力的框架。

它运行速度很快（比传统的[Hadoop MapReduce](https://www.tutorialspoint.com/hadoop/hadoop_mapreduce.htm)快多达100倍），由于内存操作，提供强大、分布式、容错的数据对象（称为[RDD](https://www.tutorialspoint.com/apache_spark/apache_spark_rdd.htm)），并通过诸如[Mlib](https://spark.apache.org/mllib/)和[GraphX](https://spark.apache.org/graphx/)等补充包与机器学习和图形分析领域完美集成。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT 工作

* * *

![使用 Apache Spark 与 PySpark 的好处与示例 | Apache Spark](../Images/30cb3911f9df0322fd922f31ff23852a.png)

Spark 是基于[Hadoop/HDFS](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html)实现的，主要用[Scala](https://www.scala-lang.org/)编写，这是一种类似于 Java 的函数式编程语言。实际上，Scala 需要系统上最新的 Java 安装并在 JVM 上运行。然而，对于大多数初学者来说，Scala 并不是他们在进入数据科学领域时首先学习的语言。幸运的是，Spark 提供了一个出色的 Python 集成，称为**PySpark**，它允许 Python 程序员与 Spark 框架进行接口，学习如何大规模操作数据，并在分布式文件系统上处理对象和算法。

在本文中，我们将学习 PySpark 的基础知识。由于有许多不断发展的概念，因此我们只关注基础知识，并通过一些简单的示例进行介绍。鼓励读者在此基础上进行拓展并自主探索。

### **Apache Spark 的简短历史**

Apache Spark 起初是 2009 年在 UC Berkeley AMPLab 的一个研究项目，并于 2010 年初开源。它最初是 UC Berkeley 的一个课程项目，目的是建立一个集群管理框架，支持不同类型的集群计算系统。多年来，系统背后的许多理念在各种研究论文中提出。发布后，Spark 成长为一个广泛的开发者社区，并于 2013 年迁移到 Apache 软件基金会。如今，该项目由来自数百个组织的数百名开发者共同开发。

### **Spark 不是一种编程语言**

需要记住的一点是，Spark 不是像 Python 或 Java 那样的编程语言。它是一个通用的分布式数据处理引擎，适用于广泛的场景。它特别适合大规模和高速度的大数据处理。

应用开发者和数据科学家通常将 Spark 集成到他们的应用程序中，以快速查询、分析和转换大规模数据。一些最常与 Spark 相关联的任务包括：– 跨大型数据集（通常为 TB 级别）的 ETL 和 SQL 批处理作业，– 处理来自 IoT 设备和节点的流数据、各种传感器的数据、各种金融和事务系统的数据，以及 – 电子商务或 IT 应用的机器学习任务。

从本质上讲，Spark 建立在处理分布式文件的 Hadoop/HDFS 框架之上。它主要用 Scala 实现，Scala 是 Java 的一种函数式语言变体。虽然有一个核心的 Spark 数据处理引擎，但在其之上，还有许多用于 SQL 类型查询分析、分布式机器学习、大规模图计算和流数据处理的库。Spark 支持多种编程语言，以易于使用的接口库形式提供：Java、Python、Scala 和 R。

### **Spark 使用 MapReduce 范式进行分布式处理**

分布式处理的基本思想是将数据块分割成小的可管理的部分（包括一些过滤和排序），将计算任务接近数据，即在大型集群的各个小节点上执行特定任务，然后再将它们重新组合。分割部分称为“Map”操作，重新组合部分称为“Reduce”操作。它们共同构成了著名的“MapReduce”范式，该范式由 Google 于 2004 年左右引入（见[原始论文](https://static.googleusercontent.com/media/research.google.com/en//archive/mapreduce-osdi04.pdf)）。

例如，如果一个文件有 100 条记录需要处理，100 个映射器可以一起运行，每个处理一条记录。或者，也许 50 个映射器可以一起运行，每个处理两条记录。所有映射器完成处理后，框架会在将结果传递给减少器之前进行洗牌和排序。一个减少器在映射器仍在进行时不能开始。所有具有相同键的映射输出值会被分配给一个减少器，减少器然后对该键的值进行聚合。

### **如何设置 PySpark**

如果你已经熟悉 Python 及 Pandas 和 Numpy 等库，那么 PySpark 是一个很好的扩展/框架，利用 Spark 的强大功能，你可以创建更具可扩展性、数据密集型的分析和管道。

安装和设置 PySpark 环境（在独立机器上）的确切过程有些复杂，并且可能会根据你的系统和环境略有不同。目标是使你的常规 Jupyter 数据科学环境在后台使用 PySpark 包与 Spark 一起工作。

[**这篇文章**](https://medium.com/free-code-camp/how-to-set-up-pyspark-for-your-jupyter-notebook-7399dd3cb389) 在 Medium 上提供了关于逐步设置过程的更多细节。

![](../Images/881536150793aa5d361af9092960c036.png)

另外，你可以使用 Databricks 设置来练习 Spark。这家公司由 Spark 的原始创建者创建，并提供了一个出色的即用环境来进行分布式分析。

但想法总是一样的。你将大型数据集分布（并复制）到许多节点上的小固定块中。然后你将计算引擎靠近它们，使整个操作实现并行化、容错和可扩展。

通过使用 PySpark 和 Jupyter notebook，你可以在不花费 AWS 或 Databricks 平台费用的情况下学习所有这些概念。你还可以轻松与 SparkSQL 和 MLlib 接口，用于数据库操作和机器学习。如果你事先掌握了这些概念，那么开始使用真实的大型集群将会容易得多！

### **弹性分布式数据集（RDD）和 SparkContext**

许多 Spark 程序围绕弹性分布式数据集（RDD）的概念展开，RDD 是一个可以并行操作的容错元素集合。SparkContext 存在于 Driver 程序中，通过集群管理器在工作节点上管理分布式数据。使用 PySpark 的好处是所有这些数据分区和任务管理的复杂性在后台自动处理，程序员可以专注于具体的分析或机器学习任务。

![使用 Apache Spark 和 PySpark 在 Python 中的好处与示例 | Rdd-1](../Images/5bb0f6532941bb2fa5a40915e804a5df.png)

*rdd-1*

创建 RDD 有两种方式——在你的驱动程序中并行化现有集合，或者引用外部存储系统中的数据集，如共享文件系统、HDFS、HBase，或任何提供 Hadoop InputFormat 的数据源。

为了使用基于 Python 的方法进行说明，我们将在这里提供第一种类型的示例。我们可以使用 Numpy 的 random.randint() 创建一个包含 20 个随机整数（范围在 0 到 10 之间）的简单 Python 数组，然后按以下方式创建 RDD 对象，

```py
from pyspark import SparkContext
import numpy as np
sc=SparkContext(master="local[4]")
lst=np.random.randint(0,10,20)
A=sc.parallelize(lst)

```

***注意参数中的 ‘4’***。它表示要为这个 ****SparkContext**** 对象使用的 4 个计算核心（在你的本地机器上）。如果我们检查 RDD 对象的类型，我们会得到以下结果，

```py
type(A)
>> pyspark.rdd.RDD
```

与并行化相对的是收集（使用 collect()），它将所有分布的元素带回头节点。

```py
A.collect()
>> [4, 8, 2, 2, 4, 7, 0, 3, 3, 9, 2, 6, 0, 0, 1, 7, 5, 1, 9, 7]
```

但是 A 不再是一个简单的 Numpy 数组。我们可以使用 glom() 方法来检查如何创建分区。

```py
A.glom().collect()
>> [[4, 8, 2, 2, 4], [7, 0, 3, 3, 9], [2, 6, 0, 0, 1], [7, 5, 1, 9, 7]]
```

现在停止 SC，并用 2 个核心重新初始化它，看看当你重复这个过程时会发生什么。

```py
sc.stop()
sc=SparkContext(master="local[2]")
A = sc.parallelize(lst)
A.glom().collect()
>> [[4, 8, 2, 2, 4, 7, 0, 3, 3, 9], [2, 6, 0, 0, 1, 7, 5, 1, 9, 7]]
```

***RDD 现在分布在两个块中，而不是四个！***

**你已经了解了分布式数据分析的第一步，即控制数据如何分配到较小的块中以便进一步处理**

### **一些 RDD 和 PySpark 的基本操作示例**

**计算元素数量**

```py
>> 20
```

**第一个元素（****first****）和前几个元素（****take****）**

```py
A.first()
>> 4
A.take(3)
>> [4, 8, 2]
```

**使用 ****distinct**** 移除重复项**

*注意*：这个操作需要进行 **shuffle**，以便检测跨分区的重复。因此，这个操作较慢。不要过度使用。

```py
A_distinct=A.distinct()
A_distinct.collect()
>> [4, 8, 0, 9, 1, 5, 2, 6, 7, 3]
```

**要对所有元素求和，请使用 ****reduce**** 方法**

注意这其中使用了 lambda 函数，

```py
A.reduce(lambda x,y:x+y)
>> 80
```

**或者直接使用 ****sum()**** 方法**

```py
A.sum()
>> 80
```

**通过 ****reduce**** 查找最大元素**

```py
A.reduce(lambda x,y: x if x > y else y)
>> 9
```

**在一段文本中查找最长的单词**

```py
words = 'These are some of the best Macintosh computers ever'.split(' ')
wordRDD = sc.parallelize(words)
wordRDD.reduce(lambda w,v: w if len(w)>len(v) else v)
>> 'computers'
```

**使用 ****filter**** 进行基于逻辑的过滤**

```py
# Return RDD with elements (greater than zero) divisible by 3
A.filter(lambda x:x%3==0 and x!=0).collect()
>> [3, 3, 9, 6, 9]
```

**编写常规的 Python 函数以配合 `reduce()` 使用**

```py
def largerThan(x,y):
  """
  Returns the last word among the longest words in a list
  """
  if len(x)> len(y):
    return x
  elif len(y) > len(x):
    return y
  else:
    if x < y: return x
    else: return y

wordRDD.reduce(largerThan)
>> 'Macintosh'
```

注意这里的 x < y 进行的是字典序比较，确定 Macintosh 比 computers 大！

**在 PySpark 中使用 lambda 函数进行映射操作**

```py
B=A.map(lambda x:x*x)
B.collect()
>> [16, 64, 4, 4, 16, 49, 0, 9, 9, 81, 4, 36, 0, 0, 1, 49, 25, 1, 81, 49]
```

**在 PySpark 中使用常规 Python 函数进行映射**

```py
def square_if_odd(x):
  """
  Squares if odd, otherwise keeps the argument unchanged
  """
  if x%2==1:
    return x*x
  else:
    return x

A.map(square_if_odd).collect()
>> [4, 8, 2, 2, 4, 49, 0, 9, 9, 81, 2, 6, 0, 0, 1, 49, 25, 1, 81, 49]
```

**groupby**** 返回一个根据给定分组操作的分组元素（可迭代）的 RDD**

在以下示例中，我们使用列表推导式与 groupby 一起创建一个包含两个元素的列表，每个元素都有一个标题（这里是 lambda 函数的结果，简单的模 2 运算），以及一个排序的元素列表，这些元素产生了该结果。你可以很容易地想象，这种分离特别适合于处理需要根据对其执行的特定操作进行分箱/整理的数据。

```py
result=A.groupBy(lambda x:x%2).collect()
sorted([(x, sorted(y)) for (x, y) in result])
>> [(0, [0, 0, 0, 2, 2, 2, 4, 4, 6, 8]), (1, [1, 1, 3, 3, 5, 7, 7, 7, 9, 9])]
```

**使用 ****histogram****

`histogram()` 方法接受一个桶/区间的列表，并返回一个包含直方图结果（分桶）的元组，

```py
B.histogram([x for x in range(0,100,10)])
>> ([0, 10, 20, 30, 40, 50, 60, 70, 80, 90], [10, 2, 1, 1, 3, 0, 1, 0, 2])
```

**集合操作**

你还可以对 RDD 进行常规的集合操作，比如 union()、 intersection()、 subtract()，或 cartesian()。

查看[**这个Jupyter笔记本**](https://github.com/tirthajyoti/Spark-with-Python/blob/master/SparkContext%20and%20RDD%20Basics.ipynb)获取更多示例。

**使用PySpark的惰性计算（和**缓存**）**

惰性计算是一种评估/计算策略，它为计算任务准备了详细的逐步内部执行管道图，但将最终执行延迟到绝对需要的时候。这种策略是Spark加速许多并行化大数据操作的核心。

让我们使用两个CPU核心作为这个例子的说明，

```py
sc = SparkContext(master="local[2]")
```

**创建一个包含100万个元素的RDD**

```py
%%time
rdd1 = sc.parallelize(range(1000000))
>> CPU times: user 316 µs, sys: 5.13 ms, total: 5.45 ms, Wall time: 24.6 ms
```

**某些计算函数 – **taketime**

```py
from math import cos
def taketime(x):
[cos(j) for j in range(100)]
return cos(x)
```

**检查taketime函数所花费的时间**

```py
%%time
taketime(2)
>> CPU times: user 21 µs, sys: 7 µs, total: 28 µs, Wall time: 31.5 µs
>> -0.4161468365471424
```

记住这个结果，`taketime()`函数的实际时间为31.5微秒。当然，确切的数字将取决于你使用的机器。

**现在对函数进行map操作**

```py
%%time
interim = rdd1.map(lambda x: taketime(x))
>> CPU times: user 23 µs, sys: 8 µs, total: 31 µs, Wall time: 34.8 µs
```

*为什么每个**taketime**函数花费45.8微秒，但处理100万个元素的RDD的map操作也花费了类似的时间？*

**由于惰性计算，即在之前的步骤中没有计算任何内容，只是制定了执行计划**。变量`interim`并不指向数据结构，而是指向一个执行计划，表示为依赖关系图。依赖关系图定义了RDD如何从彼此之间计算。

**通过**reduce**方法的实际执行**

```py
%%time
print('output =',interim.reduce(lambda x,y:x+y))
>> output = -0.28870546796843666
>> CPU times: user 11.6 ms, sys: 5.56 ms, total: 17.2 ms, Wall time: 15.6 s
```

所以，这里的实际时间是15.6秒。记住，`taketime()`函数的时间是31.5微秒吗？因此，我们期望对于一个100万的数组，总时间约为31秒。由于在两个核心上并行操作，它花费了大约15秒。

现在，我们没有在`interim`中保存（具体化）任何中间结果，所以另一个简单操作（例如计数元素 > 0）将花费几乎相同的时间。

```py
%%time
print(interim.filter(lambda x:x>0).count())
>> 500000
>> CPU times: user 10.6 ms, sys: 8.55 ms, total: 19.2 ms, Wall time: 12.1 s
```

### **缓存以减少类似操作的计算时间（消耗内存）**

记住我们在前一步中构建的依赖关系图吗？我们可以使用缓存方法运行相同的计算，告诉依赖关系图规划缓存。

```py
%%time
interim = rdd1.map(lambda x: taketime(x)).cache()
```

第一次计算不会改善，但它缓存了中间结果，

```py
%%time
print('output =',interim.reduce(lambda x,y:x+y))
>> output = -0.28870546796843666
>> CPU times: user 16.4 ms, sys: 2.24 ms, total: 18.7 ms, Wall time: 15.3 s
```

现在使用缓存结果运行相同的过滤方法，

```py
%%time
print(interim.filter(lambda x:x>0).count())
>> 500000
>> CPU times: user 14.2 ms, sys: 3.27 ms, total: 17.4 ms, Wall time: 811 ms
```

哇！计算时间从之前的12秒降到了不到1秒！这就是使用Spark编程的核心特性：缓存和并行化以及惰性执行。

### **DataFrame和SparkSQL**

除了RDD，Spark框架中的第二个关键数据结构是*DataFrame*。如果你做过Python Pandas或R DataFrame的工作，这个概念可能会很熟悉。

DataFrame 是在命名列下的分布式行集合。它在概念上等同于关系数据库中的表、带列标题的 Excel 表格或 R/Python 中的数据框，但具有更丰富的底层优化。DataFrames 可以从各种来源构建，如结构化数据文件、Hive 中的表、外部数据库或现有的 RDD。它还与 RDD 共享一些共同特征：

+   本质上是不可变的：我们可以创建 DataFrame / RDD，但不能更改它。我们可以在应用转换后对 DataFrame / RDD 进行变换。

+   惰性计算：意味着任务不会执行，直到进行某个操作。分布式：RDD 和 DataFrame 本质上都是分布式的。

![使用 Apache Spark 和 PySpark 在 Python 中的好处与示例 | PySpark 中的 DataFrame](../Images/197e1ce09b894ea1628c7da1c8d3c87d.png)

### **DataFrame 的优点**

+   DataFrames 设计用于处理大量结构化或半结构化数据。

+   Spark DataFrame 中的观察数据在命名列下组织，这帮助 Apache Spark 理解 DataFrame 的模式。这有助于 Spark 优化这些查询的执行计划。

+   Apache Spark 中的 DataFrame 具有处理 PB 级数据的能力。

+   DataFrame 支持多种数据格式和来源。

+   它支持 Python、R、Scala、Java 等多种语言的 API。

### **DataFrame 基础示例**

有关 DataFrames 的基本原理和典型使用示例，请参阅以下 Jupyter Notebooks，

[**Spark DataFrame 基础**](https://github.com/tirthajyoti/Spark-with-Python/blob/master/Dataframe_basics.ipynb)

[**Spark DataFrame 操作**](https://github.com/tirthajyoti/Spark-with-Python/blob/masterDataFrame_operations_basics.ipynb)

### **SparkSQL 帮助弥合 PySpark 的差距**

关系型数据存储容易构建和查询。用户和开发者通常更喜欢使用易于理解的声明性查询语言，如 SQL。然而，随着数据量和种类的增加，关系型方法在构建大数据应用程序和分析系统时扩展性不够好。

我们在大数据分析领域通过 Hadoop 和 MapReduce 范式取得了成功。这虽然强大，但往往速度较慢，并且为用户提供了一个低级的**过程编程接口**，要求人们即使对非常简单的数据转换也需要编写大量代码。然而，一旦 Spark 发布，它真正革新了大数据分析的方式，专注于内存计算、容错、高级抽象以及易用性。

Spark SQL 本质上试图弥合我们之前提到的两种模型——关系模型和过程模型之间的差距。Spark SQL 通过 DataFrame API 执行关系操作，可以在外部数据源和 Spark 内置的分布式集合上进行大规模操作！

![使用 PySpark 和 Python 的 Apache Spark 的好处与示例 | Spark SQL](../Images/fbbca3e99c3728cb51c0284181460269.png)

为什么 Spark SQL 这么快且经过优化？原因在于一种新的可扩展优化器**Catalyst**，基于 Scala 中的函数式编程构造。Catalyst 支持基于规则和基于成本的优化。虽然过去也提出过可扩展的优化器，但通常需要复杂的领域特定语言来指定规则。这通常会导致显著的学习曲线和维护负担。相比之下，Catalyst 使用 Scala 编程语言的标准特性，如模式匹配，使开发人员可以使用完整的编程语言，同时仍然使规则易于指定。

你可以参考以下 Jupyter 笔记本来了解 SparkSQL 的数据库操作：

[**SparkSQL 数据库操作基础**](https://github.com/tirthajyoti/Spark-with-Python/blob/master/Dataframe_SQL_query.ipynb)

### **你打算在你的项目中如何使用 PySpark？**

我们介绍了 Apache Spark 生态系统的基础知识及其工作原理，并提供了一些关于核心数据结构 RDD 使用的基本示例，使用的是 Python 接口 PySpark。此外，还讨论了 DataFrame 和 SparkSQL，并提供了示例代码笔记本的参考链接。

使用 Python 的 Apache Spark 还有很多内容可以学习和实验。[PySpark 网站](https://spark.apache.org/docs/latest/api/python/index.html)是一个很好的参考，它们会定期更新和改进——所以请保持关注。

如果你对使用 Apache Spark 进行大规模分布式机器学习感兴趣，可以查看 [PySpark 生态系统的 MLLib 部分](https://spark.apache.org/mllib/)。

[原文](https://blog.exxactcorp.com/the-benefits-examples-of-using-apache-spark-with-pyspark-using-python/)。经许可转载。

**相关内容：**

+   [学习如何在 5 分钟内使用 PySpark（安装 + 教程）](/2019/08/learn-pyspark-installation-tutorial.html)

+   [时间序列分析：使用 KNIME 和 Spark 的简单示例](/2019/10/time-series-analysis-simple-example-knime-spark.html)

+   [Spark NLP 101：LightPipeline](/2019/11/spark-nlp-101-lightpipeline.html)

### 更多相关主题

+   [使用 Pandera 对 PySpark 应用程序进行数据验证](https://www.kdnuggets.com/2023/08/data-validation-pyspark-applications-pandera.html)

+   [成为数据优先企业的好处](https://www.kdnuggets.com/2022/07/benefits-becoming-datafirst-enterprise.html)

+   [A/B 测试的 3 个好处（+ 从哪里开始）](https://www.kdnuggets.com/2022/08/sphere-3-benefits-ab-testing-get-started.html)

+   [自然语言 AI 对内容创作者的好处](https://www.kdnuggets.com/2022/08/benefits-natural-language-ai-content-creators.html)

+   [用于数据科学的 PySpark](https://www.kdnuggets.com/2023/02/pyspark-data-science.html)

+   [挑选实例以理解机器学习模型](https://www.kdnuggets.com/2022/11/picking-examples-understand-machine-learning-model.html)
