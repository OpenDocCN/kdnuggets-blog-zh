# PEFT 概述：最先进的参数高效微调

> 原文：[`www.kdnuggets.com/overview-of-peft-stateoftheart-parameterefficient-finetuning`](https://www.kdnuggets.com/overview-of-peft-stateoftheart-parameterefficient-finetuning)

![PEFT 概述：最先进的参数高效微调](img/aff0878bf10b35d37fe07ff658443928.png)

作者提供的图片

# 什么是 PEFT

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

随着 GPT-3.5、LLaMA2 和 PaLM2 等大型语言模型（LLMs）规模的不断扩大，在下游自然语言处理（NLP）任务中对它们进行微调变得越来越计算密集且内存占用巨大。

参数高效微调（PEFT）方法通过仅微调少量额外参数，同时冻结大部分预训练模型，来解决这些问题。这可以防止大型模型出现灾难性遗忘，并且使得在有限计算资源下也能进行微调。

PEFT 已被证明在图像分类和文本生成等任务中有效，同时只使用了参数的一小部分。小的调整权重可以简单地添加到原始预训练权重中。

你甚至可以在 Google Colab 的免费版本上使用 4 位量化和 PEFT 技术 QLoRA 来微调 LLMs。

PEFT 的模块化特性还允许通过添加少量任务特定的权重来将相同的预训练模型适应于多个任务，从而避免存储完整的副本。

[PEFT](https://github.com/huggingface/peft) 库集成了流行的 PEFT 技术，如 LoRA、前缀调整、AdaLoRA、提示调整、多任务提示调整和 LoHa，支持 Transformers 和 Accelerate。这提供了对前沿大型语言模型的简便访问，同时实现高效和可扩展的微调。

# 什么是 LoRA

在本教程中，我们将使用最流行的参数高效微调（PEFT）技术，称为 [LoRA](https://arxiv.org/abs/2106.09685)（大型语言模型的低秩适应）。LoRA 是一种显著加快大型语言模型微调过程，同时消耗更少内存的技术。

LoRA 的关键思想是通过低秩分解使用两个较小的矩阵表示权重更新。这些矩阵可以训练以适应新数据，同时最小化总体修改数量。原始权重矩阵保持不变，不会再进行任何调整。最终结果是通过结合原始权重和适应后的权重得到的。

使用 LoRA 有几个优势。首先，它通过减少可训练参数的数量大大提高了微调的效率。此外，LoRA 与各种其他高效参数方法兼容，并可以与它们结合使用。使用 LoRA 微调的模型表现出与完全微调模型相当的性能。重要的是，LoRA 不会引入额外的推理延迟，因为适配器权重可以与基础模型无缝合并。

# 使用案例

PEFT 有许多使用案例，从语言模型到图像分类器。你可以在官方 [文档](https://huggingface.co/docs/peft/task_guides/image_classification_lora) 中查看所有使用案例教程。

1.  [StackLLaMA: 训练 LLaMA 的实用指南](https://huggingface.co/blog/stackllama)

1.  [Finetune-opt-bnb-peft](https://colab.research.google.com/drive/1jCkpikz0J2o20FBQmYmAGdiKmJGOMo-o?usp=sharing)

1.  [使用 LoRA 和 Hugging Face 高效训练 flan-t5-xxl](https://www.philschmid.de/fine-tune-flan-t5-peft)

1.  [使用 LoRA 进行 DreamBooth 微调](https://huggingface.co/docs/peft/task_guides/dreambooth_lora)

1.  [使用 LoRA 的图像分类](https://huggingface.co/docs/peft/task_guides/image_classification_lora)

# 使用 PEFT 训练 LLMs

在本节中，我们将学习如何使用 `bitsandbytes` 和 `peft` 库加载和包装我们的 transformer 模型。我们还将涵盖加载已保存的微调 QLoRA 模型并使用它进行推理。

## 入门

首先，我们将安装所有必要的库。

```py
%%capture
%pip install accelerate peft transformers datasets bitsandbytes
```

然后，我们将导入必要的模块，并命名基础模型（[Llama-2-7b-chat-hf](https://huggingface.co/NousResearch/Llama-2-7b-chat-hf)），以使用 [mlabonne/guanaco-llama2-1k](https://huggingface.co/datasets/mlabonne/guanaco-llama2-1k) 数据集对其进行微调。

```py
from transformers import AutoModelForCausalLM, AutoTokenizer, BitsAndBytesConfig
from peft import get_peft_model, LoraConfig
import torch

model_name = "NousResearch/Llama-2-7b-chat-hf"
dataset_name = "mlabonne/guanaco-llama2-1k"
```

## PEFT 配置

创建 PEFT 配置，我们将用它来包装或训练我们的模型。

```py
peft_config = LoraConfig(
    lora_alpha=16,
    lora_dropout=0.1,
    r=64,
    bias="none",
    task_type="CAUSAL_LM",
)
```

## 4 位量化

在消费者或 Colab GPU 上加载 LLMs 面临重大挑战。然而，我们可以通过使用 BitsAndBytes 实施 4 位量化技术和 NF4 类型配置来克服这个问题。通过这种方法，我们可以有效地加载模型，从而节省内存并防止机器崩溃。

```py
compute_dtype = getattr(torch, "float16")

bnb_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_quant_type="nf4",
    bnb_4bit_compute_dtype=compute_dtype,
    bnb_4bit_use_double_quant=False,
)
```

## 包装基础 Transformers 模型

为了使我们的模型在参数上更高效，我们将使用 `get_peft_model` 来包装基础 transformer 模型。

```py
model = AutoModelForCausalLM.from_pretrained(
    model_name,
    quantization_config=bnb_config,
    device_map="auto"
)
model = get_peft_model(model, peft_config)
model.print_trainable_parameters()
```

我们的可训练参数少于基础模型的参数，这使我们能够使用更少的内存并更快地微调模型。

```py
trainable params: 33,554,432 || all params: 6,771,970,048 || trainable%: 0.49548996469513035
```

下一步是训练模型。你可以按照 [4 位量化和 QLoRA](https://huggingface.co/blog/4bit-transformers-bitsandbytes) 指南进行操作。

## 保存模型

训练后，你可以选择将模型适配器保存到本地。

```py
model.save_pretrained("llama-2-7b-chat-guanaco")
```

或者，将其推送到 Hugging Face Hub。

```py
!huggingface-cli login --token $secret_value_0
```

```py
model.push_to_hub("llama-2-7b-chat-guanaco")
```

如我们所见，模型适配器仅为 134MB，而基础的 LLaMA 2 7B 模型约为 13GB。

![PEFT 概述：最先进的参数高效微调](img/980ded1e7c785ce7884c611929a51e7e.png)

## 加载模型

要运行模型推理，我们首先需要使用 4 位精度量化加载模型，然后将训练好的 PEFT 权重与基础（LlaMA 2）模型合并。

```py
from transformers import AutoModelForCausalLM
from peft import PeftModel, PeftConfig
import torch

peft_model = "kingabzpro/llama-2-7b-chat-guanaco"
base_model = AutoModelForCausalLM.from_pretrained(
    model_name,
    quantization_config=bnb_config,
    device_map="auto"
)

model = PeftModel.from_pretrained(base_model, peft_model)
tokenizer = AutoTokenizer.from_pretrained(model_name)

model = model.to("cuda")
model.eval()
```

## 推理

在进行推理时，我们需要以 guanaco-llama2-1k 数据集风格编写提示(*“<s>[INST] {prompt} [/INST]”*)。否则你将获得不同语言的响应。

```py
prompt = "What is Hacktoberfest?"
inputs = tokenizer(f"<s>[INST] {prompt} [/INST]", return_tensors="pt")
with torch.no_grad():
    outputs = model.generate(
        input_ids=inputs["input_ids"].to("cuda"), max_new_tokens=100
    )
    print(
        tokenizer.batch_decode(
            outputs.detach().cpu().numpy(), skip_special_tokens=True
        )[0]
    ) 
```

输出似乎完美。

```py
[INST] What is Hacktoberfest? [/INST] Hacktoberfest is an open-source software development event that takes place in October. It was created by the non-profit organization Open Source Software Institute (OSSI) in 2017\. The event aims to encourage people to contribute to open-source projects, with the goal of increasing the number of contributors and improving the quality of open-source software.

During Hacktoberfest, participants are encouraged to contribute to open-source 
```

> **注意：** 如果你在 Colab 中加载模型时遇到困难，可以查看我的笔记本：[PEFT 概述](https://colab.research.google.com/drive/1qh6pAJczmC1GOw0ES-zVjPmFzspikq0m?usp=sharing)。

# 结论

诸如 LoRA 这样的参数高效微调技术使得使用少量参数高效微调大型语言模型成为可能。这避免了昂贵的全量微调，并能在有限的计算资源下进行训练。PEFT 的模块化特性允许将模型适配于多个任务。像 4 位精度这样的量化方法可以进一步减少内存使用。总体而言，PEFT 将大型语言模型的能力拓展到更广泛的受众。

[](https://www.polywork.com/kingabzpro)****[Abid Ali Awan](https://www.polywork.com/kingabzpro)**** ([@1abidaliawan](https://www.linkedin.com/in/1abidaliawan)) 是一位认证的数据科学专业人士，他喜欢构建机器学习模型。目前，他专注于内容创作和撰写关于机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是利用图神经网络为面临心理健康问题的学生开发 AI 产品。

### 更多相关话题

+   [最先进深度学习的可解释预测和现状预测](https://www.kdnuggets.com/2021/12/sota-explainable-forecasting-and-nowcasting.html)

+   [RAG 与 Finetuning：哪种是提升 LLM 应用的最佳工具？](https://www.kdnuggets.com/rag-vs-finetuning-which-is-the-best-tool-to-boost-your-llm-application)

+   [数据科学职业的当前状态](https://www.kdnuggets.com/2022/10/current-state-data-science-careers.html)

+   [KDnuggets 新闻，11 月 2 日：数据科学的当前状态…](https://www.kdnuggets.com/2022/n43.html)

+   [特征选择：科学与艺术的交汇点](https://www.kdnuggets.com/2021/12/feature-selection-science-meets-art.html)

+   [提示工程的艺术：解码 ChatGPT](https://www.kdnuggets.com/2023/06/art-prompt-engineering-decoding-chatgpt.html)
