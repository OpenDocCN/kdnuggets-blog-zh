# Ploomber 与 Kubeflow：让 MLOps 更简单

> 原文：[https://www.kdnuggets.com/2022/02/ploomber-kubeflow-mlops-easier.html](https://www.kdnuggets.com/2022/02/ploomber-kubeflow-mlops-easier.html)

在这篇简短的文章中，我将尝试总结 MLops 工具[Ploomber](https://ploomber.io/)和[Kubeflow](http://kubeflow.org)之间的主要区别。我们将讨论 Ploomber、Kubeflow 管道的背景信息，以及为什么我们需要这些工具来让我们的生活更轻松。

我们将从 3 个主要领域来看这些差异：

+   **易用性**

+   协作与快速迭代

+   调试、测试和可重现性

所以让我们深入了解吧！

## 背景

让我们从对常见数据/机器学习工作流的简要解释开始，了解为什么我们需要协调，它将如何帮助你更好更快地完成工作。

通常，当一个组织拥有数据并希望从中产生见解或预测（以推动业务结果）时，他们会聘请数据科学家或机器学习工程师来探索数据、准备数据并生成模型。这 3 项任务随后可以统一成一个数据管道，其中包括相关的任务：获取数据、清洗数据和训练模型。这种架构对数据管道来说相当基础，我们将为每个任务设定输入和输出，这就是定义管道内依赖关系的方式。

![Ploomber vs Kubeflow：让 MLOps 更简单](../Images/5d2d12756e30ab30e9acc55f3f888075.png)

**ML-Basic 架构**：我们运行的所有任务的示例（获取数据、工程化特征、合并数据），以适应我们的模型。（我发现用图表来解释要比深入代码更容易，“一图胜千言”）

## 什么是协调，为什么我们需要它？

一旦我们向流程中添加更多任务，它会突然变得更加复杂。在某些情况下，特别是在生产环境中，流程可能有并行任务，这些任务可以有一对多的输入和输出，但它总是从一个地方开始，结束于另一个地方。这通常被称为[有向无环图（DAG）](https://www.tutorialspoint.com/directed-acyclic-graph-dag)。DAG 是机器学习工作流的表示方式，在今天大多数常见的框架中，如 Ploomber、Kubeflow、Airflow、MLflow 等，它是控制和展示数据管道的方式。是的，这就是你在算法课程中学到的相同的 DAG 概念！（那些上过这门课的人）。一旦定义了每个任务的表示，协调器将遵循其顺序并执行每个任务、它的依赖项和输出，并在必要时重试（我们可以参考上面的 DAG）。

## Kubeflow 与 Ploomber

现在进入主要内容，我们对基本的数据管道、常见结构和概念有了一些了解，了解了为什么我们需要这些。我们将从 4 个不同的角度审视这两种工具，以了解它们之间的差异，以及在何时需要使用每种工具。

### **易用性**

我认为工具的价值来自于整体体验——设置、额外步骤、维护以及每个步骤的使用便利性。当我需要部署 Kubeflow 集群时，我遇到的第一个障碍是文档，[大量页面](https://www.kubeflow.org/docs/distributions/)和部署选项，此外，我后来发现我不能在我的笔记本电脑上本地运行，如果你想在考虑生产之前测试你的工作，这是一种相当基本的要求。大多数数据科学项目以“研究实验”开始，是否能够进入生产是不确定的。第一个问题之一是数据是否正确，我们是否有足够的数据？这里的最佳想法是通过启动本地环境而不是庞大的设置，迅速迭代这些问题。

现在回到设置上，我还发现安装后有两个版本，所以我继续使用旧版本（我不打算再经历一次漫长的安装过程）。一旦在我为其开设的云账户上配置了集群，我意识到我必须使用复杂的 Python API，并且在 Docker 上运行有一些限制（必须将任务保持为自由文本）。在这个过程中，我尝试了一个应该能简化这一切的框架，特别是在笔记本方面，Kale，但没有成功。另一方面，使用 Ploomber 从本地开始是直接的，我只需运行 `pip install ploomber`，然后可以从模板管道开始，在我的笔记本上进行开发。文档的结构方式是每个用例都有一本食谱，概念在一个地方得到解释。它支持本地和云端部署，所以一旦迭代完成，我可以直接提交任务到我的 Kubeflow 集群。对于基本操作有 CLI API，对于更高级的用户有 Python API。

### **协作和快速迭代**

在 Kubeflow 中，当我每次需要通过容器运行时，它会拉取镜像，并且大约需要 1 分钟才能开始运行任务。此外，我无法真正登录到容器中查看发生了什么，像使用哪个镜像、当前使用了哪些依赖等基本信息都不可见。容器中的输出非常困难，有一个特定的位置可以保存它们，而完全没有参数化。最终，我不得不使用云存储来加快迭代速度。在 Ploomber 中，我可以简单地使用 pip 虚拟环境/conda 并以我想要的方式锁定版本（在生产环境中，这一点非常相关，因为 Jupyter 对新包的访问有限）。此外，由于代码通过 Python 在本地运行，我可以迭代每个任务并确切了解发生了什么，而无需为此创建整个新管道或更改代码。你可以准确地定义输出的保存位置，无论是本地还是云端。此外，由于笔记本代码被转换为脚本，我可以将代码推送到 Git 中，并与数据科学团队的其他成员协作。

### **调试、测试和可重现性**

由于 Kubeflow（或者至少是我运行的 MiniKF）运行在云集群上，并且代码在容器内运行，因此调试和测试代码相当困难。我在他们的文档中找不到如何登录到容器并开始调试会话的说明。这使得测试代码非常困难。除此之外，由于无法登录到运行环境中查看当前的数据帧和不同的工件，因此几乎不可能重现每次运行。另一方面，在 Ploomber 中，不仅可以立即开始调试会话，而且还可以登录到你的 Docker 容器中，了解依赖关系。当代码是模块化的时，测试起来要容易得多，你不需要等待整个管道运行完成。

![Ploomber vs Kubeflow: Making MLOps Easier](../Images/ac2ace82ae0465d2664ca4defef26b4d.png)

在 Jupyter 实例内部的交互式会话示例中，查看 dag 及其任务与远程运行的黑箱相比。

## 当 Kubeflow 变得有意义时

如果你已经准备好一个管道，并且在寻找一个高性能集群来部署它，经过所有的交互式数据探索、分析和迭代后，那么 Kubeflow 是一个不错的用例。当你在早期阶段，需要快速迭代数据和调整管道时，Ploomber 将是更好的选择，因为它简化了这一过程。你可以快速迭代你的工作流程，动作更快，当你准备好时，Kubeflow 连接器将允许你无缝部署最终的数据管道。

## 结论

总体而言，这是一次启发性的经历，让我理解了现有 Kubeflow 架构中的所有缺口，并通过 Ploomber 提供了解决方案。我相信数据科学和 MLops 的工作应该是简单、可维护和可重用的。我理解 Kubeflow 的定位，希望 V2 能为用户提供更顺畅的体验。在开发工具和基础设施中，选择不仅能解决问题的工具，还要容易上手且维护友好（如 Ploomber！），毕竟，工具在几个月内被替换的可能性不高。

感谢您一直读到这里！如果您正在寻找更好的解决方案来协调工作流程，不妨尝试一下 [Ploomber](https://ploomber.io)。

**[Ido Michael](https://www.linkedin.com/in/ido-michael/)** 共同创办了 Ploomber，以帮助数据科学家提高工作效率。他曾在 AWS 领导数据工程/科学团队。在与客户合作期间，他和团队单独构建了数百条数据管道。来自以色列的他来到纽约，攻读哥伦比亚大学的硕士学位。在发现项目通常将大约 30% 的时间用于将开发工作（原型）重构为生产管道后，他专注于建设 Ploomber。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织进行 IT 工作

* * *

### 更多相关话题

+   [从 Google Colab 到 Ploomber 管道：利用 GPU 实现大规模 ML](https://www.kdnuggets.com/2022/03/google-colab-ploomber-pipeline-ml-scale-gpus.html)

+   [使用 Ploomber、Arima、Python 和 Slurm 进行时间序列预测](https://www.kdnuggets.com/2022/03/time-series-forecasting-ploomber-arima-python-slurm.html)

+   [如何简化代码文档编写](https://www.kdnuggets.com/2022/12/make-documenting-code-easier.html)

+   [数据质量在成功构建机器学习模型中的重要性](https://www.kdnuggets.com/2022/03/significance-data-quality-making-successful-machine-learning-model.html)

+   [让智能文档处理更智能：第 1 部分](https://www.kdnuggets.com/2023/02/making-intelligent-document-processing-smarter-part-1.html)

+   [从数据分析师到数据策略师：如何通过职业生涯产生影响](https://www.kdnuggets.com/2023/05/data-analyst-data-strategist-career-path-making-impact.html)
