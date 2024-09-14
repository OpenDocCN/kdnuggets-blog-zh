# 针对数据科学家的 GitHub Desktop

> 原文：[https://www.kdnuggets.com/2021/09/github-desktop-data-scientists.html](https://www.kdnuggets.com/2021/09/github-desktop-data-scientists.html)

[评论](#comments)

**作者 [Drew Seewald](https://realdrewdata.medium.com/)，数据科学家**

版本控制对代码协作、与他人共享代码、查看旧版本代码甚至自动部署代码都非常重要。刚开始时可能会有些困惑，但绝对值得花时间学习，特别是如果你在开源领域工作或在一个团队中频繁使用版本控制来处理项目。以下是一些使其值得使用的主要功能：

+   存储文件更改历史记录及注释

+   组织多个用户同时编辑同一个项目

+   促进代码审查程序

+   自动化工作流程以报告问题、请求改进和部署代码

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你在 IT 领域的职业生涯

* * *

![放轻松，你不必使用命令行](../Images/5f68391a85d401b6be5755e36c39b571.png)

放轻松，你不必使用命令行

图片由 [Dennis van Dalen](https://unsplash.com/@dennisvandalen?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 版本控制功能

版本控制的主要功能之一是每个文件的更改历史记录。这作为每个文件的变更日志，始终可以查看过去任何时刻运行的代码。每当有人更新文件并将新版本推送到仓库时，他们必须添加一个简短的注释。在理想情况下，这将详细说明更改内容和原因。如果对某项更改的原因有疑问，责任人将被标记在提交中，并附上他们提供的额外信息。

版本控制的另一个功能是能够创建分支。分支是一个新版本的代码，保持独立。这对于修改和测试代码非常有用，因为它不会改变主分支，主分支是最更新的工作版本。不同的用户也可以使用这些分支同时处理不同的代码或功能。这些分支可以在准备好时合并回主分支，合并时有一个过程来解决它们之间的差异。

代码审查是与团队合作时的最佳实践。一个人可能在一个新的分支上完成所有新功能的工作，但在盲目地将其合并到主分支之前，应该由团队进行审查。当创建一个拉取请求以将代码移动到主分支时，它也会启动一个讨论，团队成员可以讨论代码并请求更改，然后再将其合并到主分支。这一过程有助于改善进入生产的代码，以防止出现错误和问题，提高代码的效率，甚至使其符合代码格式的标准。

使用 GitHub 进行版本控制的另一个值得注意的好处是它提供的自动化选项。如果有标准的代码审查清单，它可以作为模板添加，当创建拉取请求时，模板将可用，准备在审查任务完成时填写。模板也可以在创建问题时使用，以确保人们记得输入所有必要的细节。GitHub 还提供了支持自动化的操作。这些操作可以由不同的事件触发，例如将代码合并到主分支中。操作可以运行单元测试、构建/编译包组件，甚至将代码部署到生产环境中。

## 版本控制类型

你可能听说过几个著名的版本控制工具。最受欢迎的包括 Git 和 GitHub。 [Git](https://git-scm.com/about) 是版本控制的基础技术，而 [GitHub](https://github.com/features) 是简化版本控制工作流程的软件。

Git 可以在本地使用，无需外部代码库。你可以在计算机的硬盘上完成所有版本控制任务。本地 Git 仓库非常适合个人项目或当你还不准备与整个团队分享你的代码时，但仍希望享受版本控制的好处。

GitHub 网站是一个存储代码的代码库。许多开源项目，如 Python 和 R 包，都托管在 GitHub 网站上。对于公共代码库，任何人都可以查看修订历史、包中的问题以及相关文档。

要连接到 GitHub 网站上的代码库，我们可以使用 Git 或 GitHub Desktop。对于喜欢命令行界面的用户， [Rebecca Vickery](https://rebecca-vickery.medium.com/) 有一篇关于 [使用 Git CLI 进行数据科学](https://towardsdatascience.com/introduction-to-github-for-data-scientists-2cf8b9b25fba)的很棒的文章。那么你为什么要继续阅读呢？**命令行可能会让人感到不安**。希望使用图形用户界面 (GUI) 来管理你的版本控制是完全没问题的。GitHub Desktop 提供了一个清晰且简单的界面来处理你的代码库。

## GitHub Desktop 处理流程

虽然每个人对其代码库的处理流程可能会有所不同，但在 GitHub 上更改代码有几个一般步骤：

1.  创建一个分支

1.  添加提交

1.  创建新的拉取请求

1.  完成代码审查

1.  合并拉取请求

创建一个分支会复制当前的生产代码。开发者会对文件进行更改，并将更改提交到新的分支。接下来，拉取请求会开启讨论，将新分支的更改添加到生产代码中，通常是在主分支或主分支中。代码审阅者可以添加评论并要求澄清拉取请求中所做的更改。一旦审查完成并且做出任何必要的更改，拉取请求可以被合并到主分支中并关闭。

让我们更详细地探讨这些步骤，看看如何使用 GitHub Desktop 完成每一个步骤。

## 创建分支（将新代码与旧代码分开）

要进行更改，首先创建一个新分支。如果你有对仓库的完全访问权限，你可以直接在仓库的 GitHub 网站上创建新分支。

1a. 点击分支：main

![](../Images/7a2f5a3a8e875300d915e791ef68f11e.png)

图片由作者提供

2a. 在文本框中输入新分支的名称。可能需要考虑你所在组织的分支命名规范，以保持组织的条理。

![](../Images/b4b06ee888375539073d6da2d0995beb.png)

图片由作者提供

3a. 点击创建分支

![](../Images/bfc6ec4d3df0e70160c5b667546635f3.png)

图片由作者提供

现在将选择新的分支。

![](../Images/9fc334b1ae45f64ce831e9fab827279a.png)

图片由作者提供

如果你没有对仓库的完全访问权限，这在公共项目中很常见，你需要 fork 该仓库。新的分支和 fork 是同义的。一个 fork 会在新的仓库中创建，而不是在生产代码的同一个仓库中，通常在你的个人资料下创建。要进行 fork：

1b. 在右上角，点击 Fork

![](../Images/aabe81e12567cf1fc0e97073fa2cb706.png)

图片由作者提供

2b. 等待文件复制完成

![](../Images/630c695aff8621eea6cd0b9d1599a985.png)

图片由作者提供

3b. 将选择新的 fork

![](../Images/d1d7c3809adeb2a04cf0988553e5c71d.png)

图片由作者提供

## 添加提交（增强代码/添加功能）

要对代码进行提交，你需要将仓库克隆到你的本地计算机。这会复制代码供你工作，然后再将更新发送回仓库。要将仓库克隆到本地计算机：

点击 clone 或 download。

![](../Images/5ceaf9ad18aad68cac50c1fc848e7a60.png)

图片由作者提供

点击用 GitHub Desktop 打开。

![](../Images/07acf88809c00832c3162bfa8664ba7e.png)

图片由作者提供

如果你没有 GitHub Desktop，请点击下载 GitHub Desktop。

![](../Images/c8c6cca6e38fcf5a1b904b1cefa5c627.png)

图片由作者提供

GitHub Desktop 会询问将仓库克隆到本地计算机的哪个位置。这是本地路径字段。

![](../Images/6bf04bf406e9f8bafbc1a50272752676.png)

图片由作者提供

点击分支并选择新创建的分支。这将更新你本地计算机上的文件，使其与该分支上的更新一致，并将其设为添加提交的活动分支。

![](../Images/f3202aa31e053e9bd96cb480bc43801a.png)

作者提供的图片

要进行更改，请打开你在克隆时选择的目录，并使用你的文本编辑器或集成开发环境（IDE）进行更改。保存文件。

返回 GitHub Desktop。GitHub Desktop 会不断扫描仓库文件夹树，并会看到你所做的任何更改。这些更改会显示在左侧窗格中。右侧窗格将预览选定文件的更改（某些文件类型不会预览）。

![](../Images/e9843042959d6dcb82ac53305d44bf32.png)

作者提供的图片

每次你进行一组相关更改时，都要将这些更改提交到你的仓库。记得为提交添加评论，以便人们可以轻松识别出所做的更改。上面的文本框用于快速描述，如果你有更多关于提交的备注，请将它们放在较大的描述文本框中。

在进行更改并添加评论后，提交更改。

![](../Images/1ea3c17098321267a72fa0ba6c5f0e09.png)

作者提供的图片

提交更改仅将它们保存到本地文件。要将更改推送回 GitHub 服务器，请点击推送 origin。如果有未推送到服务器的提交，右侧窗格上会出现一条消息，显示**推送 xx 次提交到 origin 远程**。origin 只是一个表示从中克隆了仓库的名称。

![](../Images/9cc725a65a3afc5a37efc05a0025d4be.png)

作者提供的图片

## 打开拉取请求

导航到 GitHub 服务器上的你的仓库。确保你在正确的分支上。如果你在原始仓库中创建了新分支，请导航到该分支。如果你必须 fork 仓库，请导航到你个人资料上的仓库。

在**拉取请求**标签下，点击**新建拉取请求**。

![](../Images/8a3f66c066514f4951860cd8817ced77.png)

作者提供的图片

选择新分支作为比较的分支，然后**点击创建拉取请求**。在我们的案例中，拉取请求会自动填充我们的提交评论。

![](../Images/3a5b0d3de61eeca6143b9f3fa5622809.png)

作者提供的图片

## 代码审查

代码审查有助于确保我们添加或更改的代码是正确的，并已被多个人审查和批准。无论你是否有权限访问仓库，你总是应该让审查者检查更改。如果有任何问题，请作为团队进行讨论。

拉取请求会显示在仓库的拉取请求标签下。每个拉取请求都有对话、提交和文件更改标签。

对话区是人们可以添加关于代码的问题或评论的地方。你可以格式化你的评论，并在评论中标记人员和问题。

提交显示拉取请求中所有的提交和评论

更改的文件显示了哪些文件被更改、添加或删除，并提供了逐行的代码比较（如果有的话）

![](../Images/fd0fd1d02f26feb7ac4e6ea318ca4942.png)

作者提供的图片

## 合并拉取请求

在我的情况下，我在新分支上工作时，主分支发生了更改。这就是为什么会有冲突需要解决的消息。点击“解决冲突”按钮会打开一个编辑器。它将显示每个分支的文件版本，允许你删除一个并保留另一个，或者创建两个版本的组合。

![](../Images/fabb8f794787fcb4b1cabde497d343aa.png)

作者提供的图片

在这种情况下，来自主分支的版本是正确的。可以删除其他分支的更改和分隔符。可以将冲突标记为已解决，并提交合并。

![](../Images/fbac7272763c6caff22817bfa1a2f691.png)

作者提供的图片

解决了代码审查和冲突之后，可以合并拉取请求。再次会有一个选项来评论合并完成的内容。合并后，代码将成为主分支或主分支的一部分！

![](../Images/38cde93dd454a07a5b3ecbf7da8fd8d5.png)

作者提供的图片

就这样，你现在知道如何使用 GitHub 和 GitHub Desktop 完成最基本的版本控制任务了！

*我写关于数据科学、分析和编程概念的文章。你可以在*[*Medium*](https://realdrewdata.medium.com/lists)*、*[*Twitter*](https://twitter.com/RealDrewData)* 和 *[*LinkedIn*](https://www.linkedin.com/in/realdrewdata/)* 上与我联系。*

## 进一步阅读

[**理解 GitHub 工作流**](https://guides.github.com/introduction/flow/)

当你在项目中工作时，你会有一堆不同的功能或想法在进行中…

[**使用 Git 的版本控制**](https://swcarpentry.github.io/git-novice/)

Wolfman 和 Dracula 被 Universal Missions（一个来自 Euphoric State University 的空间服务公司）雇佣来…

[**什么是版本控制 | Atlassian Git 教程**](https://www.atlassian.com/git/tutorials/what-is-version-control)

版本控制，也称为源代码控制，是跟踪和管理软件代码更改的实践…

**简介：[Drew Seewald](https://realdrewdata.medium.com/)** 是梅赛德斯-奔驰金融服务公司的数据科学家。可以在推特上关注Drew [@RealDrewData](https://twitter.com/realdrewdata?lang=en) 或在 [LinkedIn](https://www.linkedin.com/in/realdrewdata/) 上联系他。

[原文](https://towardsdatascience.com/github-desktop-for-data-scientists-b9d8a3afc5ea)。经允许转载。

**相关：**

+   [GitHub Copilot 开源替代品](/2021/07/github-copilot-open-source-alternatives-code-generation.html)

+   [3 个数据获取、标注和增强工具](/2021/08/3-data-labeling-synthesizing-augmentation-tools.html)

+   [GitHub Copilot 和编程自动化中 AI 语言模型的崛起](/2021/09/github-copilot-rise-ai-language-models-programming-automation.html)

### 更多相关话题

+   [从这些 GitHub 仓库学习数据科学](https://www.kdnuggets.com/2022/12/learn-data-science-github-repositories.html)

+   [从这些 GitHub 仓库学习数据工程](https://www.kdnuggets.com/2023/02/learn-data-engineering-github-repositories.html)

+   [数据科学 GitHub CLI 备忘单](https://www.kdnuggets.com/2023/03/github-cli-data-science-cheat-sheet.html)

+   [优化数据分析：在 Databricks 中集成 GitHub Copilot](https://www.kdnuggets.com/optimizing-data-analytics-integrating-github-copilot-in-databricks)

+   [数据科学项目中 GitHub 的 5 个最佳替代方案](https://www.kdnuggets.com/the-top-5-alternatives-to-github-for-data-science-projects)

+   [掌握数据工程的 10 个 GitHub 仓库](https://www.kdnuggets.com/10-github-repositories-to-master-data-engineering)
