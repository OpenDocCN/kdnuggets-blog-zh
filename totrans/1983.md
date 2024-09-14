# 机器学习 ETL 的最佳实践

> 原文：[https://www.kdnuggets.com/best-practices-for-building-etls-for-ml](https://www.kdnuggets.com/best-practices-for-building-etls-for-ml)

机器学习工程的一个关键部分是构建可靠且可扩展的数据提取、转换、丰富和加载的过程。这是数据科学家与机器学习工程师最密切合作的组成部分之一。通常，数据科学家会提出一个粗略的数据集设计方案。理想情况下，不是在 Jupyter notebook 上。然后，机器学习工程师加入这个任务，以帮助使代码更具可读性、效率和可靠性。

机器学习 ETLs 可以由多个子 ETL 或任务组成，并且可以以非常不同的形式体现。一些常见的例子：

+   基于 Scala 的 Spark 任务读取和处理存储在 S3 中的 Parquet 文件格式的事件日志数据，并通过 Airflow 每周计划。

+   Python 进程通过计划的 AWS Lambda 函数执行 Redshift SQL 查询。

+   复杂的 pandas 重处理通过 Sagemaker 处理作业执行，使用 [EventBridge 触发器](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-notebook-jobs/studio-scheduling/scheduled-example.html#Scale-interactive-experimentation-to-scheduled-jobs-on-SageMaker-Studio-Notebooks-without-changing-code.)。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT 工作

* * *

# ETL 中的实体

我们可以在这些类型的 ETL 中识别不同的实体，我们有***源***（原始数据存储的位置）、***目标***（最终数据制品存储的位置）、***数据*** ***处理***（数据如何被读取、处理和加载）和***触发器***（ETL 如何被启动）。

![机器学习 ETL 的最佳实践](../Images/d415303a783e9bd7c4dc794639ad097d.png)

+   在*源*部分，我们可以有如 AWS Redshift、AWS S3、Cassandra、Redis 或外部 API 等存储位置。*目标*也是如此。

+   *数据处理* 通常在临时的 Docker 容器中运行。我们可以通过使用 Kubernetes 或其他 AWS 管理服务，如 AWS ECS 或 AWS Fargate，再增加一层抽象。甚至可以使用 SageMaker Pipelines 或 Processing Jobs。通过利用特定的数据处理引擎，如 Spark、Dask、Hive、Redshift SQL 引擎，你可以在集群中运行这些过程。同时，你还可以使用 Python 进程和 Pandas 进行简单的单实例数据处理。除此之外，还有一些其他有趣的框架，如 Polars、Vaex、Ray 或 Modin，它们可以用于处理中间解决方案。

+   最受欢迎的 *触发器* 工具是 Airflow。其他可用的工具有 Prefect、Dagster、Argo Workflows 或 Mage。

![构建机器学习 ETLs 的最佳实践](../Images/c3c1282ef8593296b444398dfdb22dce.png)

# 我是否应该使用框架？

框架是一组抽象、约定和开箱即用的工具，可以用于创建更统一的代码库，当应用于具体问题时。框架对于 ETLs 非常方便。正如我们之前描述的那样，有很多非常通用的实体可以被抽象或封装，以生成全面的工作流。

我构建内部数据处理框架的进展步骤如下：

+   首先建立一个连接不同 *源* 和 *目标* 的库。在你进行的不同项目中，根据需要实现这些连接器。这是避免 *YAGNI* 的最佳方式。

+   创建简单且自动化的开发工作流，使你能够快速迭代代码库。例如，配置 CI/CD 工作流以自动测试、代码检查和发布你的包。

+   创建实用工具，如读取 SQL 脚本、启动 Spark 会话、日期格式化函数、元数据生成器、日志记录工具、获取凭据和连接参数的函数，以及警报工具等。

+   在构建工作流的内部框架与使用现有框架之间进行选择。在考虑内部开发时，复杂性范围非常广泛。你可以从一些简单的约定开始构建工作流，最终可能会构建一些基于 DAG 的库，如 [Luigi](https://luigi.readthedocs.io/en/stable/) 或 [Metaflow](https://metaflow.org/)。这些是你可以使用的流行框架。

# 构建 *“Utils”* 库

这是你的数据代码库的一个关键和核心部分。所有的处理过程将使用这个库来将数据从一个源移动到另一个目标。一个扎实且经过深思熟虑的初始软件设计是关键。

![构建机器学习 ETLs 的最佳实践](../Images/e851b582d0e7d9c4024ed7b35dde3019.png)

但我们为什么要这样做呢？主要原因是：

+   **重用性**：在不同的软件项目中使用相同的软件组件可以提高生产力。这个软件组件只需开发一次。然后，它可以集成到其他软件项目中。但这个想法并不新鲜。我们可以追溯到[1968年的一次会议](https://www.cs.dartmouth.edu/~doug/components.txt)，其目标是解决所谓的[软件危机](https://en.wikipedia.org/wiki/Software_crisis)。

+   **封装**：库中不同连接器的所有内部细节不需要展示给最终用户。因此，提供一个可理解的接口就足够了。例如，如果我们有一个数据库连接器，我们不希望连接字符串暴露为连接器类的公共属性。通过使用库，我们可以确保对数据源的安全访问得到保障。***审查此部分***

+   **更高质量**的代码库：我们只需开发一次测试。因此，开发者可以依赖于这个库，因为它包含了一个测试套件（理想情况下，测试覆盖率非常高）。在调试错误或问题时，我们可以忽略，至少在初次检查时，问题是否出在库中，只要我们对测试套件有信心。

+   **标准化** / “*观点*”：拥有一个连接器库在某种程度上决定了你开发ETL的方式。这是好的，因为组织中的ETL将具有相同的数据提取或写入方式。标准化有助于更好的沟通、更高的生产力以及更好的预测和规划。

在构建这种类型的库时，团队承诺在一段时间内维护它，并承担在需要时实施复杂重构的风险。需要进行这些重构的原因可能包括：

+   组织迁移到不同的公共云。

+   数据仓库引擎发生变化。

+   新的依赖版本破坏了接口。

+   需要进行更多的安全权限检查。

+   新团队进来，对库的设计有不同的意见。

## 接口类

如果你想让你的ETL与*来源*或*目的地*无关，创建基类的接口类是一个好的决定。接口作为模板定义。

例如，你可以有抽象类来定义*DatabaseConnector*所需的方法和属性。让我们展示一个简化的示例，说明这个类可能的样子：

```py
from abc import ABC

class DatabaseConnector(ABC):

    def __init__(self, connection_string: str):
        self.connection_string = connection_string

    @abc.abstractmethod
    def connect(self):
        pass

    @abc.abstractmethod
    def execute(self, sql: str):
        pass
```

其他开发者可以从*DatabaseConnector*子类化并创建新的具体实现。例如，可以以这种方式实现**MySqlConnector**或**CassandraDbConnector**。这将帮助最终用户快速理解如何使用任何从*DatabaseConnector*子类化的连接器，因为它们都将具有相同的接口（相同的方法）。

```py
mysql = MySqlConnector(connection_string)
mysql.connect()
mysql.execute("SELECT * FROM public.table")

cassandra = CassandraDbConnector(connection_string)
cassandra.connect()
cassandra.execute("SELECT * FROM public.table")
```

简单的接口与命名良好的方法非常强大，有助于提高生产力。因此，我建议花时间仔细思考这一点。

## 正确的文档

文档不仅仅指代码中的*docstrings*和内联注释。它还指你关于库的周边解释。对包的最终目标进行明确的陈述，以及对贡献的要求和指南进行清晰的解释是至关重要的。

例如：

```py
*"This utils library will be used across all the ML data pipelines and feature engineering jobs to provide simple and reliable connectors to the different systems in the organization".*
```

或者

```py
*"This library contains a set of feature engineering methods, transformations and algorithms that can be used out-of-the-box with a simple interface that can be chained in a scikit-learn-type of pipeline".*
```

对库有一个清晰的使命有助于贡献者的正确理解。这就是为什么开源库（例如：pandas、scikit-learn 等）在这些年中获得了如此大的受欢迎程度。贡献者接受了库的目标，并承诺遵循规定的标准。我们在组织中也应该做得类似。

在任务陈述之后，我们应该开发基础的软件架构。我们希望接口的样子是什么？我们应该通过接口方法中的更多灵活性（例如：更多的参数导致不同的行为）还是通过更细粒度的方法（例如：每个方法都有非常具体的功能）来覆盖功能？

在此之后，制定风格指南。概述首选的模块层次结构、所需的文档深度、如何发布 PR、覆盖要求等。

关于代码中的文档，docstrings 需要充分描述函数的行为，但我们不应仅仅复制函数名称。有时，函数名称已经足够表达其功能，此时 docstring 解释其行为就显得多余。保持简洁准确。举个简单的例子：

❌不行！

```py
class NeptuneDbConnector:
	...
	def close():
	    """This function checks if the connection to the database
             is opened. If it is, it closes it and if it doesn’t,
             it does nothing.
          """ 
```

✅是的！

```py
class NeptuneDbConnector:
	...
	def close():
	    """Closes connection to the database."""
```

说到内联注释，我总是喜欢用它们来解释一些可能看起来奇怪或不规则的事情。此外，如果我必须使用复杂的逻辑或花哨的语法，最好在该片段上方写一个清晰的解释。

```py
# Getting the maximum integer of the list
l = [23, 49, 6, 32]
reduce((lambda x, y: x if x > y else y), l)
```

此外，你还可以包含指向 Github 问题或 Stackoverflow 回答的链接。这非常有用，特别是当你需要编写一些奇怪的逻辑来解决已知的依赖问题时。当你实施了从 Stackoverflow 获得的优化技巧时，这也是非常方便的。

在我看来，这两者——接口类和清晰的文档——是让共享库长时间保持活力的最佳方法。它将抵御懒散和保守的新开发者，同时也能适应充满活力、激进且意见明确的开发者。更改、改进或革命性的重构将会顺利进行。

# 将软件设计模式应用于 ETL

从代码的角度来看，ETL 应该有 3 个明显区分的高级功能。每个功能与以下步骤之一相关：*提取、转换、加载*。这是 ETL 代码最简单的要求之一。

```py
def extract(source: str) -> pd.DataFrame:
    ...

def transform(data: pd.DataFrame) -> pd.DataFrame:
    ...

def load(transformed_data: pd.DataFrame):
    ...
```

显然，将这些函数命名为这样并不是强制的，但它会提高可读性，因为这些术语是广泛接受的。

## DRY（不要重复自己）

这是一个伟大的设计模式之一，为连接器库提供了正当理由。你写一次，并在不同的步骤或项目中重用它。

## 函数式编程

这是一种编程风格，旨在使函数“纯”或无副作用。输入必须是不可变的，给定这些输入，输出总是相同的。这些函数更容易在隔离环境下进行测试和调试。因此，为数据管道提供了更好的可重复性。

应用函数式编程到ETL时，我们应该能够提供*幂等性*。这意味着每次运行（或重新运行）管道时，应该返回相同的输出。具有这种特性，我们可以自信地操作ETL，确保重复运行不会生成重复数据。你曾经多少次需要创建一个奇怪的SQL查询来删除错误ETL运行中插入的行？确保*幂等性*有助于避免这些情况。Maxime Beauchemin，Apache Airflow和Superset的创始人，是[*函数式数据工程*](https://maximebeauchemin.medium.com/functional-data-engineering-a-modern-paradigm-for-batch-data-processing-2327ec32c42a)*的知名倡导者。

## SOLID

我们将使用类定义的引用，但这一部分也可以应用于一等函数。我们将使用重度面向对象编程来解释这些概念，但这并不意味着这是开发ETL的最佳方式。没有具体的共识，每家公司都有自己的方法。

关于*单一职责原则*，你必须创建只有一个变化原因的实体。例如，将职责分隔到两个对象中，例如*SalesAggregator*和*SalesDataCleaner*类。后者可能包含特定的业务规则来“清理”销售数据，而前者则专注于从不同系统中提取销售数据。这两个类的代码可能会因为不同的原因而变化。

对于*开放-封闭原则*，实体应该可以扩展以添加新功能，但不应开放以进行修改。想象一下，*SalesAggregator*接收了一个*StoresSalesCollector*作为组件，用于从实体店提取销售数据。如果公司开始在线销售并且我们想获取这些数据，我们会声明*SalesCollector*对于扩展是开放的，只要它也能接收另一个具有兼容接口的*OnlineSalesCollector*。

```py
from abc import ABC, abstractmethod

class BaseCollector(ABC):
      @abstractmethod
      def extract_sales() -> List[Sale]:
            pass

class SalesAggregator:

      def __init__(self, collectors: List[BaseCollector]):
		self.collectors = collectors

      def get_sales(self) -> List[Sale]: 
		sales = []
		for collector in self.collectors:
			sales.extend(collector.extract_sales())
		return sales

class StoreSalesCollector:
	def extract_sales() -> List[Sale]:
		# Extract sales data from physical stores

class OnlineSalesCollector:
	def extract_sales() -> List[Sale]:
		# Extract online sales data

if __name__ == "__main__":
     sales_aggregator = SalesAggregator(
            collectors = [
                StoreSalesCollector(),
                OnlineSalesCollector()
            ]
     sales = sales_aggregator.get_sales()
```

里氏替换原则，或[*行为子类型*](https://www.youtube.com/watch?v=-Z-17h3jG0A)并不容易直接应用于ETL设计，但对于我们之前提到的实用程序库却适用。这个原则试图为子类型设定规则。在使用超类型的程序中，理论上可以用一个子类型来替代它，而不会改变程序的行为。

```py
from abc import ABC, abstractmethod

class DatabaseConnector(ABC):
	def __init__(self, connection_string: str):
		self.connection_string = connection_string

	@abstractmethod
	def connect():
		pass

	@abstractmethod
	def execute_(query: str) -> pd.DataFrame:
		pass

class RedshiftConnector(DatabaseConnector):
	def connect():
	# Redshift Connection implementation

	def execute(query: str) -> pd.DataFrame:
	# Redshift Connection implementation

class BigQueryConnector(DatabaseConnector):
	def connect():
	# BigQuery Connection implementation

	def execute(query: str) -> pd.DataFrame:
	# BigQuery Connection implementation

class ETLQueryManager:
	def __init__(self, connector: DatabaseConnector, connection_string: str):
		self.connector = connector(connection_string=connection_string).connect()

	def run(self, sql_queries: List[str]):
		for query in sql_queries:
			self.connector.execute(query=query)
```

在下面的例子中，我们看到任何*DatabaseConnector*的子类型都符合 Liskov 替换原则，因为其任何子类型都可以在*ETLManager*类中使用。

现在，让我们谈谈*接口隔离原则*。它指出，客户端不应依赖于它们不使用的接口。这对于*DatabaseConnector*设计非常有用。如果你在实现*DatabaseConnector*，不要用在 ETL 上下文中不会使用的方法来过载接口类。例如，你不需要*grant_permissions*或*check_log_errors*等方法。这些方法与数据库的管理使用有关，而这并不是我们关注的内容。

另一个重要的原则是*依赖倒置*原则。这个原则指出，高层模块不应该依赖于低层模块，而应该依赖于抽象。这个行为在上面的*SalesAggregator*中得到了明确的体现。注意，它的*__init__*方法不依赖于*StoreSalesCollector*或*OnlineSalesCollector*的具体实现。它基本上依赖于一个*BaseCollector*接口。

# 一个优秀的 ML ETL 应该是什么样的？

我们在上面的例子中重度依赖面向对象的类，以展示如何将 SOLID 原则应用于 ETL 作业。然而，关于构建 ETL 时最好的代码格式和标准，没有普遍的共识。它可以有非常不同的形式，更多的是一个拥有内部良好文档化的观点框架的问题，而不是试图制定一个行业范围内的全球标准。

![构建 ML 用 ETL 的最佳实践](../Images/0b94f2f9bd0e78878fcfbaf31a3dd7bc.png)

因此，在这一部分，我将尝试专注于解释一些使 ETL 代码更易读、更安全、更可靠的特征。

## 命令行应用程序

所有*数据处理*本质上都是命令行应用程序。在用 Python 开发 ETL 时，总是提供一个参数化的 CLI 接口，以便可以从任何地方执行它（例如，可以在 Kubernetes 集群下运行的 Docker 容器）。有许多工具可以构建命令行参数解析，如 argparse、[click](https://github.com/pallets/click)、[typer](https://github.com/tiangolo/typer)、yaspin 或 docopt。Typer 可能是最灵活、易用且对现有代码库侵入性最小的。它由著名的 Python 网络服务库 FastApi 的创作者开发，其 Github 星标数不断增长。文档很出色，并且正变得越来越符合行业标准。

```py
from typer import Typer

app = Typer()

@app.command()
def run_etl(
    environment: str,
    start_date: str,
    end_date: str,
    threshold: int
):
    ...
```

要运行上述命令，你只需要做：

```py
python {file_name}.py run-etl --environment dev --start-date 2023/01/01 --end-date 2023/01/31 --threshold 10
```

## 处理与数据库引擎计算的权衡

构建基于数据仓库的 ETL 时，通常建议将尽可能多的计算处理推送到数据仓库。如果你拥有一个根据需求自动扩展的数据仓库引擎，这完全没问题。但并非每个公司、情况或团队都具备这种条件。一些机器学习查询可能非常长，并容易超载集群。通常需要从非常不同的表中汇总数据，回溯多年的数据，执行时间点子句等。因此，将所有计算都推送到集群并不是最好的选择。在某些情况下，将计算隔离到进程实例的内存中可能更安全。这是没有风险的，因为你不会影响集群，从而可能会破坏或延迟业务关键查询。这对于 Spark 用户来说是显而易见的，因为所有计算和数据都分布在执行器上，因为他们需要的大规模。但如果你在使用 Redshift 或 BigQuery 集群时，总是要注意可以将多少计算委托给它们。

## 跟踪输出

机器学习 ETL 生成不同类型的输出工件。一些是 HDFS 中的 Parquet 文件，S3 中的 CSV 文件，数据仓库中的表，映射文件，报告等。这些文件可以用于训练模型、丰富生产数据、在线获取特征等多个选项。

这非常有帮助，因为你可以使用工件的标识符将数据集构建作业与训练作业链接。例如，当使用 [Neptune track_files()](https://docs.neptune.ai/logging/datasets/) 方法时，你可以跟踪这些类型的文件。这里有一个非常清晰的例子 [here](https://docs.neptune.ai/tutorials/data_versioning/#add-tracking-of-dataset-version) 你可以使用。

## 实现自动回填

想象一下，你有一个每日 ETL，它获取前一天的数据以计算用于训练模型的特征。如果由于任何原因你的 ETL 一天未能运行，第二天它运行时，你将丢失前一天计算的数据。

为了解决这个问题，最好查看目标表或文件中最后注册的时间戳。然后，ETL 可以对那些滞后的两天执行。

## 开发松散耦合的组件

代码非常容易改变，而依赖数据的过程更是如此。构建表的事件可能会演变，列可能会改变，大小可能会增加等。当你的 ETL 依赖于不同的数据源时，将它们在代码中隔离总是好的。这是因为如果你需要将两个组件分开作为两个不同的任务（例如：一个需要更大的实例类型来处理，因为数据增加了），如果代码不是混乱的，将更容易做到。

## 使你的 ETL 幂等

在源表或过程本身出现问题的情况下，通常会多次运行相同的过程。为了避免生成重复的数据输出或半填充的表格，ETL 应该是幂等的。也就是说，如果你不小心用相同的条件运行相同的 ETL 两次，第一次运行的输出或副作用不应受到影响（[ref](https://www.startdataengineering.com/post/why-how-idempotent-data-pipeline/)）。你可以通过应用**删除-写入**模式来确保这一点，管道会先删除现有数据，然后再写入新数据。

## 使你的 ETL 代码简洁

我总是喜欢将实际实现代码与业务/逻辑层进行明确的分离。当我构建 ETL 时，第一层应被视为一系列步骤（函数或方法），明确说明数据发生了什么。拥有多个抽象层并不坏。如果你需要维护 ETL 多年，这将非常有帮助。

总是将高层次和低层次的函数彼此隔离。发现类似的情况非常奇怪：

```py
from config import CONVERSION_FACTORS

def transform_data(data: pd.DataFrame) -> pd.DataFrame:
    data = remove_duplicates(data=data)
    data = encode_categorical_columns(data=data)
    data["price_dollars"] = data["price_euros"] * CONVERSION_FACTORS["dollar-euro"]
    data["price_pounds"] = data["price_euros"] * CONVERSION_FACTORS["pound-euro"]
    return data
```

在上面的示例中，我们使用了高层次的函数，如“remove_duplicates”和“encode_categorical_columns”，但同时我们明确展示了一个实现操作，用于通过转换因子转换价格。将这两行代码移除并用一个“convert_prices”函数替换，会不会更好？

```py
from config import CONVERSION_FACTOR

def transform_data(data: pd.DataFrame) -> pd.DataFrame:
    data = remove_duplicates(data=data)
    data = encode_categorical_columns(data=data)
    data = convert_prices(data=data)
    return data
```

在这个例子中，可读性不是问题，但假设你将一个长达 5 行的 groupby 操作嵌入到“transform_data”中，与“remove_duplicates”和“encode_categorical_columns”一起。在这两种情况下，你都混合了高层次和低层次的函数。强烈建议保持一致的分层代码。有时候，为了保持函数或模块 100% 一致性分层，是不可避免且过度工程的，但这是一个非常有益的目标。

## 使用纯函数

不要让副作用或全局状态使你的 ETL 变得复杂。纯函数如果传入相同的参数，会返回相同的结果。

❌下面的函数不是**纯函数**。你正在传递一个与另一个从外部源读取的函数连接的 dataframe。这意味着表格可能会发生变化，因此，每次函数被调用时，返回的 dataframe 可能会不同，即使使用相同的参数。

```py
def transform_data(data: pd.DataFrame) -> pd.DataFrame:
    reference_data = read_reference_data(table="public.references")
    data = data.join(reference_data, on="ref_id")
    return data
```

要使这个函数成为纯函数，你需要执行以下操作：

```py
def transform_data(data: pd.DataFrame, reference_data: pd.DataFrame) -> pd.DataFrame:
    data = data.join(reference_data, on="ref_id")
    return data
```

现在，当传递相同的“data”和“reference_data”参数时，函数将产生相同的结果。

这是一个简单的例子，但我们都见过更糟的情况。依赖全局状态变量的函数、根据某些条件更改类属性状态的方法，可能会改变 ETL 中其他即将出现的方法的行为，等等。

最大限度地使用纯函数可以实现更具功能性的 ETL。正如我们在前面的几点中已经讨论的，它带来了巨大的好处。

## 尽可能多地参数化

ETL会发生变化。这是我们必须接受的事实。源表定义、业务规则、期望结果、实验的精细化、ML模型对更复杂特征的需求等都会发生变化。

为了在我们的ETL中拥有一定的灵活性，我们需要深入评估在哪里投入大部分精力来提供参数化执行。参数化是一种特性，通过简单的接口只需更改*参数*即可改变过程的行为。该接口可以是YAML文件、类初始化方法、函数参数甚至CLI参数。

一种简单直接的参数化方式是定义ETL的“环境”或“阶段”。在将ETL投入生产之前，最好拥有一个“测试”、“集成”或“开发”隔离环境，以便我们可以测试我们的ETL。该环境可能涉及不同的隔离级别，可以从执行基础设施（开发实例与生产实例隔离）、对象存储、数据仓库、数据源等开始。

这是一个明显的参数，可能是最重要的。但我们还可以将参数化扩展到与业务相关的参数。我们可以参数化运行ETL的窗口日期、可能更改或细化的列名、数据类型、过滤值等。

## 适量记录日志

这是ETL中最被低估的属性之一。日志对于检测生产执行异常或隐性错误、解释数据集非常有用。记录提取数据的属性总是很有用。除了代码中的验证以确保不同的ETL步骤成功运行，我们还可以记录：

+   来源表、API或目标路径的引用（例如：“从`item_clicks`表中获取数据”）

+   预期模式的变化（例如：“`promotion` 表中有一个新列”）

+   获取的行数（例如：“从`item_clicks`表中获取145234093行”）

+   关键列中的空值数量（例如：“在Source列中发现125个空值”）

+   数据的简单统计（例如：均值、标准差等）（例如：“CTR均值：0.13，CTR标准差：0.40”）

+   类别列的唯一值（例如：“Country列包含：‘Spain’，‘France’和‘Italy’”）

+   去重的行数（例如：“移除1400行重复数据”）

+   计算密集型操作的执行时间（例如：“聚合耗时560秒”）

+   ETL不同阶段的完成检查点（例如：“丰富过程成功完成”）

**[](https://www.linkedin.com/in/manuelmart%C3%ADn11/)**[Manuel Martín](https://www.linkedin.com/in/manuelmart%C3%ADn11/)**** 是一位拥有超过6年数据科学经验的工程经理。他曾担任数据科学家和机器学习工程师，现在负责Busuu的ML/AI实践。

### 更多相关内容

+   [使用 Jupysql 和 GitHub Actions 调度和运行 ETL](https://www.kdnuggets.com/2023/05/schedule-run-etls-jupysql-github-actions.html)

+   [MLOps：最佳实践及其应用](https://www.kdnuggets.com/2022/04/mlops-best-practices-apply.html)

+   [创建特定领域 AI 模型的最佳实践](https://www.kdnuggets.com/2022/07/best-practices-creating-domainspecific-ai-models.html)

+   [将 ChatGPT 集成到数据科学工作流中：技巧和最佳实践](https://www.kdnuggets.com/2023/05/integrating-chatgpt-data-science-workflows-tips-best-practices.html)

+   [数据仓库和 ETL 的最佳实践](https://www.kdnuggets.com/2023/02/data-warehousing-etl-best-practices.html)

+   [AWS 云及数据迁移的 11 个最佳实践](https://www.kdnuggets.com/2023/04/11-best-practices-cloud-data-migration-aws-cloud.html)
