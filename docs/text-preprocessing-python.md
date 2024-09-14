# Python 中的文本预处理：步骤、工具和示例

> 原文：[`www.kdnuggets.com/2018/11/text-preprocessing-python.html/2`](https://www.kdnuggets.com/2018/11/text-preprocessing-python.html/2)

**示例 12\. 使用 NLTK 的命名实体识别：**

**代码：**

```py

from nltk import word_tokenize, pos_tag, ne_chunk
input_str = “Bill works for Apple so he went to Boston for a conference.”
print ne_chunk(pos_tag(word_tokenize(input_str)))

```

**输出：**

```py

(S (PERSON Bill/NNP) works/VBZ for/IN Apple/NNP so/IN he/PRP went/VBD to/TO (GPE Boston/NNP) for/IN a/DT conference/NN ./.)

```

### 指代消解（指代解析）

代词和其他指代表达应与正确的个体连接。指代消解工具找到文本中指向同一现实世界实体的提及。例如，在句子“Andrew 说他会买一辆车”中，代词“他”指向同一个人，即“Andrew”。指代消解工具： [斯坦福 CoreNLP](https://stanfordnlp.github.io/CoreNLP/coref.html)， [spaCy](https://spacy.io/)， [Open Calais](http://www.opencalais.com/)， [Apache OpenNLP](https://opennlp.apache.org/docs/1.8.4/manual/opennlp.html#tools.coref) 在 [“指代消解”表格中](https://docs.google.com/spreadsheets/d/1-9rMhfcmxFv2V2Q5ZWn1FfLDZZYsuwb1eoSp9CiEEOg/edit?usp=sharing)中描述 |

| **名称，开发者，初始发布** | **功能** | **编程语言** | **许可证** |
| --- | --- | --- | --- |
| [**美丽的指代消解工具包（BART）**，Massimo Poesio, Simone Ponzetto, Yannick Versley，约翰霍普金斯夏季研讨会，2007 年](http://www.bart-coref.org/) | 综合了多种机器学习方法 | 基于 REST 的 Web 服务 | [Apache 2.0 许可证](https://www.apache.org/licenses/LICENSE-2.0) |
| 使用多个机器学习工具包 |
| [将结果导出为内联 XML [36]](http://www.bart-coref.org/index.html) |
| [**JavaRAP**，Long Qiu，2004 年](https://wing.comp.nus.edu.sg/~qiu/NLPTools/JavaRAP.html) | 解决第三人称代词、词汇性指代，识别冗余代词 | Java | - |
| 输出格式：指代 - 先行词对；文本中进行原位替换 |
| 准确率：57.9%（MUC6） |
| [每秒大约 1,500 个词 [37]](https://wing.comp.nus.edu.sg/~qiu/NLPTools/JavaRAP.html) |
| [**通用指代消解工具 - GuiTAR**，埃塞克斯大学，2007 年](http://cswww.essex.ac.uk/Research/nle/GuiTAR/) | 以 MAS-XML 兼容文件为输入，添加新的标记以保存指代信息（元素）。 | Java | [GPL 许可证](http://www.gnu.org/licenses/gpl.txt) |
| [评估模块也包括在内 [38]](http://cswww.essex.ac.uk/Research/nle/GuiTAR/) |
| [**Reconcile**，康奈尔大学，犹他大学，劳伦斯利佛莫尔国家实验室，2009 年](https://www.cs.utah.edu/nlp/reconcile/) | 在常见数据集或未标记文本上运行 | Java | [GPL 许可证](http://www.gnu.org/licenses/gpl.txt) |
| 利用 Weka 工具包、伯克利解析器和斯坦福命名实体识别系统的监督机器学习分类器。 |
| MUC 分数（MUC-6）：召回率 67.23%；精准度 65.54%；F 值 66.38 |
| [[39]](https://www.cs.utah.edu/nlp/reconcile/) |
| [**ARKref**，Brendan O'Connor，Michael Heilman，2009](http://www.cs.cmu.edu/~ark/ARKref/) | 确定性的，基于规则的系统 | Java | [GPL，MIT](https://github.com/brendano/arkref/blob/master/LICENSE.txt) |
| 使用来自成分分析器的句法信息和来自实体识别组件的语义信息 |
| [精确度：0.657617，召回率：0.552433，F1 值：0.600454 [40]](https://github.com/brendano/arkref) |
| [**伊利诺伊核心指代包**，Dan Roth，Eric Bengtson，2008](https://cogcomp.org/page/software_view/Coref) | 与指代相关的特性： | Java | - |
| 性别和数量匹配，WordNet 关系，包括同义词、上位词、反义词和 ACE 实体类型（人、组织和地理政治实体） |
| [特性包括指代性分类器 [41]](https://cogcomp.org/page/software_view/Coref) |
| [**Neural coref**，Hugging Face，2017](https://github.com/huggingface/neuralcoref) | 使用神经网络和 spaCy | Python | [MIT 许可证](https://github.com/huggingface/neuralcoref/blob/master/LICENCE.txt) |
| [在计算特性和解决指代时添加了各种发言人 [42]](https://medium.com/huggingface/state-of-the-art-neural-coreference-resolution-for-chatbots-3302365dcf30) |
| [在线演示](https://huggingface.co/coref/) |
| [**指代解决工具包 (cort)**，Sebastian Martschat，Thierry Goeckel，Patrick Claus](https://github.com/smartschat/cort) | 指代解决组件 | Python | [MIT 许可证](https://github.com/smartschat/cort/blob/master/LICENSE) |
| 错误分析组件 |
| 框架基于潜变量，允许用户快速设计指代解决的方法 |
| [分析和可视化指代解决系统的错误 [43]](https://github.com/smartschat/cort) |
| [**CherryPicker**，Altaf Rahman，Vincent Ng，德克萨斯大学达拉斯分校，2009](http://www.hlt.utdallas.edu/~altaf/cherrypicker/) | 在 Unix/Linux 上运行 | - | 免费用于教育和研究活动；不得用于商业或营利目的 |
| [集群排名指代模型 [44]](http://www.hlt.utdallas.edu/~altaf/cherrypicker/) |
| [**FreeLing**，TALP 研究中心，卡塔卢尼亚理工大学](http://nlp.lsi.upc.edu/freeling/) | 提供语言分析功能 | C++ | [Affero GNU 通用公共许可证](http://www.gnu.org/licenses/agpl.html) |
| 支持多种语言 |
| 提供命令行前端 |
| [输出格式：XML，JSON，CoNLL [45]](http://nlp.lsi.upc.edu/freeling/) |
| [**外部可配置的 REference 和非命名实体识别器 (xrenner)**，Zeldes，Amir 和 Zhang，Shuo，乔治敦大学语言学系，2016](https://corpling.uis.georgetown.edu/xrenner/) | 高度可配置，语言独立的指代解决器 | Python | [Apache 许可证 2.0](https://github.com/amir-zeldes/xrenner/blob/master/LICENSE.txt) |
| [可插拔分类器 – 模型可以 100%基于规则工作或使用分类器对规则输出进行排序 [46]](https://corpling.uis.georgetown.edu/xrenner/) |
|  |

**核心指代消解工具**

使用 xrenner 的核心指代消解示例可以在[这里](https://corpling.uis.georgetown.edu/xrenner/doc/using.html#importing-as-a-module)找到。

### 联词提取

联词是指比随机预期更频繁一起出现的词组合。联词示例包括“打破规则”、“空闲时间”、“得出结论”、“记住”、“准备好”等等。

| **名称，开发者，初始发布** | **特性** | **编程语言** | **许可证** |
| --- | --- | --- | --- |
| [**TermeX**，萨格勒布大学文本分析与知识工程实验室，2009](http://takelab.fer.hr/termex_s/) | UTF-8 格式的输入文本 | [前端 GUI](http://ktlab.fer.hr/termex/data/TermexHelp_e.pdf) | 研究目的可根据请求免费提供。 |
| 使用 14 种关联度量 |
| 处理长度最多为四的 n-gram |
| 手动选择术语词汇的候选 n-gram |
| 支持 Windows 和 Linux |
| 快速且内存高效地处理大规模语料库 |
| [ [47]](http://takelab.fer.hr/termex_s/) |
| [**Collocate**，Athelstan](http://www.athel.com/) | 使用统计分析（t 分数，对数似然，互信息）和频率信息来呈现候选联词列表 | 应用 | [教育价格。单用户：$45](http://www.michaelbarlow.com/) |
| 在设定的范围内搜索一个词（短语）（例如 4 个词）。 | [站点许可证（2 年，15 用户）$395](http://www.michaelbarlow.com/) |
| 生成 n-gram 列表 |  |
| [使用阈值提取联词，并使用互信息 [48]](https://www.athel.com/colloc.html) |  |
| [**CollTerm**，萨格勒布大学人文与社会科学学院；罗马尼亚科学院人工智能研究所](https://github.com/accurat-toolkit/CollTerm) | 基于五种不同的共现度量结果，用于多词单元（即联词）或从大规模代表性语料库中通过对单词单元应用 TF-IDF 测量得出的分布差异 | Python | [Apache 许可证 2.0](http://www.apache.org/licenses/LICENSE-2.0.html) |
| [语言独立 [49]](http://linghub.lider-project.eu/metashare/a89c02f4663d11e28a985ef2e4e6c59e76428bf02e394229a70428f25a839f75) |
| [**联词提取器**，Dan Ștefănescu，2012](http://metashare.ilsp.gr:8080/repository/browse/collocation-extractor/7a2432acdc7311e5aa0b00237df3e35819ec25a2cee244cd8782b24eddcad3c8/) | 本方法中的联词特征： | [应用](http://ws.racai.ro:9191/narratives/batch2/Colloc.pdf) | 限制：通知许可方，不得再分发 |
| – 它们之间的距离相对恒定； | 用户性质：学术、商业 |
| – 它们比随机预期更频繁地一起出现（对数似然） |  |
| 与语言无关 |  |
| [输出注释格式：每行一个搭配的文本输出，注释用制表符分隔 [50]](http://ws.racai.ro:9191/narratives/batch2/Colloc.pdf) |  |
| [**ICE: 习语和搭配提取器**，Verizon Labs，休斯顿大学计算机科学系，2017](https://github.com/shahryarabaki/ICE) | 习语和搭配提取 | Python | [Apache 许可证 2.0](https://github.com/shahryarabaki/ICE/blob/master/LICENSE) |
| 两种用户友好的格式 |
| [用于离线和在线搭配识别，字典搜索、网络搜索和替代，以及网络搜索独立性 [51]](http://www.aclweb.org/anthology/E17-3027) |
| [**Text::NSP**，明尼苏达大学，卡内基梅隆大学，匹兹堡大学，2000](https://metacpan.org/pod/Text::NSP) | 从文本中提取搭配和 N-gram | Perl | [GNU 通用公共许可证](http://www.gnu.org/licenses/gpl.txt) |
| Text::NSP::Measures 模块评估 N-gram 中词语的共现是否纯属偶然或具有统计意义。 |
| [[52]](https://metacpan.org/pod/Text::NSP) |

**搭配提取工具**

**示例 13\. 使用 ICE 进行搭配提取**[**[51]**](http://www.aclweb.org/anthology/E17-3027)

**代码：**

```py

input=[“he and Chazz duel with all keys on the line.”]
from ICE import CollocationExtractor
extractor = CollocationExtractor.with_collocation_pipeline(“T1” , bing_key = “Temp”,pos_check = False)
print(extractor.get_collocations_of_length(input, length = 3))

```

**输出：**

```py

[“on the line”]

```

### **关系提取**

关系提取允许从非结构化来源（如原始文本）中获得结构化信息。严格来说，就是识别命名实体（如人、组织、地点）之间的关系（例如，收购、配偶、雇佣）。例如，从句子“Mark 和 Emily 昨天结婚了”中，我们可以提取出 Mark 是 Emily 的丈夫的信息。

| **名称，开发者，首次发布** | **特性** | **编程语言** | **许可证** |
| --- | --- | --- | --- |
| [**ReVerb**，华盛顿大学图灵中心](http://reverb.cs.washington.edu/) | 自动识别和提取英语句子中的二元关系 | Java | [ReVerb 软件许可证协议](http://reverb.cs.washington.edu/LICENSE.txt) |
| 设计用于网络规模的信息提取，其中目标关系不能提前指定，并且速度很重要。 |
| 输入原始文本 |
| [输出（论点 1，关系短语，论点 2）三元组 [53]](https://github.com/knowitall/reverb) |
| [**EXEMPLAR**，阿尔伯塔大学，2013](https://github.com/U-Alberta/exemplar) | 能够识别文本中描述的任何关系实例 | Java | [GNU 通用公共许可证 v3.0](https://github.com/U-Alberta/exemplar/blob/master/LICENSE) |
| 提取具有两个或更多论点的关系 |
| [论点的角色可以是 SUBJ（主语）、DOBJ（直接宾语）和 POBJ（介词宾语） [54]](https://github.com/U-Alberta/exemplar) |
| [**用于关系提取的文本探索工具包 (TETRE)**, Alisson Oldoni, 2017](https://github.com/aoldoni/tetre) | 接受原始文本作为输入 | 命令行工具 | [MIT 许可证](https://github.com/aoldoni/tetre/blob/master/LICENSE) |
| 针对由学术论文组成的语料库中的信息提取任务进行优化 |
| 进行数据转换、解析，并封装第三方二进制任务 |
| [以 HTML 和 JSON 输出关系 [55]](https://github.com/aoldoni/tetre) |
| [**TextRazor**, TextRazor, 2011](https://www.textrazor.com/) | 使用先进的 NLP 和 AI 技术 | Python | [定价](https://www.textrazor.com/plans) |
| 每秒处理数千个词每核心 | PHP |
| [允许用户添加产品名称、人员、公司、自定义分类规则和高级语言模式 [56]](https://www.textrazor.com/technology) | Java |
|  | REST API |
| [**Python 中的信息提取 (IEPY)**, Machinalis, 2014](https://github.com/machinalis/iepy) | 尝试使用用户提供的信息预测关系 | Python | [BSD 3-Clause "New" 或 "Revised" 许可证](https://github.com/machinalis/iepy/blob/develop/LICENSE) |
| 旨在对大数据集进行信息提取 (IE) |
| 为科学实验创建，使用新的 IE 算法 |
| [配置了便捷的默认设置 [57]](https://github.com/machinalis/iepy) |
| [**Watson 自然语言理解**, IBM](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/#relations) | 识别两个实体是否相关，并确定关系类型 | Curl | [定价](https://www.ibm.com/cloud/watson-natural-language-understanding/pricing) |
| [支持的语言：阿拉伯语、英语、韩语、西班牙语 [58]](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/#relations) | Node |
|  | Java |
|  | Python |
| [**MIT 信息提取 (MITIE)**, E. Davis King, 2009](https://github.com/mit-nlp/MITIE) | 二元关系检测 | C, C++, Java, R, Python | Boost 软件许可证 |
| 用于训练自定义提取器和关系检测器的工具 |
| 使用分布式词嵌入和结构化支持向量机 |
| 提供多个预训练模型 |
| [支持英语、西班牙语和德语 [59]](https://github.com/mit-nlp/MITIE) |

使用 [NLTK 提取关系的示例可以在这里找到](http://www.nltk.org/howto/relextract.html)。

### 总结

在这篇文章中，我们讨论了文本预处理并描述了其主要步骤，包括规范化、标记化、词干提取、词形还原、分块、词性标注、命名实体识别、共指解析、搭配提取和关系提取。我们还讨论了文本预处理工具和示例。创建了一个 [对比表](https://docs.google.com/spreadsheets/d/1-9rMhfcmxFv2V2Q5ZWn1FfLDZZYsuwb1eoSp9CiEEOg/edit?usp=sharing)。

文本预处理完成后，结果可以用于更复杂的自然语言处理任务，例如机器翻译或自然语言生成。

### 资源：

1.  [`www.nltk.org/index.html`](http://www.nltk.org/index.html)

1.  [`textblob.readthedocs.io/en/dev/`](http://textblob.readthedocs.io/en/dev/)

1.  [`spacy.io/usage/facts-figures`](https://spacy.io/usage/facts-figures)

1.  [`radimrehurek.com/gensim/index.html`](https://radimrehurek.com/gensim/index.html)

1.  [`opennlp.apache.org/`](https://opennlp.apache.org/)

1.  [`opennmt.net/`](http://opennmt.net/)

1.  [`gate.ac.uk/`](https://gate.ac.uk/)

1.  [`uima.apache.org/`](https://uima.apache.org/)

1.  [`www.clips.uantwerpen.be/pages/MBSP#tokenizer`](https://www.clips.uantwerpen.be/pages/MBSP#tokenizer)

1.  [`rapidminer.com/`](https://rapidminer.com/)

1.  [`mallet.cs.umass.edu/`](http://mallet.cs.umass.edu/)

1.  [`www.clips.uantwerpen.be/pages/pattern`](https://www.clips.uantwerpen.be/pages/pattern)

1.  [`nlp.stanford.edu/software/tokenizer.html#About`](https://nlp.stanford.edu/software/tokenizer.html#About)

1.  [`tartarus.org/martin/PorterStemmer/`](https://tartarus.org/martin/PorterStemmer/)

1.  [`www.nltk.org/api/nltk.stem.html`](http://www.nltk.org/api/nltk.stem.html)

1.  [`snowballstem.org/`](https://snowballstem.org/)

1.  [`pypi.python.org/pypi/PyStemmer/1.0.1`](https://pypi.python.org/pypi/PyStemmer/1.0.1)

1.  [`www.elastic.co/guide/en/elasticsearch/guide/current/hunspell.html`](https://www.elastic.co/guide/en/elasticsearch/guide/current/hunspell.html)

1.  [`lucene.apache.org/core/`](https://lucene.apache.org/core/)

1.  [`dkpro.github.io/dkpro-core/`](https://dkpro.github.io/dkpro-core/)

1.  [`ucrel.lancs.ac.uk/claws/`](http://ucrel.lancs.ac.uk/claws/)

1.  [`www.cis.uni-muenchen.de/~schmid/tools/TreeTagger/`](http://www.cis.uni-muenchen.de/~schmid/tools/TreeTagger/)

1.  [`en.wikipedia.org/wiki/Shallow_parsing`](https://en.wikipedia.org/wiki/Shallow_parsing)

1.  [`cogcomp.org/page/software_view/Chunker`](https://cogcomp.org/page/software_view/Chunker)

1.  [`github.com/dstl/baleen`](https://github.com/dstl/baleen)

1.  [`github.com/CogComp/cogcomp-nlp/tree/master/ner`](https://github.com/CogComp/cogcomp-nlp/tree/master/ner)

1.  [`github.com/lasigeBioTM/MER`](https://github.com/lasigeBioTM/MER)

1.  [`blog.paralleldots.com/product/dig-relevant-text-elements-entity-extraction-api/`](https://blog.paralleldots.com/product/dig-relevant-text-elements-entity-extraction-api/)

1.  [`www.opencalais.com/about-open-calais/`](http://www.opencalais.com/about-open-calais/)

1.  [`alias-i.com/lingpipe/index.html`](http://alias-i.com/lingpipe/index.html)

1.  [`github.com/glample/tagger`](https://github.com/glample/tagger)

1.  [`minorthird.sourceforge.net/old/doc/`](http://minorthird.sourceforge.net/old/doc/)

1.  [`www.ibm.com/support/knowledgecenter/en/SS8NLW_10.0.0/com.ibm.watson.wex.aac.doc/aac-tasystemt.html`](https://www.ibm.com/support/knowledgecenter/en/SS8NLW_10.0.0/com.ibm.watson.wex.aac.doc/aac-tasystemt.html)

1.  [`www.poolparty.biz/`](https://www.poolparty.biz/)

1.  [`www.basistech.com/text-analytics/rosette/entity-extractor/`](https://www.basistech.com/text-analytics/rosette/entity-extractor/)

1.  [`www.bart-coref.org/index.html`](http://www.bart-coref.org/index.html)

1.  [`wing.comp.nus.edu.sg/~qiu/NLPTools/JavaRAP.html`](https://wing.comp.nus.edu.sg/~qiu/NLPTools/JavaRAP.html)

1.  [`cswww.essex.ac.uk/Research/nle/GuiTAR/`](http://cswww.essex.ac.uk/Research/nle/GuiTAR/)

1.  [`www.cs.utah.edu/nlp/reconcile/`](https://www.cs.utah.edu/nlp/reconcile/)

1.  [`github.com/brendano/arkref`](https://github.com/brendano/arkref)

1.  [`cogcomp.org/page/software_view/Coref`](https://cogcomp.org/page/software_view/Coref)

1.  [`medium.com/huggingface/state-of-the-art-neural-coreference-resolution-for-chatbots-3302365dcf30`](https://medium.com/huggingface/state-of-the-art-neural-coreference-resolution-for-chatbots-3302365dcf30)

1.  [`github.com/smartschat/cort`](https://github.com/smartschat/cort)

1.  [`www.hlt.utdallas.edu/~altaf/cherrypicker/`](http://www.hlt.utdallas.edu/~altaf/cherrypicker/)

1.  [`nlp.lsi.upc.edu/freeling/`](http://nlp.lsi.upc.edu/freeling/)

1.  [`corpling.uis.georgetown.edu/xrenner/#`](https://corpling.uis.georgetown.edu/xrenner/)

1.  [`takelab.fer.hr/termex_s/`](http://takelab.fer.hr/termex_s/)

1.  [`www.athel.com/colloc.html`](https://www.athel.com/colloc.html)

1.  [`linghub.lider-project.eu/metashare/a89c02f4663d11e28a985ef2e4e6c59e76428bf02e394229a70428f25a839f75`](http://linghub.lider-project.eu/metashare/a89c02f4663d11e28a985ef2e4e6c59e76428bf02e394229a70428f25a839f75)

1.  [`ws.racai.ro:9191/narratives/batch2/Colloc.pdf`](http://ws.racai.ro:9191/narratives/batch2/Colloc.pdf)

1.  [`www.aclweb.org/anthology/E17-3027`](http://www.aclweb.org/anthology/E17-3027)

1.  [`metacpan.org/pod/Text::NSP`](https://metacpan.org/pod/Text::NSP)

1.  [`github.com/knowitall/reverb`](https://github.com/knowitall/reverb)

1.  [`github.com/U-Alberta/exemplar`](https://github.com/U-Alberta/exemplar)

1.  [`github.com/aoldoni/tetre`](https://github.com/aoldoni/tetre)

1.  [`www.textrazor.com/technology`](https://www.textrazor.com/technology)

1.  [`github.com/machinalis/iepy`](https://github.com/machinalis/iepy)

1.  [`www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/#relations`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/#relations)

1.  [`github.com/mit-nlp/MITIE`](https://github.com/mit-nlp/MITIE)

**Bio**: [数据怪兽](https://datamonsters.com/) 帮助企业和资助的初创公司研究、设计和开发实时智能软件，以利用数据技术改进业务。

[原文](https://medium.com/@datamonsters/text-preprocessing-in-python-steps-tools-and-examples-bf025f872908)。转载许可。

**相关内容：**

+   [自然语言处理任务的数据表示](https://www.kdnuggets.com/2018/11/data-representation-natural-language-processing.html)

+   [自助数据准备工具与企业级解决方案？6 个经验教训](https://www.kdnuggets.com/2018/08/self-service-data-prep-tools-6-lessons-learned.html)

+   [主动学习简介](https://www.kdnuggets.com/2018/10/introduction-active-learning.html)

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 为您的组织提供 IT 支持

* * *

### 更多相关内容

+   [在 Pandas 中清理和预处理文本数据以进行 NLP 任务](https://www.kdnuggets.com/cleaning-and-preprocessing-text-data-in-pandas-for-nlp-tasks)

+   [掌握数据清理和预处理技术的 7 个步骤](https://www.kdnuggets.com/2023/08/7-steps-mastering-data-cleaning-preprocessing-techniques.html)

+   [SQL LIKE 操作符示例](https://www.kdnuggets.com/2022/09/sql-like-operator-examples.html)

+   [集成学习示例](https://www.kdnuggets.com/2022/10/ensemble-learning-examples.html)

+   [选择示例以理解机器学习模型](https://www.kdnuggets.com/2022/11/picking-examples-understand-machine-learning-model.html)

+   [Python 数据预处理简单指南](https://www.kdnuggets.com/2020/07/easy-guide-data-preprocessing-python.html)
