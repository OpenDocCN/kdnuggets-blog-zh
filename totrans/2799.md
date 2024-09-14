# 2020年最有用的机器学习工具

> 原文：[https://www.kdnuggets.com/2020/03/most-useful-machine-learning-tools-2020.html](https://www.kdnuggets.com/2020/03/most-useful-machine-learning-tools-2020.html)

[评论](#comments)

**作者 [Ian Xiao](https://www.linkedin.com/in/ianxiao/)，Dessa的互动负责人**

![图像](../Images/856d368859271128bf33d0e657fd71ed.png)

由 [Creatv Eight](https://unsplash.com/@creatveight?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供的照片，来自 [Unsplash](https://unsplash.com/collections/8641367/image-for-blogs/56c1d67cd0eb64f06855199bd065aa08?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**TD; DR** — 构建优秀的机器学习应用就像制作米其林风格的菜肴。拥有一个组织良好且管理得当的厨房至关重要，但有太多的选择。在这篇文章中，我将重点介绍我在交付专业项目时发现有用的工具，分享一些想法和替代方案，并进行一次实时调查（你可以在参与后查看社区的想法）。

像所有工具讨论一样，列表并不详尽；但我尝试专注于最有用和最简单的工具。欢迎在评论区提供反馈，或者告诉我是否有其他更好的替代方案我应该提到。

***免责声明***：此帖子未获得认可或赞助。我交替使用“数据科学”和“机器学习”这两个术语。

***喜欢你所读到的内容？*** 关注我 *[*Medium*](https://medium.com/@ianxiao)*，*[*LinkedIn*](https://www.linkedin.com/in/ianxiao/)*，或 *[*Twitter*](https://twitter.com/ian_xxiao)*。你想学会如何沟通并成为一名有影响力的数据科学家吗？查看我的“*[*影响力与机器学习*](https://www.bizanalyticsbootcamp.com/influence-with-ml-digital)*”指南。

### 常见问题

“我如何构建优秀的机器学习应用？”

这个问题在与学校中的有抱负的数据科学家、寻求转行的专业人士和团队经理的交流中多次出现，并且以各种形式呈现。

交付一个专业的数据科学项目有许多方面。像许多人一样，我喜欢用厨房的类比：有原料（数据）、食谱（设计）、烹饪过程（你独特的方法），以及最终的厨房（工具）。

所以，这篇文章带你了解我的 **厨房**。它突出展示了 **最有用的工具** 用于设计、开发和部署 **全栈机器学习** 应用程序——这些解决方案可以与系统集成或在生产环境中服务于用户。

如果你想了解更多关于交付ML的其他方面，请查看我的文章 [这里](http://if%20you%20want%20to%20know%20more%20about%20other%20aspects/)。

### 令人眼花缭乱的可能性

我们生活在一个黄金时代。如果你在谷歌中搜索“ML工具”或询问顾问，你可能会得到类似这样的内容：

![图像](../Images/c5b3760e4199aa44765fb36551bab1b1.png)

数据与人工智能领域 2019，[图像来源](https://www.google.com/url?sa=i&url=https%3A%2F%2Fmattturck.com%2Fdata2019%2F&psig=AOvVaw0oXq7zJf_Hz_RjdI-F70rq&ust=1583513401048000&source=images&cd=vfe&ved=0CAIQjRxqFwoTCPCR2KPlg-gCFQAAAAAdAAAAABAJ)

[GIPHY](https://media.giphy.com/media/aVytG2ds8e0tG/giphy.gif)

现在有**（太多）工具**，可能的组合是无限的。这可能会让人感到困惑和不知所措。所以，让我帮助你缩小范围。也就是说，没有完美的设置，一切都取决于你的需求和限制。因此，请根据实际情况选择。

我的列表优先考虑以下几点（不按顺序）：

+   免费 ????

+   易于学习和设置 ????

+   未来证明（采纳 & 工具成熟度） ♻️

+   工程优于研究 ????

+   适用于初创公司或大型企业的大型或小型项目 ????

+   只需完成工作 ????

*警告：我**99%**的时间使用 Python。所使用的工具都与 Python 原生兼容或是用 Python 构建的。我没有用其他编程语言（如 R 或 Java）测试过它们。*

### 1\. 冰箱：数据库

### [**PostgreSQL**](https://www.postgresql.org/)

一个免费的开源关系数据库管理系统（RDBMS），强调可扩展性和技术标准遵循。它旨在处理从单台机器到数据仓库或具有多个并发用户的 Web 服务的一系列工作负载。

![图像](../Images/e69ad619bb84e1fa727d8bd982470f2e.png)

[图像来源](https://www.google.com/url?sa=i&url=https%3A%2F%2Fwww.aquafold.com%2Fdbspecific%2Fpostgresql_client&psig=AOvVaw2Q_9WB3s7LFAJq4NStNNm_&ust=1583516333356000&source=images&cd=vfe&ved=0CAIQjRxqFwoTCMjw2Znwg-gCFQAAAAAdAAAAABAD)

*替代方案：*[*MySQL*](https://www.mysql.com/)*、*[*SAS*](https://www.sas.com/en_ca/home.html)*、*[*IBM DB2*](https://www.ibm.com/analytics/db2)*、*[*Oracle*](https://www.oracle.com/database/)*、*[*MongoDB*](https://www.mongodb.com/)*、*[*Cloudera*](https://www.cloudera.com/)*、*[*GCP*](https://cloud.google.com/storage)*、*[*AWS*](https://aws.amazon.com/products/storage/)*、*[*Azure*](https://azure.microsoft.com/en-ca/services/storage/)*

### 2\. 台面：部署管道工具

管道工具对于开发的速度和质量至关重要。我们应该能够以最少的人工处理快速迭代。这里有一个效果良好的设置，详情请参见我的 [12小时机器学习挑战](https://towardsdatascience.com/build-full-stack-ml-12-hours-50c310fedd51) 文章。每个*懒惰的*数据科学家都应该在项目初期尝试这种方法。

![图像](../Images/215efcf79988e70931e2df5e2f947c15.png)

作者的工作，[12小时机器学习挑战](https://towardsdatascience.com/build-full-stack-ml-12-hours-50c310fedd51)

### [**Github**](https://github.com/)

它提供 Git 的分布式版本控制和源代码管理（SCM）功能，并且具有自己的特性。它提供访问控制和多个协作功能，如错误跟踪、功能请求、任务管理和每个项目的维基。

*替代方案：*[*DVC*](https://dvc.org/)*, *[*BitBucket*](https://bitbucket.org/product)*, *[*GitLab*](https://about.gitlab.com/)

### [**PyCharm**](https://www.jetbrains.com/pycharm/)** 社区版**

一种用于计算机编程的集成开发环境（IDE），专为 Python 语言设计。由捷克公司 JetBrains 开发。它提供代码分析、图形调试器、集成单元测试器、与版本控制系统（VCS）的集成，并支持使用 Django 进行 web 开发以及使用 Anaconda 进行数据科学。

*替代方案：*[*Atom*](https://atom.io/)*, *[*Sublime*](https://www.sublimetext.com/)

### [Pytest](https://docs.pytest.org/en/latest/index.html)

框架使得编写小型测试变得容易，同时能够扩展以支持复杂的应用和库的功能测试。它节省了大量的手动测试时间。如果你需要每次修改代码时进行测试，可以使用 Pytest 来自动化这一过程。

*替代方案：*[*Unittest*](https://docs.python.org/3/library/unittest.html)

### [CircleCi](https://circleci.com/)

CircleCI 是一个持续集成和部署工具。它使用远程 Docker 创建自动化测试工作流程，当你提交到 Github 时，Circle CI 会拒绝任何未通过 PyTest 测试用例的提交。这确保了代码质量，尤其是当你与较大的团队合作时。

*替代方案：*[*Jenkins*](https://jenkins.io/)*, *[*Travis CI*](https://travis-ci.com/)*, *[*Github Action*](https://github.com/features/actions)

### [Heroku](https://www.heroku.com/) （仅在需要 web 托管时）

一种平台即服务（PaaS），使开发者能够完全在云中构建、运行和操作应用程序。你可以与 CircleCI 和 Github 集成，实现自动部署。

*替代方案：*[*Google App Engine,*](https://cloud.google.com/appengine)[*AWS Elastic Compute Cloud*](https://aws.amazon.com/ec2/)*, *[*其他*](https://medium.com/@brenda.clark/heroku-alternatives-top-5-picks-9095cef91d91)

### [Streamlit](https://www.streamlit.io/) （仅当你需要一个互动 UI 时）

Streamlit 是一个开源应用框架，专为机器学习和数据科学团队设计。近年来，它已经成为我最喜欢的工具之一。查看我如何使用它以及本节中的其他工具创建一个 [**电影**](https://towardsdatascience.com/build-full-stack-ml-12-hours-50c310fedd51)** 和 **[**模拟**](https://towardsdatascience.com/how-to-design-monte-carlo-simulation-138e9214910a)** 应用**。

*替代方案：*[*Flask*](https://palletsprojects.com/p/flask/)*, *[*Django*](https://www.djangoproject.com/)*, *[*Tableau*](https://www.tableau.com/en-ca)

### 3\. iPad: 探索工具

### [Streamlit](https://www.streamlit.io/) （再次提及）

忘记 Jupyter Notebook 吧。对，就是这样。

Jupyter *曾经是* 我探索数据、进行分析和试验不同数据和建模过程的首选工具。但我记不清有多少次：

1.  我花了很多时间调试（并抓狂），但最终意识到我忘记从头运行代码；Streamlit 修复了这个问题。

1.  即使对小代码更改，我也不得不等待数据管道重新运行；Streamlit 缓存修复了这个问题。

1.  我不得不重写或将代码从 Jupyter 转换为可执行文件——而且花费了时间重新测试；Streamlit 提供了捷径。

这很让人沮丧???????。所以，我使用 Streamlit 进行**早期探索**，并提供最终的前端——一箭双雕。以下是我的典型屏幕设置。左侧是 PyCharm IDE，右侧是结果可视化。试试看吧。

![Figure](../Images/45305fb492c4a5ef9f42d171860f7c01.png)

IDE（左侧）+ Streamlit 实时更新（右侧），作者作品来自 [被遗忘的算法](https://towardsdatascience.com/how-to-design-monte-carlo-simulation-138e9214910a)

替代方案：[*Jupyter Notebook*](https://jupyter.org/)*, *[*Spyder from Anaconda*](https://www.spyder-ide.org/)*, *[*Microsoft Excel*](https://products.office.com/en-ca/excel)* （认真地说）*

### 4\. 刀具：ML 框架

就像使用实际的刀子一样，你应该根据食物和切割方式选择合适的刀子。有通用型刀和专用刀。

**要小心。** 使用寿司专用刀切骨头会花费很长时间，尽管寿司刀更光亮。选择合适的工具完成工作。

### [Sklearn](https://scikit-learn.org/stable/) （常见的 ML 用例）

在 Python 中进行一般机器学习的首选框架。无需多言。

![Figure](../Images/73e54bc34291cf4258e869409a3f476f.png)

Sklearn 的使用案例，[来源](https://scikit-learn.org/stable/)

*替代方案：没有，完毕。*

### [PyTorch](https://pytorch.org/) （深度学习用例）

基于 Torch 库的开源机器学习库。鉴于其深度学习的重点，它主要用于计算机视觉和自然语言处理等应用。主要由 Facebook 的 AI 研究实验室（FAIR）开发。最近，许多知名的 AI 研究机构，如 [Open AI](https://openai.com/blog/openai-pytorch/)，都将 PyTorch 作为其标准工具。

*替代方案：*[*Tensorflow*](https://www.tensorflow.org/)*, *[*Keras*](https://keras.io/)*, *[*Fast.ai*](https://docs.fast.ai/)

### [Open AI Gym](https://gym.openai.com/) （强化学习用例）

用于开发和比较强化学习算法的工具包。它提供 API 和可视化环境。这是一个社区正在构建工具的活跃领域。现在还没有很多打包良好的工具。

*替代方案：许多小项目，但没有多少像 Gym 那样维护良好的。*

### 5\. 炉子：实验管理

### [Atlas](https://www.atlas.dessa.com/)

一个免费工具，允许数据科学家通过几个代码片段设置实验，并将结果显示在基于网页的仪表板上。

![Figure](../Images/4e3fa51661c725e7e5c237af03ffc20a.png)

Atlas 过程，[来源](https://www.atlas.dessa.com/)

**免责声明**：我曾在Dessa工作，该公司创建了Altas。

*替代方案：*[*ML Flow*](https://mlflow.org/)*、*[*SageMaker*](https://aws.amazon.com/sagemaker/)*、*[*Comet*](https://www.comet.ml/site/)*、*[*Weights & Biases*](https://www.wandb.com/)*、*[*Data Robot*](https://www.datarobot.com/)*、*[*Domino*](https://www.dominodatalab.com/)

### 一项调查

出于好奇，**在寻找合适工具时，什么困扰你最多？** 我很想听听你的想法。它是一个实时调查，所以你可以在参与后看到社区的看法。

### 替代视角

正如我提到的，没有完美的设置。这一切都取决于你的需求和限制。这里是另一个视角，展示了有哪些工具以及它们如何协同工作。

![图](../Images/ea28f7970afd18c238d140f40ccb2684.png)

[Sergey Karayev](https://www.linkedin.com/in/ACoAABqncBAB7JVSkewnHs3jvPfOOm-U23LAlTo/)在[全栈深度学习](https://full-stack-deep-learning.aerobaticapp.com/b172_eb327323-811b-4de9-8894-76ec4cfd6458/assets/slides/fsdl_4_infra_tooling.pdf)上的演讲，2019年

### 一个迷你挑战

如果你想了解更多关于如何使用这些工具，最好的方法是找到一个项目来实践。你可以将这些工具融入到当前的项目中，或进行一个**12小时机器学习挑战**。不确定怎么做？查看[我如何用讨论过的工具和流程创建用户赋能推荐应用](https://towardsdatascience.com/build-full-stack-ml-12-hours-50c310fedd51)。

我期待看到你能创造出的成果。请与社区分享，并在[Twitter](https://twitter.com/ian_xxiao)上标记我????。

***喜欢你所读的内容？**** 在*[*Medium*](https://medium.com/@ianxiao)*、*[*LinkedIn*](https://www.linkedin.com/in/ianxiao/)*或*[*Twitter*](https://twitter.com/ian_xxiao)*上关注我。此外，你想学习作为数据科学家的商业思维和沟通技巧吗？查看我的“*[*机器学习影响力*](https://www.bizanalyticsbootcamp.com/influence-with-ml-digital)*”指南。*

### 这里是一些你可能喜欢的文章：

[**被遗忘的算法**](https://towardsdatascience.com/how-to-design-monte-carlo-simulation-138e9214910a)

使用Streamlit探索蒙特卡罗模拟

[**数据科学很无聊**](https://towardsdatascience.com/data-science-is-boring-1d43473e353e)

我如何应对机器学习部署中的枯燥日子

[**12小时机器学习挑战**](https://towardsdatascience.com/build-full-stack-ml-12-hours-50c310fedd51)

如何用Streamlit和DevOps工具构建和部署机器学习应用

[**机器学习与敏捷的注定失败的结合**](https://towardsdatascience.com/a-doomed-marriage-of-ml-and-agile-b91b95b37e35)

如何不将敏捷方法应用于机器学习项目

[**人工智能的最后一公里问题**](https://towardsdatascience.com/fixing-the-last-mile-problems-of-deploying-ai-systems-in-the-real-world-4f1aab0ea10)

如何促进人类与人工智能之间的信任

[**另一个AI寒冬？**](https://towardsdatascience.com/the-last-defense-against-another-ai-winter-c589b48c561)

如何部署更多的机器学习解决方案 — 五种策略

**简介：[Ian Xiao](https://www.linkedin.com/in/ianxiao/)** 是Dessa的参与主管，负责在企业中部署机器学习。他领导商业和技术团队部署机器学习解决方案，并改善F100企业的营销和销售。

[原文](https://towardsdatascience.com/the-most-useful-ml-tools-2020-e41b54061c58)。经许可转载。

**相关：**

+   [12小时机器学习挑战：使用Streamlit和DevOps工具构建和部署应用](/2020/02/machine-learning-challenge-build-deploy-app-streamlit-devops.html)

+   [数据科学无聊（第一部分）](/2019/09/data-science-boring-part-1.html)

+   [抵御另一场AI寒冬的最后防线](/2019/11/last-defense-another-ai-winter.html)

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

### 更多相关内容

+   [数据科学中的4个有用的中级SQL查询](https://www.kdnuggets.com/2022/12/4-useful-intermediate-sql-queries-data-science.html)

+   [3个有用的Python自动化脚本](https://www.kdnuggets.com/2022/11/3-useful-python-automation-scripts.html)

+   [KDnuggets新闻，12月7日：揭穿数据科学的十大神话 • 4…](https://www.kdnuggets.com/2022/n47.html)

+   [5个真正有用的数据科学Bash脚本](https://www.kdnuggets.com/2023/02/bash-scripts-data-science.html)

+   [Kaggle竞赛对解决实际问题有用吗？](https://www.kdnuggets.com/are-kaggle-competitions-useful-for-real-world-problems)

+   [KDnuggets新闻，8月3日：10个最常用的Tableau函数 • 是…](https://www.kdnuggets.com/2022/n31.html)
