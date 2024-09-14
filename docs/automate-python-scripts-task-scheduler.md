# 使用任务计划程序自动化你的 Python 脚本：Windows 任务计划程序抓取替代数据

> 原文：[https://www.kdnuggets.com/2019/09/automate-python-scripts-task-scheduler.html](https://www.kdnuggets.com/2019/09/automate-python-scripts-task-scheduler.html)

[评论](#comments)

**由 [Vincent Tatan](https://www.linkedin.com/in/vincenttatan/)，Google 的数据分析师（机器学习）、信任与安全部门**

![figure-name](../Images/011bf2ba444bc813041c2d9c323d59ed.png) 图片来源：Stocksnap

> * * *
> 
> ## 我们的三大推荐课程
> ## 
> ![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。
> 
> ![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平
> 
> ![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在的组织的 IT
> 
> * * *
> 
> 每天运行我的 Python 脚本太麻烦了。
> 
> 我需要一种定期和自动运行 Python 脚本的方法。

想象一下，如果你的经理让你在深夜起床运行一个脚本，那将是你最大的噩梦。你会过早醒来，暴露在可怕的蓝光下，每晚都无法得到安稳的睡眠。

作为任何数据专业人士，你可能需要运行多个脚本以生成报告或部署分析管道。因此，你需要了解**调度程序**以避免毁掉你的周末。

> 每个数据工程师和科学家在某些时候都需要运行周期性任务。

按定义，周期性任务是指在某个时间间隔内重复执行的任务，几乎不需要人工干预。在数据和技术快速发展的时期，你需要运行脚本来开发数据库备份、Twitter 流等。

幸运的是，借助任务计划程序，你现在可以根据需要每天/每周/每月/每年运行你的 Python 脚本以执行周期性任务。

在本教程中，你将学习如何运行任务计划程序来 [从 Lazada 网站抓取数据](https://towardsdatascience.com/in-10-minutes-web-scraping-with-beautiful-soup-and-selenium-for-data-professionals-8de169d36319)（电子商务）并将其导入到[ SQLite RDBMS](https://towardsdatascience.com/relational-database-management-rdbms-basic-for-data-professionals-aca3567f03da)数据库中。

这是一个快速的自动运行脚本的概览！

![figure-name](../Images/c206897f159556fc3e3a5443ed948201.png) 使用任务计划程序运行网络抓取脚本，然后将其附加到 SQLite 磁盘

> 开始吧！

### 方法

在本教程中，我们将使用 Windows 任务计划程序运行一个 bat 脚本，该脚本将触发 Python 脚本。要执行这些脚本，我们有两个简单的步骤：

1.  创建 Python 可执行文件（bat 文件）

1.  在 Windows 任务计划程序中配置任务

然而，如果你是 Linux 用户而没有 Windows 任务调度程序，你应该使用[cron 调度程序](https://medium.com/@Ratik96/https-medium-com-ratik96-scheduling-jobs-with-crontab-on-macos-add5a8b26c30)。

### 创建 Windows 可执行的 bat 文件以运行 Python

> **BAT 文件**是一个 DOS **批处理文件**，用于通过 Windows 命令提示符（cmd.exe）执行命令。它包含一系列通常在 DOS 命令提示符下输入的命令。**BAT 文件**最常用于启动程序和运行 Windows 内的维护工具。— fileinfo.com

使用 bat 文件作为我们的可执行文件，我们将运行脚本存储在一个文件中，然后双击 bat 文件以在 cmd（命令提示符）中执行命令以运行 Python 脚本。

你只需创建一个新的 bat 文件（例如：web-scraping.bat），并用**<Your Python.exe Location> <Your python Scripts Location>**格式编写可执行脚本。你可以添加*pause*命令，以避免执行后命令提示符窗口关闭。

```py

C:\new_software\finance\Scripts\python.exe "C:/new_software/Web Scraping/Web-Scraping/Selenium Web Scraping/scraping-lazada.py"
pause

```

一旦你双击这个 bat 文件，Windows 将打开你的命令提示符并运行网页抓取工具。为了调度这个双击/执行，我们将任务调度程序连接到 bat 文件上。

### 在 Windows 任务调度程序中配置任务

**Windows 任务调度程序**是一个默认的 Windows 应用程序，用于管理响应事件或基于时间的触发的任务。例如，你可以建议某个点击和计算机操作（如重启），甚至建议像*每个财务季度的第一天*这样的时间来执行任务。

从更大的视角来看，这个任务将包含定义执行动作的脚本和元数据。你可以在参数中添加某些安全上下文，并控制调度器将程序运行在哪里。Windows 会将所有这些任务序列化为**.job**文件，存储在一个名为**Task Folder**的特殊文件夹中。

![figure-name](../Images/29d90cae1703795d1b33dd8e308d7e29.png)任务调度程序自动化应用程序网页抓取的流程

在本教程中，我们将设置一个基于时间的事件来运行我们的应用程序，并将数据导入 SQLite。

1.  点击“开始 Windows”，搜索“任务调度程序”并打开它。

1.  点击右侧窗口的“创建基本任务”。

1.  选择你的触发时间。

1.  为我们之前的选择确定具体时间。

1.  启动程序

1.  插入你保存 bat 文件的位置的程序脚本。

1.  点击完成。

### 开始吧！

1. **点击“开始 Windows”，搜索“任务调度程序”并打开它**。

![figure-name](../Images/245b2d79871002e9663021c3dd4837c3.png)任务调度程序窗口

2. **点击右侧窗口的“创建基本任务”**。

你应该输入你的任务名称（例如：网页抓取）和描述（例如：每天 6 点自动执行网页抓取和 SQLite 数据导入）

![figure-name](../Images/c7e0b498f37c724d1b7f2df1ea6eee74.png)

3. **选择触发时间**。

你将有一个选项来选择每天、每周甚至每月的时间触发器。从逻辑上讲，这个选择主要取决于你希望从数据源中刷新值的频率。例如，如果你的任务是抓取 MarketWatch 股票资产负债表，你应该每个财务季度运行一次脚本。

![figure-name](../Images/7ae73656ac661ef32a154734b4982b99.png)

4. **选择我们之前选择的确切时间**。

我们将选择一月、四月、七月和九月，以表示所有早期财务季度。

![figure-name](../Images/7a84204ff2abeb7a9c1cffd21c574c76.png)

5. **启动程序**

在这里你可以启动 Python 脚本，发送电子邮件，甚至显示消息。可以随意选择你最熟悉的。不过，你需要注意有些任务已被弃用，将在后续补丁中移除。

![figure-name](../Images/322c79d3035165d2fc1da98b5153a96b.png)

6. **将你的程序脚本插入到你之前保存的 bat 文件中。**

这将运行任务调度器以自动化你的 Python 脚本。确保你还包括启动位置到你的应用程序文件夹中，以访问所有相关元素（Selenium 浏览器执行文件 / SQLite 磁盘）

![figure-name](../Images/ec4fa4e7811009ced298c58269e7f200.png)

7. **点击完成**。

你可以在任务调度器的首页查看你创建的任务计划。

![figure-name](../Images/90e726c260c67b6e720ae84b03548d57.png)

> 恭喜，你已经在 Windows 中设置了第一个自动化调度程序。

### 结果

这里是 gif 动画供你参考。注意调度程序如何自动运行 Python 脚本。脚本运行完毕后，它会将提取的值存入 SQLite 数据库。将来，每次触发条件满足时，这个应用程序都会运行，并将更新的值追加到 SQLite 中。

![figure-name](../Images/c206897f159556fc3e3a5443ed948201.png)运行 Web 抓取脚本与任务调度器，然后将其追加到 SQLite 磁盘！![figure-name](../Images/1691bc308af0f91bbb96dc82603c4c8f.png)通过任务调度器将数据追加到 SQLite

### 最后…

![figure-name](../Images/97bc8f472fefd32b6a860325a8e7550a.png)男孩在读书时笑了，来源：Unsplash

我真的希望这篇文章对你有很大的帮助，并能激发你进行开发和创新。

请 **在下方评论**以提出建议和反馈。

如果你真的喜欢，请查看我的个人资料。那里有更多关于数据分析和 Python 项目的文章，符合你的兴趣。

编程愉快 :)

请通过 [**LinkedIn**](http://www.linkedin.com/in/vincenttatan/)**、**[**Medium**](https://medium.com/@vincentkernn)** 或 **[**YouTube频道**](https://www.youtube.com/user/vincelance1/videos)联系 Vincent

**简介：[Vincent Tatan](https://www.linkedin.com/in/vincenttatan/)** 目前是Google的信任与安全数据分析师（机器学习）。他对数据和技术充满热情，拥有来自Visa Inc.和Lazada的相关工作经验，实施微服务架构、商业智能和分析管道项目。

[原文](https://towardsdatascience.com/automate-your-python-scripts-with-task-scheduler-661d0a40b279)。已获得许可转载。

**相关：**

+   [R语言中的自动化网页抓取](/2018/12/automated-web-scraping-r.html)

+   [网页抓取的原因与方法——您数据武器库中的致命武器](/2018/09/promptcloud-web-scraping.html)

+   [使用Python进行网页抓取：以CIA世界概况书为例](/2018/03/web-scraping-python-cia-world-factbook.html)

### 更多相关主题

+   [3个有用的Python自动化脚本](https://www.kdnuggets.com/2022/11/3-useful-python-automation-scripts.html)

+   [5个真正有用的Bash脚本用于数据科学](https://www.kdnuggets.com/2023/02/bash-scripts-data-science.html)

+   [无需编码即可轻松从网站抓取图像](https://www.kdnuggets.com/2022/06/octoparse-scrape-images-easily-websites-nocoding-way.html)

+   [最佳文本分类任务架构：基准测试…](https://www.kdnuggets.com/2023/04/best-architecture-text-classification-task-benchmarking-options.html)

+   [HuggingChat Python API：您的无成本替代方案](https://www.kdnuggets.com/2023/05/huggingchat-python-api-alternative.html)

+   [Snapdragon上的Windows将混合AI带入边缘应用](https://www.kdnuggets.com/qualcomm-windows-on-snapdragon-brings-hybrid-ai-to-apps-at-the-edge)
