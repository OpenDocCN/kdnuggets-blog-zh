# 如何使用 Hugging Face Transformers 从零开始构建和训练一个 Transformer 模型

> 原文：[`www.kdnuggets.com/how-to-build-and-train-a-transformer-model-from-scratch-with-hugging-face-transformers`](https://www.kdnuggets.com/how-to-build-and-train-a-transformer-model-from-scratch-with-hugging-face-transformers)

![Hugging Face Transformers 库](img/7c2590f4db8e141e3008c7f98ba82fe5.png)

图片来源 | Midjourney

Hugging Face Transformers 库提供了易于加载和使用基于 transformer 架构的预训练语言模型（LMs）的工具。但你知道这个库也允许你从零开始实现和训练自己的 transformer 模型吗？本教程通过一步一步的情感分类示例来说明这一点。

* * *

## 我们的前 3 个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT

* * *

**重要说明：** 从零开始训练一个 transformer 模型是计算密集型的，训练循环通常至少需要几个小时。要运行本教程中的代码，强烈建议使用高性能计算资源，无论是本地还是通过云提供商。

## 步骤流程

#### 初始设置和数据集加载

根据你所使用的 Python 开发环境，你可能需要安装 Hugging Face 的 **transformers** 和 **datasets** 库，以及 **accelerate** 库，以在分布式计算环境中训练你的 transformer 模型。

```py
!pip install transformers datasets
!pip install accelerate -U
```

一旦安装了必要的库，让我们加载 [情感数据集](https://huggingface.co/datasets/jeffnyman/emotions) 以进行来自 Hugging Face hub 的 Twitter 消息情感分类：

```py
from datasets import load_dataset
dataset = load_dataset('jeffnyman/emotions')
```

使用数据来训练基于 transformer 的 LM 需要对文本进行分词。以下代码初始化了一个 BERT 分词器（BERT 是适用于文本分类任务的 transformer 模型家族），定义了一个用于分词、填充和截断的函数，并将其应用于数据集的批次中。

```py
from transformers import AutoTokenizer

def tokenize_function(examples):
  return tokenizer(examples['text'], padding="max_length", truncation=True)

tokenizer = AutoTokenizer.from_pretrained('bert-base-uncased')
tokenized_datasets = dataset.map(tokenize_function, batched=True)
```

在初始化 transformer 模型之前，我们先来验证数据集中的唯一标签。验证现有类别标签的集合有助于防止训练期间的 GPU 相关错误，确保标签的一致性和正确性。我们将在后面使用这个标签集合。

```py
unique_labels = set(tokenized_datasets['train']['label'])
print(f"Unique labels in the training set: {unique_labels}")

def check_labels(dataset):
  for label in dataset['train']['label']:
    if label not in unique_labels:
      print(f"Found invalid label: {label}")

check_labels(tokenized_datasets)
```

接下来，我们创建并定义一个模型配置，然后用这个配置实例化 transformer 模型。在这里，我们指定关于 transformer 架构的超参数，比如嵌入大小、注意力头的数量，以及先前计算出的唯一标签集合，这些都是构建情感分类最终输出层的关键。

```py
from transformers import BertConfig
from transformers import BertForSequenceClassification

config = BertConfig(
vocab_size=tokenizer.vocab_size,
hidden_size=512,
num_hidden_layers=6,
num_attention_heads=8,
intermediate_size=2048,
max_position_embeddings=512,
num_labels=len(unique_labels)
)

model = BertForSequenceClassification(config)
```

我们几乎准备好训练我们的 transformer 模型了。只剩下实例化两个必要的实例：**TrainingArguments**，它包含关于训练循环的规范，如 epoch 数量，以及 **Trainer**，它将模型实例、训练参数和用于训练和验证的数据粘合在一起。

```py
from transformers import TrainingArguments, Trainer

training_args = TrainingArguments(
  output_dir='./results',
  evaluation_strategy="epoch",
  learning_rate=2e-5,
  per_device_train_batch_size=16,
  per_device_eval_batch_size=16,
  num_train_epochs=3,
  weight_decay=0.01,
)

trainer = Trainer(
  model=model,
  args=training_args,
  train_dataset=tokenized_datasets["train"],
  eval_dataset=tokenized_datasets["test"],
)
```

训练模型的时间到了，坐下来放松一下吧。记住，这个过程将需要相当长的时间来完成：

```py
trainer.train()
```

训练完成后，你的 transformer 模型应该已经准备好接收输入示例进行情感预测。

#### 故障排除

如果在执行训练循环或设置过程中出现或持续存在问题，你可能需要检查所使用的 GPU/CPU 资源的配置。例如，如果使用 CUDA GPU，可以在代码开始处添加这些指令，以帮助防止训练循环中的错误：

```py
import os
os.environ["CUDA_LAUNCH_BLOCKING"] = "1"
```

这些行禁用 GPU 并使 CUDA 操作同步，从而提供更及时和准确的错误消息以供调试。

另一方面，如果你在 Google Colab 实例中尝试这段代码，执行期间可能会出现这个错误信息，即使你之前已经安装了 accelerate 库：

```py
ImportError: Using the `Trainer` with `PyTorch` requires `accelerate>=0.21.0`: Please run `pip install transformers[torch]` or `pip install accelerate -U`
```

为了解决这个问题，尝试在 'Runtime' 菜单中重启你的会话：accelerate 库通常需要在安装后重置运行环境。

## 总结与结束

本教程展示了从零开始使用 Hugging Face 库构建基于 transformer 的语言模型的关键步骤。主要步骤和涉及的元素可以总结如下：

+   加载数据集并标记文本数据。

+   使用模型配置实例初始化你的模型，以适应特定的模型类型（语言任务），例如 **BertConfig**。

+   设置 **Trainer** 和 **TrainingArguments** 实例并运行训练循环。

作为下一个学习步骤，我们鼓励你探索如何使用你新训练的模型进行预测和推断。

[](https://www.linkedin.com/in/ivanpc/)****[Iván Palomares Carrascosa](https://www.linkedin.com/in/ivanpc/)**** 是人工智能、机器学习、深度学习和大型语言模型领域的领导者、作者、演讲者和顾问。他培训和指导他人如何在现实世界中利用人工智能。

### 更多相关信息

+   [如何使用 Hugging Face Transformers 对 BERT 进行情感分析微调](https://www.kdnuggets.com/how-to-fine-tune-bert-sentiment-analysis-hugging-face-transformers)

+   [如何使用 GPT 生成创意内容与 Hugging Face…](https://www.kdnuggets.com/how-to-use-gpt-for-generating-creative-content-with-hugging-face-transformers)

+   [使用 Hugging Face Transformers 构建推荐系统](https://www.kdnuggets.com/building-a-recommendation-system-with-hugging-face-transformers)

+   [使用 Hugging Face Transformers 进行文本情感检测](https://www.kdnuggets.com/using-hugging-face-transformers-for-emotion-detection-in-text)

+   [使用 Hugging Face 和 Gradio 在 5 分钟内构建 AI 聊天机器人](https://www.kdnuggets.com/2023/06/build-ai-chatbot-5-minutes-hugging-face-gradio.html)

+   [使用 Tensorflow 训练图像分类模型指南](https://www.kdnuggets.com/2022/12/guide-train-image-classification-model-tensorflow.html)
