# MLOps 和机器学习路线图

> 原文：[https://www.kdnuggets.com/2021/08/mlops-machine-learning-roadmap.html](https://www.kdnuggets.com/2021/08/mlops-machine-learning-roadmap.html)

[评论](#comments)

**由 [Ben Rogojan](https://www.theseattledataguy.com/data-science-consulting-blog)，数据科学和数据工程解决方案架构师**

[![](../Images/e585c4bfb5eac82218a2d8368bbd8925.png)](https://miro.medium.com/max/1400/1*J3CbvCM8BMxiUveHUTkIug.png)

图片由作者创建 — [PDF 来源](https://drive.google.com/file/d/1Fca-ohcIUsSYPTUHdzfV3XXesUlYdq8_/view?usp=sharing)

每当我学习一个新主题时，我都会制定某种形式的学习计划。现在有如此多的内容，现代时代学习起来可能很困难。

这几乎是滑稽的。我们有如此丰富的知识资源，但许多人却因为不知道去哪里找而难以学习。

这就是为什么我整理了路线图和学习计划。

以下是我在接下来的几个月内将要进行的 MLOps 学习计划。

重点将首先是快速复习机器学习，并且参加一个高级 Kubernetes 课程。

从那里，我将专注于 Kubeflow、Azure ML 和 DataRobot。

## MLOps 背景

2014 年，一组 Google 研究人员发布了一篇题为 *机器学习：技术债务的高利息信用卡* 的论文。这篇论文指出了许多公司可能忽视的一个日益严重的问题。

> *使用技术债务框架，我们注意到在应用机器学习时，在系统层面产生巨大的持续维护成本是非常容易的。[*[*D. Sculley, Gary Holt 等*](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/43146.pdf)*]*

研究人员在后续演讲中将这个问题表述为，发射火箭很容易，但之后的持续操作却很困难。那时，DevOps 的概念仍在逐步形成，但这些工程师和研究人员意识到，相比于部署代码，部署机器学习模型涉及更多复杂的情况。

这时，机器学习平台的普及开始上升。最终，许多这些平台采用了 MLOps 这个术语来解释他们提供的服务。

这就引出了一个问题。什么是 MLOps？

## 什么是 MLOps？

[机器学习运维（MLOps）](https://www.theseattledataguy.com/mlops-vs-aiops-what-is-the-difference/#page-content)帮助简化了机器学习模型在运维团队和机器学习研究人员之间的管理、物流和部署。

从一个幼稚的角度来看，这只是将 DevOps 应用到机器学习领域。

但 MLOps 实际上需要管理比 DevOps 通常管理的内容多得多。

像 DevOps 一样，MLOps 也管理自动化部署、配置、监控、资源管理以及测试和调试。

可能的机器学习管道如下图所示。

[![](../Images/2c6cb2dc77bcd539b4b849c26357b4b1.png)](https://miro.medium.com/max/1400/0*e7yFLBIMZSk0EaRX.png)

图片由作者创建

与 DevOps 不同，MLOps 还可能需要考虑数据验证、模型分析和重新验证、元数据管理、特征工程以及机器学习代码本身。

但归根结底，MLOps 的目标非常相似。MLOps 的目标是创建机器学习模型的持续开发流水线。

一个能迅速让数据科学家和机器学习工程师部署、测试和监控其模型的流水线，以确保他们的模型继续按预期运行。

## MLOps 路线图

为了更好地了解 MLOps 市场中存在的各种工具，让我们制定一个快速学习计划，看看我们应该复习哪些内容。

### 1\. 数据科学数学复习

我们无法绕过这个问题。

数学是机器学习和数据科学的重要部分。由于我个人已经很久没有接触，同时也考虑到可能对我的读者感兴趣，以下是一些很棒的资源，供那些希望刷新自己的[数据科学数学技能](https://www.theseattledataguy.com/statistics-data-scientist-review/)和机器学习的朋友们使用。

有一个课程引起了我的注意，那就是约翰·霍普金斯大学的数据科学统计学课程。特别是，我对[统计推断](https://www.coursera.org/specializations/data-science-statistics-machine-learning?ranMID=40328&ranEAID=GjbDpcHcs4w&ranSiteID=GjbDpcHcs4w-PHaU4mWQKuneNQsqgnV6eg&siteID=GjbDpcHcs4w-PHaU4mWQKuneNQsqgnV6eg&utm_content=10&utm_medium=partners&utm_source=linkshare&utm_campaign=GjbDpcHcs4w)感兴趣，因为它是比较有趣的课程。

[**数据科学：统计学与机器学习**](https://www.coursera.org/specializations/data-science-statistics-machine-learning?ranMID=40328&ranEAID=GjbDpcHcs4w&ranSiteID=GjbDpcHcs4w-PHaU4mWQKuneNQsqgnV6eg&siteID=GjbDpcHcs4w-PHaU4mWQKuneNQsqgnV6eg&utm_content=10&utm_medium=partners&utm_source=linkshare&utm_campaign=GjbDpcHcs4w)

接下来，许多人可能更喜欢阅读数学书籍而不是参加课程。这时《数据科学家的实用统计学》就派上用场了。某一天我可能应该订阅 O'Reilly，但现在我打算购买这本实用统计学书籍。

特别是，O'Reilly 的书籍通常由专业人士撰写。因此，我期待能看到应用数据科学数学的实际案例。

[**数据科学家的实用统计学：使用 R 和 Python 的 50 多个基本概念**](https://www.amazon.com/Practical-Statistics-Data-Scientists-Essential/dp/149207294X/ref=pd_lpo_2?pd_rd_i=149207294X&psc=1)

### 2\. 复习机器学习和手动部署模型

在深入研究[MLOps](https://seattledataguy.substack.com/p/a-roadmap-to-learning-mlops-in-2021)之前，让我们复习一下机器学习以及如何使用 Docker 部署机器学习模型。

这将帮助我们在后续步骤中比较一些未来的课程，因为我们可以看到多种部署机器学习模型的方法。

我特别发现了一个叫做“使用 Docker 部署机器学习和 NLP 模型”的课程。由于这是部署机器学习模型的一种方式，它将作为一个很好的基础。

[**使用 Docker（DevOps）部署机器学习和 NLP 模型**](https://www.udemy.com/course/deploy-data-science-nlp-models-with-docker-containers/?ranMID=39197&ranEAID=GjbDpcHcs4w&ranSiteID=GjbDpcHcs4w-z_NWvjEAXvg3.TW_lOtaSw&LSNPUBID=GjbDpcHcs4w&utm_source=aff-campaign&utm_medium=udemyads)

## 3\. Kubernetes

Kubernetes 是一个开源的 [容器编排](https://www.vmware.com/topics/glossary/content/container-orchestration) 平台。使用 Kubernetes 允许开发者自动化手动部署、管理和扩展应用程序的过程。

换句话说，你可以将运行 Linux 容器的主机组集成在一起，而 Kubernetes 使你能够轻松高效地管理这些集群。

所以为了深入学习 Kubernetes，我打算钻研 [freeCodeCamp](https://medium.com/u/8b318225c16a?source=post_page-----95efc59927a2--------------------------------) 提供的三小时课程。一方面，这将提供一份很棒的内容。另一方面，freeCodeCamp 通常能将你需要了解的内容浓缩成一个简短的几小时课程。

[**在 3 小时内学习 Kubernetes：容器编排的详细指南**](https://www.freecodecamp.org/news/learn-kubernetes-in-under-3-hours-a-detailed-guide-to-orchestrating-containers-114ff420e882/)

一旦你完成了 freeCodeCamp 的三小时 Kubernetes 课程，你现在可以深入学习高级 Kubernetes。此课程将涵盖 **日志记录** 使用 ElasticSearch、Kibana、Fluentd 和 LogTrail，**打包** 使用 Helm，**部署** 使用 Spinnaker 等内容。

[**学习 DevOps：高级 Kubernetes 使用**](https://www.udemy.com/course/learn-devops-advanced-kubernetes-usage/?ranMID=39197&ranEAID=GjbDpcHcs4w&ranSiteID=GjbDpcHcs4w-ytWeJXw9gUkOSTYuFNS09A&LSNPUBID=GjbDpcHcs4w&utm_source=aff-campaign&utm_medium=udemyads)

## 4\. Kubeflow

Kubeflow 是一个机器学习平台，负责管理 Kubernetes 上 ML 工作流的部署。Kubeflow 的最佳部分是它提供了一个可扩展和可移植的解决方案。

该平台最适合那些希望构建和实验数据管道的数据科学家。Kubeflow 也很适合将机器学习系统部署到不同环境中，以进行测试、开发和生产级服务。

Kubeflow由Google发起，作为一个运行TensorFlow的开源平台。它最初是通过Kubernetes运行TensorFlow作业的方式，但现在已经扩展为一个多云、多架构的框架，能够运行整个ML管道。使用Kubeflow，数据科学家不需要学习新的平台或概念来部署他们的应用程序或处理网络证书等问题。他们可以像在TensorBoard上一样简单地部署他们的应用程序。

### 了解更多关于Kubeflow的信息

为了开始学习Kubeflow，让我们从Google的课程开始，该课程专注于部署机器学习模型。现在这个课程只有一个部分专门讲解Kubeflow，但希望它足够。

[**在GCP上的智能分析、机器学习和AI**](https://www.coursera.org/learn/smart-analytics-machine-learning-ai-gcp?ranMID=40328&ranEAID=GjbDpcHcs4w&ranSiteID=GjbDpcHcs4w-O25buz64mmOiy8l5uAltCw&siteID=GjbDpcHcs4w-O25buz64mmOiy8l5uAltCw&utm_content=10&utm_medium=partners&utm_source=linkshare&utm_campaign=GjbDpcHcs4w)

如果Google的课程不适合，那么[Amina A](https://medium.com/u/de46c1e173d3?source=post_page-----95efc59927a2--------------------------------)关于如何开始使用Kubeflow的文章应该有助于补充。文章中讲述了几个值得深入研究的其他资源。因此，我期待着解析这些内容。

[**（更新！）如何开始使用Kubeflow**](http://)

<h3>想象一下在任何地方、任何设备上运行机器学习训练和推理堆栈，几乎无需配置……</h3>

<p>   amina-alsherif.medium.com)

## 5.Azure ML

Azure Machine Learning (Azure ML) 是**一个用于创建和管理机器学习解决方案的云服务**。它结合了SDK和提供了一个Azure Machine Learning的网页门户，支持低代码和无代码的模型训练、部署和资产管理选项。

现在，够了营销。我对Azure ML提供的一些低代码解决方案方法很感兴趣。

我学到的第一个工具是SSIS，它有类似的感觉。这是一个拖放自动化解决方案。显然，重点更多在于ETL和数据管道。那么我应该从哪里开始呢？

首先，我总是尝试寻找由实际公司支持或开发的课程。

微软有一个课程，旨在帮助准备名为AI-900: Microsoft Azure AI Fundamentals的认证。

本课程的目的是教授在AI基础考试领域中评估的核心概念和技能。它们似乎涵盖了Azure提供的自动化机器学习，以及回归、分类和聚类。

最后，看看他们如何部署这些模型将会很有趣。

[**微软 Azure 机器学习**](https://www.coursera.org/learn/microsoft-azure-machine-learning?ranMID=40328&ranEAID=GjbDpcHcs4w&ranSiteID=GjbDpcHcs4w-y6xy3yyjB2_RZLcGN704TQ&siteID=GjbDpcHcs4w-y6xy3yyjB2_RZLcGN704TQ&utm_content=10&utm_medium=partners&utm_source=linkshare&utm_campaign=GjbDpcHcs4w)

最后，我通常喜欢挑选一些文章，看看其他人和创作者在做什么。在这种情况下，我打算查看 Toptal 如何使用 Azure ML 预测燃气价格。

[**Azure 教程：使用 Azure 机器学习工作室预测燃气价格**](https://www.toptal.com/machine-learning/predicting-gas-prices-using-azure-machine-learning-studio)

## 6.DataRobot

DataRobot 是一个 AI 自动化工具，允许数据科学家自动化部署、维护或构建 AI 的端到端过程。这个框架由开源算法驱动，这些算法不仅在云上可用，也可以在本地使用。DataRobot 允许用户在短短十个步骤中轻松快速地提升他们的 AI 应用程序。这个平台包括关注于提供价值的赋能模型。

DataRobot 不仅适用于数据科学家，还适用于希望在不学习传统数据科学方法的情况下维护 AI 的非技术人员。因此，数据科学家现在可以使用 DataRobot 自动化整个过程，而不必花费大量时间开发或测试机器学习模型。

这个平台最棒的部分是它的普及性。你可以通过多种方式在任何设备上随时访问 DataRobot，以满足你的业务需求。

### MLOps 入门任务

在这个课程中，重点将是提供使用 MLOps 的基础。但在这一点上，我假设我已经知道这些了。因此，我真的希望 DataRobot 能提供比仅仅了解 MLOps 基础更多的内容。

特别是，我希望它能涵盖如何实际使用 DataRobot。

这似乎在第二个课程中得到了更多的涵盖，该课程介绍了如何使用 MLOps 部署 DataRobot AutoML 模型。你将学习不同的部署选项，以及如何使用 MLOps 中的不同组件来部署、监控、管理和治理模型。

我会告知你！

[**MLOps 入门任务**](https://university.datarobot.com/path/mlops-starter-quest)

### DataRobot 白皮书

DataRobot 还整理了一份很有视觉吸引力的 PDF，似乎涵盖了如何最好地利用 DataRobot 的相当多内容。

此外，我还可以将其精简成一页有趣的文档。

## 7\. 下一步——机器学习与 MLOps

我预见 MLOps 在未来 2-3 年内会真正兴起。

是的，如果你在数据领域工作，你可能已经对这个概念有所了解。然而，我觉得在大公司开始大量采用之前，还需要一段时间。

我遇到过一些公司在关注这些工具。总体而言，大多数公司仍在尝试管理他们的数据管道。这就是为什么你可能会更倾向于[学习更多关于数据工程的内容](https://www.youtube.com/watch?v=SpaFPPByOhM&t=1s)而非 MLOps。

祝你好运！

## 阅读/观看接下来内容

+   [**数据科学面试学习指南**](https://betterprogramming.pub/the-data-science-interview-study-guide-c3824cb76c2e)

    *121个资源帮助你找到数据科学的梦想工作*

+   [**我如何从分析师转变为数据工程师**](https://www.youtube.com/watch?v=lGzh-QendJc)

    *如何成为数据工程师——以及如何知道这是否适合你*

+   [**如何作为顾问启动咨询业务**](https://www.youtube.com/watch?v=ZK-5yS7jJC8&t=1s)

    *获取你的第一个客户*

**✉️ [订阅我的邮件列表以获取社区更新和免费资源](https://seattledataguy.substack.com/p/scaling-a-data-analytics-team-for)**

### 关于我

我在职业生涯中专注于各种形式的数据。我致力于开发算法以检测欺诈、减少患者再入院率以及重新设计保险公司政策，以帮助降低整体医疗成本。我还帮助开发了营销和IT运营的分析，以优化员工和预算等有限资源。我在数据科学和工程问题上提供私人咨询，既可以单独工作，也可以与名为 Acheron Analytics 的公司合作。我有处理技术问题的实践经验，同时也帮助领导团队制定战略，以最大化他们的数据。

**[Seattle Data Guy 的 YouTube 频道](https://www.youtube.com/c/SeattleDataGuy/videos)**

### 在社交网络上与我联系

✅ YouTube: [https://www.youtube.com/channel/SeattleDataGuy](https://www.youtube.com/channel/UCmLGJ3VYBcfRaWbP6JLJcpA)

✅ 网站: [https://www.theseattledataguy.com/](https://www.theseattledataguy.com/)

✅ LinkedIn: [https://www.linkedin.com/company/18129251](https://www.linkedin.com/company/18129251)

✅ 个人 LinkedIn: [https://www.linkedin.com/in/benjaminrogojan/](https://www.linkedin.com/in/benjaminrogojan/)

✅ FaceBook: [https://www.facebook.com/SeattleDataGuy](https://www.facebook.com/SeattleDataGuy)

**简介: [Ben Rogojan](https://www.theseattledataguy.com/data-science-consulting-blog)** 是一位数据科学和数据工程解决方案架构师，专注于数据架构和统计学。他专注于开发端到端的数据解决方案，帮助将数据从原始格式转化为数据产品和分析。他曾为医疗保健、金融、SaaS 和技术领域的客户提供项目服务，并在大型科技公司工作过。他在网络上保持强大的存在，为数据工程师和那些对进入数据工程职业路径感兴趣的人创建内容，并在线上被称为“Seattle Data Guy”。

[原文](https://towardsdatascience.com/mlops-and-machine-learning-roadmap-95efc59927a2)。经许可转载。

**相关内容：**

+   [何时重新训练机器学习模型？运行这 5 个检查来决定计划](//2021/07/retrain-machine-learning-model-5-checks-decide-schedule.html)

+   [将 ModelOps 纳入您的 AI 战略](/2021/08/modelops-ai-strategy.html)

+   [MLOps 最佳实践](/2021/07/mlops-best-practices.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT

* * *

### 更多相关主题

+   [完整的 MLOps 学习路线图](https://www.kdnuggets.com/2022/12/complete-mlops-study-roadmap.html)

+   [KDnuggets 新闻，12 月 14 日：3 门免费的机器学习课程](https://www.kdnuggets.com/2022/n48.html)

+   [完整的机器学习学习路线图](https://www.kdnuggets.com/2022/12/complete-machine-learning-study-roadmap.html)

+   [机器学习算法选择路线图](https://www.kdnuggets.com/roadmap-to-machine-learning-algorithm-selection)

+   [提示工程师：技能、学习路线图和薪资](https://www.kdnuggets.com/prompt-engineer-skills-learning-roadmap-and-salary)

+   [掌握数据科学、数据工程、机器学习等的 25 门免费课程](https://www.kdnuggets.com/25-free-courses-to-master-data-science-data-engineering-machine-learning-mlops-and-generative-ai)
