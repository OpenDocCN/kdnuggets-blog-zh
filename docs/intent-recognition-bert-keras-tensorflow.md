# 使用 Keras 和 TensorFlow 2 的 BERT 意图识别

> 原文：[https://www.kdnuggets.com/2020/02/intent-recognition-bert-keras-tensorflow.html](https://www.kdnuggets.com/2020/02/intent-recognition-bert-keras-tensorflow.html)

[评论](#comments)

**由 [Venelin Valkov](https://www.linkedin.com/in/venelin-valkov/)，机器学习工程师**

从文本中识别意图（IR）现在非常有用。通常，你会得到一段简短的文本（一个或两个句子），并需要将其分类到一个（或多个）类别中。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织进行 IT 工作

* * *

多个产品支持系统（帮助中心）使用 IR 来减少大量员工重复回答常见问题的需要。聊天机器人、自动电子邮件回复者、答案推荐系统（来自含有问答的知识库）都力求不让你占用真正人员的时间。

本指南将展示如何使用预训练的 NLP 模型，这可能解决许多企业主的（技术）支持问题。我的意思是，BERT 实在是太强大了！而且使用起来也非常简单！

[**在浏览器中运行完整笔记本**](https://colab.research.google.com/drive/1WQY_XxdiCVFzjMXnDdNfUjDFi0CN5hkT)

[**在 GitHub 上的完整项目**](https://github.com/curiousily/Deep-Learning-For-Hackers)

### 数据

数据包含各种用户查询，分类为七个意图。它托管在 [GitHub](https://github.com/snipsco/nlu-benchmark/tree/master/2017-06-custom-intent-engines) 上，并首次在 [这篇论文](https://arxiv.org/abs/1805.10190) 中展示。

下面是意图：

+   SearchCreativeWork（例如：找到 I, Robot 电视节目）

+   GetWeather（例如：现在波士顿，MA 风很大吗？）

+   BookRestaurant（例如：我想为我和我的男朋友预定一家高评分的餐厅，明晚去）

+   PlayMusic（例如：播放 Spotify 上的 Beyoncé 最新曲目）

+   AddToPlaylist（例如：将 Diamonds 添加到我的公路旅行播放列表中）

+   RateBook（例如：给《鼠与人》打 6 星）

+   SearchScreeningEvent（例如：查看 Wonder Woman 在巴黎的放映时间）

我做了一些预处理，并将 JSON 文件转换为易于使用/加载的 CSV 文件。我们来下载它们：

```py
!gdown --id 1OlcvGWReJMuyYQuOZm149vHWwPtlboR6 --output train.csv
!gdown --id 1Oi5cRlTybuIF2Fl5Bfsr-KkqrXrdt77w --output valid.csv
!gdown --id 1ep9H6-HvhB4utJRLVcLzieWNUSG3P_uF --output test.csv
```

我们将数据加载到数据框中，并通过合并训练和验证意图来扩展训练数据：

```py
train = pd.read_csv("train.csv")
valid = pd.read_csv("valid.csv")
test = pd.read_csv("test.csv")

train = train.append(valid).reset_index(drop=True)
```

我们有`13,784`个训练样本和两列 - `text`和`intent`。我们来看看每个意图下的文本数量：

![](../Images/a7450a08fc0d97efd88b1912b26faed7.png)

每个意图的文本量相当平衡，因此我们不需要任何不平衡建模技术。

### BERT

BERT（Bidirectional Encoder Representations from Transformers）模型，介绍于 [BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding](https://arxiv.org/abs/1810.04805) 论文中，使得普通机器学习从业者能够在各种NLP任务中取得最先进的成果。你可以在没有大量数据集的情况下实现这一点！但这如何可能呢？

BERT是一个预训练的Transformer编码器堆栈。它在维基百科和[Book Corpus](https://arxiv.org/pdf/1506.06724.pdf)数据集上进行训练。它有两个版本 - Base（12个编码器）和Large（24个编码器）。

BERT建立在NLP社区多个巧妙想法的基础上。一些例子包括 [ELMo](https://arxiv.org/abs/1802.05365)、 [The Transformer](https://arxiv.org/abs/1706.03762)和 [OpenAI Transformer](https://s3-us-west-2.amazonaws.com/openai-assets/research-covers/language-unsupervised/language_understanding_paper.pdf)。

ELMo引入了上下文词嵌入（一个词根据周围的词可以有不同的含义）。Transformer使用注意力机制来理解词语的使用上下文。然后，这个上下文被编码成一个向量表示。实际上，它在处理长期依赖性方面表现更好。

BERT是一个双向模型（同时向前和向后查看）。最棒的是，BERT可以轻松用作特征提取器或在少量数据上进行微调。它在从文本中识别意图方面有多强？

### 使用BERT进行意图识别

幸运的是，BERT论文的作者 [开源了他们的工作](https://github.com/google-research/bert) 以及多个预训练模型。原始实现是用TensorFlow编写的，但也有[非常好的PyTorch实现](https://github.com/huggingface/transformers)！

让我们从下载其中一个较简单的预训练模型开始，并解压它：

```py
!wget https://storage.googleapis.com/bert_models/2018_10_18/uncased_L-12_H-768_A-12.zip
!unzip uncased_L-12_H-768_A-12.zip
```

这将解压检查点、配置和词汇表，以及其他文件。

不幸的是，原始实现与TensorFlow 2不兼容。 [bert-for-tf2](https://github.com/kpe/bert-for-tf2) 包解决了这个问题。

### 预处理

我们需要将原始文本转换为可以输入模型的向量。我们将经过三个步骤：

+   对文本进行分词

+   将令牌序列转换为数字

+   对序列进行填充，使每个序列具有相同的长度

让我们开始创建BERT分词器：

```py
tokenizer = FullTokenizer(
  vocab_file=os.path.join(bert_ckpt_dir, "vocab.txt")
)
```

让我们试试看：

```py
tokenizer.tokenize("I can't wait to visit Bulgaria again!")
```

```py
['i', 'can', "'", 't', 'wait', 'to', 'visit', 'bulgaria', 'again', '!']
```

令牌是小写的，并且标点符号是可用的。接下来，我们将把令牌转换为数字。分词器也可以做到这一点：

```py
tokens = tokenizer.tokenize("I can't wait to visit Bulgaria again!")
tokenizer.convert_tokens_to_ids(tokens)
```

```py
[1045, 2064, 1005, 1056, 3524, 2000, 3942, 8063, 2153, 999]
```

我们将自己处理填充部分。你也可以使用Keras填充工具来完成这部分。

我们将预处理打包成一个类，这个类主要基于[这个笔记本](https://github.com/kpe/bert-for-tf2/blob/master/examples/gpu_movie_reviews.ipynb)：

```py
class IntentDetectionData:
  DATA_COLUMN = "text"
  LABEL_COLUMN = "intent"

  def __init__(
    self,
    train,
    test,
    tokenizer: FullTokenizer,
    classes,
    max_seq_len=192
  ):
    self.tokenizer = tokenizer
    self.max_seq_len = 0
    self.classes = classes

    ((self.train_x, self.train_y), (self.test_x, self.test_y)) =\
     map(self._prepare, [train, test])

    print("max seq_len", self.max_seq_len)
    self.max_seq_len = min(self.max_seq_len, max_seq_len)
    self.train_x, self.test_x = map(
      self._pad,
      [self.train_x, self.test_x]
    )

  def _prepare(self, df):
    x, y = [], []

    for _, row in tqdm(df.iterrows()):
      text, label =\
       row[IntentDetectionData.DATA_COLUMN], \
       row[IntentDetectionData.LABEL_COLUMN]
      tokens = self.tokenizer.tokenize(text)
      tokens = ["[CLS]"] + tokens + ["[SEP]"]
      token_ids = self.tokenizer.convert_tokens_to_ids(tokens)
      self.max_seq_len = max(self.max_seq_len, len(token_ids))
      x.append(token_ids)
      y.append(self.classes.index(label))

    return np.array(x), np.array(y)

  def _pad(self, ids):
    x = []
    for input_ids in ids:
      input_ids = input_ids[:min(len(input_ids), self.max_seq_len - 2)]
      input_ids = input_ids + [0] * (self.max_seq_len - len(input_ids))
      x.append(np.array(input_ids))
    return np.array(x)
```

我们通过取最长文本和最大序列长度参数之间的最小值来确定填充长度。我们还将每个文本的标记用两个特殊标记包围：以`[CLS]`开头，以`[SEP]`结尾。

### 微调

让我们让BERT用于文本分类吧！我们将加载模型并在其上附加几个层：

```py
def create_model(max_seq_len, bert_ckpt_file):

  with tf.io.gfile.GFile(bert_config_file, "r") as reader:
      bc = StockBertConfig.from_json_string(reader.read())
      bert_params = map_stock_config_to_params(bc)
      bert_params.adapter_size = None
      bert = BertModelLayer.from_params(bert_params, name="bert")

  input_ids = keras.layers.Input(
    shape=(max_seq_len, ),
    dtype='int32',
    name="input_ids"
  )
  bert_output = bert(input_ids)

  print("bert shape", bert_output.shape)

  cls_out = keras.layers.Lambda(lambda seq: seq[:, 0, :])(bert_output)
  cls_out = keras.layers.Dropout(0.5)(cls_out)
  logits = keras.layers.Dense(units=768, activation="tanh")(cls_out)
  logits = keras.layers.Dropout(0.5)(logits)
  logits = keras.layers.Dense(
    units=len(classes),
    activation="softmax"
  )(logits)

  model = keras.Model(inputs=input_ids, outputs=logits)
  model.build(input_shape=(None, max_seq_len))

  load_stock_weights(bert, bert_ckpt_file)

  return model
```

我们使用我们的输入（文本和意图）微调预训练的BERT模型。我们还将输出展平，并添加两个全连接层和Dropout。最后一层具有softmax激活函数。输出数量等于我们拥有的意图数量——七个。

现在你可以使用BERT来识别意图了！

### 训练

现在是将一切整合在一起的时候了。我们将从创建数据对象开始：

```py
classes = train.intent.unique().tolist()

data = IntentDetectionData(
  train,
  test,
  tokenizer,
  classes,
  max_seq_len=128
)
```

我们现在可以使用最大序列长度来创建模型：

```py
model = create_model(data.max_seq_len, bert_ckpt_file)
```

查看模型摘要：

```py
model.summar()
```

你会注意到，即使是这个“精简版”的BERT也有近1.1亿个参数。确实，你的模型非常庞大（她是这么说的）。

微调像BERT这样的模型既是一门艺术，也涉及大量的失败实验。幸运的是，作者提出了一些建议：

+   批量大小：16，32

+   学习率（Adam）：5e-5，3e-5，2e-5

+   轮次数量：2，3，4

```py
model.compile(
  optimizer=keras.optimizers.Adam(1e-5),
  loss=keras.losses.SparseCategoricalCrossentropy(from_logits=True),
  metrics=[keras.metrics.SparseCategoricalAccuracy(name="acc")]
)
```

我们将使用Adam，并使用略微不同的学习率（因为我们很牛逼），并使用稀疏分类交叉熵，因此我们不必对标签进行独热编码。

让我们拟合模型：

```py
log_dir = "log/intent_detection/" +\
 datetime.datetime.now().strftime("%Y%m%d-%H%M%s")
tensorboard_callback = keras.callbacks.TensorBoard(log_dir=log_dir)

model.fit(
  x=data.train_x,
  y=data.train_y,
  validation_split=0.1,
  batch_size=16,
  shuffle=True,
  epochs=5,
  callbacks=[tensorboard_callback]
)
```

我们存储训练日志，以便你可以在[Tensorboard](https://www.tensorflow.org/tensorboard)中查看训练过程。让我们来看一下：

![](../Images/880f4ce53de05f06386b7e08afbde3db.png)![](../Images/b1e36a67c88c06cd6e88ac96ad8b327f.png)

### 评估

我必须对你诚实。我对结果感到惊讶。仅使用12.5k样本进行训练，我们得到了：

```py
_, train_acc = model.evaluate(data.train_x, data.train_y)
_, test_acc = model.evaluate(data.test_x, data.test_y)

print("train acc", train_acc)
print("test acc", test_acc)
```

```py
train acc 0.9915119
test acc 0.9771429
```

很了不起，对吧？让我们看看混淆矩阵：

![](../Images/a37bea84a0725c75db91a07cf296851c.png)

最后，让我们使用模型从一些自定义句子中检测意图：

```py
sentences = [
  "Play our song now",
  "Rate this book as awful"
]

pred_tokens = map(tokenizer.tokenize, sentences)
pred_tokens = map(lambda tok: ["[CLS]"] + tok + ["[SEP]"], pred_tokens)
pred_token_ids = list(map(tokenizer.convert_tokens_to_ids, pred_tokens))

pred_token_ids = map(
  lambda tids: tids +[0]*(data.max_seq_len-len(tids)),
  pred_token_ids
)
pred_token_ids = np.array(list(pred_token_ids))

predictions = model.predict(pred_token_ids).argmax(axis=-1)

for text, label in zip(sentences, predictions):
  print("text:", text, "\nintent:", classes[label])
  print()
```

```py
text: Play our song now
intent: PlayMusic

text: Rate this book as awful
intent: RateBook
```

哇，那真是（显然）很酷！好吧，虽然这些示例可能没有实际查询那么多样化，但嘿，赶紧自己试试吧！

### 结论

现在你知道如何对BERT模型进行微调以进行文本分类。你可能已经知道，它还可以用于各种其他任务！你只需要调整层即可。简单！

[**在浏览器中运行完整笔记本**](https://colab.research.google.com/drive/1WQY_XxdiCVFzjMXnDdNfUjDFi0CN5hkT)

[**GitHub上的完整项目**](https://github.com/curiousily/Deep-Learning-For-Hackers)

做AI/ML感觉就像拥有超能力，对吧？感谢美妙的NLP社区，你也可以拥有超能力！你会用它们做什么呢？

### 参考资料

+   [BERT微调教程与PyTorch](https://mccormickml.com/2019/07/22/BERT-fine-tuning/)

+   [SNIPS数据集](https://github.com/snipsco/nlu-benchmark/tree/master/2017-06-custom-intent-engines)

+   [插图版BERT、ELMo等](https://jalammar.github.io/illustrated-bert/)

+   [BERT傻瓜教程——逐步指南](https://towardsdatascience.com/bert-for-dummies-step-by-step-tutorial-fb90890ffe03)

+   [使用BERT的多标签文本分类——强大的变换器](https://medium.com/huggingface/multi-label-text-classification-using-bert-the-mighty-transformer-69714fa3fb3d)

+   [深度学习](https://www.curiousily.com/tag/deep-learning/)

+   [Keras](https://www.curiousily.com/tag/keras/)

+   [自然语言处理（NLP）](https://www.curiousily.com/tag/nlp/)

+   [文本分类](https://www.curiousily.com/tag/text-classification/)

+   [Python](https://www.curiousily.com/tag/python/)

### 继续你的机器学习之旅：

[![](../Images/c7f0e800e0a4fe82a04944c026cdf6bd.png)](http://bit.ly/Hackers-Guide-to-Machine-Learning-with-Python)

***Python黑客机器学习指南***

通过使用Scikit-Learn、TensorFlow 2和Keras解决实际机器学习问题的实践指南

[购买此书](http://bit.ly/Hackers-Guide-to-Machine-Learning-with-Python)

[![](../Images/c5323a2dc7769caab61657e19077dc2f.png)](http://bit.ly/hands-on-machine-learning-from-scratch)

***从零开始的实用机器学习***

通过从头开始使用Python构建机器学习模型、工具和概念，深入理解它们

[购买此书](http://bit.ly/hands-on-machine-learning-from-scratch)

[![](../Images/ad6654296ab3297e4e122de002cc6519.png)](http://bit.ly/deep-learning-for-javascript-hackers)

***JavaScript黑客深度学习***

初学者在浏览器中使用TensorFlow.js理解机器学习的指南

[购买此书](http://bit.ly/deep-learning-for-javascript-hackers)

**简介：[Venelin Valkov](https://www.linkedin.com/in/venelin-valkov/)** (**[GitHub](https://github.com/curiousily)**) 是一名机器学习工程师，专注于使用深度学习进行文档数据提取。在空闲时间，他喜欢写作、做副项目、骑自行车和做硬拉！

[原文](https://www.curiousily.com/posts/intent-recognition-with-bert-using-keras-and-tensorflow-2/)。经允许转载。

**相关：**

+   [BERT正在改变NLP领域](/2019/09/bert-changing-nlp-landscape.html)

+   [Lit BERT：3步完成NLP迁移学习](/2019/11/lit-bert-nlp-transfer-learning-3-steps.html)

+   [BERT、RoBERTa、DistilBERT、XLNet：选择哪个？](/2019/09/bert-roberta-distilbert-xlnet-one-use.html)

### 更多相关内容

+   [使用TensorFlow和Keras构建和训练第一个神经网络](https://www.kdnuggets.com/2023/05/building-training-first-neural-network-tensorflow-keras.html)

+   [使用BERT分类长文本文档](https://www.kdnuggets.com/2022/02/classifying-long-text-documents-bert.html)

+   [使用BERT的抽取式总结](https://www.kdnuggets.com/extractive-summarization-with-llm-using-bert)

+   [图像识别和自然语言处理中的迁移学习](https://www.kdnuggets.com/2022/01/transfer-learning-image-recognition-natural-language-processing.html)

+   [语音识别指标的发展](https://www.kdnuggets.com/2022/10/evolution-speech-recognition-metrics.html)

+   [5个需求高但不被重视的IT职位](https://www.kdnuggets.com/5-it-jobs-that-are-high-in-demand-but-dont-get-enough-recognition)
