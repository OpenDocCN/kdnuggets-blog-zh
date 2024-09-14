# 你应该选择哪种BERT风格用于你的QA任务？

> 原文：[https://www.kdnuggets.com/2020/10/flavor-bert-use-qa-task.html](https://www.kdnuggets.com/2020/10/flavor-bert-use-qa-task.html)

[评论](#comments)

**作者 [Olesya Bondarenko](https://www.linkedin.com/in/ovbondarenko/)，[Tangible AI](https://tangibleai.com/)**

![图像](../Images/efabd8cadd6a17ae6382400d8e2b50db.png)

图片由 [Evan Dennis](https://unsplash.com/@evan__bray?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

得益于开源自然语言处理库的丰富、策划的数据集和迁移学习的强大功能，制作一个智能聊天机器人从未如此简单。使用Transformers库构建一个基本的问答功能可以像这样简单：

```py
from transformers import pipeline# Context: a snippet from a Wikipedia article about Stan Lee
context = """
    Stan Lee[1] (born Stanley Martin Lieber /ˈliːbər/; December 28, 1922 - November 12, 2018) was an American comic book 
    writer, editor, publisher, and producer. He rose through the ranks of a family-run business to become Marvel Comics' 
    primary creative leader for two decades, leading its expansion from a small division of a publishing house to
    multimedia corporation that dominated the comics industry.
    """nlp = pipeline('question-answering')
result = nlp(context=context, question="Who is Stan Lee?")
```

下面是输出结果：

```py
{'score': 0.2854291316652837,
 'start': 95,
 'end': 159,
 'answer': 'an American comic book writer, editor, publisher, and producer.'}
```

哇！它工作了！

那个低信心评分有点令人担忧。稍后我们讨论BERT检测不可能问题和无关背景的能力时，你会看到它的影响。

然而，花时间选择适合你任务的正确模型将确保你能从你的对话代理中获得最佳的开箱即用性能。你对语言模型和基准数据集的选择将决定你的聊天机器人的表现。

BERT（双向编码表示转换器）模型在复杂的信息提取任务中表现非常出色。它们不仅能够捕捉单词的含义，还能捕捉上下文。在选择模型（或采用默认选项）之前，你可能需要评估候选模型的准确性和资源（RAM和CPU周期），以确保它确实符合你的期望。在这篇文章中，你将看到我们如何使用[斯坦福问答数据集（SQuAD）](https://rajpurkar.github.io/SQuAD-explorer/)对我们的QA模型进行基准测试。还有许多其他优秀的问答数据集，你可能也想使用，包括微软的[NewsQA](https://www.microsoft.com/en-us/research/project/newsqa-dataset/)、[CommonsenseQA](https://www.tau-nlp.org/commonsenseqa)、[ComplexWebQA](https://www.tau-nlp.org/compwebq)等。为了最大化应用的准确性，你需要选择一个代表你期望的问题、答案和上下文的基准数据集。

[Huggingface Transformers库](https://github.com/huggingface/transformers)拥有大量针对各种任务的预训练模型：情感分析、文本总结、同义改写，当然还有问答。我们从可用的[模型](https://huggingface.co/models?filter=question-answering)库中选择了一些候选问答模型。果然，许多模型已经在SQuAD数据集上进行了微调。太棒了！这里是我们将要评估的几个SQuAD微调模型：

+   distilbert-base-cased-distilled-squad

+   bert-large-uncased-whole-word-masking-finetuned-squad

+   ktrapeznikov/albert-xlarge-v2-squad-v2

+   mrm8488/bert-tiny-5-finetuned-squadv2

+   twmkn9/albert-base-v2-squad2

我们在SQuAD的两个版本（版本1和版本2）上使用我们选择的模型进行了预测。它们之间的区别在于，SQuAD-v1只包含可回答的问题，而SQuAD-v2还包含不可回答的问题。为了说明这一点，让我们来看下面来自SQuAD-v2数据集的例子。问题2的答案无法从维基百科提供的上下文中推导出来：

> ***问题1：****“诺曼底位于哪个国家？”*
> 
> ***问题2：****“谁在1000年代和1100年代将他们的名字给予了诺曼底？”*
> 
> ***上下文：****“诺曼人（Norman: Nourmands; French: Normands; Latin: Normanni）是指在10世纪和11世纪将他们的名字给予法国诺曼底地区的人。他们的祖先是来自丹麦、冰岛和挪威的诺斯（‘诺曼’源自‘诺斯人’）劫掠者和海盗，他们在他们的领袖罗洛的带领下，同意对西法兰克国王查理三世宣誓效忠。通过几代人与当地的法兰克人和罗马高卢人的融合，他们的后代逐渐与西法兰克的加洛林文化融合。诺曼人的独特文化和民族身份最初在10世纪上半叶出现，并在随后的几个世纪中不断发展。”*

我们理想的模型应该能够充分理解上下文以生成答案。

让我们开始吧！

要在Transformers中定义模型和分词器，我们可以使用AutoClasses。在大多数情况下，AutoModels可以自动从模型名称中推导出设置。我们只需要几行代码来完成设置：

```py
from tqdm import tqdm
from transformers import AutoTokenizer, AutoModelForQuestionAnsweringmodelname = 'bert-large-uncased-whole-word-masking-finetuned-squad'tokenizer = AutoTokenizer.from_pretrained(modelname)
model = AutoModelForQuestionAnswering.from_pretrained(modelname)
```

我们将以人类水平的表现作为我们的准确性目标。SQuAD排行榜提供了这个任务的人类水平表现，即87%的准确率和89%的F1得分。

你可能会问，“他们怎么知道人类表现是什么？”以及“他们说的是哪些人？”那些斯坦福研究人员很聪明。他们只是使用了同一组标记SQuAD数据集的众包人群。对于测试集中的每个问题，他们让多个人工提供不同的答案。为了获得人类得分，他们只是将其中一个答案留出来，并检查它是否与其他答案匹配，使用了与评估机器模型相同的文本比较算法。这种“留一个人类答案”的数据集的平均准确性就是机器所要追求的人类水平得分。

为了在我们的数据集上运行预测，首先我们需要将下载的SQuAD文件转换为计算机可解释的特征。幸运的是，Transformers库已经提供了一套方便的函数来完成这一任务：

```py
from transformers import squad_convert_examples_to_features
from transformers.data.processors.squad import SquadV2Processorprocessor = SquadV2Processor()
examples = processor.get_dev_examples(path)
features, dataset = squad_convert_examples_to_features(
    examples=examples,
    tokenizer=tokenizer,
    max_seq_length=512,
    doc_stride = 128,
    max_query_length=256,
    is_training=False,
    return_dataset='pt',
    threads=4, # number of CPU cores to use
)
```

我们将使用PyTorch及其GPU功能（可选）来进行预测：

```py
import torch
from torch.utils.data import DataLoader, SequentialSamplereval_sampler = SequentialSampler(dataset)
eval_dataloader = DataLoader(dataset, sampler=eval_sampler, batch_size=10)all_results = []
def to_list(tensor):
    return tensor.detach().cpu().tolist()for batch in tqdm(eval_dataloader):
    model.eval()
    batch = tuple(t.to(device) for t in batch)    with torch.no_grad():
        inputs = {
            "input_ids": batch[0],
            "attention_mask": batch[1],
            "token_type_ids": batch[2]
        }        example_indices = batch[3]        outputs = model(**inputs)  # this is where the magic happens        for i, example_index in enumerate(example_indices):
            eval_feature = features[example_index.item()]
            unique_id = int(eval_feature.unique_id)
```

重要的是，模型输入应该针对 DistilBERT 模型（例如 `distilbert-base-cased-distilled-squad`）进行调整。由于 DistilBERT 与 BERT 或 ALBERT 的实现差异，我们应该排除“token_type_ids”字段，以避免脚本出错。其他所有内容将保持完全相同。

最后，为了评估结果，我们可以应用来自 Transformers 库的 `squad_evaluate()` 函数：

```py
from transformers.data.metrics.squad_metrics import squad_evaluateresults = squad_evaluate(examples, 
                         predictions,
                         no_answer_probs=null_odds)
```

这是由 squad_evaluate 生成的示例报告：

```py
OrderedDict([('exact', 65.69527499368314),
             ('f1', 67.12954950681876),
             ('total', 11873),
             ('HasAns_exact', 62.48313090418353),
             ('HasAns_f1', 65.35579306586668),
             ('HasAns_total', 5928),
             ('NoAns_exact', 68.8982338099243),
             ('NoAns_f1', 68.8982338099243),
             ('NoAns_total', 5945),
             ('best_exact', 65.83003453213173),
             ('best_exact_thresh', -21.529870867729187),
             ('best_f1', 67.12954950681889),
             ('best_f1_thresh', -21.030719757080078)])
```

现在让我们比较对我们两个基准数据集 SQuAD-v1 和 SQuAD-v2 生成的精确答案准确率（“exact”）和 f1 分数。所有模型在没有负面样本的数据集（SQuAD-v1）上的表现明显更好，但我们有一个明确的赢家（`ktrapeznikov/albert-xlarge-v2-squad-v2`）。总体而言，它在两个数据集上的表现都更好。另一个好消息是我们为这个模型生成的报告与作者发布的 [报告](https://huggingface.co/ktrapeznikov/albert-xlarge-v2-squad-v2) 完全一致。准确率和 f1 分数稍微逊色于人类水平，但对于像 SQuAD 这样具有挑战性的数据集来说仍然是一个很好的结果。

![图像](../Images/85a80a058a58873103cf1a94c571a9d7.png)

表 1：5 个模型在 SQuAD v1 和 v2 上的准确率得分

我们将在下一个表格中比较 SQuAD-v2 预测的完整报告。看起来 `ktrapeznikov/albert-xlarge-v2-squad-v2` 在两个任务上表现几乎一样好：（1）识别可回答问题的正确答案，和（2）筛选出可回答的问题。有趣的是，`bert-large-uncased-whole-word-masking-finetuned-squad` 在第一个任务（可回答的问题）上提供了大约 5% 的显著提升，但在第二个任务上完全失败。

![图像](../Images/ad6851a7b682fa593807d34d0e4eb284.png)

表 2：不可回答问题的单独准确率得分

我们可以通过调整 null 阈值来优化模型在识别不可回答问题方面的表现，以获得最佳的 f1 分数。请记住，最佳的 f1 阈值是由 squad_evaluate 函数计算的输出之一（`best_f1_thresh`）。以下是当我们应用来自 SQuAD-v2 报告的 `best_f1_thresh` 时 SQuAD-v2 预测指标的变化情况：

![图像](../Images/2d82c766c61c3f4c2ba9836d38e89037.png)

表 3：调整后的准确率得分

尽管这种调整有助于模型更准确地识别不可回答的问题，但它以回答问题的准确性为代价。这个权衡应该在你的应用背景下仔细考虑。

让我们使用 Transformers QA 流水线测试三种最佳模型，并提出一些我们自己的问题。我们从一篇关于计算语言学的维基百科文章中挑选了以下未见示例：

```py
context = '''
Computational linguistics is often grouped within the field of artificial intelligence 
but was present before the development of artificial intelligence.
Computational linguistics originated with efforts in the United States in the 1950s to use computers to automatically translate texts from foreign languages, particularly Russian scientific journals, into English.[3] Since computers can make arithmetic (systematic) calculations much faster and more accurately than humans, it was thought to be only a short matter of time before they could also begin to process language.[4] Computational and quantitative methods are also used historically in the attempted reconstruction of earlier forms of modern languages and sub-grouping modern languages into language families.
Earlier methods, such as lexicostatistics and glottochronology, have been proven to be premature and inaccurate. 
However, recent interdisciplinary studies that borrow concepts from biological studies, especially gene mapping, have proved to produce more sophisticated analytical tools and more reliable results.[5]
'''
questions=['When was computational linguistics invented?',
          'Which problems computational linguistics is trying to solve?',
          'Which methods existed before the emergence of computational linguistics ?',
          'Who invented computational linguistics?',
          'Who invented gene mapping?']
```

请注意，最后两个问题在给定的上下文中是无法回答的。以下是我们测试的每个模型的结果：

```py
Model: bert-large-uncased-whole-word-masking-finetuned-squad
-----------------
Question: When was computational linguistics invented?
Answer: 1950s (confidence score 0.7105585285134239)

Question: Which problems computational linguistics is trying to solve?
Answer: earlier forms of modern languages and sub-grouping modern languages into language families. (confidence score 0.034796690637104444)

Question: What methods existed before the emergence of computational linguistics?
Answer: lexicostatistics and glottochronology, (confidence score 0.8949566496998465)

Question: Who invented computational linguistics?
Answer: United States (confidence score 0.5333964470000865)

Question: Who invented gene mapping?
Answer: biological studies, (confidence score 0.02638426599066701)

Model: ktrapeznikov/albert-xlarge-v2-squad-v2
-----------------
Question: When was computational linguistics invented?
Answer: 1950s (confidence score 0.6412413898187204)

Question: Which problems computational linguistics is trying to solve?
Answer: translate texts from foreign languages, (confidence score 0.1307672173261354)

Question: What methods existed before the emergence of computational linguistics?
Answer:  (confidence score 0.6308010582306451)

Question: Who invented computational linguistics?
Answer:  (confidence score 0.9748902345310917)

Question: Who invented gene mapping?
Answer:  (confidence score 0.9988990117797236)

Model: mrm8488/bert-tiny-5-finetuned-squadv2
-----------------
Question: When was computational linguistics invented?
Answer: 1950s (confidence score 0.5100432430158293)

Question: Which problems computational linguistics is trying to solve?
Answer: artificial intelligence. (confidence score 0.03275686739784334)

Question: What methods existed before the emergence of computational linguistics?
Answer:  (confidence score 0.06689302592967117)

Question: Who invented computational linguistics?
Answer:  (confidence score 0.05630986208743849)

Question: Who invented gene mapping?
Answer:  (confidence score 0.8440988190788303)

Model: twmkn9/albert-base-v2-squad2
-----------------
Question: When was computational linguistics invented?
Answer: 1950s (confidence score 0.630521506320747)

Question: Which problems computational linguistics is trying to solve?
Answer:  (confidence score 0.5901262729978356)

Question: What methods existed before the emergence of computational linguistics?
Answer:  (confidence score 0.2787252009804586)

Question: Who invented computational linguistics?
Answer:  (confidence score 0.9395531361082305)

Question: Who invented gene mapping?
Answer:  (confidence score 0.9998772777192002)

Model: distilbert-base-cased-distilled-squad
-----------------
Question: When was computational linguistics invented?
Answer: 1950s (confidence score 0.7759537003546768)

Question: Which problems computational linguistics is trying to solve?
Answer: gene mapping, (confidence score 0.4235580072416312)

Question: What methods existed before the emergence of computational linguistics?
Answer: lexicostatistics and glottochronology, (confidence score 0.8573431178602817)

Question: Who invented computational linguistics?
Answer: computers (confidence score 0.7313878935375229)

Question: Who invented gene mapping?
Answer: biological studies, (confidence score 0.4788379586462099)
```

如你所见，仅凭单个数据点很难评估模型，因为结果差异很大。虽然每个模型对第一个问题（“计算语言学是什么时候发明的？”）给出了正确答案，但其他问题则更具挑战性。这意味着即使是我们最好的模型也可能需要在自定义数据集上进行进一步调整，以实现更好的效果。

### 要点：

+   开源预训练（和微调过的！）模型可以为你的自然语言处理项目提供快速启动。

+   在做其他事情之前，尽量复现作者报告的原始结果（如果可用）。

+   对模型进行准确性基准测试。即使是对完全相同的数据集进行微调的模型，也可能表现出很大的差异。

**个人简介：[Olesya Bondarenko](https://www.linkedin.com/in/ovbondarenko/)** 是 Tangible AI 的首席开发者，她负责推动 QAry 变得更智能。QAry 是一个你可以信赖的开源问答系统，适用于最私密的数据和问题。

[原文](https://tangibleai.com/benchmarking-bert-based-transformers-for-question-answering-and-reading-comprehension-tests/)。经允许转载。

**相关：**

+   [BERT、RoBERTa、DistilBERT、XLNet：使用哪一个？](/2019/09/bert-roberta-distilbert-xlnet-one-use.html)

+   [在 Python 中使用文本相似性检测的简单问答系统](/2020/04/simple-question-answering-systems-text-similarity-python.html)

+   [利用 NLP 识别争议](/2020/05/spotting-controversy-nlp.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织中的 IT 工作

* * *

### 更多相关话题

+   [为你的文本分类任务选择最佳架构：基准测试…](https://www.kdnuggets.com/2023/04/best-architecture-text-classification-task-benchmarking-options.html)

+   [我应该使用哪个指标？准确率与 AUC](https://www.kdnuggets.com/2022/10/metric-accuracy-auc.html)

+   [ETL 与 ELT：哪个更适合你的数据管道？](https://www.kdnuggets.com/2023/03/etl-elt-one-right-data-pipeline.html)

+   [RAG 与 Finetuning：哪一个是提升你 LLM 应用的最佳工具？](https://www.kdnuggets.com/rag-vs-finetuning-which-is-the-best-tool-to-boost-your-llm-application)

+   [你应该使用线性回归模型而不是…的 3 个理由](https://www.kdnuggets.com/2021/08/3-reasons-linear-regression-instead-neural-networks.html)

+   [每位数据科学家都应该了解的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
