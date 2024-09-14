# StarCoder: 你一直想要的编码助手

> 原文：[https://www.kdnuggets.com/2023/05/starcoder-coding-assistant-always-wanted.html](https://www.kdnuggets.com/2023/05/starcoder-coding-assistant-always-wanted.html)

![StarCoder: 你一直想要的编码助手](../Images/dcfa6898a7d6e9d40621c1d9ab6195d2.png)

作者提供的图片

# 什么是 StarCoder？

[StarCoder](https://huggingface.co/blog/starcoder) 是一个前沿的大型语言模型，专门设计用于代码。它拥有令人印象深刻的 15.5B 参数和 8K 的扩展上下文长度，在填充能力方面表现优异，并通过多查询注意力实现快速的大批量推断。

StarCoderBase 在一个包含 1 万亿个标记的庞大数据集上进行了训练，这些数据来自 [The Stack](https://huggingface.co/datasets/bigcode/the-stack)。该集合包括许可使用的 GitHub 仓库，配有检查工具和隐私意识开发者的选择退出过程。为了进一步提升性能，BigCode 团队通过 35B Python 标记对 StarCoderBase 进行了精细调整。

结果是，StarCoder 成为一个强大且精炼的语言模型，能够处理广泛的编码任务，表现出卓越的能力。

![StarCoder: 你一直想要的编码助手](../Images/5ff301049158fd276376e51b2112c7a9.png)

来自 [StarCoder Paper](https://arxiv.org/pdf/2305.06161.pdf) 的图片

StarCoderBase 超越了所有现有的开源代码语言模型，这些模型支持多种编程语言，并在质量和结果方面表现出色，甚至在质量和结果上超越了著名的 OpenAI code-cushman-001 模型。此外，StarCoder 可以被提示达到 40% pass@1 的 HumanEval 分数。它的表现优于 LaMDA、LLaMA 和 PaLM 模型。

阅读 [研究论文](https://arxiv.org/pdf/2305.06161.pdf) 了解更多有关模型评估的信息。

# StartCoder 代码补全

[BigCode - StarCoder](https://huggingface.co/spaces/bigcode/bigcode-playground) 代码补全 playground 是测试模型能力的绝佳方式。你可以尝试各种模型格式、前缀和填充内容，以获得全面的体验。

在我看来，这是一个很好的代码补全工具，尤其适用于 Python 代码。然而，它确实存在一些缺点，例如过时的 API、幻觉现象、显示 Jupyter Notebook 元数据和不完整的代码。

使用 StarCoder 生成代码的最佳方法是使用详细解释的注释。这将帮助模型更好地理解你的意图，并生成更准确的结果。

![StarCoder: 你一直想要的编码助手](../Images/346c85e1b44df31961f7747716b5927a.png)

来自 StartCoder 代码补全的图片

# StarChat Playground

如果你习惯了 ChatGPT 生成代码的风格，那么你应该尝试 [StarChat](https://huggingface.co/spaces/HuggingFaceH4/starchat-playground) 来生成和优化代码。

StarChat 是 StarCoderBase 的一个专用版本，它经过了在 [Dolly](https://huggingface.co/datasets/databricks/databricks-dolly-15k) 和 [OpenAssistant](https://huggingface.co/datasets/OpenAssistant/oasst1) 数据集上的微调，成为了一个真正宝贵的编码助手。它是一个拥有160亿参数的模型，预训练于一万亿个令牌，这些令牌来自80多种编程语言、GitHub问题、Git提交和Jupyter笔记本。

你可以向StarChat提供指令，它会生成带有解释的代码。你还可以使用后续提示来修改代码。

![StarCoder: 你一直想要的编码助手](../Images/b9e08edb1b173d6fd2c8e05e8094aa58.png)

图片来自StarChat Playground

# HF代码自动补全

[HF代码自动补全](https://marketplace.visualstudio.com/items?itemName=HuggingFace.huggingface-vscode) 是一个免费的开源替代方案，取代了GitHub Copilot，并由StarCoder提供支持。我自其发布以来一直在使用它，对其速度和准确性感到非常满意。

![StarCoder: 你一直想要的编码助手](../Images/c8e1e13adaa2c200b1d5aa5f59946441.png)

HF代码自动补全 VSCode扩展

它与Jupyter Notebook以及VSCode中的各种文件兼容。你只需从市场安装扩展并添加Hugging Face API即可。

![StarCoder: 你一直想要的编码助手](../Images/6512bb12a082b84a1f5f0b2d31403a6d.png)

图片由作者提供 | VSCode

# 结论

我们的工作场所中始终需要高级代码助手，它们能够有效处理重复脚本，同时协助创建更复杂的系统。

在这篇博客中，我们详细探讨了StarCoder及其广泛的应用范围。值得注意的是，开源社区在不懈努力地推动代码辅助的边界，不断致力于提供突破性的解决方案，以提升我们的编码体验和生产力。

希望你喜欢阅读这篇博客，并发现它富有信息和洞察力。如果你想了解更多关于最新AI技术的信息，可以在LinkedIn上关注我。

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan)) 是一位认证数据科学专业人员，热衷于构建机器学习模型。目前，他专注于内容创作，并撰写有关机器学习和数据科学技术的技术博客。Abid拥有技术管理硕士学位和电信工程学士学位。他的愿景是使用图神经网络为面临心理健康问题的学生打造一款AI产品。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯的捷径。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 需求

* * *

### 更多相关内容

+   [为什么我们始终需要人类来训练 AI——有时甚至需要实时训练](https://www.kdnuggets.com/2021/12/why-we-need-humans-training-ai.html)

+   [事情并非总是正常：一些“其他”分布](https://www.kdnuggets.com/2023/01/things-arent-always-normal-distributions.html)

+   [始终学习：AI 如何防止数据泄露](https://www.kdnuggets.com/2023/07/always-learning-ai-prevents-data-breaches.html)

+   [MLOps 思维：始终保持生产就绪](https://www.kdnuggets.com/2023/07/mlops-mindset-always-productionready.html)

+   [你想知道的所有机器学习相关内容](https://www.kdnuggets.com/2022/09/everything-youve-ever-wanted-to-know-about-machine-learning.html)

+   [KDnuggets 新闻，9月14日：免费数据科学 Python 课程 •…](https://www.kdnuggets.com/2022/n36.html)
