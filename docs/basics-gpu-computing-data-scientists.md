# 数据科学家的 GPU 计算基础

> 原文：[https://www.kdnuggets.com/2016/04/basics-gpu-computing-data-scientists.html](https://www.kdnuggets.com/2016/04/basics-gpu-computing-data-scientists.html)

**作者：[Taposh Dutta-Roy](https://medium.com/@taposhdr)**。

![](../Images/50250a05d64b60f2c505f24cf7712949.png)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 部门

* * *

### 数据科学家的 GPU 计算基础

GPU 已成为图像分析的新核心。越来越多的数据科学家开始考虑使用 GPU 进行图像处理。在这篇文章中，我回顾了数据科学家需要的 GPU 基础知识，并列出了 [文献](http://www.sciencedirect.com/science/article/pii/S1361841514001819) 中讨论的 GPU 在算法中的适用性框架。

### 我们先来了解什么是 GPU？

图形处理单元（GPU）最初是为了渲染图像而创建的。然而，由于其高性能和低成本，它们已成为图像处理的新标准。它们的应用领域包括图像修复、分割（标记）、去噪、过滤、插值和重建。对 GPU 的网络搜索结果是：“**图形处理单元**（**GPU**）是一个计算机芯片，执行快速的数学计算，主要用于渲染图像。”

### 什么是 GPU 计算？

[Nvidia 的博客](http://www.nvidia.com/object/what-is-gpu-computing.html) 定义了 GPU 计算是指使用图形处理单元（GPU）与 CPU 一起加速科学、分析、工程、消费者和企业应用程序。[他们还说](https://blogs.nvidia.com/blog/2009/12/16/whats-the-difference-between-a-cpu-and-a-gpu/)如果 CPU 是大脑，那么 GPU 是计算机的灵魂。

用于通用计算的 GPU 具有高度的数据并行架构。它们由多个核心组成。每个核心都有多个功能单元，例如算术和逻辑单元（ALUs）等。这些功能单元中的一个或多个用于处理每个执行线程。帮助线程的这些功能单元组称为**“线程处理器”**。*GPU 中的所有线程处理器都执行相同的指令，因为它们共享相同的控制单元*。这意味着 GPU 可以对图像的每个像素并行执行相同的指令。GPU 架构复杂且因制造商而异。GPU 市场的两个主要玩家是 Nvidia 和 AMD。Nvidia 将线程处理器称为 CUDA（计算统一设备架构）核心，AMD 称之为流处理器（SP）。

线程处理器与 CPU 核心之间的主要区别在于每个 CPU 核心可以在并行的情况下对不同的数据执行不同的指令，因为每个 CPU 核心有独立的控制单元。[研究人员](http://ieeexplore.ieee.org/xpl/articleDetails.jsp?arnumber=4490125) 将核心定义为具有独立控制流的处理单元。根据这个定义，[科学家们](http://www.sciencedirect.com/science/article/pii/S1361841514001819) 将共享相同控制单元的线程处理器组称为核心。GPU 设计为在一个芯片上适配多个线程处理器，而 CPU 则专注于高级控制单元和大缓存。

GPU 编程有两个主要框架——[OpenCL](https://www.khronos.org/opencl/) 和 CUDA。CUDA 编程语言只能用于 Nvidia 显卡，而 OpenCL 是一个用于不同设备（包括 GPU、CPU 和 FPGA）的开放标准。几种图像处理库提供了 GPU 实现——OpenCV、ArrayFire、Nvidia Performance Primitives (NPP)、CUVLIB、Intel Integrated Performance Primitives、OpenCL Integrated Performance Primitives 和 Insight Took Kit (ITK)。其中较大的库有 OpenCV（支持 CUDA 和 OpenCL）和 ITK（仅支持 OpenCL）。

### 算法适用于 GPU 使用的标准

[Erik Smistad 等](http://www.sciencedirect.com/science/article/pii/S1361841514001819) 讨论了定义算法适用于 GPU 实现的 5 个因素。这些因素包括——数据并行性、线程数量、分支分歧、内存使用和同步。我创建了这个映射表以便于参考。

![](../Images/59f662fb10c5857aa5d74465ae745a88.png)

<canvas class="progressiveMedia-canvas js-progressiveMedia-canvas" width="75" height="22">![](../Images/ca1b3c0ea2d5ceb10e1cdda3e84c6cc1.png)

GPU 适用性决定您的算法

</canvas>

作为数据科学家，了解这些信息将帮助你选择适合你算法的 GPU。Nvidia CUDA 更为流行，因为大多数苹果笔记本电脑都配备了它，但其他品牌的 GPU 也在逐渐获得关注。如果你有任何问题或评论，请告诉我。

[原文](https://medium.com/@taposhdr/gpu-s-have-become-the-new-core-for-image-analytics-b8ba8bd8d8f3#.pjywke4uy)。

**相关内容：**

+   [流行的深度学习工具 – 评测](/2015/06/popular-deep-learning-tools.html)

+   [在哪里学习深度学习 – 课程、教程、软件](/2014/05/learn-deep-learning-courses-tutorials-overviews.html)

+   [CuDNN – 一个新的深度学习库](/2014/09/cudnn-new-library-deep-learning.html)

### 更多相关话题

+   [构建 GPU 机器与使用 GPU 云服务](https://www.kdnuggets.com/building-a-gpu-machine-vs-using-the-gpu-cloud)

+   [宣布博客写作比赛，获胜者将获得 NVIDIA GPU！](https://www.kdnuggets.com/2022/11/blog-writing-contest-nvidia-gpu.html)

+   [使用 RAPIDS cuDF 充分利用 GPU 进行特征工程](https://www.kdnuggets.com/2023/06/rapids-cudf-leverage-gpu-feature-engineering.html)

+   [掌握 GPU：Python 中 GPU 加速 DataFrames 的初学者指南](https://www.kdnuggets.com/2023/07/mastering-gpus-beginners-guide-gpu-accelerated-dataframes-python.html)

+   [生成式 AI 游乐场：Camel-5b 和 Open LLaMA 3B 的 LLMs]（https://www.kdnuggets.com/2024/02/intel-generative-ai-playground-llms-with-camel-5b-and-open-llama-3b）

+   [生成式 AI 游乐场：Text-to-Image 稳定扩散]（https://www.kdnuggets.com/2024/02/intel-generative-ai-playground-text-to-image-stable-diffusion）
