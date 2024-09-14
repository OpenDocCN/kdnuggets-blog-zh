# 从 Google Colab 到 Ploomber 管道：利用 GPU 扩展的机器学习

> 原文：[https://www.kdnuggets.com/2022/03/google-colab-ploomber-pipeline-ml-scale-gpus.html](https://www.kdnuggets.com/2022/03/google-colab-ploomber-pipeline-ml-scale-gpus.html)

![从 Google Colab 到 Ploomber 管道：利用 GPU 扩展的机器学习](../Images/4e5f6cb13efc294c8e256db05250c5be.png)

# **背景**

Google Colab 相当简单，你可以在一个受管的 Jupyter 环境中打开笔记本，使用免费的 GPU 进行训练，并共享驱动器笔记本，它在某种程度上也具有 Git 界面（将笔记本镜像到一个仓库）。我希望将我的笔记本从探索性数据分析（EDA）阶段扩展到一个可以在生产中运行的工作管道。拥有更好的 Git 集成以存储工件和与团队协作也很重要。Ploomber 使我实现了这一目标，同时通过逐步构建变化的任务加快了开发阶段。

# **它是如何工作的？**

我开始通过引导我的 Colab 安装一些依赖项（`pip install ploomber`）。我使用了 Ploomber 提供的第一个管道示例 - 获取数据、清理数据和可视化数据。管道结构在 pipeline.yaml 文件中指定，该文件由任务（可以是函数、.py 脚本或笔记本，即我图中的节点）和其产品组成，每个产品可以是许多东西，如原始/清洁数据、执行过的笔记本、HTML 报告等。

![XXXXX](../Images/4095e6fb981bc39d749f851d11cf4dcb.png)

这是我通过 Ploomber 运行的管道，获取原始的 Covid 数据，进行数据分析，随后清理和转换数据，然后可视化清洁数据以及分析数据。

在下图中，你可以看到 Ploomber 和我的 Colab 是同步的，从 Git 仓库加载和推送，并且我可以直接从 Colab 更改组成管道的文件。如果我完成了我的 POC，我可以通过单一命令将其部署到云端，例如 Airflow、Kubernetes 或 AWS。

![从 Google Colab 到 Ploomber 管道：利用 GPU 扩展的机器学习](../Images/76bacfd0a57aeff54823b8de4a132427.png)

Colab 笔记本是从 Git 加载的（来源于 .ipynb 文件），它在后台运行 Ploomber，允许将原始代码推送到 Git 以及进行单一命令的云部署。

# **好处**

这一补充让我能够快速开发，并从一个大型笔记本转变为一个可扩展的管道。我随后将所有代码推送到 GitHub，现在我们可以作为团队协作并同时工作 - 只有真正的代码差异被保存。此外，我的笔记本不是按顺序运行，这意味着通过并行化实现更快的运行。这也标准化了工作，因此如果我们想将这个项目复制到不同的业务用例中（这就像有一个模板），它将需要更少的维护，并且学习曲线将显著缩短。

# **什么是 Ploomber？**

Ploomber（https://github.com/ploomber/ploomber）是一个开源框架，帮助数据科学家快速开发数据管道并将其部署到任何地方。通过直接从笔记本使用它，它允许你跳过数周的重构，专注于建模工作而不是基础设施。我们有一个非常活跃的社区，你可以[在这里查看](https://ploomber.io/community)。

感谢阅读！

**[Ido Michael](https://www.linkedin.com/in/ido-michael/)** 共同创办了Ploomber，旨在帮助数据科学家更快地构建数据管道。他曾在AWS领导数据工程/科学团队。他与团队一起单独构建了数百条数据管道。来自以色列，他来到纽约攻读哥伦比亚大学的硕士学位。在发现项目通常需要将大约30%的时间用于将开发工作（原型）重构成生产管道后，他专注于构建Ploomber。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在IT领域

* * *

### 更多相关内容

+   [Ploomber与Kubeflow：让MLOps更简单](https://www.kdnuggets.com/2022/02/ploomber-kubeflow-mlops-easier.html)

+   [使用Ploomber、Arima、Python和Slurm进行时间序列预测](https://www.kdnuggets.com/2022/03/time-series-forecasting-ploomber-arima-python-slurm.html)

+   [掌握GPU：Python中GPU加速DataFrames的初学者指南](https://www.kdnuggets.com/2023/07/mastering-gpus-beginners-guide-gpu-accelerated-dataframes-python.html)

+   [利用CuPy在Python中发挥GPU的强大作用](https://www.kdnuggets.com/leveraging-the-power-of-gpus-with-cupy-in-python)

+   [在Google Colab上免费微调LLAMAv2与QLora](https://www.kdnuggets.com/fine-tuning-llamav2-with-qlora-on-google-colab-for-free)

+   [在Google Colab上免费运行Mixtral 8x7b](https://www.kdnuggets.com/running-mixtral-8x7b-on-google-colab-for-free)
