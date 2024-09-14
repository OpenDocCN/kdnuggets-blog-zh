# 用 Python 挖掘 Twitter 数据 第四部分：橄榄球和术语共现

> 原文：[`www.kdnuggets.com/2016/06/mining-twitter-data-python-part-4.html`](https://www.kdnuggets.com/2016/06/mining-twitter-data-python-part-4.html)

**作者：Marco Bonzanini，独立数据科学顾问**。

上周六（*编者注：已经过去一段时间*）是[六国锦标赛](https://en.wikipedia.org/wiki/Six_Nations_Championship)的闭幕日，这是一个年度国际[橄榄球](https://en.wikipedia.org/wiki/Rugby_union)比赛。在打开电视观看意大利被威尔士击败之前，我决定利用这个事件从 Twitter 收集一些数据，并对比我的小列表的推文进行一些更有趣的探索性文本分析。

本文继续 Twitter 数据挖掘教程，重用我们在之前文章中讨论的内容，并引入更具现实的数据。它还通过引入术语共现的概念来扩展分析。

![Twitter](img/3da5b4dea824ea453ca3ae25f3548634.png)

### 应用领域

正如名称所示，比赛涉及六支队伍：英格兰、爱尔兰、威尔士、苏格兰、法国和意大利。这意味着我们可以预计比赛会用多种语言（英语、法语、意大利语、威尔士语、盖尔语，可能还有其他语言）进行推文，其中英语是主要语言。假设队伍名称会被频繁提及，我们可以决定也查找它们的昵称，例如法国的*Les Bleus*或意大利的*Azzurri*。在比赛的最后一天，三场比赛依次进行。特别是三支队伍有机会争夺冠军：英格兰、爱尔兰和威尔士。最后，爱尔兰赢得了比赛，但一切都悬而未决直到最后一刻。

### 设置

我使用了流式 API 下载了当天包含字符串`#rbs6nations`的所有推文。显然，并非所有关于此事件的推文都包含该标签，但这是一个很好的基准。下载时间范围是从大约 12:15PM 到 7:15PM GMT，即从第一场比赛前大约 15 分钟，到最后一场比赛结束后大约 15 分钟。最后，下载了超过 18,000 条推文，JSON 格式，总数据约为 75Mb。这应该足够小以便在内存中快速处理，同时又足够大以观察到一些可能有趣的内容。

推文的文本内容已经通过使用教程第二部分中介绍的`preprocess()`函数进行了分词和小写处理。

### 有趣的术语和标签

根据我们在第三部分（术语频率）中讨论的内容，我们希望观察一天中使用的最常见的术语和标签。如果你已经跟随了关于创建不同词项列表的讨论，以捕捉没有标签的术语、仅标签、去除停用词等，你可以在不同列表中进行实验。

这是数据集中最常见的前 10 个术语（`terms_only`在第三部分）列表，这一点并不令人惊讶。

```py
[('ireland', 3163), ('england', 2584), ('wales', 2271), 
  ('', 2068), ('day', 1479), ('france', 1380), ('win', 1338), 
  ('rugby', 1253), ('points', 1221), ('title', 1180)]

```

前三个术语对应于争夺冠军的球队。频率也尊重最终表中的顺序。第四个术语则是一个我们遗漏的标点符号，未包含在停用词列表中。这是因为`string.punctuation`只包含 ASCII 符号，而这里涉及的是一个 unicode 字符。如果我们深入数据，将会有更多类似的例子，但目前我们不需要担心这个问题。

在将省略号符号添加到停用词列表后，我们在列表末尾有了一个新的条目：

```py
[('ireland', 3163), ('england', 2584), ('wales', 2271), 
  ('day', 1479), ('france', 1380), ('win', 1338), ('rugby', 1253), 
  ('points', 1221), ('title', 1180), ('__SHAMROCK_SYMBOL__', 1154)]

```

有趣的是，我们没有考虑到的新词项是一个[表情符号](https://en.wikipedia.org/wiki/Emoji)（在这种情况下是[爱尔兰三叶草](https://en.wikipedia.org/wiki/Shamrock)）。

如果我们查看最常见的标签，我们需要考虑到`#rbs6nations`将是最常见的词项（这是我们下载推文的搜索词），因此我们可以将其从列表中排除。这给我们留下了：

```py
[('#engvfra', 1701), ('#itavwal', 927), ('#rugby', 880), 
  ('#scovire', 692), ('#ireland', 686), ('#angfra', 554), 
  ('#xvdefrance', 508), ('#crunch', 500), ('#wales', 446), 
  ('#england', 406)]

```

我们可以观察到，除了`#rugby`之外，最常见的标签与个别比赛相关。特别是英格兰对法国的比赛收到了最多的提及，可能是因为这是当天最后一场比赛，结局非常戏剧化。值得注意的是，相当多的推文还包含了法语术语：`#angfra`的计数实际上应该添加到`#engvfra`中。对橄榄球不太熟悉的人可能不会意识到`#crunch`也应该与`#EngvFra`比赛一起包含，因为*Le Crunch*是该事件的传统名称。因此，到目前为止，最后一场比赛吸引了大量关注。

### 术语共现

有时我们对同时出现的术语感兴趣。这主要是因为*上下文*能让我们更好地理解术语的含义，支持诸如词义消歧或语义相似性等应用。我们讨论了使用*bigrams*的选项在上一篇文章中，但我们想将术语的上下文扩展到整个推文中。

我们可以重构来自上一篇文章的代码，以捕捉共现情况。我们构建了一个共现矩阵`com`，使得`com[x][y]`包含术语`x`在同一推文中出现与术语`y`的次数：

```py
from collections import defaultdict
# remember to include the other import from the previous post

com = defaultdict(lambda : defaultdict(int))

# f is the file pointer to the JSON data set
for line in f: 
    tweet = json.loads(line)
    terms_only = [term for term in preprocess(tweet['text']) 
                  if term not in stop 
                  and not term.startswith(('#', '@'))]

    # Build co-occurrence matrix
    for i in range(len(terms_only)-1):            
        for j in range(i+1, len(terms_only)):
            w1, w2 = sorted([terms_only[i], terms_only[j]])                
            if w1 != w2:
                com[w1][w2] += 1

```

在构建共现矩阵时，我们不想重复计算相同的术语对，例如 `com[A][B] == com[B][A]`，因此内层循环从 `i+1` 开始，以构建一个三角矩阵，而 `sorted` 会保持术语的字母顺序。

对于每个术语，我们提取 5 个最频繁的共现术语，创建一个形式为 `((term1, term2), count)` 的元组列表：

```py
com_max = []
# For each term, look for the most common co-occurrent terms
for t1 in com:
    t1_max_terms = sorted(com[t1].items(), key=operator.itemgetter(1), reverse=True)[:5]
    for t2, t2_count in t1_max_terms:
        com_max.append(((t1, t2), t2_count))
# Get the most frequent co-occurrences
terms_max = sorted(com_max, key=operator.itemgetter(1), reverse=True)
print(terms_max[:5])

```

结果：

```py
[(('6', 'nations'), 845), (('champions', 'ireland'), 760), 
  (('nations', 'rbs'), 742), (('day', 'ireland'), 731), 
  (('ireland', 'wales'), 674)]

```

这个实现方法相当直接，但根据数据集和矩阵的使用情况，可能需要考虑使用像 `scipy.sparse` 这样的工具来构建稀疏矩阵。

我们还可以寻找特定术语，并提取其最频繁的共现术语。我们只需在主循环中添加一个额外的计数器，例如：

```py
search_word = sys.argv[1] # pass a term as a command-line argument
count_search = Counter()
for line in f:
    tweet = json.loads(line)
    terms_only = [term for term in preprocess(tweet['text']) 
                  if term not in stop 
                  and not term.startswith(('#', '@'))]
    if search_word in terms_only:
        count_search.update(terms_only)
print("Co-occurrence for %s:" % search_word)
print(count_search.most_common(20))

```

“ireland”的结果：

```py
[('champions', 756), ('day', 727), ('nations', 659), ('wales', 654), ('2015', 638), 
  ('6', 613), ('rbs', 585), ('http://t.co/y0nvsvayln', 559), ('__SHAMROCK_SYMBOL__', 526), ('10', 522), 
  ('win', 377), ('england', 377), ('twickenham', 361), ('40', 360), ('points', 356), 
  ('sco', 355), ('ire', 355), ('title', 346), ('scotland', 301), ('turn', 295)]

```

“rugby”的结果：

```py
[('day', 476), ('game', 160), ('ireland', 143), ('england', 132), ('great', 105), 
  ('today', 104), ('best', 97), ('well', 90), ('ever', 89), ('incredible', 87), 
  ('amazing', 84), ('done', 82), ('amp', 71), ('games', 66), ('points', 64), 
  ('monumental', 58), ('strap', 56), ('world', 55), ('team', 55), ('http://t.co/bhmeorr19i', 53)]

```

总体来说，非常有趣。

### 总结

这篇文章讨论了一个关于 Twitter 上文本挖掘的示例，使用了一些在体育赛事中收集的现实数据。运用我们在之前的章节中学到的知识，我们通过流媒体 API 下载了一些数据，处理成 JSON 格式，并从推文中提取了一些有趣的术语和标签。文章还介绍了术语共现的概念，展示了如何构建共现矩阵，并讨论了如何使用它来发现一些有趣的洞察。

**简介： [Marco Bonzanini](https://twitter.com/marcobonzanini)** 是一名驻伦敦的 数据科学家。活跃于 PyData 社区，他喜欢从事文本分析和数据挖掘应用。他是《[用 Python 精通社交媒体挖掘](https://www.amazon.com/Mastering-Social-Media-Mining-Python-ebook/dp/B01BFD2Z2Q)》（Packt Publishing，2016 年 7 月）的作者。

[原文](https://marcobonzanini.com/2015/03/23/mining-twitter-data-with-python-part-4-rugby-and-term-co-occurrences/)。经许可转载。

**相关**：

+   用 Python 挖掘 Twitter 数据 第一部分：数据收集

+   用 Python 挖掘 Twitter 数据 第二部分：文本预处理

+   用 Python 挖掘 Twitter 数据 第三部分：术语频率

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 方面

* * *

### 更多相关话题

+   [机器学习不像你的大脑 第六部分：精确突触权重的重要性](https://www.kdnuggets.com/2022/08/machine-learning-like-brain-part-6-importance-precise-synapse-weights-ability-set-quickly.html)

+   [KDnuggets 新闻，4 月 6 日：8 门免费 MIT 课程学习数据科学](https://www.kdnuggets.com/2022/n14.html)

+   [数据科学备忘单完整合集 - 第一部分](https://www.kdnuggets.com/2022/02/complete-collection-data-science-cheat-sheets-part-1.html)

+   [构建视觉搜索引擎 - 第一部分：数据探索](https://www.kdnuggets.com/2022/02/building-visual-search-engine-part-1.html)

+   [数据科学备忘单完整合集 - 第二部分](https://www.kdnuggets.com/2022/02/complete-collection-data-science-cheat-sheets-part-2.html)

+   [数据仓库完整合集 - 第一部分](https://www.kdnuggets.com/2022/04/complete-collection-data-repositories-part-1.html)
