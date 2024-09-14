# MLOps 工程师在组织中的角色

> 原文：[https://www.kdnuggets.com/2023/04/role-mlops-engineer-organization.html](https://www.kdnuggets.com/2023/04/role-mlops-engineer-organization.html)

![MLOps 工程师在组织中的角色](../Images/3872949d0e19755bcfde6f3499c78d7e.png)

作者提供的图片

所以你已经构建了一个机器学习模型，并且它在验证数据集上达到了预期的性能。你很高兴能够运用你的数据科学和机器学习技能来构建这个模型。然而，你意识到模型在本地计算机上的 Jupyter notebook 中表现良好，但在实际应用中尚未真正发挥作用（尚未）。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT

* * *

为了让用户从你的模型中受益，并让企业能够利用机器学习，这些模型必须部署到生产环境中。然而，*部署和维护机器学习模型*并非没有挑战。在开发环境中表现良好的模型可能在生产环境中表现不佳。这可能是由于数据和概念漂移以及其他导致性能下降的因素造成的。

所以你意识到：为了使机器学习模型有用，你必须*超越模型构建*。这就是 MLOps 发挥作用的地方。今天，我们将了解 MLOps 以及**MLOps 工程师在组织中的角色**。

# 什么是 MLOps？

更多时候，你会看到 MLOps 被定义为*将 DevOps 原则应用于机器学习*。

软件开发生命周期（SDLC）因 DevOps 实践而发生了积极变化，简化了开发团队与运维团队之间的跨职能协作。如果你认识在 DevOps 领域工作的人，你可能听说过 CI/CD 管道、自动化 CI/CD 管道、应用监控等。

虽然这些概念可以迁移到机器学习应用中，但机器学习系统有其特定的挑战。构建和运营机器学习系统是一个更复杂的过程。

因此，一般来说，你可以将 MLOps 视为**一套用于构建、部署和维护机器学习系统的最佳实践**。

了解了这些概念后，让我们继续学习 MLOps 工程师在组织中具体做什么。

# MLOps 工程师的工作是什么？

我们说过，将DevOps实践应用于机器学习系统是可能的。如果这就是MLOps，那么MLOps工程师的职责就是做到这一点！

我们说的是什么意思？一旦数据科学团队构建了模型，MLOps工程师通过以下方式确保模型的成功运行：

+   自动化机器学习模型部署

+   为ML管道设置监控

+   自动化CI/CD管道以考虑数据、代码和模型的变化

+   设置自动化模型重新训练

+   确定所需的自动化水平

## 与MLOps相关的一些挑战

设置*监控*只能帮助识别问题出现的时机。为了获得不同版本模型性能的更详细信息，MLOps工程师通常使用*模型版本控制*和*实验跟踪*。

我们提到过，MLOps工程师设定了*模型重新训练*的所需自动化水平。让我们尝试理解与之相关的挑战。

一旦应用程序部署到生产环境，模型所使用的数据（在生产环境中）可能与其训练时的数据有很大不同。因此，这样的模型表现可能较差，我们通常需要重新训练。

MLOps工程师还需要处理模型重新训练的过程和自动化，包括考虑性能下降、数据变化的频率以及模型重新训练的成本。

**等等，我做MLOps。但我不是MLOps工程师。**

在一些初创公司中，你可能会有既担任机器学习工程师又担任MLOps工程师的角色。而在其他公司中，你可能会有同时担任DevOps和后端工程师的角色。

在大科技公司中，MLOps的工作可能与早期阶段的创业公司中的MLOps大相径庭。MLOps的自动化程度也可能因组织而异。

如果你在一个初创公司工作，负责从模型训练到监控和维护机器学习系统的端到端机器学习管道，你也已经是MLOps工程师。

你是否对探索这个具有挑战性的MLOps工程师角色感到兴奋？让我们总结一下你需要的技能。

# MLOps技能和工具概述

MLOps工程师通常具有强大的机器学习、DevOps和数据工程技能。

![MLOps工程师在组织中的角色](../Images/1b1bd8cd7a214f85ddab6c7f545d3f9b.png)

图片由作者提供

+   **机器学习技能**：编程、对机器学习算法和框架的工作知识，以及领域知识

+   **软件工程技能**：查询和操作数据库、测试ML模型、Git和版本控制、如FastAPI等框架

+   **DevOps基础**：精通Docker和Kubernetes等工具

+   **实验跟踪**：熟悉如MLflow等实验跟踪框架

+   **编排数据管道**：使用Prefect和Airflow等工具设置和自动化数据管道

+   **云基础设施**：熟悉云基础设施提供商，如 AWS、GCP，以及像 Terraform 这样的基础设施即代码（IaC）工具

# 学习 MLOps

如果你对学习 MLOps 感兴趣，这里有一个资源列表可以帮助你入门：

+   [DataTalks.Club 的 MLOps Zoomcamp](https://github.com/DataTalksClub/mlops-zoomcamp)：DataTalks.Club 提供的 MLOps zoomcamp 是一个免费的课程，涵盖了 MLOps 的所有内容——从模型构建到部署和监控的最佳实践。你将通过构建一个项目来整合所学的知识。

+   [Coursera 上的 MLOps 专项课程](https://www.coursera.org/specializations/machine-learning-engineering-for-production-mlops)：DeepLearning.AI 提供的生产环境机器学习（MLOps）专项课程。该专项课程（包括四门课程）将教你如何构建生产级机器学习系统。

+   [MLOps GitHub 仓库](/2023/02/learn-mlops-github-repositories.html)：一个经过筛选的仓库列表，用于提升 MLOps 技能。

# 总结

在这篇文章中，我们涵盖了组织中 MLOps 工程师的主要职责和关键 MLOps 技能。

如前所述，并非所有从事 MLOps 的工程师都被*称为* MLOps 工程师。我们还讨论了 MLOps 自动化的程度以及实际的日常工作可能因组织而异。

和其他角色一样，作为 MLOps 工程师成功需要软技能，如有效的沟通、协作和战略性解决问题。如果你想尝试成为一名 MLOps 工程师，祝你 MLOps 顺利！

**[Bala Priya C](https://www.linkedin.com/in/bala-priya/)** 是一位技术写作专家，喜欢创建长篇内容。她的兴趣领域包括数学、编程和数据科学。她通过撰写教程、操作指南等，与开发者社区分享她的学习成果。

### 更多相关主题

+   [一本将彻底改变你组织... 的新书](https://www.kdnuggets.com/2022/02/manning-new-book-revolutionize-way-organization-approaches-data.html)

+   [如何在 Pandas 中使用 MultiIndex 进行层次数据组织](https://www.kdnuggets.com/how-to-use-multiindex-for-hierarchical-data-organization-in-pandas)

+   [获得数据工程师职位：免费课程和认证](https://www.kdnuggets.com/landing-a-data-engineer-role-free-courses-and-certifications)

+   [什么是 MLOps 工程师？](https://www.kdnuggets.com/2022/03/mlops-engineer.html)

+   [成为 MLOps 工程师所需的唯一免费课程](https://www.kdnuggets.com/the-only-free-course-you-need-to-become-a-mlops-engineer)

+   [人工智能在数字营销中的作用](https://www.kdnuggets.com/the-role-of-ai-in-digital-marketing)
