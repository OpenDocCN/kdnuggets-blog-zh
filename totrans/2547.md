# 使用Snowflake和Dask构建机器学习管道

> 原文：[https://www.kdnuggets.com/2021/07/building-machine-learning-pipelines-snowflake-dask.html](https://www.kdnuggets.com/2021/07/building-machine-learning-pipelines-snowflake-dask.html)

[评论](#comments)

**作者：[Daniel Foley](https://www.linkedin.com/in/daniel-foley-1ab904a2/)，数据科学家**

![Image](../Images/9a765e838d415cad554c4eb10c45ec3e.png)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT

* * *

## 介绍

最近，我一直在尝试找到更好的方法来改善作为数据科学家的工作流程。我发现自己在工作中花了相当多的时间进行建模和构建ETL。这意味着我越来越需要依赖工具来可靠且高效地处理大型数据集。我很快意识到，使用pandas来操作这些数据集并不总是一个好方法，这促使我寻找其他的替代方案。

在这篇文章中，我想分享一些我最近探索的工具，并展示我如何使用它们以及它们如何帮助提高工作效率。我特别要谈的是Snowflake和Dask。这两者是非常不同的工具，但它们在机器学习生命周期中相互补充。我的希望是，在阅读这篇文章后，你将对Snowflake和Dask有一个良好的理解，知道如何有效使用它们，并能够快速上手自己的用例。

更具体地说，我想展示如何使用Snowflake和Python构建ETL管道，为机器学习任务生成训练数据。接着，我会介绍Dask和[Saturn Cloud](https://www.saturncloud.io/s/?utm_source=daniel-foley)，并展示如何利用云中的并行处理来真正加速机器学习训练过程，从而提升你作为数据科学家的生产力。

## 在Snowflake和Python中构建ETL

在开始编码之前，我最好简要解释一下 Snowflake 是什么。这是我最近在团队决定开始使用它时问的一个问题。从高层次来看，它是一个云中的数据仓库。在玩了一段时间后，我意识到它的强大功能。我认为对我来说，最有用的功能之一是你可以使用的虚拟仓库。虚拟仓库使你可以访问相同的数据，但与其他虚拟仓库完全独立，因此计算资源不会在团队之间共享。这被证明非常有用，因为它消除了由于其他用户在一天中执行查询而引起的性能问题的潜在可能。这减少了因查询运行而产生的挫败感和等待时间。

由于我们将使用 Snowflake，我将简要概述如何设置它并开始自己进行实验。我们需要做以下几件事：

+   *设置一个 Snowflake 账户*

+   *将我们的数据导入 Snowflake*

+   *使用 SQL 和 Snowflake UI 编写并测试我们的查询*

+   *编写一个 Python 类，以执行我们的查询来生成最终的数据集用于建模*

设置账户就像在他们的 [网站](http://.com/?_ga=2.18641021.553593417.1621861037-941570141.1610013295) 上注册一个免费试用一样简单。完成后，你可以从 [这里](https://docs.snowflake.com/en/user-guide/snowsql-install-config.html#installing-snowsql-on-macos-using-the-installer) 下载 snowsql CLI。这将使将数据添加到 Snowflake 变得简单。在完成这些步骤后，我们可以尝试使用我们的凭据和命令行连接到 Snowflake。

```py
snowsql -a <account_name> -u <user_name>
```

当你登录 Snowflake UI 时，你可以在 URL 中找到你的账户名。它应该类似于这样：xxxxx.europe-west2.gcp。好了，让我们进入下一步，把数据导入 Snowflake。我们需要遵循以下几个步骤：

+   *创建我们的虚拟仓库*

+   *创建一个数据库*

+   *定义和创建我们的表*

+   *为我们的 CSV 文件创建一个临时表*

+   *将数据复制到我们的表中*

幸运的是，这并不难，我们可以完全使用 snowsql CLI 来完成。对于这个项目，我将使用一个比我希望的小的数据集，但不幸的是，我不能使用公司中的任何数据，而且在网上找到大规模适合的数据集也相当困难。不过，我确实找到了来自 Dunnhumby 的一些交易数据，这些数据可以在 [Kaggle](https://www.kaggle.com/frtgnn/dunnhumby-the-complete-journey) 上免费获取。为了好玩，我使用这些数据创建了一个更大的合成数据集，以测试 Dask 相比于 sklearn 处理挑战的效果。

首先，我们需要在 Snowflake UI 中使用以下命令设置一个虚拟仓库和一个数据库。

**create** **or** **replace** **warehouse** analytics_wh **with**

warehouse_size=”X-SMALL”

auto_suspend=180

auto_resume=true

initially_suspended=true;

**create** **or** **replace** **database** dunnhumby;

我们的数据由 6 个 CSV 文件组成，我们将其转换为 6 个表格。我不会花太多时间讲解数据集，因为这篇文章更多的是关于使用 Snowflake 和 Dask，而不是解释数据。

以下是我们可以用来创建表格的命令。你需要提前了解的只是你将处理的列和数据类型。

```py
**create** **or** **replace** **table** campaign_desc ( 
description **string**, 
campaign number,
start_day number,
end_day number );

**create** **or** **replace** **table** campaign_table ( 
description **string**, 
Household_key number, 
campaign number );

**create** **or** **replace** **table** coupon ( 
COUPON_UPC number, 
product_id number, 
campaign number );

**create** **or** **replace** **table** coupon_redempt ( 
household_key number, 
**day** number, 
coupon_upc number, 
campaign number );

**create** **or** **replace** **table** transactions ( 
household_key number, 
BASKET_ID number, 
**day** number, 
product_id number, 
quantity number, 
sales_value number, 
store_id number, 
retail_disc decimal, 
trans_time number, 
week_no number, 
coupon_disc decimal, 
coupon_match_disc decimal );

**create** **or** **replace** **table** demographic_data ( 
age_dec **string**, 
marital_status_code **string**, 
income_desc **string**, 
homeowner_desc **string**, 
hh_comp_desc **string**, 
household_size_desc string, 
kid_category_desc **string**, 
Household_key number);
```

现在我们已经创建了表格，可以开始考虑如何将数据导入这些表格。为此，我们需要对 CSV 文件进行分阶段处理。这基本上只是一个中间步骤，以便 Snowflake 可以直接从我们的阶段加载文件到表格中。我们可以使用**PUT**命令将本地文件放入阶段，然后使用**COPY INTO**命令指示 Snowflake 将数据放置到哪里。

```py
use database dunnhumby;

**create** **or** **replace** stage dunnhumby_stage;

PUT file://campaigns_table.csv @dunnhumby.public.dunnhumby_stage;

PUT file://campaigns_desc.csv @dunnhumby.public.dunnhumby_stage;

PUT file://coupon.csv @dunnhumby.public.dunnhumby_stage;

PUT file://coupon_d=redempt.csv @dunnhumby.public.dunnhumby_stage; 
PUT file://transaction_data.csv @dunnhumby.public.dunnhumby_stage; 
PUT file://demographics.csv @dunnhumby.public.dunnhumby_stage;
```

作为快速检查，你可以运行这个命令来检查阶段区域中的内容。

```py
ls @dunnhumby.public.dunnhumby_stage;
```

现在我们只需使用下面的查询将数据复制到我们的表格中。你可以在 Snowflake UI 或登录 Snowflake 后在命令行中执行这些查询。

```py
copy into campaign_table 
from @dunnhumby.public.dunnhumby_stage/campaigns_table.csv.gz 
file_format = ( type = csv
skip_header=1 
error_on_column_count_mismatch = false 
field_optionally_enclosed_by=’”’);

copy into campaign_desc 
from @dunnhumby.public.dunnhumby_stage/campaign_desc.csv.gz 
file_format = ( type = csv
skip_header=1 
error_on_column_count_mismatch = false 
field_optionally_enclosed_by=’”’);

copy into coupon 
from @dunnhumby.public.dunnhumby_stage/coupon.csv.gz 
file_format = ( type = csv
skip_header=1 
error_on_column_count_mismatch = false 
field_optionally_enclosed_by=’”’);

copy into coupon_redempt 
from @dunnhumby.public.dunnhumby_stage/coupon_redempt.csv.gz 
file_format = ( type = csv
skip_header=1 
error_on_column_count_mismatch = false 
field_optionally_enclosed_by=’”’);

copy into transactions 
from @dunnhumby.public.dunnhumby_stage/transaction_data.csv.gz 
file_format = ( type = csv
skip_header=1 
error_on_column_count_mismatch = false 
field_optionally_enclosed_by=’”’);

copy into demographic_data 
from @dunnhumby.public.dunnhumby_stage/demographics.csv.gz 
file_format = ( type = csv skip_header=1 
error_on_column_count_mismatch = false 
field_optionally_enclosed_by=’”’);
```

好的，如果运气好的话，我们第一次尝试时数据就会在表格中。哦，真希望这么简单，这整个过程我尝试了几次才搞对（注意拼写错误）。希望你能跟上这些步骤，并顺利完成。我们离有趣的部分越来越近，但上述步骤是过程中的关键部分，所以一定要理解每一步。

### 用 SQL 编写我们的管道

在下一步中，我们将编写查询以生成我们的目标、特征，最后产生一个训练数据集。创建建模数据集的一种方法是将数据读入内存，使用 pandas 创建新特征并将所有数据框连接在一起。这通常是在 Kaggle 和其他在线教程中看到的方法。这样做的问题是效率不是很高，特别是当你处理任何合理大小的数据集时。因此，最好将繁重的工作外包给像 Snowflake 这样的工具，它非常擅长处理大规模数据集，并且可能会节省大量时间。我不会花太多时间深入探讨我们的数据集，因为这并不真正影响我想展示的内容。总的来说，你应该花相当多的时间来探索和理解你的数据，然后再开始建模。这些查询的目标是对数据进行预处理并创建一些简单的特征，我们可以在模型中使用这些特征。

### 目标定义

显然，监督机器学习的一个关键组成部分是定义一个合适的目标进行预测。对于我们的使用案例，我们将通过计算用户在截止周后两周内是否再次访问来预测流失。选择两周是相当随意的，具体取决于我们试图解决的具体问题，但我们就假设这个项目中这样做是合适的。一般来说，你会想要仔细分析你的客户，以了解访问之间的间隔分布，从而得出一个合适的流失定义。

这里的主要思想是，对于每个表，我们希望每个 household_key 具有每个特征的值的一行。

### 活动特征

### 交易特征

下面我们基于汇总统计信息（如平均值、最大值和标准差）创建一些简单的指标。

### 人口统计特征

这个数据集有很多缺失数据，所以我决定在这里使用插补。对于缺失数据，有很多技术，从丢弃缺失数据到高级插补方法。我在这里让自己简化了操作，用众数替换缺失值。我不会普遍推荐这种方法，因为理解数据缺失的原因对于决定如何处理它非常重要，但为了这个例子的目的，我会继续采用简单的方法。我们首先计算每个特征的众数，然后使用 coalesce 来替换缺失的数据。

### 训练数据

最后，我们通过将主要表连接起来，构建一个用于训练数据的查询，并最终得到一个包含我们的目标、我们的活动、交易和人口统计特征的表，我们可以用来构建模型。

顺便提一下，对于那些有兴趣了解 Snowflake 的更多功能和细节的人，我推荐以下书籍：[**Snowflake Cookbook**](https://www.amazon.co.uk/gp/product/1800560613/ref=as_li_tl?ie=UTF8&camp=1634&creative=6738&creativeASIN=1800560613&linkCode=as2&tag=mediumdanny05-21&linkId=b016babb9c48e7f068b4a6bfa70c403c)。我开始阅读这本书，它包含了如何使用 Snowflake 的非常有用的信息，并且详细程度远超我在这里所述的。

### Python 代码用于 ETL

对于这个 ETL，最终需要写一个脚本来执行它。现在，如果你打算定期运行这样的 ETL，这确实是必要的，但这是一种良好的实践，并且使得在需要时运行 ETL 更加容易。

简要讨论一下我们 EtlTraining 类的主要组件。我们的类接受一个输入，即截止周。这是由于数据在数据集中被定义的方式，但通常，这将是一个与我们选择的生成训练数据的截止日期相对应的日期格式。

我们初始化了一个查询列表，以便可以轻松地循环遍历这些查询并执行它们。我们还创建了一个包含我们参数的字典，并将其传递给我们的Snowflake连接。在这里，我们使用了在Saturn Cloud中设置的环境变量。[这里](https://saturncloud.io/docs/using-saturn-cloud/credentials/)是关于如何做到这一点的指南。连接Snowflake并不太困难，我们只需要使用Snowflake连接器并传入我们的凭据字典即可。我们在Snowflake连接方法中实现了这一点，并将此连接作为属性返回。

为了使这些查询更容易运行，我将每个查询保存为`python`字符串变量在ml_query_pipeline.py文件中。execute_etl方法正如其名，我们循环遍历每个查询，对其进行格式化，执行它，并最后关闭Snowflake连接。

要运行这个ETL，我们可以简单地在终端中输入以下命令。（其中ml_pipeline是上面脚本的名称。）

```py
python -m ml_pipeline -w 102 -j ‘train’
```

简单来说，你可能希望定期运行像这样的ETL。例如，如果你想进行每日预测，那么你将需要每天生成一个这样的数据集以传递给你的模型，从而识别哪些客户可能会流失。我不会在这里详细讲解，但在我的工作中，我们使用Airflow来编排我们的ETL，因此如果你感兴趣，我建议你去了解一下。实际上，我最近买了一本书‘[Data Pipelines with Apache Airflow](https://www.amazon.co.uk/gp/product/1617296902/ref=as_li_tl?ie=UTF8&camp=1634&creative=6738&creativeASIN=1617296902&linkCode=as2&tag=mediumdanny05-21&linkId=1ad3a1bf79e65482860570c3a484a73c)’，我认为它非常棒，提供了一些很好的示例和关于如何使用Airflow的建议。

## Dask和建模

现在我们已经构建了数据管道，我们可以开始考虑建模。我这篇文章的另一个主要目标是突出使用**Dask**作为机器学习开发过程的一部分的优势，并向大家展示它的易用性。

在这个项目的部分，我还使用了[Saturn Cloud](https://www.saturncloud.io/s/?utm_source=daniel-foley)，这是我最近遇到的一个非常好的工具，它允许我们在云中通过计算机集群利用Dask的力量。对我来说，使用Saturn的主要优势是非常容易共享你的工作、在需要时简单地扩展计算资源，并且它有一个免费的选项。模型开发通常是Dask的一个很好的应用场景，因为我们通常想要训练一组不同的模型，看看哪个效果最好。我们能越快做到这一点越好，因为我们可以有更多时间专注于模型开发的其他重要方面。类似于Snowflake，你只需要[在这里](https://www.saturncloud.io/s/?utm_source=daniel-foley)注册，你可以非常快速地启动一个Jupyter lab实例并开始自己动手实验。

现在，我意识到我在这里提到 Dask 几次，但从未真正解释过它是什么。所以让我花点时间给你一个关于 Dask 的高层次概述，以及为什么我认为它很棒。简单来说，Dask 是一个 Python 库，利用并行计算来处理和执行非常大的数据集上的操作。而且，最棒的是，如果你已经熟悉 Python，那么 Dask 应该非常直接，因为其语法非常相似。

下图突出显示了 Dask 的主要组件。

![](../Images/1a682f60d9ac33bfbfc70cbb981a4bf0.png)

来源: [Dask 文档](https://docs.dask.org/en/latest/)

Collections 允许我们创建一个任务图，这些任务图可以在多个计算机上执行。这些数据结构中有些可能听起来很熟悉，比如数组和数据框，它们类似于你在 Python 中会遇到的，但有一些重要的不同之处。例如，你可以把 Dask 数据框看作是一个由 pandas 数据框组成的集合，这些数据框以一种可以让我们并行执行操作的方式构建。

从 collections 说到调度器。一旦我们创建了任务图，调度器就会处理剩下的工作。它管理工作流程，并将这些任务发送到单台机器或分布到集群中。希望这能给你一个关于 Dask 工作原理的简要概述。欲了解更多信息，我建议你查看 [文档](https://docs.dask.org/en/latest/) 或这本 [书](https://www.amazon.co.uk/gp/product/1617295604/ref=as_li_tl?ie=UTF8&camp=1634&creative=6738&creativeASIN=1617295604&linkCode=as2&tag=mediumdanny05-21&linkId=fbcbf83b35fe6ce3c909ba9ece7001af)。这两者都是深入了解该主题的非常好资源。

### Python 建模代码

在建模时，我倾向于使用少量的常用算法，这些算法是我总是会首先尝试的。这通常会让我对可能适合我具体问题的模型有一个很好的了解。这些模型包括 Logistic Regression、Random Forest 和 GradientBoosting。在我的经验中，处理表格数据时，这些算法通常会给出相当不错的结果。下面我们使用这三种模型构建一个 sklearn 建模管道。我们在这里使用的具体模型并不是特别重要，因为该管道应该适用于任何 sklearn 分类模型，这只是我的偏好。

不再废话，让我们直接进入代码。幸运的是，我们将大部分预处理工作外包给了 Snowflake，因此在这里我们不需要过多处理训练数据，但我们将使用 sklearn 管道添加一些额外的步骤。

下方的第一个代码片段展示了使用 sklearn 的管道。注意我们的数据集是一个普通的 pandas 数据框，我们的预处理步骤都是通过 sklearn 方法完成的。这里没有特别不同的地方。我们从 Snowflake ETL 生成的表中读取数据，并将其传递到 sklearn 管道中。这里应用了常规的建模步骤。我们将数据集拆分为训练集和测试集，并进行一些预处理，即使用中位数填补缺失值，缩放数据并对分类数据进行独热编码。我非常喜欢 sklearn 管道，并且在开发模型时基本上都会使用它们，它们确实有助于编写干净简洁的代码。

这个管道在一个大约有 200 万行的数据集上的表现如何？好吧，不进行任何超参数调优的情况下运行这个模型大约需要 34 分钟。哎，有点慢。如果我们想进行任何类型的超参数调优，你可以想象这将花费多么漫长的时间。好的，所以并不理想，但让我们看看 Dask 如何应对这个挑战。

### Dask ML Python 代码

我们的目标是看看是否可以超越上述 sklearn 管道，剧透一下，我们绝对可以。Dask 的酷炫之处在于，当你已经熟悉 Python 时，上手的门槛相对较低。我们只需进行几处更改，就可以在 Dask 中启动并运行这个管道。

你可能会注意到的第一个变化是我们有一些不同的导入。这条管道与之前的主要区别之一是我们将使用 Dask 数据框而不是 pandas 数据框来训练我们的模型。你可以把 Dask 数据框想象成一堆 pandas 数据框，我们可以同时在每一个上执行计算。这是 Dask 并行性的核心，也是减少这个管道训练时间的关键所在。

注意我们使用 ***@dask.delayed*** 作为装饰器来装饰我们的 ***load_training_data*** 函数。这指示 Dask 为我们并行化这个函数。

我们还将从 Dask 导入一些预处理和管道方法，更重要的是，我们需要导入 **SaturnCluster**，它将允许我们创建一个集群来训练我们的模型。另一个关键的不同点是，在我们的训练测试拆分之后，我们使用了 ***dask.persist***。在此之前，由于 Dask 的延迟评估，我们的函数实际上并没有被计算。 一旦我们使用 persist 方法，我们就在告诉 Dask 将数据发送到工作节点，执行我们到目前为止创建的任务，并将这些对象保留在集群上。

最后，我们使用延迟方法训练我们的模型。这样，我们能够以懒惰的方式创建管道。管道不会被执行，直到我们到达这段代码：

```py
fit_pipelines = dask.compute(*pipelines_)
```

这次我们只花了大约 `10 minutes` 就在完全相同的数据集上运行了这个管道。这是提高了 3.4 倍的速度，表现不错。如果我们愿意的话，我们还可以通过在 Saturn 中一键扩展计算资源进一步加速。

## 部署我们的管道

我之前提到过，你可能会想要定期运行这样的管道，使用类似 airflow 的工具。恰好的是，如果你不想经历设置 airflow 的初始麻烦，Saturn Cloud 提供了一个简单的替代方案，即 Jobs。Jobs 允许我们打包代码，并按需或在固定间隔内运行。你只需进入现有项目并点击创建作业。一旦我们这样做，它应该会像以下这样：

![](../Images/8f159fecc5ad6c10629cf007cf4c029d.png)

来源: [Saturn](https://saturncloud.io/docs/using-saturn-cloud/jobs_and_deployments/)

从这里开始，我们需要确保上面的 Python 文件在图像中的目录中，然后可以输入上面的 Python 命令。

```py
python -m ml_pipeline -w 102 -j 'train'
```

如果需要，我们还可以使用 cron 语法设置日常 ETL 任务。对那些感兴趣的人，这里有一个 [教程](https://saturncloud.io/docs/using-saturn-cloud/jobs_and_deployments/) 详细讲解所有细节。

## 结论和收获

好了，我们现在已经到了项目的最后阶段。显然，我省略了一些 ML 开发周期的关键部分，如超参数调优和模型部署，但也许我会留到另一天。我认为你应该尝试 Dask 吗？我并不是专家，但从我目前看到的情况来看，它确实非常有用，我非常期待进一步实验，并寻找更多将其融入我作为数据科学家的日常工作的机会。希望你觉得这有用，并且你也能看到 Snowflake 和 Dask 的一些优点，开始自己动手尝试。

### 资源

+   [使用 Apache Airflow 的数据管道](https://www.amazon.co.uk/gp/product/1617296902/ref=as_li_tl?ie=UTF8&camp=1634&creative=6738&creativeASIN=1617296902&linkCode=as2&tag=mediumdanny05-21&linkId=1ad3a1bf79e65482860570c3a484a73c)

+   [Snowflake 食谱](https://www.amazon.co.uk/gp/product/1800560613/ref=as_li_tl?ie=UTF8&camp=1634&creative=6738&creativeASIN=1800560613&linkCode=as2&tag=mediumdanny05-21&linkId=b016babb9c48e7f068b4a6bfa70c403c)

+   [使用 Python 和 Dask 进行大规模数据科学](https://www.amazon.co.uk/gp/product/1617295604/ref=as_li_tl?ie=UTF8&camp=1634&creative=6738&creativeASIN=1617295604&linkCode=as2&tag=mediumdanny05-21&linkId=fbcbf83b35fe6ce3c909ba9ece7001af)

+   [Coursera: 数据科学中的 SQL](https://click.linksynergy.com/deeplink?id=z2stMJEP3T4&mid=40328&murl=https%3A%2F%2Fwww.coursera.org%2Fspecializations%2Flearn-sql-basics-data-science%23courses)

### 你可能会发现我其他的一些文章很有趣

[**让我们构建一个流式数据管道**](https://towardsdatascience.com/lets-build-a-streaming-data-pipeline-e873d671fc57)

[**高斯混合模型 (GMM)**](https://towardsdatascience.com/gaussian-mixture-modelling-gmm-833c88587c7f)

[**一种贝叶斯时间序列预测方法**](https://towardsdatascience.com/a-bayesian-approach-to-time-series-forecasting-d97dd4168cb7)

*注意：本文中的部分链接为附属链接。*

**个人简介： [丹尼尔·福伊](https://www.linkedin.com/in/daniel-foley-1ab904a2/)** 是一位曾经的经济学家，现转行成为从事移动游戏行业的数据科学家。

[原文](https://towardsdatascience.com/building-machine-learning-pipelines-using-snowflake-and-dask-10ae5e7fff0f)。经授权转载。

**相关内容：**

+   [BigQuery 与 Snowflake：数据仓库巨头的比较](/2021/06/bigquery-snowflake-comparison-data-warehouse-giants.html)

+   [Pandas 不够用？这里有几个处理更大更快数据的 Python 备选方案](/2021/07/pandas-alternatives-processing-larger-faster-data-python.html)

+   [你还在用 Pandas 处理 2021 年的大数据吗？这里有两个更好的选择](/2021/03/pandas-big-data-better-options.html)

### 更多相关话题

+   [天空才是极限：了解 JetBlue 如何利用 Monte Carlo 和 Snowflake…](https://www.kdnuggets.com/2022/12/monte-carlo-jetblue-snowflake-build-trust-improve-model-accuracy.html)

+   [使用 Pandas 构建数据科学管道](https://www.kdnuggets.com/building-data-science-pipelines-using-pandas)

+   [初学者的 Snowflake 数据仓库](https://www.kdnuggets.com/2022/02/data-warehousing-snowflake-beginners.html)

+   [提高在 Snowflake 上工作效率的 6 大工具](https://www.kdnuggets.com/2023/08/top-6-tools-improve-productivity-snowflake.html)

+   [如何在 Snowflake 上构建流式半结构化分析平台](https://www.kdnuggets.com/2023/07/build-streaming-semistructured-analytics-platform-snowflake.html)

+   [构建数据管道以创建大型语言模型应用](https://www.kdnuggets.com/building-data-pipelines-to-create-apps-with-large-language-models)
