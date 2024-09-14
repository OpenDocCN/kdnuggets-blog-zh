# 5 个免费工具，用于检测 ChatGPT、GPT3 和 GPT2

> 原文：[https://www.kdnuggets.com/2023/02/5-free-tools-detecting-chatgpt-gpt3-gpt2.html](https://www.kdnuggets.com/2023/02/5-free-tools-detecting-chatgpt-gpt3-gpt2.html)

![5 个免费工具，用于检测 ChatGPT、GPT3 和 GPT2](../Images/be03db8b560f9e18801ff60215608ad9.png)

作者提供的图片

在 [ChatGPT](https://openai.com/blog/chatgpt/) 推出后，潘多拉的盒子被打开了。我们现在观察到工作方式的技术转变。人们正在使用 ChatGPT 创建网站、应用程序，甚至撰写小说。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

随着 AI 生成工具的热度和推出，我们看到恶意行为者的增加。如果你关注最新新闻，你一定听说过 ChatGPT 已经通过了沃顿 MBA 考试。ChatGPT 已通过的考试列表从医学到法律学位 - [列表：ChatGPT 迄今已通过的考试](https://www.businessinsider.com/list-here-are-the-exams-chatgpt-has-passed-so-far-2023-1?op=1#wharton-mba-exam-1)。

除了考试，学生还使用它来提交作业，作家提交生成内容，研究人员只需输入提示即可生成高质量论文。

为了应对生成内容的滥用，我向你介绍 5 个免费的 AI 内容检测工具。你可以使用它们来检查内容的有效性并提高你的 SEO 排名。

# 1\. GPT-ZERO

[GPTZero](https://gptzero.me/) 是由普林斯顿大学的高年级生爱德华·田设计的，用于检测 ChatGPT 生成的写作。创造者发现这项技术被学生用来作弊，因此提出了这个保护措施。

![5 个免费工具，用于检测 ChatGPT、GPT3 和 GPT2](../Images/19a2087be477bf95ada77dba79facda3.png)

图片来自 GPTZero

这个基于网络的应用程序相当简单。你可以粘贴文本或加载 pdf、docx 和 txt 文件。只需点击一次“获取结果”按钮，你就能找到摘要、平均困惑度和突发性评分。此外，它还会突出显示可能由 AI 撰写的文本。

![5 个免费工具，用于检测 ChatGPT、GPT3 和 GPT2](../Images/b377a31dc66e9e4f455926c5b1d7ee0c.png)

图片来自 GPTZero

> **注意：** 如果内容完全由 AI 撰写，你将不会看到突出显示的文本。

你还可以试用 [Streamlit 演示](https://etedward-gptzero-main-zqgfwb.streamlit.app/) 应用程序。

# 2\. OpenAI GPT2 输出检测器

[OpenAI GPT2 输出检测器](https://openai-openai-detector.hf.space/) 是由 OpenAI 开发，并托管在 HuggingFace 上供用户自由检查内容有效性。它可以检测由 ChatGPT、GPT3 和 GPT2 生成的文本。该应用程序使用基于 [Transformers](https://github.com/huggingface/transformers/commit/1c542df7e554a2014051dd09becf60f157fed524) 的 [GPT-2 输出检测器](https://github.com/openai/gpt-2-output-dataset/tree/master/detector) 模型。

![5 个免费工具检测 ChatGPT、GPT3 和 GPT2](../Images/524dec9fa4623aade53d61b79fdb2455.png)

来自 OpenAI GPT2 输出检测器的图像

要检查内容有效性，只需复制并粘贴文本，它将自动显示真实/虚假的百分比。

我使用了几天，认为它在检测各种 AI 文本生成方面相当准确。

# 3\. Hello-SimpleAI ChatGPT 检测器

[Hello-SimpleAI ChatGPT 检测器](https://hello-simpleai-chatgpt-detector-ling.hf.space/) 包含三种版本来检测由 ChatGPT 生成的文本。

1.  [QA 版本](https://huggingface.co/spaces/Hello-SimpleAI/chatgpt-detector-qa)：需要同时提供问题和答案，以检查是否由 ChatGPT 生成。在后台，它使用基于 PLM 的分类器。

1.  [单文本版本](https://huggingface.co/spaces/Hello-SimpleAI/chatgpt-detector-single)：需要较长文本来检测由 ChatGPT 生成的文本，使用基于 PLM 的分类器。

1.  [语言学版本](https://huggingface.co/spaces/Hello-SimpleAI/chatgpt-detector-ling)：根据语言特征检测 ChatGPT。

![5 个免费工具检测 ChatGPT、GPT3 和 GPT2](../Images/2fc3a835a2c6959f9b91655cbe42a881.png)

来自 Hello-SimpleAI 的图像

在我看来，最准确的是语言学版本。你需要提供文本并点击预测按钮来查看结果。该应用程序将展示来自两个不同模型 GLTR 和 PPL 的结果。你可以在 [chatgpt-comparison-detection 项目](https://github.com/Hello-SimpleAI/chatgpt-comparison-detection) 上了解更多信息。

# 4\. Contentatscale AI 内容检测器

[Contentatscale AI 内容检测器](https://contentatscale.ai/ai-content-detector/) 是我经常用于质量评估的工具。它快速、简单且准确。AI 内容检测器允许用户通过粘贴文本来获取**人工内容**分数。

![5 个免费工具检测 ChatGPT、GPT3 和 GPT2](../Images/f5ce9640e6a28449ebb23c35f6a0f218.png)

来自 Contentatscale 的图像

> **注意：** 为了获得准确的结果，请尝试粘贴一小段文本（150-200 字）。

# 5\. Writers AI 内容检测器

[Writers AI 内容检测器](https://writer.com/ai-content-detector/) 类似于 Contentatscale。它需要页面的 URL 或文本来计算“人工生成内容”分数。它快速且相当准确。

![5 个免费工具检测 ChatGPT、GPT3 和 GPT2](../Images/7ef19e0b7a4ba48a064406529b42c7a3.png)

来自 Writers AI 内容检测器的图像

> **注意：** 你有 1500 个字符的限制，因此请尽量粘贴小段文本以生成分数。

# 结论

随着 AI 生成工具的兴起，我们也看到了 AI 检测工具的崛起。在目前阶段，它们在检测代码片段和混合文本（人类 + AI）方面不够准确，但未来你会看到许多公司采用检测工具来提高质量和生产力。

在这篇文章中，我们了解了检测 AI 生成文本的 5 个最佳工具。这些工具快速、可靠且简单。你只需粘贴文本即可获得结果。

如果你对学习更多关于 AI、机器学习、MLOps 和数据科学感兴趣，请关注我的社交媒体。谢谢。

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan)) 是一名认证的数据科学专业人士，喜欢构建机器学习模型。目前，他专注于内容创作，并撰写有关机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是利用图神经网络为挣扎于心理疾病的学生开发 AI 产品。

### 更多相关主题

+   [检测 ChatGPT、GPT-4、Bard 和 Claude 的十大工具](https://www.kdnuggets.com/2023/05/top-10-tools-detecting-chatgpt-gpt4-bard-llms.html)

+   [检测数据漂移以确保生产 ML 模型质量，使用 Eurybia](https://www.kdnuggets.com/2022/07/detecting-data-drift-ensuring-production-ml-model-quality-eurybia.html)

+   [视觉 ChatGPT：微软将 ChatGPT 和 VFMs 结合](https://www.kdnuggets.com/2023/03/visual-chatgpt-microsoft-combine-chatgpt-vfms.html)

+   [ChatGPT CLI: 将你的命令行界面转变为 ChatGPT](https://www.kdnuggets.com/2023/07/chatgpt-cli-transform-commandline-interface-chatgpt.html)

+   [5 门免费的 AI 和 ChatGPT 课程，助你从零基础到精通](https://www.kdnuggets.com/5-free-courses-on-ai-and-chatgpt-to-take-you-from-0-100)

+   [GPT4All 是你文档的本地 ChatGPT，而且是免费的！](https://www.kdnuggets.com/2023/06/gpt4all-local-chatgpt-documents-free.html)
