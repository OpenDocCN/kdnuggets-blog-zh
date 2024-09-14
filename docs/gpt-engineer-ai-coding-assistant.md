# GPT-Engineer：你的全新 AI 编程助手

> 原文：[`www.kdnuggets.com/2023/07/gpt-engineer-ai-coding-assistant.html`](https://www.kdnuggets.com/2023/07/gpt-engineer-ai-coding-assistant.html)

![](img/6b94e9e2a86562b8155251594d79954d.png)

由作者使用 Midjourney 创建

# 引言

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 维护

* * *

拥有一个永不疲倦、全天候工作并能从单一项目描述生成整个代码库的编程伙伴岂不是太棒了？这就是名为**GPT-Engineer**的新项目的目标，它是众多新兴的 AI 驱动编码工具之一。它是一个利用 GPT 力量帮助你构建应用程序的 AI 驱动应用构建器。由 Anton Osika 开发的 GPT-Engineer 是 AI 辅助开发领域的一个突破。你可以查看 [GPT-Engineer GitHub 仓库](https://github.com/AntonOsika/gpt-engineer) 来了解它的实际操作。

GPT-Engineer 不仅仅是另一个 AI 工具；它是你的编程助手。无论是构建一个简单的应用程序还是更复杂的项目，例如游戏，GPT-Engineer 都可以提供帮助。它就像有一个初级开发者在你身边，提出澄清问题以确保完全理解你的项目需求。

# 启动并运行 GPT-Engineer

在你开始使用 GPT-Engineer 编程之前，你需要安装它。为此，从 [项目的 GitHub 页面](https://github.com/AntonOsika/gpt-engineer) 克隆 GPT-Engineer 仓库。接下来，创建一个新的 Python 环境，并安装必要的模块。有关详细的步骤指南，请访问 [GPT-Engineer GitHub 页面](https://github.com/AntonOsika/gpt-engineer)。

# 利用 GPT-4 进行设计文档提示

GPT-Engineer 的提示是基于设计文档的输入。但如何起草一个有用的设计文档呢？何不使用 GPT-4 来帮助创建它们？！

让我们使用 GPT-4 生成详细的项目设计描述，然后将其输入到 GPT-Engineer 中。这样，你可以确保你的 AI 编程伙伴拥有构建应用程序所需的所有信息。

作为示例，让我们用 GPT-4 创建一个设计文档来提示 GPT-Engineer。实际上，让 ChatGPT 生成一个有用的设计文档并不需要太多的工作——尽管它们**可以**更加复杂，正如你下面将看到的。以下是我们的提示：

> 我想用 Python 实现一个经典规则和玩法的蛇游戏。它可以使用外部库来实现。生成这个项目的设计文档。

以下是生成的设计文档的前几行，我可以向你保证，总体上很好地概述了需要完成的内容。

> 介绍
> 
> 本设计文档的目的是概述用 Python 创建经典蛇游戏的实现细节。游戏将遵循传统规则和玩法，玩家控制一条通过吃食物而变长的蛇，同时避免与自己和游戏边界碰撞。游戏将使用外部库来实现图形和用户输入。
> 
> 游戏组件
> 
> 蛇游戏将由以下组件组成：
> 
> a) 蛇：一个可移动的对象，当它吃到食物时会变长，如果它与自己或游戏边界碰撞则游戏失败。
> 
> b) 食物：一个随机放置的对象，蛇可以吃掉它来增加长度和分数。
> 
> ...

现在我们有了一个设计文档的提示，我们可以将其与 GPT-Engineer 一起使用。

# 使用 ChatGPT 构建设计文档

GPT-Engineer 的一个伟大之处在于其多功能性。它可以实现一个简单的蛇游戏，也可以帮助你构建各种应用程序，包括仅使用 Python 的标准库模块构建简单的键值数据库。只需在 `main.prompt` 文件中描述项目，GPT-Engineer 会询问你任何必要的澄清。一旦你提供了详细信息，坐下来等待 GPT-Engineer 生成你的代码库。

首先，让我们使用 ChatGPT-4 生成设计文档。请记住，你不必使用 ChatGPT 来完成这项工作。如果你有一个详细的软件描述，你可以使用它。但是为了将你的设计笔记全面转化为 GPT-Engineer 容易处理的内容，这是一个合理的路线。

这是作者使用 ChatGPT-4 生成设计文档的提示：

> 生成一个高质量的设计文档，以便我们的项目团队可以用来设计下面描述的软件。它应该完整且可用，作为唯一的设计文档以完成我们的项目。
> 
> ---
> 
> 使用仅标准库模块（以下提到的模块除外）编写的简单键值数据库引擎。
> 
> 当数据库引擎运行时，基本的交互应通过导入的 Python 模块进行，该模块具有清晰简洁的库 API。
> 
> 还应该有一个单独的控制台可以运行，作为与数据库引擎和数据库交互的命令行 REPL。
> 
> 额外的交互方式应包括一个使用 Fast API（外部库）创建的 API。
> 
> 设计规格：
> 
> 键值数据库是一种 NoSQL 数据库，以键值对的集合形式存储数据。与传统的关系型数据库不同，键值数据库不强制使用固定模式，为某些用例提供了灵活性和可扩展性。设计键值数据库时需要考虑几个重要的规格。
> 
> 应该可以访问的命令：
> 
> - 连接到数据库
> 
> - 删除数据库
> 
> - 获取键
> 
> - 插入数据
> 
> - 更新数据
> 
> - 删除数据
> 
> - 查找键和值的存在性
> 
> 数据模型：每条记录由一个唯一的键和一个关联的值组成。值可以是简单的标量值，也可以是复杂的数据结构（如 JSON 或 XML）。
> 
> 键结构：键作为每个数据条目的主要标识符，必须在数据库中唯一。键可以是字母数字字符串、整数，甚至是复合键（多个值组合形成的单个键）。
> 
> 值结构：与每个键关联的值可以是任何数据类型。它可以是基本数据类型（如字符串、数字、布尔值），也可以是更复杂的结构，如嵌套对象或数组。
> 
> 性能和缓存：应使用内存缓存技术来加快读操作速度和减少延迟。

这是 ChatGPT-4 生成的设计文档：

> ## 概述
> ## 
> 本项目的主要目标是设计和开发一个轻量级的键值数据库引擎，使用 Python 编写，仅使用标准库模块，并使用 FastAPI 作为 HTTP 接口。该应用程序将提供键值数据库的基本功能，包括连接数据库、数据插入、更新、删除和检索，以及键和值的搜索功能。
> 
> 数据库将通过三种方法进行接口：
> 
> 1\. 一个可以被其他 Python 程序导入的 Python 模块。
> 
> 2\. 一个用于手动交互的命令行 REPL 控制台。
> 
> 3\. 一个使用 FastAPI 创建的用于远程交互的 API。
> 
> ## 组件
> ## 
> 数据库引擎将具有以下主要组件：
> 
> 1\. **数据存储**：键值对的实际存储位置。作为一个简单的 Python 实现，这可以是一个字典，其中键是唯一标识符，值可以是任何数据类型。数据存储应具有高效的方法来访问、添加、更新和删除数据。
> 
> 2\. **数据库引擎**：提供与数据存储交互的方法的引擎。这将包括连接数据库、插入、更新和删除数据的方法，以及通过键获取值的方法。它还应包括一个检查数据存储中某个特定键或值是否存在的方法。
> 
> 3\. **内存缓存**：一个缓存层，用于保存频繁访问的数据，以提高读取速度和减少延迟。这可以通过 Python 的 `functools.lru_cache` 装饰器或自定义缓存解决方案来实现。
> 
> 4\. **CLI**: 一个用于与数据库引擎手动交互的命令行接口。将使用 Python 标准库中的 `cmd` 或 `argparse` 模块来创建一个 REPL 界面。
> 
> 5\. **HTTP API**: 一个 FastAPI 应用，将数据库引擎方法暴露为 HTTP API。这将允许远程与数据库进行交互。
> 
> ## 数据模型
> ## 
> 数据库引擎将遵循一个简单的键值数据模型。数据库中的每条记录将由一个唯一的键和一个关联的值组成。
> 
> ### 键结构
> ### 
> 键可以是字母数字字符串、整数或复合键。它们在数据库中必须是唯一的，并将作为每个数据条目的主要标识符。
> 
> ### 值结构
> ### 
> 每个键关联的值可以是任何 Python 数据类型。它可以是基本数据类型（字符串、数字、布尔值），也可以是更复杂的结构，如嵌套对象或数组。
> 
> ## API 设计
> ## 
> Python 库 API、CLI 和 FastAPI 应提供以下功能：
> 
> - `connect(database_name: str)`: 连接到数据库。如果数据库不存在，则创建它。
> 
> - `delete_database(database_name: str)`: 删除一个数据库。
> 
> - `insert(key: str, value: Any)`: 将新的键值对插入数据库中。
> 
> - `update(key: str, value: Any)`: 更新现有键的值。
> 
> - `delete(key: str)`: 从数据库中删除一个键值对。
> 
> - `get(key: str)`: 从数据库中获取键的值。
> 
> - `exists_key(key: str)`: 检查数据库中是否存在某个键。
> 
> - `exists_value(value: Any)`: 检查数据库中是否存在某个值。
> 
> ## 性能和缓存
> ## 
> 内存缓存技术将被用来加速读取操作并减少延迟。可以使用`functools.lru_cache`装饰器或自定义缓存解决方案来实现这一点。缓存应该有一个最大大小以防止过度使用内存。
> 
> ## 结论
> ## 
> 本文档提供了一个使用 Python 和 FastAPI 的简单键值数据库引擎的高级设计。通过遵循这个设计，我们可以确保数据库将具有灵活性和可扩展性，满足各种应用的需求。

就这些了。剩下的唯一任务就是运行 GPT-Engineer 并根据这些标准进行调整。

# 结论

那么接下来怎么办？我们如何解决生成代码中存在的问题？最近，作者一直在使用 GPT-Engineer 配合 Aider，这是另一个功能强大的 GPT 驱动的软件生成项目，尝试调整结果。然而，随着 ChatGPT Code Generator 插件的公开发布，尝试使用它来生成和修复代码似乎是个不错的主意。

总之，GPT-Engineer 不仅仅是一个工具，它是我们构建应用程序的潜在革命。它证明了 AI 和大型语言模型的强大，也许还预示了编码的未来。不论你是经验丰富的开发者还是刚刚起步，GPT-Engineer 都绝对值得一试。

祝~~编码~~提示愉快！

### 更多相关主题

+   [StarCoder：你一直想要的编程助手](https://www.kdnuggets.com/2023/05/starcoder-coding-assistant-always-wanted.html)

+   [揭开 StableCode 的面纱：AI 辅助编程的新视野](https://www.kdnuggets.com/2023/08/unveiling-stablecode-new-horizon-ai-assisted-coding)

+   [忘掉 ChatGPT，这款新的 AI 助手遥遥领先，将…](https://www.kdnuggets.com/2023/08/forget-chatgpt-new-ai-assistant-leagues-ahead-change-way-work-forever.html)

+   [用 Ruff 提升你的 Python 编程风格](https://www.kdnuggets.com/enhance-your-python-coding-style-with-ruff)

+   [停止在数据科学项目中硬编码 - 改用配置文件](https://www.kdnuggets.com/2023/06/stop-hard-coding-data-science-project-config-files-instead.html)

+   [如何回答数据科学编程面试问题](https://www.kdnuggets.com/2022/01/answer-data-science-coding-interview-questions.html)
