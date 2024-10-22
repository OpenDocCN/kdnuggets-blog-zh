# 版本控制机器学习实验与跟踪实验

> 原文：[`www.kdnuggets.com/2021/12/versioning-machine-learning-experiments-tracking.html`](https://www.kdnuggets.com/2021/12/versioning-machine-learning-experiments-tracking.html)

**由 [Maria Khalusova](https://www.linkedin.com/in/maria-khalusova-a958aa14/?originalSubdomain=ca) 撰写，Iterative 的高级开发者倡导者**

![版本控制机器学习实验与跟踪实验](img/86ffbd4ee67e0886da4b36b7d3f226a0.png)

摄影： [Debby Hudson](https://unsplash.com/@hudsoncrafted?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)  在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在进行机器学习项目时，通常会进行大量实验，以寻找一种算法、参数和数据预处理步骤的组合，从而产生最适合当前任务的模型。为了跟踪这些实验，数据科学家们曾经将其记录在 Excel 表格中，因为当时缺乏更好的选择。然而，由于主要是手动操作，这种方法存在一些缺点。比如，它容易出错、不方便、速度慢，并且与实际实验完全脱节。

幸运的是，在过去几年中，实验跟踪技术取得了长足的进展，我们看到市场上出现了许多能够改善实验跟踪方式的工具，例如 Weights&Biases、MLflow 和 Neptune。通常，这些工具提供了一个 API，你可以从代码中调用以记录实验信息。然后，这些信息会存储在数据库中，你可以使用仪表盘进行可视化比较。一旦你更改了代码，就不再需要担心忘记记录结果——这会自动为你完成。仪表盘有助于可视化和共享。

这是跟踪已完成工作的一个重大改进，但…… 在仪表盘上发现产生最佳指标的实验并不自动转化为准备好部署的模型。你可能需要首先重现最佳实验。然而，你直接观察到的跟踪仪表盘和表格与实际实验的连接较弱。因此，你仍然可能需要半自动地追踪你的步骤，拼接出准确的代码、数据和管道步骤来重现实验。这能自动化吗？

在这篇博客文章中，我想谈谈实验的版本控制，而不是跟踪实验，以及这如何在实验跟踪的好处基础上带来更容易的可重复性。

为了实现这一点，我将使用 [DVC](http://dvc.org/)，这是一款开源工具，主要以数据版本控制而闻名（毕竟这也是它名字的一部分）。然而，这款工具实际上可以做很多事情。例如，你可以使用 DVC 定义机器学习管道，运行多个实验，并比较指标。你还可以对所有参与实验的移动部分进行版本控制。

## 实验版本控制

要[开始版本控制](https://dvc.org/doc/command-reference/exp/init)实验，你需要从任何 Git 仓库初始化 DVC，如下所示。请注意，DVC 期望你的项目按照一定的逻辑结构组织，你可能需要稍微重新组织一下文件夹。

```py
$ dvc exp init -i

This command will guide you to set up a default stage in dvc.yaml.
See [`dvc.org/doc/user-guide/project-structure/pipelines-files`](https://dvc.org/doc/user-guide/project-structure/pipelines-files). DVC assumes the following workspace structure:

├── data
├── metrics.json
├── models
├── params.yaml
├── plots
└── srcCommand to execute: python src/train.py
Path to a code file/directory [src, n to omit]: src/train.py
Path to a data file/directory [data, n to omit]: data/images/
Path to a model file/directory [models, n to omit]:
Path to a parameters file [params.yaml, n to omit]:
Path to a metrics file [metrics.json, n to omit]:
Path to a plots file/directory [plots, n to omit]: logs.csv
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
default:
  cmd: python src/train.py
  deps:
  - data/images/
  - src/train.py
  params:
  - model
  - train
  outs:
  - models
  metrics:
  - metrics.json:
      cache: false
  plots:
  - logs.csv:
      cache: false
Do you want to add the above contents to dvc.yaml? [y/n]: yCreated default stage in dvc.yaml. To run, use "dvc exp run".
See [`dvc.org/doc/user-guide/experiment-management/running-experiments`](https://dvc.org/doc/user-guide/experiment-management/running-experiments).
```

你可能还会注意到 DVC 假设你将参数和指标存储在文件中，而不是通过 API 进行日志记录。这意味着你需要修改代码以从 YAML 文件中读取参数，并将指标写入 JSON 文件。

最后，在初始化时，DVC 自动创建一个基本流程并将其存储在 dvc.yaml 文件中。这样，你的训练代码、流程、参数和指标现在都保存在可以版本控制的文件中。

## 实验即代码方法的好处

## 干净的代码

以这种方式设置后，你的代码不再依赖于实验跟踪 API。你不需要在代码中插入跟踪 API 调用来将实验信息保存到中央数据库，而是将其保存在可读文件中。这些文件始终可以在你的仓库中找到，你的代码保持干净，依赖项减少。即使没有 DVC，你也可以通过 Git 阅读、保存和版本化实验参数和指标，尽管使用纯 Git 不是比较机器学习实验的最方便方式。

```py
$ git diff HEAD~1 -- params.yaml
diff --git a/params.yaml b/params.yaml
index baad571a2..57d098495 100644
--- a/params.yaml
+++ b/params.yaml
@@ -1,5 +1,5 @@
 train:
   epochs: 10
-model:
-  conv_units: 16
+model:
+  conv_units: 128
```

## 可复现性

实验跟踪数据库无法捕捉到你需要复现实验的所有信息。其中一个常常缺失的重要部分是运行实验所需的完整流程。让我们来看看`dvc.yaml`文件，它是生成的流程文件。

```py
$ cat dvc.yaml
stages:
  default:
    cmd: python src/train.py
    deps:
    - data/images
    - src/train.py
    params:
    - model
    - train
    outs:
    - models
    metrics:
    - metrics.json:
        cache: false
    plots:
    - logs.csv:
        cache: false
```

这个流程捕捉了运行实验的命令、参数和其他依赖项、指标、图表以及其他输出。它有一个单一的`default`阶段，但你可以根据需要添加任意多的阶段。当将实验的所有方面都视为代码时，包括流程，任何人都更容易复现实验。

## 减少噪音

在仪表盘中，你可以看到所有的实验，我指的是所有的实验。在某个时刻，你会有很多实验，你需要排序、标记和过滤它们以保持跟进。通过实验版本控制，你在共享和组织事情时有更多的灵活性。

例如，你可以在一个新的 Git 分支中尝试一个实验。如果出现问题或结果不尽如人意，你可以选择不推送该分支。这样你可以减少在实验跟踪仪表盘中可能遇到的一些不必要的杂乱。

同时，如果某个实验看起来有前景，你可以将它和你的代码一起推送到你的仓库，以保持结果与代码和流程的同步。结果与相同的人共享，并且已经按照你现有的分支名称进行了组织。你可以继续在该分支上迭代，如果实验差异过大，可以启动新的分支，或者合并到主分支，使其成为你的主要模型。

## 为什么使用 DVC？

即使没有 DVC，你也可以将代码更改为从文件中读取参数，并将指标写入其他文件，并使用 Git 跟踪更改。然而，DVC 在 Git 之上添加了一些特定于 ML 的功能，可以简化比较和复现实验的过程。

## 大型数据版本控制

大型数据和模型在 Git 中难以跟踪，但使用 DVC，你可以使用自己的存储来跟踪它们，同时它们与 Git 兼容。当初始化 DVC 时，它开始跟踪`models`文件夹，使 Git 忽略它，但仍存储和版本控制它，以便你可以在任何地方备份版本，并与实验代码一起查看它们。

## 单命令可复现性

编码整个实验管道是实现可复现性的良好第一步，但它仍然需要用户执行该管道。使用 DVC，你可以通过单个命令复现整个实验。不仅如此，它还会检查缓存的输入和输出，并跳过重新计算之前生成的数据，这有时可以节省大量时间。

```py
$ dvc exp run
'data/images.dvc' didn't change, skipping
Stage 'default' didn't change, skippingReproduced experiment(s): exp-44136
Experiment results have been applied to your workspace.To promote an experiment to a Git branch run:dvc exp branch <exp> <branch>
```

## 更好的分支组织

虽然 Git 分支是一种灵活的实验组织和管理方式，但通常实验太多而无法适应任何 Git 分支工作流。DVC 跟踪实验，因此你不需要为每个实验创建提交或分支：

```py
$ dvc exp show 

┏━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━┳━━━━━━━━━┳━━━━━━━━┓ 
┃Experiment               ┃ Created      ┃    loss ┃    acc ┃ 
┡━━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━╇━━━━━━━━━╇━━━━━━━━┩ 
│workspace                │ -            │ 0.25183 │ 0.9137 │ 
│mybranch                 │ Oct 23, 2021 │       - │      - │ 
│├──9a4ff1c [exp-333c9]   │ 10:40 AM     │ 0.25183 │ 0.9137 │ 
│├──138e6ea [exp-55e90]   │ 10:28 AM     │ 0.25784 │ 0.9084 │ 
│├──51b0324 [exp-2b728]   │ 10:17 AM     │ 0.25829 │ 0.9058 │ 
└─────────────────────────┴──────────────┴─────────┴────────┘
```

一旦你决定哪些实验值得与团队分享，它们可以转换为 Git 分支：

```py
$ dvc exp branch exp-333c9 conv-units-64
Git branch 'conv-units-64' has been created from experiment 'exp-333c9'.
To switch to the new branch run:git checkout conv-units-64
```

这样，你将避免在你的仓库中创建混乱的分支，并可以专注于仅比较有前途的实验。

## 结论

总结来说，实验版本控制允许你将实验以一种方式编码，使你的实验日志始终与进入实验的确切数据、代码和管道相关联。你可以控制哪些实验最终与同事共享以供比较，这可以防止混乱。

最终，复现一个版本化的实验变得像运行单个命令一样简单，如果某些管道步骤有缓存的输出仍然相关，甚至可能比最初的时间更短。

感谢你坚持看到文章的最后！要了解更多关于使用 DVC 管理实验的信息， [查看文档](https://dvc.org/doc/user-guide/experiment-management)

**简介：[Maria Khalusova](https://www.linkedin.com/in/maria-khalusova-a958aa14/?originalSubdomain=ca)** ([@mariaKhalusova](https://twitter.com/mariaKhalusova))是 Iterative 的高级开发者倡导者。她从事开源 MLOps 工具的工作。

[原文](https://towardsdatascience.com/versioning-machine-learning-experiments-vs-tracking-them-f3096a67faa1)。经许可转载。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 事务

* * *

### 更多相关话题

+   [7 款最佳机器学习实验跟踪工具](https://www.kdnuggets.com/2023/02/7-best-tools-machine-learning-experiment-tracking.html)

+   [为分析跟踪开发开放标准](https://www.kdnuggets.com/2022/07/developing-open-standard-analytics-tracking.html)

+   [用于深度学习实验的 Hydra 配置](https://www.kdnuggets.com/2023/03/hydra-configs-deep-learning-experiments.html)

+   [如何为数据收集设计实验](https://www.kdnuggets.com/2022/04/design-experiments-data-collection.html)

+   [机器学习与大脑的不同第六部分：精确突触权重的重要性…](https://www.kdnuggets.com/2022/08/machine-learning-like-brain-part-6-importance-precise-synapse-weights-ability-set-quickly.html)

+   [数据科学编程语言及其应用时机](https://www.kdnuggets.com/2022/02/data-science-programming-languages.html)
