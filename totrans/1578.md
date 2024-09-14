# 自动化工具将如何改变数据科学？

> 原文：[https://www.kdnuggets.com/2018/12/automation-data-science.html](https://www.kdnuggets.com/2018/12/automation-data-science.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者：[藤巻良平博士](https://www.linkedin.com/in/ryohei-fujimaki-1a6bb95/)，dotData的首席执行官和创始人**

数据科学现在是技术投资的一个主要领域，鉴于其对客户体验、收入、运营、供应链、风险管理以及许多其他业务功能的影响。数据科学使组织能够实现数据驱动的决策过程，加速数字化转型和人工智能计划。[根据Gartner, Inc](https://www.gartner.com/en/newsroom/press-releases/2018-02-13-gartner-says-nearly-half-of-cios-are-planning-to-deploy-artificial-intelligence)，只有4%的首席信息官已经实施了人工智能，只有46%的人有计划这么做。虽然投资持续增长，但许多企业发现实施和加速数据科学实践越来越具有挑战性。本文概述了机器学习和数据科学自动化工具的最新趋势，并讨论了这些工具将如何改变数据科学。

### 传统的数据科学过程

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你在IT领域的组织

* * *

那么，是什么阻碍了企业中数据科学的采用和加速呢？一个典型的企业数据科学项目非常复杂，涉及多个步骤，包括数据收集、最后一公里的ETL*（数据整理）、特征工程、机器学习、可视化和生产（见下图）。即使对于经验丰富的团队，传统的数据科学项目也需要几个月才能完成。这是一个高度参与和协作的过程，需要各种专业技能，如领域专家、数据工程师、数据科学家、商业智能工程师和软件架构师。此外，大多数企业数据科学项目的结果难以解释，这使得业务用户很难实施这些结果。

![传统的数据科学过程](../Images/197d800844e2c9af04825e42a84db46e.png)

**传统的数据科学过程**

### 数据科学为什么这么难？

玩转机器学习（ML）模型被认为是有趣的部分，但任何数据科学项目的真正痛点通常是最后一公里的ETL和特征工程。如下面所示，机器学习需要一个称为特征表的单一扁平表。给定一个特征表，数据科学家可以使用ML算法进行操作。但实际的企业数据从来不是一个单一的扁平表，而是一组具有复杂关系的数据表。

![机器学习数据](../Images/e07af82a49e9327b0a351c6accbac2d8.png)

**机器学习所需的数据（左）与实际企业源数据（右）**

最后一公里的ETL和特征工程是将多个表转换为特征表的必要步骤。这些是数据科学项目中最具挑战性和耗时的步骤，需由高技能的数据科学家和领域专家完成——这些资源既昂贵又稀缺。

“……特征工程通常是机器学习项目中大部分努力投入的地方……在这里，直觉、创造力和‘黑艺’与技术内容一样重要……” - **Pedro Domingos博士**

### 数据科学与机器学习自动化工具

自动化机器学习的尝试始于2010年代初（例如2013年的[AutoWEKA](https://www.cs.ubc.ca/labs/beta/Projects/autoweka/)），并且变得非常流行。[DataRobot](https://www.datarobot.com/)和[H2O.ai](http://h2o.ai)是机器学习自动化领域的领先初创公司。

机器学习自动化的基本理念是使用不同的算法（包括缺失值填充等预处理）和不同的超参数训练评分模型，并验证其准确性以选择最佳模型。最近，像[微软](https://www.microsoft.com)这样的公司也开始支持[机器学习自动化工具](https://docs.microsoft.com/en-gb/azure/machine-learning/service/tutorial-auto-train-models)（更多细节可以在[这里](https://www.automl.org/)或[这里](https://www.kdnuggets.com/software/automated-data-science.html)找到）。这些优秀的工具显著简化了机器学习模型的构建。另一方面，最后一公里的ETL和特征工程仍然是一个手动过程，需要领域专家和数据科学家的大量参与。

尽管已有努力自动化特征工程，但大多数关注于给定特征表的非线性转换，这只是特征工程过程中的一个小组成部分，并且依赖于手动创建特征表。[dotData](https://dotdata.com/)发布了一个平台，它不仅自动化了从源数据中生成特征工程，还自动化了机器学习。dotData称之为“数据科学自动化”。其人工智能驱动的特征工程自动设计和生成重要且可解释的特征，无需领域知识。该平台涵盖了与数据科学过程相关的广泛任务，使构建和实施数据科学项目变得更容易、更快捷。

### 自动化工具将如何改变数据科学？

数据科学家或领域专家会被自动化工具取代吗？显然不会。没有任何工具可以真正取代熟练的专家。相反，它使他们更高效。自动化将从三个主要方面影响数据科学：

+   **敏捷性：** 传统的数据科学过程通常遵循“瀑布”方法，这涉及大量前期工作，如数据清洗、ETL 和特征工程，因为每个单独步骤都需要大量的人工和耗时的工作。自动化工具使得尝试想法变得更容易、更快捷，从而使数据科学家能够探索高影响力的用例。

+   **民主化：** 大型企业中有数百种潜在的分析用例（甚至可能更多）。自动化工具使具有不同技能的人能够执行数据科学任务，并使难以招聘的成熟数据科学团队能够专注于高价值创造的用例。

+   **操作化：** 如本博客开头所述，大多数企业尚未实施人工智能和数据科学。许多企业级自动化工具，如 [dotData](https://dotdata.com/)，可以自动生成 API 或可执行包，立即在生产中操作。这显著缩短了在企业中实施数据科学的时间和障碍（上图的最后一步）。

随着企业转向数据驱动文化，数据科学变得越来越重要。自动化工具有助于加速数据科学和商业创新。

注释：

* 企业中有两种类型的 ETL（包括数据清洗）。一种是“主数据 ETL”，用于准备组织中通用的数据。有许多出色的工具来支持这个过程，如 [informatica](https://www.informatica.com/)。另一方面，即使主数据准备得很好，我们仍然需要针对每个分析用例的定制 ETL 工作，这被称为“最后一公里 ETL”。

**ACM通讯，第55卷第10期，2012年10月**

**简介**： [藤卷良平博士](https://www.linkedin.com/in/ryohei-fujimaki-1a6bb95/) 是 dotData 的创始人兼首席执行官。在创立 dotData 之前，他曾是 NEC 公司 119 年历史上最年轻的研究员，这一荣誉仅授予了1000多名研究人员中的六位。在 NEC 任职期间，良平积极参与开发许多前沿的数据科学解决方案，并在多个高-profile 分析解决方案的成功交付中发挥了重要作用，这些解决方案现在在行业中被广泛使用。

**资源：**

+   [在线和基于网络的：分析、数据挖掘、数据科学、机器学习教育](https://www.kdnuggets.com/education/online.html)

+   [分析、数据科学、数据挖掘和机器学习的软件](https://www.kdnuggets.com/software/index.html)

**相关：**

+   [2018 年 AI、数据科学、分析的主要发展及 2019 年的关键趋势](https://www.kdnuggets.com/2018/12/predictions-data-science-analytics-2019.html)

+   [AI 解决主义](https://www.kdnuggets.com/2018/07/polonski-ai-solutionism.html)

+   [数据科学中数据使用的 4 个层次](https://www.kdnuggets.com/2018/07/4-levels-data-usage-data-science.html)

### 更多相关话题

+   [数据科学工作流中的自动化](https://www.kdnuggets.com/2023/03/automation-data-science-workflows.html)

+   [免费 Python 自动化课程](https://www.kdnuggets.com/2022/07/free-automate-python-course.html)

+   [3 个有用的 Python 自动化脚本](https://www.kdnuggets.com/2022/11/3-useful-python-automation-scripts.html)

+   [Excel 中的 Python：这将永远改变数据科学](https://www.kdnuggets.com/python-in-excel-this-will-change-data-science-forever)

+   [我使用 ChatGPT（每天）5 个月。这里有一些隐藏的宝石……](https://www.kdnuggets.com/2023/07/used-chatgpt-every-day-5-months-hidden-gems-change-life.html)

+   [人工智能将如何改变移动应用](https://www.kdnuggets.com/2022/12/artificial-intelligence-change-mobile-apps.html)
