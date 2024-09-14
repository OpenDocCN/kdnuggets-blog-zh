# MLOps中的复杂性管理

> 原文：[https://www.kdnuggets.com/2020/05/taming-complexity-mlops.html](https://www.kdnuggets.com/2020/05/taming-complexity-mlops.html)

[评论](#comments)

**由[苏拉夫·戴](https://www.linkedin.com/in/souravdeymit/)，Manifold.ai的CTO**。

将近两年前，我的同事亚历山大·吴[分享了我们的想法](https://www.kdnuggets.com/2018/05/torus-docker-first-data-science.html)，即为什么机器学习工程师（MLEs）的兴起意味着我们需要一种DevOps方法来处理机器学习（ML），以及为什么我们的团队创建了一个开源工具，以使其他从事AI和数据科学的团队的工作更加轻松。Orbyter 1.0（当时称为Torus）的发布重点是通过简化创建一个完全配置的开箱即用的Docker化本地开发环境来简化ML管道，以支持数据科学项目。

对于刚刚发布的2.0版本，我们专注于减少偶然复杂性，同时继续添加使你的工作更轻松的功能。

### 实现最佳实践的即开即用

Orbyter 2.0内置了偶然复杂性的减少。如果你还不熟悉软件中的固有复杂性与偶然复杂性的对立面，你可以阅读[David Hayes的精彩解释](https://pressupinc.com/blog/2014/05/root-causes-software-complexity/)：

> 所有软件的复杂性可以准确地被视为属于固有复杂性和偶然复杂性这两种对立面之一。将你软件中的所有糟糕问题都拿出来，去掉那些因为难题而糟糕的部分，你剩下的就是那（小山还是大山，由你选择）的偶然复杂性……

数据科学和AI充满了难题，因此编写ML软件往往伴随着显著的固有复杂性。我们实际上无法在工具中解决这个问题；难题就是难。然而，我们可以通过对软件工程最佳实践进行规范，以减少偶然复杂性，例如：

+   使用[Black](https://black.readthedocs.io/en/stable/)符合PEP8标准的代码格式检查

+   使用[pytest](https://docs.pytest.org/en/latest/)进行轻松的单元测试

+   集中式日志设置

+   为GitHub中的持续集成添加钩子

+   符合数据管道抽象

例如，Orbyter 2.0现在允许你使用内置的本地脚本自动将代码格式化为[Python PEP 8](https://www.python.org/dev/peps/pep-0008/)标准。它还与[GitHub Actions](https://github.com/features/actions)进行了即开即用的持续集成，因此当你将代码推送到GitHub时，它会自动进行lint检查并运行单元测试。一个YAML文件提供了一个单一的位置来配置强大的日志记录。此外，代码被构建成符合隔离输入/输出架构，以允许在数据管道中快速迭代。我们还定义了一些明确的端点——train、predict、evaluate——作为Click命令行脚本，可以作为Docker入口点调用。

![](../Images/2bc22ef5c66306e53a412ecd17f81cf8.png)

*示例项目架构。*

本质上，我们尽力将良好的软件工程纪律做得尽可能一键式。你未来的自己会感谢你。

### 跟踪你的实验

尽管许多组织现在拥有强大的数据科学团队进行重要的实验和建模研发，但这些公司往往仍然难以将这些工作的结果[from the lab to the factory](https://hbr.org/2013/04/two-departments-for-data-succe)。没有人希望看到他们的辛勤工作在孤岛中消沉；兴奋在于看到你的模型在现实世界中产生影响。但要实现这种过渡需要大量的工程工作，许多组织仍然没有准备好进行生产部署。

为了解决这种脱节，我们添加了一个带有MLflow的新容器。Orbyter 1.0提供了一个单一的容器，是流行的[Cookiecutter Data Science](https://drivendata.github.io/cookiecutter-data-science/)的一个分支，适合轻松设置Docker化的工作流程。Orbyter 2.0仍然是基本的Docker化数据科学，但现在当你启动它时，你会得到三个不同的容器：第一个提供Jupyter，和以前一样；第二个提供Bash，用于运行脚本；第三个提供[MLflow](https://mlflow.org/)，用于实验跟踪。

我们认为实验跟踪是应用机器学习中的一个重要部分，并且非常喜欢使用MLflow来完成这一步骤。它还可以帮助打包以实现可重复运行，并将模型发送到部署。

### 持续降低准入门槛

我们最初开始构建Orbyter，是因为我们相信Docker和容器化是未来的发展方向。Alex在他的初始帖子中解释得很好：

> 尽管通过小心使用虚拟环境和严格的系统级配置管理可以实现相同的一致性，但容器在新环境的启动/关闭时间和开发者生产力方面仍然提供了显著的优势。然而，我们从数据科学社区那里听到的反馈是：我知道Docker会让这变得更容易，但我没有时间或资源来设置和搞清楚这一切。

Orbyter 诞生的目的是降低进入 Docker 优先工作流的门槛。通过 Orbyter 2.0 的额外改进，转向 Docker 优先工作流变得比以往更简单。转向这种 Docker 化的工作流可以使用各种云工具—包括 SageMaker、Kubernetes、ECS 等。例如：

+   我们使用 AWS Batch 为一家制药客户构建了一个 AWS 大规模实验平台。由于在 Docker 容器中进行，我们快速完成了这项工作，并在 MLflow 中进行跟踪。我们会在夜间运行任务，早上可以查看结果。

+   我们为几位客户开发了复杂的 CI/CD 管道，在 Git 标签后自动生成生产 Docker 镜像，并将其放入 ECR 中，从而可以轻松投入生产。

现在 Orbyter 本身变得更加功能丰富，我们致力于保持入门过程的简单和直接。我们创建了一个演示仓库，展示了如何使用它—公开了我们在 Strata 数据大会上展示、经过路测和完善的演示。你可以在[这里](https://github.com/manifoldai/orbyter-demo)找到它。

请留意进一步的更新，因为我们正在继续构建 Orbyter 并优化开发过程。

**相关：**

+   [揭开 AI 基础设施堆栈的神秘面纱](https://www.kdnuggets.com/2020/05/demystifying-ai-infrastructure-stack.html)

+   [部署机器学习模型意味着什么？](https://www.kdnuggets.com/2020/02/deploy-machine-learning-model.html)

+   [10 个有用的 Python 开发者机器学习实践](https://www.kdnuggets.com/2020/05/10-useful-machine-learning-practices-python-developers.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT

* * *

### 了解更多相关话题

+   [变压器的内存复杂性](https://www.kdnuggets.com/2022/12/memory-complexity-transformers.html)

+   [线性回归模型选择：平衡简单性与复杂性](https://www.kdnuggets.com/2023/02/linear-regression-model-selection-balancing-simplicity-complexity.html)

+   [MLOps 机器学习设计模式](https://www.kdnuggets.com/2022/02/design-patterns-machine-learning-mlops.html)

+   [MLOps 是一团乱麻，但这在预期之中](https://www.kdnuggets.com/2022/03/mlops-mess-expected.html)

+   [全面的 MLOps 指南](https://www.kdnuggets.com/2023/08/comprehensive-guide-mlops.html)

+   [Ploomber 与 Kubeflow：简化 MLOps 的方法](https://www.kdnuggets.com/2022/02/ploomber-kubeflow-mlops-easier.html)
