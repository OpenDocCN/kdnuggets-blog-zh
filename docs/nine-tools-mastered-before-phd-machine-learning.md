# 我希望在机器学习博士学位之前掌握的九种工具

> 原文：[https://www.kdnuggets.com/2021/09/nine-tools-mastered-before-phd-machine-learning.html](https://www.kdnuggets.com/2021/09/nine-tools-mastered-before-phd-machine-learning.html)

[comments](#comments)

**由 [Aliaksei Mikhailiuk](https://www.linkedin.com/in/aliakseimikhailiuk/) 提供，人工智能科学家**

![](../Images/f2afdbc6c250a36342be9c182412c909.png)

图片由作者提供。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你在IT领域的组织

* * *

尽管在推动技术进步中扮演了重要角色，学术界往往忽视工业成就。到我博士学位结束时，我意识到有许多伟大的辅助工具在学术界被忽视，但在工业界被广泛采用。

从个人经验来看，我知道学习和整合新工具可能会很无聊、令人害怕，可能会拖慢进度并使人气馁，特别是当当前的设置如此熟悉且有效时。

摆脱坏习惯可能很困难。对下面列出的每个工具，我都不得不接受我做事的方式是不够理想的。然而，在这个过程中，我也学到了有时候当下看不到的结果在后期会带来十倍的回报。

在下面，我将讨论一些我在研究和构建机器学习应用时发现非常有用的工具，这些工具既适用于学术研究，也适用于人工智能工程师。我将这些工具按其目的分为四个部分：环境隔离、实验跟踪、协作和可视化。

## 隔离环境

机器学习是一个极其快速发展的领域，因此常用的软件包经常更新。尽管开发者们努力工作，但新版本往往与其前身不兼容。这确实会带来很多麻烦！

幸运的是，有工具可以解决这个问题！

## [Docker](https://www.docker.com/)

![](../Images/eebbdcc0b2ecc174b62a99489a772e57.png)

图片由作者提供。

这些 NVIDIA 驱动程序给你带来了多少麻烦？在我的博士学位期间，我有一台由大学管理的机器，定期更新。更新在晚上进行且没有任何通知。想象一下，当我发现第二天早上大部分工作与最新驱动程序不兼容时的惊讶。

尽管这些课程并非专门针对这一点，但 docker 可以帮助你避免这些特别在截止日期前的压力情况。

Docker 允许将软件封装在称为容器的包中。容器是独立的单元，具有自己的软件、库和配置文件。从简化的角度看，容器是一个独立的虚拟操作系统，具有与外界沟通的手段。

Docker 提供了大量现成的容器供你使用，无需对如何自己配置所有内容有广泛的了解，开始使用基础知识非常简单。

对于那些想要快速入门的人，可以查看这个 [教程](https://docs.docker.com/get-started/)。此外，Amazon AWS 也很好地解释了为什么以及如何在机器学习中使用 Docker，可以在 [这里](https://aws.amazon.com/blogs/opensource/why-use-docker-containers-for-machine-learning-development/) 查看。

## [Conda](https://docs.conda.io/en/latest/)

现在重复使用别人的代码已经成为一种新常态。有人在 GitHub 上创建了一个有用的代码库，你可以克隆代码，安装并得到你的解决方案，而无需自己编写任何东西。

不过有一个小不便。当多个项目一起使用时，你会遇到包管理的问题，因为不同的项目需要不同版本的包。

我很高兴在我的博士研究中发现了 Conda。Conda 是一个包和环境管理系统。它允许创建多个环境，并快速安装、运行和更新包及其依赖项。你可以快速切换隔离的环境，并始终确保你的项目仅与预期的包进行交互。

Conda 提供了自己的 [教程](https://conda.io/projects/conda/en/latest/user-guide/getting-started.html#starting-conda) ，介绍如何创建你的第一个环境。

## 运行、跟踪和记录实验

获取博士学位所需的两个关键支柱是严谨性和一致性，没有这两个条件，在应用领域获得博士学位几乎是不可能的。如果你曾经尝试过使用机器学习模型，你可能知道跟踪测试参数是多么容易丢失。以前，参数跟踪是在实验室笔记本中进行的，我相信这些在其他领域仍然非常有用，但在计算机科学领域，我们现在拥有比这更强大的工具。

## [Weights and biases](https://wandb.ai/site)

![](../Images/6831c60dea2746d029ded5c99b361116.png)

wandb 面板的一组简单指标的快照——训练损失、学习率和平均验证损失。注意你还可以跟踪系统参数！图片由作者提供。

```py
experiment_res_1.csv
experiment_res_1_v2.csv
experiment_res_learning_rate_pt_5_v1.csv
...
```

这些名字看起来熟悉吗？如果是，那么你的模型跟踪技能应该提高了。这就是我在博士第一年的情况。作为借口，我应该说我有一个电子表格，在那里我记录每个实验的详细信息和所有相关文件。然而，这仍然非常复杂，每次参数变更的记录不可避免地会影响后处理脚本。

Weights and Biases (W&B/wandb) 是我发现得比较晚的一个宝藏工具，但现在我在每个项目中都使用它。它让你能够用几行代码跟踪、比较、可视化和优化机器学习实验。它还允许你跟踪你的数据集。尽管选项众多，我发现 W&B 易于设置，并且具有非常友好的网页界面。

对有兴趣的人来说，可以查看他们的快速设置教程 [这里](https://docs.wandb.ai/quickstart)。

## [MLflow](https://mlflow.org/)

![](../Images/3d43457ac771cfdf1d0ccede4941e326.png)

作者提供的图片。

类似于 W&B，MLFlow 提供了记录代码、模型和数据集的功能，你的模型在这些数据集上进行训练。虽然我主要用它来记录数据、模型和代码，但它提供的功能远不止这些。它允许管理整个 ML 生命周期，包括实验、可重复性和部署。

如果你想将它快速集成到你的模型中，请查看这个 [教程](https://www.mlflow.org/docs/latest/tutorials-and-examples/tutorial.html)。Databricks 还分享了关于 MLflow 的一个非常好的 [解释](https://databricks.com/blog/2018/06/05/introducing-mlflow-an-open-source-machine-learning-platform.html)。

## [Screen](https://linuxize.com/post/how-to-use-linux-screen/)

在博士生涯的前半年，我常常选择将实验留到过夜，希望机器不会进入睡眠状态。当工作转到远程时，我会担心 ssh 会话中断——代码已经运行了好几个小时，几乎收敛了。

我对 *screen* 功能的了解较晚，因此在早晨总是不能避免半成品结果。但在这种情况下，确实是晚做总比不做好。

*Screen* 让你从单一的 ssh 会话中启动和使用多个 shell 会话。用 *screen* 启动的进程可以从会话中分离，然后在稍后的时间重新附加。因此，你的实验可以在后台运行，无需担心会话关闭或终端崩溃。

功能总结 [这里](https://www.geeksforgeeks.org/screen-command-in-linux-with-examples/)。

## 协作

学术界以缺乏有效团队管理机制而臭名昭著。这在一定程度上是由于对个人贡献的严格要求。然而，机器学习的发展速度需要共同努力。以下是两个相当基础的工具，对于有效沟通特别是在远程工作的新领域中会非常有用。

## [GitHub](https://github.com/)

相当基础，对吧？在看到学术界人们如何跟踪他们的代码的种种恐怖之后，我不能更强调掌握版本控制的重要性。不再有名为 code_v1、code_v2 的文件夹。

Github 提供了一个非常有用的代码跟踪、合并和审查框架。每当一个团队构建一个[深度图像质量指标](https://towardsdatascience.com/deep-image-quality-assessment-30ad71641fac)时，每个成员可以有自己的代码分支并行工作。解决方案的不同部分可以合并在一起。每当有人引入一个bug时，可以很容易地恢复到工作版本。总体而言，我认为git是我在这篇文章中提到的所有工具中最重要的。

查看这个逐步[指南](https://guides.github.com/activities/hello-world/)了解如何快速启动。

## [Lucidchart](https://www.lucidchart.com/pages/)

最近我接触到了Lucidchart，此前我一直在使用[draw.io](https://app.diagrams.net/)— 一个非常简单的图表创建界面。Lucidchart强大了千倍，功能也更为多样。它的主要优势在于协作共享空间和在图表旁边做笔记的能力。想象一下一个巨大的在线白板，配有一整套模板。

要快速入门，请查看Lucidchart的这个[tutorial](https://www.lucidchart.com/pages/tour)页面。

## 可视化

许多论文提交，特别是那些不成功的案例，教会了我演示往往和结果一样重要。如果审稿人通常没有太多时间，不理解文本，工作就会直接被拒绝。匆忙制作的图像会留下不好的印象。有人曾经告诉我：“如果你无法制作图表，我怎么能相信你的结果？”我不同意这个说法，但我确实同意印象很重要。

## [Inkscape](https://inkscape.org/)

一图胜千言（实际上，[纠正 84.1](https://www.cl.cam.ac.uk/~afb21/publications/Student-ESP.html)字）。

Inkscape 是一款免费的矢量图形软件。事实上，我在本科时的网页开发课程中学会了使用它。然而，我在攻读博士学位期间才真正学会了充分利用它——为论文制作那些漂亮的图像。

在Inkscape提供的所有功能中，特别有价值的是[TexText](https://inkscape.org/~jcwinkler/%E2%98%85textext)扩展。使用这个软件包，你可以将你的[latex](https://www.latex-project.org/)公式无缝集成到图像中。

有大量的教程，然而对于基本功能，我推荐Inkscape团队提供的教程[这里](https://inkscape.org/learn/tutorials/)。

## [Streamlit](https://streamlit.io/)

你是否曾经需要创建一个简单的网站来展示你的结果或一个简单的机器学习应用？只需几行Python代码，使用Streamlit就可以实现。

我发现它特别适用于论文补充材料，但它甚至可以在项目展示和客户演示中更加有用。

要快速入门，请查看这个[教程](https://builtin.com/machine-learning/streamlit-tutorial)。

## 总结及拓展

在攻读博士学位的同时，顺利进入行业并不容易。但这让我学到了几条重要的教训，希望我在博士学习的早期就能了解到这些。

最重要的一课是，**好奇心**和**愿意学习和改变**可以极大地影响你工作的质量。

以下是我在每个部分提到的教程的总结：

[Docker](https://www.docker.com/): [教程](https://aws.amazon.com/blogs/opensource/why-use-docker-containers-for-machine-learning-development/)

[Conda](https://docs.conda.io/en/latest/): [教程](https://conda.io/projects/conda/en/latest/user-guide/getting-started.html#starting-conda)

[Weights and biases](https://wandb.ai/site): [教程](https://docs.wandb.ai/quickstart)

[MLflow](https://mlflow.org/): [教程](https://www.mlflow.org/docs/latest/tutorials-and-examples/tutorial.html)

[GitHub](https://github.com/): [教程](https://guides.github.com/activities/hello-world/)

[Screen](https://linuxize.com/post/how-to-use-linux-screen/): [教程](https://www.geeksforgeeks.org/screen-command-in-linux-with-examples/)

[Inkscape](https://inkscape.org/): [教程](https://inkscape.org/learn/tutorials/)

[Streamlit](https://streamlit.io/): [教程](https://builtin.com/machine-learning/streamlit-tutorial)

[Lucidchart](https://www.lucidchart.com/pages/): [教程](https://www.lucidchart.com/pages/tour)

如果你喜欢这篇文章，请与朋友分享！要阅读更多机器学习和图像处理的话题，请按订阅！

我有没有遗漏什么？请随时留下注释、评论或直接给我发消息！

**简介： [Aliaksei Mikhailiuk](https://www.linkedin.com/in/aliakseimikhailiuk/)** 在计算机视觉、偏好聚合和自然语言处理领域拥有丰富的研究、开发、部署和维护机器学习算法的经验。

[原文](https://towardsdatascience.com/nine-tools-i-wish-i-mastered-before-my-phd-in-machine-learning-708c6dcb2fb0)。经许可转载。

**相关：**

+   [我在数据科学职业生涯三年中学到的三大重要课程](/2021/09/3-important-lessons-data-science-career.html)

+   [数据科学、数据可视化和机器学习的38个顶级Python库](/2020/11/top-python-libraries-data-science-data-visualization-machine-learning.html)

+   [数据科学工具的受欢迎程度，动画版](/2020/06/data-science-tools-popularity-animated.html)

### 更多相关话题

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [我希望在开始数据科学时知道的三件事](https://www.kdnuggets.com/2023/01/3-things-wish-knew-started-data-science.html)

+   [成为优秀数据科学家所需的五项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应该掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [建立一个强大的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)

+   [使用管道编写干净的 Python 代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)
