# 在Google Colab上免费微调LLAMAv2与QLora

> 原文：[https://www.kdnuggets.com/fine-tuning-llamav2-with-qlora-on-google-colab-for-free](https://www.kdnuggets.com/fine-tuning-llamav2-with-qlora-on-google-colab-for-free)

![在Google Colab上免费微调LLAMAv2与QLora](../Images/c4099ca8d941760761ce95a69a350c6b.png)

使用ideogram.ai生成，提示为：“一张LLAMA的照片，横幅上写着‘QLora’，3D渲染，野生动物摄影”

直到最近，在Google Colab上使用单个GPU免费微调一个7B模型曾是一个梦想。2023年5月23日，Tim Dettmers及其团队提交了一篇关于微调量化大语言模型的革命性论文[1]。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织IT需求

* * *

量化模型是将其权重的数据类型降至比训练时使用的数据类型更低的模型。例如，如果你在32位浮点数上训练一个模型，然后将这些权重转换为较低的数据类型，如16/8/4位浮点数，而对模型性能的影响最小或没有影响。

![在Google Colab上免费微调LLAMAv2与QLora](../Images/0b322b679c4a64d081cc5545a40f8991.png)

来源 [2]

我们这里不会多谈量化的理论，你可以参考Hugging-Face的优秀博客文章[2][3]以及Tim Dettmers本人的优秀YouTube视频[4]以了解基础理论。

简而言之，可以说QLora的意思是：

> 使用低秩适配矩阵（LoRA）微调量化大语言模型[5]

让我们直接进入代码：

# 数据准备

重要的是要理解，大语言模型旨在接受指令，这一点在2021年ACL论文[6]中首次介绍。其核心思想很简单，我们给语言模型一个指令，它会遵循指令并执行该任务。因此，我们要微调的模型所需的数据集应为指令格式，如果不是，我们可以转换它。

一种常见的格式是指令格式。我们将使用Alpaca Prompt模板[7]。

```py
Below is an instruction that describes a task, paired with an input that provides further context. Write a response that appropriately completes the request.

### Instruction:
{instruction}

### Input:
{input}

### Response:
{response}
```

我们将使用[SNLI数据集](https://nlp.stanford.edu/projects/snli/)，这是一个包含2个句子及其之间关系的数据集，关系可能是矛盾、蕴含或中立。我们将利用它通过LLAMAv2生成句子的矛盾。我们可以简单地使用pandas加载此数据集。

```py
import pandas as pd

df = pd.read_csv('snli_1.0_train_matched.csv')
df['gold_label'].value_counts().plot(kind='barh')
```

![在 Google Colab 上免费微调 LLAMAv2 与 QLora](../Images/d2eac6c0d0e494320d1cdd66f4ce7704.png)

标签分布

我们可以在这里看到一些随机的矛盾示例。

```py
df[df['gold_label'] == 'contradiction'].sample(10)[['sentence1', 'sentence2']]
```

![在 Google Colab 上免费微调 LLAMAv2 与 QLora](../Images/6effab1043714f8a9c0e22f8cdfe4170.png)

来自 SNLI 的矛盾示例

现在我们可以创建一个小函数，只处理矛盾的句子并将数据集转换为 instruct 格式。

```py
def convert_to_format(row):
    sentence1 = row['sentence1']
    sentence2 = row['sentence2']ccccc
    prompt = """Below is an instruction that describes a task paired with input that provides further context. Write a response that appropriately completes the request."""
    instruction = """Given the following sentence, your job is to generate the negation for it in the json format"""
    input = str(sentence1)
    response = f"""```json

{{'orignal_sentence': '{sentence1}', 'generated_negation': '{sentence2}'}}

```py
"""
    if len(input.strip()) == 0:  #  prompt + 2 new lines + ###instruction + new line + input + new line + ###response
        text = prompt + "\n\n### Instruction:\n" + instruction + "\n### Response:\n" + response
    else:
        text = prompt + "\n\n### Instruction:\n" + instruction + "\n### Input:\n" + input + "\n" + "\n### Response:\n" + response

    # we need 4 columns for auto train, instruction, input, output, text
    return pd.Series([instruction, input, response, text])

new_df = df[df['gold_label'] == 'contradiction'][['sentence1', 'sentence2']].apply(convert_to_format, axis=1)
new_df.columns = ['instruction', 'input', 'output', 'text']

new_df.to_csv('snli_instruct.csv', index=False)
```

这是一个示例数据点：

```py
"Below is an instruction that describes a task paired with input that provides further context. Write a response that appropriately completes the request.

### Instruction:
Given the following sentence, your job is to generate the negation for it in the json format
### Input:
A couple playing with a little boy on the beach.

### Response:
```json

{'orignal_sentence': '一对夫妇在海滩上和一个小男孩玩耍。', 'generated_negation': '一对夫妇在海滩上看着一个小女孩自己玩耍。'}

```py
```

现在我们有了正确格式的数据集，开始微调吧。在开始之前，让我们安装必要的软件包。我们将使用 accelerate、peft（参数高效微调），结合 Hugging Face 的 Bits 和 bytes 以及 transformers。

```py
!pip install -q accelerate==0.21.0 peft==0.4.0 bitsandbytes==0.40.2 transformers==4.31.0 trl==0.4.7
```

```py
import os
import torch
from datasets import load_dataset
from transformers import (
    AutoModelForCausalLM,
    AutoTokenizer,
    BitsAndBytesConfig,
    HfArgumentParser,
    TrainingArguments,
    pipeline,
    logging,
)
from peft import LoraConfig, PeftModel
from trl import SFTTrainer
```

你可以将格式化的数据集上传到云端并在 Colab 中加载它。

```py
from google.colab import drive
import pandas as pd

drive.mount('/content/drive')

df = pd.read_csv('/content/drive/MyDrive/snli_instruct.csv')
```

你可以使用 `from_pandas` 方法轻松地将其转换为 Hugging Face 数据集格式，这将有助于训练模型。

```py
from datasets import Dataset

dataset = Dataset.from_pandas(df)
```

我们将使用由 [abhishek/llama-2–7b-hf-small-shards](https://huggingface.co/abhishek/llama-2-7b-hf-small-shards) 提供的已量化 LLamav2 模型。在这里定义一些超参数和变量：

```py
# The model that you want to train from the Hugging Face hub
model_name = "abhishek/llama-2-7b-hf-small-shards"

# Fine-tuned model name
new_model = "llama-2-contradictor"

################################################################################
# QLoRA parameters
################################################################################

# LoRA attention dimension
lora_r = 64

# Alpha parameter for LoRA scaling
lora_alpha = 16

# Dropout probability for LoRA layers
lora_dropout = 0.1

################################################################################
# bitsandbytes parameters
################################################################################

# Activate 4-bit precision base model loading
use_4bit = True

# Compute dtype for 4-bit base models
bnb_4bit_compute_dtype = "float16"

# Quantization type (fp4 or nf4)
bnb_4bit_quant_type = "nf4"

# Activate nested quantization for 4-bit base models (double quantization)
use_nested_quant = False

################################################################################
# TrainingArguments parameters
################################################################################

# Output directory where the model predictions and checkpoints will be stored
output_dir = "./results"

# Number of training epochs
num_train_epochs = 1

# Enable fp16/bf16 training (set bf16 to True with an A100)
fp16 = False
bf16 = False

# Batch size per GPU for training
per_device_train_batch_size = 4

# Batch size per GPU for evaluation
per_device_eval_batch_size = 4

# Number of update steps to accumulate the gradients for
gradient_accumulation_steps = 1

# Enable gradient checkpointing
gradient_checkpointing = True

# Maximum gradient normal (gradient clipping)
max_grad_norm = 0.3

# Initial learning rate (AdamW optimizer)
learning_rate = 1e-5

# Weight decay to apply to all layers except bias/LayerNorm weights
weight_decay = 0.001

# Optimizer to use
optim = "paged_adamw_32bit"

# Learning rate schedule
lr_scheduler_type = "cosine"

# Number of training steps (overrides num_train_epochs)
max_steps = -1

# Ratio of steps for a linear warmup (from 0 to learning rate)
warmup_ratio = 0.03

# Group sequences into batches with same length
# Saves memory and speeds up training considerably
group_by_length = True

# Save checkpoint every X updates steps
save_steps = 0

# Log every X updates steps
logging_steps = 100

################################################################################
# SFT parameters
################################################################################

# Maximum sequence length to use
max_seq_length = None

# Pack multiple short examples in the same input sequence to increase efficiency
packing = False

# Load the entire model on the GPU 0
device_map = {"": 0}
```

其中大多数是相当直观的超参数，具有这些默认值。你可以随时参考文档获取更多详细信息。

我们现在可以简单地使用 BitsAndBytesConfig 类来创建 4 位微调的配置。

```py
compute_dtype = getattr(torch, bnb_4bit_compute_dtype)

bnb_config = BitsAndBytesConfig(
    load_in_4bit=use_4bit,
    bnb_4bit_quant_type=bnb_4bit_quant_type,
    bnb_4bit_compute_dtype=compute_dtype,
    bnb_4bit_use_double_quant=use_nested_quant,
)
```

现在我们可以加载带有 4 位 BitsAndBytesConfig 和 tokenizer 的基础模型进行微调。

```py
tokenizer = AutoTokenizer.from_pretrained(model_name, trust_remote_code=True)
tokenizer.pad_token = tokenizer.eos_token
tokenizer.padding_side = "right"

model = AutoModelForCausalLM.from_pretrained(
    model_name,
    quantization_config=bnb_config,
    device_map=device_map
)
model.config.use_cache = False
model.config.pretraining_tp = 1
```

我们现在可以创建 LoRA 配置并设置训练参数。

```py
# Load LoRA configuration
peft_config = LoraConfig(
    lora_alpha=lora_alpha,
    lora_dropout=lora_dropout,
    r=lora_r,
    bias="none",
    task_type="CAUSAL_LM",
)

# Set training parameters
training_arguments = TrainingArguments(
    output_dir=output_dir,
    num_train_epochs=num_train_epochs,
    per_device_train_batch_size=per_device_train_batch_size,
    gradient_accumulation_steps=gradient_accumulation_steps,
    optim=optim,
    save_steps=save_steps,
    logging_steps=logging_steps,
    learning_rate=learning_rate,
    weight_decay=weight_decay,
    fp16=fp16,
    bf16=bf16,
    max_grad_norm=max_grad_norm,
    max_steps=max_steps,
    warmup_ratio=warmup_ratio,
    group_by_length=group_by_length,
    lr_scheduler_type=lr_scheduler_type,
    report_to="tensorboard"
)
```

现在我们可以简单地使用由 HuggingFace 提供的 trl 中的 SFTTrainer 开始训练。

```py
# Set supervised fine-tuning parameters
trainer = SFTTrainer(
    model=model,
    train_dataset=dataset,
    peft_config=peft_config,
    dataset_text_field="text",  # this is the text column in dataset 
    max_seq_length=max_seq_length,
    tokenizer=tokenizer,
    args=training_arguments,
    packing=packing,
)

# Train model
trainer.train()

# Save trained model
trainer.model.save_pretrained(new_model)
```

这将开始训练你上述设置的周期数。一旦模型训练完成，确保将其保存在云端，以便你可以再次加载它（因为你需要在 Colab 中重启会话）。你可以通过 zip 和 mv 命令将模型存储在云端。

```py
!zip -r llama-contradictor.zip results llama-contradictor
!mv llama-contradictor.zip /content/drive/MyDrive
```

现在当你重启 Colab 会话时，可以将其移回到你的会话中。

```py
!unzip /content/drive/MyDrive/llama-contradictor.zip -d .
```

你需要重新加载基础模型，并将其与微调后的 LoRA 矩阵合并。这可以使用 `merge_and_unload()` 函数来完成。

```py
tokenizer = AutoTokenizer.from_pretrained(model_name, trust_remote_code=True)
tokenizer.pad_token = tokenizer.eos_token
tokenizer.padding_side = "right"

base_model = AutoModelForCausalLM.from_pretrained(
    "abhishek/llama-2-7b-hf-small-shards",
    low_cpu_mem_usage=True,
    return_dict=True,
    torch_dtype=torch.float16,
    device_map={"": 0},
)

model = PeftModel.from_pretrained(base_model, '/content/llama-contradictor')
model = model.merge_and_unload()
pipe = pipeline(task="text-generation", model=model, tokenizer=tokenizer, max_length=200)
```

# 推理

你可以通过将输入传递到我们上述定义的相同提示模板中来测试你的模型。

```py
prompt_template = """### Instruction:
Given the following sentence, your job is to generate the negation for it in the json format
### Input:
{}

### Response:
"""

sentence = "The weather forecast predicts a sunny day with a high temperature around 30 degrees Celsius, perfect for a day at the beach with friends and family."

input_sentence = prompt_template.format(sentence.strip())

result = pipe(input_sentence)
print(result)
```

# 输出

```py
### Instruction:
Given the following sentence, your job is to generate the negation for it in the json format
### Input:
The weather forecast predicts a sunny day with a high temperature around 30 degrees Celsius, perfect for a day at the beach with friends and family.

### Response:
```json

{

"sentence": "天气预报预测天气晴朗，气温高达 30 摄氏度，非常适合与朋友和家人一起去海滩度过一天。",

"negation": "天气预报预测降雨，气温低至 10 摄氏度，不适合与朋友和家人一起去海滩度过一天。"

}

```py
```

# 过滤有用的输出

由于令牌限制，模型可能在生成响应后仍会继续预测。在这种情况下，你需要添加一个后处理函数来过滤我们需要的JSON部分。可以使用简单的正则表达式来完成。

```py
import re
import json

def format_results(s):
  pattern = r'```json\n(.*?)\n```py'

  # Find all occurrences of JSON objects in the string
  json_matches = re.findall(pattern, s, re.DOTALL)
  if not json_matches:
    # try to find 2nd pattern
    pattern = r'\{.*?"sentence":.*?"negation":.*?\}'
    json_matches = re.findall(pattern, s)

  # Return the first JSON object found, or None if no match is found
  return json.loads(json_matches[0]) if json_matches else None
```

这样会给你所需的输出，而不是模型重复随机输出的令牌。

# 摘要

在这篇博客中，你学习了QLora的基础知识，如何在Colab上使用QLora微调LLama v2模型，Instruction Tuning，以及一个可以用来进一步指导微调模型的Alpaca数据集样本模板。

## 参考文献

[1]: QLoRA：高效微调量化LLM，2023年5月23日，Tim Dettmers等人

[2]: [https://huggingface.co/blog/hf-bitsandbytes-integration](https://huggingface.co/blog/hf-bitsandbytes-integration)

[3]: [https://huggingface.co/blog/4bit-transformers-bitsandbytes](https://huggingface.co/blog/4bit-transformers-bitsandbytes)

[4]: [https://www.youtube.com/watch?v=y9PHWGOa8HA](https://www.youtube.com/watch?v=y9PHWGOa8HA)

[5]: [https://arxiv.org/abs/2106.09685](https://arxiv.org/abs/2106.09685)

[6]: [https://aclanthology.org/2022.acl-long.244/](https://aclanthology.org/2022.acl-long.244/)

[7]: [https://crfm.stanford.edu/2023/03/13/alpaca.html](https://crfm.stanford.edu/2023/03/13/alpaca.html)

[8]: Colab Notebook由@[maximelabonne](https://twitter.com/maximelabonne) [https://colab.research.google.com/drive/1PEQyJO1-f6j0S_XJ8DV50NkpzasXkrzd?usp=sharing](https://colab.research.google.com/drive/1PEQyJO1-f6j0S_XJ8DV50NkpzasXkrzd?usp=sharing)

**Ahmad Anis**是一位热情的机器学习工程师和研究员，目前在[redbuffer.ai](https://redbuffer.ai/)工作。除了本职工作外，Ahmad还积极参与机器学习社区。他担任了Cohere for AI的区域负责人，这是一个致力于开放科学的非营利组织，同时还是AWS社区建设者。Ahmad是Stackoverflow的活跃贡献者，拥有2300多积分。他为许多著名的开源项目做出了贡献，包括OpenAI的Shap-E。

### 更多相关话题

+   [在Google Colab上免费运行Mixtral 8x7b](https://www.kdnuggets.com/running-mixtral-8x7b-on-google-colab-for-free)

+   [用HuggingFace微调BERT以进行推文分类](https://www.kdnuggets.com/2022/01/finetuning-bert-tweets-classification-ft-hugging-face.html)

+   [用噪声标签数据微调OpenAI语言模型](https://www.kdnuggets.com/2023/04/finetuning-openai-language-models-noisily-labeled-data.html)

+   [PEFT概述：最先进的参数高效微调](https://www.kdnuggets.com/overview-of-peft-stateoftheart-parameterefficient-finetuning)

+   [掌握大型语言模型微调的7个步骤](https://www.kdnuggets.com/7-steps-to-mastering-large-language-model-fine-tuning)

+   [Mistral 7B-V0.2：用Hugging Face微调Mistral的新开源LLM…](https://www.kdnuggets.com/mistral-7b-v02-fine-tuning-mistral-new-open-source-llm-with-hugging-face)
