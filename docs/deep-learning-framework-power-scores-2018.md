# 深度学习框架实力评分 2018

> 原文：[https://www.kdnuggets.com/2018/09/deep-learning-framework-power-scores-2018.html](https://www.kdnuggets.com/2018/09/deep-learning-framework-power-scores-2018.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**[Jeff Hale](https://www.linkedin.com/in/jeff-hale-99a7877/)，企业家**

深度学习仍然是数据科学中最热门的领域。深度学习框架正在快速变化。就在五年前，除了 Theano 之外，没有任何一个领导者存在。

我希望找到哪些框架值得关注的证据，因此我开发了这个实力排名。我使用了 11 个数据源，涵盖了 7 个不同的类别，以评估框架的使用、兴趣和受欢迎程度。然后，我在[这个 Kaggle Kernel](https://www.kaggle.com/discdiver/deep-learning-framework-power-scores-2018)中加权和合并了这些数据。

**更新 2018 年 9 月 20 日**：由于需求旺盛，我扩大了评估的框架，包括 Caffe、Deeplearning4J、Caffe2 和 Chainer。现在所有在 KDNuggets 使用调查中报告使用率超过 1% 的深度学习框架都被包含在内。

**更新 2018 年 9 月 21 日**：我在多个指标中进行了若干方法改进。

不再赘述，以下是深度学习框架的实力评分：

![](../Images/146fdb09129cfbe47dcdeb459d368dc2.png)

虽然 TensorFlow 是明显的赢家，但也有一些令人惊讶的发现。让我们深入了解一下！

### 竞争者

所有这些框架都是开源的。除了一个框架，其他框架都可以与 Python 一起使用，并且有些[可以与 R 一起使用](https://tensorflow.rstudio.com/)或其他语言。

![](../Images/023402a1bbacec05ec37ac11b98ab100.png)

[TensorFlow](https://www.tensorflow.org/)是无可争议的重量级冠军。它在 GitHub 活跃度、Google 搜索量、Medium 文章、Amazon 书籍和 ArXiv 文章中都是最多的。它也拥有最多的开发者使用，并且在在线招聘广告中出现频率最高。TensorFlow 得到了 Google 的支持。

![](../Images/ed17a7eb7e96b6f1636e449c4988ec84.png)

[Keras](https://kerasi.io/)有一个“为人类而设计的 API，而非机器”。它几乎在所有评估领域中都是第二受欢迎的框架。[Keras](https://keras.io/)建立在 TensorFlow、Theano 或 CNTK 之上。如果你对深度学习很陌生，建议从 Keras 开始。

![](../Images/3d4de4f8393e9d4b77420987c306bd6f.png)

[PyTorch](https://pytorch.org/)是第三大最受欢迎的框架，也是第二大最受欢迎的独立框架。它比 TensorFlow 更年轻，并且在受欢迎程度上迅速增长。它允许 TensorFlow 不具备的自定义功能。它得到了 Facebook 的支持。

![](../Images/3a2c9a555710618e8ea42cd82825b360.png)

[Caffe](http://caffe.berkeleyvision.org/)是第四大最受欢迎的框架。它已经存在近五年。它在雇主中相对有需求，并且经常出现在学术文章中，但最近的使用情况报道较少。

![](../Images/18efe380df4095c18bb18c75d4a1efb6.png)

[Theano](http://www.deeplearning.net/software/theano/)是2007年在蒙特利尔大学开发的，是最早的重大Python深度学习框架之一。它已经失去了很多人气，其领导者[声明](https://en.wikipedia.org/wiki/Theano_%28software%29)主要版本不再在计划中。然而，更新仍在进行中。Theano仍然是第五高分的框架。

![](../Images/54c53dc3ef216f66180337f9a5f92e14.png)

[MXNET](https://mxnet.apache.org/)由Apache孵化并被Amazon使用。它是第六大受欢迎的深度学习库。

![](../Images/6124f62fa0806920e544e15aedec1f92.png)

[CNTK](https://github.com/Microsoft/CNTK)是微软认知工具包。它让我想起了许多其他微软产品，因为它试图与Google和Facebook的产品竞争，但并未获得显著的采用。

![](../Images/905583486a4eb8313be921319919f3b0.png)

[Deeplearning4J](https://deeplearning4j.org/)，也称为DL4J，是用于Java语言的。它是唯一一个没有Python版本的半流行框架。然而，你可以将用Keras编写的模型导入到DL4J中。这是唯一一个不同搜索词偶尔会有不同结果的框架。我使用了每项指标的较高数字。由于框架得分相当低，这没有实质性区别。

![](../Images/cdf1af8a99f4250245080a58c6fe6c7c.png)

[Caffe2](https://caffe2.ai/)是另一个Facebook的开源产品。它基于Caffe，现在被托管在PyTorch的GitHub存储库中。由于它不再拥有自己的存储库，我使用了其旧存储库的GitHub数据。

![](../Images/ab77c90f160e71872ab0fefd98b13aa3.png)

[Chainer](https://chainer.org/)是由日本公司Preferred Networks开发的框架。它有一个小众的追随者。

![](../Images/fcbd068a392d3656956a5c0f4a416629.png)

[FastAI](http://www.fast.ai/)基于PyTorch构建。其API受Keras的启发，所需代码更少，效果却很强。FastAI截至2018年9月中旬处于前沿阶段，正在进行1.0版本的重写，预计于2018年10月发布。Jeremy Howard，FastAI的幕后推手，是顶尖的Kaggler和[Kaggle](https://www.kaggle.com/)的主席。他讨论了为何FastAI从Keras转向自己构建的框架，[点击这里](http://www.fast.ai/2017/09/08/introducing-pytorch-for-fastai/)了解更多。

FastAI目前尚未在职业中广泛需求，也未被广泛使用。然而，通过其受欢迎的[免费在线课程](http://course.fast.ai/)，它拥有大量内置用户。它既强大又易于使用，未来的采用率可能会显著增长。

### 标准

我选择了以下类别，以提供对深度学习框架的受欢迎程度和兴趣的全面视角。

评估类别：

+   在线职位列表

+   KDnuggets 使用调查

+   谷歌搜索量

+   Medium 文章

+   亚马逊书籍

+   ArXiv 文章

+   GitHub 活动

搜索是在 2018 年 9 月 16 日至 9 月 21 日进行的。源数据在 [这个 Google 表格](https://docs.google.com/spreadsheets/d/1mYfHMZfuXGpZ0ggBVDot3SJMU-VsCsEGceEL8xd1QBo/edit?usp=sharing)中。

我使用了 plotly 数据可视化库和 Python 的 pandas 库来探讨受欢迎程度。有关互动式 plotly 图表，请参见我的 Kaggle Kernel [这里](https://www.kaggle.com/discdiver/deep-learning-framework-power-scores-2018)。

### 在线职位列表

今天的职位市场上哪些深度学习库最受需求？我在 LinkedIn、Indeed、Simply Hired、Monster 和 Angel List 上搜索了职位列表。

![](../Images/0030814c624f9510c1d5eb81223b4cfd.png)

在职位列表中提到的框架中，TensorFlow 无疑是赢家。如果你想找一份做深度学习的工作，就学习它吧。

我使用了术语*机器学习*，然后是库名称。因此，TensorFlow 是通过*机器学习 TensorFlow*来评估的。我测试了几种搜索方法，这种方法给出了最相关的结果。

需要一个额外的关键词来区分这些框架和无关的术语，因为 Caffe 可能有多种含义。

### 使用情况

[KDnuggets](/2018/05/poll-tools-analytics-data-science-machine-learning-results.html/2)，一个流行的数据科学网站，对全球的数据科学家进行了调查，了解他们使用的软件。他们询问了：

> 你在过去 12 个月中在实际项目中使用了哪些分析、大数据、数据科学、机器学习软件？

这里是这一类别框架的结果。

![](../Images/1196d297227f3c1afe8d8d89e908ffa3.png)

Keras 的使用量出乎意料地多——几乎和 TensorFlow 一样。很有趣的是，美国雇主显然在寻找 TensorFlow 技能，而在国际上，Keras 的使用频率几乎是一样的。

这一类别是唯一包含国际数据的类别，因为包含其他类别的国际数据会显得繁琐。

KDnuggets 报告了多年的数据。虽然我在这项分析中仅使用了 2018 年的数据，但我应当指出，自 2017 年以来，Caffe、Theano、MXNET 和 CNTK 的使用量都有所下降。

### 谷歌搜索活动

在最大的搜索引擎上的网页搜索是受欢迎程度的良好指标。我查看了过去一年在 Google Trends 上的搜索历史。谷歌没有提供绝对的搜索数量，但提供了相对数据。

![](../Images/439bdfef18cc16dc0835ab7aabfea3d2.png)

我于 2018 年 9 月 21 日更新了这篇文章，因此这些分数包括了截至 2018 年 9 月 15 日的一周内全球搜索的*机器学习与人工智能*类别数据。感谢 François Chollet 提出的建议，改进了这个搜索指标。

Keras 与 TensorFlow 相差不远。PyTorch 排在第三位，其他框架的相对搜索量得分在四分之一或以下。这些得分用于功率得分计算。

让我们简要看看搜索量随时间的变化，以提供更多的历史背景。下方的图表来自 Google，展示了过去两年的搜索情况。

![](../Images/017ca970bf989ffe6bbd3c3c8eb0cf28.png)

TensorFlow = 红色，Keras = 黄色，PyTorch = 蓝色，Caffe = 绿色

过去一年中，Tensor Flow 的搜索量并没有明显增长，但 Keras 和 PyTorch 的搜索量有所增加。Google Trends 仅允许同时比较五个术语，因此其他库在单独的图表中进行比较。除了 TensorFlow 外，其他库的搜索兴趣都很有限。

### 出版物

我在评分中包括了几种出版类型。让我们先来看 Medium 文章。

### Medium 文章

Medium 是流行的数据科学文章和指南的发布地。现在你在这里——**太棒了**！

![](../Images/61126fc7aa85d571112972dc3daa5ba0.png)

最终出现了一个新的赢家。在 Medium 文章中的提及方面，Keras 超过了 TensorFlow。FastAI 的表现相较于平时也有所提升。

我假设这些结果可能是因为 Keras 和 FastAI 对初学者友好。它们受到了新手深度学习从业者的相当多关注，而 Medium 通常是教程的论坛。

我在过去 12 个月中使用框架名称和“learning”作为关键词在 Medium.com 上进行 Google 站点搜索。这个方法是为了防止“caffe”一词出现错误结果。它在几种搜索选项中减少的文章数量最小。

现在让我们看看哪些框架在亚马逊上有相关书籍。

### 亚马逊书籍

我在 Amazon.com 上的 *Books*->*Computers & Technology* 分类下搜索了每个深度学习框架。

![](../Images/485e97815f8e7dd5cd0d3b5067ef624c.png)

TensorFlow 再次获胜。MXNET 的书籍数量超出了预期，而 Theano 的书籍数量则较少。PyTorch 的书籍相对较少，但这可能是因为该框架较年轻。这项指标偏向于较老的库，因为出版一本书需要时间。

### ArXiv 文章

[ArXiv](https://arxiv.org/) 是大多数学术机器学习文章发布的在线存储库。我使用 Google 站点搜索结果，搜索了过去 12 个月每个框架在 arXiv 上的情况。

![](../Images/fe4b6aeb7e3c32b158987be9a66039ca.png)

TensorFlow 在学术文章中继续保持相同的表现。注意 Keras 在 Medium 和亚马逊上的受欢迎程度远高于学术文章中的表现。PyTorch 在这一类别中位居第二，展示了其在实现新想法方面的灵活性。Caffe 的表现也相对较好。

### GitHub 活动

GitHub 活动是框架受欢迎程度的另一个指标。我在下面的图表中分别列出了 stars、forks、watchers 和 contributors，因为它们分开来看比合在一起更有意义。

![](../Images/01e5c8eac51ce97e7b79b1f1646201c2.png)

TensorFlow 显然是 GitHub 上最受欢迎的框架，拥有大量活跃用户。考虑到 FastAI 甚至还不到一年，它有一个相当不错的追随者。值得注意的是，贡献者水平在所有框架中更接近，而不是其他三个指标。

在收集和分析数据之后，到了将其整合成一个指标的时刻。

### 权力评分程序

这是我创建权力评分的方法：

1.  将所有特征缩放到 0 和 1 之间。

1.  汇总的职位搜索列表和 GitHub 活动子类别。

1.  根据以下权重对类别进行加权。

![](../Images/3b23cbf5d3a7de5a4e7a223042a602a8.png)

如上所示，在线职位列表和 KDnuggets 使用调查占总分的一半，而网络搜索、出版物和 GitHub 关注度占另一半。这种分配似乎是各种类别中最合适的平衡。

1.  将加权得分乘以 100 以便于理解。

1.  将每个框架的总类别得分汇总成一个总体评分。

这是数据：

![](../Images/2cdbc240801c8b7d0a245edf717bbbb2.png)

以下是加权和汇总子类别后的得分。

![](../Images/89d219c168c670b2821c11ded89a100f.png)

这里再次展示了最终权力评分的漂亮图表。

![](../Images/146fdb09129cfbe47dcdeb459d368dc2.png)

100 是最高可能得分，表示在每个类别中都是第一名。TensorFlow 几乎达到了 100，这在看到它在每个类别中都接近或位于顶端之后并不令人惊讶。Keras 排在第二位。

要互动地查看图表或叉开 Jupyter Notebook，请访问 [这个 Kaggle Kernel](https://www.kaggle.com/discdiver/deep-learning-framework-power-scores-2018)。

### 未来

目前，TensorFlow 牢牢占据领先地位。在短期内它似乎会继续主导。不过，鉴于深度学习领域的变化如此之快，这种情况可能会改变。

时间会证明 PyTorch 是否会超越 TensorFlow，就像 React 超越 Angular 一样。这些框架可能是类比的。PyTorch 和 React 都是由 Facebook 支持的灵活框架，通常被认为比其由 Google 支持的竞争对手更容易使用。

FastAI 会在其课程之外获得用户吗？它有一个庞大的学生群体，API 比 Keras 更加适合初学者。

你认为未来会怎样？请在下方分享你的想法。

### 学习者建议

如果你考虑学习这些框架之一，并且拥有 Python、numpy、pandas、sklearn 和 matplotlib 技能，我建议你从 Keras 开始。它有大量用户基础，受到雇主的需求，Medium 上有很多相关文章，而且 API 使用起来很简单。

如果你已经了解了 Keras，它可能会很难决定下一个学习的框架。我建议你选择 TensorFlow 或 PyTorch 并好好学习，以便你可以创建出色的深度学习模型。

TensorFlow显然是你想掌握市场需求框架的最佳选择。但PyTorch的易用性和灵活性使其在研究人员中越来越受欢迎。这是一个[Quora讨论](https://www.quora.com/Should-I-go-for-TensorFlow-or-PyTorch)关于这两个框架的对比。

一旦你掌握了这些框架，我建议你关注FastAI。如果你想学习基础和高级深度学习技能，可以查看它的[免费在线课程](http://course.fast.ai/)。FastAI 1.0承诺让你轻松实现最新的深度学习策略，并迅速迭代。

无论你选择哪个框架，希望你现在对哪些深度学习框架最受需求、最常用以及最被讨论有了更好的理解。

### 如果你觉得这篇文章有趣或有帮助，请通过分享和点赞帮助其他人发现它。

### 祝深度学习愉快！

**个人简介: [Jeff Hale](https://www.linkedin.com/in/jeff-hale-99a7877/)** ([**@discdiver**](https://twitter.com/@discdiver)) 是一位经验丰富的企业家和经理，拥有MBA和硕士学位。他专注于数据科学，并撰写关于机器学习、数据可视化和深度学习的文章。[在LinkedIn上与他联系](https://www.linkedin.com/in/jeff-hale-99a7877/)。

[原文](https://towardsdatascience.com/deep-learning-framework-power-scores-2018-23607ddf297a)。经许可转载。

**相关：**

+   [你应该知道的9件关于TensorFlow的事](/2018/08/9-things-tensorflow.html)

+   [Keras四步工作流程](/2018/06/keras-4-step-workflow.html)

+   [fast.ai 深度学习第一部分完整课程笔记](/2018/07/fast-ai-deep-learning-part-1-notes.html)

* * *

## 我们的前3个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT

* * *

### 更多相关主题

+   [解锁AI的力量 - KDnuggets和Machine的特别发布…](https://www.kdnuggets.com/2023/07/mlm-unlock-power-ai-special-release-kdnuggets-machine-learning-mastery.html)

+   [AI/ML模型的风险管理框架](https://www.kdnuggets.com/2022/03/risk-management-framework-aiml-models.html)

+   [Django框架中的社交用户认证](https://www.kdnuggets.com/2023/01/social-user-authentication-django-framework.html)

+   [适用于所有用途的唯一提示框架](https://www.kdnuggets.com/the-only-prompting-framework-for-every-use)

+   [探索GPT-4的力量和局限性](https://www.kdnuggets.com/2023/07/exploring-power-limitations-gpt4.html)

+   [利用数据力量的三步法](https://www.kdnuggets.com/2022/05/3-steps-harnessing-power-data.html)
