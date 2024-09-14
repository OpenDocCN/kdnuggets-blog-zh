# 优化你的 LLM 以提高性能和可扩展性

> 原文：[https://www.kdnuggets.com/optimizing-your-llm-for-performance-and-scalability](https://www.kdnuggets.com/optimizing-your-llm-for-performance-and-scalability)

![文章主封面](../Images/a88cf9dfeda8c9650770b5f02f9edfa1.png)

图片来源：作者

大型语言模型或 LLM 已成为自然语言处理的推动力。它们的使用场景从聊天机器人和虚拟助手到内容生成和翻译服务。然而，它们已经成为科技界增长最快的领域之一——我们可以在各种场景中见到它们。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

随着对更强大语言模型需求的增长，对有效优化技术的需求也在增加。

然而，许多自然的问题出现了：

如何提升它们的知识？

如何提升它们的整体性能？

如何扩展这些模型？

OpenAI DevDay 上由 John Allard 和 Colin Jarvis 主讲的题为“最大化 LLM 性能的技术调查”的深刻演讲试图回答这些问题。如果你错过了这一活动，可以在[YouTube](https://www.youtube.com/watch?v=ahnGLM-RC1Y)上观看这场讲座。

这场演讲提供了各种技术和最佳实践的精彩概述，以提升 LLM 应用的性能。本文旨在总结改善我们 AI 驱动的解决方案性能和可扩展性的最佳技术。

## 理解基础知识

LLM 是精密的算法，旨在理解、分析和生成连贯且符合上下文的文本。它们通过对大量语言数据进行广泛训练，涵盖各种主题、方言和风格，从而实现这一目标。因此，它们能够理解人类语言的运作方式。

然而，在将这些模型集成到复杂应用中时，需要考虑一些关键挑战：

#### 优化 LLM 面临的主要挑战

+   LLM 的准确性：确保 LLM 输出准确且可靠的信息，没有幻觉。

+   资源消耗：LLM 需要大量的计算资源，包括 GPU 能力、内存和大型基础设施。

+   延迟：实时应用要求低延迟，这可能由于 LLM 的规模和复杂性而具有挑战性。

+   可扩展性：随着用户需求的增长，确保模型能够处理增加的负载而不降低性能至关重要。

## 提升表现的策略

第一个问题是关于“如何提升其知识？”

创建一个部分功能的LLM演示相对简单，但将其完善以投入生产需要迭代改进。LLM可能在需要深厚专业知识、系统和流程理解或精准行为的任务中遇到困难。

团队使用提示工程、检索增强和微调来应对这一问题。一个常见的错误是认为这个过程是线性的，应按特定顺序进行。相反，根据问题的性质，从两个轴来处理更为有效：

1.  上下文优化：问题是否由于模型缺乏获取正确信息或知识？

1.  LLM优化：模型是否未能生成正确的输出，例如不准确或不符合期望的风格或格式？

![理解我们LLM的上下文需求。](../Images/3cae5f5fa83b8fc4333fc9bdccaf50f6.png)

作者提供的图像

为应对这些挑战，可以使用三种主要工具，每种工具在优化过程中都扮演着独特的角色：

#### 提示工程

定制提示以引导模型的响应。例如，改进客服机器人提示，以确保其始终提供有用且礼貌的回答。

#### 检索增强生成（RAG）

通过外部数据提升模型的上下文理解。例如，将医疗聊天机器人与最新研究论文数据库集成，以提供准确且最新的医疗建议。

#### 微调

修改基础模型以更好地适应特定任务。就像使用法律文本数据集对法律文档分析工具进行微调，以提高其总结法律文档的准确性。

该过程高度迭代，并非所有技术都会对你的具体问题有效。然而，许多技术是可以叠加的。当你找到有效的解决方案时，可以将其与其他性能改进结合，以实现**最佳结果**。

## **优化性能的策略**

第二个问题是关于“如何提升其整体性能？”

在拥有准确的模型后，第二个令人关切的问题是推理时间。推理是一个训练好的语言模型，如GPT-3，在现实应用中生成对提示或问题的响应的过程（例如聊天机器人）。

这是一个关键阶段，在实际场景中对模型进行测试，生成预测和响应。对于像GPT-3这样的巨大LLM，计算需求是巨大的，因此在推理期间进行优化至关重要。

考虑像GPT-3这样的模型，它有1750亿个参数，相当于700GB的float32数据。这个规模，加上激活需求，要求显著的RAM。这就是为什么在没有优化的情况下运行GPT-3需要一个庞大的设置。

一些技术可以用来减少执行这些应用所需的资源量：

#### 模型剪枝

这涉及修剪非关键参数，确保只有对性能至关重要的参数保留。这可以大幅减少模型的大小，而不会显著影响其准确性。

这意味着计算负载显著减少，同时保持相同的准确性。你可以在以下GitHub中找到易于实现的修剪代码。

#### 量化

这是一种模型压缩技术，将LLM的权重从高精度变量转换为低精度变量。这意味着我们可以将32位浮点数减少到16位或8位等更节省内存的格式，这可以大幅减少内存占用并提高推理速度。

LLMs可以通过HuggingFace和bitsandbytes以量化方式轻松加载。这使我们能够在低功耗资源上执行和微调LLMs。

```py
from transformers import AutoModelForSequenceClassification, AutoTokenizer 
import bitsandbytes as bnb 

# Quantize the model using bitsandbytes 
quantized_model = bnb.nn.quantization.Quantize( 
model, 
quantization_dtype=bnb.nn.quantization.quantization_dtype.int8 
) 
```

#### 蒸馏

这是训练一个较小模型（学生）以模拟较大模型（也称为教师）性能的过程。这个过程涉及训练学生模型以模拟教师的预测，使用教师的输出logits和真实标签的组合。通过这样做，我们可以在资源需求的很小一部分中实现类似的性能。

这个想法是将较大模型的知识转移到具有更简单架构的较小模型上。最著名的例子之一是Distilbert。

这个模型是模拟Bert性能的结果。它是BERT的一个较小版本，保留了97%的语言理解能力，同时速度快60%，体积小40%。

## 可扩展性技术

第三个问题是关于“如何扩展这些模型？”

这一步通常至关重要。当一个系统被少量用户使用时，它的行为可能与在扩展以应对密集使用时大相径庭。以下是一些应对这一挑战的技术：

#### 负载均衡

这种方法有效分配传入请求，确保计算资源的最佳利用和对需求波动的动态响应。例如，为了在不同国家提供像ChatGPT这样的广泛使用服务，最好部署多个相同模型的实例。

有效的负载均衡技术包括：

横向扩展：添加更多模型实例以应对增加的负载。使用像Kubernetes这样的容器编排平台来管理不同节点上的这些实例。

垂直扩展：升级现有机器资源，例如CPU和内存。

#### 分片

模型分片将模型的片段分布在多个设备或节点上，实现并行处理并显著减少延迟。完全分片数据并行（FSDP）提供了利用各种硬件的关键优势，如GPU、TPU和其他在多个集群中的专用设备。

这种灵活性允许组织和个人根据他们的具体需求和预算来优化硬件资源。

#### 缓存

实现缓存机制可以通过存储频繁访问的结果来减少对 LLM 的负载，这对于有重复查询的应用程序尤其有利。缓存这些频繁的查询可以通过消除重复处理相同请求的需要，从而显著节省计算资源。

此外，批处理可以通过将类似任务分组来优化资源使用。

## 结论

对于那些依赖 LLM 的应用程序来说，这里讨论的技术对于最大化这种变革性技术的潜力至关重要。掌握并有效应用策略以获得更准确的模型输出、优化其性能以及实现扩展，是从一个有前景的原型发展为一个强大的、生产就绪的模型的重要步骤。

要充分理解这些技术，我强烈推荐深入了解并开始在你的 LLM 应用程序中实验这些技术，以获得最佳结果。

**[](https://www.linkedin.com/in/josep-ferrer-sanchez/)**[Josep Ferrer](https://www.linkedin.com/in/josep-ferrer-sanchez)**** 是一位来自巴塞罗那的分析工程师。他毕业于物理工程专业，目前在应用于人类流动的数据科学领域工作。他是一个兼职内容创作者，专注于数据科学和技术。Josep 撰写关于人工智能的文章，涵盖了该领域的持续爆炸性发展。

### 更多相关内容

+   [使用大型语言模型时优化性能和成本的策略](https://www.kdnuggets.com/strategies-for-optimizing-performance-and-costs-when-using-large-language-models-in-the-cloud)

+   [优化 Python 代码性能：深入探讨 Python 性能分析器](https://www.kdnuggets.com/2023/02/optimizing-python-code-performance-deep-dive-python-profilers.html)

+   [Web LLM：将 LLM 聊天机器人带到浏览器](https://www.kdnuggets.com/2023/05/webllm-bring-llm-chatbots-browser.html)

+   [数据科学中的可扩展性挑战与策略](https://www.kdnuggets.com/scalability-challenges-strategies-in-data-science)

+   [优化数据存储：探索 SQL 中的数据类型和规范化](https://www.kdnuggets.com/optimizing-data-storage-exploring-data-types-and-normalization-in-sql)

+   [使用遗传算法优化基因](https://www.kdnuggets.com/2022/04/optimizing-genes-genetic-algorithm.html)
