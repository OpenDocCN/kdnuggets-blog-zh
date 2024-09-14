# 《解决 MySQL 中幻读问题的权威指南》

> 原文：[`www.kdnuggets.com/2022/06/definitive-guide-solving-phantom-read-mysql.html`](https://www.kdnuggets.com/2022/06/definitive-guide-solving-phantom-read-mysql.html)

![解决 MySQL 中幻读问题的权威指南](img/3278c7a60acba0db970ee57e95209464.png)

图片来源：[storyset](https://www.freepik.com/author/stories)

当大多数人想到关系数据库时，MySQL 通常会首先浮现在脑海中。MySQL 使用 InnoDB 作为存储引擎，而 *可重复读* 隔离级别是最常见的，它在事务开始之前查看数据。

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

然而，与 PostgreSQL 不同，InnoDB 中的 [*可重复读* 隔离级别](https://www.ibm.com/docs/en/wxs/8.6.1?topic=overview-transaction-isolation) 无法平滑处理某些问题。具体来说，诸如丢失更新和幻读等问题无法在 InnoDB *可重复读* 隔离级别下处理，而在 PostgreSQL 中，解决丢失更新不需要额外的黑客手段。然而，你可以使用一些技巧来执行幻读，例如范围类型和其他机制。

MySQL 开发人员应了解可能的陷阱，并能够适当地解决它们，以避免丢失更新和幻读等问题。本文将介绍 MySQL 开发人员 如何排除幻读可能造成的“写入”偏差。

# 导致幻读的场景

导致幻读的场景各不相同。然而，一般来说，这些场景都遵循类似的模式。它们最初会在 MySQL 数据库中搜索特定范围，然后根据搜索结果进行 CREATE、UPDATE 或 DELETE 操作。之后，执行的操作直接影响从已搜索范围中获得的结果。

假设，例如，在搜索特定范围后采取的操作是 UPDATE 或 DELETE。在这种情况下，MySQL 开发人员可以使用排他锁以避免“写偏斜”。然后，开发人员可以在 SELECT 的开头使用 FOR UPDATE，之后他们可以强制两个同时进行的事务一个接一个地执行。因此，这两个同时进行的事务在竞争条件期间，绕过了“写偏斜”。

如果我们假设根据特定范围的搜索结果采取的操作是 CREATE，那么上述解决方案是不完整的：没有对应的行允许开发人员在 SELECT 时锁定，这意味着行会在后面形成。

# 使用 CREATE 时解决幽灵读

我们将介绍一个实际场景，以更好地理解在使用 CREATE 时幽灵读导致的问题。一旦我们描述了这个例子，我们将讨论相应的解决方案。

想象一个允许人们预订会议室的系统；在有人通过这个系统预订了房间后，会向表中添加新数据。这个系统让用户知道房间是否可用，取决于他们想要预订的时间段。一旦有人创建了新的预订记录，所有其他用户就可以避免时间冲突。

然而，当两个人需要同时预订一个房间时，就会出现问题。两个用户都能够通过初始 SELECT 验证，这意味着在纸面上，他们都有同一时间的预订，从而导致时间冲突。如果例如，多个用户[通过 VPN 连接](https://thebestvpn.com/reviews/surfshark/)到远程 SQL 服务器并需要使用这个预订系统，那么问题可能会加剧。即使 MySQL 开发人员添加了排他锁，也无法避免这个问题，因为他们不能在开始的 SELECT 验证时锁定行。

## 使用唯一约束索引来解决

MySQL 开发人员不能通过使用排他锁将并发操作转变为顺序操作。因此，他们需要通过向表中添加唯一约束来让一个操作失败。

开发人员可以在房间预订表的列中，针对房间号和会议开始时间使用唯一约束索引。这种解决方案可以防止有人预订已经被别人预订的时间段，并且开发人员可以确保没有人能预订超过一个小时的房间。

然而，这个解决方案还会阻止唯一约束在两个用户的会议时间重叠时有效。要正确解决这个问题，开发人员必须改为实现冲突检测。

## 通过实现冲突来解决

解决我们讨论的幻读问题的正确方法是揭示表隐藏的冲突。开发人员可以用数据集预填充一个全新的表，这些数据集协调并发操作。如果我们以我们的会议室系统为例，可以想象创建一个新表来规定时间段，并提前显示所有可用的时间段。

使用这个新表，开发人员现在需要对决定可用时间段的列执行 SELECT，并包括 FOR UPDATE，因为数据已经存在。开发人员需要在初始 SELECT 之前运行这个 SELECT FOR UPDATE。

通过在上述示例中实现冲突，开发人员可以使用排他锁阻止任何两个重叠的保留时间段，从而强制一个时间段在另一个时间段之前或之后。较晚的时间段会因为第一个时间段的完成而立即失败。

# 结论

尽管实现冲突是一种困难且不直观的解决方案，但在使用 MySQL 数据库时，为了避免牺牲任何显著的性能水平，这是必要的。遗憾的是，MySQL 的 InnoDB 隔离级别并非可序列化，因此开发人员需要为可接受的性能水平牺牲一定的复杂性。

使用数据库的人必须[了解该数据库的能力](https://www.itprotoday.com/analytics-and-reporting/5-questions-evaluating-dbms-features-and-capabilities)以及其难以解决的解决方案。否则，无法预见该数据库的哪些行为可能会影响数据库设计和开发工作。

此外，了解如何适当地应对潜在风险同样重要。尽管我们在本文中通过时间预订系统描述的用例与其他用例不完全相同，但其展示的模式足够相似，理解如何解决这些问题可以使处理其他情况更加轻松。

**[Nahla Davies](http://nahlawrites.com/)** 是一名软件开发人员和技术作家。在全职投入技术写作之前，她管理了包括三星、时代华纳、Netflix 和索尼在内的 Inc. 5000 实验性品牌组织的首席程序员等其他引人注目的工作。

### 更多相关话题

+   [转行数据科学的权威指南](https://www.kdnuggets.com/2022/05/definitive-guide-switching-career-data-science.html)

+   [Python 函数参数：权威指南](https://www.kdnuggets.com/2023/02/python-function-arguments-definitive-guide.html)

+   [解决 5 个复杂 SQL 问题：难解查询解析](https://www.kdnuggets.com/2022/07/5-hardest-things-sql.html)

+   [思维图谱：大型语言模型中精细问题解决的新范式](https://www.kdnuggets.com/graph-of-thoughts-a-new-paradigm-for-elaborate-problem-solving-in-large-language-models)

+   [你应该阅读的生成代理研究论文](https://www.kdnuggets.com/generative-agent-research-papers-you-should-read)

+   [在转行数据科学之前请阅读此文](https://www.kdnuggets.com/read-this-before-making-a-career-switch-to-data-science)
