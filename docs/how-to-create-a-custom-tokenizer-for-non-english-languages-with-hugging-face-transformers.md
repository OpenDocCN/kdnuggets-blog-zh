# 如何使用 Hugging Face Transformers 为非英文语言创建自定义分词器

> 原文：[`www.kdnuggets.com/how-to-create-a-custom-tokenizer-for-non-english-languages-with-hugging-face-transformers`](https://www.kdnuggets.com/how-to-create-a-custom-tokenizer-for-non-english-languages-with-hugging-face-transformers)

![非英文语言的自定义分词器](img/cc11ae130d36388a8ade84799bcca8fe.png)

作者插图 | Canva

每当你考虑进行自然语言处理任务时，你最可能需要采取的第一步是对你使用的文本进行分词。这非常重要，因为下游任务的性能在很大程度上依赖于此。我们可以很容易地在网上找到可以分词英文数据集的分词器。但是，如果你想对非英文语言数据进行分词呢？如果你找不到适合你所使用的特定类型数据的分词器，你将如何进行分词？好吧，本文正是解释了这些问题。

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT

* * *

我们将告诉你所有你需要知道的关于训练自定义分词器的知识。

## 什么是分词，为什么它很重要？

分词过程将一段文本拆分成片段或词汇，这些片段随后可以转换成模型可以处理的格式。在这种情况下，模型只能处理数字，因此这些词汇被转换成数字。这个过程至关重要，因为它将原始内容转换为模型可以理解和分析的结构化格式。如果没有分词，我们将无法将文本传递给机器学习模型，因为这些模型只能理解数字，而不能直接理解文本字符串。

## 为什么需要自定义分词器？

从头开始训练分词器在处理非英文语言或特定领域时尤其重要。标准的预训练分词器可能无法有效处理不同语言或专业字符的独特特征、词汇和语法。可以训练新的分词器以管理特定语言的特征和术语。这可以提高分词的准确性和模型的性能。不仅如此，自定义分词器还允许我们包含特定领域的词汇，并处理稀有或词汇表外的词汇。这提高了 NLP 模型在涉及多语言的应用中的效果。

现在你一定在想，你知道分词化很重要，并且可能需要训练你自己的分词器。那么你会怎么做呢？既然你已经了解了训练自己分词器的需求和重要性，我们将一步一步地解释如何做到这一点。

## 自定义分词器的逐步训练过程

#### 第一步：安装所需的库

首先，确保你已安装所需的库。你可以使用 pip 安装 Hugging Face 的 transformers 和 datasets 库：

```py
pip install transformers datasets
```

#### 第二步：准备你的数据集

要训练一个分词器，你需要一个目标语言的文本数据集。Hugging Face 的 datasets 库提供了一种方便的方式来加载和处理数据集。

```py
from datasets import load_dataset

# Load a dataset
dataset = load_dataset('oscar', 'unshuffled_deduplicated_ur)
```

**输出：**

![output](img/e7cc24805f76c9489210cc7c3484ba78.png)

请注意，你可以在此步骤中加载任何语言的数据集。我们的乌尔都语数据集的一个示例如下所示：

```py
موضوعات کی ترتیب بلحاظ: آخری پیغام کا وقت موضوع شامل کرنے کا وقت ٹائٹل (حروف تہجی کے لحاظ سے) جوابات کی تعداد مناظر پسند کردہ پیغامات تلاش کریں. 
```

#### 第三步：初始化分词器

Hugging Face 提供了几种分词器模型。我将使用来自 Hugging Face 库的 BPE 分词器。这是 GPT-3 和 GPT-4 分词器使用的完全相同的算法。但你仍然可以根据需要选择不同的分词器模型。

```py
from tokenizers import ByteLevelBPETokenizer

# Initialize a Byte-Pair Encoding(BPE) tokenizer
tokenizer = ByteLevelBPETokenizer()
```

#### 第四步：训练分词器

在你的数据集上训练分词器。确保数据集经过预处理并正确格式化。你可以指定各种参数，如 vocab_size、min_frequency 和 special_tokens。

```py
# Prepare the dataset for training the tokenizer
def batch_iterator(dataset, batch_size=1000):
    for i in range(0, len(dataset), batch_size):
        yield dataset[i: i + batch_size]

# Train the tokenizer
tokenizer.train_from_iterator(batch_iterator(dataset['text']), vocab_size=30_000, min_frequency=2, special_tokens=[
    "&lts&gt", "&ltpad&gt", "&lt/s&gt", "&ltunk&gt", "&ltmask&gt",
]) 
```

#### 第五步：保存分词器

训练后，将分词器保存到磁盘。这允许你以后加载并使用它。

```py
# Save the tokenizer
import os
# Create the directory if it does not exist
output_dir = 'my_tokenizer'
os.makedirs(output_dir, exist_ok=True)
tokenizer.save_model('my_tokenizer')
```

将 `path_to_save_tokenizer` 替换为保存分词器文件的所需路径。运行此代码将保存两个文件，即：

![](img/308b6424472f4314a60e0cf5e71da9b2.png)

#### 第六步：加载和使用分词器

你可以加载分词器以备将来使用，并在目标语言中对文本进行分词。

```py
from transformers import RobertaTokenizerFast

# Load the tokenizer
tokenizer = RobertaTokenizerFast.from_pretrained('path_to_save_tokenizer')

# Tokenize a sample text
text = "عرصہ ہوچکا وائرلیس چارجنگ اپنا وجود رکھتی ہے لیکن اسمارٹ فونز نے اسے اختیار کرنے میں کافی وقت لیا۔ یہ حقیقت ہے کہ جب پہلی بار وائرلیس چارجنگ آئی تھی تو کچھ مسائل تھے لیکن ٹیکنالوجی کے بہتر ہوتے ہوتے اب زیادہ تر مسائل ختم ہو چکے ہیں۔"
encoded_input = tokenizer(text)
print(encoded_input)
```

此单元格的输出将是：

```py
{'input_ids': [0, 153, 122, 153, 114, 153, 118, 156, 228, 225, 156, 228, 154, 235, 155, 233, 155, 107, 153, 105, 225, 154, 235, 153, 105, 153, 104, 153, 114, 154, 231, 156, 239, 153, 116, 225, 155, 233, 153, 105, 153, 114, 153, 110, 154, 233, 155, 112, 225, 153, 105, 154, 127, 154, 233, 153, 105, 225, 154, 235, 153, 110, 154, 235, 153, 112, 225, 153, 114, 155, 107, 155, 127, 153, 108, 156, 239, 225, 156, 228, 156, 245, 225, 154, 231, 156, 239, 155, 107, 154, 233, 225, 153, 105, 153, 116, 154, 232, 153, 105, 153, 114, 154, 122, 225, 154, 228, 154, 235, 154, 233, 153, ], 'attention_mask': [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, ]}
```

## 总结

要使用 Hugging Face Transformers 为非英语语言训练你自己的分词器，你需要准备一个数据集，初始化一个分词器，训练它，并最终保存以备将来使用。通过以下步骤，你可以创建针对特定语言或领域的分词器。这将提高你所处理的任何特定 NLP 任务的性能。你现在可以轻松地在任何语言上训练分词器。告诉我们你创建了什么。

**[](https://www.linkedin.com/in/kanwal-mehreen1/)**[Kanwal Mehreen](https://www.linkedin.com/in/kanwal-mehreen1/)**** Kanwal 是一名机器学习工程师和技术作家，对数据科学及 AI 与医学的交集充满了深厚的热情。她共同撰写了电子书《利用 ChatGPT 最大化生产力》。作为 2022 年 APAC 的 Google Generation Scholar，她倡导多样性和学术卓越。她还被认可为 Teradata 多样性技术学者、Mitacs Globalink 研究学者和哈佛 WeCode 学者。Kanwal 是变革的热情倡导者，她创立了 FEMCodes 以赋能 STEM 领域的女性。

### 相关话题

+   [如何使用 MarianMT 和 Hugging Face Transformers 进行语言翻译](https://www.kdnuggets.com/how-to-translate-languages-with-marianmt-and-hugging-face-transformers)

+   [如何使用 Hugging Face Transformers 微调 BERT 进行情感分析](https://www.kdnuggets.com/how-to-fine-tune-bert-sentiment-analysis-hugging-face-transformers)

+   [如何利用 GPT 生成创意内容，使用 Hugging Face…](https://www.kdnuggets.com/how-to-use-gpt-for-generating-creative-content-with-hugging-face-transformers)

+   [使用 Hugging Face Transformers 构建推荐系统](https://www.kdnuggets.com/building-a-recommendation-system-with-hugging-face-transformers)

+   [使用 Hugging Face Transformers 进行文本情感检测](https://www.kdnuggets.com/using-hugging-face-transformers-for-emotion-detection-in-text)

+   [如何从零开始构建和训练 Transformer 模型…](https://www.kdnuggets.com/how-to-build-and-train-a-transformer-model-from-scratch-with-hugging-face-transformers)
