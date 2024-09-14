# 使用 Hugging Face 理解 BERT

> 原文：[https://www.kdnuggets.com/2021/07/understanding-bert-hugging-face.html](https://www.kdnuggets.com/2021/07/understanding-bert-hugging-face.html)

[评论](#comments)

![bert-lg-hugging-face.jpg](../Images/095380cfb6379c817ecc69910337478f.png)

### **使用 BERT 和 Hugging Face 创建问答模型**

在最近的一篇关于[BERT](https://www.exxactcorp.com/blog/Deep-Learning/how-do-bert-transformers-work)的文章中，我们讨论了 BERT 转换器及其基本工作原理。文章涵盖了 BERT 的架构、训练数据和训练任务。

然而，我们在真正实现之前并不了解某些东西。所以在这篇文章中，我们将使用 BERT 和 Hugging Face 库实现一个问答神经网络。

### **什么是问答任务？**

在这个任务中，我们给定一个问题和一个段落，其中包含答案的段落是我们的[BERT架构](https://www.exxactcorp.com/blog/Deep-Learning/a-deep-dive-into-the-transformer-architecture-the-development-of-transformer-models)，目标是确定答案在段落中的起始和结束跨度。

![Figure](../Images/042fb23b542c92d878f7fb880dd4e8ff.png)

*BERT微调问答任务示意图*

正如上一篇文章中所解释的，在上述示例中，我们向 BERT 架构提供两个输入。段落和问题通过<SEP>标记分隔。紫色层是 BERT 编码器的输出。

我们现在定义两个向量 S 和 E（在微调过程中将被学习），它们的形状为 (1x768)。然后我们将这些向量与 BERT 的第二句输出向量进行点积，得到一些分数。我们随后对这些分数应用 Softmax 以获得概率。训练目标是正确起始和结束位置的对数似然的总和。在数学上，对于起始位置的概率向量：

![equation-700.jpg](../Images/2457015fb5dfbd7b790e050ef2eb1594.png)

其中 T_i 是我们关注的词。结束位置的公式是类似的。

为了预测一个跨度，我们获取所有的分数 — S.T 和 E.T，并选择得分最高的跨度，即在所有 j≥i 中最大化(S.T_i + E.T_j)的跨度。

### **我们如何使用 Hugging Face 实现这一点？**

[Hugging Face](https://huggingface.co/) 提供了一种相当直接的方法来实现这一点。

输出是：

```py
Question: How many pretrained models are available in Transformers?

Answer: over 32 +

Question: What do Transformers provide?

Answer: general purpose architectures

Question: Transformers provide interoperability between which frameworks?

Answer: TensorFlow 2\. 0 and PyTorch
```

因此，我们仅使用了 Hugging Face 提供的[SQuAD 数据集](https://huggingface.co/datasets/squad)上的预训练分词器和模型来完成这个任务。

```py
tokenizer = AutoTokenizer.from_pretrained(“bert-large-uncased-whole-word-masking-finetuned-squad”)

model = AutoModelForQuestionAnswering.from_pretrained(“bert-large-uncased-whole-word-masking-finetuned-squad”)
```

一旦我们有了模型，我们只需获取起始和结束概率分数，并预测跨度为起始分数最高的 token 和结束分数最高的 token 之间的那个跨度。

例如，如果段落的起始分数是：

```py
...
Transformers - 0.1
are - 0.2
general - 0.5
purpose - 0.1
architectures -0.01
(BERT - 0.001
...
```

而结束分数是：

```py
...
Transformers - 0.01
are - 0.02
general - 0.05
purpose - 0.01
architectures -0.01
(BERT - 0.8
...
```

我们将得到输出为input_ids[answer_start:answer_end]，其中answer_start是词“general”（最大开始分数的词）的索引，answer_end是BERT（最大结束分数的词）的索引。答案将是“general purpose architectures”。

### **使用问答数据集微调我们自己的模型**

在大多数情况下，我们将希望在自己的数据集上训练自己的QA模型。在这种情况下，我们将从SQuAD数据集和Hugging Face库中的基础BERT模型开始进行微调。

让我们看看在开始微调之前，SQuAD数据集的样子：

```py
Context:Within computer systems, two of many security models capable of enforcing privilege separation are access control lists (ACLs) and capability-based security. Using ACLs to confine programs has been proven to be insecure in many situations, such as if the host computer can be tricked into indirectly allowing restricted file access, an issue known as the confused deputy problem. It has also been shown that the promise of ACLs of giving access to an object to only one person can never be guaranteed in practice. Both of these problems are resolved by capabilities. This does not mean practical flaws exist in all ACL-based systems, but only that the designers of certain utilities must take responsibility to ensure that they do not introduce flaws. [citation needed]

Question:The confused deputy problem and the problem of not guaranteeing only one person has access are resolved by what?

Answer:['capabilities']

Answer Start in Text:[553]

--------------------------------------------------------------------

Context:In recent years, the nightclubs on West 27th Street have succumbed to stiff competition from Manhattan's Meatpacking District about fifteen blocks south, and other venues in downtown Manhattan.

Question:How many blocks south of 27th Street is Manhattan's Meatpacking District?

Answer:['fifteen blocks']

Answer Start in Text:[132]

--------------------------------------------------------------------
```

我们可以看到每个示例包含上下文、答案和答案的开始标记。我们可以使用下面的脚本将数据预处理为所需的格式。一旦我们拥有上述格式的数据，脚本将处理很多事情，其中最重要的是答案在max_length附近的情况以及使用答案和开始标记索引计算跨度。

一旦我们获得了所需格式的数据，我们就可以从那里对我们的BERT基础模型进行微调。

```py
model_checkpoint = "bert-base-uncased"
model = AutoModelForQuestionAnswering.from_pretrained(model_checkpoint)
tokenizer = AutoTokenizer.from_pretrained(model_checkpoint)tokenized_datasets = datasets.map(prepare_train_features, batched=True, remove_columns=datasets["train"].column_names)args = TrainingArguments(
     f"test-squad",
     evaluation_strategy = "epoch",
     learning_rate=2e-5,
     per_device_train_batch_size=16,
     per_device_eval_batch_size=16,
     num_train_epochs=3,
     weight_decay=0.01,
)data_collator = default_data_collator
trainer = Trainer(
     model,
     args,
     train_dataset=tokenized_datasets["train"],
     eval_dataset=tokenized_datasets["validation"],
     data_collator=data_collator,
     tokenizer=tokenizer,
)trainer.train()
trainer.save_model(trainer.save_model("test-squad-trained"))
```

![图示](../Images/f592cf5eaa766177b529c57fd508cca9.png)

*微调BERT基础模型的输出*

一旦我们训练了模型，我们可以这样使用它：

在这种情况下，我们也取最大开始分数和最大结束分数的索引，并预测答案为它们之间的内容。如果我们想获得BERT论文中提供的精确实现，我们可以稍微调整上述代码，找出最大化（start_score + end_score）的索引。

![图示](../Images/6827e92363f87bb01fd57443ee9d9ccf.png)

*模型训练中的代码输出*

### **参考文献**

+   [Attention Is All You Need](https://arxiv.org/abs/1706.03762)：介绍Transformer的论文。

+   [BERT论文](https://arxiv.org/abs/1810.04805)：请阅读这篇论文。

+   [Hugging Face](https://huggingface.co/transformers/usage.html)

在这篇文章中，我介绍了如何从零开始使用BERT创建一个问答模型。我希望这对理解BERT以及Hugging Face库都很有用。

如果你想查看本系列中的其他文章，可以查看这些：

+   [从数据科学的角度理解Transformers](https://mlwhiz.com/blog/2020/09/20/transformers/)

+   [从编程的角度理解Transformers](https://mlwhiz.com/blog/2020/10/10/create-transformer-from-scratch/)

+   [用草图简单解释BERT](https://mlwhiz.medium.com/explaining-bert-simply-using-sketches-ba30f6f0c8cb)

我们将来会写更多类似的主题，所以请告诉我们你的想法。我们应该写一些技术性很强的主题，还是更偏向于初学者水平？评论区是你的朋友，使用它吧。

[原文](https://www.exxactcorp.com/blog/Deep-Learning/understanding-bert-with-hugging-face)。经允许转载。

**相关：**

+   [学习实用NLP的最佳方法？](/2021/06/best-way-learn-practical-nlp.html)

+   [如何使用 T5 Transformer 生成有意义的句子](/2021/05/generate-meaningful-sentences-t5-transformer.html)

+   [如何通过 API 创建和部署简单的情感分析应用](/2021/06/create-deploy-sentiment-analysis-app-api.html)

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT 工作

* * *

### 相关主题

+   [如何使用 Hugging Face Transformers 微调 BERT 进行情感分析](https://www.kdnuggets.com/how-to-fine-tune-bert-sentiment-analysis-hugging-face-transformers)

+   [前10名机器学习演示：Hugging Face Spaces 版](https://www.kdnuggets.com/2022/05/top-10-machine-learning-demos-hugging-face-spaces-edition.html)

+   [一个开发 Hugging Face 客户数据建模的社区](https://www.kdnuggets.com/2022/08/objectiv-community-developing-hugging-face-customer-data-modeling.html)

+   [用 Hugging Face 和 Gradio 在 5 分钟内构建 AI 聊天机器人](https://www.kdnuggets.com/2023/06/build-ai-chatbot-5-minutes-hugging-face-gradio.html)

+   [如何使用 Hugging Face AutoTrain 微调 LLMs](https://www.kdnuggets.com/how-to-use-hugging-face-autotrain-to-finetune-llms)

+   [如何使用 Hugging Face AutoTrain 微调 Mistral AI 7B LLM](https://www.kdnuggets.com/how-to-finetune-mistral-ai-7b-llm-with-hugging-face-autotrain)
