# 7 个数据工程师在 Google BigQuery 中需要注意的“陷阱”

> 原文：[https://www.kdnuggets.com/2019/03/7-gotchas-data-engineers-google-bigquery.html](https://www.kdnuggets.com/2019/03/7-gotchas-data-engineers-google-bigquery.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者：[Josh Levy](https://www.linkedin.com/in/joshlevymstr/)，[Aspirent](http://www.aspirent.com)**

![BigQuery](../Images/e61b003f2909458008871a278be95a21.png)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 部门

* * *

如今，无论你看向哪里，IT 组织都在寻求通过云计算来解决数据存储、传输和分析的挑战……这也有很好的理由！来自 Amazon、Google、Microsoft 等的云服务彻底改变了我们对数据的思考方式，无论是从 IT 还是最终用户的角度来看。它们大大减少了企业数据中心的功能，这通常不是组织的核心能力。它们使数据、分析和处理能力变得民主化。它们打破了我们对数据仓储和分析成本的思考方式。云计算拥有所有这些特性以及更多，因此很多组织急于将大数据操作转移到云端也就不足为奇了。成本节约、性能提升和简化操作都是很好的，但当你真正开始工作时会发生什么呢？很可能你的数据工程师对你组织决定使用的任何云技术都很陌生。本文详细介绍了我作为数据工程师第一次接触 Google BigQuery (GBQ) 的经验。

我已经做了多年的数据工程师，并且在职业生涯中使用过大多数类型的 RDBMS 和 SQL。最近的一个项目中，我和我的同事将一个现有的 SQL Server 数据库重新设计为 GBQ，这激发了我撰写本文，作为对初次接触 GBQ 的数据工程师的资源。以下是一些可能需要适应的内容，以及我发现的缓解策略：

+   **区分大小写** - 与大多数 RDBMS 不同，BigQuery 对大小写敏感，不仅在字符串比较中如此，在对象名称中也一样。对于字符串比较，在两边（或一边对比字面量）使用 UPPER 函数可以消除大小写敏感的影响。 有趣的是，虽然在 FROM 子句中引用对象名称时是区分大小写的，但列名称则不区分大小写。

+   **完全限定的对象名称** - 在 FROM 子句中引用的所有对象名称必须完全限定到数据集级别，如果这些对象不在当前计费项目中，则必须到项目级别。不仅如此，整个完全限定的名称必须用反向单引号（通常与波浪键共享）括起来。你绝对会在某个时刻花费几分钟沮丧地尝试找出查询错误的原因，直到你意识到这些引号中的一个与其他的看起来不一样。

+   **标准 SQL 与遗留 SQL** - BigQuery 有两种 SQL 版本，不知何故。标准 SQL 非常类似于 ANSI SQL，这是你应该使用的。然而，经典的 BigQuery Web UI（由于某些我将很快提到的原因，我偏爱它）默认使用遗留 SQL。可以选择使用标准 SQL，也可以使用名为 BigQuery Mate 的 Chrome 扩展程序来跳过这一步。

+   **没有存储过程或函数** - 这是一个大问题，真的没有简单的解决办法。Google 提供了进行 ETL 和数据转换准备的工具，但它们没有编码自己过程的灵活性。可以创建函数，但它们是临时的，只在单个会话中存在，这使得它们作为编程工具非常有限。

+   **经典 UI 与新 UI** - 经典 UI（看起来像 Gmail 的那个）有一个大优势，那就是它允许你添加一个你没有明确命名访问权限的项目到对象浏览器。出于某种原因，新 UI 没有这个功能。然而，它默认使用标准 SQL，这可以为你节省一些麻烦。两者都不错，但都不完美。

+   **没有查询计划估算** - 你不得不怀疑运行 Google 云服务部门的那些人是否曾在前世当过医生。还有哪个行业会在没有先解释成本的情况下提供服务？这本质上就是 Google 在做的事情，尽管这种方式不那么掠夺性。简单来说，没有办法在实际运行复杂查询之前确定其是否高效。一旦运行查询，执行计划实际上非常有用且可操作。

+   **有限的 DDL 能力** - 一旦表存在，就几乎没有能力去修改它。你不能移除列或更改数据类型。你不能重命名字段或表。几乎任何表或视图的修改操作都涉及“CREATE TABLE AS SELECT”类型的操作，这意味着你技术上会得到一个“新”的表或视图。

我想读到这份列表时，可能会觉得我对 BigQuery 有些贬低。这完全不是事实。这些大多数只是新 GBQ 开发人员会遇到并克服的烦恼。实际上，还有很多值得喜欢的地方。在我第一次 GBQ 项目中，我遇到了一些令人愉快的惊喜：

+   **自动对象过期** - 这是为那些总是让 25 个临时表在数据库角落里积灰的懒惰 DBA 准备的。在 GBQ 中，你可以（而且应该）创建一个数据集并指定一个短暂的对象生命周期。这样，你可以拥有一个自动清理的沙盒环境，不必担心额外的存储费用，因为你不再需要的数据会被自动清理。

+   **查询历史** - GBQ 会记录你运行的所有查询，当然是为了计费目的，但它也会将这些查询以易于搜索的列表形式展示给你。如果你丢失了某段代码，这会非常方便，这种情况即使对最优秀的人来说也会发生。

+   **缓存查询结果** - 谷歌对存储数据和检索数据通常会收费。如果你像我一样，可能会在开发过程中定期运行相同的查询。GBQ 默认会缓存这些结果，你的项目将不会被收费。如果需要，你可以在 Web UI 选项中关闭此功能。

+   **自动补全** - 大多数查询工具，如 TOAD 或 SQL Management Studio，会以不同程度的成功来自动补全表和列名称。我发现 GBQ Web UI 的自动补全功能非常好用。

+   **基于 GUI 的表创建** - 有时你只需要创建一个快速的小表来存储参数或类似的东西。GBQ Web UI 允许没有 SQL 技能的用户创建表并添加各种数据类型的列。

总的来说，GBQ 需要一点时间适应，并且仍然有一两个明显的功能缺陷，主要与无法创建存储过程或函数有关。然而，云数据存储和分析的潜在好处远远超过了这些考虑因素。我每天都在学习新的快捷方式和技巧，随着我克服各种小挑战，我能看到前方光明的“云端”未来。

**个人简介：[Josh Levy](https://www.linkedin.com/in/joshlevymstr/)** 是 aspirent 分析实践部门的经理。他在商业智能领域从事了 20 多年的工作，担任过各种职位和行业。欲了解更多信息，请访问 [www.aspirent.com](http://www.aspirent.com)。

**相关内容：**

+   [BigQuery 与 Redshift：定价策略]( /2018/07/bigquery-vs-redshift-pricing-strategy.html)

+   [旅行大数据工程炒作列车时你应该知道的事]( /2018/10/big-data-engineering-hype-train.html)

+   [构建有效数据科学团队]( /2019/03/building-effective-data-science-teams.html)

### 更多相关话题

+   [5 个常见的 Python 陷阱（以及如何避免它们）](https://www.kdnuggets.com/5-common-python-gotchas-and-how-to-avoid-them)

+   [如何获得 ML 职位：来自 Meta、Google Brain 和 SAP 的工程师建议](https://www.kdnuggets.com/2022/08/corise-land-ml-job-advice-engineers-meta-google-brain-sap.html)

+   [数据工程师和数据科学家都需要的高保真合成数据](https://www.kdnuggets.com/2022/tonic-high-fidelity-synthetic-data-engineers-scientists-alike.html)

+   [我们不需要数据科学家，我们需要数据工程师](https://www.kdnuggets.com/2021/02/dont-need-data-scientists-need-data-engineers.html)

+   [数据科学家和数据工程师如何协作？](https://www.kdnuggets.com/2022/08/data-scientists-data-engineers-work-together.html)

+   [关于数据工程师的11个问题：这个职业到底是怎样的，等等…](https://www.kdnuggets.com/2022/10/11-questions-data-engineers-profession-heading.html)
