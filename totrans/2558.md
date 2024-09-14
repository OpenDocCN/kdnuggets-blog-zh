# 高性能深度学习：如何训练更小、更快、更好的模型——第 5 部分

> 原文：[https://www.kdnuggets.com/2021/07/high-performance-deep-learning-part5.html](https://www.kdnuggets.com/2021/07/high-performance-deep-learning-part5.html)

[评论](#comments)

![](../Images/9970edb2850289adc136ca465d25bc5e.png)

在之前的部分中（[第 1 部分](https://www.kdnuggets.com/2021/06/efficiency-deep-learning-part1.html)，[第 2 部分](https://www.kdnuggets.com/2021/06/high-performance-deep-learning-part2.html)，[第 3 部分](https://www.kdnuggets.com/2021/07/high-performance-deep-learning-part3.html)，[第 4 部分](https://www.kdnuggets.com/2021/07/high-performance-deep-learning-part4.html)），我们讨论了效率为何对深度学习模型的重要性，以实现高性能的帕累托最优模型，以及深度学习中效率的关注领域。我们还涵盖了四个关注领域（压缩技术、学习技术、自动化和高效架构）。让我们以关于基础设施的最终部分结束这一系列，这对训练和部署高性能模型至关重要。

顺便提一下，欢迎您阅读我们关于深度学习效率的[调查论文](https://arxiv.org/abs/2106.08962)，该论文详细涵盖了这个主题。

### 基础设施

为了高效地训练和运行推断，必须有一个稳健的软件和硬件基础设施。在这一部分，我们将探讨这两个方面。

![](../Images/731ef6c8b32607021623999d6c9877f7.png)

*软件和硬件基础设施的心理模型，以及它们如何相互作用。*

**Tensorflow 生态系统：**

Tensorflow (TF) [1] 是一个流行的机器学习框架，已被许多大型企业投入生产。它拥有一些最广泛的软件支持，以提高模型效率。

Tensorflow Lite 适用于设备端用例：Tensorflow Lite (TFLite) [2] 是一组工具和库，旨在低资源环境（如边缘设备）中进行推断。从高层次来看，我们可以将 TFLite 分为两个核心部分：

+   解释器和操作内核：TFLite 提供了一个解释器用于运行专门的 TFLite 模型，并实现了常见的神经网络操作（全连接、卷积、最大池化、ReLu、Softmax 等，每个操作都有一个 Op）。截至本文撰写时，这些解释器和操作主要针对 ARM 处理器进行了优化。它们还可以利用智能手机 DSP（如高通的 Hexagon [3]）来加速执行。

+   转换器：这是一个将给定的 TF 模型转换为解释器进行推断的单个文件的工具。除了转换本身，它还处理许多内部细节，例如为量化推断准备图（如前面帖子中提到的），融合操作，向模型添加其他元数据等。

设备端推理的其他工具：类似地，其他平台也有推理工具。例如，TF Micro [4] 具有精简的解释器和一组较小的操作，用于在非常低资源的微控制器如 DSP 上进行推理。TensorflowJS (TF.JS) [5] 是 TF 生态系统中的一个库，可用于在浏览器中或使用 Node.js 训练和运行神经网络。这些模型也可以通过 WebGL 接口 [6] 通过 GPU 加速。它支持导入在 TF 中训练的模型以及在 TF.JS 中从头开始创建新模型。还有 TF 模型优化工具包 [7]，提供了在模型图中添加量化、稀疏性、权重聚类等功能。

服务器端加速的 XLA：XLA (Accelerated Linear Algebra) [8] 是一个图编译器，通过生成为图定制的操作（内核）的新实现来优化模型中的线性代数计算。例如，可以将多个可以融合在一起的操作合并为一个复合操作。这避免了在操作数仍在缓存中时进行多次昂贵的 RAM 写入。Kanwar 等人 [9] 报告称，使用 XLA 训练 BERT 的吞吐量增加了 7 倍，最大批量大小增加了 5 倍。这使得在 Google Cloud 上用 $32 训练 BERT 模型成为可能。

**PyTorch 生态系统：**

PyTorch [10] 是另一个流行的机器学习平台，广泛用于学术界和工业界，其在可用性和功能上可以与 Tensorflow 相比。

设备端用例：PyTorch 还具有一个轻量级解释器，使得可以在移动设备上运行 PyTorch 模型 [11]，为 Android 和 iOS 提供了原生运行时（类似于 TFLite 解释器）。PyTorch 还提供了训练后量化 [12] 和其他图优化步骤，如常量折叠、融合某些操作、将通道置于最后（NHWC）格式以优化卷积层。

通用模型优化：PyTorch 还提供了 Just-in-Time (JIT) 编译功能 [13]，用于从 TorchScript [14] 代码生成可序列化的中间表示形式，TorchScript 是 Python 的一个子集，添加了类型检查等功能。它允许在灵活的 PyTorch 代码用于研究和开发与可部署于生产环境进行推理的表示形式之间创建桥梁。这是 PyTorch 模型在移动设备上执行的主要方式。

PyTorch 世界中 XLA 的替代方案似乎是 Glow [15] 和 TensorComprehension [16] 编译器。它们有助于生成从高层次 IR（如 TorchScript）派生的低层次中间表示 (IR)。

PyTorch 还提供了一个模型调优指南 [17]，详细说明了 ML 从业者可用的各种选项。其中的一些核心思想包括：

+   使用 PyTorch JIT 进行点对点操作（加法、减法、乘法、除法等）的融合。

+   启用缓冲区检查点功能可以只将某些层的输出保存在内存中，并在反向传播时计算其余部分。这特别有助于处理计算便宜但输出较大的层，例如激活层。

+   启用设备特定优化，如 cuDNN 库和 NVIDIA GPU 的混合精度训练（在 GPU 子节中解释）。

+   使用分布式数据并行训练，当数据量大且有多个 GPU 可用时，这种方法适用。

**硬件优化库：**

我们还可以通过针对神经网络运行的硬件优化软件堆栈进一步提升效率。例如，ARM 的 Cortex 系列处理器支持 SIMD（单指令多数据）指令，允许使用 Neon 指令集对操作进行向量化（处理数据批次）。QNNPACK [18] 和 XNNPACK [19] 库针对移动和嵌入式设备的 ARM Neon 进行优化，对于 x86 SSE2、AVX 架构等也进行了优化。前者用于 PyTorch 的量化推理模式，后者支持 TFLite 的 32 位浮点模型和 16 位浮点模型。类似地，还有其他低级库，如 iOS 的 Accelerate [20] 和 Android 的 NNAPI [21]，它们尝试将硬件级加速决策从更高级的 ML 框架中抽象出来。

**硬件**

GPU：图形处理单元（GPUs），最初设计用于加速计算机图形，但自 2007 年 CUDA 库 [22] 发布后，开始用于通用用途。AlexNet [23] 在 2012 年赢得 ImageNet 比赛，标准化了 GPU 在深度学习模型中的使用。自那时起，Nvidia 发布了几代 GPU 微架构，越来越注重深度学习性能。它还引入了 Tensor Cores [24]，这些是专用执行单元，支持多种精度（fp32、TensorFloat32、fp16、bfloat16、int8、int4）的训练和推理。

![](../Images/565b5a29e37b45b895dd6b165cd28f3c.png)

*降低精度的乘加（MAC）操作：B x C 是一个昂贵的操作，因此使用降低精度来完成。*

张量核心优化了标准的乘法和累加（MAC）操作，A = (B × C) + D。其中，B和C采用减少精度（fp16、bfloat16、TensorFloat32），而A和D采用fp32。NVIDIA报告说，这种减少精度的MAC操作在训练速度上可以提高1×到15×，具体取决于模型架构和选择的GPU [25]。NVidia最新的Ampere架构GPU中的张量核心也支持稀疏性下的更快推理（特别是2:4的结构稀疏性，即在一个4元素块中有2个元素是稀疏的）。它们在推理时间上能提高多达1.5×的速度，在单层上能提高多达1.8×的速度。除此之外，NVidia还提供了cuDNN库 [29]，其中包含了标准神经网络操作的优化版本，如全连接、卷积、批量归一化、激活等。

![](../Images/e9cc7b74251b383ecca7ae12475ef576.png)

*用于GPU和TPU训练和推理的数据类型。bfloat16起源于TPU，而Tensor Float 32仅在NVidia GPU上支持。*

TPU: TPU是谷歌设计的专有应用特定集成电路（ASIC），用于加速深度学习应用，并且针对并行化和加速线性代数运算进行了精细调优。TPU架构支持使用TPU进行训练和推理，并通过谷歌云服务向公众提供。TPU芯片的核心架构利用了脉动阵列设计 [26]，在这种设计中，大规模计算被分割到类似网格的拓扑中，每个单元计算一个部分结果，并在每个时钟周期（以类似于脉动心跳的节奏）将结果传递给下一个单元，而无需将中间结果存储在内存中。

每个TPU芯片有两个张量核心（不要与NVidia的张量核心混淆），每个核心都有一个脉动阵列网格。一个TPU板上有4个互联的TPU芯片。为了进一步扩展训练和推理，可以将更多的TPU板以网状拓扑结构连接起来，形成一个“pod”。根据公开发布的数据，每个TPU芯片（v3）可以达到420 teraflops，而一个TPU pod可以达到100+ petaflops [27]。TPU在谷歌内部用于训练谷歌搜索模型、通用的BERT模型、如DeepMind的世界领先AlphaGo和AlphaZero模型等应用，以及许多其他研究应用 [28]。与GPU类似，TPU支持bfloat16数据类型，这是一种减少精度的替代方案，用于全浮点32位精度训练。XLA支持允许在不改变模型的情况下透明地切换到bfloat16。

![](../Images/dd046fd4d4c0c60ff9e84a57d199abf3.png)

*基本脉动阵列单元的示意图，以及使用脉动阵列单元网格进行4x4矩阵乘法的演示。*

![](../Images/e07609aef4d23226d614e16bb983f126.png)

*EdgeTPU、Coral 和开发板的近似大小。**（提供者：Bhuwan Chopra）*

EdgeTPU: EdgeTPU 也是 Google 设计的定制 ASIC 芯片。类似于 TPU，它专门用于加速线性代数操作，但仅限于推理，并且在边缘设备上具有更低的计算预算。它还仅限于部分操作，并且仅与 int8 量化的 TensorFlow Lite 模型兼容。EdgeTPU 芯片本身比美国便士还小，使其适合部署在多种 IoT 设备中。它已被部署在类似 Raspberry-Pi 的开发板上，在 Pixel 4 智能手机中作为 Pixel Neural Core，也可以独立焊接到 PCB 上。

![](../Images/9a43025da1ed0691405d0901df757dec.png)

*Jetson Nano 模块。([来源](https://www.nvidia.com/en-us/autonomous-machines/jetson-store/))*

Jetson: Jetson 是 Nvidia 提供的一系列加速器，用于支持嵌入式和物联网设备中的深度学习应用。它包括 Nano，这是一个低功耗的“系统模块”（SoM），适用于轻量级部署，还有更强大的 Xavier 和 TX 变体，它们基于 Nvidia Volta 和 Pascal GPU 架构。Nano 适合家庭自动化等应用，其余则适用于如工业机器人等计算密集型应用。

### 摘要

在本系列中，我们首先展示了深度学习领域模型的快速增长，并激发了在扩展模型时对效率和性能的需求。接着，我们为读者提供了一个心理模型，以理解他们可以使用的广泛工具和技术。然后，我们深入到每个重点领域，提供了详细的示例，并进一步阐明了迄今为止在行业和学术界所做的工作。我们希望这系列博客文章能提供具体且可操作的建议，以及优化模型训练和部署时需要考虑的权衡。我们邀请读者进一步阅读我们的[模型优化和效率的详细调查论文](https://arxiv.org/abs/2106.08962)以获取更多见解。

### 参考文献

[1] Martín Abadi, Paul Barham, Jianmin Chen, Zhifeng Chen, Andy Davis, Jeffrey Dean, Matthieu Devin, Sanjay Ghemawat, Geoffrey Irving, Michael Isard 等。2016\. Tensorflow: 大规模机器学习系统。在第12届 {USENIX} 操作系统设计与实现会议 ({OSDI} 16)。265–283。

[2] Tensorflow 作者。2021\. TensorFlow Lite | 移动和边缘设备的 ML。https://www.tensorflow.org/lite [在线; 访问日期 2021\. 6月3日]。

[3] XNNPACK 作者。2021\. XNNPACK 后端用于 TensorFlow Lite。https://github.com/tensorflow/tensorflow/blob/master/tensorflow/lite/delegates/xnnpack/README.md/#sparse-inference。

[4] Pete Warden 和 Daniel Situnayake。2019\. Tinyml: 在 Arduino 和超低功耗微控制器上使用 TensorFlow Lite 进行机器学习。“O’Reilly Media, Inc.”。

[5] Tensorflow JS: [https://www.tensorflow.org/js](https://www.tensorflow.org/js)

[6] WebGL: [https://en.wikipedia.org/wiki/WebGL](https://en.wikipedia.org/wiki/WebGL)

[7] TensorFlow. 2019\. TensorFlow 模型优化工具包 — 后训练整数量化。Medium（2019年11月）。[https://medium.com/tensorflow/tensorflow-model-optimization-toolkit-post-training-integer-quantizationb4964a1ea9ba](https://medium.com/tensorflow/tensorflow-model-optimization-toolkit-post-training-integer-quantizationb4964a1ea9ba)

[8] Tensorflow 作者. 2021\. XLA：机器学习优化编译器 | TensorFlow。https://www.tensorflow.org/xla

[9] Pankaj Kanwar, Peter Brandt 和 Zongwei Zhou. 2021\. TensorFlow 2 MLPerf 提交展示了 Google Cloud 上的最佳性能。https://blog.tensorflow.org/2020/07/tensorflow-2-mlperf-submissions.html

[10] Adam Paszke, Sam Gross, Francisco Massa, Adam Lerer, James Bradbury, Gregory Chanan, Trevor Killeen, Zeming Lin, Natalia Gimelshein, Luca Antiga 等. 2019\. Pytorch：一种命令式风格的高性能深度学习库。arXiv 预印本 arXiv:1912.01703 (2019).

[11] PyTorch 作者. 2021\. PyTorch Mobile。https://pytorch.org/mobile/home

[12] PyTorch 作者. 2021\. 量化配方 — PyTorch Tutorials 1.8.1+cu102 文档。https://pytorch.org/tutorials/recipes/quantization.html

[13] PyTorch 作者. 2021\. torch.jit.script — PyTorch 1.8.1 文档。https://pytorch.org/docs/stable/generated/torch.jit.script.html

[14] PyTorch 作者. 2021\. torch.jit.script — PyTorch 1.8.1 文档。https://pytorch.org/docs/stable/generated/torch.jit.script.html

[15] Nadav Rotem, Jordan Fix, Saleem Abdulrasool, Garret Catron, Summer Deng, Roman Dzhabarov, Nick Gibson, James Hegeman, Meghan Lele, Roman Levenstein 等. 2018\. Glow：用于神经网络的图形降级编译技术。arXiv 预印本 arXiv:1805.00907 (2018).

[16] Nicolas Vasilache, Oleksandr Zinenko, Theodoros Theodoridis, Priya Goyal, Zachary DeVito, William S Moses, Sven Verdoolaege, Andrew Adams 和 Albert Cohen. 2018\. Tensor comprehensions：框架无关的高性能机器学习抽象。arXiv 预印本 arXiv:1802.04730 (2018).

[17] PyTorch 作者. 2021\. 性能调优指南 — PyTorch Tutorials 1.8.1+cu102 文档。https://pytorch.org/tutorials/recipes/recipes/tuning_guide.html

[18] Marat Dukhan, Yiming Wu Wu 和 Hao Lu. 2020\. QNNPACK：优化的移动深度学习开源库 - Facebook 工程。https://engineering.fb.com/2018/10/29/ml-applications/qnnpack

[19] XNNPACK 作者. 2021\. XNNPACK。https://github.com/google/XNNPACK

[20] Apple 作者. 2021\. Accelerate | Apple 开发者文档。https://developer.apple.com/documentation/accelerate

[21] Android 开发者. 2021\. 神经网络 API | Android NDK | Android 开发者。https://developer.android.com/ndk/guides/neuralnetworks

[22] 维基媒体项目贡献者. 2021\. CUDA - 维基百科。 https://en.wikipedia.org/w/index.php?title=CUDA&oldid=1025500257.

[23] Alex Krizhevsky, Ilya Sutskever, 和 Geoffrey E Hinton. 2012\. 使用深度卷积神经网络进行Imagenet分类。神经信息处理系统进展 25 (2012), 1097–1105.

[24] NVIDIA. 2020\. 深入Volta：全球最先进的数据中心GPU | NVIDIA开发者博客。 https://developer.nvidia.com/blog/inside-volta.

[25] Dusan Stosic. 2020\. 使用Tensor Cores训练神经网络 - Dusan Stosic, NVIDIA。 https://www.youtube.com/watch?v=jF4-_ZK_tyc

[26] HT Kung 和 CE Leiserson. 1980\. 《VLSI系统简介》。Mead, C. A_ 和 Conway, L., (编辑), Addison-Wesley, Reading, MA (1980), 271–292.

[27] Kaz Sato. 2021\. 什么使TPU适合深度学习？| Google Cloud博客。 https://cloud.google.com/blog/products/ai-machine-learning/what-makes-tpus-fine-tuned-for-deep-learning

**相关：**

+   [如何使用NVIDIA GPU加速库](https://www.kdnuggets.com/2021/07/nvidia-gpu-accelerated-libraries.html)

+   [使用PyTorch和Ray进行分布式机器学习入门](https://www.kdnuggets.com/2021/03/getting-started-distributed-machine-learning-pytorch-ray.html)

+   [适用于TensorFlow的最佳机器学习框架及扩展](https://www.kdnuggets.com/2021/04/best-machine-learning-frameworks-extensions-tensorflow.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的IT

* * *

### 更多相关内容

+   [7种ChatGPT让你编程更高效的方式](https://www.kdnuggets.com/2023/06/7-ways-chatgpt-makes-code-better-faster.html)

+   [oBERT：复合稀疏化为NLP带来更快更准确的模型](https://www.kdnuggets.com/2022/05/obert-compound-sparsification-delivers-faster-accurate-models-nlp.html)

+   [如何从零开始构建和训练Transformer模型…](https://www.kdnuggets.com/how-to-build-and-train-a-transformer-model-from-scratch-with-hugging-face-transformers)

+   [通过参与竞赛将机器学习学习速度提高4倍](https://www.kdnuggets.com/2022/01/learn-machine-learning-4x-faster-participating-competitions.html)

+   [为何我们始终需要人类来训练AI — 有时是实时的](https://www.kdnuggets.com/2021/12/why-we-need-humans-training-ai.html)

+   [使用 Tensorflow 训练图像分类模型指南](https://www.kdnuggets.com/2022/12/guide-train-image-classification-model-tensorflow.html)
