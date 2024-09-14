# 使用 PyTorch 和 Ray 开始分布式机器学习

> 原文：[https://www.kdnuggets.com/2021/03/getting-started-distributed-machine-learning-pytorch-ray.html](https://www.kdnuggets.com/2021/03/getting-started-distributed-machine-learning-pytorch-ray.html)

[评论](#comments)

**由 [Michael Galarnyk](https://twitter.com/GalarnykMichael)、[Richard Liaw](https://twitter.com/richliaw) 和 [Robert Nishihara](https://twitter.com/robertnishihara) 编写**

![文章图片](../Images/2e652136f9d14830daf0b1e7bcade4e5.png)

今天的机器学习 *需要* 分布式计算。无论你是在 [训练网络](https://www.youtube.com/watch?v=rEB3NPUoxMM)、[调整超参数](https://docs.ray.io/en/master/tune/)、[服务模型](https://docs.ray.io/en/master/serve/) 还是 [处理数据](https://medium.com/distributed-computing-with-ray/data-processing-support-in-ray-ae8da34dce7e)，机器学习计算密集型，且在没有集群的情况下可能会变得极其缓慢。 [Ray](https://ray.io/) 是一个流行的分布式 Python 框架，可以与 PyTorch 配合使用，迅速扩展机器学习应用程序。

本文涵盖了 Ray 生态系统的各个方面以及它如何与 PyTorch 一起使用！

### 什么是 Ray

![文章图片](../Images/0c7d3372f00ec70dbc844670bec0f7e5.png)

Ray 是一个开源的并行和分布式 Python 库。上图显示了从高层次来看，Ray 生态系统包括三个部分：核心 Ray 系统、用于机器学习的可扩展库（包括原生和第三方）以及 [在任何集群或云提供商上启动集群的工具](https://medium.com/distributed-computing-with-ray/how-to-scale-python-on-every-major-cloud-provider-12b3bde01208)。

### 核心 Ray 系统

[Ray](https://github.com/ray-project/ray) 可以用来 [扩展 Python 应用程序](https://towardsdatascience.com/modern-parallel-and-distributed-python-a-quick-tutorial-on-ray-99f8d70369b8) 跨多个核心或机器。它具有几个主要优点，包括：

+   简单性：你可以在不重写代码的情况下扩展 Python 应用程序，且相同的代码可以在单台机器或多台机器上运行。

+   鲁棒性：应用程序可以优雅地处理机器故障和抢占。

+   [性能](https://towardsdatascience.com/10x-faster-parallel-python-without-python-multiprocessing-e5017c93cce1)：任务以毫秒级延迟运行，扩展到数万核心，并处理最小序列化开销的数值数据。

### 库生态系统

由于 Ray 是一个通用框架，社区已经在其基础上构建了许多库和框架来完成不同的任务。这些库中的绝大多数支持 PyTorch，只需对代码进行最小的修改，并与其他库无缝集成。以下只是生态系统中一些 [众多库](https://www.anyscale.com/blog/understanding-the-ray-ecosystem-and-community)。

**RaySGD**

![用于帖子图像](../Images/4f506a6863cbb7f15269e461d4ae661a.png)

在p3dn.24xlarge实例上比较PyTorch的DataParallel与Ray（Ray在底层使用PyTorch的Distributed DataParallel）。 [图片来源](https://medium.com/distributed-computing-with-ray/faster-and-cheaper-pytorch-with-raysgd-a5a44d4fd220)。

RaySGD是一个为数据并行训练提供分布式训练包装器的库。例如，[RaySGD TorchTrainer](https://docs.ray.io/en/master/raysgd/raysgd_pytorch.html) 是一个围绕torch.distributed.launch的包装器。它提供了一个Python API，以便轻松地将分布式训练集成到更大的Python应用程序中，而不是需要将训练代码包装在bash脚本中。

该库的一些其他优点包括：

+   易于使用：您可以扩展PyTorch的原生DistributedDataParallel，而无需监控单独的节点。

+   可扩展性：您可以向上或向下扩展。从单个CPU开始。通过更改2行代码，扩展到多节点、多CPU或多GPU集群。

+   加速训练：内置支持NVIDIA Apex的混合精度训练。

+   故障容错：支持在云机器被抢占时自动恢复。

+   兼容性：与其他库如 [Ray Tune](https://docs.ray.io/en/master/tune/index.html) 和 [Ray Serve](https://docs.ray.io/en/master/serve/) 的无缝集成。

您可以通过安装Ray（pip install -U ray torch）并运行下面的代码来开始使用TorchTrainer：

该脚本将下载CIFAR10，并使用ResNet18模型进行图像分类。通过更改一个参数(num_workers=N)，可以利用多个GPU。

如果您想了解更多关于RaySGD以及如何在集群中扩展PyTorch训练的信息，请查看这篇 [博客文章](https://medium.com/distributed-computing-with-ray/faster-and-cheaper-pytorch-with-raysgd-a5a44d4fd220)。

**Ray Tune**

![用于帖子图像](../Images/b430f66bf5d49d073a678c7a63666bb1.png)

Ray Tune对优化算法（如上所示的Population Based Training）的实现 [可以与PyTorch一起使用](https://docs.ray.io/en/master/tune/tutorials/tune-advanced-tutorial.html)，以获得更高性能的模型。图片来自 [Deepmind](https://deepmind.com/blog/article/population-based-training-neural-networks)。

[Ray Tune](https://docs.ray.io/en/master/tune/index.html) 是一个用于实验执行和超参数调优的Python库，适用于任何规模。一些库的优点包括：

+   在不到10行代码的情况下启动多节点 [分布式超参数搜索](https://docs.ray.io/en/master/tune/tutorials/tune-distributed.html#tune-distributed) 的能力。

+   对每个主要机器学习框架的支持 [包括PyTorch](https://pytorch.org/tutorials/beginner/hyperparameter_tuning_tutorial.html)。

+   对GPU的一级支持。

+   自动管理检查点和日志记录到 [TensorBoard](https://docs.ray.io/en/master/tune/user-guide.html#tune-logging)。

+   访问最新的算法，例如 [基于人群的训练（PBT）](https://docs.ray.io/en/master/tune/api_docs/schedulers.html#tune-scheduler-pbt)、[贝叶斯优化搜索（BayesOptSearch）](https://docs.ray.io/en/master/tune/api_docs/suggestion.html#bayesopt)、[HyperBand/ASHA](https://docs.ray.io/en/master/tune/api_docs/schedulers.html#tune-scheduler-hyperband)。

你可以通过安装 Ray（pip install ray torch torchvision）并运行下面的代码来开始使用 Ray Tune。

如果你想了解如何将 Ray Tune 集成到你的 PyTorch 工作流中，应该查看这篇 [博客文章](https://towardsdatascience.com/fast-hyperparameter-tuning-at-scale-d428223b081c)。

**Ray Serve**

![Image for post](../Images/0b9b606dabee68b96e923ccb4b9d7d5a.png)

Ray Serve 不仅可以单独用于模型服务，还可以用于[ 扩展其他服务工具，如 FastAPI](https://medium.com/distributed-computing-with-ray/how-to-scale-up-your-fastapi-application-using-ray-serve-c9a7b69e786)。

[Ray Serve](https://docs.ray.io/en/master/serve/) 是一个易于使用的可扩展模型服务库。该库的一些优势包括：

+   使用单一工具包服务于从深度学习模型（PyTorch、TensorFlow 等）到 scikit-learn 模型，再到任意 Python 业务逻辑的所有内容。

+   扩展到多个机器，无论是在你的数据中心还是在云端。

+   与许多其他库兼容，如 [Ray Tune](https://medium.com/distributed-computing-with-ray/simple-end-to-end-ml-from-selection-to-serving-with-ray-tune-and-ray-serve-10f5564d33ba) 和 [FastAPI](https://medium.com/distributed-computing-with-ray/how-to-scale-up-your-fastapi-application-using-ray-serve-c9a7b69e786)。

如果你想学习如何将 Ray Serve 和 Ray Tune 集成到你的 PyTorch 工作流中，应该查看 [文档](https://docs.ray.io/en/master/tune/tutorials/tune-serve-integration-mnist.html#tune-trainable-for-model-selection) 和 [完整代码示例](https://docs.ray.io/en/master/tune/tutorials/tune-serve-integration-mnist.html#sphx-glr-download-tune-tutorials-tune-serve-integration-mnist-py)。

**RLlib**

![Image for post](../Images/edfac255c7cffdb09e6501e2860a316c.png)

RLlib 提供了自定义训练几乎所有方面的方法，包括神经网络模型、动作分布、策略定义、环境以及样本收集过程。

[RLlib](https://docs.ray.io/en/master/rllib.html) 是一个强化学习库，提供高扩展性和统一的 API，适用于各种应用。一些优势包括：

+   原生支持 PyTorch、TensorFlow Eager 和 TensorFlow（1.x 和 2.x）。

+   支持无模型、有模型、进化、规划和多智能体算法

+   通过简单的配置标志和自动包装器支持复杂模型类型，如注意力网络和 LSTM 堆栈

+   与其他库兼容，如 [Ray Tune](https://docs.ray.io/en/master/rllib-training.html)。

**Cluster Launcher**

![Image for post](../Images/0bc773ad98d89810da54245138a3805e.png)

Ray 集群启动器简化了在任何集群或云提供商上启动和扩展的过程。

一旦你在笔记本电脑上开发了一个应用程序并想要将其扩展到云端（可能是更多的数据或更多的 GPU），接下来的步骤并不总是很清晰。这个过程要么是让基础设施团队为你设置，要么是按照以下步骤进行。

1\. 选择一个云提供商（AWS、GCP 或 Azure）。

2\. 导航管理控制台以设置实例类型、安全组、竞价价格、实例限制等。

3\. 确定如何将你的 Python 脚本分发到集群中。

更简单的方法是使用 [Ray 集群启动器来启动和扩展机器跨任何集群或云提供商](https://medium.com/distributed-computing-with-ray/how-to-scale-python-on-every-major-cloud-provider-12b3bde01208)。集群启动器允许你自动扩展、同步文件、提交脚本、端口转发等。这意味着你可以在 Kubernetes、AWS、GCP、Azure 或私有集群上运行 Ray 集群，而无需了解集群管理的底层细节。

### 结论

![Image for post](../Images/b84fa86cf0d0c2f8285a4409d1178478.png)

Ray 为 [Ant Group 的 Fusion Engine](https://youtu.be/Wwv9YNlXx0Q) 提供了分布式计算基础。

本文介绍了 Ray 在 PyTorch 生态系统中的一些好处。[Ray](https://github.com/ray-project/ray) 被广泛应用于 [Ant Group 使用 Ray 支持其金融业务](https://youtu.be/Wwv9YNlXx0Q)、[LinkedIn 在 Yarn 上运行 Ray](https://youtu.be/0Z0Th9ySIfs)、[Pathmind 使用 Ray 将强化学习与模拟软件连接](https://youtu.be/U9deXfKYDSs) 等。如果你对 Ray 有任何问题或想了解更多关于 [并行和分布式 Python](https://docs.ray.io/en/master/) 的信息，请通过 [Discourse](https://discuss.ray.io/) 或 [Slack](https://docs.google.com/forms/d/e/1FAIpQLSfAcoiLCHOguOm8e7Jnn-JJdZaCxPGjgVCvFijHB5PLaQLeig/viewform) 加入我们的社区。

[原文](https://medium.com/pytorch/getting-started-with-distributed-machine-learning-with-pytorch-and-ray-fd83c98fdead)。经授权转载。

**相关：**

+   [如何加速 Scikit-Learn 模型训练](/2021/02/speed-up-scikit-learn-model-training.html)

+   [训练 sklearn 快速 100 倍](/2019/09/train-sklearn-100x-faster.html)

+   [使用 Dask 和 PyTorch 的计算机视觉](https://medium.com/distributed-computing-with-ray/how-to-scale-python-on-every-major-cloud-provider-12b3bde01208)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织进行 IT

* * *

### 更多相关主题

+   [入门 PyTorch Lightning](https://www.kdnuggets.com/2022/12/getting-started-pytorch-lightning.html)

+   [5 步骤入门 PyTorch](https://www.kdnuggets.com/5-steps-getting-started-pytorch)

+   [KDnuggets™ 新闻 22:n07, 2 月 16 日：如何学习机器学习中的数学…](https://www.kdnuggets.com/2022/n07.html)

+   [数据网格及其分布式数据架构](https://www.kdnuggets.com/2022/02/data-mesh-distributed-data-architecture.html)

+   [入门 Scikit-learn 进行机器学习中的分类](https://www.kdnuggets.com/getting-started-with-scikit-learn-for-classification-in-machine-learning)

+   [入门 Claude 3 Opus，这款刚刚击败 GPT-4 和 Gemini 的模型](https://www.kdnuggets.com/getting-started-with-claude-3-opus-that-just-destroyed-gpt-4-and-gemini)
