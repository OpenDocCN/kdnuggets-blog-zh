# Hugging Face Diffusers 概览

> 原文：[`www.kdnuggets.com/an-overview-of-hugging-face-diffusers`](https://www.kdnuggets.com/an-overview-of-hugging-face-diffusers)

![Hugging Face Diffusers 概览](img/f6108832a80f1123ece521a4c494e821.png)

图片由作者提供

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

Diffusers 是一个由 HuggingFace 开发和维护的 Python 库。它简化了从用户定义的提示生成图像的扩散模型的开发和推理。代码在 [GitHub](https://github.com/huggingface/diffusers) 上公开提供，仓库有 22.4k 星标。HuggingFace 还维护了多种 Stable Diffusion 和其他扩散模型，可以轻松地与其库一起使用。

## 安装和设置

从一个新的 Python 环境开始是很好的，以避免库版本和依赖之间的冲突。

要设置一个新的 Python 环境，请运行以下命令：

```py
python3 -m venv venv
source venv/bin/activate
```

安装 Diffusers 库非常简单。它作为一个官方 pip 包提供，并内部使用 PyTorch 库。此外，许多扩散模型基于 Transformers 架构，因此加载模型也需要 transformers pip 包。

```py
pip install 'diffusers[torch]' transformers
```

## 使用 Diffusers 生成 AI 图像

Diffuser 库使得通过稳定扩散模型从提示生成图像变得非常简单。这里，我们将逐行探讨一个简单的代码，了解 Diffusers 库的不同部分。

### 导入

```py
import torch
from diffusers import AutoPipelineForText2Image
```

torch 包将用于 diffuser 管道的一般设置和配置。AutoPipelineForText2Image 是一个自动识别正在加载的模型的类，例如 StableDiffusion1-5、StableDiffusion2.1 或 SDXL，并内部加载适当的类和模块。这让我们避免了每次想加载新模型时都需要更改管道的麻烦。

### 加载模型

扩散模型由多个组件组成，包括但不限于文本编码器、UNet、调度器和变分自编码器。我们可以单独加载这些模块，但 diffusers 库提供了一个构建方法，可以加载给定结构化检查点目录的预训练模型。对于初学者来说，可能很难知道使用哪个管道，因此 AutoPipeline 使得为特定任务加载模型变得更容易。

在这个例子中，我们将加载一个由 Stability AI 训练的、在[HuggingFace](https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0)上公开提供的 SDXL 模型。目录中的文件根据其名称进行结构化，每个目录都有自己的 safetensors 文件。SDXL 模型的目录结构如下所示：

![SDXL 模型的目录结构](img/13b6b0b3589de3e35e8729d6e8c7f19c.png)

要在我们的代码中加载模型，我们使用 AutoPipelineForText2Image 类并调用 from_pretrained 函数。

```py
pipeline = AutoPipelineForText2Image.from_pretrained(
	"stability/stable-diffusion-xl-base-1.0",
	torch_dtype=torch.float32 # Float32 for CPU, Float16 for GPU,  
) 
```

我们将模型路径作为第一个参数提供。它可以是上述的 HuggingFace 模型卡名称，也可以是您提前下载模型的本地目录。此外，我们将模型权重精度定义为关键字参数。当我们在 CPU 上运行模型时，通常使用 32 位浮点精度。然而，运行扩散模型计算成本高昂，且在 CPU 设备上运行推理将需要几个小时！对于 GPU，我们可以使用 16 位或 32 位数据类型，但 16 位更优，因为它利用了较低的 GPU 内存。

上述命令将从 HuggingFace 下载模型，下载时间可能会根据您的互联网连接而有所不同。模型的大小可以从 1GB 到超过 10GB 不等。

一旦模型加载完成，我们需要将模型移动到合适的硬件设备上。使用以下代码将模型移动到 CPU 或 GPU。请注意，对于 Apple Silicon 芯片，将模型移动到 MPS 设备以利用 MacOS 设备上的 GPU。

```py
# "mps" if on M1/M2 MacOS Device
DEVICE = "cuda" if torch.cuda.is_available() else "cpu"   
pipeline.to(DEVICE) 
```

### 推理

现在，我们已经准备好使用加载的扩散模型从文本提示中生成图像。我们可以使用以下代码进行推理：

```py
prompt = "Astronaut in a jungle, cold color palette, muted colors, detailed, 8k"
results = pipeline(
  prompt=prompt,
  height=1024,
  width=1024,
  num_inference_steps=20,
)
```

我们可以使用管道对象，并通过多个关键字参数调用它以控制生成的图像。我们将提示定义为描述我们希望生成的图像的字符串参数。此外，我们可以定义生成图像的高度和宽度，但应为 8 或 16 的倍数，因为底层的变压器架构要求如此。此外，总的推理步骤可以调整以控制最终图像的质量。更多的去噪步骤会生成更高质量的图像，但需要更长时间生成。

最终，管道返回一个生成的图像列表。我们可以从数组中访问第一个图像，并将其作为 Pillow 图像进行操作，以便保存或显示图像。

```py
img = results.images[0]
img.save('result.png')
img # To show the image in Jupyter notebook 
```

![生成的图像](img/d66a9e1aa7d812723bc97cbab89a52d4.png)

生成的图像

## 高级用法

文本到图像的示例只是一个基础教程，旨在突出 Diffusers 库的基本用法。它还提供了多种其他功能，包括图像到图像生成、修复、扩展以及控制网络。此外，它们还提供了对扩散模型中每个模块的精细控制。它们可以作为小型构建块，无缝集成以创建自定义扩散管道。此外，它们还提供了在自己的数据集和用例上训练扩散模型的额外功能。

## 总结

在这篇文章中，我们介绍了 Diffusers 库的基础知识以及如何使用扩散模型进行简单的推理。这是最常用的生成 AI 管道之一，每天都有功能和修改。你可以尝试很多不同的用例和功能，而 [HuggingFace 文档](https://huggingface.co/docs/diffusers/en/index) 和 [GitHub 代码](https://github.com/huggingface/diffusers) 是你入门的最佳地方。

**[](https://www.linkedin.com/in/kanwal-mehreen1/)**[Kanwal Mehreen](https://www.linkedin.com/in/kanwal-mehreen1/)**** Kanwal 是一位机器学习工程师和技术作家，对数据科学以及 AI 与医学的交叉领域充满热情。她共同撰写了电子书《用 ChatGPT 最大化生产力》。作为 2022 年 APAC 地区的 Google Generation 学者，她倡导多样性和学术卓越。她还被认可为 Teradata 技术多样性学者、Mitacs Globalink 研究学者和哈佛 WeCode 学者。Kanwal 是变革的积极倡导者，创立了 FEMCodes，旨在赋能 STEM 领域的女性。

### 更多相关话题

+   [前 10 名机器学习演示：Hugging Face Spaces 版](https://www.kdnuggets.com/2022/05/top-10-machine-learning-demos-hugging-face-spaces-edition.html)

+   [一个社区正在为客户数据建模开发 Hugging Face](https://www.kdnuggets.com/2022/08/objectiv-community-developing-hugging-face-customer-data-modeling.html)

+   [用 Hugging Face 和 Gradio 在 5 分钟内构建 AI 聊天机器人](https://www.kdnuggets.com/2023/06/build-ai-chatbot-5-minutes-hugging-face-gradio.html)

+   [如何使用 Hugging Face AutoTrain 对 LLM 进行微调](https://www.kdnuggets.com/how-to-use-hugging-face-autotrain-to-finetune-llms)

+   [如何使用 Hugging Face AutoTrain 微调 Mistral AI 7B LLM](https://www.kdnuggets.com/how-to-finetune-mistral-ai-7b-llm-with-hugging-face-autotrain)

+   [Mistral 7B-V0.2：使用 Hugging Face 微调 Mistral 的新开源 LLM](https://www.kdnuggets.com/mistral-7b-v02-fine-tuning-mistral-new-open-source-llm-with-hugging-face)
