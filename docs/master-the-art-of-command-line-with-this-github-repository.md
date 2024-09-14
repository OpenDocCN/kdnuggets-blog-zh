# 掌握命令行艺术与此 GitHub 仓库

> 原文：[https://www.kdnuggets.com/master-the-art-of-command-line-with-this-github-repository](https://www.kdnuggets.com/master-the-art-of-command-line-with-this-github-repository)

![掌握命令行艺术与此 GitHub 仓库](../Images/58068d9a378bb6aebcb101d8f3d35526.png)

作者提供的图片

作为一名处理数据的专业人士，我理解在工作中高效和准确的重要性。这就是为什么我认为掌握命令行是简化数据分析任务和提高生产力的必备技能。对于希望优化操作系统使用和自动化各种任务的普通用户来说，这也同样重要。

* * *

## 我们的前三课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

在这篇博客中，我们将回顾一个在 GitHub 上广受欢迎的（144k ?）单页指南。该指南旨在为你提供可以提升工作流程的基本命令行技能。

# 什么是命令行？

命令行（CLI），也称为终端或控制台，是一种基于文本的界面，允许用户通过输入命令与计算机操作系统互动。它提供了图形用户界面（GUIs）的替代方案，并提供了一种更直接、更精确的方式来访问和操作文件、目录和系统资源。

![掌握命令行艺术与此 GitHub 仓库](../Images/62f1dd6a8dbbde568c8a8cd90b4a2bdb.png)

作者提供的截图

用户可以在终端中输入命令，以精确和自动化的方式执行任务，例如脚本编写、软件开发、数据处理和系统管理。终端允许用户通过一个命令执行多个复杂操作。

# 为什么这个指南很重要？

[掌握命令行艺术](https://github.com/jlevy/the-art-of-command-line)是一个可以显著提升你工作效率和计算机系统理解的过程。无论你是初学者还是有经验的用户，命令行都提供了一种强大的方式来导航、定制和自动化计算机上的任务。

对于数据科学家来说尤其有益。通过命令行，数据专业人士可以简化数据清洗、执行数据管道、自动化数据相关任务，并使用各种命令行工具进行测试和模型开发。

![掌握命令行的艺术，尽在这个GitHub仓库](../Images/99f85ed404fb9f84f85ab0aeef79901a.png)

来自[jlevy/the-art-of-command-line](https://github.com/jlevy/the-art-of-command-line?tab=readme-ov-file#meta)的截图

本指南旨在在一页内提供基本的命令行知识，重点是Linux，但也包括适用于macOS和Windows用户的工具。它涵盖了基本命令、文件和数据处理、系统调试，以及仅在Mac和Windows上可用的命令。由于各种作者和翻译者的贡献，该指南提供了多种语言版本。

> **语言：** Čeština ∙ Deutsch ∙ Ελληνικά ∙ English ∙ Español ∙ Français ∙ Indonesia ∙ Italiano ∙ 日本語 ∙ 한국어 ∙ polski ∙ Português ∙ Română ∙ Русский ∙ Slovenščina ∙ Українська ∙ 简体中文 ∙ 繁體中文

# 指南内容

本指南的范围广泛而简洁，旨在涵盖所有重要内容，提供具体示例，避免不必要的细节。它设计用于交互式Bash使用，但许多提示也适用于其他Shell和Bash脚本。

## 基础知识

学习基本的Bash命令，并理解它们的文档`man <command>`，以及至少精通一种基于文本的编辑器（如Vim、Emacs、nano），以便进行高效的终端编辑。此外，还要了解文件和输出操作，包括重定向（>、<、|）和文件模式匹配。

## 日常使用

为了高效的命令补全和历史记录，分别使用Tab和Ctrl-R。要导航和管理文件，了解使用ls、cd、ln、chmod和chown进行目录导航。

## 文件和数据处理

学习使用文本处理工具：grep、awk、sed、cut、sort、uniq和wc。对于文件搜索，学习使用find和locate来定位文件和目录。

## 系统调试

熟悉系统监控和调试工具，如top、ps、netstat、dmesg和iotop。使用strace、ltrace和系统日志进行性能分析和问题诊断。

## 一行命令

一行命令是执行复杂任务的强大命令序列。示例包括排序和计数文本文件中的出现次数、批量重命名和系统监控。

批量重命名脚本，将目录中的所有.txt文件更改为.md：

```py
**for** file **in** *.txt; do mv "$file" "${file%.txt}.md"; done
```

## 既冷僻又有用

专用命令如expr、cal、yes、env和printenv在特定场景下提供有用的功能。

## 仅限macOS

Mac用户可以使用如Homebrew这样的独特工具进行软件包管理，pbcopy和pbpaste用于剪贴板交互，以及特定的文件和系统实用程序（mdfind、mdls）。

## 仅限Windows

Windows用户可以转向Cygwin、Windows Subsystem for Linux（WSL）或MinGW，以获得类Unix命令行环境。工具如wmic、ipconfig和PowerShell脚本扩展了Windows上的命令行功能。

## 有趣的命令

使用诸如curl、egrep、tr和cowsay等工具，你可以创造性地获取、处理和显示信息，展示你手中的力量和灵活性。

# 结论

-   本指南是一个有用的速查表，帮助您了解新的 CLI 工具及其在各种场景中的应用。它在积极维护中，您甚至可以通过创建拉取请求来贡献您的力量。 [Master The Art Of Command Line](https://github.com/jlevy/the-art-of-command-line) 指南由社区制作并服务于社区，因此如果您发现任何错误或学到了一些新的遗漏内容，请更新主要的 README.md 文件。

-   我希望您能从本指南中了解新的工具和实用程序，并将其应用于您的项目。根据我的经验，我在数据项目中使用了比实际 Python 代码更多的命令行工具，特别是如果您是数据工程师或 MLOps 工程师的话。

## 进一步阅读

+   [5 个更多的数据科学命令行工具](/2023/03/5-command-line-tools-data-science.html)

+   [如何在命令行中清理文本数据](/2020/12/clean-text-data-command-line.html)

+   [数据科学命令行：免费电子书](/2022/03/data-science-command-line-free-ebook.html) **（必读）**

-   [**Abid Ali Awan**](https://www.polywork.com/kingabzpro)（[@1abidaliawan](https://www.linkedin.com/in/1abidaliawan)）是一位认证的数据科学专业人士，热爱构建机器学习模型。目前，他专注于内容创作和撰写有关机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是利用图神经网络为在精神健康方面挣扎的学生开发 AI 产品。

### 相关话题

+   [数据科学命令行：免费电子书](https://www.kdnuggets.com/2022/03/data-science-command-line-free-ebook.html)

+   [5 个更多的数据科学命令行工具](https://www.kdnuggets.com/2023/03/5-command-line-tools-data-science.html)

+   [ChatGPT CLI：将您的命令行界面转换为 ChatGPT](https://www.kdnuggets.com/2023/07/chatgpt-cli-transform-commandline-interface-chatgpt.html)

+   [用 Python 在 7 个简单步骤中构建命令行应用](https://www.kdnuggets.com/build-a-command-line-app-with-python-in-7-easy-steps)

+   [掌握数据讲故事的 7 个步骤](https://www.kdnuggets.com/7-steps-to-master-the-art-of-data-storytelling)

+   [10 个 GitHub 仓库，助你精通机器学习](https://www.kdnuggets.com/10-github-repositories-to-master-machine-learning)
