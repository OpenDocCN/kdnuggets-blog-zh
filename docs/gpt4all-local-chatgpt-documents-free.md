# GPT4All 是本地 ChatGPT，用于您的文档，并且它是免费的！

> 原文：[https://www.kdnuggets.com/2023/06/gpt4all-local-chatgpt-documents-free.html](https://www.kdnuggets.com/2023/06/gpt4all-local-chatgpt-documents-free.html)

在这篇文章中，我们将学习 **如何在仅使用 CPU 的计算机上部署和使用 GPT4All 模型**（我使用的是 *Macbook Pro*，没有 GPU！）

![GPT4All 是本地 ChatGPT，用于您的文档，并且它是免费的！](../Images/164c034be698c48aeca6a7141b048cc9.png)

在您的计算机上使用 GPT4All — 图片由作者提供

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您组织的 IT

* * *

在这篇文章中，我们将安装 GPT4All（一个强大的大型语言模型）到本地计算机，并发现如何用 Python 与文档交互。一组 PDF 或在线文章将作为我们问题/答案的知识库。

# 什么是 GPT4All

从 [官方 GPT4All 网站](https://gpt4all.io/index.html) 可以看到，它被描述为 *一个免费使用、在本地运行、注重隐私的聊天机器人。* **无需 GPU 或互联网。**

GTP4All 是一个生态系统，用于训练和部署 **强大的** 和 **定制化的** 大型语言模型，这些模型在 **消费者级** CPU 上 **本地** 运行。

我们的 GPT4All 模型是一个 4GB 的文件，您可以下载并接入 GPT4All 开源生态系统软件。 *Nomic AI* 促进高质量和安全的软件生态系统，推动个人和组织轻松在本地训练和实现自己的大型语言模型。

# 它将如何运作？

![GPT4All 是本地 ChatGPT，用于您的文档，并且它是免费的！](../Images/d2645a736b7389d955c7058f9ea42dc4.png)

GPT4All 的问答工作流程 — 作者创建

这个过程非常简单（当你知道如何做时），也可以用其他模型重复。步骤如下：

+   加载 GPT4All 模型

+   使用 *Langchain* 来检索我们的文档并加载它们

+   将文档拆分成适合嵌入的较小块

+   使用 FAISS 创建我们的向量数据库并加入嵌入

+   对我们的向量数据库执行相似度搜索（语义搜索），根据我们要传递给 GPT4All 的问题：这将作为我们问题的 *上下文*

+   用 *Langchain* 将问题和上下文提供给 GPT4All，并等待答案。

所以我们需要的是嵌入。嵌入是信息的数值表示，例如文本、文档、图像、音频等。这种表示捕捉了被嵌入内容的语义意义，这正是我们所需要的。对于这个项目，我们不能依赖重型 GPU 模型：因此我们将下载 Alpaca 原生模型并从 *Langchain* 使用 *LlamaCppEmbeddings*。别担心！一切都会一步一步地解释清楚

# 开始编码吧

## 创建虚拟环境

创建一个新的文件夹用于你的新 Python 项目，例如 GPT4ALL_Fabio（放上你的名字……）：

```py
mkdir GPT4ALL_Fabio
cd GPT4ALL_Fabio
```

接下来，创建一个新的 Python 虚拟环境。如果你安装了多个 Python 版本，请指定你想要的版本：在这个例子中，我将使用我的主要安装，与 Python 3.10 相关联。

```py
python3 -m venv .venv
```

命令 `python3 -m venv .venv` 创建了一个名为 `.venv` 的新虚拟环境（点会创建一个名为 venv 的隐藏目录）。

虚拟环境提供了一个隔离的 Python 安装，它允许你只为特定项目安装包和依赖项，而不会影响系统范围的 Python 安装或其他项目。这种隔离有助于保持一致性并防止不同项目需求之间的潜在冲突。

一旦虚拟环境创建完成，你可以使用以下命令激活它：

```py
source .venv/bin/activate
```

![GPT4All 是你的文档的本地 ChatGPT，并且是免费的！](../Images/3ee7e916d5dcc6c4d1a6da8715b77627.png)

已激活虚拟环境

## 需要安装的库

对于我们正在构建的项目，我们不需要太多的包。我们只需要：

+   GPT4All 的 Python 绑定

+   Langchain 与我们的文档进行交互

LangChain 是一个用于开发由语言模型驱动的应用程序的框架。它不仅允许你通过 API 调用语言模型，还可以将语言模型连接到其他数据源，并允许语言模型与其环境进行交互。

```py
pip install pygpt4all==1.0.1
pip install pyllamacpp==1.0.6
pip install langchain==0.0.149
pip install unstructured==0.6.5
pip install pdf2image==1.16.3
pip install pytesseract==0.3.10
pip install pypdf==3.8.1
pip install faiss-cpu==1.7.4
```

对于 LangChain，你会看到我们也指定了版本。这个库最近收到很多更新，因此为了确保我们的设置明天也能正常工作，最好指定一个我们知道运行良好的版本。Unstructured 是 pdf loader 的必需依赖，*pytesseract* 和 *pdf2image* 也是如此。

**注意**：在 GitHub 仓库中有一个 requirements.txt 文件（由 [jl adcr](https://medium.com/u/816bd47943c0?source=post_page-----df1016bc335--------------------------------) 提供），其中包含与此项目相关的所有版本。你可以在下载到主项目文件目录后，使用以下命令一次性完成安装：

```py
pip install -r requirements.txt
```

在文章末尾，我创建了一个[故障排除部分](https://artificialcorner.com/gpt4all-is-the-local-chatgpt-for-your-documents-and-it-is-free-df1016bc335#3706)。GitHub 仓库也更新了包含所有这些信息的 READ.ME 文件。

请记住，一些 **库的版本取决于你在虚拟环境中运行的 python 版本**。

## 在你的 PC 上下载模型

这是一个非常重要的步骤。

对于项目，我们确实需要 GPT4All。Nomic AI 上描述的过程非常复杂，并且需要不是我们所有人都有的硬件（像我）。所以 [这是转换后的模型链接](https://huggingface.co/mrgaang/aira/blob/main/gpt4all-converted.bin)，已经转换并准备使用。只需点击下载。

![GPT4All 是你的文档本地 ChatGPT，而且是免费的！](../Images/d2c094d736d30e97efaf5e8ca711fa8b.png)

下载 GPT4All 模型

正如简介中简要描述的，我们还需要用于嵌入的模型，一个可以在 CPU 上运行而不会崩溃的模型。点击 [这里的链接下载 alpaca-native-7B-ggml](https://huggingface.co/Pi3141/alpaca-native-7B-ggml/tree/397e872bf4c83f4c642317a5bf65ce84a105786e)，它已经转换为 4-bit，并准备作为我们的嵌入模型使用。

![GPT4All 是你的文档本地 ChatGPT，而且是免费的！](../Images/c7ba2460adee0a81425d5fdcdd3541d4.png)

点击 [ggml-model-q4_0.bin](https://huggingface.co/Pi3141/alpaca-native-7B-ggml/blob/397e872bf4c83f4c642317a5bf65ce84a105786e/ggml-model-q4_0.bin) 旁边的下载箭头

为什么我们需要嵌入？如果你记得流程图，收集知识库文档后的第一步是 *嵌入* 它们。来自 Alpaca 模型的 LLamaCPP 嵌入非常适合这个任务，而且这个模型也相当小（4 Gb）。顺便说一下，你也可以使用 Alpaca 模型进行问答！

更新于 2023.05.25：Mani Windows 用户在使用 llamaCPP 嵌入时遇到问题。这主要是因为在安装 python 包 llama-cpp-python 时：

```py
pip install llama-cpp-python
```

pip 包将从源代码编译库。Windows 通常默认未安装 CMake 或 C 编译器。但不用担心，这里有解决方案。

在 Windows 上安装 llama-cpp-python（LangChain 所需的 llamaEmbeddings）时，CMake C 编译器默认未安装，因此你无法从源代码构建。

在 Mac 用户使用 Xtools 和 Linux 上，通常 C 编译器已在操作系统中提供。

为了避免这个问题 **你必须使用预编译的 wheel**。

前往 [https://github.com/abetlen/llama-cpp-python/releases](https://github.com/abetlen/llama-cpp-python/releases)

并且寻找与你的架构和 python 版本相匹配的编译好的 wheel — **你必须使用 Weels 版本 0.1.49**，因为更高版本不兼容。

![GPT4All 是你的文档本地 ChatGPT，而且是免费的！](../Images/059c34d6ca9fb76e5db455d7252a6238.png)

截图来自 [https://github.com/abetlen/llama-cpp-python/releases](https://github.com/abetlen/llama-cpp-python/releases)

我的情况是 Windows 10，64 位，python 3.10

所以我的文件是 llama_cpp_python-0.1.49-cp310-cp310-win_amd64.whl

这个[问题在 GitHub 仓库中跟踪](https://github.com/fabiomatricardi/GPT4All_Medium/issues/2)

下载后，你需要将两个模型放入模型目录，如下所示。

![GPT4All 是你文档的本地 ChatGPT，并且是免费的！](../Images/2fdc93855b6963db59ee794c8fd8e21f.png)

目录结构以及模型文件的放置位置

# 与 GPT4All 的基本互动

由于我们想要控制与 GPT 模型的交互，我们需要创建一个 Python 文件（我们称之为*pygpt4all_test.py*），导入依赖项并给模型下达指令。你会发现这很简单。

```py
from pygpt4all.models.gpt4all import GPT4All
```

这是我们模型的 Python 绑定。现在我们可以调用它并开始提问。让我们试一个创意的。

我们创建了一个函数来读取模型的回调，并要求 GPT4All 完成我们的句子。

```py
def new_text_callback(text):
    print(text, end="")

model = GPT4All('./models/gpt4all-converted.bin')
model.generate("Once upon a time, ", n_predict=55, new_text_callback=new_text_callback)
```

第一个声明是告诉我们的程序在哪里找到模型（记住我们在上面部分做了什么）

第二个声明是要求模型生成回应，并完成我们的提示“从前，一次…”

要运行它，请确保虚拟环境仍然激活，然后简单地运行：

```py
python3 pygpt4all_test.py
```

你应该看到模型的加载文本和句子的完成。根据你的硬件资源，这可能需要一点时间。

![GPT4All 是你文档的本地 ChatGPT，并且是免费的！](../Images/2c46d0381e481042610ded3f36e4bde4.png)

结果可能与您的不同……但对我们来说，重要的是它在工作，我们可以继续使用 LangChain 创建一些高级功能。

**注意（更新于 2023.05.23）**：如果遇到与 pygpt4all 相关的错误，请查看本主题的故障排除部分，解决方案由[Rajneesh Aggarwal](https://artificialcorner.com/gpt4all-is-the-local-chatgpt-for-your-documents-and-it-is-free-df1016bc335#25f6) 或 [Oscar Jeong](https://artificialcorner.com/gpt4all-is-the-local-chatgpt-for-your-documents-and-it-is-free-df1016bc335#a6a6) 提供。

# LangChain 在 GPT4All 上的模板

LangChain 框架是一个非常了不起的库。它提供了 *组件* 以一种易于使用的方式与语言模型进行工作，同时也提供了 *链*。链可以被视为以特定方式组装这些组件，以最佳方式完成特定用例。这些旨在成为一个高级接口，通过它人们可以轻松开始特定用例。这些链也被设计为可自定义的。

在我们下一个 Python 测试中，我们将使用 *Prompt Template*。语言模型接受文本作为输入——这些文本通常被称为提示。通常，这不仅仅是一个硬编码的字符串，而是模板、一些示例和用户输入的组合。LangChain 提供了几个类和函数，使构造和使用提示变得容易。让我们看看如何做到这一点。

创建一个新的 Python 文件，并称其为 *my_langchain.py*

```py
# Import of langchain Prompt Template and Chain
from langchain import PromptTemplate, LLMChain

# Import llm to be able to interact with GPT4All directly from langchain
from langchain.llms import GPT4All

# Callbacks manager is required for the response handling 
from langchain.callbacks.base import CallbackManager
from langchain.callbacks.streaming_stdout import StreamingStdOutCallbackHandler

local_path = './models/gpt4all-converted.bin' 
callback_manager = CallbackManager([StreamingStdOutCallbackHandler()])
```

我们从 LangChain 中导入了 Prompt Template 和 Chain 以及 GPT4All llm 类，以便能够直接与我们的 GPT 模型进行交互。

然后，在设置好我们的 llm 路径（如我们之前所做的）之后，我们实例化回调管理器，以便能够捕获对我们查询的响应。

创建模板其实非常简单：按照 [文档教程](https://python.langchain.com/en/latest/modules/prompts/prompt_templates/getting_started.html) 的指导，我们可以使用类似这样的东西……

```py
template = """Question: {question}

Answer: Let's think step by step on it.

"""
prompt = PromptTemplate(template=template, input_variables=["question"])
```

*template* 变量是一个多行字符串，包含了我们与模型交互的结构：在花括号中插入外部变量，在我们的场景中是我们的 *question*。

由于这是一个变量，你可以决定它是一个硬编码问题还是用户输入的问题：以下是两个示例。

```py
# Hardcoded question
question = "What Formula 1 pilot won the championship in the year Leonardo di Caprio was born?"

# User input question...
question = input("Enter your question: ")
```

对于我们的测试运行，我们将注释掉用户输入部分。现在我们只需要将模板、问题和语言模型连接起来。

```py
template = """Question: {question}
Answer: Let's think step by step on it.
"""

prompt = PromptTemplate(template=template, input_variables=["question"])

# initialize the GPT4All instance
llm = GPT4All(model=local_path, callback_manager=callback_manager, verbose=True)

# link the language model with our prompt template
llm_chain = LLMChain(prompt=prompt, llm=llm)

# Hardcoded question
question = "What Formula 1 pilot won the championship in the year Leonardo di Caprio was born?"

# User imput question...
# question = input("Enter your question: ")

#Run the query and get the results
llm_chain.run(question)
```

记得检查你的虚拟环境是否仍然激活，并运行以下命令：

```py
python3 my_langchain.py
```

你可能会得到与我不同的结果。令人惊讶的是，你可以看到 GPT4All 尝试为你找到答案的整个推理过程。调整问题也可能会给你更好的结果。

![GPT4All 是您文档的本地 ChatGPT，它是免费的！](../Images/37f6fc0d50f8004779fd138b45aca20f.png)

Langchain 与 GPT4All 上的 Prompt Template

# 使用 LangChain 和 GPT4All 回答有关您文档的问题

现在我们进入了令人兴奋的部分，因为我们将使用 GPT4All 作为聊天机器人与我们的文档对话，回答我们的提问。

步骤序列，参见 *QnA with GPT4All 的工作流程*，是加载我们的 PDF 文件，将它们拆分成块。之后我们将需要一个向量存储来存储我们的嵌入。我们需要将分块的文档放入向量存储中进行信息检索，然后将它们与此数据库上的相似性搜索一起嵌入，作为我们 LLM 查询的上下文。

为此目的，我们将直接使用 *Langchain* 库中的 FAISS。FAISS 是 Facebook AI Research 提供的开源库，旨在快速找到大规模高维数据集中相似的项目。它提供了索引和搜索方法，使得在数据集中快速找到最相似的项目变得更加容易和迅速。它对我们特别方便，因为它简化了 **信息检索** 并允许我们将创建的数据库本地保存：这意味着在第一次创建之后，它会非常快速地加载以便进一步使用。

## 向量索引数据库的创建

创建一个新文件，并命名为 *my_knowledge_qna.py*

```py
from langchain import PromptTemplate, LLMChain
from langchain.llms import GPT4All
from langchain.callbacks.base import CallbackManager
from langchain.callbacks.streaming_stdout import StreamingStdOutCallbackHandler

# function for loading only TXT files
from langchain.document_loaders import TextLoader

# text splitter for create chunks
from langchain.text_splitter import RecursiveCharacterTextSplitter

# to be able to load the pdf files
from langchain.document_loaders import UnstructuredPDFLoader
from langchain.document_loaders import PyPDFLoader
from langchain.document_loaders import DirectoryLoader

# Vector Store Index to create our database about our knowledge
from langchain.indexes import VectorstoreIndexCreator

# LLamaCpp embeddings from the Alpaca model
from langchain.embeddings import LlamaCppEmbeddings

# FAISS  library for similaarity search
from langchain.vectorstores.faiss import FAISS

import os  #for interaaction with the files
import datetime
```

第一个库与我们之前使用的相同：此外，我们还使用了 *Langchain* 来创建向量存储索引，*LlamaCppEmbeddings* 用于与我们的 Alpaca 模型（量化为 4 位并与 cpp 库编译）交互，以及 PDF 加载器。

让我们还加载带有各自路径的 LLM：一个用于嵌入，另一个用于文本生成。

```py
# assign the path for the 2 models GPT4All and Alpaca for the embeddings 
gpt4all_path = './models/gpt4all-converted.bin' 
llama_path = './models/ggml-model-q4_0.bin' 
# Calback manager for handling the calls with  the model
callback_manager = CallbackManager([StreamingStdOutCallbackHandler()])

# create the embedding object
embeddings = LlamaCppEmbeddings(model_path=llama_path)
# create the GPT4All llm object
llm = GPT4All(model=gpt4all_path, callback_manager=callback_manager, verbose=True)
```

为了测试，我们来看看是否成功读取了所有的 pfd 文件：第一步是声明三个函数，用于每一个单独的文档。第一个是将提取的文本分割成块，第二个是使用元数据（如页码等）创建向量索引，最后一个是测试相似性搜索（稍后我会详细解释）。

```py
# Split text 
def split_chunks(sources):
    chunks = []
    splitter = RecursiveCharacterTextSplitter(chunk_size=256, chunk_overlap=32)
    for chunk in splitter.split_documents(sources):
        chunks.append(chunk)
    return chunks

def create_index(chunks):
    texts = [doc.page_content for doc in chunks]
    metadatas = [doc.metadata for doc in chunks]

    search_index = FAISS.from_texts(texts, embeddings, metadatas=metadatas)

    return search_index

def similarity_search(query, index):
    # k is the number of similarity searched that matches the query
    # default is 4
    matched_docs = index.similarity_search(query, k=3) 
    sources = []
    for doc in matched_docs:
        sources.append(
            {
                "page_content": doc.page_content,
                "metadata": doc.metadata,
            }
        )

    return matched_docs, sources
```

现在我们可以测试 *docs* 目录中文档的索引生成：我们需要将所有 PDF 文件放到那里。*Langchain* 也有一个加载整个文件夹的方法，无论文件类型如何：由于后处理比较复杂，我将在下一篇关于 LaMini 模型的文章中介绍。

![GPT4All 是您文档的本地 ChatGPT，并且是免费的！](../Images/08db5a1bdd2f487dab7db09671d6910f.png)

我的 docs 目录包含 4 个 PDF 文件。

我们将对列表中的第一个文档应用我们的函数。

```py
# get the list of pdf files from the docs directory into a list  format
pdf_folder_path = './docs'
doc_list = [s for s in os.listdir(pdf_folder_path) if s.endswith('.pdf')]
num_of_docs = len(doc_list)
# create a loader for the PDFs from the path
loader = PyPDFLoader(os.path.join(pdf_folder_path, doc_list[0]))
# load the documents with Langchain
docs = loader.load()
# Split in chunks
chunks = split_chunks(docs)
# create the db vector index
db0 = create_index(chunks)
```

在前几行中，我们使用 os 库来获取 docs 目录中的 **pdf 文件列表**。然后，我们用 *Langchain* 从 docs 文件夹加载第一个文档 (*doc_list[0]*)，将其分割成块，然后用 *LLama* 嵌入创建向量数据库。

正如你所看到的，我们正在使用 [pyPDF 方法](https://python.langchain.com/en/latest/modules/indexes/document_loaders/examples/pdf.html?highlight=pypdf#using-pypdf)。这个方法使用起来有点长，因为你需要逐个加载文件，但使用 `pypdf` 加载 PDF 到文档数组中可以让你拥有一个数组，其中每个文档包含页面内容和带有 `page` 号的元数据。这在你想知道我们将给 GPT4All 查询的上下文来源时非常方便。这里是来自 readthedocs 的示例：

![GPT4All 是您文档的本地 ChatGPT，并且是免费的！](../Images/57c5bcbffab238b4be4b1c8fdabbfa8f.png)

来自 [Langchain 文档](https://python.langchain.com/en/latest/modules/indexes/document_loaders/examples/pdf.html) 的截图

我们可以通过终端命令运行 Python 文件：

```py
python3 my_knowledge_qna.py
```

在加载嵌入模型后，你会看到用于索引的 token 在工作：不要惊慌，因为这会花费一些时间，特别是如果你只在 CPU 上运行，就像我一样（花了 8 分钟）。

![GPT4All 是您文档的本地 ChatGPT，并且是免费的！](../Images/7c0e84382e2c6cec24cb602d31b91ab4.png)

第一个向量数据库的完成

正如我所解释的，pyPDF 方法较慢，但可以为我们提供额外的数据用于相似性搜索。为了遍历所有文件，我们将使用 FAISS 提供的一个便捷方法，它允许我们将不同的数据库合并在一起。我们现在所做的是使用上述代码生成第一个数据库（我们称之为 *db0*），然后使用 for 循环创建列表中下一个文件的索引，并立即将其与 *db0* 合并。

这是代码：注意我添加了一些日志，用*datetime.datetime.now()* 显示进度状态，并打印结束时间和开始时间的差值来计算操作花费了多长时间（如果你不喜欢可以删除它）。

合并指令如下

```py
# merge dbi with the existing db0
db0.merge_from(dbi)
```

最后一条指令是将我们的数据库保存在本地：整个生成过程可能需要几个小时（取决于你有多少文档），所以我们只需做一次真的很不错！

```py
# Save the databasae locally
db0.save_local("my_faiss_index")
```

这是完整的代码。当我们与 GPT4All 互动时，我们将直接从文件夹加载索引，并注释掉代码的许多部分。

```py
# get the list of pdf files from the docs directory into a list  format
pdf_folder_path = './docs'
doc_list = [s for s in os.listdir(pdf_folder_path) if s.endswith('.pdf')]
num_of_docs = len(doc_list)
# create a loader for the PDFs from the path
general_start = datetime.datetime.now() #not used now but useful
print("starting the loop...")
loop_start = datetime.datetime.now() #not used now but useful
print("generating fist vector database and then iterate with .merge_from")
loader = PyPDFLoader(os.path.join(pdf_folder_path, doc_list[0]))
docs = loader.load()
chunks = split_chunks(docs)
db0 = create_index(chunks)
print("Main Vector database created. Start iteration and merging...")
for i in range(1,num_of_docs):
    print(doc_list[i])
    print(f"loop position {i}")
    loader = PyPDFLoader(os.path.join(pdf_folder_path, doc_list[i]))
    start = datetime.datetime.now() #not used now but useful
    docs = loader.load()
    chunks = split_chunks(docs)
    dbi = create_index(chunks)
    print("start merging with db0...")
    db0.merge_from(dbi)
    end = datetime.datetime.now() #not used now but useful
    elapsed = end - start #not used now but useful
    #total time
    print(f"completed in {elapsed}")
    print("-----------------------------------")
loop_end = datetime.datetime.now() #not used now but useful
loop_elapsed = loop_end - loop_start #not used now but useful
print(f"All documents processed in {loop_elapsed}")
print(f"the daatabase is done with {num_of_docs} subset of db index")
print("-----------------------------------")
print(f"Merging completed")
print("-----------------------------------")
print("Saving Merged Database Locally")
# Save the databasae locally
db0.save_local("my_faiss_index")
print("-----------------------------------")
print("merged database saved as my_faiss_index")
general_end = datetime.datetime.now() #not used now but useful
general_elapsed = general_end - general_start #not used now but useful
print(f"All indexing completed in {general_elapsed}")
print("-----------------------------------")
```

![GPT4All 是你文档的本地 ChatGPT，而且是免费的！](../Images/b7462d11c62eb0ad963007bac8f2c01f.png)运行 Python 文件花费了 22 分钟

## 向 GPT4All 提问关于你的文档

现在我们在这里了。我们有了索引，可以加载它，并通过提示模板要求 GPT4All 回答我们的问题。我们从一个硬编码的问题开始，然后我们将循环遍历输入的问题。

将以下代码放入 Python 文件*db_loading.py*中，并使用终端命令*python3 db_loading.py*运行它

```py
from langchain import PromptTemplate, LLMChain
from langchain.llms import GPT4All
from langchain.callbacks.base import CallbackManager
from langchain.callbacks.streaming_stdout import StreamingStdOutCallbackHandler
# function for loading only TXT files
from langchain.document_loaders import TextLoader
# text splitter for create chunks
from langchain.text_splitter import RecursiveCharacterTextSplitter
# to be able to load the pdf files
from langchain.document_loaders import UnstructuredPDFLoader
from langchain.document_loaders import PyPDFLoader
from langchain.document_loaders import DirectoryLoader
# Vector Store Index to create our database about our knowledge
from langchain.indexes import VectorstoreIndexCreator
# LLamaCpp embeddings from the Alpaca model
from langchain.embeddings import LlamaCppEmbeddings
# FAISS  library for similaarity search
from langchain.vectorstores.faiss import FAISS
import os  #for interaaction with the files
import datetime

# TEST FOR SIMILARITY SEARCH

# assign the path for the 2 models GPT4All and Alpaca for the embeddings 
gpt4all_path = './models/gpt4all-converted.bin' 
llama_path = './models/ggml-model-q4_0.bin' 
# Calback manager for handling the calls with  the model
callback_manager = CallbackManager([StreamingStdOutCallbackHandler()])

# create the embedding object
embeddings = LlamaCppEmbeddings(model_path=llama_path)
# create the GPT4All llm object
llm = GPT4All(model=gpt4all_path, callback_manager=callback_manager, verbose=True)

# Split text 
def split_chunks(sources):
    chunks = []
    splitter = RecursiveCharacterTextSplitter(chunk_size=256, chunk_overlap=32)
    for chunk in splitter.split_documents(sources):
        chunks.append(chunk)
    return chunks

def create_index(chunks):
    texts = [doc.page_content for doc in chunks]
    metadatas = [doc.metadata for doc in chunks]

    search_index = FAISS.from_texts(texts, embeddings, metadatas=metadatas)

    return search_index

def similarity_search(query, index):
    # k is the number of similarity searched that matches the query
    # default is 4
    matched_docs = index.similarity_search(query, k=3) 
    sources = []
    for doc in matched_docs:
        sources.append(
            {
                "page_content": doc.page_content,
                "metadata": doc.metadata,
            }
        )

    return matched_docs, sources

# Load our local index vector db
index = FAISS.load_local("my_faiss_index", embeddings)
# Hardcoded question
query = "What is a PLC and what is the difference with a PC"
docs = index.similarity_search(query)
# Get the matches best 3 results - defined in the function k=3
print(f"The question is: {query}")
print("Here the result of the semantic search on the index, without GPT4All..")
print(docs[0])
```

打印的文本是与查询最匹配的 3 个来源的列表，还包括文档名称和页码。

![GPT4All 是你文档的本地 ChatGPT，而且是免费的！](../Images/cb666d32a09071a7e154c4b2610c1394.png)

运行文件*db_loading.py*的语义搜索结果

现在我们可以使用相似性搜索作为我们查询的上下文，通过提示模板进行操作。在这 3 个函数之后，将所有代码替换为以下内容：

```py
# Load our local index vector db
index = FAISS.load_local("my_faiss_index", embeddings)

# create the prompt template
template = """
Please use the following context to answer questions.
Context: {context}
---
Question: {question}
Answer: Let's think step by step."""

# Hardcoded question
question = "What is a PLC and what is the difference with a PC"
matched_docs, sources = similarity_search(question, index)
# Creating the context
context = "\n".join([doc.page_content for doc in matched_docs])
# instantiating the prompt template and the GPT4All chain
prompt = PromptTemplate(template=template, input_variables=["context", "question"]).partial(context=context)
llm_chain = LLMChain(prompt=prompt, llm=llm)
# Print the result
print(llm_chain.run(question))
```

运行后你将得到类似这样的结果（但可能有所不同）。惊人吧！？!

```py
Please use the following context to answer questions.
Context: 1.What is a PLC
2.Where and Why it is used
3.How a PLC is different from a PC
PLC is especially important in industries where safety and reliability are
critical, such as manufacturing plants, chemical plants, and power plants.
How a PLC is different from a PC
Because a PLC is a specialized computer used in industrial and
manufacturing applications to control machinery and processes.,the
hardware components of a typical PLC must be able to interact with
industrial device. So a typical PLC hardware include:
---
Question: What is a PLC and what is the difference with a PC
Answer: Let's think step by step. 1) A Programmable Logic Controller (PLC), 
also called Industrial Control System or ICS, refers to an industrial computer 
that controls various automated processes such as manufacturing 
machines/assembly lines etcetera through sensors and actuators connected 
with it via inputs & outputs. It is a form of digital computers which has 
the ability for multiple instruction execution (MIE), built-in memory 
registers used by software routines, Input Output interface cards(IOC) 
to communicate with other devices electronically/digitally over networks 
or buses etcetera
2). A Programmable Logic Controller is widely utilized in industrial 
automation as it has the ability for more than one instruction execution. 
It can perform tasks automatically and programmed instructions, which allows 
it to carry out complex operations that are beyond a 
Personal Computer (PC) capacity. So an ICS/PLC contains built-in memory 
registers used by software routines or firmware codes etcetera but 
PC doesn't contain them so they need external interfaces such as 
hard disks drives(HDD), USB ports, serial and parallel 
communication protocols to store data for further analysis or 
report generation.
```

如果你想用用户输入的问题来替换这一行

```py
question = "What is a PLC and what is the difference with a PC"
```

用这样的东西：

```py
question = input("Your question: ")
```

# 结论

现在是你进行实验的时候了。对与你的文档相关的所有主题提出不同的问题，看看结果如何。确实有很大的改进空间，尤其是在提示和模板上：你可以查看[这里的一些灵感](https://github.com/hwchase17/langchain/blob/master/docs/modules/prompts/prompt_templates/examples/custom_prompt_template.ipynb)。但*Langchain*文档真的很棒（我可以跟着它！！）。

你可以按照文章中的代码，或者在[我的 GitHub 仓库](https://github.com/fabiomatricardi/GPT4All_Medium)中查看它。

**[Fabio Matricardi](https://www.linkedin.com/in/fabio-matricardi-29354835/)** 是一位教育工作者、教师、工程师和学习爱好者。他已在年轻学生中教授了 15 年课程，现在在 Key Solution Srl 培训新员工。他在 2010 年开始了工业自动化工程师的职业生涯。自青少年时期起，他就对编程充满热情，发现了构建软件和人机界面的美妙，以便赋予事物生命。教学和辅导是他的日常工作的一部分，同时他也在学习和研究如何成为一名充满激情的领导者，掌握最新的管理技能。加入我，一同踏上设计改进之旅，利用机器学习和人工智能在整个工程生命周期中实现预测系统集成。

[原始](https://artificialcorner.com/gpt4all-is-the-local-chatgpt-for-your-documents-and-it-is-free-df1016bc335)。转载经许可。

### 更多相关话题

+   [使用 BERT 对长文本文档进行分类](https://www.kdnuggets.com/2022/02/classifying-long-text-documents-bert.html)

+   [使用 tfidfvectorizer 将文本文档转换为 TF-IDF 矩阵](https://www.kdnuggets.com/2022/09/convert-text-documents-tfidf-matrix-tfidfvectorizer.html)

+   [使用 CountVectorizer 将文本文档转换为词频计数](https://www.kdnuggets.com/2022/10/converting-text-documents-token-counts-countvectorizer.html)

+   [LangChain + Streamlit + Llama: 将对话 AI 带入你的…](https://www.kdnuggets.com/2023/08/langchain-streamlit-llama-bringing-conversational-ai-local-machine.html)

+   [Llama, Llama, Llama: 使用你的内容进行本地 RAG 的 3 个简单步骤](https://www.kdnuggets.com/3-simple-steps-to-local-rag-with-your-content)

+   [ChatGPT CLI: 将你的命令行界面转变为 ChatGPT](https://www.kdnuggets.com/2023/07/chatgpt-cli-transform-commandline-interface-chatgpt.html)
