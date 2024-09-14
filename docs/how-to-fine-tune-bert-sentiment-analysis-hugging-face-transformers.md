# 如何使用 Hugging Face Transformers 微调 BERT 进行情感分析

> 原文：[`www.kdnuggets.com/how-to-fine-tune-bert-sentiment-analysis-hugging-face-transformers`](https://www.kdnuggets.com/how-to-fine-tune-bert-sentiment-analysis-hugging-face-transformers)

![如何使用 Hugging Face Transformers 微调 BERT 进行情感分析](img/0e059bc833fc77fec44bf0e5333af868.png)

图片由作者使用 Midjourney 创建

## 介绍

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析水平

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

情感分析是指自然语言处理（NLP）技术，用于判断文本中的情感表达，是现代客户反馈评估、社交媒体情感跟踪和市场研究应用背后的核心技术。情感分析帮助企业和其他组织评估公众意见，提供更好的客户服务，并增强他们的产品或服务。

BERT，即双向编码器表示模型，是一种语言处理模型，当初发布时，通过对词语上下文的重要理解，显著提升了 NLP 的最先进水平。BERT 的双向性——同时读取给定词的左右上下文——在情感分析等用例中表现得尤为有价值。

在这个全面的指南中，你将学习如何使用 Hugging Face Transformers 库微调 BERT 以适应你自己的情感分析项目。无论你是新手还是现有的 NLP 从业者，我们将在这一步一步的教程中涵盖许多实用的策略和考虑因素，以确保你能够为自己的目的正确地微调 BERT。

## 设置环境

在微调模型之前，需要完成一些必要的前置条件。具体而言，这将至少需要 Hugging Face Transformers，以及 PyTorch 和 Hugging Face 的数据集库。你可以按如下方式进行。

```py
pip install transformers torch datasets
```

就这样。

## 数据预处理

你需要选择一些数据来训练文本分类器。在这里，我们将使用 IMDb 电影评论数据集，这是展示情感分析的地方之一。让我们使用 `datasets` 库来加载数据集。

```py
from datasets import load_dataset

dataset = load_dataset("imdb")
print(dataset)
```

我们需要对数据进行分词，以便为自然语言处理算法做准备。BERT 有一个特殊的分词步骤，可以确保句子片段在转换时尽可能保持对人类的连贯性。让我们看看如何通过使用 `BertTokenizer` 从 Transformers 来分词数据。

```py
from transformers import BertTokenizer

tokenizer = BertTokenizer.from_pretrained('bert-base-uncased')

def tokenize_function(examples):
    return tokenizer(examples['text'], padding="max_length", truncation=True)

tokenized_datasets = dataset.map(tokenize_function, batched=True)
```

## 准备数据集

我们将把数据集拆分为训练集和验证集，以评估模型的性能。以下是我们将如何进行。

```py
from datasets import train_test_split

train_testvalid = tokenized_datasets['train'].train_test_split(test_size=0.2)
train_dataset = train_testvalid['train']
valid_dataset = train_testvalid['test']
```

DataLoaders 有助于在训练过程中高效地管理数据批次。以下是我们将如何为训练集和验证集创建 DataLoaders。

```py
from torch.utils.data import DataLoader

train_dataloader = DataLoader(train_dataset, shuffle=True, batch_size=8)
valid_dataloader = DataLoader(valid_dataset, batch_size=8)
```

## 为微调设置 BERT 模型

我们将使用 `BertForSequenceClassification` 类来加载我们的模型，该模型已针对序列分类任务进行了预训练。以下是我们将如何进行。

```py
from transformers import BertForSequenceClassification, AdamW

model = BertForSequenceClassification.from_pretrained('bert-base-uncased', num_labels=2) 
```

## 训练模型

训练我们的模型涉及定义训练循环，指定损失函数、优化器以及其他训练参数。以下是我们如何设置和运行训练循环。

```py
from transformers import Trainer, TrainingArguments

training_args = TrainingArguments(
    output_dir='./results',
    evaluation_strategy="epoch",
    learning_rate=2e-5,
    per_device_train_batch_size=8,
    per_device_eval_batch_size=8,
    num_train_epochs=3,
    weight_decay=0.01,
)

trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=train_dataset,
    eval_dataset=valid_dataset,
)

trainer.train()
```

## 模型评估

评估模型涉及使用准确率、精确率、召回率和 F1-score 等指标来检查其性能。以下是我们如何评估模型。

```py
metrics = trainer.evaluate()
print(metrics)
```

## 进行预测

微调后，我们现在可以使用模型对新数据进行预测。这是我们如何在验证集上进行推断的。

```py
predictions = trainer.predict(valid_dataset)
print(predictions)
```

## 总结

本教程涵盖了使用 Hugging Face Transformers 对 BERT 进行情感分析的微调，并包括环境设置、数据集准备和分词、DataLoader 创建、模型加载和训练，以及模型评估和实时模型预测。

对 BERT 进行情感分析的微调在许多现实世界情境中都非常有价值，例如分析客户反馈、追踪社交媒体的语调等。通过使用不同的数据集和模型，你可以为自己的自然语言处理项目扩展这一方法。

有关这些主题的更多信息，请查看以下资源：

+   [Hugging Face Transformers 文档](https://huggingface.co/transformers/)

+   [PyTorch 文档](https://pytorch.org/docs/stable/index.html)

+   [Hugging Face 数据集文档](https://huggingface.co/docs/datasets/)

这些资源值得深入研究，以更好地理解这些问题并提升你的自然语言处理和情感分析能力。

[](https://www.linkedin.com/in/mattmayo13/)****[马修·梅奥](https://www.kdnuggets.com/wp-content/uploads/./profile-pic.jpg)**** ([**@mattmayo13**](https://twitter.com/mattmayo13)) 拥有计算机科学硕士学位和数据挖掘研究生文凭。作为 [KDnuggets](https://www.kdnuggets.com/) 和 [Statology](https://www.statology.org/) 的执行编辑以及 [Machine Learning Mastery](https://machinelearningmastery.com/) 的特约编辑，马修旨在让复杂的数据科学概念变得易于理解。他的职业兴趣包括自然语言处理、语言模型、机器学习算法以及探索新兴的人工智能。他致力于使数据科学社区中的知识实现民主化。马修从 6 岁起就开始编程。

### 更多相关主题

+   [如何使用 Hugging Face AutoTrain 微调 Mistral AI 7B LLM](https://www.kdnuggets.com/how-to-finetune-mistral-ai-7b-llm-with-hugging-face-autotrain)

+   [如何使用 GPT 生成创意内容，结合 Hugging Face……](https://www.kdnuggets.com/how-to-use-gpt-for-generating-creative-content-with-hugging-face-transformers)

+   [使用 Hugging Face Transformers 构建推荐系统](https://www.kdnuggets.com/building-a-recommendation-system-with-hugging-face-transformers)

+   [使用 Hugging Face Transformers 进行文本情感检测](https://www.kdnuggets.com/using-hugging-face-transformers-for-emotion-detection-in-text)

+   [如何从头开始构建和训练 Transformer 模型……](https://www.kdnuggets.com/how-to-build-and-train-a-transformer-model-from-scratch-with-hugging-face-transformers)

+   [如何使用 MarianMT 和 Hugging Face Transformers 翻译语言](https://www.kdnuggets.com/how-to-translate-languages-with-marianmt-and-hugging-face-transformers)
