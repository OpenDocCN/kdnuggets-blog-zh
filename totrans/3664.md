# 了解 Gorilla：UC 伯克利和微软的 API 增强 LLM 超越 GPT-4、Chat-GPT 和 Claude

> 原文：[https://www.kdnuggets.com/2023/06/meet-gorilla-uc-berkeley-microsoft-apiaugmented-llm-outperforms-gpt4-chatgpt-claude.html](https://www.kdnuggets.com/2023/06/meet-gorilla-uc-berkeley-microsoft-apiaugmented-llm-outperforms-gpt4-chatgpt-claude.html)

![了解 Gorilla：UC 伯克利和微软的 API 增强 LLM 超越 GPT-4、Chat-GPT 和 Claude](../Images/e0e6aaffd58f0206cd7a019eacfc72f4.png)

图片来自 Adobe Firefly

> * * *
> 
> ## 我们的三大课程推荐
> ## 
> ![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。
> 
> ![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能
> 
> ![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您组织的 IT 需求
> 
> * * *
> 
> 我最近开始了一份专注于 AI 的教育新闻通讯，目前已有超过 160,000 名订阅者。TheSequence 是一份没有废话（即没有炒作、新闻等）的 ML 取向新闻通讯，阅读时间为 5 分钟。其目标是让您了解最新的机器学习项目、研究论文和概念。请通过下面的链接订阅尝试一下：

大型语言模型（LLMs）的最新进展彻底改变了该领域，使其具备了自然对话、数学推理和程序合成等新能力。然而，LLMs 仍面临固有的局限性。它们的信息存储能力受到固定权重的限制，其计算能力也受限于静态图和狭窄的上下文。此外，随着世界的发展，LLMs 需要重新训练以更新其知识和推理能力。为了克服这些限制，研究人员已开始赋予 LLMs 工具。通过访问广泛而动态的知识库和支持复杂计算任务，LLMs 可以利用搜索技术、数据库和计算工具。领先的 LLM 提供商已开始集成插件，允许 LLMs 通过 API 调用外部工具。从有限的手动编码工具转向访问大量云 API，这一过渡有可能将 LLMs 转变为计算基础设施和网络的主要接口。像预订假期或举办会议这样的任务，可能像与一个可以访问航班、租车、酒店、餐饮和娱乐网络 API 的 LLM 对话一样简单。

最近，UC Berkeley 和 Microsoft 的研究人员揭示了[Gorilla](https://shishirpatil.github.io/gorilla/)，这是一个专门为 API 调用设计的 LLaMA-7B 模型。Gorilla 依靠自我指导微调和检索技术，使 LLM 能够从一个大规模且不断发展的工具集合中准确选择，这些工具通过其 API 和文档表达出来。作者通过从主要模型库如 TorchHub、TensorHub 和 HuggingFace 中抓取机器学习 API，构建了一个名为 APIBench 的大型 API 语料库。使用自我指导，他们生成了指令和相应 API 的配对。微调过程涉及将数据转换为用户代理聊天风格的对话格式，并对基础 LLaMA-7B 模型进行标准指令微调。

![认识 Gorilla：UC Berkeley 和 Microsoft 的 API 增强型 LLM 超越 GPT-4、Chat-GPT 和 Claude。](../Images/24a42ceed0c88615bcca7700eed61f53.png)

图片来源：UC Berkeley

API 调用通常带有约束，这增加了 LLM 理解和分类调用的复杂性。例如，一个提示可能要求调用具有特定参数大小和精度约束的图像分类模型。这些挑战突显了 LLM 不仅需要理解 API 调用的功能描述，还需要对嵌入的约束进行推理。

# 数据集

当前的技术数据集涵盖了三个不同的领域：Torch Hub、Tensor Hub 和 HuggingFace。每个领域提供了大量的信息，揭示了数据集的多样性。例如，Torch Hub 提供了 95 个 API，奠定了坚实的基础。相比之下，Tensor Hub 更进一步，拥有多达 696 个 API。最后，HuggingFace 以惊人的 925 个 API 领先，使其成为最全面的领域。

为了提升数据集的价值和可用性，进行了额外的努力。数据集中的每个 API 都附带了一组 10 个精心设计且独特定制的指令。这些指令作为训练和评估的不可或缺的指南。此举确保每个 API 超越了单纯的表示，实现了更强的利用和分析能力。

# 架构

Gorilla 引入了检索感知训练的概念，其中指令调优的数据集包含一个额外的字段，用于参考检索到的 API 文档。这种方法旨在教会 LLM 根据提供的文档解析和回答问题。作者展示了这种技术使 LLM 能够适应 API 文档的变化，提升了性能，减少了幻觉错误。

在推理过程中，用户以自然语言提供提示。猩猩可以在两种模式下操作：零样本和检索。在零样本模式下，提示直接输入到猩猩 LLM 模型中，该模型返回推荐的 API 调用以完成任务或目标。在检索模式下，检索器（BM25 或 GPT-Index）从 API 数据库中检索最新的 API 文档。这些文档与用户提示一起连接，并附有指示 API 文档参考的消息。然后将连接的输入传递给猩猩，猩猩输出要调用的 API。在此系统中，除了连接步骤外，不会进行提示调整。

![认识猩猩：UC 伯克利和微软的 API 增强型 LLM 超越 GPT-4、Chat-GPT 和 Claude。](../Images/d8de042dc35753c3f03d77c8e3b4cc49.png)

图片来源：UC 伯克利

归纳程序合成在多个领域取得了成功，通过合成满足特定测试用例的程序。然而，当涉及评估 API 调用时，仅依靠测试用例是不够的，因为验证代码的语义正确性变得困难。以图像分类为例，该任务有超过 40 个不同的模型可用。即使将其缩小到特定的模型系列，如 Densenet，也有四种可能的配置。因此，存在多种正确答案，使得通过单元测试确定所用 API 是否在功能上等同于参考 API 变得困难。为了评估模型的性能，使用收集的数据集对其功能等价性进行比较。为了识别 LLM 在数据集中调用的 API，采用 AST（抽象语法树）树匹配策略。通过检查候选 API 调用的 AST 是否为参考 API 调用的子树，可以追踪使用的是哪个 API。

识别和定义幻觉是一个重大挑战。AST 匹配过程被利用来直接识别幻觉。在这种情况下，幻觉指的是调用一个不在任何数据库中的 API，本质上是调用一个完全虚构的工具。值得注意的是，这种幻觉的定义不同于错误地调用 API，后者被定义为一种错误。

AST 子树匹配在识别数据集中被调用的具体 API 中扮演了至关重要的角色。由于 API 调用可能具有多个参数，因此每个参数都需要进行匹配。此外，考虑到 Python 允许默认参数，需要定义在数据库中每个 API 应匹配的参数。

![认识猩猩：UC 伯克利和微软的 API 增强型 LLM 超越 GPT-4、Chat-GPT 和 Claude。](../Images/3290cfd852867ae81d5742ba3f6e27f6.png)

图片来源：UC 伯克利

# 《行动中的猩猩》

研究人员与论文一起，[开源了 Gorilla 的一个版本](https://github.com/ShishirPatil/gorilla)。该版本包括一个包含多个示例的笔记本。此外，以下视频清晰地展示了 Gorillas 的一些魔力。

**[gorilla_720p.mp4](https://drive.google.com/file/d/1E0k5mG1mTiaz0kukyK1PdeohJipTFh6j/view)**

Gorilla 是工具增强 LLM 领域中最有趣的方法之一。希望我们能在一些主要的 ML 中心看到该模型的分发。

**[Jesus Rodriguez](https://www.linkedin.com/in/jesusmrv/)** 目前是 Intotheblock 的首席技术官。他是一位技术专家、执行投资者和创业顾问。Jesus 创立了 Tellago，这是一家获奖的软件开发公司，专注于通过利用新兴企业软件趋势帮助公司成为卓越的软件组织。

[原文](https://medium.com/towards-artificial-intelligence/meet-gorilla-uc-berkeley-and-microsofts-api-augmented-llm-outperforms-gpt-4-chat-gpt-and-claude-d764d16bc25b)。已获得许可转载。

### 相关话题

+   [检测 ChatGPT、GPT-4、Bard 和 Claude 的前 10 个工具](https://www.kdnuggets.com/2023/05/top-10-tools-detecting-chatgpt-gpt4-bard-llms.html)

+   [ChatGPT 被推翻：Claude 如何成为新的 AI 领导者](https://www.kdnuggets.com/2023/07/chatgpt-dethroned-claude-became-new-ai-leader.html)

+   [认识 MetaGPT：将文本转化为...的 ChatGPT 驱动 AI 助手](https://www.kdnuggets.com/meet-metagpt-the-chatgptpowered-ai-assistant-that-turns-text-into-web-apps)

+   [Visual ChatGPT: 微软结合 ChatGPT 和 VFMs](https://www.kdnuggets.com/2023/03/visual-chatgpt-microsoft-combine-chatgpt-vfms.html)

+   [开始使用刚刚打破 GPT-4 和 Gemini 的 Claude 3 Opus](https://www.kdnuggets.com/getting-started-with-claude-3-opus-that-just-destroyed-gpt-4-and-gemini)

+   [3 种免费访问 Claude AI 的方法](https://www.kdnuggets.com/2023/06/3-ways-access-claude-ai-free.html)
