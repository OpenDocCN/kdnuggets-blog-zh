# 酿造特定领域的LLM魔药

> 原文：[https://www.kdnuggets.com/2023/08/brewing-domainspecific-llm-potion.html](https://www.kdnuggets.com/2023/08/brewing-domainspecific-llm-potion.html)

![酿造特定领域的LLM魔药](../Images/f59d17e898dc57d2cb666479c010020f.png)

图片来自Unsplash

亚瑟·克拉克曾名言：“任何足够先进的技术都与魔法无异。” 人工智能通过引入视觉和语言（V&L）模型以及大语言模型（LLMs）已越过了这条界限。像[Promptbase](https://promptbase.com)这样的项目本质上是将正确的词汇以正确的顺序编织起来，以召唤看似自发的结果。如果“提示工程”不符合施法的标准，那么很难说什么符合。而且，提示的质量[很重要](https://toloka.ai/blog/best-stable-diffusion-prompts/)。更好的“咒语”能带来更好的结果！

几乎每家公司都热衷于利用这股LLM魔力。但只有当你能够将LLM与特定业务需求对接时，它才是魔力，比如从你的知识库中总结信息。

让我们开始一场冒险，揭示制作强效魔药的秘诀——一个具有领域特定专业知识的LLM。作为一个有趣的例子，我们将开发一个擅长《文明6》的LLM，这个概念足够极客，能引起我们的兴趣，拥有一个出色的[WikiFandom](https://civilization.fandom.com/wiki/Civilization_Games_Wiki)并且许可证为CC-BY-SA，并且不复杂，以至于即使是非粉丝也能跟随我们的例子。

# 第1步：解读文档

语言模型（LLM）可能已经具备一些特定领域的知识，使用正确的提示词即可访问这些知识。然而，你可能已有现存的文档存储了你想要利用的知识。找到这些文档并继续下一步。

# 第2步：分割你的咒语

为了让你的领域特定知识对LLM可访问，将你的文档分割成更小、更易消化的部分。这种分割可以提高理解度，并使相关信息的检索变得更加容易。对我们来说，这涉及到将Fandom Wiki的Markdown文件分割成若干部分。不同的LLM可以处理不同长度的提示。将文档拆分成长度显著小于（例如，10%或更少）最大LLM输入长度的部分是合理的。

# 第3步：创建知识精华并构建你的向量数据库

使用相应的嵌入向量对每个分割后的文本进行编码，例如使用[Sentence Transformers](https://sbert.net/)。

将生成的嵌入向量及其对应的文本存储在向量数据库中。你可以使用Numpy和SKlearn的KNN自行完成，但经验丰富的从业者通常推荐使用向量[数据库](https://towardsdatascience.com/milvus-pinecone-vespa-weaviate-vald-gsi-what-unites-these-buzz-words-and-what-makes-each-9c65a3bd0696)。

# 第4步：打造引人入胜的提示词

当用户询问LLM关于《文明6》的问题时，你可以在向量数据库中搜索与问题嵌入相似的元素。你可以在你制作的提示中使用这些文本。

# 第5步：管理背景的药剂锅

让我们认真对待魔法吧！你可以在提示中添加数据库元素，直到达到为提示设置的最大上下文长度。密切注意第2步中你的文本部分的大小。通常，嵌入文档的大小和你在提示中包含的数量之间存在显著的权衡。

# 第6步：选择你的魔法成分

无论你为最终解决方案选择了哪个LLM，这些步骤都是适用的。LLM的格局正在迅速变化，因此，一旦你的流程准备好后，选择你的成功指标，并对不同模型进行并行比较。例如，我们可以比较[Vicuna-13b](https://medium.com/@martin-thissen/vicuna-13b-best-free-chatgpt-alternative-according-to-gpt-4-tutorial-gpu-ec6eb513a717)和GPT-3.5-turbo。

# 第7步：测试你的药剂

测试我们的“药剂”是否有效是下一步。说起来容易做起来难，因为目前尚无科学共识来评估LLM。一些研究人员开发了新的基准，如[HELM](https://github.com/stanford-crfm/helm)或[BIG-bench](https://github.com/google/BIG-bench)，而其他人则提倡人类在环评估，或通过一个更高级的模型来评估领域特定LLM的输出。每种方法都有优缺点。对于涉及领域特定知识的问题，你需要建立一个与你的业务需求相关的评估流程。不幸的是，这通常需要从零开始。

# 第8步：揭示神谕并施展答案和评估

首先，收集一组问题来评估领域特定LLM的表现。这可能是一项繁琐的任务，但在我们的《文明》示例中，我们利用了Google Suggest。我们使用了像“文明6如何……”这样的搜索查询，并将Google的建议作为评估我们解决方案的问题。然后，使用一组与领域相关的问题，运行你的问答流程。为每个问题形成提示并生成答案。

# 第9步：通过预言者的视角评估质量

一旦你拥有了答案和原始查询，你必须评估它们的一致性。根据你期望的精确度，你可以将你的LLM答案与优越模型进行比较，或使用[Toloka上的并排比较](https://toloka.ai/docs/template-builder/templates/sbs-text/)。第二种选择的优势在于直接的人类评估，如果做得正确，可以防止优越LLM可能存在的隐性偏见（例如，[GPT-4](https://arxiv.org/abs/2305.11206)往往会将自己的回答评分更高）。这对实际业务实施可能至关重要，因为这种隐性偏见可能对你的产品产生负面影响。由于我们处理的是一个玩具示例，我们可以遵循第一条路径：将Vicuna-13b和GPT-3.5-turbo的回答与GPT-4进行比较。

# 第10步：提炼质量评估

LLMs通常在开放设置中使用，因此理想情况下，你希望一个LLM能够区分你的向量数据库中有答案的问题与没有答案的问题。以下是由人类在[Toloka](https://toloka.ai/)（即Tolokers）和GPT上对Vicuna-13b和GPT-3.5的并排比较。

| 方法 | Tolokers | GPT-4 |
| --- | --- | --- |
| 模型 | vicuna-13b | GPT-3.5 |
| 可回答，正确答案 | 46.3% | 60.3% | 80.9% |
| 无法回答，AI没有给出答案 | 20.9% | 11.8% | 17.7% |
| 可回答，错误答案 | 20.9% | 20.6% | 1.4% |
| 无法回答，AI给出了一些答案 | 11.9% | 7.3% | 0 |

如果我们检查Tolokers对Vicuna-13b的评估，就如第一列所示，我们可以看到优越模型与人工评估之间的差异。从这次比较中可以得出几个关键点。首先，GPT-4与Tolokers之间的差异值得注意。这些不一致主要发生在领域特定的LLM适当地不作回应时，但GPT-4却将这些无回应的情况评分为可回答问题的正确答案。这突显了当LLM的评估未与人工评估对比时，可能会出现的评估偏差。

其次，GPT-4和人工评估者在评估整体表现时显示出一致性。这是通过比较前两行的数字总和与后两行的总和来计算的。因此，将两个领域特定的LLM与优越模型进行比较可以成为一种有效的DIY初步模型评估方法。

这就是结果！你已经掌握了迷人的技巧，你的领域特定LLM管道已全面运行。

**[伊万·亚姆什奇科夫](https://www.linkedin.com/in/kroniker/?originalSubdomain=de)** 是应用科学大学维尔茨堡-施韦因富特人工智能与机器人中心的语义数据处理与认知计算教授。他还领导Toloka AI的数据倡导者团队。他的研究兴趣包括计算创造力、语义数据处理和生成模型。

* * *

## 我们的前三课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的IT需求

* * *

### 更多相关话题

+   [Web LLM：将LLM聊天机器人带到浏览器](https://www.kdnuggets.com/2023/05/webllm-bring-llm-chatbots-browser.html)

+   [使用LM Studio本地运行LLM](https://www.kdnuggets.com/run-an-llm-locally-with-lm-studio)

+   [聊天机器人竞技场：LLM基准测试平台](https://www.kdnuggets.com/2023/05/chatbot-arena-llm-benchmark-platform.html)

+   [Falcon LLM：开源LLM的新霸主](https://www.kdnuggets.com/2023/06/falcon-llm-new-king-llms.html)

+   [认识Gorilla：UC Berkeley和微软的API增强型LLM…](https://www.kdnuggets.com/2023/06/meet-gorilla-uc-berkeley-microsoft-apiaugmented-llm-outperforms-gpt4-chatgpt-claude.html)

+   [免费全栈LLM训练营](https://www.kdnuggets.com/2023/06/free-full-stack-llm-bootcamp.html)
