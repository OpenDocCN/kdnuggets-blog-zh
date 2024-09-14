# Kaggle Kernels 初学者指南：逐步教程

> 原文：[https://www.kdnuggets.com/2019/07/kaggle-kernels-guide-beginners-tutorial.html](https://www.kdnuggets.com/2019/07/kaggle-kernels-guide-beginners-tutorial.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由 [Abdul Majed Raja](https://www.linkedin.com/in/amrrs/)，思科分析师提供**

![](../Images/ec1ca3209400fcee605ce57d486440c7.png)

不久前，我写了一篇标题为“[用 Kaggle Kernels 展示你的数据科学技能](https://towardsdatascience.com/show-off-your-data-science-skills-with-kaggle-kernels-762403618c5?source=post_page---------------------------)”的文章，后来意识到尽管这篇文章很好地阐述了 Kaggle Kernels 如何成为数据科学家的强大作品集，但它并没有解决完全初学者如何开始使用 Kaggle Kernels 的问题。

这是一个试图引导完全初学者并带他们了解 Kaggle Kernels 世界的尝试——让他们能够开始使用。

### 注册 Kaggle — [https://www.kaggle.com/](https://www.kaggle.com/?source=post_page---------------------------)

如果你没有 Kaggle 账户，第一步是注册 Kaggle。你可以使用 Google 账户或 Facebook 账户来创建新的 Kaggle 账户并登录。如果以上都不可行，你可以输入你的电子邮件地址和首选密码来创建新的账户。

![图示](../Images/4bfe5a67d7383bcb283cd5b77b922f23.png)

Kaggle 注册页面

### 登录 Kaggle

如果你已经有账户或者刚刚创建了一个，请点击页面右上角的**登录**按钮来启动登录过程。同样，你将有一个选项使用 Google / Facebook / Yahoo 登录，或者使用你在创建账户时输入的用户名和密码。

![图示](../Images/5fc9c62213e0a74edbd24fe1f9059842.png)

Kaggle 登录/注册界面

**Kaggle 控制面板**

登录后，你将进入 Kaggle 控制面板。（这只是欢迎页面，我不知道该怎么称呼，所以称之为控制面板）。

![](../Images/e26f2e956251de2daa53870361336140.png)

这是你登录后立即看到的首页（如果你是从 [https://www.kaggle.com/](https://www.kaggle.com/?source=post_page---------------------------) 登录的）。它包含许多组件，其中一些如下：

+   最近更新或由 Kaggle 推荐的 Kaggle Kernels 的信息流

+   个人资料摘要（右侧边栏的最上方）

+   职位广告（右侧边栏）

+   你的比赛（右侧边栏—向下滚动后）

+   你的 Kernels（右侧边栏—向下滚动后）

接下来我们要去的是导航栏顶部的*Kernels*按钮。

### Kaggle Kernels 列表（最热）：

一旦我们点击 Kaggle 旅程中任何位置的顶部 Kernels 按钮，我们将会看到这个屏幕。

![](../Images/ec1ca3209400fcee605ce57d486440c7.png)

这是每个人都希望看到的界面，因为它像是 Kernels 的首页，这意味着你的 Kernel 更有可能获得更多的曝光。如果 Kernel 最终出现在这里，默认的排序方式是**热门度**，这是基于 Kaggle 秘密算法的，旨在展示相关的 Kernels，但它也提供了其他排序选项，如新建、最多票数等。Kaggle 也使用这个页面来宣传是否有 Kernel 比赛正在进行或即将进行。

在这里，Kernel 比赛是 Kaggle 的一个比赛，但由于比赛的性质（输出是 Kaggle Kernel，并且通常侧重于讲故事），它不属于比赛层级。Data Science for Good 是一系列 Kernel 比赛，数据科学家 / Kaggle 用户需要使用数据科学帮助解决社会问题。为了更好地理解，你可以查看 Kernel Grandmaster [**Shivam Bansal**](https://www.kaggle.com/shivamb?source=post_page---------------------------) 的 Kernels，他已经多次赢得这些比赛。

### Kaggle Kernels — 新建 / 创建：

现在我们已经理解了 Kaggle Kernels 的概念，我们可以直接进入新建 Kernels 的过程。创建 Kaggle Kernel 主要有两种方式：

1.  从 Kaggle Kernels（首页）使用新建 Kernel 按钮

1.  从数据集页面使用新建 Kernel 按钮

**方法 #1：从 Kaggle Kernels（首页）使用新建 Kernel 按钮**

正如上面的截图所示，从 Kernels 页面点击新建 Kernel 按钮将允许你创建一个新的 Kernel。如果你想练习自己的内容或计划输入自己的数据集，这个方法是不错的。如果你想为 Kaggle 上已存在的数据集创建 Kernel，我个人不建议使用这种方法。

**方法 #2：从数据集页面使用新建 Kernel 按钮**

这是我用来创建新 Kernels 最常用的方法之一。你可以打开你感兴趣的数据集页面（如下面截图所示），然后点击其中的新建 Kernel 按钮。这个方法的好处是，与方法 #1 不同，方法 #2 创建的 Kernel 默认会附带 Kaggle 数据集，因此将数据集输入到你的 Kernel 的过程变得更简单、更快速、更直接。

![图示](../Images/feb3c8c89c886374f55aeeebef15d5d7.png)

[https://www.kaggle.com/google/google-landmarks-dataset](https://www.kaggle.com/google/google-landmarks-dataset?source=post_page---------------------------)

### Kaggle Kernels — Kernel 类型：

无论是方法 #1 还是方法 #2，一旦点击新建 Kernel，你将看到一个模态窗口，让你选择要创建的 Kaggle Kernel 类型。

![](../Images/6e44346ba74c5ee83bb3f1421f96c772.png)

大致分为两类 — 1. 脚本 vs 2. 笔记本。

正如我们所知，Notebook（基于单元格的布局）就是 Jupyter Notebook，而脚本则是你可能会在 Pycharm、Sublime Text 或 RStudio 中编写的代码。此外，对于 R 用户而言，脚本是 RMarkdown 的 Kernel 类型——一种通过编程方式生成报告的优美方式。

总结一下 Kernels 的类型：

+   **脚本**

    * Python

    * R

    * RMarkdown

+   **Notebook**

    * **Python** *****R

### Kaggle Kernels — Kernel 语言：

这种第二层的 Kernel 语言选择仅在第一层 Kernel 类型选择之后发生。

[![Kaggle GIF](../Images/dac2340054c09301983a11957a702b70.png)](https://miro.medium.com/max/700/1*jLJA3ONLFjFuLaZ_21ozug.gif)

如上面的 Kaggle Kernel 脚本类型 GIF 所示，通过进入设置并选择所需语言（R / Py / RMarkdown）可以更改 Kernel 的语言。相同的设置还提供了将 Kernel 分享设为公开的选项（默认情况下是私密的，除非设置为公开）。如果你正在进行大学作业或自学并且不希望公开代码，通常使用私密 Kernels。参与竞赛的 Kagglers 也会使用私密 Kernels 来利用 Kaggle 的计算能力，但不透露他们的代码/方法。

### Notebook Kernel：

类似于上面的 GIF，其中选择了 Kernel 类型 *Script*，你也可以选择 *Notebook* 来创建一个 Notebook Kernel。

### RMarkdown Kernel —（Kernel 类型：脚本 > RMarkdown）

RMarkdown 通过结合 R 和 Markdown 生成包含交互式可视化的分析报告。虽然这是解释 RMarkdown 的最简单方法，但它的用途和潜力远远超出了这个定义。

幸运的是，Kaggle Kernel 脚本支持 Rmarkdown，这意味着它可以帮助创建交互式文档以及许多 Notebook 情境下无法实现的功能。这是一个在 Kaggle Kernel 上构建的[a full-fledged Interactive Dashboard](https://www.kaggle.com/tavoosi/suicide-data-full-interactive-dashboard?source=post_page---------------------------)，由[Saba Tavoosi,](https://www.kaggle.com/tavoosi/suicide-data-full-interactive-dashboard?source=post_page---------------------------)展示了 Kaggle Kernels 的潜力，不仅用于构建机器学习模型，还可以进行最佳形式的交互式讲故事。如果你有兴趣学习[如何使用 flexdashboard 构建仪表板](https://bit.ly/2S2LSGS?source=post_page---------------------------)，可以查看[这门课程](https://bit.ly/2S2LSGS?source=post_page---------------------------)。

[![Figure](../Images/09795fd2acd60072afbdc49ec1404eb2.png)](https://miro.medium.com/max/700/1*u2lBm_oWelB7yWojaoeS4A.gif)

Kernel Courtesy: Saba Tavoosi

### 复制和编辑（以前称为 Forking）

类似于 Github 的 **Fork** 选项，如果你想使用现有的 Kaggle Kernel 并在自己的空间中进行修改或增添个人风格，你需要使用右上角的蓝色按钮 `复制并编辑`。事实上，在 Kaggle 竞争跟踪中的许多高分公共 Kernel 通常是 `forks of forks forks`，即一个 Kaggle 用户会在另一个 Kaggle 用户已经构建的模型上进行改进，并将其作为公共 Kernel 提供。

![图示](../Images/cef79b8c9df61716908fc6da58b45410.png)

标记的符号表示一个 Forked / 复制并编辑的 Kernel

### 公开 / 私有 Kernel

正如我们在另一部分看到的，Kaggle Kernel 的访问设置可以是公开或私有。公开 Kernel（显然名字就说明了）对所有人（包括 Kagglers 和非 Kagglers）可用并且可见。私有 Kernel 仅对所有者（创建者）和所有者分享 Kernel 的人可用。一个公开的 Kernel 也可以基于私有数据集。例如，如果这是一个机器学习竞赛，你用一些第三方数据进行了特征工程，并且不希望在竞赛期间泄露数据。这是 Kagglers 通常将数据集设为私有，而将 Kernel 设为公开的典型场景，以便其他人可以看到他们的方法并从中学习。

![](../Images/14e29e8a1640ef18942b1183eb28c510.png)

上图展示了如何将现有 Kernel 的访问设置更改为私有或公开。所有新创建的 Kernel 默认都是私有的（截至本文撰写时），如果需要，所有者可以将其更改为公开。

### TL;DR — 如何创建一个新的 Kaggle Kernel

如果上述内容初看起来有些难以理解，这一部分将帮助你创建第一个 Kaggle Kernel。

![](../Images/d761c65343f670e62e3237cd6f823180.png)

步骤：

1.  使用你的凭证登录 Kaggle

1.  访问任何公开的 Kaggle 数据集

1.  点击右上角的 **新建 Kernel**（蓝色按钮）

1.  选择你感兴趣的 Notebook/Script

1.  如果 Python 是你的首选语言，就保持不变；如果是 R，那么请前往右侧的 **设置** 并点击展开项，在语言旁边你会看到 **Python**，点击它可以更改为 **R**

1.  前往屏幕的编辑器部分 / 面板（左侧），开始编写你的精彩代码（上面的 GIF 也展示了如何使用你创建 Kernel 的数据集）

1.  一旦你的代码完成，点击右上角的 **提交**（蓝色按钮）

1.  如果你的 Kernel 执行成功（没有任何错误），请将你的 Kernel 设为公开（可以通过编辑 Kernel 的 **设置 > 共享（公开）** 或重新打开 Kernel 并点击顶部的 **访问** 按钮）

1.  目前，你的第一个 Kaggle Kernel 应该准备好与网络中的朋友分享了！

查看这个[Kaggle 视频](https://www.kaggle.com/static/video/homepage_landingvideo.mp4?source=post_page---------------------------)获取帮助。

### **完**

对于许多 Kagglers 来说，竞赛轨道是他们的乐趣所在，但[对我来说](https://www.kaggle.com/nulldata?source=post_page---------------------------)，Kaggle Kernels 轨道是我的最爱，它为我们提供了从数据准备到数据可视化——机器学习建模到讲故事的*全栈数据科学旅程*的巨大潜力。希望你也会喜欢。祝你在 Kaggle Kernel 旅程中好运。

*查看我的*[*Kaggle Kernels 在我的 Kaggle 个人资料中*](https://www.kaggle.com/nulldata/kernels?source=post_page---------------------------)*并在*[*我的 LinkedIn 个人资料*](https://www.linkedin.com/in/amrrs/?source=post_page---------------------------)*上与我分享你的反馈。这个教程中使用的视频/GIF/截图可在*[*我的 GitHub*](https://github.com/amrrs/kaggle-kernel-guide-rsc?source=post_page---------------------------)*上找到。*

**简历：** [AbdulMajedRaja](https://www.linkedin.com/in/amrrs/) 是 Cisco 的分析师。

[原文](https://towardsdatascience.com/kaggle-kernels-for-beginners-a-step-by-step-guide-3db6b1cd7606)。经许可转载。

**相关：**

+   [展示你的数据科学技能与 Kaggle Kernels](/2019/06/data-science-kaggle-kernels.html)

+   [如何制作令人惊叹的 3D 图以提升讲故事效果](/2019/07/stunning-3d-plots-better-storytelling.html)

+   [使用 Coindeskr 和 Shiny 在 R 中构建每日比特币价格追踪器](/2018/02/bitcoin-price-tracker-coindeskr-shiny-r.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 更多相关内容

+   [逐步教程：构建你的第一个机器学习模型](https://www.kdnuggets.com/step-by-step-tutorial-to-building-your-first-machine-learning-model)

+   [部署机器学习模型：逐步教程](https://www.kdnuggets.com/deploying-machine-learning-models-a-step-by-step-tutorial)

+   [7 个免费 Kaggle 微课程供数据科学初学者使用](https://www.kdnuggets.com/7-free-kaggle-micro-courses-for-data-science-beginners)

+   [如何成为数据科学家的指南（逐步方法）](https://www.kdnuggets.com/2021/05/guide-become-data-scientist.html)

+   [如何构建数据科学项目：逐步指南](https://www.kdnuggets.com/2022/05/structure-data-science-project-stepbystep-guide.html)

+   [使用 Python 和 Beautiful Soup 进行网页抓取的分步指南](https://www.kdnuggets.com/2023/04/stepbystep-guide-web-scraping-python-beautiful-soup.html)
