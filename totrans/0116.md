# Pythia: 一套用于深入研究的 16 个 LLM

> 原文：[https://www.kdnuggets.com/2023/08/pythia-suite-16-llms-indepth-research.html](https://www.kdnuggets.com/2023/08/pythia-suite-16-llms-indepth-research.html)

![Pythia: 一套用于深入研究的 16 个 LLM](../Images/c61095fd3fa8a2c8268d5fed9d416497.png)

图片来源：作者

如今，大型语言模型和如 ChatGPT 和 GPT-4 这样的 LLM 驱动的聊天机器人已广泛融入我们的日常生活中。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT

* * *

然而，仅解码的自回归变换器模型在 LLM 应用成为主流之前就已经广泛用于生成 NLP 应用。了解它们在训练中的演变以及它们随着规模扩大而性能的变化是很有帮助的。

Pythia 是 [Eleuther AI](https://www.eleuther.ai/) 的一个项目，提供了一套 16 个大型语言模型，用于研究、分析和进一步研究的可重复性。本文介绍了 [Pythia](https://github.com/EleutherAI/pythia)。

# Pythia 套件提供了什么？

如前所述，Pythia 是一套由 16 个大型语言模型组成的套件——仅解码的自回归变换器模型——在公开可用的数据集上进行训练。套件中的模型大小从 7000 万到 120 亿参数不等。

+   整个套件是在相同的数据和相同的顺序下训练的。这有助于训练过程的可重复性。因此，我们不仅可以复制训练管道，还可以深入分析语言模型并研究其行为。

+   套件还提供了下载训练数据加载器和每个 16 个语言模型的 154 个模型检查点的设施。

# 训练数据和训练过程

现在，让我们深入了解 Pythia LLM 套件的详细信息。

## 训练数据集

Pythia LLM 套件是在以下数据集上训练的：

+   [Pile 数据集](https://arxiv.org/abs/2101.00027)，包含 3000 亿个标记

+   去重的 Pile 数据集，包含 2070 亿个标记。

共有 8 种不同的模型尺寸，其中最小和最大的模型分别具有 7000 万和 120 亿个参数。其他模型尺寸包括 1600 万、4100 万、10 亿、14 亿、28 亿和 69 亿。

这些模型中的每一个都在 Pile 和去重的 Pile 数据集上进行训练，形成了总共 16 个模型。下表展示了模型的大小和部分超参数。

![Pythia: 一套用于深入研究的 16 个 LLM](../Images/4c7cde031787d773503da571915b54d9.png)

模型和超参数 | [图像来源](https://arxiv.org/pdf/2304.01373.pdf)

有关使用的超参数的完整细节，请参阅 [Pythia: A Suite for Analyzing Large Language Models Across Training and Scaling](https://arxiv.org/abs/2304.01373)。

## 训练过程

这里是架构和训练过程的概述：

+   所有模型都有完全密集的层，并使用 [flash attention](https://github.com/HazyResearch/flash-attention)。

+   为了更容易解释，使用了未绑定的嵌入矩阵。

+   使用了 1024 的批量大小和 2048 的序列长度。这个大批量大小大大减少了实际训练时间。

+   训练过程还利用了诸如数据和张量并行等优化技术

对于训练过程，使用了由 Eleuther AI 开发的 [GPT-Neo-X library](https://github.com/EleutherAI/gpt-neox)（包括来自 [DeepSpeed](https://www.deepspeed.ai/) library 的功能）。

## 模型检查点

每个模型有154个检查点。每1000次迭代设置一个检查点。此外，在训练过程的早期阶段，还有按对数间隔设置的检查点：1、2、4、8、16、32、64、128、256 和 512。

## Pythia 与其他语言模型的比较

Pythia LLM 套件与现有语言建模基准进行评估，包括 [OpenAI 的 LAMBADA](https://paperswithcode.com/sota/language-modelling-on-lambada) 变体。结果发现，Pythia 的表现与 OPT 和 BLOOM 语言模型相当。

# 优势和局限性

Pythia LLM 套件的主要优势是可重复性。数据集是公开的，预标记的数据加载器和154个模型检查点也都是公开的。超参数的完整列表也已发布。这使得模型训练和分析的复制变得更简单。

在 [1] 中，作者解释了他们为何选择英文数据集而非多语言文本语料库的理由。但拥有可重复的多语言大型语言模型训练流程是有帮助的，特别是在鼓励更多的研究和多语言大型语言模型动态的研究方面。

# 案例研究概述

研究还展示了利用 Pythia 套件中大型语言模型训练过程的可重复性的一些有趣的案例研究。

## 性别偏见

所有大型语言模型都容易受到偏见和虚假信息的影响。研究集中在通过修改预训练数据来减轻性别偏见，使得固定比例的数据包含特定性别的代词。这种预训练也是可重复的。

## 记忆化

大型语言模型中的记忆化也是一个广泛研究的领域。序列记忆化被建模为泊松点过程。研究旨在了解特定序列在训练数据集中的位置是否会影响记忆化。观察结果表明，位置不会影响记忆化。

## 预训练术语频率的影响

对于参数数量为 2.8B 及以上的语言模型，在预训练语料库中出现特定任务术语的情况被发现可以提高模型在问答等任务上的表现。

模型的规模与在更复杂任务（如算术和数学推理）上的表现之间也存在相关性。

![Pythia：一个用于深入研究的 16 个 LLM 套件](../Images/972d18fbdb29c398a48f15661db1b227.png)

算术加法任务表现 | [图片来源](https://arxiv.org/pdf/2304.01373.pdf)

# 总结与下一步

让我们总结一下讨论中的关键点。

+   Pythia by Eleuther AI 是一个包含 16 个大型语言模型（LLMs）的套件，这些模型在公开可用的 Pile 数据集和去重后的 Pile 数据集上进行了训练。

+   LLMs 的规模范围从 70M 到 12B 参数不等。

+   训练数据和模型检查点是开源的，能够重建精确的训练数据加载器。因此，LLM 套件对于更好地理解大型语言模型的训练动态非常有帮助。

下一步，你可以在 [Hugging Face Hub](https://huggingface.co/EleutherAI) 上探索 Pythia 模型套件和模型检查点。

# 参考资料

[1] [Pythia：分析大型语言模型的训练和扩展的套件](https://arxiv.org/abs/2304.01373)，arXiv，2023

**[Bala Priya C](https://www.linkedin.com/in/bala-priya/)** 是来自印度的开发人员和技术作家。她喜欢在数学、编程、数据科学和内容创作的交汇处工作。她的兴趣和专业领域包括 DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编程和喝咖啡！目前，她致力于学习并通过编写教程、操作指南、观点文章等与开发者社区分享她的知识。

### 更多相关话题

+   [理解机器学习算法：深入概述](https://www.kdnuggets.com/understanding-machine-learning-algorithms-an-indepth-overview)

+   [功能数据中的异常检测密度核深度](https://www.kdnuggets.com/density-kernel-depth-for-outlier-detection-in-functional-data)

+   [利用 LLMs 从非结构化到结构化数据](https://www.kdnuggets.com/2023/06/predibase-unstructured-structured-data-llms.html)

+   [确保 LLMs 的可靠少量样本提示选择](https://www.kdnuggets.com/2023/07/ensuring-reliable-fewshot-prompt-selection-llms.html)

+   [介绍 OpenLLM：开源大型语言模型库](https://www.kdnuggets.com/2023/07/introducing-openllm-open-source-library-llms.html)

+   [ReAct：推理和行动增强 LLMs 以使用工具！](https://www.kdnuggets.com/react-reasoning-and-acting-augments-llms-with-tools)
