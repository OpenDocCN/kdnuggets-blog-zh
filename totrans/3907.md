# 优步正在悄悄组建市场上最令人印象深刻的开源深度学习堆栈之一

> 原文：[https://www.kdnuggets.com/2020/01/uber-quietly-assembling-impressive-open-source-deep-learning.html](https://www.kdnuggets.com/2020/01/uber-quietly-assembling-impressive-open-source-deep-learning.html)

[评论](#comments)

![](../Images/9eb8f68d354e9d5615601a86bf9f0026.png)

人工智能（AI）一直是一个非典型的技术趋势。在传统的技术周期中，创新通常从试图颠覆行业现有者的初创公司开始。就AI而言，大部分创新来自于像谷歌、Facebook、优步或微软这样的企业实验室。这些公司不仅在研究方面处于领先地位，而且定期开源新的框架和工具，简化了AI技术的采用。在这种背景下，优步已成为当前生态系统中最活跃的开源AI技术贡献者之一。在短短几年内，优步在AI生命周期的不同领域定期开源项目。今天，我想回顾一些我最喜欢的项目。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

优步是一个近乎完美的AI技术试验场。公司将大型技术公司传统的AI需求与前沿AI交通场景结合起来。因此，优步在从客户分类到自动驾驶车辆等各种多样化场景中建立了机器/深度学习应用程序。优步团队使用的许多技术已经开源，并获得了机器学习社区的好评。让我们看看我最喜欢的一些项目：

*注意：我不会涵盖像Michelangelo或PyML这样的技术，因为它们已经有很好的文档，且已开源。*

### Ludwig：一个无代码机器学习模型的工具箱

![](../Images/e974233d4429a4d56d1c20aeaa705914.png)

[Ludwig](https://uber.github.io/ludwig/)是一个基于TensorFlow的工具箱，允许在不编写代码的情况下训练和测试深度学习模型。概念上，Ludwig是基于五个基本原则创建的：

+   **无需编码**：不需要编码技能即可训练模型并使用它来获得预测。

+   **通用性**：一种基于新数据类型的深度学习模型设计方法，使工具可以用于许多不同的使用案例。

+   **灵活性**：经验丰富的用户可以对模型构建和训练进行广泛的控制，而新手则会发现它易于使用。

+   **可扩展性**：易于添加新的模型架构和新的特征数据类型。

+   **可理解性**：深度学习模型的内部通常被认为是黑箱，但我们提供了标准的可视化工具来理解它们的性能并比较它们的预测。

使用 Ludwig，数据科学家可以通过提供一个包含训练数据的 CSV 文件以及一个包含模型输入和输出的 YAML 文件来训练深度学习模型。利用这两个数据点，Ludwig 执行多任务学习例程以同时预测所有输出并评估结果。在幕后，Ludwig 提供了一系列不断评估的深度学习模型，并可以在最终架构中组合。Uber 工程团队通过以下类比来解释这一过程：*“如果深度学习库提供了建造你建筑物的构件，那么 Ludwig 就提供了建造你城市的建筑物，你可以从可用的建筑物中选择，或者将你自己的建筑物添加到可用的建筑物集合中。”*

### Pyro: 一个本地的概率编程语言

![](../Images/ea6089cd3eb8ca1d2df8db34dd345aa7.png)

[Pyro](http://pyro.ai/)是由 Uber AI Labs 发布的深度概率编程语言（PPL）。Pyro 构建于 PyTorch 之上，基于四个基本原则：

+   **通用性**：Pyro 是一个通用的 PPL——它可以表示任何可计算的概率分布。怎么做的？从一个具有迭代和递归（任意 Python 代码）的通用语言开始，然后添加随机采样、观察和推断。

+   **可扩展**：Pyro 可以处理大数据集，开销仅略高于手写代码。怎么做到的？通过构建现代黑箱优化技术，使用数据的迷你批次来近似推断。

+   **简约性**：Pyro 灵活且易于维护。怎么做的？Pyro 由一个小核心强大、可组合的抽象构建。在可能的情况下，繁重的工作被委托给 PyTorch 和其他库。

+   **灵活**：Pyro 旨在根据需要提供自动化和控制。怎么做的？Pyro 使用高层次的抽象来表达生成和推断模型，同时允许专家轻松自定义推断。

这些原则通常会使 Pyro 的实现朝着相反的方向发展。例如，成为通用的，要求允许在 Pyro 程序中使用任意控制结构，但这种通用性使得扩展变得困难。然而，总的来说，Pyro 在这些能力之间达到了一个精彩的平衡，使其成为实际应用中最优秀的概率编程语言之一。

### Manifold: 一个用于机器学习模型的调试和解释工具集

![](../Images/714ca5378bb4bb9e1a5ce2470983a8e0.png)

[Manifold](https://github.com/uber/manifold) 是 Uber 技术用于大规模调试和解释机器学习模型。使用 Manifold，Uber 工程团队希望实现一些非常具体的目标：

+   调试机器学习模型中的代码错误。

+   理解一个模型在孤立状态下以及与其他模型比较的优缺点。

+   比较和集成不同的模型。

+   将通过检查和性能分析获得的见解纳入模型迭代。

为了实现这些目标，Manifold 将机器学习分析过程分为三个主要阶段：检查、解释和精炼。

+   **检查：** 在分析过程的第一部分，用户设计模型并尝试调查和比较模型结果与其他现有模型的结果。在此阶段，用户比较典型的性能指标，如准确性、精度/召回率和接收者操作特征曲线（ROC），以获得新模型是否优于现有模型的粗略信息。

+   **解释：** 这一分析过程的阶段试图解释前一阶段制定的不同假设。此阶段依赖比较分析来解释一些特定模型的症状。

+   **精炼：** 在这一阶段，用户尝试通过将从解释中提取的知识编码到模型中并测试性能，来验证从前一阶段生成的解释。

### 柏拉图：大规模构建对话代理的框架

![](../Images/b0ddd56bc6cc2bc810712d9c10b90cd9.png)

Uber 构建了 [Plato Research Dialogue System (PRDS)](https://github.com/uber-research/plato-research-dialogue-system) 以应对构建大规模对话应用的挑战。从概念上讲，PRDS 是一个框架，用于在多种环境中创建、训练和评估对话 AI 代理。从功能角度看，PRDS 包括以下构建模块：

+   语音识别（将语音转录为文本）

+   语言理解（从文本中提取意义）

+   状态跟踪（汇总迄今为止所说和所做的内容）

+   API 调用（搜索数据库、查询 API 等）

+   对话策略（生成代理响应的抽象意义）

+   语言生成（将抽象意义转换为文本）

+   语音合成（将文本转换为语音）

PRDS 设计时考虑了模块化，以便纳入最先进的对话系统研究，并持续发展平台的每个组件。在 PRDS 中，每个组件可以在线（通过交互）或离线训练，并纳入核心引擎。从训练角度来看，PRDS 支持与人类和模拟用户的交互。后者在研究场景中常用于启动对话 AI 代理，而前者更能代表实际交互。

### Horovod: 用于大规模训练深度学习的框架

![](../Images/01a5c1c4d4c94262c48bc727ddd14629.png)

[Horovod](https://github.com/uber/horovod) 是 Uber 机器学习堆栈之一，在社区中变得极其受欢迎，并被像 DeepMind 或 OpenAI 这样的 AI 巨头的研究团队采纳。从概念上讲，Horovod 是一个用于大规模运行分布式深度学习训练作业的框架。

Horovod 利用消息传递接口堆栈，如 [OpenMPI](https://www.open-mpi.org/) 使训练作业能够在高度并行和分布式的基础设施上运行，而无需任何修改。在 Horovod 中运行分布式 TensorFlow 训练作业分为四个简单步骤：

1.  **hvd.init()** 初始化 Horovod。

1.  **config.gpu_options.visible_device_list = str(hvd.local_rank())** 将 GPU 分配给每个 TensorFlow 进程。

1.  **opt=hvd.DistributedOptimizer(opt)**将任何常规的 TensorFlow 优化器封装成 Horovod 优化器，后者负责使用环形全减法平均梯度。

1.  **hvd.BroadcastGlobalVariablesHook(0)** 将变量从第一个进程广播到所有其他进程，以确保一致的初始化。

### Uber AI Research: 人工智能研究的常规来源

最后但同样重要的是，我们应提到 Uber 在 AI 研究方面的积极贡献。许多 Uber 的开源发布都受到他们研究工作的启发。[Uber AI Research](https://eng.uber.com/research/?_ga=2.187917355.2063649135.1579178178-1991244199.1527728931) 网站是一个精彩的论文目录，突出显示了 Uber 在 AI 研究中的最新努力。

这些是 Uber 工程团队的一些贡献，这些贡献已被 AI 研究和开发社区广泛采用。随着 Uber 继续大规模实施 AI 解决方案，我们应该会看到新的创新框架，这些框架将简化数据科学家和研究人员对机器学习的采纳。

[原文](https://towardsdatascience.com/uber-has-been-quietly-assembling-one-of-the-most-impressive-open-source-deep-learning-stacks-in-b645656ddddb). 经许可转载。

**相关：**

+   [Uber 创建生成式教学网络以更好地训练深度神经网络](/2020/01/uber-generative-teaching-networks-train-neural-networks.html)

+   [微软推出 Project Petridish 以找到最适合您问题的神经网络](/2020/01/microsoft-introduces-project-petridish-best-neural-network.html)

+   [Facebook 已悄悄开源一些令人惊叹的 PyTorch 深度学习能力](/2019/11/facebook-quietly-open-sourcing-amazing-deep-learning-capabilities-pytorch.html)

### 更多相关话题

+   [具有开发者准备好的软件堆栈的设备端 AI](https://www.kdnuggets.com/2022/03/qualcomm-ondevice-ai-developer-ready-software-stacks.html)

+   [GPT-4 细节已泄露！](https://www.kdnuggets.com/2023/07/gpt4-details-leaked.html)

+   [闭源与开源图像注释](https://www.kdnuggets.com/closed-source-vs-open-source-image-annotation)

+   [LLaMA 3：Meta推出的最强大的开源模型](https://www.kdnuggets.com/llama-3-metas-most-powerful-open-source-model-yet)

+   [HuggingFace推出了一个免费的深度强化学习课程](https://www.kdnuggets.com/2022/05/huggingface-launched-free-deep-reinforcement-learning-course.html)

+   [每位数据科学家至少犯过一次的错误](https://www.kdnuggets.com/2022/09/mistake-every-data-scientist-made-least.html)
