# 使用大语言模型在云中优化性能和成本的策略

> 原文：[`www.kdnuggets.com/strategies-for-optimizing-performance-and-costs-when-using-large-language-models-in-the-cloud`](https://www.kdnuggets.com/strategies-for-optimizing-performance-and-costs-when-using-large-language-models-in-the-cloud)

![使用大语言模型在云中优化性能和成本的策略](img/99ce9c90cb6e9401eac8cdaa858d85add.png)

图片来源：[pch.vector](https://www.freepik.com/free-vector/money-income-attraction_9176032.htm#query=cost&position=29&from_view=search&track=sph&uuid=61aa0541-882f-4e86-b49d-e7cc8e315f4b) 在 [Freepik](https://www.freepik.com/)

大语言模型（LLM）最近开始在业务中站稳脚跟，并将进一步扩展。随着公司开始理解实施 LLM 的好处，数据团队将调整模型以满足业务需求。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织进行 IT 工作

* * *

对于业务来说，最佳路径是利用云平台来扩展业务所需的任何 LLM 要求。然而，许多障碍可能会阻碍 LLM 在云中的性能并增加使用成本。这正是我们希望在业务中避免的。

这就是为什么本文将尝试概述一种可以用来优化云中 LLM 性能的策略，同时考虑到成本。策略是什么？让我们深入探讨一下。

# 1\. 制定清晰的预算计划

我们必须在实施任何优化性能和成本的策略之前了解我们的财务状况。我们愿意在 LLM 上投入的预算将成为我们的限制。更高的预算可能带来更显著的性能结果，但如果不支持业务，可能并不理想。

预算计划需要与各种利益相关者进行广泛讨论，以避免浪费。确定您的业务希望解决的关键问题，并评估 LLM 是否值得投资。

这一策略同样适用于任何个人或独立业务。为 LLM 设定一个您愿意支出的预算，将有助于您长期解决财务问题。

# 2\. 确定合适的模型规模和硬件

随着研究的进展，我们可以选择多种 LLM 来解决问题。使用较小参数的模型，优化速度更快，但可能没有最佳的业务问题解决能力。而更大的模型具有更丰富的知识库和创造力，但计算成本更高。

在决定模型时，我们需要考虑 LLM 大小的性能和成本之间的权衡。我们是需要更大参数的模型，这样性能更好但成本更高，还是相反？这是我们需要考虑的问题。因此，尽量评估你的需求。

此外，云硬件也可能影响性能。更好的 GPU 内存可能会有更快的响应时间，支持更复杂的模型，并减少延迟。然而，更高的内存意味着更高的成本。

# 3\. 选择合适的推理选项

根据云平台的不同，推理选项也会有所不同。根据你的应用程序负载需求，你选择的选项可能也会有所不同。然而，推理还可能影响成本使用，因为每种选项的资源数量不同。

如果我们以[Amazon SageMaker 推理选项](https://docs.aws.amazon.com/sagemaker/latest/dg/deploy-model.html)为例，你的推理选项包括：

1.  **实时推理**。这种推理在输入到达时即时处理响应。它通常用于实时应用，如聊天机器人、翻译器等。由于实时推理总是需要低延迟，因此即使在需求低谷期，应用程序也需要高计算资源。这意味着如果需求不足，实时推理的 LLM 可能导致更高的成本而没有任何好处。

1.  **无服务器推理**。这种推理是指云平台根据需要动态扩展和分配资源。性能可能会受到影响，因为每次请求时资源启动都会有轻微的延迟。但这是最具成本效益的，因为我们只为所使用的资源付费。

1.  **批处理转换**。这种推理是指我们批量处理请求。这意味着推理仅适用于离线处理，因为我们不会立即处理请求。对于任何需要即时处理的应用程序可能不适用，因为延迟总是存在，但成本不高。

1.  **异步推理**。这种推理适用于后台任务，因为它在后台运行推理任务，结果稍后检索。从性能角度来看，它适合需要较长处理时间的模型，因为它可以同时处理各种任务。从成本角度来看，由于更好的资源分配，它也可能是有效的。

尝试评估你的应用程序需求，以便选择最有效的推理选项。

# 4\. 构建有效的提示

LLM 是一种具有特定情况的模型，因为令牌数量会影响我们需要支付的费用。这就是为什么我们需要有效构建提示，以使用最少的令牌（无论是输入还是输出）同时保持输出质量。

尝试构建一个指定特定段落输出的提示，或者使用诸如“总结”、“简洁”等结尾段落。还要精确构造输入提示，以生成你所需的输出。不要让 LLM 模型生成超出你需求的内容。

# 5\. 缓存响应

会有信息被反复询问，并且每次的响应都是一样的。为了减少查询次数，我们可以将所有典型信息缓存到数据库中，并在需要时调用它们。

通常，数据存储在如 Pinecone 或 Weaviate 这样的向量数据库中，但云平台也应有它们自己的向量数据库。我们想要缓存的响应会转换为向量形式，并为未来的查询存储。

在有效缓存响应时面临一些挑战，因为我们需要管理缓存响应不足以回答输入查询的策略。此外，一些缓存可能类似，这可能导致错误的响应。良好管理响应并拥有足够的数据库可以帮助降低成本。

# 结论

如果我们没有正确处理 LLM，部署的 LLM 可能会花费过多并表现不准确。这就是为什么这里有一些策略可以用来优化你在云中的 LLM 的性能和成本：

1.  制定明确的预算计划，

1.  决定合适的模型规模和硬件，

1.  选择合适的推理选项，

1.  构建有效的提示，

1.  缓存响应。

**[](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**** 是一位数据科学助理经理和数据撰稿人。在全职工作于 Allianz Indonesia 的同时，他喜欢通过社交媒体和写作媒体分享 Python 和数据技巧。Cornellius 涉及各种 AI 和机器学习主题的写作。

### 更多相关主题

+   [优化大型语言模型的最佳策略](https://www.kdnuggets.com/the-best-strategies-for-fine-tuning-large-language-models)

+   [如何让大型语言模型与你的软件和谐相处…](https://www.kdnuggets.com/how-to-make-large-language-models-play-nice-with-your-software-using-langchain)

+   [等级系统如何帮助预测 AI 成本](https://www.kdnuggets.com/2022/03/level-system-help-forecast-ai-costs.html)

+   [优化你的 LLM 性能和可扩展性](https://www.kdnuggets.com/optimizing-your-llm-for-performance-and-scalability)

+   [大型语言模型是什么，如何工作？](https://www.kdnuggets.com/2023/05/large-language-models-work.html)

+   [优化 Python 代码性能：深入探讨 Python 分析器](https://www.kdnuggets.com/2023/02/optimizing-python-code-performance-deep-dive-python-profilers.html)
