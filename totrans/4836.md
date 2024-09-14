# Plotly 的 Dash Python 框架概述用于构建仪表板

> 原文：[https://www.kdnuggets.com/2018/05/overview-dash-python-framework-plotly-dashboards.html](https://www.kdnuggets.com/2018/05/overview-dash-python-framework-plotly-dashboards.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者：[Damian Rodziewicz](https://appsilondatascience.com/blog)，Appsilon。**

![Dash Python Plotly](../Images/aeca29c2f5f5eeea4b4e8be99251c696.png)

> Appsilon 技术讲座 - 每周三我们举行内部会议，讨论技术。通过与社区分享我们的技术讲座并贡献开源，我们激发了讨论和成长，让其他人从我们的成就中学习并与我们分享。

最近，我有机会做一个关于[Dash Python 框架](https://plot.ly/products/dash/)的技术演讲。Dash 是一个用于构建仪表板的反应式框架。它们类似于使用[Shiny R 框架](https://appsilondatascience.com/blog/rstats/2016/12/28/make-shiny-awesome.html)构建的仪表板。在 Appsilon，我们已经在 Shiny 中构建了复杂的仪表板。我们还为[成千上万的用户](https://appsilondatascience.com/blog/rstats/2017/10/17/scaling-shiny.html)进行了部署。

Python 社区正在赶上，我们很高兴探索这些机会。值得比较这两者。我们已经进行了若干内部测试。我们已经为最具创新性的客户交付了 Dash 项目。从今天开始，我们为更广泛的受众交付用 Dash 编写的业务项目。以下是我们经验教训的快速总结！

本次技术演讲涵盖了基础知识和更高级的主题。你将从简单的开始，逐步完成自定义组件的构建和扩展。

技术演讲中涵盖的主题广泛列表：

### 1\. 什么是 Dash？

+   Dash 是一个用于构建 web 应用程序的高效 Python 框架。

+   由 Plotly 创建和维护。 https://plot.ly/products/dash/。

+   基于 Flask、Plotly.js 和 React.js 上层构建。

+   仪表板使用纯 Python 实现。

+   Dash 是一个开源库，发布在宽松的 MIT 许可证下。

### 2\. 使用 Dash 可以获得什么

+   前端用 Python 生成

+   反应式计算抽象

+   每个 HTML 标签的组件类以及在 dash_html_components 包中实现的所有 HTML 参数的关键字参数

+   在 dash-core-components 中实现的交互式 HTML 元素

+   Dash-core-components 中实现的 Plotly python API，通过 Graph 类提供

### 3\. Dash 演示应用程序及其源代码解释。

更深入地了解 Dash 的交互性以及重新计算的工作原理。

### 4\. Dash 核心组件是什么？如何构建自定义组件？

你可以在 Dash 中实现自己的组件。有关如何做到这一点的信息，请访问 [https://dash.plot.ly/plugins](https://dash.plot.ly/plugins)

我们已经开始实施自己的组件和助手：

+   Leaflet 地图

+   时间线（反应式水平时间线移植）

+   Mixpanel 钩子（允许使用 Mixpanel 的前端插件通过 cookie 识别用户）

+   IncludeScript（按需包含并运行外部脚本 - 如果应用在本地模式下运行则很有用）

### 5\. 单线程 Dash

图形的重新计算是阻塞的且是单线程的。然而，我们可以提取生成的 Flask 服务器，并使用 Gunicorn 来使计算并发。

[Gunicorn](http://gunicorn.org/) ‘Green Unicorn’ 是一个用于 UNIX 的 Python WSGI HTTP 服务器。它是一个预分叉工作模型。Gunicorn 服务器与各种网络框架广泛兼容，实现简单，占用服务器资源少，并且相当迅速。

似乎仍然对并发用户数量有一定限制。

### 6\. Dash 的限制

+   我们仍在探索它在众多并发用户下的可扩展性。

+   在某些时候，你将需要比 Dash 默认提供的更复杂的组件。

+   你将不得不在 React.js 中编写自己的组件。

+   或者你将不得不将现有组件从 React.js 移植到 Dash。

+   你很快会发现一些快速解决方案仍在开发中（Mapbox 栅格示例）。

+   在反应图中没有中间值。

+   你必须添加一个隐藏的 div 以存放中间数据（如 plotly 所建议的）。

+   你必须为每一个输出编写单独的函数，这迫使你重构代码。

+   有一些问题我们可能无法解决，除非深入了解 Dash 的工作方式。

+   可能仍然存在一些我们不知道的 Dash 问题 - 我们必须为我们的工作留出余地。

### 7\. 问答环节

1.  状态是保留在服务器上还是客户端上？与 Shiny 和 Dash 比较。

1.  使用记忆化作为一种变通方法。讨论带有记忆化的线程。

1.  在 Dash 和 Shiny 之间指定反应性有什么不同？每种模型的优缺点。

希望你喜欢！请在评论中告诉我你想接下来探索什么！

#### 资源：

+   [https://github.com/DamianRodziewicz/dash_example](https://github.com/DamianRodziewicz/dash_example)

+   [Slideshare](https://www.slideshare.net/secret/nnwCIXI6iBDOmG)

简介: [Damian](https://preview.appsilondatascience.com/blog) 喜欢称自己为技术狂热者，这一点很贴切，因为他是我们的联合创始人和首席数据科学家。他拥有计算机科学硕士学位和管理法律研究生学位。在创办 Appsilon 之前，他曾在 Accenture、UBS、Microsoft 和 Domino Data Lab 工作。他是一名热衷于游泳和心理学的爱好者。

![](../Images/7a5a84bee7701ded3ebc338a1c8b6859.png)

[原文](https://appsilondatascience.com/blog/rstats/2018/05/08/dash.html)。经许可转载。

**相关：**

+   [Dash 社区制作的 7 个新 Dash 应用](https://www.kdnuggets.com/2018/03/dash-apps-workshops-boston.html)

+   [数据可视化最佳实践](https://www.kdnuggets.com/2018/05/jmp-best-practices-data-visualization.html)

+   [适用于任何学科的10个有用的 Python 数据可视化库](https://www.kdnuggets.com/2016/06/python-data-visualization-libraries.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的IT需求

* * *

### 更多相关主题

+   [Plotly Express数据可视化备忘单](https://www.kdnuggets.com/2023/03/plotly-express-data-visualization-cheat-sheet.html)

+   [使用Python和Dash创建仪表板](https://www.kdnuggets.com/2023/08/create-dashboard-python-dash.html)

+   [为有效的Tableau和Power BI仪表板准备数据](https://www.kdnuggets.com/2022/06/prepare-data-effective-tableau-power-bi-dashboards.html)

+   [人工智能/机器学习模型的风险管理框架](https://www.kdnuggets.com/2022/03/risk-management-framework-aiml-models.html)

+   [Django框架中的社交用户认证](https://www.kdnuggets.com/2023/01/social-user-authentication-django-framework.html)

+   [适用于所有用途的唯一提示框架](https://www.kdnuggets.com/the-only-prompting-framework-for-every-use)
