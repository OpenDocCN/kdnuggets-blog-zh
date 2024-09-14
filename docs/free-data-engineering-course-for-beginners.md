# 初学者免费数据工程课程

> 原文：[`www.kdnuggets.com/free-data-engineering-course-for-beginners`](https://www.kdnuggets.com/free-data-engineering-course-for-beginners)

![初学者免费数据工程课程](img/929b1c2ba1e2408a445d419b06e71126.png)

[Image by storyset](https://www.freepik.com/free-vector/data-processing-concept-illustration_12219361.htm#query=data%20engineering&position=4&from_view=search&track=ais&uuid=7f623f02-b55e-49d0-a0c3-2a10039bc7fb) 在 Freepik 上

现在是进入数据工程领域的好时机。那么你从哪里开始呢？

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

学习数据工程有时会因为需要掌握大量工具而感到压倒性，更不用说那些令人畏惧的职位描述了！

如果你正在寻找一个适合初学者的数据工程入门课程，这个由 Airbyte 的开发者倡导者 Justin Chau 主讲的免费 [数据工程初学者课程](https://www.youtube.com/watch?v=PHsC_t0j1dU) 是一个很好的起点。

在大约三小时内，你将学习到基本的数据工程技能：Docker、SQL、分析工程等。因此，如果你想探索数据工程并看看这是否适合你，这门课程是一个很好的入门选择。现在让我们来了解一下这门课程的内容。

课程链接：[数据工程初学者课程](https://www.youtube.com/watch?v=PHsC_t0j1dU)

# 为什么选择数据工程？

这门课程首先介绍了你为什么应该考虑成为一名数据工程师。我认为在深入技术主题之前，了解这些是非常有帮助的。

教师 Justin Chau 讲解了：

+   确保大数据项目成功所需的高质量数据和数据基础设施

+   数据工程角色的需求不断增长，薪酬也很高

+   作为数据工程师，您可以为组织的数据基础设施增加的业务价值

# Docker

当你学习数据工程时，Docker 是你可以添加到工具箱中的第一个工具之一。Docker 是一种流行的容器化工具，允许你将应用程序（包括依赖项和配置）打包成一个叫做镜像的单一工件。这样，Docker 让你能够创建一个一致且可重复的环境，在容器内运行你的所有应用程序。

本课程的 Docker 模块从基础知识开始，比如：

+   Dockerfile

+   Docker 镜像

+   Docker 容器

然后，讲师会讲解如何使用 Docker 对应用程序进行容器化：创建 Dockerfile 和使容器启动的命令。这个部分还涵盖了持久化卷、Docker 网络基础知识以及使用 Docker-Compose 管理多个容器。

总的来说，如果你对容器化是新手，这个模块本身就是一个很好的 Docker 快速入门课程！

# SQL

在下一个关于 SQL 的模块中，你将学习如何在 Docker 容器中运行 Postgres，然后通过创建一个示例 Postgres 数据库并执行以下操作来学习 SQL 的基础知识：

+   CRUD 操作

+   聚合函数

+   使用别名

+   连接

+   联接和联合所有

+   子查询

# 从零开始构建数据管道

有了 Docker 和 SQL 基础，你现在可以从零开始学习构建数据管道。你将从构建一个简单的 ELT 管道开始，并在课程的其余部分进行改进。

此外，你将看到你迄今为止学到的所有 SQL、Docker 网络和 Docker-compose 概念如何结合在一起，构建一个在 Docker 中运行 Postgres 的管道，涵盖源和目标。

# dbt

课程接下来进入分析工程部分，你将学习 dbt（数据构建工具）来组织你的 SQL 查询作为自定义数据转换模型。

教师将指导你开始使用 dbt：安装所需的适配器和 dbt-core，并设置项目。这个模块特别关注使用 dbt 模型、宏和 jinja。你将学习如何：

+   定义自定义 dbt 模型并在目标数据库中的数据上运行它们

+   将 SQL 查询组织为 dbt 宏以便重用

+   使用 dbt jinja 为 SQL 查询添加控制结构

# CRON 作业

到目前为止，你已经构建了一个基于手动触发的 ELT 管道。但你肯定需要一些自动化，而最简单的方法就是定义一个定时任务（cron job），在每天的特定时间自动运行。

这个超级简短的部分介绍了 cron 作业。但是像 Airflow（你将在下一个模块中学习）这样的数据编排工具提供了对管道的更精细控制。

# Airflow

为了编排数据管道，你将使用开源工具，如 Airflow、Prefect、Dagster 等。在这一部分你将学习如何使用开源编排工具 Airflow。

这一部分比之前的部分更为详细，因为它涵盖了你需要了解的一切，以便迅速掌握为当前项目编写 Airflow DAG 的方法。

你将学习如何设置 Airflow 的 Web 服务器和调度器以安排作业。接着你将学习 Airflow 操作符：Python 和 Bash 操作符。最后，你将定义进入 DAGs 的任务以进行示例。

# Airbyte

在最后一个模块中，你将学习 Airbyte，一个开源的数据集成/迁移平台，它让你轻松连接更多的数据源和目标。

你将学习如何设置你的环境，并了解如何使用 Airbyte 简化 ELT 过程。为此，你需要修改现有项目的组件：ELT 脚本和 DAGs，以将 Airbyte 集成到工作流中。

# 总结

希望你觉得这篇关于免费数据工程课程的评测对你有帮助。我很喜欢这个课程——特别是构建和逐步改进数据管道的实践方法，而不仅仅是理论。代码也提供了供你跟随。所以，祝你数据工程愉快！

**[](https://twitter.com/balawc27)**[Bala Priya C](https://www.kdnuggets.com/wp-content/uploads/bala-priya-author-image-update-230821.jpg)** 是来自印度的开发者和技术写作员。她喜欢在数学、编程、数据科学和内容创作的交叉领域工作。她的兴趣和专长包括 DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编码和喝咖啡！目前，她致力于学习并通过编写教程、操作指南、观点文章等与开发者社区分享知识。Bala 还创建引人入胜的资源概述和编码教程。**

### 更多相关主题

+   [免费的 MLOps 快速入门课程](https://www.kdnuggets.com/2022/08/free-mlops-crash-course.html)

+   [免费的 AI 初学者课程](https://www.kdnuggets.com/2022/08/free-ai-beginners-course.html)

+   [免费的 Microsoft Excel 初学者课程](https://www.kdnuggets.com/2022/09/free-microsoft-excel-beginners-course.html)

+   [来自 Google + Uplimit 的免费站点可靠性工程课程](https://www.kdnuggets.com/2024/02/uplimit-free-site-reliability-engineering-course-from-google)

+   [初学者特征工程](https://www.kdnuggets.com/feature-engineering-for-beginners)

+   [KDnuggets 新闻，10 月 5 日：初学者最佳免费 Git GUI 客户端 •…](https://www.kdnuggets.com/2022/n39.html)
