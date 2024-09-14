# 使用 spaCy 的 Python 自然语言：简介

> 原文：[`www.kdnuggets.com/2019/09/natural-language-python-using-spacy-introduction.html`](https://www.kdnuggets.com/2019/09/natural-language-python-using-spacy-introduction.html)

评论

**作者：[Paco Nathan](https://twitter.com/pacoid)**

*本文提供了使用 spaCy 和 Python 相关库的自然语言简要介绍。[补充的 Domino 项目也可以查看](https://try.dominodatalab.com/u/domino-johnjoo/spacy-dev/overview)。*

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT

* * *

### 简介

本文和配套的 [Domino 项目](https://try.dominodatalab.com/u/domino-johnjoo/spacy-dev/overview) 提供了使用 *自然语言*（有时称为“文本分析”）的 Python 简要介绍，使用的工具包括 [*spaCy*](https://spacy.io/) 和相关库。工业界的数据科学团队必须处理大量文本，这是机器学习中使用的四大数据类别之一。通常是人类生成的文本，但并非总是如此。

想一想：商业的“操作系统”是如何工作的？通常，包括合同（销售合同、工作协议、合作伙伴关系）、发票、保险单、规章制度及其他法律等。所有这些都以文本形式存在。

你可能会遇到一些缩略语：*自然语言处理*（NLP）、*自然语言理解*（NLU）、*自然语言生成*（NLG）——它们分别大致表示“读取文本”、“理解意义”、“编写文本”。这些任务的重叠越来越多，因此很难对任何特定功能进行分类。

*spaCy* 框架——以及越来越多的插件和其他集成——提供了广泛的自然语言任务功能。它已成为 Python 中工业应用最广泛使用的自然语言库之一，并拥有相当大的社区——因此，对研究进展的商业化支持也较多，随着该领域的快速发展，支持也在不断增加。

### 入门

我们已经在 Domino 中配置了默认计算环境，以包含本教程所需的所有软件包、库、模型和数据。[查看 Domino 项目以运行代码。](https://try.dominodatalab.com/u/domino-johnjoo/spacy-dev/overview)

![](img/12edb7abbae9b1e4aad03968716e8847.png)

![](img/6a52a1f569a31b20cf65e6d774fbf268.png)

如果你对 Domino’s 计算环境如何工作感兴趣，可以查看 [支持页面](https://support.dominodatalab.com/hc/en-us/articles/115000392643-Environment-management)。

现在让我们加载 *spaCy* 并运行一些代码：

```py
import spacy

nlp = spacy.load("en_core_web_sm")
```

现在，`nlp` 变量是你接触所有 *spaCy* 相关内容的入口，并加载了用于英语的 `en_core_web_sm` 小型模型。接下来，让我们通过自然语言解析器运行一个小的“文档”：

```py
text = "The rain in Spain falls mainly on the plain."
doc = nlp(text)

for token in doc:
    print(token.text, token.lemma_, token.pos_, token.is_stop)
```

```py
The the DET True
rain rain NOUN False
in in ADP True
Spain Spain PROPN False
falls fall VERB False
mainly mainly ADV False
on on ADP True
the the DET True
plain plain NOUN False
. . PUNCT False
```

首先，我们从文本中创建了一个 [doc](https://spacy.io/api/doc)，它是一个容器，包含了文档及其所有注释。然后我们遍历了文档，以查看 *spaCy* 解析了什么。

很好，但信息量很大，阅读起来有点困难。让我们将 *spaCy* 解析的句子重新格式化为一个 [pandas](https://pandas.pydata.org/) 数据框：

```py
import pandas as pd

cols = ("text", "lemma", "POS", "explain", "stopword")
rows = []

for t in doc:
    row = [t.text, t.lemma_, t.pos_, spacy.explain(t.pos_), t.is_stop]
    rows.append(row)

df = pd.DataFrame(rows, columns=cols)

df
```

![](img/6efac198b9c30e6826f149f2fef68f55.png)

可读性提高了！在这个简单的例子中，整个文档只是一个简短的句子。在那个句子中的每个词，*spaCy* 都创建了一个标记，我们访问了每个标记中的字段来显示：

+   原始文本

+   [词元](https://en.wikipedia.org/wiki/Lemma_(morphology) - 词的根形式

+   [词性](https://en.wikipedia.org/wiki/Part_of_speech)

+   一个标记是否是 *停用词* 的标志——即，可能被过滤掉的常见词

接下来，让我们使用 [displaCy](https://ines.io/blog/developing-displacy) 库来可视化该句子的解析树：

```py
from spacy import displacy

displacy.render(doc, style="dep")
```

![](img/11f4063ce82c16944df254f90c40ecbd.png)

这是否让你想起了小学的回忆？坦白说，对于那些来自计算语言学背景的人来说，这个图示确实带来快乐。

但我们稍微退一步。你如何处理多个句子？

有用于 *句子边界检测*（SBD）—也称为 *句子分割*—的功能，基于内置/默认的 [sentencizer](https://spacy.io/api/sentencizer/)：

```py
text = "We were all out at the zoo one day, I was doing some acting, walking on the railing of the gorilla exhibit. I fell in. Everyone screamed and Tommy jumped in after me, forgetting that he had blueberries in his front pocket. The gorillas just went wild."

doc = nlp(text)

for sent in doc.sents:
    print(">", sent)
```

```py
> We were all out at the zoo one day, I was doing some acting, walking on the railing of the gorilla exhibit.
> I fell in.
> Everyone screamed and Tommy jumped in after me, forgetting that he had blueberries in his front pocket.
> The gorillas just went wild.
```

当 *spaCy* 创建文档时，它使用了一种 *非破坏性分词* 原则，意味着标记、句子等只是长数组中的索引。换句话说，它们不会将文本流切割成小块。因此，每个句子都是一个 [span](https://spacy.io/api/span)，具有 *start* 和 *end* 索引到文档数组：

```py
for sent in doc.sents:
    print(">", sent.start, sent.end)
```

```py
> 0 25
> 25 29
> 29 48
> 48 54
```

我们可以在文档数组中索引，以提取一个句子的标记：

```py
doc[48:54]
```

```py
The gorillas just went wild.
```

或简单地索引到一个特定的标记，例如最后一个句子中的动词 `went`：

```py
token = doc[51]
print(token.text, token.lemma_, token.pos_)
```

```py
went go VERB
```

到目前为止，我们可以解析文档，将文档分割成句子，然后查看每个句子中标记的注释。这是一个良好的开端。

### 获取文本

现在我们可以解析文本了，那我们从哪里获取文本呢？一个快速的来源是利用网络。当然，当我们下载网页时，我们会得到 HTML，然后需要从中提取文本。[Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) 是一个流行的包来实现这一点。

首先，进行一些基础操作：

```py
import sys
import warnings

warnings.filterwarnings("ignore")
```

在以下函数`get_text()`中，我们将解析 HTML 以查找所有的`<p/>`标签，然后提取这些标签的文本：

```py
from bs4 import BeautifulSoup
import requests
import traceback

def get_text (url): 
    buf = []

    try:
        soup = BeautifulSoup(requests.get(url).text, "html.parser")

        for p in soup.find_all("p"): 
            buf.append(p.get_text())

        return "\n".join(buf)
    except:  
        print(traceback.format_exc())
        sys.exit(-1)
```

现在让我们从在线来源抓取一些文本。我们可以比较[Open Source Initiative](https://opensource.org/licenses/)网站上托管的开源许可证：

```py
lic = {}
lic["mit"] = nlp(get_text("https://opensource.org/licenses/MIT"))
lic["asl"] = nlp(get_text("https://opensource.org/licenses/Apache-2.0"))
lic["bsd"] = nlp(get_text("https://opensource.org/licenses/BSD-3-Clause"))

for sent in lic["bsd"].sents: 
    print(">", sent)
```

```py
> SPDX short identifier: BSD-3-Clause
> Note: This license has also been called the "New BSD License" or  "Modified BSD License"
> See also the 2-clause BSD License.
…
```

自然语言工作中的一个常见用例是比较文本。例如，通过这些开源许可证，我们可以下载它们的文本，解析，然后比较[similarity](https://spacy.io/api/doc#similarity)度量：

```py
pairs = [
    ["mit", "asl"], 
    ["asl", "bsd"], 
    ["bsd", "mit"]
]

for a, b in pairs:
    print(a, b, lic[a].similarity(lic[b]))
```

```py
mit asl 0.9482039305669306
asl bsd 0.9391555350757145
bsd mit 0.9895838089575453
```

这很有趣，因为[BSD](https://opensource.org/licenses/BSD-3-Clause)和[MIT](https://opensource.org/licenses/MIT)许可证似乎是最相似的文档。事实上，它们密切相关。

坦率地说，由于 OSI 免责声明在页脚中包含了一些额外的文本——但这为比较许可证提供了一个合理的近似。

### 自然语言理解

现在让我们*深入探讨*一些*spaCy*的自然语言理解（NLU）功能。鉴于我们有一个文档的解析，从纯语法的角度来看，我们可以提取[noun chunks](https://spacy.io/usage/linguistic-features#noun-chunks)，即每个名词短语：

```py
text = "Steve Jobs and Steve Wozniak incorporated Apple Computer on January 3, 1977, in Cupertino, California."
doc = nlp(text)

for chunk in doc.noun_chunks: 
    print(chunk.text)
```

```py
Steve Jobs
Steve Wozniak
Apple Computer
January
Cupertino
California
```

不错。句子中的名词短语通常提供更多的信息内容——作为一种简单的过滤器，用于将长文档减少到更“精炼”的表示形式。

我们可以进一步采用这种方法，并识别文本中的[named entities](https://spacy.io/usage/linguistic-features#named-entities)，即专有名词：

```py
for ent in doc.ents:
    print(ent.text, ent.label_)
```

```py
Steve Jobs PERSON
Steve Wozniak PERSON
Apple Computer ORG
January 3, 1977 DATE
Cupertino GPE
California GPE
```

*displaCy*库提供了一种出色的可视化命名实体的方式：

```py
displacy.render(doc, style="ent")
```

![](img/d89ad761f54624be253828504cdedc5e.png)

如果你正在处理[knowledge graph](https://www.akbc.ws/2019/)应用程序和其他[linked data](http://linkeddata.org/)，你的挑战是构建文档中的命名实体与其他相关信息之间的链接，这称为[entity linking](http://nlpprogress.com/english/entity_linking.html)。识别文档中的命名实体是这种特定类型的 AI 工作中的第一步。例如，给定上述文本，可以将`Steve Wozniak`命名实体链接到[lookup in DBpedia](http://dbpedia.org/page/Steve_Wozniak)。

更一般来说，还可以将*lemmas*链接到描述其含义的资源。例如，在早期部分，我们解析了句子`The gorillas just went wild`，并能够显示单词`went`的词元是动词`go`。此时我们可以使用一个叫做[WordNet](https://wordnet.princeton.edu/)的古老项目，它提供了一个英语的词汇数据库——换句话说，它是一个可计算的词典。

有一个名为[spacy-wordnet](https://github.com/recognai/spacy-wordnet)的*spaCy*与 WordNet 的集成，由自然语言和知识图谱工作的专家*[Daniel Vila Suero](https://twitter.com/dvilasuero)*提供。

然后我们将通过 NLTK 加载 WordNet 数据（这些事情会发生）：

```py
import nltk
nltk.download("wordnet")

[nltk_data] Downloading package wordnet to /home/ceteri/nltk_data...
[nltk_data] Package wordnet is already up-to-date!
```

```py
True
```

注意到*spaCy*作为一个“管道”运行，并允许自定义管道中的部分内容。这对于支持数据科学工作中的非常有趣的工作流集成非常有用。在这里，我们将从*spacy-wordnet*项目中添加*WordnetAnnotator*：

```py
from spacy_wordnet.wordnet_annotator import WordnetAnnotator

print("before", nlp.pipe_names)

if "WordnetAnnotator" not in nlp.pipe_names:
    nlp.add_pipe(WordnetAnnotator(nlp.lang), after="tagger")

print("after", nlp.pipe_names)
```

```py
before ['tagger', 'parser', 'ner']
after ['tagger', 'WordnetAnnotator', 'parser', 'ner']
```

在英语中，一些词因具有多种可能的含义而臭名昭著。例如，通过[WordNet](http://wordnetweb.princeton.edu/perl/webwn?s=star&sub=Search+WordNet&o2&o0=1&o8=1&o1=1&o7&o5&o9&o6&o3&o4&h)搜索结果来查找与`withdraw`一词相关的含义。

现在让我们使用*spaCy*来自动执行该查找：

```py
token = nlp("withdraw")[0]
token._.wordnet.synsets()
```

```py
[Synset('withdraw.v.01'),
Synset('retire.v.02'),
Synset('disengage.v.01'), 
Synset('recall.v.07'), 
Synset('swallow.v.05'), 
Synset('seclude.v.01'), 
Synset('adjourn.v.02'),
Synset('bow_out.v.02'), 
Synset('withdraw.v.09'), 
Synset('retire.v.08'), 
Synset('retreat.v.04'), 
Synset('remove.v.01')]
```

```py
token._.wordnet.lemmas()
```

```py
[Lemma('withdraw.v.01.withdraw'),
Lemma('withdraw.v.01.retreat'),
Lemma('withdraw.v.01.pull_away'), 
Lemma('withdraw.v.01.draw_back'), 
Lemma('withdraw.v.01.recede'),
Lemma('withdraw.v.01.pull_back'), 
Lemma('withdraw.v.01.retire'),
…
```

```py
token._.wordnet.wordnet_domains()
```

```py
['astronomy',
'school',
'telegraphy',
'industry',
'psychology',
'ethnology',
'ethnology',
'administration',
'school',
'finance',
'economy',
'exchange',
'banking',
'commerce',
'medicine',
'ethnology', 
'university',
…
```

同样，如果你正在处理知识图谱，那些来自*WordNet*的“词义”链接可以与图算法一起使用，以帮助识别特定词的含义。这也可以用于通过一种叫做*总结*的技术开发较大文本部分的摘要。这超出了本教程的范围，但在自然语言处理领域中这是一个有趣的应用。

反过来，如果你知道*a priori*一份文档是关于特定领域或主题集合的，那么你可以限制从*WordNet*返回的含义。在以下示例中，我们希望考虑金融和银行领域的 NLU 结果：

```py
domains = ["finance", "banking"]
sentence = nlp("I want to withdraw 5,000 euros.")

enriched_sent = []

for token in sentence:
    # get synsets within the desired domains
    synsets = token._.wordnet.wordnet_synsets_for_domain(domains)

    if synsets:
       lemmas_for_synset = []

       for s in synsets:
           # get synset variants and add to the enriched sentence
           lemmas_for_synset.extend(s.lemma_names())
           enriched_sent.append("({})".format("|".join(set(lemmas_for_synset))))
    else:
        enriched_sent.append(token.text)

print(" ".join(enriched_sent))
```

```py
I (require|want|need) to (draw_off|withdraw|draw|take_out) 5,000 euros .
```

这个示例看起来很简单，但如果你玩一下`domains`列表，你会发现结果在没有合理约束的情况下会出现一种组合爆炸的情况。想象一下拥有一个包含数百万个元素的知识图谱：你会想尽可能地限制搜索，以避免每个查询都需要数天/周/月/年来计算。

有时，试图理解文本——或者更好地说，理解一个*corpus*（一个包含许多相关文本的数据集）——时遇到的问题变得如此复杂，以至于你需要先可视化它。这是一个用于理解文本的交互式可视化工具：[scattertext](https://spacy.io/universe/project/scattertext)，这是[Jason Kessler](https://twitter.com/jasonkessler)的天才作品。

让我们分析 2012 年美国总统选举期间的党派大会的文本数据。注意：这个单元可能需要几分钟才能运行，但经过所有这些数字运算后，结果是值得等待的。

```py
import scattertext as st

if "merge_entities" not in nlp.pipe_names:
    nlp.add_pipe(nlp.create_pipe("merge_entities"))

if "merge_noun_chunks" not in nlp.pipe_names:
    nlp.add_pipe(nlp.create_pipe("merge_noun_chunks"))

convention_df = st.SampleCorpora.ConventionData2012.get_data() 
corpus = st.CorpusFromPandas(convention_df,
                             category_col="party",
                             text_col="text",
                             nlp=nlp).build()
```

一旦你准备好了`corpus`，生成一个 HTML 中的交互式可视化：

```py
html = st.produce_scattertext_explorer(
    corpus,
    category="democrat",
    category_name="Democratic",
    not_category_name="Republican",
    width_in_pixels=1000,
    metadata=convention_df["speaker"]
)
```

```py
from IPython.display import IFrame

file_name = "foo.html"

with open(file_name, "wb") as f:
     f.write(html.encode("utf-8"))

IFrame(src=file_name, width = 1200, height=700)
```

现在我们将渲染 HTML——请稍等一两分钟，它值得等待：

![](img/82c17273d5bee33940a24dde852dbb2a.png)

想象一下，如果你有过去三年中针对特定产品的客户支持文本。假设你的团队需要了解客户如何谈论该产品？这个*scattertext*库可能会非常有用！你可以对*NPS 评分*（一个客户评估指标）进行聚类（k=2），然后用聚类中的前两个组件替换民主党/共和党的维度。

### 概要

五年前，如果你询问有关 Python 中自然语言的开源工具，许多数据科学工作者的默认答案可能是[NLTK](https://www.nltk.org/)。该项目包括几乎所有内容，但组件相对学术。另一个受欢迎的自然语言项目是斯坦福大学的[CoreNLP](https://stanfordnlp.github.io/CoreNLP/)，虽然也相当学术，但功能强大，不过 CoreNLP 在生产环境中与其他软件集成可能会有挑战。

几年前，这个自然语言领域的一切开始发生变化。*spaCy*的两位主要作者，[Matthew Honnibal](https://twitter.com/honnibal)和[Ines Montani](https://twitter.com/_inesmontani)，于 2015 年推出了该项目，行业采纳迅速。他们专注于一种*主观性*的方法（做必要的事，做到最好，不多也不少），这使得在 Python 中的数据科学工作流程中能够简单、快速地集成，并且执行速度更快、准确性更高。基于这些优先级，*spaCy*变成了*NLTK*的反面。自 2015 年以来，*spaCy*始终专注于作为一个开源项目（即依赖其社区进行方向指导、集成等）以及商业级软件（而非学术研究）。尽管如此，*spaCy*迅速纳入了机器学习的 SOTA 进展，实际上成为了将研究成果转化为行业应用的桥梁。

值得注意的是，自 2000 年代中期谷歌开始赢得国际语言翻译竞赛后，自然语言的机器学习得到了极大的提升。另一个重大变化发生在 2017-2018 年，当时，随着*深度学习*的诸多成功，这些方法开始超越之前的机器学习模型。例如，请参见[ELMo](https://arxiv.org/abs/1802.05365)对语言嵌入的工作，由 Allen AI 主导，接着是[Google 的 BERT](https://ai.googleblog.com/2018/11/open-sourcing-bert-state-of-art-pre.html)，以及最近的[百度的 ERNIE](https://medium.com/syncedreview/baidus-ernie-tops-google-s-bert-in-chinese-nlp-tasks-d6a42b49223d)——换句话说，全球的搜索引擎巨头们为我们提供了基于深度学习的开源嵌入语言模型的《芝麻街》剧目，这些模型现在是最先进的（SOTA）。说到这一点，为了跟踪自然语言的 SOTA，可以关注[NLP-Progress](http://nlpprogress.com/)和[Papers with Code](https://paperswithcode.com/sota)。

过去两年自然语言的应用场景发生了剧烈变化，深度学习技术的出现使得这些变化加速。大约在 2014 年，一个 Python 自然语言教程可能会展示*词频统计*、*关键词搜索*或*情感检测*，当时的目标用例相对较为平淡。而到了 2019 年，我们谈论的则是分析成千上万的文档以优化工业*supply chain*…或分析数亿份文档用于保险公司保单持有人，或无数份关于财务披露的文档。更现代的自然语言处理工作倾向于自然语言理解（NLU），通常用于支持*知识图谱*的构建，并且越来越多地应用于自然语言生成（NLG），在这里，大量类似文档可以在人工规模下进行总结。

[spaCy Universe](https://spacy.io/universe) 是检查特定用例深度探讨以及了解这一领域如何发展的好地方。从这个“宇宙”中选择的一些项目包括：

+   [Blackstone](https://spacy.io/universe/project/blackstone) – 解析非结构化法律文本

+   [Kindred](https://spacy.io/universe/project/kindred) – 从生物医学文本（例如，制药领域）中提取实体

+   [mordecai](https://spacy.io/universe/project/mordecai) – 解析地理信息

+   [Prodigy](https://spacy.io/universe/project/prodigy) – 人工干预标注数据集

+   [spacy-raspberry](https://spacy.io/universe/project/spacy-raspberry) – Raspberry PI 镜像，用于在边缘设备上运行 spaCy 和深度学习

+   [Rasa NLU](https://spacy.io/universe/project/rasa) – Rasa 集成用于聊天应用

另外，还有几个超级新的项目值得一提：

+   [spacy-pytorch-transformers](https://explosion.ai/blog/spacy-pytorch-transformers) 用于微调（即，使用迁移学习）Sesame Street 的角色和朋友：BERT、GPT-2、XLNet 等。

+   [spaCy IRL 2019](https://irl.spacy.io/2019/) 会议 – 查看演讲视频！

我们可以用*spaCy*做更多的事情——希望这个教程能够提供一个入门介绍。祝你在自然语言处理工作中一切顺利。

[原始](https://blog.dominodatalab.com/natural-language-in-python-using-spacy/)。经许可转载。

**相关：**

+   自然语言处理中的迁移学习现状

+   Reddit 帖子分类

+   2018 年数据科学和 AI 的顶级 7 个 Python 库

### 相关话题

+   [使用 spaCy 的自然语言处理](https://www.kdnuggets.com/2023/01/natural-language-processing-spacy.html)

+   [使用 spaCy 入门 NLP](https://www.kdnuggets.com/2022/11/getting-started-spacy-nlp.html)

+   [自然语言处理中的 N-gram 语言建模](https://www.kdnuggets.com/2022/06/ngram-language-modeling-natural-language-processing.html)

+   [自然语言处理的温和介绍](https://www.kdnuggets.com/2022/06/gentle-introduction-natural-language-processing.html)

+   [自然语言处理简介](https://www.kdnuggets.com/introduction-to-natural-language-processing)

+   [如何开始使用 PyTorch 进行自然语言处理](https://www.kdnuggets.com/2022/04/start-natural-language-processing-pytorch.html)
