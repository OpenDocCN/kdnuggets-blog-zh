# ETL 与 ELT：数据集成对决

> 原文：[https://www.kdnuggets.com/2022/08/etl-elt-data-integration-showdown.html](https://www.kdnuggets.com/2022/08/etl-elt-data-integration-showdown.html)

![ETL 与 ELT：数据集成对决](../Images/1cdad56135ba710f07cda2ffee1db1c5.png)

[EJ Strat](https://unsplash.com/@xoforoct) via Unsplash

***提取-转换-加载*** 与 ***提取-加载-转换***

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT工作

* * *

它们都是用于将数据从一个来源转移到数据仓库的数据集成方法。虽然它们的方法目标相似，但有所不同。

# 什么是 ETL？

ETL 是将数据从多个来源转移到集中单一数据库的过程。原始数据从源中**提取**，在单独的处理服务器上**转换**，然后**加载**到目标数据库中。

转换在加载之前进行的原因是**提取**的数据需要符合目标数据库的数据规范。例如，有些数据仓库只能接受基于SQL的数据结构。

ETL 方法在一定意义上确保了合规性，即**提取**的数据以正确的数据形式**转换**到目标数据库。如果**提取**的数据没有正确**转换**，则不会成功移动和加载到数据仓库中。

# 什么是 ELT？

ELT 不要求原始数据在加载之前进行转换。原始数据被加载到数据仓库中，转换、数据清理等操作发生在数据仓库中。

由于数据以原始格式留在数据仓库中，这允许不同类型的转换和分析进行。

ELT 在技术行业中较新，其发展得益于可扩展的云基础数据仓库。因此，随着时间的推移和更多公司采纳云基础设施，你会看到 ELT 过程也变得越来越受欢迎。

# ETL 和 ELT 过程的比较

|  | **ETL** | **ELT** |
| --- | --- | --- |
| **发现** | 存在已超过20年 | 对数据集成方法较新 |
| **提取** | 原始数据使用API连接器提取。 | 原始数据使用API连接器提取。 |
| **转换** | 原始数据在次级处理服务器上进行转换。 | 原始数据在目标数据库内进行转换。 |
| **加载** | 原始数据必须在加载到目标数据库之前进行转换。 | 原始数据直接加载到目标数据库中。 |
| **时间** | 数据转换使ETL过程耗时较多 | 数据转换是并行进行的 - 提高了时间效率 |
| **隐私** | 数据在加载前进行转换可以消除个人识别信息（PII） | 这需要更多的隐私标准 |
| **成本** | 使用备用处理服务器可能会增加成本 | 由于简化的数据堆栈，成本更低 |
| **数据结构** | 结构化 | 可以是结构化、半结构化和非结构化的 |
| **数据大小** | 通常用于较小的数据集 | 通常用于较大的数据集 |
| **数据集需求** | 复杂的转换 | 速度和效率 |
| **重新查询** | 因为数据在进入目标数据库之前已被转换，所以无法重新查询。 | 是的，因为数据尚未被转换 |
| **数据湖兼容性** | 否 | 是 |

# 结论

如果你想确定需要使用哪种数据集成过程，那取决于你团队的需求。

是的，ELT相对较新且具有更多优势，但可能无法实现你的需求。

了解你的需求将帮助你确定使用哪种处理过程。

**[Nisha Arya](https://www.linkedin.com/in/nisha-arya-ahmed/)**是一位数据科学家和自由职业技术作家。她特别感兴趣于提供数据科学职业建议或教程，以及围绕数据科学的理论知识。她还希望探索人工智能如何/能够有益于人类生命的持久性。作为一个热衷的学习者，她寻求扩展她的技术知识和写作技能，同时帮助指导他人。

### 更多相关话题

+   [SQL和数据集成：ETL和ELT](https://www.kdnuggets.com/2023/01/sql-data-integration-etl-elt.html)

+   [ETL与ELT：哪个更适合你的数据管道？](https://www.kdnuggets.com/2023/03/etl-elt-one-right-data-pipeline.html)

+   [人工智能如何转变数据集成](https://www.kdnuggets.com/2022/04/artificial-intelligence-transform-data-integration.html)

+   [AI/ML技术集成如何帮助企业实现…](https://www.kdnuggets.com/2021/12/aiml-technology-integration-help-business-achieving-goals-2022.html)

+   [数据仓库和ETL最佳实践](https://www.kdnuggets.com/2023/02/data-warehousing-etl-best-practices.html)

+   [ETL的演变：跳过转换如何增强数据管理](https://www.kdnuggets.com/evolution-in-etl-how-skipping-transformation-enhances-data-management)
