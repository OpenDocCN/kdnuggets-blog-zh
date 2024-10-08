# 介绍 OpenLLM：LLM 的开源库

> 原文：[`www.kdnuggets.com/2023/07/introducing-openllm-open-source-library-llms.html`](https://www.kdnuggets.com/2023/07/introducing-openllm-open-source-library-llms.html)

![介绍 OpenLLM：LLM 的开源库](img/1efb563e445901db6e7d4ada83d0ed00.png)

图片来源于作者

此时，我们都在思考同一个问题。LLM 的世界真的在主导吗？一些人可能预计热度会平稳，但它仍在持续上升。更多资源投入到 LLM 中，因为它显示出巨大的需求。

LLM 的性能不仅成功，而且它们在适应各种 NLP 任务（如翻译和情感分析）方面的多功能性也非常强。微调预训练的 LLM 使得针对特定任务变得更加容易，减少了从头构建模型的计算成本。LLM 已迅速被应用于各种现实世界应用，推动了研究和开发的数量。

开源模型也是 LLM 的一个重要优势，因为开源模型的可用性使研究人员和组织能够持续改进现有模型，并探索它们如何安全地融入社会。

# 什么是 OpenLLM？

[OpenLLM](https://github.com/bentoml/OpenLLM) 是一个用于生产环境中操作 LLM 的开放平台。使用 OpenLLM，您可以对任何开源 LLM 进行推理、微调、部署，并轻松构建强大的 AI 应用程序。

OpenLLM 包含最先进的 LLM，如 StableLM、Dolly、ChatGLM、StarCoder 等，所有这些都由内置支持。您还可以自由构建自己的 AI 应用程序，因为 OpenLLM 不仅仅是一个独立产品，还支持 LangChain、BentoML 和 Hugging Face。

这些功能，还开源？听起来有点疯狂，对吧？

更棒的是，它易于安装和使用。

## 如何使用 OpenLLM？

要使用 LLM，您需要系统中至少安装 Python 3.8 以及 pip。为了防止包冲突，建议使用虚拟环境。

1.  一旦准备好这些，您可以通过使用以下命令轻松安装 OpenLLM：

```py
pip install open-llm
```

1.  为确保正确安装，可以运行以下命令：

```py
$ openllm -h

Usage: openllm [OPTIONS] COMMAND [ARGS]...

   ██████╗ ██████╗ ███████╗███╗   ██╗██╗     ██╗     ███╗   ███╗
  ██╔═══██╗██╔══██╗██╔════╝████╗  ██║██║     ██║     ████╗ ████║
  ██║   ██║██████╔╝█████╗  ██╔██╗ ██║██║     ██║     ██╔████╔██║
  ██║   ██║██╔═══╝ ██╔══╝  ██║╚██╗██║██║     ██║     ██║╚██╔╝██║
  ╚██████╔╝██║     ███████╗██║ ╚████║███████╗███████╗██║ ╚═╝ ██║
   ╚═════╝ ╚═╝     ╚══════╝╚═╝  ╚═══╝╚══════╝╚══════╝╚═╝     ╚═╝

  An open platform for operating large language models in production.
  Fine-tune, serve, deploy, and monitor any LLMs with ease.
```

1.  为了启动一个 LLM 服务器，请使用以下命令，并包括您选择的模型：

```py
openllm start
```

例如，如果您想启动一个 [OPT](https://huggingface.co/docs/transformers/model_doc/opt) 服务器，请执行以下操作：

```py
openllm start opt
```

## 支持的模型

OpenLLM 支持 10 种模型。您还可以在下方找到安装命令：

1.  [chatglm](https://github.com/THUDM/ChatGLM-6B)

```py
pip install "openllm[chatglm]"
```

该模型需要 GPU。

1.  [Dolly-v2](https://github.com/databrickslabs/dolly)

```py
pip install openllm
```

该模型可以在 CPU 和 GPU 上使用。

1.  [falcon](https://falconllm.tii.ae/)

```py
pip install "openllm[falcon]"
```

该模型需要 GPU。

1.  [flan-t5](https://huggingface.co/docs/transformers/model_doc/flan-t5)

```py
pip install "openllm[flan-t5]"
```

该模型可以在 CPU 和 GPU 上使用。

1.  [gpt-neox](https://github.com/EleutherAI/gpt-neox)

```py
pip install openllm
```

这个模型需要一个 GPU。

1.  [mpt](https://huggingface.co/mosaicml)

```py
pip install "openllm[mpt]"
```

这个模型可以在 CPU 和 GPU 上使用。

1.  [opt](https://huggingface.co/docs/transformers/model_doc/opt)

```py
pip install "openllm[opt]"
```

这个模型可以在 CPU 和 GPU 上使用。

1.  [stablelm](https://github.com/Stability-AI/StableLM)

```py
pip install openllm
```

这个模型可以在 CPU 和 GPU 上使用。

1.  [starcoder](https://github.com/bigcode-project/starcoder)

```py
pip install "openllm[starcoder]"
```

这个模型需要一个 GPU。

1.  [baichuan](https://github.com/baichuan-inc/Baichuan-7B)

```py
pip install "openllm[baichuan]"
```

这个模型需要一个 GPU。

要了解有关运行时实现、微调支持、集成新模型和生产部署的更多信息，请查看 [这里](https://github.com/bentoml/OpenLLM#runtime-implementations-experimental)，找到适合你需求的方案。

# 总结一下

如果你想使用 OpenLLM 或需要帮助，你可以加入他们的 [Discord](https://l.bentoml.com/join-openllm-discord) 和 [Slack 社区](https://l.bentoml.com/join-slack)。你也可以通过他们的 [开发者指南](https://github.com/bentoml/OpenLLM/blob/main/DEVELOPMENT.md) 为 OpenLLM 的代码库做贡献。

有没有人试过这个？如果试过，请在评论中告诉我们你的想法！

**[Nisha Arya](https://www.linkedin.com/in/nisha-arya-ahmed/)** 是一名数据科学家、自由技术写作者以及 KDnuggets 的社区经理。她特别感兴趣于提供数据科学职业建议或教程以及有关数据科学的理论知识。她还希望探索人工智能如何/能如何有利于人类寿命的不同方式。她是一位渴望学习者，寻求拓宽自己的技术知识和写作技能，同时帮助指导他人。

### 更多相关信息

+   [介绍 Objectiv：开源产品分析基础设施](https://www.kdnuggets.com/2022/06/objectiv-introducing-objectiv-opensource-product-analytics-infrastructure.html)

+   [介绍 MPT-7B：一个新的开源 LLM](https://www.kdnuggets.com/2023/05/introducing-mpt7b-new-opensource-llm.html)

+   [介绍 MetaGPT 的数据解释器：SOTA 开源 LLM 基于……](https://www.kdnuggets.com/metagpt-data-interpreter-open-source-llm-based-data-solutions)

+   [介绍自然语言处理测试库](https://www.kdnuggets.com/2023/04/introducing-testing-library-natural-language-processing.html)

+   [RedPajama 项目：一个开源倡议，旨在普及 LLMs](https://www.kdnuggets.com/2023/06/redpajama-project-opensource-initiative-democratizing-llms.html)

+   [Falcon LLM：开源 LLM 的新王者](https://www.kdnuggets.com/2023/06/falcon-llm-new-king-llms.html)
