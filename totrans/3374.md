# 通过 AI 实现 IoT 的**持续改进** / **持续学习**

> 原文：[https://www.kdnuggets.com/2016/11/continuous-improvement-iot-ai-learning.html](https://www.kdnuggets.com/2016/11/continuous-improvement-iot-ai-learning.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png)[评论](#comments)

**概要**

[![通过 AI 实现 IoT 的持续改进](../Images/08237e2fdfda5c6e9420d8bafa23ba09.png)](/wp-content/uploads/continuous-improvement-Iot-ai.png)

在这篇文章中，我们讨论了如何在 IoT 系统中实施持续改进和持续学习。持续改进是一个众所周知的概念（例如来自 Kaizen 或 Six Sigma）。对于 IoT，我们建议实施持续改进时，必须实施持续学习。具体来说，这意味着我们在一个环境中训练一个模型，并将其部署到 IoT 生态系统中的多个点（设备级、工作流级和系统级）。模型可以在各个设备之间构建和部署，以在不同点捕捉学习。这些学习然后通过一个中央模型进行整合。新数据会持续监控，当数据模式发生变化时，模型会重新校准以适应最新的知识。因此，这个模型与企业 AI 相关联。我们将在即将推出的[企业 AI 实施课程，2024年1月开课](/2016/11/futuretext-implementing-enterprise-ai-course.html)中讨论这些想法。

**IoT 数据分析——数据流动和分析应用**

在我们讨论**持续改进**和**持续学习**的问题之前，我们首先讨论一个问题：*你可以在 IoT 生态系统中的哪些点应用分析？*

通常，传感器生成的数据是时间序列格式，并且通常带有地理标签。这意味着，IoT 的分析有两种形式：**时间序列**和**空间分析**。时间序列分析通常会导致像异常检测这样的洞察。因此，**异常检测**用来检测异常模式。在此基础上，通过查看历史趋势、流数据、结合多个事件的数据（**传感器融合**），我们可以获得新的洞察。

IoT 设备生成大量数据。通常，IoT 分析的目标是尽可能接近事件地分析数据，因为我们需要**实时**交互。因此，对于 IoT 分析，我们经常需要执行边缘处理——这是我在[IoT 边缘分析](/2016/09/evolution-iot-edge-analytics.html)中讨论过的主题。

此外，你可以在多个 IoT 流中关联数据。通常，在**流处理**中，我们试图了解现在发生了什么（而不是过去发生了什么）。传感器数据可以结合起来推断事件（**复杂事件处理**）。最后，你可以在**数据湖**中应用这些模型。

因此，IoT 分析（IoT 的数据科学）可以应用于 IoT 生态系统中的多个地方：边缘、流、数据湖，以及像复杂事件处理这样的模式。

**物联网分析：实施持续改进**

物联网数据的流动和物联网分析的应用显示在图表的右侧。左侧则讲述了持续改进。持续改进并不是一个新概念。它是[精益管理和六西格玛](kaizen%20https:/www.isixsigma.com/methodology/kaizen/kaizen-six-sigma-ensures-continuous-improvement/)的一部分，通过反馈循环等技术实现。

**在这里，我们提出，在物联网的实施层面，持续改进与持续学习相关联。** 具体而言，

+   每个设备都有一个状态和一个‘智能’——构建和/或部署模型的能力。模型可以在设备之间构建和部署，以在不同点捕捉学习。

+   这些学习随后通过中央模型进行整合。

持续学习可以在物联网设备的三个层级上实施

+   **设备级别分析**主要关注设备的状态和报告其状态，监控条件如阈值。

+   **工作流级别分析**主要关注一个或多个物理设备上的链式过程状态。

+   **系统级别分析**主要关注历史趋势和推断新的分析。

在这个背景下，**分析**是一个模型（基于数据集训练的算法）。分析通常可以在系统级别开发。一旦开发完成，它可以部署到设备（**操作级别**）或与其他分析链接以创建一个**工作流**（可能跨多个设备）。分析可以在**多个层级进行协调**。

**持续学习和持续改进的实施**

通过不断学习的模型实施持续改进的想法虽然复杂但并非纯概念。在行业中有许多实际例子，最著名的是[Predix（GE）](https://www.ge.com/digital/sites/default/files/APM-Asset%20Performance%20Management-GE-Digital-infographic.pdf)和[西门子（工业4.0）](https://www.siemens.com/content/dam/mam/tag-siemens-com/projects/customer-magazine/print-archiv/advance/adv152-en-screen.pdf)。

我们还可以看到在各种地方，如[Salesforce.com Einstein](https://www.salesforce.com/eu/products/einstein/overview/)、[Dell Edge分析与Statistica](https://www.dell.com/en-us/work/learn/internet-of-things-analytics)、[IBM的PMML实现](https://www.ibm.com/developerworks/library/ba-ind-PMML1/)、[Databricks(Spark)的PMML支持](https://databricks.com/blog/2015/07/02/pmml-support-in-apache-spark-mllib.html)、[Python中的PMML支持](http://stackoverflow.com/questions/33221331/export-python-scikit-learn-models-into-pmml)以及[H2O.ai的POJO](http://docs.h2o.ai/h2o/latest-stable/h2o-docs/productionizing.html)，这些模型在一个平台上创建并在其他平台上部署。我们还可以看到它在[DevOps作为持续改进模型](https://www.zdnet.com/article/a-better-name-for-devops-continuous-improvement/)方面的应用，即角色的模糊和开发与运营的整合，分析处于中心位置。这也符合我一直在谈论的一个模型，即[企业的AI层](http://www.opengardensblog.futuretext.com/archives/2016/09/the-ai-layer-for-the-enterprise-and-the-role-of-iot.html)。

**结论**

在物联网的实施层面上，我们提出持续改进与持续学习相结合。具体来说，

1.  持续改进需要持续学习。

1.  每个设备都有一个状态和一个“智能”，并且可能具有构建和/或部署模型的能力。

1.  模型可以跨设备构建和部署，以捕捉各个点的学习。这些学习然后通过中央模型进行整合。

1.  通过不断学习的模型来实现持续改进的理念复杂但并非纯粹概念性的。在行业中有许多实际的这种模型的例子。

我们将在即将开设的[实施企业人工智能课程（从2017年1月开始）](/2016/11/futuretext-implementing-enterprise-ai-course.html)中讨论这些想法。

**个人简介：** [Ajit Jaokar](https://www.linkedin.com/in/ajitjaokar)的工作涉及物联网、预测分析和移动性的研究、创业和学术。他目前的研究重点是将数据科学算法应用于物联网应用。这包括时间序列、传感器融合和深度学习（主要在R/Apache Spark中）。这一研究支撑了他在牛津大学（物联网数据科学）和UPM（马德里）的“城市科学”项目的教学。Ajit还是UPM（马德里大学）新成立的未来城市AI/深度学习实验室的主任。

**相关：**

+   [工业4.0内幕：第四次工业革命的驱动力是什么？](/2016/10/industry-40-fourth-industrial-revolution.html)

+   [KDnuggets顶级博主：与Ajit Jaokar关于物联网和数据科学的访谈](/2016/10/top-blogger-interview-ajit-jaokar-iot-data-science.html)

+   [物联网（IoT）的数据科学：与传统数据科学的十个区别](/2016/09/data-science-iot-10-differences.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 部门

* * *

### 更多相关话题

+   [停止学习数据科学以寻找目标，并寻找目标以...](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [一个 90 亿美元的 AI 失败案例分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [成功的数据科学家的五大特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么使 Python 成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每个数据科学家都应该了解的三个 R 语言库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
