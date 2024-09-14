# 数据科学的软件工程技巧与最佳实践

> 原文：[https://www.kdnuggets.com/2020/10/software-engineering-best-practices-data-science.html](https://www.kdnuggets.com/2020/10/software-engineering-best-practices-data-science.html)

[评论](#comments)

**作者 [Ahmed Besbes](https://ahmedbesbes.com)，AI 工程师 // 博主 // 跑者**。

![](../Images/78dbf50aeffb2874715d1afcd7beab53.png)

* * *

## 我们的前 3 名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

如果你对数据科学感兴趣，那么你可能对这种工作流程很熟悉：你通过启动一个 Jupyter notebook 开始一个项目，然后开始编写 Python 代码，进行复杂分析，甚至训练模型。随着 notebook 文件中函数、类、图表和日志的增多，你会发现自己面前有一大块单一的代码块。如果你运气好，事情可能会顺利。那就祝贺你了！

然而，Jupyter notebooks 隐藏了一些严重的问题，这些问题可能会让你的编码变成一场噩梦。让我们看看这些问题是如何出现的，然后讨论防止这些问题的编码最佳实践。

### Jupyter Notebook 的问题

![](../Images/c6d5b3b1ae9a15554525c6807fa87a01.png)

*来源: [datascience.foundation](https://datascience.foundation/datatalk/setting-up-a-python-jupyter-notebook-online-working-with-python-on-the-cloud)。*

很多时候，如果你想将你的 Jupyter 原型开发提升到一个新的水平，事情可能不会如你所愿。这些是我在使用该工具时遇到的一些情况，你可能也会感到熟悉：

+   由于所有对象（函数或类）都在一个地方定义和实例化，**维护性变得非常困难**：即使你只想对某个函数做一个小改动，你也必须在 notebook 中找到它，修复它并重新运行整个代码。你不希望这样，相信我。把你的逻辑和处理函数分离到外部脚本中不是更简单吗？

+   由于其互动性和即时反馈，jupyter笔记本推动数据科学家在全局命名空间中声明变量，而不是使用函数。这在python开发中被认为是[不良实践](https://stackoverflow.com/questions/19158339/why-are-global-variables-evil/19158418#19158418)，因为它**限制了有效的代码重用。**它也有害于可重复性，因为你的笔记本变成了一个包含所有变量的大型状态机。在这种配置下，你必须记住哪个结果被缓存了，哪个没有，同时还要期望其他用户遵循你的单元格执行顺序。

+   笔记本的格式在后台是JSON对象，这使得**代码版本控制变得困难。**这就是为什么我很少看到数据科学家使用GIT提交笔记本的不同版本或合并特定功能的分支。因此，团队合作变得低效且笨拙：团队成员开始通过电子邮件或Slack交换代码片段和笔记本，回滚到代码的前一个版本是一场噩梦，文件组织也开始变得混乱。这是我常见的情况，在使用jupyter笔记本没有适当版本控制的两三周后：

    ****analysis.**ipynb** **analysis_COPY(1).ipynb

    analysis_COPY(2).ipynb

    analysis_FINAL.ipynb

    analysis_FINAL_2.ipynb**

+   Jupyter笔记本适合探索和快速原型设计。它们显然不是为了可重用性或生产用途而设计的。如果你用jupyter笔记本开发了一个数据处理管道，你最多只能说你的代码仅在你的笔记本电脑或虚拟机上以线性同步的方式按单元格的执行顺序工作。这并没有说明你的代码在更复杂的环境下如何表现，比如更大的输入数据集、其他异步并行任务或较少的资源分配。

    **笔记本实际上很难测试，因为它们的行为有时是不可预测的。**

+   作为一个大部分时间都在VSCode中利用强大扩展功能进行代码[linting](https://marketplace.visualstudio.com/items?itemName=ms-python.python)、样式[格式化](https://prettier.io/)、代码结构、自动补全和代码库搜索的人，我在切换回jupyter时不禁感到有些无力。

    **与VSCode相比，jupyter notebook缺乏强制编码最佳实践的扩展。**

好了，各位，暂时别再吐槽了。我真的很喜欢jupyter，我认为它在设计上做得很好。你绝对可以用它来启动小项目或快速原型设计。

但为了以工业化的方式推出这些想法，你必须遵循一些在数据科学家使用笔记本时可能会丢失的软件工程原则。让我们一起回顾其中的一些，看看它们为什么重要。

### 使你的代码再次出色的技巧

**这些建议来源于不同的项目、我参加的会议，以及与我过去合作的软件工程师和架构师的讨论。如果你有其他建议和想法，欢迎在评论区分享，我会在帖子中给予你的答案以信用。**

**以下部分假设我们正在编写 Python 脚本，而不是笔记本。**

### 1 - 清理你的代码

![](../Images/da7e4c6d843c484a8b08c17bfbdc3a2c.png)

*照片由 [Florian Olivo](https://unsplash.com/@florianolv?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供。*

代码质量的一个重要方面是清晰性。清晰和可读的代码对协作和维护至关重要。

以下是可能帮助你保持代码更清晰的建议：

+   使用**有意义的变量名**，它们应具有描述性并暗示类型。例如，如果你要声明一个关于某个属性（例如年龄）的布尔变量，以检查一个人是否年长，你可以通过使用有描述性和类型信息的命名来使其更具解释性。声明数据时也是如此：使其具有解释性。

```py
# not good ... 
import pandas as pd
df = pd.read_csv(path)

# better!
transactions = pd.read_csv(path)

```

+   **避免使用只有你自己能理解的缩写**和**没有人能忍受的长变量名**。

+   **不要在代码中硬编码“魔法数字”**。将它们定义在变量中，以便每个人都能理解它们指的是什么。

```py
# not good ...
optimizer = SGD(0.0045, momentum=True)

# better !
learning_rate = 0.0045
optimizer = SGD(learning_rate, momentum=True)

```

+   **在命名对象时遵循 PEP8 约定**：例如，函数和方法的名称使用小写字母，单词之间用下划线分隔；类名称遵循 UpperCaseCamelCase 约定；常量全大写等。

    了解更多关于这些约定的信息，[点击这里](https://visualgit.readthedocs.io/en/latest/pages/naming_convention.html)。

+   **使用缩进和空白**让你的代码有呼吸的空间。存在一些标准约定，比如“每个缩进使用 4 个空格”、“不同部分之间应有额外的空行”等……由于我总是记不住这些，我使用了一个非常好的**VSCode 插件 [**prettier**](https://prettier.io/)**，它可以在按下 ctrl+s 时自动重新格式化我的代码**。

![](../Images/cc1aada2ec1307d7795092311db43747.png)

*来源：[https://prettier.io/](https://prettier.io/)。*

### 2 - 使你的代码模块化

当你开始构建一些你认为可以在相同或其他项目中重用的东西时，你需要将代码组织成逻辑函数和模块。这有助于更好的组织和维护。

例如，你正在进行一个 NLP 项目，你可能有不同的处理函数来处理文本数据（如分词、去除 URL、词形还原等）。你可以将这些单元放在一个名为 text_processing.py 的 Python 模块中，并从中导入它们。这样，你的主程序会轻得多！

这些是我学习到的一些编写模块化代码的好建议：

+   **DRY: 不要重复自己。** 尽可能地将代码通用化和整合。

+   **函数应该做一件事**。如果一个函数执行多个操作，就会变得更难以概括。

+   **将你的逻辑抽象到函数中，但** **不要过度工程化**：你可能会遇到模块过多的情况。根据你的判断，如果你经验不足，可以查看流行的GitHub库，例如 [scikit-learn](https://github.com/scikit-learn/scikit-learn) 并查看他们的编码风格。

### 3 - 重构你的代码

重构旨在重新组织代码的内部结构，而不改变其功能。通常在一个有效（但尚未完全组织）的代码版本上进行。它有助于去重函数，重组文件结构，并增加更多抽象。

要了解更多关于Python重构的内容，这篇文章是一个很好的 [resource](https://realpython.com/python-refactoring/)。

### 4 - 提高代码效率

编写高效的代码，使其执行迅速且占用更少的内存和存储是软件开发中的另一项重要技能。

编写高效代码需要多年的经验，但这里有一些快速提示，可能会帮助你发现代码运行缓慢的原因以及如何提升性能：

+   在运行任何代码之前，检查算法的复杂性以评估其执行时间

+   通过检查每个操作的运行时间来检查脚本的可能瓶颈

+   尽量避免使用for循环，并向量化你的操作，特别是如果你使用像 [NumPy](https://numpy.org/) 或 [pandas](https://pandas.pydata.org/)这样的库

+   通过使用多进程来利用机器的CPU核心

### 5 - 使用GIT或任何其他版本控制系统

根据我的个人经验，使用GIT + Github帮助我提高了编码技能并更好地组织了我的项目。由于我在与朋友和/或同事合作时使用了它，这使我遵守了过去没有遵守的标准。

![](../Images/414a1e5ca6eb074cff85a8e61606fdb3.png)

*来源: [freecodecamp](https://www.freecodecamp.org/news/the-beginners-guide-to-git-github/)。*

使用版本控制系统有很多好处，无论是在数据科学还是软件开发中。

+   跟踪你的更改

+   回滚到任何先前的代码版本

+   通过合并和拉取请求实现团队成员之间的高效协作

+   提高代码质量

+   代码审查

+   为团队成员分配任务并监控他们的进展

像Github或Gitlab这样的平台甚至提供了持续集成和持续交付钩子，用于自动构建和部署你的项目。

如果你是Git的新手，那么我推荐查看这个 [tutorial](https://nvie.com/posts/a-successful-git-branching-model/)。或者你可以查看这个备忘单：

![](../Images/85a170b7c85c43de0628582251eb0eac.png)

*来源: [Atlassian](https://www.atlassian.com/git/tutorials/atlassian-git-cheatsheet)。*

如果你想专门了解如何为机器学习模型进行版本控制，可以查看这篇[文章](https://algorithmia.com/blog/how-to-version-control-your-production-machine-learning-models)。

### 6 - 测试你的代码

如果你在构建一个执行一系列操作的数据管道，确保它按设计执行的一种方法是编写**测试**来检查预期的行为。

测试可以简单到检查输出形状或函数返回的预期值。

![](../Images/14b4af8b9d46b3df6fa8c98dbc2053b5.png)

*[https://pytest-c-testrunner.readthedocs.io/](https://pytest-c-testrunner.readthedocs.io/)*

为你的函数和模块编写测试带来了许多好处：

+   它提高了代码的稳定性，并使错误更容易被发现。

+   它防止了意外输出。

+   它有助于检测边界情况。

+   它防止了将损坏的代码推送到生产环境中。

### 7 - 使用日志记录

一旦你的代码的第一个版本运行起来，你肯定会希望在每一步进行监控，以了解发生了什么、跟踪进展或发现故障行为。这就是你可以使用日志记录的地方。

以下是高效使用日志记录的一些技巧：

+   根据你想记录的消息的性质，使用不同的级别（调试、信息、警告）。

+   在日志中提供有用的信息，以帮助解决相关问题。

```py
import logging

logging.basicConfig(filename='example.log',level=logging.DEBUG)
logging.debug('This message should go to the log file')
logging.info('So should this')
logging.warning('And this, too')

```

![](../Images/de411838c7bf41eeb639cad1aa115d46.png)

*来源: [realpython](https://realpython.com/python-logging-source-code/)。*

### 结论

数据科学家通过生成与公司系统和基础设施不相关的报告和jupyter笔记本来解决问题的时代早已过去。如今，数据科学家开始生成可测试和可运行的代码，与IT系统无缝集成。**因此，遵循软件工程最佳实践成为了必需。**

[原文](https://medium.com/swlh/software-engineering-tips-and-best-practices-for-data-science-5d85dbcf87fd)。经许可转载。

**简介:** [Ahmed Besbes](https://ahmedbesbes.com)是一位居住在法国的数据科学家，工作领域涵盖金融服务、媒体和公共部门。Ahmed的工作包括设计、构建和部署AI应用程序以解决业务问题。Ahmed还博客分享技术话题，如深度学习。

**相关：**

+   [将机器学习模型投入生产的5大最佳实践](https://www.kdnuggets.com/2020/10/5-best-practices-machine-learning-models-production.html)

+   [数据科学家的软件工程基础](https://www.kdnuggets.com/2020/06/software-engineering-fundamentals-data-scientists.html)

+   [数据科学家的编码习惯](https://www.kdnuggets.com/2020/05/coding-habits-data-scientists.html)

### 更多相关话题

+   [停止学习数据科学以寻找目的，并找到目的……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的5个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [每位数据科学家都应该知道的三个R语言库（即使你使用Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [一个90亿美元的人工智能失败案例分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [是什么让Python成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)
