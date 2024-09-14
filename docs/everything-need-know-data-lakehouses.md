# 关于数据湖仓你需要了解的一切

> 原文：[https://www.kdnuggets.com/2022/09/everything-need-know-data-lakehouses.html](https://www.kdnuggets.com/2022/09/everything-need-know-data-lakehouses.html)

![关于数据湖仓你需要了解的一切](../Images/e8f6e0a069698b2c349a18a4279221b6.png)

来源：[Dremio](https://www.dremio.com/)

你是否可以将**数据湖仓**视为科技界的新流行词？看起来我们可以。我们最初使用的数据仓库，指的是一种可以分析以帮助决策的信息存储架构。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的IT

* * *

数据仓库可以追溯到20世纪80年代，在商业世界的许多不同方面都发挥了作用。然而，大数据时代很快来临，当时非结构化原始数据占据了许多组织中可用数据和信息的80-90%。

正是因为数据仓库无法处理非结构化数据，其模型基于结构化数据，数据湖仓成为了下一个大趋势。

作为这篇文章的一部分，我与[Dremio](https://www.dremio.com/)，一个提供“适合熟悉和喜爱SQL的团队的湖仓平台”的组织合作。Dremio成立于2015年，专注于数据湖仓。他们的技术已应用于包括联合利华、德勤、诺基亚在内的各种组织，并与AWS等多个云和技术合作伙伴合作。Dremio已成为数据湖仓领域的领导者。

> “数据湖仓结合了数据湖的可扩展性和灵活性，以及数据仓库的性能和功能。数据湖仓是两者的最佳结合，是分析目的的数据管理的未来。”
> 
> — 马克·莱昂斯，[Dremio](https://www.dremio.com/)

他们的团队很乐于助人，并回答了一系列我们提出的问题，以帮助你了解有关数据湖仓的所有信息。

# 与Dremio的马克·莱昂斯的讨论

**编辑注**：以下问题由[马克·莱昂斯](https://www.linkedin.com/in/markclyons/)，[Dremio](https://www.dremio.com/)的产品管理副总裁回答。

## 什么是数据湖仓？

> [数据湖屋](https://www.dremio.com/blog/how-we-got-to-open-lakehouse/)在技术市场上相对较新，它是在2010年发明的，并迅速获得了主流采用。数据湖屋具有用于数据分析和处理非结构化数据的能力，这与数据仓库不同。

## 数据湖屋的作用是什么？

> 数据湖屋使公司能够在数据探索到关键业务智能仪表板的所有分析工作负载中，在一个数据副本上运行（通常为[Apache Parquet](https://parquet.apache.org/)格式，这是性能最好的），因为它存储在云对象存储中。

## 数据湖屋如何工作？

> 数据湖屋是2.0版本的数据湖，具有许多相同的目标，但改进了技术，解决了数据湖即“数据沼泽”的历史性不足。数据湖屋不仅仅是对象存储中的文件——它的意义更大。
> 
> 数据湖屋具有添加表格格式的能力，这些格式支持仓库功能，例如一致的插入、更新和删除，以及对底层数据优化（如文件压缩，以防止著名的“小文件”问题）等。

## 数据湖屋的基本特征是什么？

> **重要特征包括：**
> 
> +   多引擎支持（机器学习、SQL、流处理等）
> +   
> +   对行业标准表格格式（如[Apache Iceberg](https://iceberg.apache.org/)）进行插入、更新和删除
> +   
> +   像传统数据库管理系统（DBMS）一样的原子事务和数据一致性保证
> +   
> +   元数据（[Hive metastore](https://hive.apache.org/)、[Project Nessie](https://projectnessie.org/)、[AWS Glue Catalog](https://docs.aws.amazon.com/glue/latest/dg/start-data-catalog.html)等）
> +   
> +   供应商无关
> +   
> +   标准的客户端集成以支持使用笔记本或仪表板工具的数据消费者
> +   
> +   自动扩展
> +   
> +   用户可以选择最适合自己需求的软件或SaaS。

## 数据湖屋与数据仓库之间的关系是什么？

> 数据湖屋与数据仓库有相似之处也有不同之处。数据湖屋使SQL引擎（如Dremio、Hive、Athena、Spark SQL等）能够对行业标准表格格式提供[数据操作语言](https://en.wikipedia.org/wiki/Data_manipulation_language)（DML）功能，并为事务和数据一致性提供数据库级别的保证。
> 
> 这使得数据湖屋相当于数据仓库的能力，但当你查看其差异时，情况还远不止于此。数据湖屋支持其他引擎用于SQL之外的用例，如机器学习或流处理，同时保持供应商无关性，这意味着它们以及所有使用的数据都保持在遵循开放数据架构哲学的开放格式中。

## 为什么数据湖屋变得如此受欢迎？

> 通过数据湖仓架构，数据消费者可以立即使用他们喜欢的工具来分析数据——这是一个巨大的优势！数据工程师节省了大量将数据加载到仓库中供他人使用以及维护相关基础设施的时间和金钱。
> 
> 此外，数据湖仓消除了云数据仓库臭名昭著的供应商锁定和锁定问题。云对象存储中的数据以开放的、与供应商无关的格式存储，如[Apache Parquet](https://parquet.apache.org/)和[Apache Iceberg](https://iceberg.apache.org/)，因此没有供应商能够对数据施加影响。
> 
> 在湖仓领域，公司自然受益于竞争和创新。它们在选择计算引擎以处理当前使用的数据类型方面具有灵活性，并且能够轻松尝试未来出现的新计算引擎。

# 结论

Dremio的目标是打破一个持续了30年的范式，这个范式束缚了每个公司。他们希望消除某些障碍，帮助公司变得创新，加快洞察时间，并将控制权交还给用户。

我希望这次与来自Dremio的Mark Lyons的访谈能帮助你更好地理解数据湖仓是什么，它们为何进入市场，它们的工作原理，它们的重要特性，以及它们与数据仓库的区别。

**我们要感谢Mark Lyons和[Dremio](https://www.dremio.com/)参与本文的创作。**

**[Nisha Arya](https://www.linkedin.com/in/nisha-arya-ahmed/)** 是一位数据科学家和自由技术作家。她特别关注提供数据科学职业建议或教程，以及围绕数据科学的理论知识。她还希望探索人工智能如何（或能）促进人类寿命的不同方式。作为一名热心的学习者，她寻求拓宽自己的技术知识和写作技能，同时帮助指导他人。

### 更多相关内容

+   [KDnuggets新闻，4月13日: 数据科学家应了解的Python库…](https://www.kdnuggets.com/2022/n15.html)

+   [朴素贝叶斯算法: 你需要知道的一切](https://www.kdnuggets.com/2020/06/naive-bayes-algorithm-everything.html)

+   [你需要知道的张量一切](https://www.kdnuggets.com/2022/05/everything-need-know-tensors.html)

+   [你需要知道的MLOps一切: KDnuggets技术简报](https://www.kdnuggets.com/tech-brief-everything-you-need-to-know-about-mlops)

+   [ChatGPT: 你需要知道的一切](https://www.kdnuggets.com/2023/01/chatgpt-everything-need-know.html)

+   [GPT-4: 你需要知道的一切](https://www.kdnuggets.com/2023/03/gpt4-everything-need-know.html)
