# 如何检索增强生成使LLM变得更聪明

> 原文：[https://www.kdnuggets.com/how-retrieval-augment-generation-makes-llms-smarter](https://www.kdnuggets.com/how-retrieval-augment-generation-makes-llms-smarter)

![如何检索增强生成使LLM变得比以往更聪明](../Images/ab38ea2eabccfbf98ef8e8a9dc26f39b.png)

## 理想生成型AI vs. 现实

基础的LLM（大规模语言模型）已经阅读了它们能找到的每一个字节的文本，它们的聊天机器人对等体可以被提示进行智能对话，并被要求执行特定任务。获取全面的信息已经实现了民主化；不再需要找出正确的搜索关键词或挑选阅读的网站。然而，LLM容易唠叨，并且通常会以统计上最可能的回应来回答你想听的内容（[谄媚](https://arxiv.org/abs/2310.13548)），这是变压器模型的固有结果。从LLM的知识库中提取100%准确的信息并不总是能得到可信的结果。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行IT管理

* * *

聊天型LLM因编造不存在的科学论文或法院案件的引用而臭名昭著。[对航空公司提起诉讼的律师](https://www.theguardian.com/technology/2023/jun/23/two-us-lawyers-fined-submitting-fake-court-citations-chatgpt)包括了实际上从未发生过的法院案件的引用。[2023年的一项研究报告](https://arxiv.org/abs/2309.09401)显示，当ChatGPT被提示包含引用时，它仅在14%的时间里提供了实际存在的参考文献。伪造来源、唠叨以及为了迎合提示而提供不准确的信息被称为幻觉，这是在AI被大众全面采纳和信任之前需要克服的一大障碍。

应对LLM编造虚假来源或产生不准确内容的一种对策是检索增强生成（RAG）。RAG不仅可以减少LLM的幻觉倾向，还有其他几个优势。

这些优势包括访问更新的知识库，专业化（*例如* 提供私有数据源），赋予模型超越参数记忆中存储的信息（允许更小的模型），以及有可能从合法的参考资料中获得更多的数据。

## 什么是RAG（检索增强生成）？

检索增强生成（RAG）是一种深度学习架构，实施在LLMs和变换器网络中，通过检索相关文档或其他片段并将其添加到上下文窗口中以提供额外的信息，从而帮助LLM生成有用的响应。一个典型的RAG系统有两个主要模块：检索和生成。

![检索增强生成架构 - RAG](../Images/d69cffca46379cd698dcd2203c816120.png)

RAG的主要参考文献是Facebook的[一篇由Lewis等人撰写的论文](https://arxiv.org/abs/2005.11401)。在论文中，作者使用一对基于BERT的文档编码器通过将文本嵌入到向量格式中来转换查询和文档。这些嵌入随后用于通过最大内积搜索（MIPS）来识别前*k*（通常是5或10）个文档。顾名思义，MIPS基于查询的编码向量表示与预计算的文档向量数据库中的向量表示之间的内积（或点积）。

正如Lewis *等人* 在文章中所描述的那样，RAG的设计目的是让LLMs在处理“人类在没有外部知识来源的情况下无法合理完成的知识密集型任务”时表现得更好。考虑一下开放书籍和非开放书籍考试的对比，你就能很好地理解RAG如何补充基于LLM的系统。

## 使用Hugging Face 🤗 库的RAG

Lewis *等人* 在Hugging Face Hub上开源了他们的RAG模型，因此我们可以使用论文中相同的模型进行实验。推荐使用Python 3.8的虚拟环境，并使用virtualenv。

```py
virtualenv my_env --python=python3.8
source my_env/bin/activate
```

激活环境后，我们可以使用pip安装依赖项：来自Hugging Face的transformers和datasets，RAG使用的Facebook的FAISS库用于向量搜索，以及用于作为后端的PyTorch。

```py
pip install transformers
pip install datasets
pip install faiss-cpu==1.8.0
#https://pytorch.org/get-started/locally/ to 
#match the pytorch version to your system
pip install torch
```

Lewis *等人* 实现了两种不同版本的RAG：rag-sequence 和 rag-token。rag-sequence使用相同的检索文档来增强整个序列的生成，而rag-token可以为每个标记使用不同的片段。这两个版本都使用相同的Hugging Face类进行标记化和检索，API也基本相同，但每个版本都有一个独特的生成类。这些类是从transformers库中导入的。

```py
from transformers import RagTokenizer, RagRetriever
from transformers import RagTokenForGeneration
from transformers import RagSequenceForGeneration
```

当首次实例化具有默认“wiki_dpr”数据集的RagRetriever模型时，它将启动大规模下载（约300 GB）。如果你有一个大型数据驱动器并希望Hugging Face使用它（而不是默认的缓存文件夹），可以设置一个Shell变量HF_DATASETS_CACHE。

```py
# in the shell:
export HF_DATASETS_CACHE="/path/to/data/drive"
# ^^ add to your ~/.bashrc file if you want to set the variable
```

在下载完整的wiki_dpr数据集之前，请确保代码正常运行。为了避免大规模下载直到你准备好，你可以在实例化检索器时传递use_dummy_dataset=True。你还需要实例化一个标记器，将字符串转换为整数索引（对应于词汇表中的标记）及反之。RAG的序列版本和标记版本使用相同的标记器。RAG序列（rag-sequence）和RAG标记（rag-token）各自有经过微调的（*例如* rag-token-nq）和基础版本（*例如* rag-token-base）。

```py
tokenizer = RagTokenizer.from_pretrained(\
"facebook/rag-token-nq")

token_retriever = RagRetriever.from_pretrained(\
"facebook/rag-token-nq", \
     index_name="compressed", \
use_dummy_dataset=False)
sequence_retriever = RagRetriever.from_pretrained(\
"facebook/rag-sequence-nq", \
     index_name="compressed", \
use_dummy_dataset=False)

dummy_retriever = RagRetriever.from_pretrained(\
"facebook/rag-sequence-nq", \
   index_name="exact", \
use_dummy_dataset=True)

token_model = RagTokenForGeneration.from_pretrained(\
"facebook/rag-token-nq", \
     retriever=token_retriever)
seq_model = RagTokenForGeneration.from_pretrained(\
"facebook/rag-sequence-nq", \
     retriever=seq_retriever)
dummy_model = RagTokenForGeneration.from_pretrained(\
"facebook/rag-sequence-nq", \
   retriever=dummy_retriever)
```

一旦你的模型被实例化，你可以提供查询，进行标记化，然后将其传递给模型的“generate”函数。我们将比较使用带有dummy版本wiki_dpr数据集的rag-sequence、rag-token和RAG的结果。请注意，这些rag模型是不区分大小写的。

```py
query = "what is the name of the oldest tree on Earth?"
input_dict = tokenizer.prepare_seq2seq_batch(\
query, return_tensors="pt")

token_generated = token_model.generate(**input_dict)  token_decoded = token_tokenizer.batch_decode(\
token_generated, skip_special_tokens=True)

seq_generated = seq_model.generate(**input_dict)
seq_decoded = seq_tokenizer.batch_decode(\
seq_generated, skip_special_tokens=True)

dummy_generated = dummy_model.generate(**input_dict)
dummy_decoded = seq_tokenizer.batch_decode(\
dummy_generated, skip_special_tokens=True)

print(f"answers to query '{query}': ")
print(f"\t rag-sequence-nq: {seq_decoded[0]},"\
f" rag-token-nq: {token_decoded[0]},"\
f" rag (dummy): {dummy_decoded[0]}")
```

```py
>> answers to query 'What is the name of the oldest tree on Earth?': Prometheus was the oldest tree discovered until 2012, with its innermost, extant rings exceeding 4862 years of age.

>> rag-sequence-nq:  prometheus, rag-token-nq:  prometheus, rag (dummy):  4862
```

通常，rag-token的正确率高于rag-sequence（尽管两者都经常正确），而rag-sequence的正确率高于使用dummy数据集的RAG。

“检索器提供了什么样的上下文？”你可能会好奇。为了找出答案，我们可以解构生成过程。使用上面实例化的seq_retriever和seq_model，我们查询“地球上最古老的树叫什么名字”

```py
query = "what is the name of the oldest tree on Earth?"
inputs = tokenizer(query, return_tensors="pt")
input_ids = inputs["input_ids"]

question_hidden_states = seq_model.question_encoder(input_ids)[0]

docs_dict = seq_retriever(input_ids.numpy(),\
question_hidden_states.detach().numpy(),\
return_tensors="pt")
doc_scores = torch.bmm(\
question_hidden_states.unsqueeze(1),\
docs_dict["retrieved_doc_embeds"]\
.float().transpose(1, 2)).squeeze(1)

generated = model.generate(\
context_input_ids=docs_dict["context_input_ids"],\
context_attention_mask=\
docs_dict["context_attention_mask"],\
doc_scores=doc_scores)

generated_string = tokenizer.batch_decode(\
generated,\
skip_special_tokens=True)

contexts = tokenizer.batch_decode(\
docs_dict["context_input_ids"],\
attention_mask=docs_dict["context_attention_mask"],\
skip_special_tokens=True)

best_context = contexts[doc_scores.argmax()]
```

我们可以编写代码让模型打印“best context”变量，以查看捕获了什么。

```py
print(f" based on the retrieved context"\
f":\n\n\t {best_context}: \n")
```

基于检索到的上下文：

```py
Prometheus (tree) / In a clonal organism, however, the individual clonal stems are not nearly so old, and no part of the organism is particularly old at any given time. Until 2012, Prometheus was thus the oldest "non-clonal" organism yet discovered, with its innermost, extant rings exceeding 4862 years of age. In the 1950s dendrochronologists were making active efforts to find the oldest living tree species in order to use the analysis of the rings for various research purposes, such as the evaluation of former climates, the dating of archaeological ruins, and addressing the basic scientific question of maximum potential lifespan. Bristlecone pines // what is the name of the oldest tree on earth?
```

```py
print(f" rag-sequence-nq answers '{query}'"\
f" with '{generated_string[0]}'")
```

我们还可以通过调用`generated_string`变量来打印答案。rag-sequence-nq回答“地球上最古老的树叫什么名字？”为“Prometheus”。

## 你可以用RAG做什么？

在过去一年半里，LLMs和LLM工具经历了真正的爆炸性增长。Lewis *等* 使用的BART基础模型仅有4亿个参数，与当前的LLMs相比，后者通常以十亿参数级别的“轻量”变体为起点。此外，许多正在训练、合并和微调的模型都是多模态的，将文本输入和输出与图像或其他标记化的数据源结合起来。将RAG与其他工具结合可以构建复杂的能力，但基础模型仍然不能免于常见LLM缺陷。谄媚、幻觉和可靠性的问题依然存在，并有可能随着LLM的使用增长而加剧。

RAG最明显的应用是对话语义搜索的变体，但它们也可能包括将多模态输入或图像生成作为输出的一部分。例如，具有领域知识的LLM中的RAG可以生成你可以聊天的软件文档。或者RAG可以用于在文献综述的研究项目或论文中保持互动笔记。

通过整合“链式思维”推理能力，你可以采取更具代理性的方式，赋能你的模型查询RAG系统并组装更复杂的询问或推理路径。

还需要特别注意的是，RAG 并不能解决常见的大型语言模型（LLM）问题（如幻觉、谄媚等），它仅作为一种缓解或引导你的 LLM 达到更专业回应的手段。最终重要的是，取决于你的使用案例、你提供给模型的信息以及模型的微调方式。

**[Kevin Vu](https://blog.exxactcorp.com/)** 负责管理 [Exxact Corp 博客](https://blog.exxactcorp.com/)，并与许多才华横溢的作者合作，他们撰写关于深度学习不同方面的文章。

### 更多相关话题

+   [检索增强生成：信息检索与…的结合](https://www.kdnuggets.com/retrieval-augmented-generation-where-information-retrieval-meets-text-generation)

+   [让智能文档处理更智能：第 1 部分](https://www.kdnuggets.com/2023/02/making-intelligent-document-processing-smarter-part-1.html)

+   [如何优化 SQL 查询以加快数据检索速度](https://www.kdnuggets.com/2023/06/optimize-sql-queries-faster-data-retrieval.html)

+   [Bark：终极音频生成模型](https://www.kdnuggets.com/2023/05/bark-ultimate-audio-generation-model.html)

+   [人工智能的未来：探索下一代生成模型](https://www.kdnuggets.com/2023/05/future-ai-exploring-next-generation-generative-models.html)

+   [揭示 Midjourney 5.2：AI 图像生成的跃进](https://www.kdnuggets.com/2023/06/unveiling-midjourney-52-leap-forward.html)
