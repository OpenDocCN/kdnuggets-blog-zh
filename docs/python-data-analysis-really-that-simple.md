# Python 用于数据分析……真的那么简单吗？！

> 原文：[https://www.kdnuggets.com/2020/04/python-data-analysis-really-that-simple.html](https://www.kdnuggets.com/2020/04/python-data-analysis-really-that-simple.html)

[评论](#comments)

**由 [Ferenc Bodon 博士](https://www.linkedin.com/in/ferencbodon/)，数据工程师，Kx 云解决方案架构师**

![图示](../Images/b8a0052d9e1d377c6bae71923bcd8ad5.png)

图形设计及制作由 CineArt 完成

[Python](https://www.python.org/) 是一种流行的编程语言，易于学习，效率高，并且得到了一个大型活跃社区的支持。它是一个通用语言，具有专门针对各种领域的库，包括网页开发、脚本编写、数据科学和 DevOps。

它的主要数据分析库 [Pandas](https://pandas.pydata.org/) 在数据科学家和数据工程师中获得了很大人气。它遵循 Python 的原则，因此似乎易于学习、阅读并允许快速开发……至少基于教科书的示例。但如果我们离开教科书示例的安全和便利世界会发生什么？Pandas 是否仍然是一个易于使用的**表格数据**分析工具？它与其他专业工具如 [R](https://www.r-project.org/) 和 [kdb+](https://code.kx.com/q/learn/) 相比表现如何？

在这篇文章中，我将通过执行一些**基于多个列的聚合**的示例，探讨超出最简单用例的情况。我的用例复杂度大约在5级中的第2级。任何分析数据表的人都会遇到这个问题，可能在第二周。为了比较，我还将介绍其他旨在数据分析的流行工具。

+   首先，这个问题可以通过 [ANSI SQL](https://en.wikipedia.org/wiki/SQL) 解决，因此所有传统的 RDBM 系统如 [PostegreSQL](https://www.postgresql.org/)、[MySQL](https://www.linkedin.com/redir/general-malware-page?url=https%3A%2F%2Fwww%2emysql%2ecom%2F) 等都可以参与。在实验中，我将使用 [BigQuery](https://cloud.google.com/bigquery/)，这是 Google 提供的一种无服务器、高度可扩展的数据仓库解决方案。

+   [R](https://www.r-project.org/)编程语言专为统计分析设计。它通过其类[data.frame](https://www.google.com/search?q=r+data.frame&oq=r+data.fr&aqs=chrome.0.0j69i57j0l4j69i60j69i61.6225j1j7&sourceid=chrome&ie=UTF-8)本地支持表格。由于核心函数[aggregate](https://www.rdocumentation.org/packages/stats/versions/3.6.2/topics/aggregate)的限制，使用多重聚合非常不方便。R社区开发了库[plyr](https://cran.r-project.org/web/packages/plyr/index.html)以简化data.frame的使用。库[plyr](https://cran.r-project.org/web/packages/plyr/index.html)已经停用，库[dplyr](https://www.rdocumentation.org/packages/dplyr/versions/0.7.8)被引入，承诺提供改进的API和更快的执行速度。库[dplyr](https://www.rdocumentation.org/packages/dplyr/versions/0.7.8)是[tidyverse](https://www.tidyverse.org/)集合的一部分，旨在用于专业数据科学工作。它提供了一个抽象查询层，并将查询与数据存储解耦，无论数据是data.frame还是支持ANSI SQL的外部数据库。库[data.table](https://cran.r-project.org/web/packages/data.table/vignettes/datatable-intro.html)是[dplyr](https://www.rdocumentation.org/packages/dplyr/versions/0.7.8)的一个替代方案，以其速度和简洁的语法而闻名。data.table也可以通过dplyr语法进行查询。

+   在[Q/Kdb+](https://code.kx.com/v2/learn/q-for-all/)编程语言中，表格也是一等公民，并且速度是语言的主要设计概念。Kdb+ 利用多核处理器，并从2004年诞生之初就[使用了map-reduce](https://code.kx.com/q4m3/14_Introduction_to_Kdb+/#1437-map-reduce)（如果数据在磁盘上是[分区的](https://code.kx.com/q4m3/14_Introduction_to_Kdb+/#143-partitioned-tables)）。从4.0版本（2020年3月发布）开始，大多数原语（如sum、avg、dev）使用从属线程并行执行，即使表格在内存中。生产力是另一个设计考量——任何不有助于理解的冗余编程元素（即使是一个括号）都被视为视觉噪音。Kdb+ 是任何数据分析工具的良好候选者。

我将考虑各种解决方案的优雅性、简洁性和速度。同时，我会调查如何**调优性能**，并通过**并行计算**利用多核处理器或计算机集群。

### 问题

![一个示例输入表格](../Images/90809ed59bbfbe9914c3071e0b9e2e8d.png)

我们得到一个简单的表格，包含四列，其中一列是名义列，称为**bucket**，另外三列是数值列，分别是**qty**、**risk**和**weight**。为了简化起见，我们假设数值列包含整数。

我们希望查看每个**bucket**的情况。

+   元素的数量，作为列NR

+   **qty** 和 **risk** 的总和和平均值，作为列 TOTAL_QTY/TOTAL_RISK 和 AVG_QTY/AVG_RISK

+   **qty** 和 **risk** 的加权平均值，作为列 W_AVG_QTY 和 W_AVG_RISK。权重在列 **weight** 中提供。

为了得到解决方案，我不会使用任何已弃用的方法，例如 [通过嵌套字典重命名聚合](https://github.com/pandas-dev/pandas/issues/18366)。让我们分别解决每个任务。

### 每个桶中的元素数量

获取每个桶中的元素数量看起来不太吸引人，需要大量输入

![没有提供图像的替代文本](../Images/09dbc34b1ce0a1f854605837fbbecd46.png)

字面上的 **bucket** 被要求三次，并且你需要使用 5 个括号/圆括号 [????](https://en.wikipedia.org/wiki/%F0%9F%98%90)。

R 中的解决方案看起来更具吸引力。

![没有提供图像的替代文本](../Images/d8dd9c42ea18835850c82d7ec1698196.png)

dplyr 和 data.table 库的开发人员也厌恶字词重复。他们分别引入了特殊的内置变量 **n()** 和 **.N**，用于表示当前组中的观察数量。这简化了表达式，我们可以省略一对括号。

![没有提供图像的替代文本](../Images/d0a435e726edac8b2dfa565a41abb199.png)

ANSI SQL 表达式简单易懂。

![没有提供图像的替代文本](../Images/cf1334e8735319fcab68085a13ebf857.png)

你可以通过在 GROUP BY 子句中使用列索引来避免字面上的重复。依我看，这不是一个推荐的设计，因为表达式不具自我文档性且不够稳健。实际上，[Apache 已弃用数字的使用](https://issues.apache.org/jira/browse/DRILL-942) 在 GROUP BY 子句中。

kdb+ 表达式更优雅。它不需要括号、引号或任何字词重复。

![没有提供图像的替代文本](../Images/18dd6a40ac5e9e05bc79f239bb8bdf60.png)

SQL 是数据分析的基础，因此每个人可能都理解 ANSI SQL 和 kdb+ 解决方案。R 和 kdb+ 开发人员同意“GROUP BY”过于冗长，简单的“by”字面量就足够表达。

请注意，除了 Pandas 之外，没有语言在这个简单表达式中使用任何引号。在 Pandas 中，查询需要四对引号 [????](https://en.wikipedia.org/wiki/%F0%9F%98%90) 来包裹列名。在 R、SQL 和 kdb+ 中，你可以像引用变量一样引用列。R 中的 .() 表示 list()，允许这种方便的特性。

### 多列的聚合

使用 Pandas 计算单列的总和和平均值以及多个列的总和都非常简单

![没有提供图像的替代文本](../Images/6ec1c299d8ec3781122391df1160c25f.png)

![没有提供图像的替代文本](../Images/bdb598a90547d3cec86cc0947080d21e.png)

![没有提供图像的替代文本](../Images/a236c220eb5a01618c1178fc6015d892.png)

![未提供此图像的替代文本](../Images/abc52c2336cdd49ae81ecc4a7f89f146.png)

如果你尝试将这两种方法结合起来，代码会变得很麻烦，因为这会导致列名冲突。需要引入 [多级列](https://pandas.pydata.org/pandas-docs/stable/advanced.html)和函数 [map](https://docs.python.org/3/library/functions.html#map)。

![未提供此图像的替代文本](../Images/1152478e711c7acec5ae6ff74ebb8b27.png)

![未提供此图像的替代文本](../Images/b579576cf182169ab5bfa0e1bd7b5bf3.png)

SQL、R 和 kdb+ 的等效方式不需要引入任何新概念。新的聚合操作只需用逗号分隔即可。你可以使用关键字 [sum](https://code.kx.com/q/ref/arith-integer/#sum)和 [avg](https://code.kx.com/v2/ref/stats-aggregates/#avg-average)/[mean](https://www.rdocumentation.org/packages/base/versions/3.6.2/topics/mean)来分别获取总和和平均值。

![未提供此图像的替代文本](../Images/2f244c5f53b988918700736159dfec04.png)

观察 kdb+ 表达式的简洁性；它不需要括号或方括号。

![未提供此图像的替代文本](../Images/a229bab4db85c16e330ea10bcdd452b3.png)

### 加权平均

加权平均由 [Numpy](http://www.numpy.org/) 库支持，而 Pandas 依赖于该库。不幸的是，它不能像 np.sum 一样使用。你需要将其包装在 [lambda 表达式](https://docs.python.org/3/tutorial/controlflow.html#lambda-expressions)中，引入一个局部变量，使用 [apply](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.apply.html)而不是 [agg](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.agg.html)，并从系列创建数据框。

![未提供此图像的替代文本](../Images/ddb0578e6a3329675ac8fec347b94763.png)

![未提供此图像的替代文本](../Images/31ddf49336f61983417332e95ee3c09a.png)

标准 SQL 以及其 Google 扩展 BigQuery 都没有提供内置的加权平均函数。你需要记住定义并手动实现它。

![未提供此图像的替代文本](../Images/9451c257a36f1647822fc087988a3722.png)

同样，R 和 Q/Kdb+ 的解决方案也不需要引入任何新概念。加权平均函数是原生支持的，接受两个列名作为参数。

![未提供此图像的替代文本](../Images/4d22bb3a4180e3b8d37f45f5225f8945.png)

在 kdb+ 中，你可以使用中缀表示法来获得更自然的语法——只需说“**w** 加权平均 **x**”。

![未提供此图像的替代文本](../Images/44af1a3b6d157860e89282b6d5e55796.png)

### 一语双关

让我们将所有部分放在一起。我们创建了多个数据框，因此我们需要将它们合并。

![未提供此图像的替代文本](../Images/385ca65b6d18111fbafd145f8b680d10.png)

![未提供此图像的替代文本](../Images/adb4ef52dba992faf73efce55d01ae7f.png)

请注意，第一个 [join](https://docs.python.org/3/library/stdtypes.html#str.join) 表达式与其他表达式无关。它从一个字符串列表中创建一个字符串，而其他表达式则执行 [left joins](https://en.wikipedia.org/wiki/Join_(SQL))。

为了获得最终结果，我们需要三个表达式和一个临时变量**res**。如果我们花更多时间去搜索论坛，我们会发现这个复杂性部分是由于函数 [agg](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.agg.html) 中嵌套字典的弃用造成的。同时，我们可能会发现使用仅函数 [apply](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.apply.html) 的另一种不太被记录的方法，无需连接。不幸的是，这种解决方案返回所有类型为 float 的数值列，因此需要显式地转换整数列。

![未提供图片的替代文本](../Images/43c61ad539c87048ef81ac96b602cc49.png)

这个解决方案需要创建一个可能在源代码中永远不会再使用的临时函数。我们可以将所有语句压缩到一个无状态的解决方案中，但这样会导致代码难以阅读和维护，且会变得嵌套。而且，这第二种方法在中型表格上会更慢。

Pandas 在 [2019年7月18日的发布](https://pandas.pydata.org/pandas-docs/stable/whatsnew/v0.25.0.html) 中支持了通过 [named aggregator](http://pandas-docs.github.io/pandas-docs-travis/user_guide/groupby.html#aggregation) 的分组聚合。它提供了一种比前面提到的基于 apply 的解决方案更优雅的语法，并且不需要类型转换。同时，开发人员可能也认识到过多使用引号带来的痛苦。不幸的是，加权平均数不受支持，因为在聚合中只能使用单列。为了完整性，我们提供了新的正确语法，忽略了加权平均数的计算。很高兴看到输出列名称不再需要引号。

![未提供图片的替代文本](../Images/629c97d73d3196e3a291e0f8889848d6.png)

相比之下，SQL 早在 30 年前就已经能够提供优雅的解决方案。

![未提供图片的替代文本](../Images/9b1f4d0545f1bb90da296f0b3f167ab9.png)

让我们看看 R 是如何使用 `data.table` 来解决这个任务的

![未提供图片的替代文本](../Images/97325ee6d91274a11ecbbea83638169c.png)

以及 kdb+ 中的解决方案是如何呈现的

![未提供图片的替代文本](../Images/50880fb4fe953406cda883a5245f61e9.png)

看起来 kdb+ 具有**最直观、最简单和最可读**的解决方案。它是无状态的，不需要括号/方括号和临时变量（或函数）的创建。

### 那性能怎么样呢？

实验在 Windows 和 Linux 上使用稳定的最新二进制文件和库进行。查询执行了一百次，测试 Jupyter 笔记本可在 [Github.](https://github.com/BodonFerenc/PythonIsThisReallySimple) 找到。数据是随机生成的。桶字段是大小为二的字符串，字段**qty**和**risk**由 64 位整数表示。R 中使用库 [bit64](https://cran.r-project.org/web/packages/bit64/index.html) 来获取 64 位整数。下表总结了以毫秒为单位的执行时间。坐标轴为对数刻度。将两个 Python 3.6.8 版本与 Pandas 0.25.3 比较了三个 R 库（版本 3.6.0）和两个 kdb+ 版本。

![没有提供此图像的替代文本](../Images/50507e7d70a7e844b6ebc76ee774bdac.png)

kdb+ 解决方案不仅比 Pandas 更优雅，而且速度快了一个数量级。R 的 data.table 1.12.6 版本在这个特定查询上比 kdb+ 3.6 慢三倍。kdb+ 4.0 在处理亿级行的表时比 kdb+ 3.6 快五倍。dplyr 0.8.3 版本比 plyr 1.8.5 慢两个数量级。

Pandas 在处理 10 亿行的输入表时达到了内存限制。所有其他解决方案都能处理这个数据量而不会耗尽内存。

让我们看看如何减少执行时间。

### 性能优化

列的桶包含字符串。如果领域大小很小且有很多重复，则建议使用**分类**值而不是字符串。类别类似于枚举，由整数表示，因此它们消耗的内存较少且比较速度更快。你可以通过以下方式在 Pandas 中将字符串转换为分类。

![没有提供此图像的替代文本](../Images/73accb51f6d0fb3b5bc5ef29919a4f3a.png)

但在构建表格时创建分类列更节省内存。我们使用函数 [product](https://docs.python.org/2/library/itertools.html#itertools.product) 通过运用 [笛卡尔积](https://en.wikipedia.org/wiki/Cartesian_product)生成长度为二的字符串全集。在下面的代码片段中，我们省略了创建其他列的语法。*N* 存储要插入的行数。

![没有提供此图像的替代文本](../Images/c5b89afd5ab91b225ef844c1d90ffb9b.png)

在 R 中，类别称为**因素**。类 data.frame 会自动将字符串转换为因素（使用 "stringAsFactors = FALSE" 以避免这种情况），但在 data.table 中，字符串被保留原样，原因很充分。

![没有提供此图像的替代文本](../Images/5b272ddf500676bc805aa50daf5d8217.png)

BigQuery 没有类别或因子的概念。相反，它应用了各种[编码和压缩技术](http://db.csail.mit.edu/projects/cstore/abadisigmod06.pdf)以实现最佳性能。要生成随机字符串，你可以使用 Unicode [码点](https://en.wikipedia.org/wiki/Code_point)、RAND 函数和[CODE_POINTS_TO_STRING](https://cloud.google.com/bigquery/docs/reference/standard-sql/string_functions#code_points_to_string)以及一些类型转换。小写字母 "a" 的码点是 97，你可以通过使用函数[TO_CODE_POINTS](https://cloud.google.com/bigquery/docs/reference/standard-sql/string_functions#to_code_points)来确定这一点。

![此图像未提供替代文本](../Images/ea82a723615b346ff150438ac487e9e7.png)

你可以使用[用户定义函数](https://cloud.google.com/bigquery/docs/reference/standard-sql/user-defined-functions)来避免代码重复。

![此图像未提供替代文本](../Images/871bafeb6392a6867c6ebdd44de59c04.png)

相比之下，在 kdb+ 中，操作看起来是这样的：

![此图像未提供替代文本](../Images/59f019d6bf657f2bec2628a52651a4ab.png)

构造[N?M](https://code.kx.com/q/ref/deal/)会根据 M 的类型生成 N 个随机值。如果 M 是整数、浮点数、日期或布尔值，则返回随机的整数、浮点数、日期或布尔值向量。如果 M 是列表，则会随机选择列表元素。如果 N 是负整数，则结果将不包含重复项。在 kdb+ 中，许多运算符以类似的方式被重载。

在 kdb+ 术语中，枚举被称为**符号**，并被开发者广泛使用。该语言强烈支持符号。你可以在不预定义可能值的情况下使用它们，kdb+ 会为你维护映射。

根据我的测量，通过类别/符号的优化在 Pandas 和 kdb+ 中将运行时间减少了**两倍**。R 的 data.table 显示了不同的特性。使用因子而不是字符串对性能没有影响。这是由于通过**全局字符串池**的内置字符串优化。尽管因子以 32 位整数存储，而字符串需要 64 位指针指向池元素，但这种差异对执行时间的影响微乎其微。

如果我们使用需要更少空间的类型，可以进一步提高性能。例如，如果列**qty**可以放入 2 字节，那么我们可以在 Pandas 中使用[int16](https://numpy.org/devdocs/user/basics.types.html)以及在 kdb+ 中使用[short](https://code.kx.com/q4m3/2_Basic_Data_Types_Atoms/#212-short-and-int)。这会导致较少的内存操作，而内存操作通常是数据分析中的瓶颈。R 不支持 2 字节整数，因此我们使用了其默认的占用 4 字节的整数类型。

默认的 32 位整数（在 R 中）会导致 5-6 倍的执行时间改进。仅在真正需要大基数时使用 64 位整数。特别是对于 dplyr 包尤其如此。

在计算聚合时，我们需要跟踪由于分组而产生的桶。如果表按组列排序，则聚合会更快，因为组已经在 RAM 中连续存储。执行时间在 Pandas 中降到约三分之一，在 R 中降到一半，在 kdb+ 中降到五分之一。下面展示了在排序表上进行类型优化后的执行时间，表大小为 10 亿。

![未提供此图像的替代文本](../Images/f76b3f1c536fe46463888ba67835d7b9.png)

### 并行化

所有现代计算机都有多个 CPU 核心。Pandas 默认在单个核心上运行。我们需要做些什么才能使计算并行化并利用多个核心？

Python 库 [Dask](https://dask.org/) 和 [Ray](https://github.com/ray-project/ray) 是用于执行并行计算的两个最著名的库。库 [Modin](https://modin.readthedocs.io/en/latest/) 是这些引擎之上的 Pandas 数据框的包装器。媒体广泛报道，通过将一行代码替换为

![未提供此图像的替代文本](../Images/cbd9819fe215b1dfa1a2eebe966e7882.png)

到

![未提供此图像的替代文本](../Images/3ca36daed1406cf034705e2564d9f129.png)

这对于少量教科书示例可能是正确的，但在实际生活中并不适用。在我的简单练习中，我遇到了 Ray 和 Dask 的多个问题。让我逐一描述这些问题，从 Ray 开始。

首先，类别类型不受支持。其次，函数 [apply](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.apply.html#pandas.DataFrame.apply) 和 [agg](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.agg.html#pandas.DataFrame.agg) 的行为与相应的 Pandas 函数不同。使用多个聚合与 [agg](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.agg.html#pandas.DataFrame.agg) 中的 group-by 不受支持，因此操作回退到 Pandas 操作。函数 [applies](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.apply.html#pandas.DataFrame.apply) 仅处理返回标量的 lambda 函数。这禁用了第二种更优雅的 Pandas 解决方案。此外，apply 返回一个 Modin 数据框，而不是一个系列。你需要转置结果并将列索引（0）重命名为有意义的列名，例如

![未提供此图像的替代文本](../Images/2921d63f8270367cfdd8f64cebfde02c.png)

最终，代码运行速度明显慢于 Pandas 等效版本，并且在处理 1 亿行数据时出现了故障。Pandas 和其他工具可以轻松处理 10 亿行数据。

转到 Dask 库也有一些麻烦。首先，如果桶的类型是分类的，它不处理加权平均数。我们失去了一个重要的性能提升技巧。其次，你需要提供类型提示以抑制警告。这意味着又一次列名重复。在更优雅的基于 apply 的解决方案中，你需要四次输入输出列名（如 TOTAL_QTY）☹️。因此，转到 Dask 并不像通过简单的 [compute](https://docs.dask.org/en/latest/dataframe.html) 语句扩展代码那样简单。

![没有提供图像的替代文本](../Images/c43adaa24c846a34ec6614a449924c16.png)

**kdb+ 的并行化是自动的**，对于版本 4.0 的磁盘上分区表和内存表都适用。你不会观察到任何类型问题 - 一切运行顺畅。你只需要通过 [命令行参数 -s](https://code.kx.com/q/basics/syscmds/#s-number-of-slaves) 启动 kdb+ 的多进程模式。 [内置的 map-reduce](https://code.kx.com/q4m3/14_Introduction_to_Kdb+/#1437-map-reduce) 分解将计算扩展到多个核心，涵盖大多数操作，包括 [sum](https://code.kx.com/q/ref/sum/)、[avg](https://code.kx.com/q/ref/avg/)、[count](https://code.kx.com/q/ref/count/)、[wavg](https://code.kx.com/q/ref/avg/)、[cor](https://code.kx.com/q/ref/cor/)，等等。你还可以通过手动分区表来获得性能提升，并使用函数 [peach](https://code.kx.com/v2/ref/each/) 并行执行函数。我们需要做的只是从

![没有提供图像的替代文本](../Images/b008380e3dddeb72d3431f2b32531649.png)

到

![没有提供图像的替代文本](../Images/9644443ab45161925cc211f122377808.png)

这种简单的代码修改使得在 16 核心的 kdb+ 版本 3.6 机器上执行速度提高了近一个数量级。由于版本 4.0 已经使用了并行计算，手动并行化没有价值。如果你追求最佳性能，那么代码在 kdb+ 4.0 中将比在 3.6 中更简单。

**R data.table 默认也使用多个线程**，并在后台并行执行查询。你可以通过函数 [setDTthreads](https://www.rdocumentation.org/packages/data.table/versions/1.12.8/topics/setDTthreads) 检查和设置 data.table 使用的线程数量。

让我们比较所有语言中最有效版本的执行时间。对于 SQL，我们评估了 BigQuery，因为它由于大规模并行化被认为是处理大型数据集的最快 SQL 实现。

![没有提供图像的替代文本](../Images/ae75206d32368a5a479ea33b41000b14.png)

kdb+ 再次在这一类别中获胜，BigQuery 排名第二。R data.table 比 Pandas 快两倍。

### 超过 10 亿行

我们的经验中最大的数据表包含十亿行。超过这个数量，表格无法完全加载到内存中，因此 Pandas 不再适用。与其他竞争者相比，专为并行处理和计算机集群设计的 Dask 和 Ray 性能表现较差。**对于 BigQuery，表的大小几乎无关紧要。** 如果我们将行数从十亿增加到十亿或一百亿行，执行时间几乎不会增加。

在 kdb+ 中，数据可以持久化到磁盘，因此可以处理 TB 级别的数据。查询将保持不变，kdb+ 自动应用 map-reduce 并利用多核处理器。此外，如果数据被分段且分段位于具有独立 IO 通道的不同存储上，则 IO 操作将并行执行。这些低级优化使基于 kdb+ 的解决方案能够优雅地扩展。

### 使用 kdb+ 的分布式计算

作为一个最终的简单练习，我们来探讨如何将计算分布到多个 kdb+ 进程中，利用我们的机器集群，并水平分区我们的示例表。要实现类似 Ray/Dask/Spark 的分布式计算，kdb+ 有多困难？

函数 peach 使用外部的、从属的 kdb+ 进程，而不是从属线程，如果变量 [.z.pd](https://code.kx.com/v2/ref/dotz/#zpd-peach-handles) 存储连接到从属 kdb+ 进程的连接句柄。

![未提供此图像的替代文本](../Images/7076b41e633ca03c787d89834e9c2625.png)

我们可以通过桶值拆分表**t**

![未提供此图像的替代文本](../Images/9e2947cfcc84890d63991af6544b3986.png)

最后，我们可以分布式执行 select 语句并合并结果。函数 [raze](https://code.kx.com/q/ref/raze/) 类似于 Pandas 的 [concat](https://pandas.pydata.org/pandas-docs/stable/user_guide/merging.html) 函数。从一个表格列表中，它通过连接生成一个大表。

![未提供此图像的替代文本](../Images/db8042fb22f14074923e27a320a5be16.png)

做得好！我们用四行代码实现了 "kdb-spark"。

### 代码简洁性

我收集了一些关于代码本身的统计数据。虽然短代码不一定意味着干净的代码，但对于这个特定的例子，这些指标与简洁性有很好的相关性。

![未提供此图像的替代文本](../Images/68d2942f4fa51ce053a89dab058e893c.png)

### 结论

我的观察结果汇总在下表中。表情符号 ???? 代表优秀，✔️ 代表良好，☹️ 代表令人失望的表现。

![未提供此图像的替代文本](../Images/2b59ea9e74f33cc6be88ae78a1e435b8.png)

Python 通常是学生学习的第一个编程语言。它简单、性能优越，学习曲线平缓。它的库 Pandas 是将新手引入数据分析世界的自然步骤。Pandas 也常用于专业环境和更复杂的数据分析中。Pandas 在简单的教科书练习中看起来很诱人，但在我们的简单现实用例中使用起来不够方便。

目前更多的关注集中在扩展 Pandas 和通过集群计算支持大表。Ray、Dask 和 Modin 仍处于早期阶段，存在许多限制。在我们的用例中，它们只是增加了语法复杂性，并且实际上降低了性能。

R 在简洁性、优雅性和性能方面都优于 Pandas。它拥有几个内置的优化功能，例如固有的多线程。字符串表示的优化效果非常好，使得开发人员可以专注于分析而非细节问题。不足为奇的是，R data.table 正在 [移植到 Python](https://github.com/h2oai/datatable)。也许未来 data.table 包会成为 Pandas 的替代品？

kdb+ 将数据分析提升到了一个新的水平。它设计用于极高的生产力。在我们的用例中，它在简洁性、优雅性和性能方面都是明显的赢家。难怪资本市场的 20 家顶尖组织中有 20 家选择了 kdb+ 作为数据分析的主要工具。在这个行业中，数据分析推动了收入，工具在极端条件下进行测试。

如果你没有手头的计算机集群，那么 BigQuery 在处理超过 100 亿行的数据时表现出色。如果你需要分析巨大的表格且对运行时间非常敏感，那么 BigQuery 能很好地满足你的需求。

### 相关工作

[数据库操作基准测试](https://h2oai.github.io/db-benchmark/) 最初由 [Matt Dowle](https://twitter.com/MattDowle) 启动，他是 R data.table 的创建者。除了 Pandas、data.table、dask dplyr，他们还测试了 [Apache Spark](https://spark.apache.org/)、[ClickHouse](https://clickhouse.yandex/) 和 [Julia 数据帧](https://juliadata.github.io/DataFrames.jl/stable/) 在各种查询和不同参数下的表现。任何人都可以并排查看这些查询，甚至可以下载测试环境以进行自定义硬件的实验。

[Mark Litwintschik](https://www.linkedin.com/in/marklitwintschik/) 使用了 NYC 出租车驾驶数据库和四个查询，测试了 [33 个数据库类系统](https://tech.marksblogg.com/benchmarks.html)。他提供了测试环境的详细描述、所用设置以及一些宝贵的个人备注。这是一项全面的工作，展示了 Mark 在大数据平台方面的杰出知识。他的观察与我们的实验一致，kdb+ 在非 GPU 基于的数据库解决方案中表现出最快的速度。

STAC-M3基准测试最初由几家全球最大的银行和交易公司于[2010年开发](https://stacresearch.com/system/files/central/STAC-M3_Overview.pdf)。其目的是精确测量新兴硬件和软件创新如何提高时间序列分析的性能。在STAC-M3开发之后，kdb+迅速成为硬件供应商运行测试时的首选数据库平台，因为它设定了其他软件供应商无法超越的性能标准。此外，Google也认可STAC-M3作为时间序列分析的行业标准基准。他们使用kdb+来[展示](https://cloud.google.com/blog/products/compute/can-cloud-instances-perform-better-than-bare-metal-latest-stac-m3-benchmarks-say-yes)将数据和工作负载从本地迁移到GCP并不意味着性能的妥协。

### 致谢

我想感谢[Péter Györök](https://github.com/gyorokpeter)、[Péter Simon Vargha](https://www.linkedin.com/in/varghaps/)和[Gergely Daróczi](https://www.linkedin.com/in/daroczig/)提供的有见地的建议。

**简介：[Ferenc Bodon博士](https://www.linkedin.com/in/ferencbodon/)** 是一位经验丰富的数据工程师、软件开发人员、多语言程序员和软件架构师，拥有数据挖掘和统计学的学术背景。他具备长期思考能力，始终致力于寻找高质量、可靠且可扩展的解决方案，并允许快速开发。相信软件质量，并且在找到“卓越解决方案”之前无法放松。

[原文](https://www.linkedin.com/pulse/python-data-analysis-really-simple-ferenc-bodon-ph-d-/)。经许可转载。

**相关：**

+   [10个简单技巧加速你的Python数据分析](/2019/07/10-simple-hacks-speed-data-analysis-python.html)

+   [你所需的最后一份SQL数据分析指南](/2019/10/last-sql-guide-data-analysis-ever-need.html)

+   [柏林租金冻结：我可以在网上找到多少非法的高价房源？](/2020/03/berlin-rent-freeze-illegal-overpriced-offers.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的捷径。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在IT方面

* * *

### 更多相关话题

+   [每个数据科学家都应了解的三个R库（即使你使用Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [是什么让Python成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [停止学习数据科学以寻找目的，并以寻找目的为…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学的顶级统计资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的5个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [一项90亿美元的AI失败，经过审查](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)
