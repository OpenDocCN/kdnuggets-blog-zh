# 掌握 MLOPs 的 7 个步骤

> 原文：[https://www.kdnuggets.com/7-steps-to-mastering-mlops](https://www.kdnuggets.com/7-steps-to-mastering-mlops)

![掌握 MLOPs 的 7 个步骤](../Images/f8f63f2c75445c88efc0b4816d9d5c9b.png)

作者提供的图片

如今，许多公司希望将 AI 纳入其工作流程，特别是通过微调大型语言模型并将其投入生产。由于这种需求，MLOps 工程变得越来越重要。公司不仅仅寻找数据科学家或机器学习工程师，而是希望找到能够自动化和简化在云中训练、评估、版本控制、部署和监控模型过程的人员。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

在本初学者指南中，我们将专注于掌握 MLOps 工程的七个关键步骤，包括设置环境、实验追踪和版本控制、编排、持续集成/持续交付（CI/CD）、模型服务和部署以及模型监控。在最后一步中，我们将使用各种 MLOps 工具构建一个完全自动化的端到端机器学习管道。

# 1\. 本地和云环境设置

为了训练和评估机器学习模型，你首先需要设置本地和云环境。这涉及到使用 Docker 对机器学习管道、模型和框架进行容器化。之后，你将学习使用 Kubernetes 自动化部署、扩展和管理这些容器化应用程序。

在第一步结束时，你将熟悉你选择的云平台（例如 AWS、Google Cloud 或 Azure），并学习如何使用 Terraform 实现基础设施即代码，以自动化设置你的云基础设施。

> **注意：** 你需要对 Docker、Git 有基本了解，并熟悉命令行工具。然而，如果你有软件工程背景，你可能可以跳过这一部分。

# 2\. 实验追踪、版本控制和模型管理

你将学习如何使用 MLflow 跟踪机器学习实验，使用 DVC 进行模型和数据版本控制，以及使用 Git 进行代码版本控制。MLflow 可以用于记录参数、输出文件、模型管理和服务。

这些实践对于维护一个文档齐全、可审计且可扩展的 ML 工作流程至关重要，最终有助于 ML 项目的成功和效率。

查看 [7款最佳机器学习实验跟踪工具](/2023/02/7-best-tools-machine-learning-experiment-tracking.html)，选择适合你工作流程的工具。

# 3\. 编排和机器学习管道

在第三步中，你将学习如何使用像**Apache Airflow**或**Prefect**这样的编排工具来自动化和调度机器学习工作流程。工作流程包括数据预处理、模型训练、评估等，确保从数据到部署的管道无缝且高效。

这些工具使得机器学习流程中的每个步骤都可以在不同项目中模块化和重用，从而节省时间并减少错误。

了解 [5款数据编排的Airflow替代方案](/5-airflow-alternatives-for-data-orchestration)，这些方案用户友好且具备现代功能。同时，查看 [使用Prefect进行机器学习工作流程](https://www.datacamp.com/tutorial/ml-workflow-orchestration-with-prefect)教程，构建并执行你的第一个机器学习管道。

# 4\. 机器学习的CI/CD

将持续集成和持续部署（CI/CD）实践融入你的机器学习工作流程中。像**Jenkins**、**GitLab CI**和**GitHub Actions**这样的工具可以自动化机器学习模型的测试和部署，确保变更能够高效且安全地推出。你将学习将数据、模型和代码的自动化测试纳入其中，以便早期发现问题并维持高质量标准。

通过遵循 [机器学习的CI/CD初学者指南](https://www.datacamp.com/tutorial/ci-cd-for-machine-learning)，了解如何使用GitHub Actions来自动化模型训练、评估、版本控制和部署。

# 5\. 模型服务和部署

模型服务是有效利用机器学习模型在生产环境中的关键方面。通过使用如**BentoML**、**Kubeflow**、**Ray Serve**或**TFServing**等模型服务框架，你可以高效地将模型作为微服务进行部署，使其在多个应用程序和服务中可访问和可扩展。这些框架提供了一种无缝的方式来本地测试模型推断，并提供功能以安全高效地在生产环境中部署模型。

了解 [7款顶级模型部署和服务工具](/top-7-model-deployment-and-serving-tools)，这些工具被顶级公司用于简化和自动化模型部署过程。

# 6\. 模型监控

在第六步中，你将学习如何实现监控，以跟踪模型的性能并检测数据随时间的变化。你可以使用像**Evidently**、**Fiddler**这样的工具，甚至编写自定义代码进行实时监控和警报。通过使用监控框架，你可以构建一个完全自动化的机器学习管道，其中模型性能的任何显著下降都会触发CI/CD管道。这将导致在最新数据集上重新训练模型，并最终将最新模型部署到生产环境中。

如果你想了解构建、维护和执行端到端 ML 工作流所使用的重要工具，你应该查看[2024年需要了解的25大MLOps工具](https://www.datacamp.com/blog/top-mlops-tools)列表。

# 7. 项目

在本课程的最终步骤中，你将有机会利用迄今为止学到的所有内容构建一个端到端的机器学习项目。该项目将包括以下步骤：

1.  选择一个你感兴趣的数据集。

1.  在选定的数据集上训练模型并跟踪实验。

1.  创建一个模型训练管道，并使用 GitHub Actions 自动化它。

1.  将模型部署到批处理、Web服务或流式服务中。

1.  监控模型的性能，并遵循最佳实践。

收藏此页面：[掌握 MLOps 的 10 个 GitHub 仓库](/10-github-repositories-to-master-mlops)。利用它来了解有关 MLOps 的最新工具、指南、教程、项目和免费课程。

# 结论

你可以注册一个[ MLOps 工程师课程](/the-only-free-course-you-need-to-become-a-mlops-engineer)，该课程详细涵盖了所有七个步骤，帮助你获得必要的经验，来训练、跟踪、部署和监控生产中的机器学习模型。

在本指南中，我们了解了成为一名专业 MLOps 工程师所需的七个必要步骤。我们了解了工程师在云中自动化和简化训练、评估、版本控制、部署和监控模型过程所需的工具、概念和流程。

[](https://www.polywork.com/kingabzpro)****[Abid Ali Awan](https://www.polywork.com/kingabzpro)**** ([@1abidaliawan](https://www.linkedin.com/in/1abidaliawan)) 是一位认证的数据科学专业人士，热衷于构建机器学习模型。目前，他专注于内容创作，并撰写关于机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是使用图神经网络为面临心理疾病困扰的学生构建一个AI产品。

### 更多相关内容

+   [最后呼叫：Stefan Krawcyzk的《Mastering MLOps》直播课程](https://www.kdnuggets.com/2022/08/sphere-last-call-stefan-krawcyzk-mastering-mlops.html)

+   [2022年掌握Python机器学习的7个步骤](https://www.kdnuggets.com/2022/02/7-steps-mastering-machine-learning-python.html)

+   [掌握数据清洗的7个步骤，使用Python和Pandas](https://www.kdnuggets.com/7-steps-to-mastering-data-cleaning-with-python-and-pandas)

+   [KDnuggets™ 新闻 22:n05, 2月2日：掌握机器学习的7个步骤…](https://www.kdnuggets.com/2022/n05.html)

+   [掌握数据科学中SQL的7个步骤](https://www.kdnuggets.com/2022/04/7-steps-mastering-sql-data-science.html)

+   [掌握数据科学中Python的7个步骤](https://www.kdnuggets.com/2022/06/7-steps-mastering-python-data-science.html)
