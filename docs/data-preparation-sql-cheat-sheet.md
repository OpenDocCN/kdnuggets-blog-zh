# SQL 数据准备备忘单

> 原文：[`www.kdnuggets.com/2021/05/data-preparation-sql-cheat-sheet.html`](https://www.kdnuggets.com/2021/05/data-preparation-sql-cheat-sheet.html)

# 数据准备的重要性

无论我们喜欢与否，数据准备都是每个数据科学项目的重要部分。数据准备包括将数据以可重复的过程准备好以用于业务分析的任务，包括数据获取、数据存储和处理、数据清洗以及早期的特征工程。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 需求

* * *

> **马修·梅约：** "为什么数据准备常被描述为数据相关任务中 80%的工作，你认为这是一种准确的概括吗？"
> 
> **塞巴斯蒂安·拉什卡：** "80%？我经常听到>90%！"
> 
> 从 **数据准备技巧、窍门和工具：与内部人士的访谈** 中，KDnuggets

一个基本的问题是如何以及在哪里进行数据准备。选择不同工具和方法有其合理的理由：

+   你们的员工/同行最熟悉哪些工具或语言？

+   你是否需要一个可重复的流程，并定期运行？

+   你目前的大部分数据存储在哪里？

+   你处理的数据量和数据流速是多少？

+   目前有哪些云服务或本地资源可用于运行该过程？

+   哪些工具与贵组织当前的架构最兼容？

+   你的数据集是否包含需要特殊保护的私人元素（HIPA、FERPA、GDPR）？

对于许多组织来说，这些问题的答案将会引向 SQL。SQL 不仅在大多数组织中被广泛知晓和使用，而且还利用了现有的数据库资源、安全性和数据管道。如果你的原始数据在 SQL 基础的数据湖中，为什么要花时间和金钱将数据导出到新的平台进行数据准备？

# SQL 数据准备备忘单

以下 [快速参考备忘单指南](https://www.kdnuggets.com/publications/sheets/Data-Prep-with-SQL-Pugsley-KDnuggets.pdf) 将展示 SQL 在数据准备各个步骤中的应用。这并不是 SQL 函数或选项的详尽列表，而是一个起点。

![SQL 数据准备备忘单](https://www.kdnuggets.com/publications/sheets/Data-Prep-with-SQL-Pugsley-KDnuggets.pdf)

点击查看完整备忘单

**[在这里下载快速参考备忘单指南 PDF](https://www.kdnuggets.com/publications/sheets/Data-Prep-with-SQL-Pugsley-KDnuggets.pdf)**！

关于为您的模型创建接口的最后一条建议。SQL 视图允许您以干净、安全、模块化的格式封装许多数据准备步骤的复杂性。与其在 Python 或 R 代码中嵌入长而复杂的查询，不如创建一个视图，以简单、可重用的格式访问这些代码。视图也是通过遮蔽或隐藏数据元素来对私密数据应用安全性的绝佳方式。

如果您已经投资了高性能的数据库许可证，为什么不利用它们在 SQL 中进行数据准备以支持数据科学呢？

**[斯坦·帕格斯利](https://www.linkedin.com/in/spugsley/)** 是一位数据仓库和分析顾问，隶属于[艾德·贝利技术咨询公司](https://technologyconsulting.eidebailly.com/services/data-analytics/)，公司总部位于犹他州盐湖城。他还是犹他大学厄克尔斯商学院的兼职教师。您可以通过电子邮件联系作者。

### 更多相关主题

+   [R 中的数据准备备忘单](https://www.kdnuggets.com/2021/10/data-preparation-r-dplyr-cheat-sheet.html)

+   [SQL 备忘单入门指南](https://www.kdnuggets.com/2022/08/getting-started-sql-cheatsheet.html)

+   [机器学习中的数据准备和原始数据](https://www.kdnuggets.com/2022/07/data-preparation-raw-data-machine-learning.html)

+   [KDnuggets 新闻，11 月 30 日：什么是切比雪夫定理及其如何应用…](https://www.kdnuggets.com/2022/n46.html)

+   [数据科学中的 Git 备忘单](https://www.kdnuggets.com/2022/11/git-data-science-cheatsheet.html)

+   [数据科学中的 Linux 备忘单](https://www.kdnuggets.com/2022/11/linux-data-science-cheatsheet.html)
