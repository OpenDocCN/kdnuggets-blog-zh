# 使用 Pandera 进行 PySpark 应用的数据验证

> 原文：[`www.kdnuggets.com/2023/08/data-validation-pyspark-applications-pandera.html`](https://www.kdnuggets.com/2023/08/data-validation-pyspark-applications-pandera.html)

![使用 Pandera 进行 PySpark 应用的数据验证](img/dcef72aa649e62b1541405e7fd23c7e0.png)

照片由 [Jakub Skafiriak](https://unsplash.com/@jakubskafiriak?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/AljDaiCbCVY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如果你是数据从业者，你会认识到数据验证在确保准确性和一致性方面的重要性。这在处理大型数据集或来自不同来源的数据时尤为关键。然而，[Pandera](https://pandera.readthedocs.io/en/stable/) Python 库可以帮助简化和自动化数据验证过程。Pandera 是一个 [开源库](https://github.com/unionai-oss/pandera)，精心设计以简化模式和数据验证任务。它在 pandas 的稳健性和多功能性基础上构建，并引入了一个直观且富有表现力的 API，专门用于数据验证目的。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

本文简要介绍了 Pandera 的关键特性，然后深入说明了如何将 Pandera 数据验证集成到使用本地 PySpark SQL 的数据处理工作流中，自最新版本 [(Pandera 0.16.0](https://github.com/unionai-oss/pandera/releases/tag/v0.16.0)) 起。

Pandera 设计用于与其他流行的 Python 库如 pandas、pyspark.pandas、Dask 等一起工作。这使得将数据验证融入现有的数据处理工作流程变得容易。直到最近，Pandera 还缺乏对[PySpark SQL](https://spark.apache.org/docs/latest/api/python/)的原生支持，但为了弥补这一空白，[QuantumBlack, AI by McKinsey](https://www.mckinsey.com/capabilities/quantumblack/how-we-help-clients)的团队包括[Ismail Negm-PARI](https://github.com/mkinegm)、[Neeraj Malhotra](https://github.com/NeerajMalhotra-QB)、[Jaskaran Singh Sidana](https://github.com/jaskaransinghsidana)、[Kasper Janehag](https://github.com/kasperjanehag)、[Oleksandr Lazarchuk](https://github.com/oleksandr-lazarchuk)及 Pandera 创始人[ Niels Bantilan](https://github.com/cosmicBboy)开发了对 PySpark SQL 的原生支持，并将其贡献给了 Pandera。本文的文字也是由该团队准备的，以下是他们的描述。

# Pandera 的关键特性

如果你不熟悉使用 Pandera 验证数据，我们建议查看[Khuyen Tran](https://khuyentran1476.medium.com/)的“[使用 Pandera 验证你的 pandas DataFrame](https://towardsdatascience.com/validate-your-pandas-dataframe-with-pandera-2995910e564)”，其中描述了基础知识。简而言之，我们在这里简要说明了简单直观的 API、内置验证函数和自定义的关键特性和优势。

## 简单直观的 API

Pandera 的一个显著特点是其简单直观的 API。你可以使用易于阅读和理解的声明式语法定义数据模式。这使得编写高效且有效的数据验证代码变得简单。

这是 Pandera 中模式定义的一个示例：

```py
class InputSchema(pa.DataFrameModel):
   year: Series[int] = pa.Field()
   month: Series[int] = pa.Field()
   day: Series[int] = pa.Field()
```

## 内置验证函数

Pandera 提供了一组内置函数（更常称为检查）来执行数据验证。当我们在 Pandera 模式上调用 `validate()` 时，它将执行模式和数据验证。数据验证将在后台调用 `check` 函数。

这是一个使用 Pandera 对数据框对象运行数据 `check` 的简单示例。

```py
class InputSchema(pa.DataFrameModel):
   year: Series[int] = pa.Field(gt=2000, coerce=True)
   month: Series[int] = pa.Field(ge=1, le=12, coerce=True)
   day: Series[int] = pa.Field(ge=0, le=365, coerce=True)

InputSchema.validate(df)
```

如上所示，对于 `year` 字段，我们定义了一个检查 `gt=2000`，强制所有此字段的值必须大于 2000，否则 Pandera 将引发验证失败。

这是 Pandera 默认提供的所有内置检查的列表：

```py
eq: checks if value is equal to a given literal
ne: checks if value is not equal to a given literal
gt: checks if value is greater than a given literal
ge: checks if value is greater than & equal to a given literal
lt: checks if value is less than a given literal
le: checks if value is less than & equal to a given literal
in_range: checks if value is given range
isin: checks if value is given list of literals
notin: checks if value is not in given list of literals
str_contains: checks if value contains string literal
str_endswith: checks if value ends with string literal
str_length: checks if value length matches
str_matches: checks if value matches string literal
str_startswith: checks if value starts with a string literal
```

## 自定义验证函数

除了内置的验证检查，Pandera 还允许你定义自定义验证函数。这让你能够根据使用情况定义自己的验证规则。

例如，你可以定义一个用于数据验证的 lambda 函数，如下所示：

```py
schema = pa.DataFrameSchema({
   "column2": pa.Column(str, [
       pa.Check(lambda s: s.str.startswith("value")),
       pa.Check(lambda s: s.str.split("_", expand=True).shape[1] == 2)
   ]),
})
```

# 将对 PySpark SQL 数据框的支持添加到 Pandera 中

在添加对 PySpark SQL 支持的过程中，我们遵循了两个基本原则：

+   接口和用户体验的一致性。

+   PySpark 性能优化。

首先，让我们深入探讨一致性的问题，因为从用户的角度来看，他们需要无论选择什么框架，都能有一套一致的 API 和接口。由于 Pandera 提供了多个框架选择，因此在 PySpark SQL API 中拥有一致的用户体验变得尤为重要。

有了这一点，我们可以使用 PySpark SQL 来定义 Pandera 模式，如下所示：

```py
from pyspark.sql import DataFrame, SparkSession
import pyspark.sql.types as T
import pandera.pyspark as pa

spark = SparkSession.builder.getOrCreate()

class PanderaSchema(DataFrameModel):
       """Test schema"""
       id: T.IntegerType() = Field(gt=5)
       product_name: T.StringType() = Field(str_startswith="B")
       price: T.DecimalType(20, 5) = Field()
       description: T.ArrayType(T.StringType()) = Field()
       meta: T.MapType(T.StringType(), T.StringType()) = Field()

data_fail = [
       (5, "Bread", 44.4, ["description of product"], {"product_category": "dairy"}),
       (15, "Butter", 99.0, ["more details here"], {"product_category": "bakery"}),
   ]

spark_schema = T.StructType(
       [
           T.StructField("id", T.IntegerType(), False),
           T.StructField("product", T.StringType(), False),
           T.StructField("price", T.DecimalType(20, 5), False),
           T.StructField("description", T.ArrayType(T.StringType(), False), False),
           T.StructField(
               "meta", T.MapType(T.StringType(), T.StringType(), False), False
           ),
       ],
   )
df_fail = spark_df(spark, data_fail, spark_schema)
```

在上述代码中，`PanderaSchema`定义了传入的 pyspark 数据框的模式。它有 5 个字段，具有不同的`dtypes`，并对`id`和`product_name`字段进行了数据检查的强制执行。

```py
class PanderaSchema(DataFrameModel):
       """Test schema"""
       id: T.IntegerType() = Field(gt=5)
       product_name: T.StringType() = Field(str_startswith="B")
       price: T.DecimalType(20, 5) = Field()
       description: T.ArrayType(T.StringType()) = Field()
       meta: T.MapType(T.StringType(), T.StringType()) = Field()
```

接下来，我们制作了一个虚拟数据并强制执行` spark_schema`*中定义的本地 PySpark SQL 模式*。

```py
spark_schema = T.StructType(
       [
           T.StructField("id", T.IntegerType(), False),
           T.StructField("product", T.StringType(), False),
           T.StructField("price", T.DecimalType(20, 5), False),
           T.StructField("description", T.ArrayType(T.StringType(), False), False),
           T.StructField(
               "meta", T.MapType(T.StringType(), T.StringType(), False), False
           ),
       ],
   )

df_fail = spark_df(spark, data_fail, spark_schema)
```

这是为了模拟模式和数据验证失败。

这是`df_fail`数据框的内容：

```py
df_fail.show()

   +---+-------+--------+--------------------+--------------------+
   | id|product|   price|         description|                meta|
   +---+-------+--------+--------------------+--------------------+
   |  5|  Bread|44.40000|[description of p...|{product_category...|
   | 15| Butter|99.00000| [more details here]|{product_category...|
   +---+-------+--------+--------------------+--------------------+
```

接下来，我们可以调用 Pandera 的 validate 函数来执行模式和数据级验证，如下所示：

```py
df_out = PanderaSchema.validate(check_obj=df)
```

我们将很快探索`df_out`的内容。

# PySpark 性能优化

我们的贡献专门针对在处理 PySpark 数据框时的最佳性能进行设计，这对于处理大型数据集以应对 PySpark 分布式计算环境的独特挑战至关重要。

Pandera 利用 PySpark 的分布式计算架构来高效处理大型数据集，同时保持数据的一致性和准确性。我们重写了 Pandera 的自定义验证函数以提高 PySpark 性能，使得大型数据集的验证更快、更高效，同时降低了在高负载情况下数据错误和不一致的风险。

## 综合错误报告

我们在 Pandera 中新增了生成详细错误报告的功能，这些报告以 Python 字典对象的形式提供。这些报告可以通过 validate 函数返回的数据框访问，提供了所有模式和数据级验证的全面摘要，依据用户的配置。

这一功能对开发人员来说非常有价值，可以快速识别和解决数据相关问题。通过使用生成的错误报告，团队可以编制应用程序中的模式和数据问题的全面列表，从而高效而精准地优先解决问题。

需要注意的是，这一功能目前仅对 PySpark SQL 可用，为用户在处理 Pandera 中的错误报告时提供了增强的体验。

在上述代码示例中，请记住我们对 spark 数据框调用了`validate()`：

```py
df_out = PanderaSchema.validate(check_obj=df)
```

它返回了一个数据框对象。使用访问器，我们可以提取其中的错误报告，如下所示：

```py
print(df_out.pandera.errors)
```

```py
{
  "SCHEMA":{
     "COLUMN_NOT_IN_DATAFRAME":[
        {
           "schema":"PanderaSchema",
           "column":"PanderaSchema",
           "check":"column_in_dataframe",
           "error":"column 'product_name' not in dataframe Row(id=5, product='Bread', price=None, description=['description of product'], meta={'product_category': 'dairy'})"
        }
     ],
     "WRONG_DATATYPE":[
        {
           "schema":"PanderaSchema",
           "column":"description",
           "check":"dtype('ArrayType(StringType(), True)')",
           "error":"expected column 'description' to have type ArrayType(StringType(), True), got ArrayType(StringType(), False)"
        },
        {
           "schema":"PanderaSchema",
           "column":"meta",
           "check":"dtype('MapType(StringType(), StringType(), True)')",
           "error":"expected column 'meta' to have type MapType(StringType(), StringType(), True), got MapType(StringType(), StringType(), False)"
        }
     ]
  },
  "DATA":{
     "DATAFRAME_CHECK":[
        {
           "schema":"PanderaSchema",
           "column":"id",
           "check":"greater_than(5)",
           "error":"column 'id' with type IntegerType() failed validation greater_than(5)"
        }
     ]
  }
}
```

如上所示，错误报告以两级汇总的形式存储在一个 Python 字典对象中，以便下游应用程序轻松使用，比如使用 Grafana 等工具对错误的时间序列进行可视化：

1.  验证类型 = `SCHEMA` 或 `DATA`

1.  错误类别 = `DATAFRAME_CHECK` 或 `WRONG_DATATYPE` 等。

这种重构错误报告的新格式是在 0.16.0 版本中引入的，作为我们贡献的一部分。

## 开/关开关

对于依赖于 PySpark 的应用程序，开/关开关是一个重要的功能，可以在灵活性和风险管理方面带来显著的差异。具体来说，开/关开关允许团队在生产环境中禁用数据验证，而无需修改代码。

这对大数据管道尤其重要，其中性能至关重要。在许多情况下，数据验证可能会占用大量处理时间，这可能会影响管道的整体性能。使用开/关开关，团队可以在必要时快速轻松地禁用数据验证，而无需经过耗时的代码修改过程。

我们的团队为 Pandera 引入了开/关开关，使用户可以通过简单地更改配置设置，轻松关闭生产环境中的数据验证。这提供了在必要时优先考虑性能的灵活性，同时不牺牲开发中的数据质量或准确性。

要启用验证，请在您的环境变量中设置以下内容：

```py
export PANDERA_VALIDATION_ENABLED=False
```

这将被 Pandera 拦截，以禁用应用程序中的所有验证。默认情况下，验证是启用的。

目前，该功能仅在 0.16.0 版本及更高版本的 PySpark SQL 中可用，因为这是我们贡献的新概念。

## Pandera 执行的细粒度控制

除了开/关开关功能，我们还引入了对 Pandera 验证流程的更细粒度控制。这是通过引入可配置设置实现的，这些设置允许用户在三个不同的层级上控制执行：

1.  `SCHEMA_ONLY`：此设置仅执行模式验证。它检查数据是否符合模式定义，但不执行任何额外的数据级别验证。

1.  `DATA_ONLY`：此设置仅执行数据级别的验证。它检查数据是否符合定义的约束和规则，但不验证模式。

1.  `SCHEMA_AND_DATA`：此设置执行模式和数据级别的验证。它会检查数据是否符合模式定义以及定义的约束和规则。

通过提供这种细粒度的控制，用户可以选择最适合其特定用例的验证级别。例如，如果主要关注的是确保数据符合定义的模式，可以使用 `SCHEMA_ONLY` 设置以减少总体处理时间。或者，如果数据已知符合模式，并且关注点是确保数据质量，则可以使用 `DATA_ONLY` 设置来优先考虑数据级别的验证。

对 Pandera 执行的增强控制允许用户在精确性和效率之间找到微调平衡，从而实现更有针对性和优化的验证体验。

```py
export PANDERA_VALIDATION_DEPTH=SCHEMA_ONLY
```

默认情况下，验证是启用的，深度设置为`SCHEMA_AND_DATA`，可以根据用例需要更改为`SCHEMA_ONLY`或`DATA_ONLY`。

目前，这一功能仅适用于从版本 0.16.0 开始的 PySpark SQL，因为这是我们贡献的新概念。

## 列和数据框级别的元数据

我们的团队为 Pandera 添加了一个新功能，允许用户在`Field`和`Schema / Model`级别存储额外的元数据。此功能旨在让用户在其模式定义中嵌入上下文信息，以供其他应用程序使用。

例如，通过存储关于特定列的详细信息，如数据类型、格式或单位，开发人员可以确保下游应用程序能够正确解释和使用数据。类似地，通过存储关于某个模式中哪些列对于特定用例是必要的信息，开发人员可以优化数据处理管道，降低存储成本，并提高查询性能。

在模式级别，用户可以存储信息以帮助分类整个应用程序中的不同模式。这些元数据可以包括模式的目的、数据来源或数据的日期范围等细节。这对于管理复杂的数据处理工作流尤其有用，其中多个模式用于不同的目的，需要高效地跟踪和管理。

```py
class PanderaSchema(DataFrameModel):
       """Pandera Schema Class"""
       id: T.IntegerType() = Field(
           gt=5,
           metadata={"usecase": ["RetailPricing", "ConsumerBehavior"],
              "category": "product_pricing"},
       )
       product_name: T.StringType() = Field(str_startswith="B")
       price: T.DecimalType(20, 5) = Field()

       class Config:
           """Config of pandera class"""
           name = "product_info"
           strict = True
           coerce = True
           metadata = {"category": "product-details"}
```

在上述示例中，我们引入了有关模式对象本身的附加信息。这允许在两个级别进行：字段和模式。

要提取模式级别的元数据（包括其中所有字段），我们提供了以下帮助函数：

```py
PanderaSchema.get_metadata()
The output will be dictionary object as follows:
{
       "product_info": {
           "columns": {
               "id": {"usecase": ["RetailPricing", "ConsumerBehavior"],
                      "category": "product_pricing"},
               "product_name": None,
               "price": None,
           },
           "dataframe": {"category": "product-details"},
       }
}
```

当前，这一功能在 0.16.0 中是一个新概念，并已添加到 PySpark SQL 和 Pandas 中。

# 摘要

我们引入了几个新功能和概念，包括一个开关，允许团队在生产中禁用验证而无需更改代码，对 Pandera 验证流程的精细控制，以及在列和数据框级别存储额外元数据的能力。您可以在[更新的 Pandera 文档](https://pandera.readthedocs.io/en/stable/pyspark_sql.html)中找到更多关于版本 0.16.0 的详细信息。

正如 Pandera 创始人 Niels Bantilan 在[关于 Pandera 0.16.0 发布的近期博客文章中所解释的：](https://www.union.ai/blog-post/pandera-0-16-going-beyond-pandas-data-validation)

> *为了证明 Pandera 在新的模式规范和后端 API 中的可扩展性，我们与* [*QuantumBlack*](https://www.mckinsey.com/capabilities/quantumblack/how-we-help-clients) *团队合作，实现了一个模式和后端，用于* [*Pyspark SQL*](https://spark.apache.org/docs/3.3.1/api/python/index.html#:~:text=PySpark%20is%20an%20interface%20for,data%20in%20a%20distributed%20environment.) *…并在几个月内完成了一个 MVP！*

最近对 [Pandera](https://github.com/unionai-oss/pandera/pull/1243) 开源代码库的贡献将惠及使用 PySpark 和其他大数据技术的团队。

以下在 [QuantumBlack（麦肯锡的人工智能部门）](https://www.mckinsey.com/capabilities/quantumblack/how-we-help-clients) 的团队成员负责了这次贡献：[Ismail Negm-PARI](https://github.com/mkinegm)、[Neeraj Malhotra](https://github.com/NeerajMalhotra-QB)、[Jaskaran Singh Sidana](https://github.com/jaskaransinghsidana)、[Kasper Janehag](https://github.com/kasperjanehag)、[Oleksandr Lazarchuk](https://github.com/oleksandr-lazarchuk)。特别感谢 Neeraj 在准备这篇文章出版过程中的协助。

**[Jo Stichbury](https://www.linkedin.com/in/jostichbury/)** 是 QuantumBlack（麦肯锡的人工智能部门）的技术作家。他是一名技术内容创作者，撰写关于数据科学和软件的文章。曾是老派 Symbian C++ 开发者，现在是偶然的猫群管理者和鹅追逐者。

### 更多相关话题

+   [PySpark 数据科学](https://www.kdnuggets.com/2023/02/pyspark-data-science.html)

+   [Pydantic 教程：Python 数据验证变得简单](https://www.kdnuggets.com/pydantic-tutorial-data-validation-in-python-made-simple)

+   [MarshMallow：最甜美的 Python 数据序列化和验证库](https://www.kdnuggets.com/marshmallow-the-sweetest-python-library-for-data-serialization-and-validation)

+   [为什么使用 k-fold 交叉验证？](https://www.kdnuggets.com/2022/07/kfold-cross-validation.html)

+   [KDnuggets 新闻，5 月 18 日：5 个免费的机器学习平台](https://www.kdnuggets.com/2022/n20.html)

+   [LangChain 101：构建你自己的 GPT 驱动应用](https://www.kdnuggets.com/2023/04/langchain-101-build-gptpowered-applications.html)
