# 训练和可视化词向量

> 原文：[https://www.kdnuggets.com/2018/01/training-visualising-word-vectors.html](https://www.kdnuggets.com/2018/01/training-visualising-word-vectors.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由 [Priyanka Kochhar](https://github.com/priya-dwivedi) 编写，深度学习顾问**

在本教程中，我想展示如何在 TensorFlow 中实现 Skip Gram 模型，以生成你正在处理的任何文本的词向量，然后使用 TensorBoard 进行可视化。我发现这个练习非常有用：一是理解 Skip Gram 模型如何工作，二是感受这些向量在你将它们用于 CNN 或 RNN 之前捕捉的文本关系。

我在 text8 数据集上训练了一个 Skip Gram 模型，该数据集是英语维基百科文章的集合。我使用 TensorBoard 可视化了这些嵌入。TensorBoard 允许你通过使用 PCA 选择三个主轴来投影数据，从而查看整个词云。非常酷！你可以输入任何单词，它会显示其邻近词。你也可以隔离离它最近的 101 个点。

请参见下面的剪辑。

![](../Images/95e7808cb6d9ccfb709d13388288fc97.png)

你可以在我的 [Github](https://github.com/priya-dwivedi/Deep-Learning/blob/master/word2vec_skipgram/Skip-Grams-Solution.ipynb) 仓库中找到完整代码。

为了可视化训练，我还查看了与一组随机单词最接近的预测单词。在第一次迭代中，最接近的预测单词似乎非常随意，这很合理，因为所有词向量都是随机初始化的。

```py
Nearest to cost: sensationalism, adversity, ldp, durians, hennepin, expound, skylark, wolfowitz,

Nearest to engine: vdash, alloys, fsb, seafaring, tundra, frot, arsenic, invalidate,

Nearest to construction: dolphins, camels, quantifier, hellenes, accents, contemporary, colm, cyprian,

Nearest to http: internally, chaffee, avoid, oilers, mystic, chappell, vascones, cruciger,
```

到训练结束时，模型在发现单词之间的关系方面变得更为出色。

```py
Nearest to cost: expense, expensive, purchase, technologies, inconsistent, part, dollars, commercial,

Nearest to engine: engines, combustion, piston, stroke, exhaust, cylinder, jet, thrust,

Nearest to construction: completed, constructed, bridge, tourism, built, materials, building, designed,

Nearest to http: www, htm, com, edu, html, org, php, ac,
```

### Word2Vec 和 Skip Gram 模型

创建词向量是将大量文本语料库中的每个单词创建一个向量的过程，使得在语料库中共享相同上下文的单词在向量空间中彼此接近。

这些词向量在捕捉单词之间的上下文关系方面表现得非常好（例如，黑色、白色和红色的向量会非常接近），我们在使用这些向量而不是原始单词进行自然语言处理任务（如文本分类或新文本生成）时，性能会更好。

生成这些词向量主要有两种模型——连续词袋模型（CBOW）和 Skip Gram 模型。CBOW 模型试图根据上下文单词预测中心单词，而 Skip Gram 模型试图根据中心单词预测上下文单词。一个简化的例子是：

CBOW: 猫吃了____。填补空白，在这种情况下，是“食物”。

Skip-gram: ___ ___ ___ 食物。填补词的上下文。在这种情况下，是“猫吃了”

如果你对这两种方法的详细比较感兴趣，请参见这个 [链接](https://iksinc.wordpress.com/tag/continuous-bag-of-words-cbow/)。

各种论文发现 Skip Gram 模型生成的词向量更好，因此我专注于实现这一点。

### 在 Tensorflow 中实现 Skip Gram 模型

在这里，我将列出构建模型的主要步骤。详细的实现请参见我的[Github](https://github.com/priya-dwivedi/Deep-Learning/blob/master/word2vec_skipgram/Skip-Grams-Solution.ipynb)。

1\. 数据预处理

我们首先清理数据。去除任何标点符号、数字，并将文本拆分为单独的词。由于程序对整数的处理比对词的处理更好，我们将每个词映射到一个整数，通过创建一个词到整数的字典。下面是代码。

```py
counts = collections.Counter(words)
vocab = sorted(counts, key=counts.get, reverse=True)
vocab_to_int = {word: ii for ii, word in enumerate(vocab, 0)}
```

2\. 子采样

像“the”、“of”和“for”这样的常见词对附近的词没有提供太多上下文。如果我们丢弃一些这样的词，我们可以去除数据中的一些噪声，从而获得更快的训练速度和更好的表示。这个过程被称为由[Mikolov](https://arxiv.org/pdf/1301.3781.pdf)提出的子采样。对于训练集中每个词，我们将以其频率的倒数给出的概率丢弃它。

3\. 创建输入和目标

Skip Gram的输入是每个词（编码为整数），目标是该窗口周围的词。Mikolov等人发现，如果这个窗口的大小是可变的，并且离中心词较近的词被更频繁地采样，性能会更好。

“由于距离较远的词通常与当前词的相关性不如靠近的词，因此我们通过在训练示例中减少从这些词中采样的频率来给予远离词较少的权重……如果我们选择窗口大小=5，对于每个训练词，我们将随机选择一个在1到窗口大小之间的数字R，然后使用当前词的历史中R个词和未来中R个词作为正确标签。”

```py
 R = np.random.randint(1, window_size+1)
 start = idx — R if (idx — R) > 0 else 0
 stop = idx + R
 target_words = set(words[start:idx] + words[idx+1:stop+1])
```

4\. 构建模型

从[Chris McCormick 的博客](http://mccormickml.com/2016/04/19/word2vec-tutorial-the-skip-gram-model/)中，我们可以看到我们将要构建的网络的一般结构。

![](../Images/968c128d8ca16ad61a212372dd92165f.png)

我们将把输入词“ants”表示为一个独热向量。这个向量将有10,000个分量（每个词汇表中的一个），我们将在对应“ants”词的位置上放置一个“1”，其他位置上放置“0”。

网络的输出是一个包含10,000个分量的单一向量，其中包含我们词汇表中每个词的概率，即随机选择的附近词是该词汇表词的概率。

在训练结束时，隐藏层将拥有训练好的词向量。隐藏层的大小对应于我们向量的维度数量。在上述示例中，每个词将具有长度为300的向量。

你可能已经注意到，skip-gram神经网络包含大量权重……以我们300个特征和10,000个单词的词汇表为例，每个隐藏层和输出层都有300万权重！在大型数据集上训练将非常困难，因此word2vec的作者引入了一些调整以使训练成为可能。你可以在 [链接](http://mccormickml.com/2017/01/11/word2vec-tutorial-part-2-negative-sampling/) 中阅读更多内容。 [Github](https://github.com/priya-dwivedi/Deep-Learning/blob/master/word2vec_skipgram/Skip-Grams-Solution.ipynb) 上的代码实现了这些调整以加速训练。

5\. 使用Tensorboard进行可视化

你可以使用Tensorboard中的嵌入投影器来可视化嵌入。为此，你需要做几件事：

+   在训练结束时将模型保存在检查点目录中

+   创建一个metadata.tsv文件，其中包含每个整数到单词的映射，以便Tensorboard显示单词而不是整数。将此tsv文件保存在相同的检查点目录中

+   运行这段代码：

```py
from tensorflow.contrib.tensorboard.plugins import projector
summary_writer = tf.summary.FileWriter(‘checkpoints’, sess.graph)
config = projector.ProjectorConfig()
embedding_conf = config.embeddings.add()
# embedding_conf.tensor_name = ‘embedding:0’
embedding_conf.metadata_path = os.path.join(‘checkpoints’, ‘metadata.tsv’)
projector.visualize_embeddings(summary_writer, config)
```

+   通过将Tensorboard指向检查点目录来打开Tensorboard

就这些了！

如果你喜欢这篇文章，请给我一个❤️:) 希望你能拉取代码并自己尝试。如果你对此话题有其他想法，请在这篇文章下评论或通过 [priya.toronto3@gmail.com](mailto:priya.toronto3@gmail.com) 联系我。

**其他文章**：[ https://medium.com/@priya.dwivedi/](https://medium.com/@priya.dwivedi/)

PS：我有一家深度学习咨询公司，喜欢处理有趣的问题。如果你有一个我们可以合作的项目，请通过 [priya.toronto3@gmail.com](mailto:priya.toronto3@gmail.com) 联系我。

**参考文献：**

+   [Udacity](https://www.udacity.com/) 深度学习纳米学位

**简介：[Priyanka Kochhar](https://github.com/priya-dwivedi)** 已从事数据科学工作超过10年。她现在有自己的深度学习咨询公司，喜欢处理有趣的问题。她帮助多个初创公司部署创新的AI解决方案。如果你有一个她可以合作的项目，请通过 [priya.toronto3@gmail.com](mailto:priya.toronto3@gmail.com) 联系她。

[原文](https://towardsdatascience.com/training-and-visualising-word-vectors-2f946c6430f8)。经许可转载。

**相关：**

+   [接近文本数据科学任务的框架](/2017/11/framework-approaching-textual-data-tasks.html)

+   [文本数据预处理的一般方法](/2017/12/general-approach-preprocessing-text-data.html)

+   [超越Word2Vec仅用于词语](/2018/01/beyond-word2vec-for-words.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 工作

* * *

### 更多相关话题

+   [使用 Python 自动化 Microsoft Excel 和 Word](https://www.kdnuggets.com/2021/08/automate-microsoft-excel-word-python.html)

+   [NLP 中不同词嵌入技术的终极指南](https://www.kdnuggets.com/2021/11/guide-word-embedding-techniques-nlp.html)

+   [使用 TensorFlow 和 Keras 构建并训练你的第一个神经网络](https://www.kdnuggets.com/2023/05/building-training-first-neural-network-tensorflow-keras.html)

+   [与 Nvidia 的在线培训和研讨会](https://www.kdnuggets.com/2022/07/online-training-workshops-nvidia.html)

+   [机器学习中训练数据和测试数据的区别](https://www.kdnuggets.com/2022/08/difference-training-testing-data-machine-learning.html)

+   [来自 Anaconda 的新动态！数据科学培训和云托管笔记本](https://www.kdnuggets.com/2022/11/anaconda-new-anaconda-data-science-training-cloud-hosted-notebooks.html)
