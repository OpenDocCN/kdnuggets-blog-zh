# 7 步骤掌握 pandas 和 Python 的数据整理

> 原文：[`www.kdnuggets.com/7-steps-to-mastering-data-wrangling-with-pandas-and-python`](https://www.kdnuggets.com/7-steps-to-mastering-data-wrangling-with-pandas-and-python)

![7 步骤掌握 pandas 和 Python 的数据整理](img/ff0b15afe644696456cd7f6fe13a1582.png)

使用 DALLE 3 生成的图像

你是一个有志成为数据分析师的人吗？如果是的话，学习使用 [pandas](https://pandas.pydata.org/) 进行数据整理是你工具箱中必备的技能。

* * *

## 我们的前 3 个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织中的 IT

* * *

几乎所有的数据科学课程和训练营都在其课程中涵盖 pandas。尽管 pandas 易于学习，但其习惯用法和掌握常用函数及方法调用仍需要练习。

本指南将学习 pandas 分解为 7 个简单的步骤，从你可能已经*熟悉*的内容开始，逐步探索 pandas 的强大功能。从先决条件，到各种数据整理任务，再到构建仪表板，这是一个全面的学习路径。

# 步骤 1：Python 和 SQL 基础

如果你希望进入数据分析或数据科学领域，你首先需要掌握一些基本的编程技能。我们建议从 Python 或 R 开始，但本指南将重点关注 Python。

## 学习 Python 和网页抓取

要刷新你的 Python 技能，你可以使用以下资源之一：

+   [免费 Python 入门视频讲座 - freeCodeCamp](https://www.youtube.com/watch?v=8DvywoWv6fI)

+   [Python 课程 - Kaggle](https://www.kaggle.com/learn/python)

Python 易于学习和构建。你可以专注于以下主题：

+   **Python 基础**：熟悉 Python 语法、数据类型、控制结构、内置数据结构和基本的面向对象编程 (OOP) 概念。

+   **网页抓取基础**：学习网页抓取的基本知识，包括 HTML 结构、HTTP 请求和解析 HTML 内容。熟悉用于网页抓取任务的库，如 BeautifulSoup 和 requests。

+   **连接数据库**：学习如何使用 [SQLAlchemy](https://www.sqlalchemy.org/) 或 [psycopg2](https://pypi.org/project/psycopg2/) 等库将 Python 连接到数据库系统。了解如何从 Python 执行 SQL 查询并从数据库中检索数据。

虽然不是强制性的，但使用 [Jupyter Notebooks](https://jupyter.org/) 进行 Python 和网络抓取练习可以提供一个互动的学习和实验环境。

## 学习 SQL

SQL 是数据分析的必备工具；**那么学习 SQL 如何帮助你学习 pandas 呢？**

一旦您了解编写 SQL 查询的逻辑，将这些概念转移到 pandas 数据框上进行类似操作是非常简单的。

学习 SQL（结构化查询语言）的基础知识，包括如何创建、修改和查询关系数据库。了解 SQL 命令，如 SELECT、INSERT、UPDATE、DELETE 和 JOIN。

要学习和刷新您的 SQL 技能，您可以使用以下资源：

+   [SQL 教程 - 可汗学院](https://www.khanacademy.org/computing/computer-programming/sql)

+   [SQL 入门 - Kaggle 学习](https://www.kaggle.com/learn/intro-to-sql)

+   [高级 SQL - Kaggle 学习](https://www.kaggle.com/learn/advanced-sql)

通过掌握本步骤中概述的技能，您将拥有坚实的 Python 编程、SQL 查询和网络抓取基础。这些技能作为更高级的数据科学和分析技术的基石。

# 第 2 步：从各种来源加载数据

首先，设置您的工作环境。安装 pandas（及其所需的依赖项，如 NumPy）。遵循最佳实践，例如使用虚拟环境来管理项目级安装。

如前所述，pandas 是一个强大的 Python 数据分析库。然而，在开始使用 pandas 之前，您应该熟悉基本的数据结构：**pandas DataFrame** 和 **series**。

为了分析数据，您首先需要将数据从其源加载到 pandas 数据框中。学习如何从各种来源（如 CSV 文件、Excel 电子表格、关系数据库等）摄取数据是很重要的。以下是概述：

+   **从 CSV 文件中读取数据**：学习如何使用`pd.read_csv()`函数从逗号分隔值（CSV）文件中读取数据并将其加载到 DataFrame 中。了解您可以使用的参数，以自定义导入过程，例如指定文件路径、分隔符、编码等。

+   **从 Excel 文件中导入数据**：探索 `pd.read_excel()` 函数，它允许您从 Microsoft Excel 文件（.xlsx）中导入数据，并将其存储在 DataFrame 中。了解如何处理多个工作表并自定义导入过程。

+   **从 JSON 文件中加载数据**：学习使用 `pd.read_json()` 函数从 JSON（JavaScript 对象表示法）文件中导入数据并创建 DataFrame。了解如何处理不同的 JSON 格式和嵌套数据。

+   **从 Parquet 文件中读取数据**：了解 `pd.read_parquet()` 函数，它允许您从 Parquet 文件（一种列式存储文件格式）中导入数据。学习 Parquet 文件如何在大数据处理和分析中提供优势。

+   **从关系数据库表中导入数据**：了解`pd.read_sql()`函数，它允许你从关系数据库查询数据并将其加载到数据框中。了解如何建立数据库连接、执行 SQL 查询，并直接将数据提取到 pandas 中。

我们现在已经学习了如何将数据集加载到 pandas 数据框中。那么接下来呢？

# 第 3 步：选择行和列，筛选数据框

接下来，你应该学习如何从 pandas 数据框中选择特定的行和列，以及如何根据特定标准筛选数据。学习这些技巧对于数据处理和从数据集中提取相关信息至关重要。

## 索引和切片数据框

理解如何**根据标签或整数位置选择特定的行和列**。你应该学习如何使用`.loc[]`、`.iloc[]`和布尔索引来切片和索引数据框。

+   `.loc[]`：此方法用于**基于标签的索引**，允许你通过标签选择行和列。

+   `.iloc[]`：此方法用于**基于整数索引**，使你能够通过整数位置选择行和列。

+   布尔索引：这种技术涉及使用布尔表达式根据特定条件筛选数据。

按名称选择列是常见的操作。因此，学习如何使用列名访问和检索特定列。练习使用单列选择和一次选择多列。

## 筛选数据框

在筛选数据框时，你应该熟悉以下内容：

+   **根据条件筛选**：理解如何使用布尔表达式根据特定条件筛选数据。学习使用比较运算符（>、<、==等）创建过滤器，以提取满足特定标准的行。

+   **组合过滤器**：学习如何使用逻辑运算符如'&'（和）、'|'（或）和'~'（非）组合多个过滤器。这将使你能够创建更复杂的筛选条件。

+   **使用 isin()**：学习使用`isin()`方法根据值是否在指定列表中进行数据筛选。这对提取某列值匹配任何提供项的行非常有用。

通过学习本步骤中概述的概念，你将能够有效地从 pandas 数据框中选择和筛选数据，从而提取出最相关的信息。

## 关于资源的快速说明

对于第 3 到第 6 步，你可以使用以下资源进行学习和实践：

+   [10 分钟了解 pandas - pandas 用户指南](https://pandas.pydata.org/docs/user_guide/10min.html)

+   [通过实例学习 Pandas 和 Python 数据分析 - freeCodeCamp](https://www.youtube.com/watch?v=gtjxAH8uaP0)

+   [Intro to pandas - Kaggle Learn](https://www.kaggle.com/learn/pandas)

# 第 4 步：探索和清理数据集

到目前为止，你已经知道如何将数据加载到 pandas 数据框中，选择列和过滤数据框。在这一步，你将学习如何使用 pandas 探索和清理数据集。

探索数据帮助你理解其结构、识别潜在问题，并在进一步分析之前获得洞察。清理数据包括处理缺失值、处理重复项以及确保数据一致性。

+   **数据检查**：学习如何使用 `head()`、`tail()`、`info()`、`describe()` 和 `shape` 属性来概述数据集。这些方法提供有关前几行/最后几行、数据类型、摘要统计信息和数据框维度的信息。

+   **处理缺失数据**：了解处理数据集中的缺失值的重要性。学习如何使用 `isna()` 和 `isnull()` 方法识别缺失数据，并使用 `dropna()`、`fillna()` 或插补方法处理这些缺失值。

+   **处理重复项**：学习如何使用 `duplicated()` 和 `drop_duplicates()` 方法检测和删除重复行。重复项可能会扭曲分析结果，应加以处理以确保数据准确性。

+   **清理字符串列**：学习使用 `.str` 访问器和字符串方法执行字符串清理任务，如去除空格、提取和替换子字符串、拆分和连接字符串等。

+   **数据类型转换**：了解如何使用 `astype()` 等方法转换数据类型。将数据转换为适当的类型可以确保数据准确表示并优化内存使用。

此外，你可以通过简单的可视化来探索数据集，并执行数据质量检查。

## 数据探索和数据质量检查

使用 **可视化和统计分析** 来获得数据洞察。学习如何使用 pandas 和其他库如 Matplotlib 或 Seaborn 创建基本图表，以可视化数据中的分布、关系和模式。

执行 **数据质量检查** 以确保数据完整性。这可能涉及验证值是否在预期范围内、识别异常值或检查相关列的一致性。

你现在知道如何探索和清理数据集，从而获得更准确和可靠的分析结果。适当的数据探索和清理对于任何数据科学项目都至关重要，因为它们为成功的数据分析和建模奠定了基础。

# 第 5 步：转换、分组和聚合

到现在为止，你已经能够使用 pandas DataFrames 并执行基本操作，如选择行和列、过滤和处理缺失数据。

你通常会想要 **根据不同标准总结数据**。为此，你应该学习如何执行数据转换、使用 GroupBy 功能以及对数据集应用各种聚合方法。可以进一步分解为以下内容：

+   **数据转换**：学习如何使用添加或重命名列、删除不必要的列以及在不同格式或单位之间转换数据等技术来修改数据。

+   **应用函数**：理解如何使用 `apply()` 方法将自定义函数应用于数据框，从而以更灵活和定制的方式转换数据。

+   **数据重塑**：探索其他数据框方法，如 `melt()` 和 `stack()`，这些方法允许你重塑数据，使其适合特定的分析需求。

+   **GroupBy 功能**：`groupby()` 方法允许你根据特定的列值对数据进行分组。这使你可以在每个组的基础上执行聚合和数据分析。

+   **聚合函数**：了解常见的聚合函数，如总和、均值、计数、最小值和最大值。这些函数与 `groupby()` 一起使用，用于汇总数据并计算每个组的描述统计。

本步骤中概述的技术将帮助你有效地转换、分组和聚合数据。

# 第 6 步：连接和透视表

接下来，你可以通过学习如何使用 pandas 执行数据连接和创建透视表来提升技能。**连接** 允许你根据共同的列合并来自多个数据框的信息，而 **透视表** 帮助你以表格格式汇总和分析数据。以下是你应该了解的内容：

+   **合并数据框**：了解不同类型的连接，如内连接、外连接、左连接和右连接。学习如何使用 `merge()` 函数根据共享列合并数据框。

+   **连接**：学习如何使用 `concat()` 函数纵向或横向连接数据框。当你需要合并结构相似的数据框时，这非常有用。

+   **索引操作**：理解如何设置、重置和重命名数据框中的索引。正确的索引操作对于有效地执行连接和创建透视表至关重要。

+   **创建透视表**：`pivot_table()` 方法允许你将数据转换为汇总和交叉表格式。学习如何指定所需的聚合函数，并根据特定的列值对数据进行分组。

可选地，你可以探索如何创建多级透视表，通过多列作为索引级别来分析数据。通过足够的练习，你将知道如何使用连接将多个数据框的数据结合起来，并创建信息丰富的透视表。

# 第 7 步：构建数据仪表板

现在你已经掌握了 pandas 数据整理的基础知识，是时候通过**构建数据仪表板**来检验你的技能了。

构建交互式仪表板将帮助你提升数据分析和可视化技能。为此步骤，你需要熟悉 Python 中的数据可视化。[数据可视化 - Kaggle Learn](https://www.kaggle.com/learn/data-visualization) 是一个全面的入门介绍。

当你在数据领域寻找机会时，你需要拥有一系列项目作品集——而且你需要超越 Jupyter notebooks 中的数据分析。是的，你可以学习并使用 Tableau。但你可以在 Python 基础上进行扩展，使用 Python 库 [Streamlit](https://streamlit.io/) 来构建仪表板。

Streamlit 帮助你构建交互式仪表板——无需担心编写数百行 HTML 和 CSS 代码。

如果你在寻找灵感或学习 Streamlit 的资源，你可以查看这个免费的课程：[使用 Python 和 Streamlit 构建 12 个数据科学应用](https://www.youtube.com/watch?v=JwSS70SZdyM)，包括股票价格、体育和生物信息学数据的项目。选择一个真实世界的数据集，分析它，并构建一个数据仪表板，以展示你的分析结果。

# 下一步

在具备扎实的 Python、SQL 和 pandas 基础之后，你可以开始申请并面试数据分析师职位。

我们已经包括了构建数据仪表板的内容：从数据收集到仪表板和洞察。因此，确保建立一个项目作品集。在此过程中，超越常规，包含那些你*真正喜欢*的项目。如果你喜欢阅读或音乐（大多数人都是这样），试着分析你的 Goodreads 和 Spotify 数据，构建一个仪表板，并加以改进。继续努力！

**[](https://twitter.com/balawc27)**[Bala Priya C](https://www.kdnuggets.com/wp-content/uploads/bala-priya-author-image-update-230821.jpg)**** 是来自印度的开发者和技术作家。她喜欢在数学、编程、数据科学和内容创作的交汇点上工作。她的兴趣和专长包括 DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编程和咖啡！目前，她正致力于学习并通过编写教程、操作指南、评论文章等方式与开发者社区分享她的知识。Bala 还创建了引人入胜的资源概述和编程教程。

### 更多相关话题

+   [掌握 SQL、Python、数据清洗、数据处理等的指南合集](https://www.kdnuggets.com/collection-of-guides-on-mastering-sql-python-data-cleaning-data-wrangling-and-exploratory-data-analysis)

+   [掌握数据清洗的 7 个步骤，使用 Python 和 Pandas](https://www.kdnuggets.com/7-steps-to-mastering-data-cleaning-with-python-and-pandas)

+   [KDnuggets™ 新闻 22:n05, 2 月 2 日：掌握机器…的 7 个步骤](https://www.kdnuggets.com/2022/n05.html)

+   [掌握数据科学中的 Python 的 7 个步骤](https://www.kdnuggets.com/2022/06/7-steps-mastering-python-data-science.html)

+   [2022 年掌握机器学习的 7 个步骤](https://www.kdnuggets.com/2022/02/7-steps-mastering-machine-learning-python.html)

+   [掌握数据清洗和预处理技术的 7 个步骤](https://www.kdnuggets.com/2023/08/7-steps-mastering-data-cleaning-preprocessing-techniques.html)
