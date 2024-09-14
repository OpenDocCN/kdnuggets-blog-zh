# 使用 RNN 生成文本的 4 行代码

> 原文：[https://www.kdnuggets.com/2018/06/generating-text-rnn-4-lines-code.html](https://www.kdnuggets.com/2018/06/generating-text-rnn-4-lines-code.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

生成文本是一个对机器学习和自然语言处理初学者来说似乎很有趣的项目，但它也相当令人畏惧。或者，至少对我来说是这样。

幸运的是，在线有各种优秀的材料可以学习如何使用 RNN 生成文本，这些材料涵盖了从理论到技术深度的内容，也有专注于实际应用的内容。其中一些非常好的文章 [涵盖了所有内容](http://karpathy.github.io/2015/05/21/rnn-effectiveness/) ，现已被认为是这一领域的经典。所有这些材料有一个共同点：在某个阶段，你必须构建和调整一个 RNN 来完成工作。

虽然这是一个显然值得做的任务，特别是为了学习，但如果你对更高层次的抽象感到满意呢，无论你出于什么原因？如果你是一个数据科学家，需要一个以 RNN 文本生成器形式存在的构建块来插入你的项目呢？或者，作为一个新手，你只是想稍微动手一下——但不要太深入——以测试水域或作为进一步挖掘的动机呢？

在这个背景下，让我们看看 [textgenrnn](https://github.com/minimaxir/textgenrnn) 项目，它允许你“轻松训练自己生成文本的神经网络，无论大小和复杂度，使用几行代码即可。” textgenrnn 的作者是 Max Woolf，他是 BuzzFeed 的副数据科学家，曾是 Apple 软件 QA 工程师。

textgenrnn 基于 Keras 和 TensorFlow，可以用于生成字符级和词级文本（字符级是默认的）。网络架构使用了注意力加权和跳跃嵌入来加速训练和提高质量，并允许调整多个超参数，如 RNN 大小、RNN 层数以及是否包含双向 RNN。你可以在其 [Github 仓库](https://github.com/minimaxir/textgenrnn) 或这篇 [介绍博客文章](http://minimaxir.com/2018/05/text-neural-networks/) 中阅读更多关于 textgenrnn 及其功能和架构的内容。

![图片](../Images/17a900e37b077effb7be6c76c3d7527a.png)

由于文本生成的“Hello, World!”（至少在我看来）似乎是生成特朗普的推文，我们就用这个吧。textgenrnn 的默认预训练模型可以很容易地在新文本上进行训练——虽然你也可以使用 textgenrnn 训练一个新模型（只需将 new_model=True 添加到其任何训练函数中）——由于我们想要快速生成推文，我们就这样做吧。

### 获取数据

我从[特朗普推特档案](http://www.trumptwitterarchive.com/archive)中获取了一些唐纳德·特朗普的推文——2014年1月1日至2018年6月11日（在撰写时的前一天），这明显包括了他作为美国总统就职前后的推文。这个网站可以轻松查询和下载总统的推文。我只提取了该日期范围内的推文文本，因为我不关心任何元数据，并将其保存到一个名为`trump-tweets.txt`的文本文件中。

[![图片](../Images/05ded0f38c126d07de8ca31bff7f3f09.png)](https://image.ibb.co/mdzied/trump_tweets_archive.jpg)

### 训练模型

让我们看看使用 textgenrnn 生成文本有多简单。以下4行代码是我们需要的所有内容，用于导入库、创建文本生成对象、在`trump-tweets.txt`文件上训练模型10个周期，然后生成一些示例推文。

```py
from textgenrnn import textgenrnn
textgen = textgenrnn()
textgen.train_from_file('trump-tweets.txt', num_epochs=10)
textgen.generate(5)

```

大约30分钟后，以下是生成的内容（在第10个周期）：

```py

My @FoxNews will be self finally complaining about me that so he is a great day and companies and is starting to report the president in safety and more than any mention of the bail of the underaches to the construction and freedom and efforts the politicians and expensive meetings should have bee

The world will be interviewed on @foxandfriends at 7:30pm. Enjoy!

.@JebBush and Fake News Media is a major place in the White House in the service and sense where the people of the debate and his show of many people who is a great press considering the GREAT job on the way to the U.S. A the best and people in the biggest! Thank you!

New Hampshire Trump Int'l Hotel Leadership Barrier Lou Clinton is a forever person politically record supporters have really beginning in the media on the heart of the bad and women who have been succeeded and before you can also work the people are there a time strong and send out the world with 

Join me in Maryland at 7:00 A.M. and happened to the WALL and be true the longer of the same sign into the Fake News Media will be a great honor to serve that the Republican Party will be a great legal rate the media with the Best Republican Party and the American people that will be the bill by a
```

撇开政治不谈，鉴于我们只使用了约12K条推文进行仅10个周期的训练，这些生成的推文并不……糟糕。想试试[温度](https://www.quora.com/What-is-Temperature-in-LSTM)（textgenrnn 的默认值为0.5）来生成一些更具创意的推文吗？我们来试试吧：

```py
textgen.generate(5, temperature=0.9)

```

```py

“Via-can see this Democrats were the opening at GREAT ENSUS CALL!

.@GovSeptorald Taster is got to that the subcent Vote waiting them. @Calkers

Major President Obama will listen for the disaster!

Grateful and South Carolina so his real ability and much better-- or big crisis on many signing!

It is absolutely dumbers for well tonight. Love us in the great inherition of fast. With bill of badly to forget the greatest puppet at my wedds. No Turnberry is "bigger.” - Al

```

嗯，这说服力稍差。怎么样，我们试试一些更保守的，这样模型会更有信心的：

```py
textgen.generate(5, temperature=0.1)

```

```py

The Fake News Media is a great people of the president was a great people of the many people who would be a great people of the president was a big crowd of the statement of the media is a great people of the people of the statement of the people of the people of the world with the statement of th

Thank you @TrumpTowerNY #Trump2016 https://t.co/25551R58350

Thank you for your support! #Trump2016 https://t.co/7eN53P55c

The people of the U.S. has been a great people of the presidential country is a great time and the best thing that the people of the statement of the media is the people of the state of the best thing that the people of the statement of the statement of the problem in the problem and success and t

Thank you @TheBrodyFile tonight at 8:00 A.M. Enjoy!
```

现在，其中一些看起来更易读了。

当然，这并不完美。我们可以尝试各种其他方法，幸运的是，如果你不想实现自己的解决方案，textgenrnn 可以用来执行许多这些操作（再次参见[Github 仓库](https://github.com/minimaxir/textgenrnn)）：

+   从零开始训练我们自己的模型

+   使用更多的样本数据进行更多次迭代训练

+   调整其他超参数

+   对数据进行一些预处理（至少要去除虚假的网址）

有点有趣。我很想看看一个默认的 textgenrnn 模型在开箱即用时与一个定制的、经过良好调整的模型相比表现如何。也许下次可以尝试一下。

**相关**：

+   [5个你不应该忽视的机器学习项目，2018年6月](/2018/06/5-machine-learning-projects-overlook-jun-2018.html)

+   [使用 spaCy 进行自然语言处理入门](/2018/05/getting-started-spacy-natural-language-processing.html)

+   [了解名人最常发布什么内容](/2017/10/what-celebrities-tweet-about-most.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 加快你在网络安全领域的职业发展。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的IT需求

* * *

### 更多相关内容

+   [停止学习数据科学以寻找目标，并找到目标再……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [9亿美元的AI失败，深入剖析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [学习数据科学统计学的最佳资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的5个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么让Python成为初创企业理想的编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每位数据科学家都应了解的三个R库（即使你使用Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
