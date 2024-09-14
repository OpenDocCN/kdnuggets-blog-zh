# 数据科学项目基础设施：如何创建它

> 原文：[https://www.kdnuggets.com/2021/08/data-science-project-infrastructure.html](https://www.kdnuggets.com/2021/08/data-science-project-infrastructure.html)

[评论](#comments)

![](../Images/396024f37481e6c3c20242af4d4808dd.png)

任何数据科学项目的主要起点是商业或任何其他现实生活中的问题。这是你在决定数据科学项目应该如何时需要记住的最重要的一点。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 相关工作

* * *

当你为项目构建数据科学基础设施时也是一样。你是在为其他人使用而构建你的项目及其基础设施。因此，我不会过多涉及基础设施的技术细节。我将在这里提供一些建议，如果你想要构建一个有目的的数据科学项目，你应该牢记这些建议。当然，这也包括一些技术建议。

遵循这些建议将使问题解决保持在你项目的核心位置，这正是它应有的状态。它们还将帮助你建立 [展示你技能所需的唯一数据科学项目](https://www.youtube.com/watch?v=c4Af2FcgamA)。

## 建立稳固的数据科学项目基础设施的四个步骤

构建一个稳固的数据科学基础设施并不容易，尤其是当你经验不足时。但为了确保安全，请使用此清单确保你走在正确的道路上：

+   使用 API 和其他技术获取真实数据

+   使用（云）数据库存储数据

+   构建一个模型

+   部署你的模型

通过遵循这些步骤，你将通常遵循所谓的 OSEMN 框架。这是一个概述你数据科学项目应该经过的过程的框架：

![](../Images/47f3ceab4fe9f725a210547f52ac55a5.png)

*来源：TOWARDS DATA SCIENCE*

现在我将详细说明每一点。

### 使用 API 和其他技术获取真实数据

你是在用你的项目解决实际问题，对吧？使用真实数据来做这些事情是很自然的。我指的是用户生成的数据，这些数据是实时更新的，比如流数据。如果你使用这些数据，你的项目将变得不那么理论化。你正在处理的是今天相关的数据，需要不断更新，这也考验了你处理这些数据的能力。这才是挑战所在，而不是仅仅在一些历史数据集上进行实践，尽管那也有其目的。

主要的问题是你如何以及在哪里获取真实的数据。你通过使用 API 来获取这些数据。为了获取所需的真实数据，你必须知道如何使用、配置和设置 API。有一种简单的方法可以帮助你可视化 API 是什么以及它们的功能：

![](../Images/1d23320b6f998c1cdbd9e3c3e2ce49a5.png)

*来源：ALTEXSOFT*

一些受欢迎的 API 有：

+   [推特](https://developer.twitter.com/en/products/twitter-api)

+   [谷歌分析](https://developers.google.com/analytics)

+   [YouTube](https://developers.google.com/youtube/v3)

+   [亚马逊](https://aws.amazon.com/api-gateway/)

使用这些技术将允许你获取的数据包括：

+   实时更新

+   每条记录的数据和时间戳

+   地理位置

+   数字和文本数据

了解如何使用 API 是数据科学行业中极力推荐的技能之一。首先，用静态数据构建机器学习模型是没有意义的。这就是为什么你（可能）需要知道如何使用 API。它们将允许你自动化数据更新。如果你使用 API，你可以避免创建集成层，因为 API 也是不同系统之间的通信平台。如果你掌握了 API，你还可以在以后部署你的模型，甚至构建将使用这些实际数据的应用程序。

[这篇 Springboard 文章](https://www.springboard.com/library/data-science/top-apis-for-data-scientists/) 清楚地解释了什么是 API 以及哪些 API 推荐给数据科学家。每个推荐都有 API 文档和教程的链接以及一些在线资源用于实践 API。

使用 API 时，你还需要使用其他技术，例如：

+   帮助你进行 API 调用的库

+   数据结构如 JSON 和字典，用于从 API 收集和保存数据

一旦你获得了数据，你需要将其存储在某个地方，对吧？这是在构建基础设施中的另一个重要步骤。

### 使用云数据库存储数据

由于你处理的是实时数据，建议你将数据存储起来。否则，你将不得不不断从 API 中提取所有数据，包括你已经拥有的数据以及此期间出现的新数据。

当你使用数据库存储数据时，你只需提取新数据，清理它，并将其附加到数据库中已存在的干净数据中。一些最受欢迎的云数据库是：

+   [亚马逊网络服务](https://aws.amazon.com/)

+   [Google Cloud](https://cloud.google.com/)

但为什么你特别应该使用云数据库？假设你在做一个相对较大的项目，并且有大量自动更新的实时数据，在这种情况下，云数据库将允许你以相对便宜的方式存储这些数据，并具有几乎无限的存储容量。例如，Amazon Web Services和Google Cloud也提供运行机器学习算法的可能性。如果你使用内部数据存储，你将无法做到这一点。而且你也不必担心备份和数据可用性。

如果你为某家公司工作，尤其是那些生成大量数据的大公司，你很可能需要精通（或至少想要学习）云计算。公司之所以转向云数据库，正是因为你也应该这样做的原因。即使你现在还不是数据科学家，也不意味着你不能成为。看看这个 [如何获得数据科学职位的指南](https://www.stratascratch.com/blog/how-to-get-a-data-science-job-the-ultimate-guide/)，我相信它会帮助你实现目标。

[Pupuweb上的一篇精彩文章](https://pupuweb.com/enterprise-cloud-database/)值得阅读，如果你想了解云数据库的基本特征、其提供者、优缺点，以便决定哪个选项最适合你。

如果你不想自己学习云数据库，可能可以尝试一个在线课程。我认为 [这个Coursera课程](https://www.coursera.org/learn/cloud-data-engineering-duke#syllabus) 可能是一个很好的起点。这是杜克大学的课程，而且是免费的。如果你参加这个课程，你将与Amazon Web Services、[Azure](https://azure.microsoft.com/en-us/free/) 和Google Cloud Platform合作。

### 建立模型

一旦你有了前两个基础设施元素，就可以开始建立模型。当你建立回归或机器学习模型时，你需要再次记住模型应该解决实际问题。要建立这样的模型，你应该问自己以下问题：

+   我试图通过这个模型完成什么？为什么我选择了这个模型而不是其他模型？

+   我如何清理数据，为什么这样做？

+   我将对数据执行什么验证测试，以使其适合模型？

+   模型的假设是什么，我将如何验证它们？

+   我应该如何优化模型？我应该做出哪些权衡决策？

+   我应该如何实施测试/控制？

+   模型中的基础数学是什么，它是如何工作的？

使你能够提出好问题的主要工具是经验。然而，要获得经验，你需要建立一些模型。为了建立这些模型，你还需要一些技术工具来帮助你。构建机器学习模型的主要工具类别包括：

1.  机器学习工具包

1.  机器学习平台

1.  分析解决方案

1.  数据科学笔记本

1.  云原生机器学习服务（MLaaS）

首先，我推荐这篇[Forbes文章](https://www.forbes.com/sites/cognitiveworld/2021/05/30/the-five-ways-to-build-machine-learning-models/?sh=3857d08311a8)来熟悉每个类别所提供的内容。

如果你掌握了Python编程、数学和统计学知识，并且有一定的机器学习基础，一些课程可以进一步提升你的模型构建技能。例如，AT&T提供了[一个免费的Udacity课程](https://www.udacity.com/course/model-building-and-validation--ud919)，专注于构建优秀模型所需的关键问题。

通常认为数据科学家的唯一工作就是构建模型。虽然这并不完全正确，但这确实非常重要。所以我不需要强调你为什么应该擅长构建模型。

尽管构建模型看似是你基础设施拼图的最后一块，但实际上并非如此。

还需进行最后一步。确保你构建的模型确实能达到其目的，即解决现实中的问题。

### 部署你的模型

这是构建任何数据科学项目基础设施时非常重要的一部分。这最后一步真正考验了你的模型及其目的。你可以通过让其他人成为你项目的一部分来实现这一点。允许他们使用你的模型及其产生的见解。

请记住，你的工作是通过将数据转化为见解，再将见解转化为建议来帮助他人。你如何接触到其他人？理想情况下，你会部署你的模型。你可以使用如[ Django](https://www.djangoproject.com/)或[ Flask](https://palletsprojects.com/p/flask/)等应用框架来实现这一点。或者，你可以使用云服务提供商如Amazon Web Services或Google Cloud。

你可以将你的见解以简单的仪表盘形式展示给用户，用户可以进行交互。或者，也许它可以是一个API，用户可以连接到该API以获取你的见解和建议。

为什么部署模型是一个重要的步骤以及你应该掌握的技能？它将导致业务流程中有时会出现根本性的变化。曾经手动执行的任务将由基于算法的解决方案接管。只有通过部署，你的模型才能实现其解决现实问题的目的。部署模型将展示你作为数据科学家的更广泛技能：不仅是你的技术技能，还包括对业务及其流程的理解。

再次，这需要通过经验来获得。如果你缺乏经验，弥补一点技术知识总是明智的。因此，如果你已经学会了如何使用API和云服务来获取和存储模型数据，你也可以使用它们来部署模型。

如果你真的想将应用框架添加到你的技能中，我只能鼓励你。有大量的 [Flask](https://www.fullstackpython.com/flask.html) 和 [Django](https://www.fullstackpython.com/django.html) 书籍、教程和资源可以帮助你掌握这些工具。

如果你缺乏数据科学项目的想法，这里有一个建议，看看 [这八个你可以尝试的项目。](https://www.stratascratch.com/blog/8-data-science-projects-you-can-do-at-your-current-job/) 只需遵循本文中的四条指南，你应该没问题。

## 总结

创建数据科学基础设施时，构建项目的主要要点包括：

+   时刻牢记，你的工作不仅仅是构建一个模型，而是解决一个实际问题。

+   你的项目基础设施应专注于帮助你解决一个问题。

+   项目基础设施的四个主要组件应包括：

+   输入数据的 API

+   (云) 数据库

+   模型

+   洞察的 API

以这种方式构建数据科学基础设施，你将确保你的项目产生了影响。

[原文](https://cult.honeypot.io/reads/infrastructure-data-science-projects/). 已获许可转载。

**简介：** [奈特·罗西迪](https://cult.honeypot.io/contributors/nate-rosidi) 是一名数据科学家和产品经理。

**相关：**

+   [揭示 AI 基础设施堆栈](https://www.kdnuggets.com/2020/05/demystifying-ai-infrastructure-stack.html)

+   [初学者十大数据科学项目](https://www.kdnuggets.com/2021/06/top-10-data-science-projects-beginners.html)

+   [如何在 2021 年组织你的数据科学项目](https://www.kdnuggets.com/2021/04/how-organize-your-data-science-project-2021.html)

### 更多相关主题

+   [介绍 Objectiv: 开源产品分析基础设施](https://www.kdnuggets.com/2022/06/objectiv-introducing-objectiv-opensource-product-analytics-infrastructure.html)

+   [如何为你的数据项目创建采样计划](https://www.kdnuggets.com/2022/11/create-sampling-plan-data-project.html)

+   [利用你的数据科学技能创建 5 个收入来源](https://www.kdnuggets.com/2023/03/data-science-skills-create-5-streams-income.html)

+   [用 Tableau 创建高效的综合数据源](https://www.kdnuggets.com/2022/05/create-efficient-combined-data-sources-tableau.html)

+   [构建数据管道以创建大型语言模型应用](https://www.kdnuggets.com/building-data-pipelines-to-create-apps-with-large-language-models)

+   [用 ChatGPT 秒级创建惊艳的数据可视化](https://www.kdnuggets.com/create-stunning-data-viz-in-seconds-with-chatgpt)
