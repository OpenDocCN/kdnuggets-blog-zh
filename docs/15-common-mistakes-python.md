# 15 个数据科学家在 Python 中常犯的错误（以及如何修复它们）

> 原文：[https://www.kdnuggets.com/2021/03/15-common-mistakes-python.html](https://www.kdnuggets.com/2021/03/15-common-mistakes-python.html)

[评论](#comments)

**由 [Gerold Csendes](https://www.linkedin.com/in/gcsendes/)，EPAM Systems 的数据科学家**。

![](../Images/ec752ba42e144fea772521e9af7c7416.png)

*照片由 [Joshua Aragon](https://unsplash.com/@goshua13?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。*

在我的数据科学职业生涯中，我逐渐意识到，通过应用软件工程的最佳实践，你可以交付更高质量的项目。更高的质量可能意味着更少的 bug、更可靠的结果以及更高的编码生产力。本文并不打算详细介绍这些最佳实践。相反，它总结了我遇到（并且自己也犯过）的最常见错误，并提供了方法、想法和资源，以帮助你最好地解决这些问题。

当你阅读我的文章时，你可能会想：“好吧，当我独自工作时，我并不真的需要遵循这些建议，因为我了解我的代码。” 实际上，至少会有另一个人阅读你的代码：你的未来自己。你目前认为显而易见的东西，几个月后可能会变成完全的胡言乱语。让她的生活更轻松一点，避免以下错误吧。

### 1\. 你并不是在一个孤立的环境中工作

好吧，这可能不完全是一个编码问题，但我仍然认为孤立的环境对于我的代码来说是一个重要的特性。为什么你要考虑为每个项目使用一个专门的环境？你想让你的代码具有可重现性：在你未来的计算机上、在你同事的机器上，以及在生产环境中。曾经遇到过你的同事无法运行你的代码的问题吗？很可能是因为她没有与你相同的依赖项。（或者也许在运行了数百个单元格后，你忘记检查在使用清除的内核时 Notebook 是否会崩溃）。如果你不知道什么是依赖项管理，那么最好从 [Anaconda 虚拟环境](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html) 或 [Pipenv](https://realpython.com/pipenv-guide/) 开始。我个人使用 Anaconda，这里有一个很好的 [教程](https://towardsdatascience.com/a-guide-to-conda-environments-bc6180fc533)，你可以通过点击链接访问。如果你想深入了解，那么 Docker 是你的首选。

### 2\. (过度使用) Jupyter Notebook

Notebook 在教育用途和完成一些快速且粗糙的工作时确实很有用，但它在作为一个良好的 IDE 时却表现不佳。一个好的 IDE 是你处理数据科学任务时的真正武器，并且可以大大提高你的生产力。很多聪明的人都在指出 Notebook 的不足之处。我认为 Joel Grus 的 [演讲](https://www.youtube.com/watch?v=7jiPeIFXb6U) 是最好的，而且非常有趣。

别误解我的意思，笔记本对于实验是很好的，你可以轻松地向同事展示结果。然而，它们非常容易出错，当涉及到长期的、协作的和可部署的项目时，你最好寻找一个真正的IDE，比如VScode、Pycharm、Spyder等。我确实偶尔使用笔记本，但我建立了一个心理模型：只有在项目不超过一天的情况下，我才会使用笔记本。

### 3\. 你没有组织你的代码

数据科学家有一个习惯，就是将所有项目文件堆放在一个目录中。这是一种不好的做法。看看下面的图，想象一下你需要接手同事的一个项目。哪种项目结构会让你在几个小时内尝试弄清楚发生了什么后陷入生存危机？当然，左侧的结构是你的首选。 [Cookiecutter](https://drivendata.github.io/cookiecutter-data-science/) 是一个促进数据科学标准化项目结构的出色倡议。确保查看一下。

![](../Images/71a013d2741c042d3691b309182aa879.png)

*良好与不良的项目结构 — 作者截图。*

### 4\. 绝对路径而非相对路径

是否曾遇到过代码中的评论“pls fix your path”？这样的评论暗示了糟糕的代码设计。修复这个问题包括两个步骤。1) 与你的同事（也许是上述建议的那个）分享项目结构 2) 将你的IDE根目录/工作目录设置为项目根目录，通常是项目中的最外层目录。后者有时可能不那么简单，但绝对值得付出努力，因为你的同事将能够在不修改代码的情况下运行它。

### 5\. 魔法数字

魔法数字是代码中没有上下文的数字。使用魔法数字可能会导致很难追踪的错误。下面的要点清楚地显示了，通过简单地在乘法中使用未赋值的数字，你会失去为什么会发生这种情况的上下文，并且如果你后来需要更改它，会感到相当有压力。因此，建议在Python中使用大写的命名常量。你实际上不必使用大写，这只是一个约定，但区分“常量”和“普通”变量是个好主意。

### 6\. 不处理警告

我们都经历过代码运行时生成奇怪的警告信息的情况。你很高兴你的代码终于运行了，并且得到了有意义的输出。那么为什么要处理这些警告呢？其实，警告本身不是错误，但它们引起对潜在错误或问题的关注。当你的代码运行成功但可能不是按预期的方式运行时，警告会出现。我遇到的最常见的警告是 Pandas 的 SettingWithCopyWarning 和 DeprecationWarning。DataSchool 以一种 [简洁的方式](https://www.youtube.com/watch?v=4R4WsDJ-KVc) 解释了 SettingWithCopyWarning 是如何触发的。DeprecationWarning 通常指出 Pandas 已经弃用了某些功能，并且你的代码在使用较新版本时会出现问题。当然，还有其他几种警告类型，我的经验是，当以非设计的方式使用某些功能时，它们就会出现。理解函数的源代码总是有帮助的。通过这样做，你可以 99% 的时间摆脱这些警告。

### 7\. 你没有使用类型注解

我必须承认，这是我最近才学到的一个实践，但我已经能看到它的好处。类型注解（或类型提示）是一种给变量指定类型的方法。你基本上是用提示来扩展你的代码，这些提示实际上是对你的代码的扩展，指明了变量/参数的类型。这使得你的代码更易于阅读，因为编码者的意图是明确的。为了演示这一点，我从 Daniel Starner 的 [dev.to](https://dev.to/dstarner/using-pythons-type-annotations-4cfe) 上拿了一个例子。没有类型提示的情况下，*mystery_combine()* 可以接受整数和字符串作为输入，并输出整数或字符串。这对其他开发者可能会造成困惑。通过使用类型注解，你可以明确你的意图，让同事的工作变得更轻松。

此外，带有类型注解的代码可以静态（在不实际运行代码的情况下）检查错误。下面的截图显示了前两个参数没有很好地指定。静态检查你的代码是运行前进行预检查的一个好方法。

![](../Images/265435f278090cebd9ed4a563ab16e6e.png)

*作者截图。*

### 8\. 你没有使用（足够的）列表推导式

列表推导式是 Python 的一个非常强大的特性。许多 for 循环可以用更具可读性、更符合 Python 风格并且更快的列表推导式来替代。下面的代码示例旨在读取目录中的 CSV 文件。你可能会说，在这种情况下使用 for 循环并没有错，但试着只检查 CSV 文件（可能还有其他格式的文件如 JSON）。你会发现，当使用列表推导式时，添加这样的功能很容易维护。

### 9\. 你的 pandas 代码不可读

方法链是一项在 pandas 中很棒的功能，但如果你将所有内容表达在一行中，代码可能会变得难以阅读。有一个技巧可以让你将表达式拆分开来。如果你将表达式放入括号中，那么你可以为表达式的每个组件使用单独的一行。这样不是更清晰吗？

### 10\. 你害怕使用日期

日期在 Python 中可能令人畏惧。语法很奇怪，很难理解。我看到的一个常见错误是，人们将日期处理得像数字一样。你可以始终做一些变通处理和编写黑客代码，但这真的容易出错，难以阅读和维护。以下是一个示例，任务是列出两个日期之间的所有月份，格式为 %Y%m。如果你遵循 datetime 实现，你会发现代码变得更加可读和易于维护。就我而言，处理日期仍然需要大量的 Google 搜索，但我已经学会了即使第一次尝试没有找到解决方案，也不要感到畏惧。

### 11\. 你没有使用好的变量名

将你的数据框命名为 df 和 i、j、k 作为循环索引只是没有描述性，并且使你的代码可读性降低。为了保持变量名过短而努力只会让项目中的其他开发者感到困惑。不要害怕为变量使用较长的名称。没有什么阻止你使用更多的‘_’。确保查看 Will Koehrsen 关于此主题的精彩[文章](https://towardsdatascience.com/data-scientists-your-variable-names-are-awful-heres-how-to-fix-them-89053d2855be)，以获得更多见解。

### 12\. 你没有模块化你的代码

模块化意味着将长而复杂的代码拆分成更简单的模块，这些模块执行更小、更具体的任务。不要仅仅为你的项目创建一个长脚本。在代码的顶部定义类或函数是一种不好的做法。这很难维护和阅读。相反，创建模块（包）并根据它们的功能对其进行结构化。你可以访问 realpython.org 的[Python 模块和包](https://realpython.com/courses/python-modules-packages/)教程，以获取深入的介绍。

### 13\. 你不遵循 PEP 规范

当我开始学习 Python 编程时，我最终写出了丑陋且难以阅读的代码，并开始制定自己的设计规则来使代码看起来更好。花了相当多的时间来制定这些规则，而且我经常打破这些规则。然后，我发现了[PEP](https://www.python.org/dev/peps/)，这是 Python 的官方样式指南。我非常喜欢 PEP，因为它通过使你的代码外观标准化来简化协作。顺便说一句，我确实忽略了一些 PEP 规则，但我会说我在 90% 的代码中使用了它们。

任何好的 Python IDE 都可以通过 linter 扩展。下面的图片演示了 linter 在实践中的工作原理。它们指出代码质量问题，如果这对你来说仍然不清楚，你可以查看括号中指示的具体 PEP 索引。如果你想了解有哪些可用的 linter，那么一如既往，[realpython.com](https://realpython.com/python-code-quality/#linters) 是一个关于 Python 的好资源。

![](../Images/b91396aada9f5194acc8e4673d24152b.png)

*由作者截屏。*

### 14\. 你不会使用编码助手

想在编码中获得大幅度的生产力提升吗？开始使用编码助手，它可以通过智能自动完成、打开文档和提供改进代码的建议来帮助你。我喜欢使用 pylance，它是微软开发的一个新工具，并且在 VScode 中可用。Kite 是另一个非常不错的替代工具，也可以在多个编辑器中使用。

查看作者的[这个视频](https://thumbs.gfycat.com/BaggyNiceLemur-mobile.mp4)。

### 15\. 你不会在代码中隐藏秘密

将秘密（密码、密钥）推送到公开的 GitHub 仓库是一个广泛存在的安全隐患。如果你想了解这个问题的严重性，请查看这篇[qz](https://qz.com/674520/companies-are-sharing-their-secret-access-codes-on-github-and-they-may-not-even-know-it/)文章。网络上有机器人在等待你犯这样的错误。就我个人而言，安全性是数据科学课程中几乎从未涉及的话题。所以，你需要自己填补这个空白。我建议你首先使用操作系统环境变量。这个[dev.to](https://dev.to/biplov/handling-passwords-and-secret-keys-using-environment-variables-2ei0)文章可能是一个好的开始。

[原文](https://towardsdatascience.com/15-common-coding-mistakes-data-scientist-make-in-python-and-how-to-fix-them-7760467498af)。经许可转载。

**相关：**

+   [数据科学的软件工程技巧和最佳实践](https://www.kdnuggets.com/2020/10/software-engineering-best-practices-data-science.html)

+   [数据科学家的软件工程基础](https://www.kdnuggets.com/2020/06/software-engineering-fundamentals-data-scientists.html)

+   [数据科学家的编码习惯](https://www.kdnuggets.com/2020/05/coding-habits-data-scientists.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织中的 IT

* * *

### 更多相关话题

+   [10种最常见的数据质量问题及其解决方法](https://www.kdnuggets.com/2022/11/10-common-data-quality-issues-fix.html)

+   [5个常见的数据科学错误及其避免方法](https://www.kdnuggets.com/5-common-data-science-mistakes-and-how-to-avoid-them)

+   [5个常见的Python陷阱（及其避免方法）](https://www.kdnuggets.com/5-common-python-gotchas-and-how-to-avoid-them)

+   [避免这5个每个AI新手都会犯的常见错误](https://www.kdnuggets.com/avoid-these-5-common-mistakes-every-novice-in-ai-makes)

+   [新手数据科学家应避免的错误](https://www.kdnuggets.com/2022/06/mistakes-newbie-data-scientists-avoid.html)

+   [2022年数据科学家的收入有多少？](https://www.kdnuggets.com/2022/02/much-data-scientists-make-2022.html)
