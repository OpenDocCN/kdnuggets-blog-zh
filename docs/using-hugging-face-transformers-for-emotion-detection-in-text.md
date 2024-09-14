# 使用 Hugging Face Transformers 进行文本情感检测

> 原文：[`www.kdnuggets.com/using-hugging-face-transformers-for-emotion-detection-in-text`](https://www.kdnuggets.com/using-hugging-face-transformers-for-emotion-detection-in-text)

![情感检测和情感分析是流行的 NLP 任务](img/22573f8999395d71f8c12dd0adedaba6.png)

图片由[juicy_fish 在 Freepik](https://www.freepik.com/free-vector/three-feedback-emoji-happy-sad-medium-flat-style_41681695.htm#fromView=search&page=1&position=13&uuid=6fbf4fab-f376-491f-992a-75df016f5210)提供

Hugging Face 托管了各种基于变换器的语言模型（LMs），这些模型专门用于处理语言理解和语言生成任务，包括但不限于：

+   文本分类

+   命名实体识别（NER）

+   文本生成

+   问答

+   总结

+   翻译

* * *

## 我们的前三名课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 工作

* * *

一个特定的——也相当常见的——文本分类任务是情感分析，其目标是识别给定文本的情感。最“简单”的情感分析语言模型被训练来确定输入文本的极性，例如产品的客户评价，可以分为正面和负面，或正面、负面和中性。这两个具体问题分别被表述为二分类或多分类任务。

还有一些语言模型，虽然仍然可以被识别为情感分析模型，但它们被训练来将文本分类为几种情感，例如愤怒、快乐、悲伤等。

本教程基于 Python，重点介绍了如何加载和展示使用 Hugging Face 预训练模型来分类输入文本的主要情感。我们将使用[情感数据集](https://huggingface.co/datasets/jeffnyman/emotions)，这是 Hugging Face 中心公开提供的数据集。该数据集包含成千上万条用英语编写的 Twitter 消息。

## 加载数据集

我们将通过运行以下指令来开始加载情感数据集中的训练数据：

```py
!pip install datasets
from datasets import load_dataset
all_data = load_dataset("jeffnyman/emotions")
train_data = all_data["train"] 
```

以下是*train_data*变量中的训练子集所包含的内容摘要：

```py
Dataset({
features: ['text', 'label'],
num_rows: 16000
})
```

情感数据集中的训练折叠包含 16000 个与 Twitter 消息相关的实例。每个实例有两个特征：一个输入特征包含实际消息文本，一个输出特征或标签包含其关联的情感作为数字标识符：

+   0: 悲伤

+   1: 快乐

+   2: 爱

+   3: 愤怒

+   4: 恐惧

+   5: 惊讶

例如，训练集中的第一个标记实例被分类为“悲伤”情感：

```py
train_data[0]
```

输出：

```py
{'text': 'i didnt feel humiliated', 'label': 0}
```

## 加载语言模型

一旦我们加载了数据，下一步是从 Hugging Face 加载一个适合我们目标情感检测任务的预训练语言模型。使用 **Hugging Face 的 Transformer 库** 有两种主要方法来加载和利用语言模型：

1.  **管道** 提供了一个非常高的抽象层次，使得几乎可以立即加载语言模型并进行推断，只需极少的代码行，但配置选项较少。

1.  **Auto 类** 提供了较低的抽象层次，需要更多的编码技能，但提供了更多的灵活性，以调整模型参数以及自定义文本预处理步骤，如分词。

本教程通过专注于将模型作为管道加载，为你提供了一个简单的起点。管道需要至少指定语言任务的类型， optionally 还可以指定要加载的模型名称。由于情感检测是一种非常具体的文本分类问题，因此加载模型时使用的任务参数应该是“text-classification”：

```py
from transformers import pipeline
classifier = pipeline("text-classification", model="j-hartmann/emotion-english-distilroberta-base")
```

另一方面，强烈建议使用“model”参数指定 Hugging Face hub 中一个能够处理我们特定情感检测任务的模型名称。否则，默认情况下，我们可能会加载一个没有针对这个特定 6 类分类问题训练过的文本分类模型。

你可能会问自己：“我怎么知道使用哪个模型名称？”答案很简单：在 Hugging Face 网站上进行一点探索，找到适合的模型或训练于特定数据集（如情感数据）的模型。

下一步是开始进行预测。管道使得这个推断过程变得极其简单，只需调用我们新实例化的管道变量，并将要分类的输入文本作为参数传递：

```py
example_tweet = "I love hugging face transformers!"
prediction = classifier(example_tweet)
print(prediction) 
```

结果，我们得到一个预测标签和一个置信度分数：这个分数越接近 1，预测结果的“可靠性”越高。

```py
[{'label': 'joy', 'score': 0.9825918674468994}]
```

因此，我们的输入示例“I love hugging face transformers!” 自信地传达了喜悦的情感。

你可以将多个输入文本传递给管道，以同时执行多个预测，如下所示：

```py
example_tweets = ["I love hugging face transformers!", "I really like coffee but it's too bitter..."]
prediction = classifier(example_tweets)
print(prediction)
```

在这个例子中，第二个输入对模型来说似乎更具挑战性，难以进行自信的分类：

```py
[{'label': 'joy', 'score': 0.9825918674468994}, {'label': 'sadness', 'score': 0.38266682624816895}]
```

最后，我们还可以将数据集中一批实例传递给我们的语言模型管道，例如之前加载的“情感”数据。这个例子将前 10 个训练输入传递给我们的语言模型管道进行情感分类，然后打印一个包含每个预测标签的列表，置信度分数不予显示：

```py
train_batch = train_data[:10]["text"]
predictions = classifier(train_batch)
labels = [x['label'] for x in predictions]
print(labels) 
```

输出：

```py
['sadness', 'sadness', 'anger', 'joy', 'anger', 'sadness', 'surprise', 'fear', 'joy', 'joy']
```

作为比较，这里是这 10 个训练实例的原始标签：

```py
print(train_data[:10]["label"])
```

输出：

```py
[0, 0, 3, 2, 3, 0, 5, 4, 1, 2]
```

通过查看每个数字标识符关联的情感，我们可以看到大约 7 成的预测与这 10 个实例的真实标签匹配。

既然你已经知道如何使用 Hugging Face transformer 模型来检测文本情感，为什么不探索其他用例和语言任务，看看预训练的 LMs 如何提供帮助呢？

[](https://www.linkedin.com/in/ivanpc/)****[Iván Palomares Carrascosa](https://www.linkedin.com/in/ivanpc/)**** 是一位在 AI、机器学习、深度学习和 LLMs 领域的领袖、作家、演讲者和顾问。他训练和指导他人在实际应用中利用 AI。

### 了解更多相关话题

+   [如何使用 Hugging Face Transformers 对 BERT 进行微调以进行情感分析](https://www.kdnuggets.com/how-to-fine-tune-bert-sentiment-analysis-hugging-face-transformers)

+   [如何使用 GPT 生成创意内容与 Hugging Face…](https://www.kdnuggets.com/how-to-use-gpt-for-generating-creative-content-with-hugging-face-transformers)

+   [使用 Hugging Face Transformers 构建推荐系统](https://www.kdnuggets.com/building-a-recommendation-system-with-hugging-face-transformers)

+   [如何使用 Hugging Face Tokenizers 库预处理文本数据](https://www.kdnuggets.com/how-to-use-the-hugging-face-tokenizers-library-to-preprocess-text-data)

+   [前 10 大机器学习演示：Hugging Face Spaces 版](https://www.kdnuggets.com/2022/05/top-10-machine-learning-demos-hugging-face-spaces-edition.html)

+   [一个为客户数据建模的 Hugging Face 社区](https://www.kdnuggets.com/2022/08/objectiv-community-developing-hugging-face-customer-data-modeling.html)
