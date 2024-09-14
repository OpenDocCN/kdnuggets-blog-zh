# 系好安全带：Falcon 180B 到来了！

> 原文：[https://www.kdnuggets.com/fasten-your-seatbelt-falcon-180b-is-here](https://www.kdnuggets.com/fasten-your-seatbelt-falcon-180b-is-here)

![系好安全带：Falcon 180B 到来了！](../Images/e65a7e76e2f4c174d5c28fe9e2c34569.png)

图片来自作者

几个月前，我们了解到 [Falcon LLM](/2023/06/falcon-llm-new-king-llms.html)，这是由 [技术创新研究所](https://www.tii.ae/)（TII）创立的，该公司是阿布扎比政府先进技术研究委员会的一部分。几个月后，他们变得更大、更强——字面上的意义，更大。

# Falcon 180B: 你需要了解的一切

Falcon 180B 是最大且公开可用的语言模型，拥有 1800 亿个参数。没错，你没看错——1800 亿。它在 TII 的 RefinedWeb 数据集上进行了 3.5 万亿标记的训练。这代表了对一个开放模型进行的最长单周期预训练。

但我们这里关注的不仅仅是模型的规模，还包括其背后的力量和潜力。在大语言模型（LLMs）领域，Falcon 180B 正在创造新的标准。

目前可用的模型：

+   [Falcon 180B](https://huggingface.co/tiiuae/falcon-180B)

+   [Falcon 180B Chat](https://huggingface.co/tiiuae/falcon-180B-chat)

Falcon-180B 基础模型是一个因果解码器模型。我建议使用该模型对自己的数据进行进一步的细化训练。

Falcon-180B-Chat 模型与基础版本有相似之处，但通过使用 Ultrachat、Platypus 和 Airoboros 指令（对话）数据集进行细化调整，进一步提升了能力。

## 训练

Falcon 180B 在其前身 Falcon 40B 的基础上进行了扩展，新增了多查询注意力机制以提高可扩展性。该模型在 Amazon SageMaker 上使用了 4096 个 GPU，并在 3.5 万亿个标记上进行了训练。这大约需要 7,000,000 GPU 小时。这意味着 Falcon 180B 比如 Llama 2 等大语言模型快 2.5 倍，并且训练所用计算资源是其 4 倍。

哇，真是太多了。

## 数据

Falcon 180B 使用的数据集主要来自 RefinedWeb，占比达 85%，此外还在一系列精心策划的数据（如技术论文、对话和一些代码元素）上进行了训练。

## 基准测试

你们都想知道的部分——Falcon 180B 在竞争对手中表现如何？

到目前为止（2023年9月），Falcon 180B 是公开发布的最佳大语言模型。它在 [MMLU](https://paperswithcode.com/dataset/mmlu) 上的表现超过了 Llama 2 70B 和 OpenAI 的 GPT-3.5。它通常介于 GPT 3.5 和 GPT 4 之间。

![系好安全带：Falcon 180B 到来了！](../Images/2b578ab8e51a55f1336154eed1d9b9a5.png)

图片来自 [HuggingFace Falcon 180B](https://huggingface.co/blog/falcon-180b)

Falcon 180B 在 Hugging Face 排行榜上排名 68.74，使其成为最高分的公开发布的预训练大语言模型，超过了 Meta 的 LLaMA 2，后者的得分为 67.35。

# 如何使用 Falcon 180B？

对于开发者和自然语言处理（NLP）爱好者，Falcon 180B 在 Hugging Face 生态系统中可用，开始于 Transformers 版本 4.33。

然而，正如你可以想象的，由于模型的体积，你需要考虑硬件要求。为了更好地了解硬件要求，HuggingFace 进行了不同使用案例所需的模型运行测试，如下图所示：

![系好安全带：Falcon 180B 来了！](../Images/424f925c66da18a9be9a9153c7800098.png)

图片由 [HuggingFace Falcon 180B](https://huggingface.co/blog/falcon-180b) 提供

如果你想测试一下并尝试一下，可以通过点击这个链接尝试 Falcon 180B 的演示：[Falcon 180B 演示](https://huggingface.co/spaces/tiiuae/falcon-180b-demo)。

## Falcon 180B 与 ChatGPT

该模型有一些严苛的硬件要求，并不是所有人都能轻易获得。然而，根据其他人对 Falcon 180B 和 ChatGPT 的测试结果，ChatGPT 在回答相同问题时胜出。

在代码生成方面表现良好，但在文本提取和总结方面仍需提升。

# 总结一下

如果你有机会尝试过它，请告诉我们你与其他 LLMs 的比较结果。Falcon 180B 作为目前 Hugging Face 模型中心最大的公开模型，是否值得所有的宣传？

好吧，看来它确实如此，因为它已经在开放访问模型的排行榜上名列前茅，并且对像 PaLM-2 这样的模型构成了威胁。我们迟早会知道结果。

[](https://www.linkedin.com/in/nisha-arya-ahmed/)****[尼莎·阿雅](https://www.linkedin.com/in/nisha-arya-ahmed/)**** 是一位数据科学家、自由技术作家，以及 KDnuggets 的编辑和社区经理。她特别关注提供数据科学职业建议或教程及理论知识。尼莎涵盖了广泛的主题，并希望探索人工智能如何有助于人类寿命的延续。作为一个热衷学习者，尼莎寻求拓宽她的技术知识和写作技能，同时帮助指导他人。

### 更多相关主题

+   [Falcon LLM：开源 LLM 的新王者](https://www.kdnuggets.com/2023/06/falcon-llm-new-king-llms.html)

+   [想用你的数据技能解决全球问题？这里是…](https://www.kdnuggets.com/2022/04/jhu-want-data-skills-solve-global-problems.html)

+   [我每天使用 ChatGPT 5 个月。这里是一些隐藏的宝石…](https://www.kdnuggets.com/2023/07/used-chatgpt-every-day-5-months-hidden-gems-change-life.html)

+   [这里是我使用的 AI 工具及我的技能来赚取 $10,000 的方法…](https://www.kdnuggets.com/2023/07/ai-tools-along-skills-make-10000-monthly-bs.html)

+   [无法找到数据科学职位？这里是原因](https://www.kdnuggets.com/2022/01/unable-land-data-science-job.html)

+   [数据科学被高估了，原因如下](https://www.kdnuggets.com/2022/06/data-science-overrated.html)
