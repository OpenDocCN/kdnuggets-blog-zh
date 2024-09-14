# 如何将变压器应用于任何长度的文本

> 原文：[`www.kdnuggets.com/2021/04/apply-transformers-any-length-text.html`](https://www.kdnuggets.com/2021/04/apply-transformers-any-length-text.html)

评论

**由[James Briggs](https://youtube.com/c/JamesBriggs)，数据科学家**

目前在许多自然语言处理（NLP）任务中，使用变压器（transformer）已成为事实上的标准。文本生成？*变压器*。问答系统？*变压器*。语言分类？*变压器*！

然而，许多这些模型（这个问题不仅仅限于变压器模型）面临的问题是我们**不能**处理长篇文本。

我在 Medium 上写的几乎每一篇文章都包含 1000 多个单词，当这些单词被用于像 BERT 这样的变压器模型时，会生成 1000 多个标记。BERT（和许多其他变压器模型）最多接受**512 个标记**——超出这个长度的内容将被截断。

尽管我认为你可能会发现处理我的 Medium 文章的价值不大，但这同样适用于许多有用的数据来源——如新闻文章或 Reddit 帖子。

我们将探讨如何绕过这个限制。在本文中，我们将找出来自*/r/investing*子版块的长帖子中的情感。这篇文章将涵盖：

```py
**High-Level Approach****Getting Started**
- Data
- Initialization**Tokenization****Preparing The Chunks**
- Split
- CLS and SEP
- Padding
- Reshaping For BERT**Making Predictions**
```

如果你更喜欢视频，我在这里也涵盖了所有内容：

### 高级方法

计算长篇文本情感的逻辑实际上非常简单。

我们将把文本（例如 1361 个标记）分解成每个不超过 512 个标记的块。

![](img/14b47a652ba55c759d07ea03746fb0c7.png)

一个包含 1361 个标记的张量可以被拆分成三个较小的张量。前两个张量每个包含 512 个标记，而最后一个张量包含剩余的 337 个标记。

一旦我们有了这些块并将其转换为可以被 BERT（稍后会详细说明）处理的格式——我们将它们传递给我们的模型，并检索每个块的情感得分。

最后，对每个情感类别取平均——为我们提供整个文本（所有 1361 个标记）的总体情感预测。

现在，解释高级方法是一回事。编写代码则是另一回事。所以让我们开始通过一个示例来工作。

### 入门指南

### 数据

首先，我们需要一些数据进行处理。我在/r/investing 上找到了这个相当长的帖子：

```py
I would like to get your all  thoughts on the bond yield increase this week.  I am not worried about the market downturn but the sudden increase in yields. On 2/16 the 10 year bonds yields increased by almost  9 percent and on 2/19 the yield increased by almost 5 percent.

Key Points from the CNBC Article:

* **The “taper tantrum” in 2013 was a sudden spike in Treasury yields due to market panic after the Federal Reserve announced that it would begin tapering its quantitative easing program.**
* **Major central banks around the world have cut interest rates to historic lows and launched unprecedented quantities of asset purchases in a bid to shore up the economy throughout the pandemic.**
* **However, the recent rise in yields suggests that some investors are starting to anticipate a tightening of policy sooner than anticipated to accommodate a potential rise in inflation.**

The recent rise in bond yields and U.S. inflation expectations has some investors wary that a repeat of the 2013 “taper tantrum” could be on the horizon.

The benchmark U.S. 10-year Treasury note climbed above 1.3**% f**or the first time since February 2020 earlier this week, while the 30-year bond also hit its highest level for a year. Yields move inversely to bond prices.

Yields tend to rise in lockstep with inflation expectations, which have reached their highest levels in a decade in the U.S., powered by increased prospects of a large fiscal stimulus package, progress on vaccine rollouts and pent-up consumer demand.

The “taper tantrum” in 2013 was a sudden spike in Treasury yields due to market panic after the Federal Reserve announced that it would begin tapering its quantitative easing program.

Major central banks around the world have cut interest rates to historic lows and launched unprecedented quantities of asset purchases in a bid to shore up the economy throughout the pandemic. The Fed and others have maintained supportive tones in recent policy meetings, vowing to keep financial conditions loose as the global economy looks to emerge from the Covid-19 pandemic.

However, the recent rise in yields suggests that some investors are starting to anticipate a tightening of policy sooner than anticipated to accommodate a potential rise in inflation.

With central bank support removed, bonds usually fall in price which sends yields higher. This can also spill over into stock markets as higher interest rates means more debt servicing for firms, causing traders to reassess the investing environment.

“The supportive stance from policymakers will likely remain in place until the vaccines have paved a way to some return to normality,” said Shane Balkham, chief investment officer at Beaufort Investment, in a research note this week.

“However, there will be a risk of another ‘taper tantrum’ similar to the one we witnessed in 2013, and this is our main focus for 2021,” Balkham projected, should policymakers begin to unwind this stimulus.

Long-term bond yields in Japan and Europe followed U.S. Treasurys higher toward the end of the week as bondholders shifted their portfolios.

“The fear is that these assets are priced to perfection when the ECB and Fed might eventually taper,” said Sebastien Galy, senior macro strategist at Nordea Asset Management, in a research note entitled “Little taper tantrum.”

“The odds of tapering are helped in the United States by better retail sales after four months of disappointment and the expectation of large issuance from the $1.9 trillion fiscal package.”

Galy suggested the Fed would likely extend the duration on its asset purchases, moderating the upward momentum in inflation.

“Equity markets have reacted negatively to higher yield as it offers an alternative to the dividend yield and a higher discount to long-term cash flows, making them focus more on medium-term growth such as cyclicals” he said. Cyclicals are stocks whose performance tends to align with economic cycles.

Galy expects this process to be more marked in the second half of the year when economic growth picks up, increasing the potential for tapering.

## Tapering in the U.S., but not Europe

Allianz CEO Oliver Bäte told CNBC on Friday that there was a geographical divergence in how the German insurer is thinking about the prospect of interest rate hikes.

“One is Europe, where we continue to have financial repression, where the ECB continues to buy up to the max in order to minimize spreads between the north and the south — the strong balance sheets and the weak ones — and at some point somebody will have to pay the price for that, but in the short term I don’t see any spike in interest rates,” Bäte said, adding that the situation is different stateside.

“Because of the massive programs that have happened, the stimulus that is happening, the dollar being the world’s reserve currency, there is clearly a trend to stoke inflation and it is going to come. Again, I don’t know when and how, but the interest rates have been steepening and they should be steepening further.”

## Rising yields a ‘normal feature’

However, not all analysts are convinced that the rise in bond yields is material for markets. In a note Friday, Barclays Head of European Equity Strategy Emmanuel Cau suggested that rising bond yields were overdue, as they had been lagging the improving macroeconomic outlook for the second half of 2021, and said they were a “normal feature” of economic recovery.

“With the key drivers of inflation pointing up, the prospect of even more fiscal stimulus in the U.S. and pent up demand propelled by high excess savings, it seems right for bond yields to catch-up with other more advanced reflation trades,” Cau said, adding that central banks remain “firmly on hold” given the balance of risks.

He argued that the steepening yield curve is “typical at the early stages of the cycle,” and that so long as vaccine rollouts are successful, growth continues to tick upward and central banks remain cautious, reflationary moves across asset classes look “justified” and equities should be able to withstand higher rates.

“Of course, after the strong move of the last few weeks, equities could mark a pause as many sectors that have rallied with yields look overbought, like commodities and banks,” Cau said.

“But at this stage, we think rising yields are more a confirmation of the equity bull market than a threat, so dips should continue to be bought.”
```

我们将用这个作为我们的示例。

### 初始化

下一步是初始化我们的模型和分词器。我们将使用 PyTorch 和 HuggingFace 变压器库来完成所有任务。

幸运的是，使用变压器库进行初始化非常简单。我们将使用 BERT 模型进行序列分类和相应的 BERT 分词器，所以我们写：

因为我们处理的是金融相关的语言，我们加载了`ProsusAI/finbert`模型——一个更懂金融的 BERT [1]。你可以在[这里](https://huggingface.co/ProsusAI/finbert)找到模型的详细信息。

### 分词

标记化是将文本字符串转换为标记（单词/标点）和/或标记 ID（将单词映射到嵌入数组中单词向量表示的整数）的过程。

使用 transformers 库和 BERT，这通常如下所示：

```py
txt = "<this is the large post included above>"tokens = tokenizer.encode_plus(
    txt, add_special_tokens=True,
    max_length=512, truncation=True,
    padding="max_length"
)
```

在这里，我们使用标记器`encode_plus`方法从`txt`字符串创建我们的标记。

+   `add_special_tokens=True`添加像*[CLS]*，*[SEP]*和*[PAD]*这样的特殊 BERT 标记到我们新的“标记化”编码中。

+   `max_length=512`告知编码器我们编码的目标长度。

+   `truncation=True`确保我们裁剪任何长于指定`max_length`的序列。

+   `padding="max_length"`指示编码器用填充标记填充任何短于`max_length`的序列。

这些参数构成了标记化的典型方法。但正如你所见，当我们旨在将较长的序列分割成多个较短的块时，它们显然不兼容。

为此，我们修改了`encode_plus`方法，以不进行任何裁剪或填充。

此外，特殊标记*[CLS]*和*[SEP]*分别期望出现在序列的开始和结束。由于我们将分别创建这些序列，我们也必须分别添加这些标记。

新的`encode_plus`方法如下所示：

这将返回一个包含三个键值对的字典，`input_ids`，`token_type_ids`和`attention_mask`。

我们*还*添加了`return_tensors='pt'`以从标记器返回 PyTorch 张量（而不是 Python 列表）。

### 准备块

现在我们有了标记化的张量；我们需要将其分解为不超过**510**个标记的块。我们选择 510 而不是 512，以保留两个位置用于添加我们的*[CLS]*和*[SEP]*标记。

### 分割

我们对输入 ID 和注意力掩码张量应用`split`方法（我们不需要标记类型 ID，因此可以丢弃它们）。

现在我们有三个块用于每个张量集。注意，我们需要为最后一个块添加填充，因为它不满足 BERT 所需的 512 的张量大小。

### CLS 和 SEP

接下来，我们添加序列的开始标记*[CLS]*和分隔标记*[SEP]*。为此，我们可以使用`torch.cat`函数，它将一个张量列表**拼接**在一起。

我们的标记已经是标记 ID 格式，所以我们可以参考上述特殊标记表来创建我们的*[CLS]*和*[SEP]*标记的标记 ID 版本。

由于我们要对多个张量执行此操作，我们将`torch.cat`函数放入一个循环中，并分别对每个块执行拼接。

此外，我们的注意力掩码块使用**1**而不是**101**和**102**进行连接。我们这样做是因为注意力掩码不包含*标记 ID*，而是由一组**1**和**0**组成。

注意力掩码中的零表示填充标记的位置（我们将接下来添加这些标记），由于*[CLS]*和*[SEP]*不是填充标记，因此它们用**1**表示。

### 填充

我们需要向张量块中添加填充，以确保它们满足 BERT 要求的 512 张量长度。

我们的前两个块不需要填充，因为它们已经满足了这个长度要求，但最后的块需要填充。

为了检查一个块是否需要填充，我们添加了一个检查张量长度的 if 语句。如果张量短于 512 个令牌，我们使用`torch.cat`函数进行填充。

我们应该将此语句添加到添加* [CLS]*和*[SEP]*令牌的相同 for 循环中——如果你需要帮助，我已经在文章末尾附上了完整的脚本。

### 为 BERT 重新格式化

我们有了我们的块，但现在需要将它们重新格式化为单个张量，并将其添加到 BERT 的输入字典中。

将所有张量堆叠在一起是通过`torch.stack`函数完成的。

然后，我们将这些格式化为输入字典，并将输入 ID 张量的数据类型更改为`long`，将注意力掩码张量的数据类型更改为`int`——这是 BERT 所要求的。

这样，我们的数据就准备好传递给 BERT 了！

### 做出预测

做出预测是容易的部分。我们将`input_dict`作为`**kwargs`参数传递给我们的`model`——***kwargs*允许模型将`input_ids`和`attention_mask`关键字匹配到模型中的变量。

从这里，我们可以看到每个块得到一组三个激活值。这些激活值还不是我们的输出概率。要将其转化为输出概率，我们必须对输出张量应用 softmax 函数。

最后，我们对每个类别（或列）的值进行`mean`计算，以获得最终的正面、负面或中性情感概率。

如果你想提取获胜的类别，我们可以添加一个`argmax`函数：

这样，我们就得到了对更长文本的情感预测！

我们将一段包含 1000 多个令牌的长文本分解成块，手动添加了特殊令牌，并计算了所有块的平均情感。

大多数时候，查看文本的完整长度是理解讨论主题情感的绝对必要的。我们建立了一种方法来实现这一点，并绕过了典型的文本大小限制。

如果你想查看完整的代码——可以在下面的参考文献中找到（有两个笔记本，但编号**二**包含了这里使用的确切代码）。

希望你喜欢这篇文章。如果有任何问题或建议，请通过[Twitter](https://twitter.com/jamescalam)或在下面的评论中告诉我！如果你对类似的内容感兴趣，我也会在[YouTube](https://www.youtube.com/c/jamesbriggs)上发布相关内容。

感谢阅读！

### 参考文献

[1] D. Araci，[FinBERT: Financial Sentiment Analysis with Pre-trained Language Models](https://arxiv.org/abs/1908.10063) (2019)

[Jupyter Notebook 1](https://github.com/jamescalam/transformers/blob/main/course/language_classification/03_long_text_sentiment.ipynb)

[Jupyter Notebook 2](https://github.com/jamescalam/transformers/blob/main/course/language_classification/04_window_method_in_pytorch.ipynb)

如果你想了解更多关于使用变换器进行情感分析（这次使用 TensorFlow），请查看我关于语言分类的文章：

[**使用 Bert 和 Tensorflow 构建自然语言分类器**](https://www.kdnuggets.com/2022/11/shap-explain-machine-learning-model-python.html)

[将前沿变换器模型应用于你的语言问题](https://betterprogramming.pub/build-a-natural-language-classifier-with-bert-and-tensorflow-4770d4442d41)

**个人简介：[James Briggs](https://youtube.com/c/JamesBriggs)** 是一名数据科学家，专注于自然语言处理，工作于英国伦敦的金融行业。他还是一名自由职业导师、作家和内容创作者。你可以通过电子邮件联系作者（jamescalam94@gmail.com）。

[原文](https://towardsdatascience.com/how-to-apply-transformers-to-any-length-of-text-a5601410af7f)。转载经许可。

**相关：**

+   [Hugging Face Transformers 包 – 它是什么以及如何使用](https://www.kdnuggets.com/using-hugging-face-transformers-for-emotion-detection-in-text)

+   使用 Wav2Vec 2.0 进行语音转文本

+   使用 BERT 进行主题建模

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 更多相关内容

+   [NExT-GPT 介绍：任何对任何的多模态大型语言模型](https://www.kdnuggets.com/introduction-to-nextgpt-anytoany-multimodal-large-language-model)

+   [Streaming-LLM 介绍：无限长度输入的 LLM](https://www.kdnuggets.com/introduction-to-streaming-llm-llms-for-infinite-length-inputs)

+   [SHAP：用 Python 解释任何机器学习模型](https://www.kdnuggets.com/2022/11/shap-explain-machine-learning-model-python.html)

+   [如何在没有工作经验的情况下获得数据科学领域的第一份工作](https://www.kdnuggets.com/2021/02/first-job-data-science-without-work-experience.html)

+   [在参加任何免费数据科学课程前请阅读此文](https://www.kdnuggets.com/read-this-before-you-take-any-free-data-science-course)

+   [使用 Hugging Face Transformers 进行情感检测](https://www.kdnuggets.com/using-hugging-face-transformers-for-emotion-detection-in-text)
