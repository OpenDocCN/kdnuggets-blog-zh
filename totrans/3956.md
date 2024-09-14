# 每个数据工程师都应该知道的 10 个内置 Python 模块

> 原文：[https://www.kdnuggets.com/10-built-in-python-modules-every-data-engineer-should-know](https://www.kdnuggets.com/10-built-in-python-modules-every-data-engineer-should-know)

![每个数据工程师都应该知道的 10 个内置 Python 模块](../Images/6e455bb9f0812cffe77fc8a0247e588f.png)

作者提供的图像

Python 是你作为数据工程师将使用的编程语言之一。作为数据工程师，你应该熟悉许多 [Python 库](https://www.kdnuggets.com/7-python-libraries-every-data-engineer-should-know)。但 Python 的标准库中包含了用于各种相关任务的强大模块，从文件操作到数据序列化、文本处理等。

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

本文汇编了一些对数据工程最有帮助的内置 Python 模块，具体包括以下内容：

+   文件和目录管理

+   数据处理和序列化

+   数据库交互

+   文本处理

+   日期和时间操作

+   系统交互

让我们开始吧。

![python-modules-de](../Images/e7ddd9abf64a5e3813f724b2f64dd6e4.png)

数据工程的内置 Python 模块 | 作者提供的图像

## 1\. os

[os](https://docs.python.org/3/library/os.html) 模块是你与操作系统交互的首选工具。它使你能够执行各种任务，如文件路径操作、目录管理和处理环境变量。

你可以使用 os 模块的功能执行以下数据工程任务：

+   自动创建和删除用于临时或输出数据存储的目录

+   在不同目录中组织大型数据集时操作文件路径

+   处理环境变量以管理数据管道中的配置设置

[OS 模块 - 使用底层操作系统功能](https://www.youtube.com/watch?v=tJxcKyFMTGo)，由 Corey Schafer 制作的教程，涵盖了 os 模块的所有功能。

## 2\. pathlib

[pathlib](https://docs.python.org/3/library/pathlib.html) 模块提供了一种更现代和面向对象的处理文件系统路径的方法。它允许通过直观且可读的语法轻松操作文件和目录路径，使其成为文件管理任务的最爱。

pathlib 模块在以下数据工程任务中非常有用：

+   精简迭代和验证大型数据集的过程

+   在 ETL（提取、转换、加载）过程中简化移动或复制文件时路径的管理

+   确保跨平台兼容性，特别是在多环境数据工程工作流中

这里有几个教程涵盖了 pathlib 模块的基础知识：

+   [如何使用 Python 的 Pathlib 导航文件系统](https://www.kdnuggets.com/how-to-navigate-the-filesystem-with-pythons-pathlib)

+   [用 Python 的 Pathlib 组织、搜索和备份文件](https://www.kdnuggets.com/organize-search-and-back-up-files-with-pythons-pathlib)

## 3\. shutil

[shutil](https://docs.python.org/3/library/shutil.html) 模块用于常见的高级文件操作，包括复制、移动和删除文件及目录。它非常适合处理涉及大量数据集或多个文件的任务。

在数据工程项目中，shutil 可以帮助：

+   高效地在不同存储位置之间移动或复制大型数据集

+   自动清理处理数据后的临时文件和目录

+   在处理或分析之前创建关键数据集的备份

[shutil: 终极 Python 文件管理工具包](https://www.youtube.com/watch?v=sXzezIK0d7c) 是一个关于 shutil 的全面教程。

## 4\. csv

[csv](https://docs.python.org/3/library/csv.html) 模块对于处理 CSV 文件至关重要，CSV 是一种常见的数据存储和交换格式。它提供了读取和写入 CSV 文件的工具，并且可以自定义处理不同 CSV 格式的选项。

这里有一些你可以使用 csv 模块完成的任务：

+   作为 ETL 管道的一部分解析和处理大型 CSV 文件

+   将 CSV 数据转换为其他格式，例如 JSON 或数据库表

+   将处理或转换的数据写回 CSV 格式以供下游应用程序使用

[CSV 模块 - 如何读取、解析和写入 CSV 文件](https://www.youtube.com/watch?v=q5uM4VKywbA) 是使用 csv 模块的好参考。

## 5\. json

内置的 [json](https://docs.python.org/3/library/json.html) 模块是处理 JSON 数据的首选——在处理 Web 服务和 API 时很常见。它允许你将 Python 对象序列化和反序列化为 JSON 字符串，使你的应用程序和外部系统之间的数据交换变得简单。

你将使用 json 模块来：

+   无缝地将 API 响应转换为 Python 对象以进行进一步处理

+   以结构化格式存储配置信息或元数据

+   处理在大数据应用中常见的复杂嵌套数据结构

[使用 json 模块处理 JSON 数据](https://www.youtube.com/watch?v=9N6a-VLBa2I) 将帮助你学习如何在 Python 中处理 JSON。

## 6\. pickle

[pickle](https://docs.python.org/3/library/pickle.html) 模块用于将 Python 对象序列化和反序列化为二进制格式。它特别适合于将复杂的数据结构，如列表、字典或自定义对象，保存到磁盘并在需要时重新加载。

pickle 模块适用于以下任务：

+   缓存转换后的数据以加速数据管道中的重复任务

+   持久化训练的模型或数据转换步骤以保证可重复性

+   在处理阶段之间存储和重新加载复杂配置或数据集

[Python Pickle 模块用于保存对象（序列化）](https://www.youtube.com/watch?v=2Tw39kZIbhs) 是一篇简短但有用的教程，讲解了 pickle 模块。

## 7\. sqlite3

[sqlite3](https://docs.python.org/3/library/sqlite3.html) 模块提供了一个简单的接口用于操作 SQLite 数据库，这些数据库轻量且自包含。这个模块非常适合那些需要结构化数据存储但不想使用数据库服务器的项目。

+   在将 ETL 管道扩展到完整的数据库系统之前进行原型设计

+   存储元数据、记录信息或在数据处理过程中保存中间结果

+   快速查询和管理结构化数据，无需设置数据库服务器

[在 Python 中使用 SQLite 数据库的指南](https://www.kdnuggets.com/a-guide-to-working-with-sqlite-databases-in-python) 是一篇全面的教程，帮助你入门 SQLite 数据库的使用。

## 8\. datetime

在处理真实世界数据集时，处理日期和时间是非常常见的。[datetime](https://docs.python.org/3/library/datetime.html) 模块帮助你在应用程序中管理日期和时间数据。

它提供了用于处理日期、时间和时间间隔的工具，并支持日期字符串的格式化和解析，具体包括：

+   解析和格式化日志或事件数据中的时间戳

+   管理日期范围和计算时间间隔，当处理真实世界的数据集时

[Datetime 模块 - 如何处理日期、时间、时间差和时区](https://www.youtube.com/watch?v=eirjjyP2qcQ) 是一篇极好的教程，帮助你学习所有关于 datetime 模块的知识。

## 9\. re

[re](https://docs.python.org/3/library/re.html) 模块提供了强大的工具来处理正则表达式，这对文本处理至关重要。它使你能够根据复杂的模式搜索、匹配和操控字符串，这使其在数据清洗、验证和转换任务中不可或缺。

+   从日志、原始数据或非结构化文本中提取特定模式

+   验证数据格式，如日期、电子邮件或电话号码，在 ETL 过程中

+   清理原始文本数据以便进一步分析

你可以参考 [re 模块 - 如何编写和匹配正则表达式 (Regex)](https://www.youtube.com/watch?v=K8L6KVGG-7o) 来详细学习使用内置的 re 模块。

## 10\. subprocess

[subprocess](https://docs.python.org/3/library/subprocess.html) 模块是一个强大的工具，用于在 Python 脚本中运行 shell 命令和与系统 shell 交互。

这对于自动化系统任务、调用命令行工具或捕获来自外部进程的输出至关重要，例如：

+   自动化执行 shell 脚本或数据处理命令

+   从命令行工具捕获输出以集成到 Python 工作流中

+   组织涉及多个工具和命令的复杂数据处理管道

[使用 Subprocess 模块调用外部命令](https://www.youtube.com/watch?v=2Fp1N6dof0Y) 是一个关于如何开始使用 subprocess 模块的教程。

## 总结

希望你发现这篇关于 Python 内置模块在数据工程中的总结有帮助。

这些模块可以成为你数据工程工具包中的良好补充——提供处理各种任务所需的基本功能，而无需依赖外部库。

如果你对数据工程的 Python 库感兴趣，可以阅读 [7 Python Libraries Every Data Engineer Should Know](https://www.kdnuggets.com/7-python-libraries-every-data-engineer-should-know)。

**[](https://twitter.com/balawc27)**[Bala Priya C](https://www.kdnuggets.com/wp-content/uploads/bala-priya-author-image-update-230821.jpg)**** 是一位来自印度的开发者和技术作家。她喜欢在数学、编程、数据科学和内容创作的交叉点上工作。她的兴趣和专长领域包括 DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编程和咖啡！目前，她正通过撰写教程、操作指南、观点文章等方式，学习并与开发者社区分享她的知识。Bala 还创建了引人入胜的资源概述和编码教程。

### 更多相关主题

+   [7 Python Libraries Every Data Engineer Should Know](https://www.kdnuggets.com/7-python-libraries-every-data-engineer-should-know)

+   [每个机器学习工程师应该具备的 5 种机器学习技能](https://www.kdnuggets.com/2023/03/5-machine-learning-skills-every-machine-learning-engineer-know-2023.html)

+   [每个 AI 工程师应该知道的工具：实用指南](https://www.kdnuggets.com/tools-every-ai-engineer-should-know-a-practical-guide)

+   [KDnuggets 新闻，5 月 25 日：每个…](https://www.kdnuggets.com/2022/n21.html)

+   [每个数据科学家应该了解的 6 种 Python 机器学习工具](https://www.kdnuggets.com/2022/05/6-python-machine-learning-tools-every-data-scientist-know.html)

+   [每个数据科学家应该知道的 10 个 Python 库](https://www.kdnuggets.com/10-python-libraries-every-data-scientist-should-know)
