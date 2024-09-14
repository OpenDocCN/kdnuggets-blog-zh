# 12 个 VSCode 的 Python 开发技巧和窍门

> 原文：[https://www.kdnuggets.com/2023/05/12-vscode-tips-tricks-python-development.html](https://www.kdnuggets.com/2023/05/12-vscode-tips-tricks-python-development.html)

![12 个 VSCode 的 Python 开发技巧和窍门](../Images/d56491937200bf93c18bde6c0c7a9966.png)

图片由作者提供

[Visual Studio Code](https://code.visualstudio.com/) (VSCode) 是一个流行的 Python 开发集成开发环境 (IDE)。它快速且拥有丰富的功能，使开发体验变得有趣且轻松。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

VSCode 的 Python 扩展是我将其用于所有工作相关任务的主要原因之一。它为你提供了语法自动补全、代码检查、单元测试、Git、调试、笔记本、编辑工具，并能够自动化大多数任务。你可以通过按键盘快捷键或点击几个按钮来代替手动操作。

在这篇文章中，我们将学习如何将 VSCode 提升到一个新水平，并提高在构建 Python 软件和解决方案方面的生产力。

> **注意：** 如果你是 VSCode 的新手，并且想学习所有基础知识，请阅读 [设置 VSCode 以进行 Python 开发](https://www.datacamp.com/tutorial/setting-up-vscode-python) 教程，以了解关键功能。

# 1\. 命令行

你可以通过 **终端** 或 **Bash** 使用 CLI 命令启动 VSCode。

1.  在当前目录中打开 VSCode: `code .`

1.  在最近使用的窗口中，在当前目录中打开 VSCode: `code -r .`

1.  创建一个新窗口: `code -n`

1.  打开文件差异编辑器 VSCode: `code --diff <file1> <file2>`

# 2\. 命令面板

根据当前上下文访问所有可用的命令和快捷键。你可以通过使用键盘快捷键来启动命令面板：**Ctrl+Shift+P**。之后，你可以输入相关的关键词以访问特定命令。

![12 个 VSCode 的 Python 开发技巧和窍门](../Images/6691a56d9145e6d6205ccdd4c1e1e215.png)

图片由作者提供

# 3\. 键盘快捷键

什么比命令面板更好呢？键盘快捷键。你可以根据需要修改键盘快捷键，或者通过阅读 [键盘快捷键](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf) 参考表来了解默认的键盘快捷键。

键盘快捷键将帮助我们直接访问命令，而不是滚动浏览命令面板选项。

# 4\. 错误和警告

通过使用键盘快捷键**Ctrl+Shift+M**快速访问错误和警告，并通过点击警告或按**F8**或**Shift+F8**键在它们之间循环。

![12 VSCode技巧与窍门](../Images/df565eb6396d6d31e05fe0b77709f091.png)

图片由作者提供

# 5\. 完全可定制的开发环境

你可以自定义主题、图标、键盘快捷键、调试设置、字体、代码检查和代码片段。VSCode是一个完全可定制的开发环境，允许你甚至创建自己的扩展。

# 6\. 扩展

Python的VSCode扩展可以提升开发体验，并使你更高效。这不仅仅关乎生产力，还关乎视觉效果。大多数流行的Python扩展在[Visual Studio Marketplace](https://marketplace.visualstudio.com/vscode)上提供带有统计数据和图表的互动GUI。

![12 VSCode技巧与窍门](../Images/9e46e9562e4c069d8614ba670a58be0f.png)

图片由作者提供

查看我列出的[12个数据科学必备VSCode扩展](/2022/07/12-essential-vscode-extensions-data-science.html)，这些扩展会使VSCode成为一个超级应用，你可以在不离开应用的情况下执行所有数据科学任务。

# 7\. Jupyter Notebook

让你进行数据分析和机器学习实验的最重要的扩展是[Jupyter Notebook](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter)扩展。

![12 VSCode技巧与窍门](../Images/08a72d7072aa5adc56e54164aadf3797.png)

图片由作者提供

这个扩展被高度推荐给数据科学家，用于执行数据科学实验和构建生产级代码。

# 8\. 多光标选择

多光标选择在你需要对同一实例进行多个编辑时是一个救命工具。

+   使用**Alt+Click**添加多个光标点

+   要将光标设置在上方，请使用**Ctrl+Alt+Up**，设置在下方请使用**Ctrl+Alt+Down**

+   使用**Ctrl+Shift+L**将额外的光标添加到当前选择的所有出现位置

![12 VSCode技巧与窍门](../Images/bd7771da69ea36c2c2de21e1ce3298fe.png)

图片来自[Visual Studio Code](https://code.visualstudio.com/docs/getstarted/tips-and-tricks#_editing-hacks)

# 9\. 搜索和修改

我知道这是一个简单的功能，但当你在文件中的不同位置编辑类似的变量、参数和参数时，它非常方便。你可以逐一搜索和替换它们，也可以一次性替换所有。

要重命名符号或参数，请选择该符号并按下**F2**键。

![12 VSCode技巧与窍门](../Images/e2ca942c060646b77dbc6cc6f7a194d7.png)

图片由作者提供

# 10\. 内置Git集成

这是一种内置集成，允许你通过点击几个按钮来执行所有与 Git 相关的任务，而不是在 CLI 中输入 Git 命令。你可以通过与用户友好的 GUI 互动来可视化历史记录、查看差异并创建新分支。这甚至比 GitHub Desktop 应用程序还要简单。

![12 个 Python 开发的 VSCode 提示与技巧](../Images/d5baf03cb92e6ea30262df84fa0d0b88.png)

图片由作者提供

# 11\. 代码片段

代码片段就像自动补全，但你可以对其进行更多控制。你可以为重复的代码模式创建自定义代码片段。你可以输入一个单词，它将自动填充其余部分，而不是创建一个 Python 函数。

要创建自定义代码片段，请选择 **文件** > **首选项** > **配置用户代码片段**，然后选择语言。

![12 个 Python 开发的 VSCode 提示与技巧](../Images/4aa463f8d0cbd5c05123b729ce6d611b.png)

图片由作者提供

# 12\. GitHub Copilot

每个人都在谈论 ChatGPT 的代码建议，但 [GitHub Copilot](https://github.com/features/copilot) 已经存在了两年多，它在理解用户行为和帮助他们快速高效地编写代码方面越来越出色。GitHub Copilot 基于 GPT-3，通过建议代码行或整个函数来提升开发体验。

![12 个 Python 开发的 VSCode 提示与技巧](../Images/194ddde6045ff3760d349ac8b8c67652.png)

图片来自 [GitHub Copilot](https://github.com/features/copilot)

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan)) 是一位认证的数据科学专业人士，热衷于构建机器学习模型。目前，他专注于内容创作和撰写关于机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是利用图神经网络构建一个 AI 产品，帮助那些在心理健康方面挣扎的学生。

### 更多相关内容

+   [KDnuggets 新闻，7月6日：12 个必备的数据科学 VSCode…](https://www.kdnuggets.com/2022/n27.html)

+   [12 个必备的 VSCode 扩展插件](https://www.kdnuggets.com/2022/07/12-essential-vscode-extensions-data-science.html)

+   [数据科学领域 VSCode 的 7 个最佳替代品](https://www.kdnuggets.com/top-7-alternatives-to-vscode-for-data-science)

+   [10 个 Jupyter Notebook 的提示和技巧](https://www.kdnuggets.com/2023/06/10-jupyter-notebook-tips-tricks-data-scientists.html)

+   [快速学习 SAS 的数据科学提示和技巧](https://www.kdnuggets.com/2022/05/sas-quick-data-science-tips-tricks-learn.html)

+   [在 Heroku 云上部署深度学习 Web 应用的技巧和窍门](https://www.kdnuggets.com/2021/12/tips-tricks-deploying-dl-webapps-heroku.html)
