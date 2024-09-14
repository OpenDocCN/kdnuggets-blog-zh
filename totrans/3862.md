# GitHub Copilot：你的 AI 编程助手——究竟有何亮点？

> 原文：[https://www.kdnuggets.com/2021/07/github-copilot-ai-pair-programmer.html](https://www.kdnuggets.com/2021/07/github-copilot-ai-pair-programmer.html)

[评论](#comments)

上周，GitHub 公开发布了 **[Copilot](https://copilot.github.com/)**，这是其“AI 编程助手”的预览版，是一种在 IDE 中提供行或函数建议的代码补全工具。它无疑在编程及其他领域引起了轰动，你可能已经听说过一些关于它的事情。

但 Copilot 不仅仅是简单的自动补全，它比其他代码助手更具上下文意识。由 OpenAI 的 Codex AI 系统驱动，Copilot 通过 docstrings、函数名、评论和前面的代码来 contextualize 情境，以生成和建议最合适的代码。Copilot 旨在随着时间的推移不断改进，“学习”开发者的使用方式。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT。

* * *

> 在数十亿行公共代码上训练过的 GitHub Copilot 将你需要的知识触手可及，节省时间并帮助你保持专注。

目前可用于 Visual Studio Code 及由 VS Code 后端驱动的平台——如 GitHub 的 Codespaces——Copilot 能“理解”多种语言，技术预览版在“特别适用于 Python、JavaScript、TypeScript、Ruby 和 Go”方面表现突出。你可以接受默认的代码建议，浏览其他提案，修改你接受的代码，或完全忽略 Copilot 在你代码中的建议。

Copilot 的一些主要卖点，如 Copilot 网站上的突出介绍所示，包括以下内容（直接摘自 [网站](https://copilot.github.com/)）：

+   **将评论转换为代码**。编写描述你想要逻辑的评论，让 GitHub Copilot 为你组装代码。

+   **自动填充重复代码**。GitHub Copilot 非常适合快速生成样板和重复的代码模式。给它几个示例，让它生成剩余的部分！

+   **无烦恼的测试**。测试是任何稳健软件工程项目的核心。导入单元测试包，让 GitHub Copilot 提供与实现代码匹配的测试建议。

+   **显示替代方案**。想要评估几种不同的方法？GitHub Copilot可以为你显示解决方案列表。使用提供的代码，或编辑以满足你的需求。

![Figure](../Images/7b441802bcb88227d4b76e19b1095eb1.png)

Copilot的工作原理 ([source](https://copilot.github.com/))

这些都是令人印象深刻的用例和主张，并且从提供的示例来看，它们也是令人印象深刻的结果。以下是Copilot将其视为上下文来生成代码的一个示例，由函数名称和详细的文档字符串组合而成：

```py
import datetime

def parse_expenses(expenses_string):
    """Parse the list of expenses and return the list of triples (date, value, currency).
    Ignore lines starting with #.
    Parse the date using datetime.
    Example expenses_string:
        2016-01-02 -34.01 USD
        2016-01-03 2.59 DKK
        2016-01-03 -2.72 EUR
    """
```

以下是Copilot生成的函数：

```py
    expenses = []
    for line in expenses_string.splitlines():
        if line.startswith("#"):
            continue
        date, value, currency = line.split(" ")
        expenses.append((datetime.datetime.strptime(date, "%Y-%m-%d"),
                         float(value),
                         currency))
    return expenses
```

这确实如描述的那样运作。

我已经申请了Copilot，但截至出版时，尚未获得访问权限。因此，我们目前只能依赖于其他地方的阅读来形成我们的初步印象。

最让我兴奋的是Copilot在“从注释到代码”上下文中的应用，尤其是与函数文档字符串相关的部分。作为一名优秀的程序员，你应该在规划好函数功能并实现之前，为你的函数编写高质量的文档字符串（我当然也是如此！）。借助Copilot，我们应该能够将更多时间投入到规划和文档编写阶段，或许可以跳过实现阶段的初始步骤，专注于之后对生成代码的调整。

这里是另一个简单却强大的例子，一个注释变成代码块的示例（来自[GitHub文档中对Copilot的分析](https://docs.github.com/en/github/copilot/research-recitation)，我鼓励你阅读）：

![Image](../Images/1a0ab0a2b1827cc6ff857d8bf4204d3a.png)

我最近写了[一篇关于数据科学家如何组织代码的文章](/2021/06/managing-reusable-python-code-data-scientist.html)，但我担心它可能很快需要更新，因为我预计Copilot可能会在某种程度上改变我写代码的方式。

Copilot页面上还有一些推荐信，来自已经使用该系统一段时间的GitHub和OpenAI开发人员，等等。这里有来自你可能听说过的人的强烈推荐：

> 在第一天，GitHub Copilot已经教会了我Javascript对象比较的细微差别，对我们的数据库架构的熟悉程度和我一样。这是我见过的最令人震惊的机器学习应用。
> 
> — Mike Krieger // Instagram 联合创始人

目前来看，Copilot似乎是机器学习的一个非常令人印象深刻的应用。更值得注意的是，它不是理论上的或不切实际的；Copilot立即有用，并有望对编码方式产生实际影响。

相信我，当我说我真的很期待使用Copilot时，我是认真的，并承诺在使用后会分享一些来自我亲身体验的观察。

**相关**：

+   [作为数据科学家，如何管理你的可重用 Python 代码](/2021/06/managing-reusable-python-code-data-scientist.html)

+   [数据科学家，你需要知道如何编码](/2021/06/data-scientists-need-know-code.html)

+   [用 Python 自动化的 5 个任务](/2021/06/5-tasks-automate-python.html)

### 更多相关主题

+   [GitHub Copilot 开源替代方案](https://www.kdnuggets.com/2021/07/github-copilot-open-source-alternatives-code-generation.html)

+   [优化数据分析：在 Databricks 中集成 GitHub Copilot](https://www.kdnuggets.com/optimizing-data-analytics-integrating-github-copilot-in-databricks)

+   [每个程序员都应该了解的 11 个 Python 魔法方法](https://www.kdnuggets.com/11-python-magic-methods-every-programmer-should-know)

+   [从这些 GitHub 仓库学习数据科学](https://www.kdnuggets.com/2022/12/learn-data-science-github-repositories.html)

+   [从这些 GitHub 仓库学习数据工程](https://www.kdnuggets.com/2023/02/learn-data-engineering-github-repositories.html)

+   [数据科学 GitHub CLI 备忘单](https://www.kdnuggets.com/2023/03/github-cli-data-science-cheat-sheet.html)
