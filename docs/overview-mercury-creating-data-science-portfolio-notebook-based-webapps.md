# Mercury 概述：创建数据科学作品集和基于 notebook 的网页应用

> 原文：[https://www.kdnuggets.com/2022/05/overview-mercury-creating-data-science-portfolio-notebook-based-webapps.html](https://www.kdnuggets.com/2022/05/overview-mercury-creating-data-science-portfolio-notebook-based-webapps.html)

![Mercury 概述：创建数据科学作品集和基于 notebook 的网页应用](../Images/d2a151772b8f2208d087518a95f21f0e.png)

图片由作者提供

[Mercury](https://mljar.com/mercury/) 是一个将你的 Python Jupyter notebooks 转换为互动式网页应用并部署到云端的工具。它适用于那些希望创建惊人仪表盘和使用少量 YAML 脚本制作机器学习演示的数据分析师和机器学习工程师。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT 工作

* * *

**Mercury 的关键功能：**

+   使用 YAML 头部添加互动小部件。

+   更改值，执行 notebook 并保存结果。

+   在 Jupyter 单元格中显示或隐藏代码的选项。

+   易于在云服务器上部署。

+   创建并将多个 notebooks 添加到服务器上。

+   作品集首页。

+   管理员控制和身份验证。

+   使用 iframe 嵌入互动网页应用。

+   AGPLv3 下的开源

Mercury 的开源版本包含了诸如多个网页应用、欢迎页面编辑和互动小部件等核心功能。Mercury Pro 版本则具有额外的功能，如团队支持、私人共享、管理员控制和身份验证，还提供专门的支持。

使用 `pip install mljar-mercury` 安装 Mercury 或在终端中使用 `mercury run demo`。要验证安装，请运行演示 `mercury run demo`，然后在浏览器中输入 `http://127.0.0.1` 地址。你也可以在 Mercury 文档中参加一个 [迷你教程](https://mercury-docs.readthedocs.io/en/latest/get-started/)，以了解基本操作。

在本概述中，我们将了解 Mercury 的关键功能以及如何利用它。

# YAML 参数

最重要的是为 Jupyter notebook 添加一个头部。你需要在顶部创建一个新单元格，并将其更改为 Raw `NBConvert`。然后，添加 YAML 代码以修改参数。

![YAML 参数](../Images/89736cc9dce81967d6567e3f4103ca50.png)

来自 [Mercury](https://mljar.com/mercury/) 的 Gif

**YAML 头部中使用的参数列表：**

+   **标题**：是网络应用程序的名称。它显示在侧边栏和主页上。

+   **作者**：网络应用程序的创建者。

+   **描述**：添加网络应用程序的详细描述。

+   **显示代码**：隐藏或显示python代码。

+   **显示提示**：隐藏或显示jupyter单元格的提示信息。

+   **共享**：与公众、私有或特定组共享网络应用程序。

+   **参数**：添加、删除、修改小部件。这是控制应用程序输入和输出的最重要的参数。

# 小部件

小部件是你应用程序的交互式输入。你可以添加文本、整数、滑块、复选框、范围、多个选项，甚至上传文件用于你的机器学习演示。

下面的示例代码演示了如何将一个小部件（滑块）添加到Python变量（my_variable）中。

```py
params:
    my_variable:
        input: slider
        label: This is slider label
        value: 5
        min: 0
        max: 10

```

![小部件](../Images/cbd459c97db006293092557792bbf406.png)

图片由Mercury提供

# 欢迎页面

你可以使用Markdown创建自定义首页。如果问我，这是创建数据科学作品集的最简单方法，其中包括你的简历、成就和通过网络应用程序了解的项目。要创建首页，请添加**welcome.md**文件并使用Markdown添加信息。

要获取灵感，请查看由作者（Piotr Płoński）创建的[数据科学作品集](https://github.com/pplonski/data-science-portfolio)。

![欢迎页面](../Images/af52e05b4af02945259fd59589d2b1a4.png)

图片来自[Mercury文档](https://mercury-docs.readthedocs.io/en/latest/welcome/)

# 嵌入

你也可以通过使用iframe将你的网络应用程序添加到博客或WordPress网站中。只需复制你的网络应用程序的链接，并在iframe脚本中添加“/embed”，如下面所示。

```py
<iframe src="https://mercury.mljar.com/app/5/embed" height="700px" width="1200px"/&gt

```

这个功能很特别，因为没有其他网络应用程序提供嵌入功能。

![嵌入](../Images/e5215096814b5df022ff5a8fdd43faf0.png)

# 管理员和认证

如果你想限制谁可以查看你的笔记本，请将你的应用程序设置为私有。要访问Mercury网络应用程序，你需要提供用户名和密码。你可以在YAML头部的`share`参数中列出用户或组。你可以使用管理面板来管理笔记本、任务、用户和组。添加、删除和更新用户或组需要**专业版**。

![管理员和认证](../Images/bc429c24599de1237b43fd750e2a9b98.png)

Gif由Mercury提供

# Heroku上的部署

对我来说，fork一个仓库并将其部署到[Heroku](https://www.heroku.com/home)服务器上相当简单。你可以查看我的[项目](https://github.com/kingabzpro/dashboard-from-jupyter-with-mercury)，了解我是如何在不写一行代码的情况下部署GitHub仓库的。要访问我部署的网络应用程序，请点击[这里](https://mercury-dashboard-abid.herokuapp.com/)。同样，你也可以轻松地在AWS或Google Cloud上部署你的应用程序。确保你已经添加了环境变量：

+   NOTEBOOKS= *.ipynb

+   PORT= 8080

+   SERVE_STATIC= True

+   ALLOWED_HOST= <your-wep-app-url>

# 结论

Mercury 为 Python Web 应用的工作带来了不同的风味。你可以一次添加多个笔记本，甚至创建你的作品集页面，而不是仅仅创建一个。在这篇博客中，我们学习了如何使用 YAML 头文件创建仪表盘或机器学习演示。Mercury 非常简单，你可以通过 GitHub 集成部署你的应用。此外，你可以添加认证、管理用户、创建欢迎页面、添加多个小部件，并将输出保存为 HTML 文件。

+   **网站**: [从 Python Notebook 构建 Web 应用](https://mljar.com/mercury/)

+   **GitHub**: [mljar/mercury](https://github.com/mljar/mercury)

+   **文档**: [Mercury 文档](https://mercury-docs.readthedocs.io/en/latest/)

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan)) 是一位认证的数据科学专业人士，热衷于构建机器学习模型。目前，他专注于内容创作和撰写关于机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是使用图神经网络构建一个 AI 产品，帮助那些在心理健康方面挣扎的学生。

### 相关主题

+   [数据科学家的 10 个 Jupyter Notebook 小技巧和窍门](https://www.kdnuggets.com/2023/06/10-jupyter-notebook-tips-tricks-data-scientists.html)

+   [Jupyter Notebook 上的 5 个免费数据科学项目模板](https://www.kdnuggets.com/5-free-templates-for-data-science-projects-on-jupyter-notebook)

+   [Python 在金融领域：Jupyter Notebook 中的实时数据流](https://www.kdnuggets.com/python-in-finance-real-time-data-streaming-within-jupyter-notebook)

+   [如何在 Jupyter Notebook 上设置 Julia](https://www.kdnuggets.com/2022/11/setup-julia-jupyter-notebook.html)

+   [Jupyter Notebook 魔法方法速查表](https://www.kdnuggets.com/jupyter-notebook-magic-methods-cheat-sheet)

+   [如何作为初学者建立强大的数据科学作品集](https://www.kdnuggets.com/2021/10/strong-data-science-portfolio-as-beginner.html)
