# 如何使用 Hugging Face Tokenizers 库来预处理文本数据

> 原文：[https://www.kdnuggets.com/how-to-use-the-hugging-face-tokenizers-library-to-preprocess-text-data](https://www.kdnuggets.com/how-to-use-the-hugging-face-tokenizers-library-to-preprocess-text-data)

![ Hugging Face Tokenizers Library](../Images/23c326950f8c1dfd30d486cd87509ab3.png)

作者提供的图像

如果你学习过 NLP，你可能听说过“分词”这个术语。这是文本预处理中的一个重要步骤，我们通过将文本数据转换为机器可以理解的形式来完成。这是通过将句子拆分成更小的部分，称为标记。根据使用的分词算法，这些标记可以是单词、子词，甚至是字符。本文将探讨如何使用 Hugging Face Tokenizers 库来预处理我们的文本数据。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 管理

* * *

## 设置 Hugging Face Tokenizers 库

要开始使用 Hugging Face Tokenizers 库，你需要先安装它。你可以使用 pip 来完成这个操作：

```py
pip install tokenizers
```

Hugging Face 库支持多种分词算法，但主要有三种类型：

+   **字节对编码 (BPE)：** 迭代地合并最频繁的字符或子词对，创建一个紧凑的词汇表。GPT-2 等模型使用了这种方法。

+   **WordPiece：** 类似于 BPE，但侧重于概率合并（不选择最频繁的对，而是选择合并后能最大化语料库概率的对），通常被 BERT 等模型使用。

+   **SentencePiece：** 一种更灵活的分词器，可以处理不同语言和脚本，通常与 ALBERT、XLNet 或 Marian 框架等模型一起使用。它将空格视为字符，而不是单词分隔符。

Hugging Face Transformers 库提供了一个`AutoTokenizer`类，可以自动选择适合给定预训练模型的最佳分词器。这是使用特定模型正确分词器的一种便捷方式，并且可以从`transformers`库中导入。然而，考虑到我们对 Tokenizers 库的讨论，我们将不采用这种方法。

我们将使用预训练的`BERT-base-uncased`分词器。这个分词器是基于与`BERT-base-uncased`模型相同的数据和技术训练的，这意味着它可以用来预处理与 BERT 模型兼容的文本数据：

```py
# Import the necessary components
from tokenizers import Tokenizer
from transformers import BertTokenizer

# Load the pre-trained BERT-base-uncased tokenizer
tokenizer = BertTokenizer.from_pretrained('bert-base-uncased')
```

## 单句分词

现在，让我们使用这个标记器对一个简单句子进行编码：

```py
# Tokenize a single sentence
encoded_input = tokenizer.encode_plus("This is sample text to test tokenization.")
print(encoded_input)
```

**输出：**

```py
{'input_ids': [101, 2023, 2003, 7099, 3793, 2000, 3231, 19204, 3989, 1012, 102], 'token_type_ids': [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 'attention_mask': [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]}
```

为确保准确性，让我们解码标记化的输入：

```py
tokenizer.decode(encoded_input["input_ids"])
```

**输出：**

```py
[CLS] this is sample text to test tokenization. [SEP]
```

在这个输出中，你可以看到两个特殊标记。`[CLS]` 标记输入序列的开始，`[SEP]` 标记结束，表示单个文本序列。

## 批量标记化

现在，让我们使用 `batch_encode_plus` 对文本语料库进行标记化，而不是单个句子：

```py
corpus = [
    "Hello, how are you?",
    "I am learning how to use the Hugging Face Tokenizers library.",
    "Tokenization is a crucial step in NLP."
]
encoded_corpus = tokenizer.batch_encode_plus(corpus)
print(encoded_corpus)
```

**输出：**

```py
{'input_ids': [[101, 7592, 1010, 2129, 2024, 2017, 1029, 102], [101, 1045, 2572, 4083, 2129, 2000, 2224, 1996, 17662, 2227, 19204, 17629, 2015, 3075, 1012, 102], [101, 19204, 3989, 2003, 1037, 10232, 3357, 1999, 17953, 2361, 1012, 102]], 'token_type_ids': [[0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]], 'attention_mask': [[1, 1, 1, 1, 1, 1, 1, 1], [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1], [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]]}
```

为了更好地理解，让我们像处理单句时那样解码批量编码的语料库。这将提供原始句子，标记化得当。

```py
tokenizer.batch_decode(encoded_corpus["input_ids"])
```

**输出：**

```py
['[CLS] hello, how are you? [SEP]',
 '[CLS] i am learning how to use the hugging face tokenizers library. [SEP]',
 '[CLS] tokenization is a crucial step in nlp. [SEP]']
```

## 填充和截断

在为机器学习模型准备数据时，确保所有输入序列具有相同长度通常是必要的。实现这一点的两种方法是：

### 1\. 填充

填充通过在较短序列的末尾添加特殊标记 `[PAD]`，以匹配批次中最长序列的长度或模型支持的最大长度（如果定义了 `max_length`）。你可以通过以下方式实现：

```py
encoded_corpus_padded = tokenizer.batch_encode_plus(corpus, padding=True)
print(encoded_corpus_padded)
```

**输出：**

```py
{'input_ids': [[101, 7592, 1010, 2129, 2024, 2017, 1029, 102, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1045, 2572, 4083, 2129, 2000, 2224, 1996, 17662, 2227, 19204, 17629, 2015, 3075, 1012, 102], [101, 19204, 3989, 2003, 1037, 10232, 3357, 1999, 17953, 2361, 1012, 102, 0, 0, 0, 0]], 'token_type_ids': [[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]], 'attention_mask': [[1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0], [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1], [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0]]}
```

现在，你可以看到额外的 0 被放置，但为了更好地理解，让我们解码以查看标记器放置 `[PAD]` 标记的位置：

```py
tokenizer.batch_decode(encoded_corpus_padded["input_ids"], skip_special_tokens=False)
```

**输出：**

```py
['[CLS] hello, how are you? [SEP] [PAD] [PAD] [PAD] [PAD] [PAD] [PAD] [PAD] [PAD]',
 '[CLS] i am learning how to use the hugging face tokenizers library. [SEP]',
 '[CLS] tokenization is a crucial step in nlp. [SEP] [PAD] [PAD] [PAD] [PAD]']
```

### 2\. 截断

许多 NLP 模型有最大输入长度序列，截断通过剪切较长序列的末尾来满足这个最大长度。它减少了内存使用，并防止模型被非常大的输入序列压垮。

```py
encoded_corpus_truncated = tokenizer.batch_encode_plus(corpus, truncation=True, max_length=5)
print(encoded_corpus_truncated)
```

**输出：**

```py
{'input_ids': [[101, 7592, 1010, 2129, 102], [101, 1045, 2572, 4083, 102], [101, 19204, 3989, 2003, 102]], 'token_type_ids': [[0, 0, 0, 0, 0], [0, 0, 0, 0, 0], [0, 0, 0, 0, 0]], 'attention_mask': [[1, 1, 1, 1, 1], [1, 1, 1, 1, 1], [1, 1, 1, 1, 1]]}
```

现在，你还可以使用 `batch_decode` 方法，但为了更好地理解，让我们以不同的方式打印这些信息：

```py
for i, sentence in enumerate(corpus):
    print(f"Original sentence: {sentence}")
    print(f"Token IDs: {encoded_corpus_truncated['input_ids'][i]}")
    print(f"Tokens: {tokenizer.convert_ids_to_tokens(encoded_corpus_truncated['input_ids'][i])}")
    print()
```

**输出：**

```py
Original sentence: Hello, how are you?
Token IDs: [101, 7592, 1010, 2129, 102]
Tokens: ['[CLS]', 'hello', ',', 'how', '[SEP]']

Original sentence: I am learning how to use the Hugging Face Tokenizers library.
Token IDs: [101, 1045, 2572, 4083, 102]
Tokens: ['[CLS]', 'i', 'am', 'learning', '[SEP]']

Original sentence: Tokenization is a crucial step in NLP.
Token IDs: [101, 19204, 3989, 2003, 102]
Tokens: ['[CLS]', 'token', '##ization', 'is', '[SEP]']
```

本文是我们关于 Hugging Face 的精彩系列的一部分。如果你想更深入了解这个话题，这里有一些参考资料可以帮助你：

+   [Hugging Face 标记器文档](https://huggingface.co/docs/transformers/main/en/main_classes/tokenizer#tokenizer)

+   [Hugging Face Transformers 课程：数据预处理](https://huggingface.co/docs/transformers/v4.14.1/en/preprocessing)

+   [HuggingFace: 各种标记器的总结](https://huggingface.co/docs/transformers/en/tokenizer_summary)

**[](https://www.linkedin.com/in/kanwal-mehreen1/)**[Kanwal Mehreen](https://www.linkedin.com/in/kanwal-mehreen1/)**** Kanwal 是一位机器学习工程师和技术作家，对数据科学及人工智能与医学的交叉领域充满热情。她共同撰写了电子书《利用 ChatGPT 最大化生产力》。作为 2022 年亚太地区的 Google Generation 学者，她倡导多样性和学术卓越。她还被认可为 Teradata 多样性科技学者、Mitacs Globalink 研究学者和哈佛 WeCode 学者。Kanwal 是变革的积极倡导者，创立了 FEMCodes 以赋能女性在 STEM 领域的成长。

### 更多相关话题

+   [如何使用 Hugging Face 的数据集库进行高效的数据加载](https://www.kdnuggets.com/how-to-use-hugging-faces-datasets-library-for-efficient-data-loading)

+   [使用 Hugging Face Transformers 进行文本情感检测](https://www.kdnuggets.com/using-hugging-face-transformers-for-emotion-detection-in-text)

+   [如何使用 Hugging Face AutoTrain 微调 LLMs](https://www.kdnuggets.com/how-to-use-hugging-face-autotrain-to-finetune-llms)

+   [如何使用 GPT 生成创意内容与 Hugging Face…](https://www.kdnuggets.com/how-to-use-gpt-for-generating-creative-content-with-hugging-face-transformers)

+   [一个社区正在开发用于客户数据建模的 Hugging Face](https://www.kdnuggets.com/2022/08/objectiv-community-developing-hugging-face-customer-data-modeling.html)

+   [前十名机器学习演示：Hugging Face Spaces 版](https://www.kdnuggets.com/2022/05/top-10-machine-learning-demos-hugging-face-spaces-edition.html)
