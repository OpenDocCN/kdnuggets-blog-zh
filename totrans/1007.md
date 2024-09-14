# R 数据准备备忘单

> 原文：[https://www.kdnuggets.com/2021/10/data-preparation-r-dplyr-cheat-sheet.html](https://www.kdnuggets.com/2021/10/data-preparation-r-dplyr-cheat-sheet.html)

# 数据准备的重要性

我之前写过，不论我们愿意与否，数据准备是每个数据科学项目的重要组成部分。数据准备包括将数据以可重复的过程准备好以用于业务分析的任务，包括数据获取、数据存储和处理、数据清洗以及特征工程的早期阶段。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 需求

* * *

数据团队可以使用至少三种常见工具来完成这些数据处理任务：

+   SQL，许多大数据平台如 Spark 支持的 SQL，非常适合从原始数据源如数据湖文件集合中进行粗略的数据过滤和收集。

+   Python 和 Pandas 库的使用正在日益普及，并且功能不断增强。

+   R，特别是使用 dplyr 包，提供了一套由大量开源 R 库支持的统一函数集合。

在这三者之间的选择很可能取决于你组织中可用的技能、可用的基础设施和代码库，以及需要使用的高级模型。本文将重点讨论使用 R 的原因，并提供一个实用的参考表。

dplyr 自 2016 年推出以来，具有一些重要功能，使其成为 R 中数据准备的优秀工具。

+   几乎适用于行业中任何数据源或文件格式的数据连接。

+   dplyr 被构建为一个和谐的包，简化了许多任务，这些任务如果需要将其他 R 包拼凑在一起，则可能会变得混乱或令人困惑。

+   脚本可以轻松集成版本控制和 Dev Ops 实践。

+   轻松将数据交给强大的 R 库，以便与 AI/ML 模型集成。

# R 数据准备备忘单

以下[快速参考备忘单指南](https://www.kdnuggets.com/publications/sheets/Data-Prep-in-R-dyplr-Pugsley-KDnuggets.pdf)将提供 dplyr 在数据准备每个步骤中的方法样例。这不是一个 dplyr 函数或选项的详尽列表，而是一个起点。

[![R 数据准备备忘单](../Images/b24af19659a7a94b18b71a6fce6812f4.png)](https://www.kdnuggets.com/publications/sheets/Data-Prep-in-R-dyplr-Pugsley-KDnuggets.pdf)

点击查看完整备忘单

[**在这里下载快速参考备忘单 PDF！**](https://www.kdnuggets.com/publications/sheets/Data-Prep-in-R-dyplr-Pugsley-KDnuggets.pdf)

十年前，R 是数据科学的唯一选择，但 Python 和 SQL 的竞争让它变得更好，因为一个生态系统中引入的功能很快会被复制或移植到另一个生态系统中。广泛的 R 用户社区有着确保其库活跃和发展的历史，确保你对 R 的投资在未来十年仍然具有相关性。未来某一天，也许 dplyr 和 Tidyverse 将不再是数据准备的最佳选择。但目前它们仍是极佳的选择（尽管有一些尴尬的语法元素，比如 %>% 管道！）。

**[Stan Pugsley](https://www.linkedin.com/in/spugsley/)** 是一位驻盐湖城，犹他州的独立数据仓库和分析顾问。他还是犹他大学 - 埃克尔斯商学院的助理教授/讲师。你可以通过 [电子邮件](mailto:stanford.pugsley@eccles.utah.edu) 联系作者。

### 更多相关主题

+   [SQL 数据准备备忘单](https://www.kdnuggets.com/2021/05/data-preparation-sql-cheat-sheet.html)

+   [机器学习中的数据准备和原始数据](https://www.kdnuggets.com/2022/07/data-preparation-raw-data-machine-learning.html)

+   [KDnuggets 新闻，11月30日：什么是切比雪夫定理及其如何…](https://www.kdnuggets.com/2022/n46.html)

+   [数据科学的 Git 备忘单](https://www.kdnuggets.com/2022/11/git-data-science-cheatsheet.html)

+   [数据科学的 Linux 备忘单](https://www.kdnuggets.com/2022/11/linux-data-science-cheatsheet.html)

+   [Python 字符串处理备忘单](https://www.kdnuggets.com/2020/01/python-string-processing-primer.html)
