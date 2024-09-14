# 我如何将100多个ETL重新设计为ELT数据管道

> 原文：[https://www.kdnuggets.com/2021/11/redesigned-over-100-etl-elt-data-pipelines.html](https://www.kdnuggets.com/2021/11/redesigned-over-100-etl-elt-data-pipelines.html)

[评论](#comments)

**作者 [Nicholas Leong](https://www.linkedin.com/in/nickefy/)，数据工程师，作家**

![](../Images/5a64ce73f49a562a81fa5893ec7d42df.png)

作者提供的图片

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 在IT领域支持你的组织

* * *

> 大家：数据工程师做什么？

> 我：我们构建管道。
> 
> 大家：你是说像水管工那样？

有点像，但不是水通过管道流动，而是**数据通过我们的管道流动**。

数据科学家构建模型，数据分析师将数据传达给利益相关者。那么，我们为什么需要数据工程师？

他们几乎不知道，没有数据工程师，模型甚至不会存在。不会有任何数据可以传递。数据工程师构建仓库和管道，使数据能够在组织内流动。我们连接这些点。

[**你应该在2021年成为数据工程师吗？**](https://towardsdatascience.com/should-you-become-a-data-engineer-in-2021-4db57b6cce35)

数据工程师是2019年增长最快的工作，**年增长率为50%**，高于数据科学家**32%的年增长率**。

因此，我在这里为大家介绍一些数据工程师的日常任务。数据管道只是其中之一。

## ETL/ELT管道

> ETL — 提取、转换、加载
> 
> ELT — 提取、加载、转换

这些是什么意思，它们之间有什么不同？

在数据管道的世界里，有一个**源头**和一个**目的地**。最简单的形式是，源头是数据工程师获取数据的地方，而目的地是他们希望将数据加载到的地方。

事实上，数据处理过程中往往需要进行一些**处理**。这可能是由于许多原因，包括但不限于—

+   数据存储类型的差异

+   数据的目的

+   数据治理/质量

数据工程师将数据处理标记为转换。这是他们施展魔法的地方，将各种数据转化为他们希望的形式。

在**ETL 数据管道**中，数据工程师在将数据加载到目标之前执行转换。如果表之间存在关系转换，这些转换发生在源数据库本身。在我的情况下，源是一个 Postgres 数据库。因此，我们在源中执行关系联接以获取所需的数据，然后将其加载到目标中。

在**ELT 数据管道**中，数据工程师将数据原始地加载到目标中。然后，他们在目标本身内执行任何关系转换。

在这篇文章中，我们将讨论我如何将组织中的 100 多个 ETL 管道转换为 ELT 管道，我们还将探讨我这样做的原因。

## 我是怎么做到的

最初，管道是使用 Linux cron 作业运行的。Cron 作业类似于传统的任务调度程序，它们通过 Linux 终端初始化。它们是调度程序的最基本方式，没有任何功能，例如 —

+   设置依赖关系

+   设置动态变量

+   构建连接

![](../Images/97fc8c807073933c0797cd585086ef7c.png)

图片由作者提供

这是首先需要改变的，因为它造成了太多问题。我们需要扩展。为此，我们必须建立一个适当的工作流管理系统。

我们选择了**Apache Airflow**。我在这里详细写了关于它的所有内容。

[**数据工程 — Apache Airflow 基础 — 构建你的第一个管道**](https://towardsdatascience.com/data-engineering-basics-of-apache-airflow-build-your-first-pipeline-eefecb7f1bb9)

Airflow 最初是由**Airbnb**的团队开发的，并开放源代码。它还被像**Twitter**这样的大公司作为它们的管道管理系统使用。你可以阅读上面关于 Airflow 好处的所有内容。

在解决这个问题后，我们必须改变提取数据的方式。团队建议**将我们的 ETL 管道重新设计为 ELT 管道**。稍后会详细讲解我们这样做的原因。

![](../Images/15658012ec0844894f7de86408543162.png)

图片由作者提供

这是在重新设计之前的管道示例。我们处理的源是一个 Postgres 数据库。因此，为了以预期的形式获取数据，我们必须在源数据库中执行联接操作。

```py
Select 
a.user_id,
b.country,
a.revenue
from transactions a 
left join users b on
a.user_id = b.user_id
```

这是在源数据库中运行的查询。当然，我简化了示例，实际的查询超过了 400 行 SQL。

查询结果被保存到 CSV 文件中，然后上传到目标，即**Google Bigquery 数据库**。在 Apache Airflow 中，它是这样的 —

这是一个 ETL 管道的简单示例。它按预期工作，但团队意识到将其重新设计为 ELT 管道的**好处**。稍后会详细介绍。

![](../Images/36c7974a56ab4e7173aec07e1358e3b6.png)

图片由作者提供

这是在重新设计之后的管道示例。观察一下表格是如何**保持原样**地带入目标的。在所有表格成功提取后，我们在目标中执行关系转换。

```py
--transactions
Select 
*
from transactions --
Select
*
from users
```

这是在源数据库中运行的查询。大多数提取使用的是‘Select *’语句**没有任何联接**。对于追加任务，我们包括**where 条件**以正确分隔数据。

同样，查询结果被保存到一个 CSV 文件中，然后上传到 Google BigQuery 数据库。我们随后为转换任务制作了一个单独的 dag，通过**在 Apache Airflow 中设置依赖关系**来实现。这是为了确保所有提取任务在运行转换任务之前都已完成。

我们使用**Airflow Sensors**设置依赖关系。你可以在[这里](https://towardsdatascience.com/data-engineering-how-to-set-dependencies-between-data-pipelines-in-apache-airflow-using-sensors-fc34cfa55fba)了解它们。

[**数据工程 — 如何在 Apache Airflow 中设置数据管道之间的依赖关系**](https://towardsdatascience.com/data-engineering-how-to-set-dependencies-between-data-pipelines-in-apache-airflow-using-sensors-fc34cfa55fba)

## 为什么我这么做

![](../Images/29a9cdb5ff4f6a2b66b1819e49f13a39.png)

图片由 [Markus Winkler](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

现在你了解了我是怎么做的，我们来讨论一下**为什么**——我们究竟为何将所有 ETL 重新编写为 ELT 管道？

## 成本

使用我们旧的数据管道花费了团队**资源**，特别是时间、精力和金钱。

为了理解成本方面的情况，你需要知道我们的源数据库（Postgres）是2008年设置的古老机器。它托管在本地，也运行着旧版本的 Postgres，这使事情变得更加复杂。

直到最近几年，组织才意识到对数据科学家和分析师来说需要一个集中的数据仓库。于是他们开始将旧的管道迁移到定时任务上。随着任务数量的增加，这消耗了机器上的资源。

之前的数据分析师编写的 SQL 联接也非常混乱。在某些数据管道中，一个查询中有超过**20 个联接**，而我们的数据管道接近 100 个。我们的任务通常在午夜开始运行，通常在下午 1 到 2 点结束，总共大约需要**12 个小时以上**，这绝对不可接受。

对于那些不知道的人来说，SQL 联接是**最耗费资源的命令之一**。随着联接数量的增加，它将使查询的运行时间呈指数级增长。

![](../Images/31cd342fe86942f3c1f3a7daedfdc14b.png)

作者提供的图片

由于我们迁移到 Google Cloud，团队理解到 Google BigQuery 在计算 SQL 查询方面**非常快速**。你可以在[**这里**](https://cloud.google.com/blog/products/bigquery/anatomy-of-a-bigquery-query)阅读有关内容。

[**BigQuery 速度有多快？| Google Cloud 博客**](https://cloud.google.com/blog/products/bigquery/anatomy-of-a-bigquery-query)

因此，关键是只在源中运行简单的‘Select *’语句，并在 Google Cloud 上执行所有连接。

这**使我们数据管道的** **效率和速度** **增加了两倍以上**。

## 可扩展性

![](../Images/995369f87b81e5cf6bb9e7439b0b2a3e.png)

由 [Quinten de Graaf](https://unsplash.com/@quinten149?utm_source=medium&utm_medium=referral) 提供的照片，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

随着业务的扩展，它们的工具和技术也在扩展。

通过迁移到 Google Cloud，我们可以轻松扩展我们的机器和管道，而无需过多担心。

Google Cloud 利用**Cloud Monitoring**，这是一个收集 Google Cloud 技术（如 Google Cloud Composer、Dataflow、Bigquery 等）的指标、事件和元数据的工具。你可以监控各种数据点，包括但不限于 —

+   虚拟机的成本

+   每个在 Google Bigquery 中运行的查询的成本

+   每个在 Google Bigquery 中运行的查询的大小

+   数据管道的持续时间

这使得监控对我们来说变得轻松。因此，通过在 Google Bigquery 上执行所有转换，我们能够准确监控我们的查询大小、持续时间和成本，随着扩展而进行调整。

即使我们**增加**我们的机器规模、数据仓库、数据管道等，我们也完全**理解随之而来的成本和利益**，并能完全控制是否需要开启或关闭。

这已经并将为我们省去很多麻烦。

## 结论

![](../Images/7d220a315695610328a8d88fc84123d7.png)

由 [Fernando Brasil](https://unsplash.com/@nandovish?utm_source=medium&utm_medium=referral) 提供的照片，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

如果你读到这里，你肯定对数据非常感兴趣。

你应该这样做！

我们已经做了 ETLs 和 ELTs。谁知道我们将来会构建什么样的管道呢？

在这篇文章中，我们讨论了 —

+   什么是 ELT/ETL 数据管道？

+   我是如何将 ETL 重新设计为 ELT 管道的

+   我为什么这样做

一如既往，我以一句名言结尾。

> 数据是新的科学。大数据持有答案
> 
> — 佩特·盖尔辛格

### [订阅我的新闻通讯以保持联系。](https://www.nicholas-leong.com/sign-up-here)

你也可以通过 [**我的链接**](https://nickefy.medium.com/membership) 支持我。你将能阅读我和其他优秀作家的无限故事！

我正在数据行业中创作更多故事、文章和指南。你可以期待更多类似的帖子。在此期间，随时查看我其他的 [**文章**](https://nickefy.medium.com/) 来暂时满足你对数据的渴望。

***感谢*** 阅读！如果你想与我联系，请随时通过 nickmydata@gmail.com 或我的 *[*LinkedIn 个人资料*](https://www.linkedin.com/in/nickefy/)* 联系我。你还可以查看我以前文章的代码，位于我的 *[*Github*](https://github.com/nickefy)*。*

**简历：[Nicholas Leong](https://www.linkedin.com/in/nickefy/)** 是一名数据工程师，目前在一家在线分类广告技术公司工作。在他的多年经验中，Nicholas 完全设计了批处理和流处理管道，改进了数据仓库解决方案，并为组织执行了机器学习项目。在空闲时间，Nicholas 喜欢从事自己的项目以提高技能。他还撰写关于自己工作、项目和经验的文章，以与世界分享。[访问我的网站查看我的工作](https://nickefy.github.io/porfoliosite/)！

[原文](https://towardsdatascience.com/how-i-redesigned-over-100-etl-into-elt-data-pipelines-c58d3a3cb3c)。经许可转载。

**相关：**

+   [Prefect：如何使用Python编写和调度你的第一个ETL管道](/2021/08/prefect-write-schedule-etl-pipeline-python.html)

+   [机器学习管道的设计模式](/2021/11/design-patterns-machine-learning-pipelines.html)

+   [ETL与ELT：指南与市场分析](/2021/10/etl-elt-guide-market-analysis.html)

### 更多相关话题

+   [ETL与ELT：哪种更适合你的数据管道？](https://www.kdnuggets.com/2023/03/etl-elt-one-right-data-pipeline.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [ETL与ELT：数据集成对决](https://www.kdnuggets.com/2022/08/etl-elt-data-integration-showdown.html)

+   [SQL与数据集成：ETL和ELT](https://www.kdnuggets.com/2023/01/sql-data-integration-etl-elt.html)

+   [停止学习数据科学以寻找目标，并通过寻找目标…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [成功数据科学家的5个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)
