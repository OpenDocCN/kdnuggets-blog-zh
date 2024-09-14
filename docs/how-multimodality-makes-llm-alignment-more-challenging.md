# 多模态如何使 LLM 对齐变得更具挑战性

> 原文：[`www.kdnuggets.com/how-multimodality-makes-llm-alignment-more-challenging`](https://www.kdnuggets.com/how-multimodality-makes-llm-alignment-more-challenging)

![多模态如何使 LLM 对齐变得更具挑战性](img/e6cbe8ab3a64b72a64c59cf090a65e96.png)

图片来源：[Gerd Altmann](https://pixabay.com/users/geralt-9301/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2814937) 来自 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2814937)

大约一个月前，OpenAI 宣布 ChatGPT 现在可以看、听和说。这意味着模型可以帮助你处理更多的日常任务。例如，你可以上传冰箱里食材的照片，并请求根据你拥有的材料提出用餐建议。或者你可以拍摄客厅的照片，向 ChatGPT 请求艺术和装饰建议。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT

* * *

这是可能的，因为 ChatGPT 使用了多模态的 GPT-4 作为底层模型，它可以接受图像和文本输入。然而，这些新功能为模型对齐团队带来了新的挑战，我们将在本文中讨论。

# LLM 的对齐

“*对齐大型语言模型*”一词指的是训练模型按照人类期望的行为。这通常意味着理解人类指令，并生成有用、准确、安全且无偏见的回答。为了教会模型正确的行为，我们通过两个步骤提供示例：监督微调（SFT）和带有人类反馈的强化学习（RLHF）。

监督微调（SFT）教会模型遵循特定的指令。在 ChatGPT 的情况下，这意味着提供对话示例。基础模型 GPT-4 目前还不能做到这一点，因为它被训练来预测序列中的下一个词，而不是回答类似聊天机器人的问题。

尽管 SFT 赋予 ChatGPT “聊天机器人”的特性，但其回答仍远非完美。因此，应用了带有人类反馈的强化学习（RLHF）来提高回答的真实性、无害性和有用性。基本上，指令调整算法被要求生成多个回答，然后由人类根据上述标准进行排名。这使得奖励算法能够学习人类的偏好，并用于重新训练 SFT 模型。

在这一步之后，一个模型被对齐到人类价值观，或者至少我们希望如此。但为什么多模态使得这个过程变得更复杂呢？

# 多模态数据和新挑战

当我们讨论多模态 LLM 的对齐时，我们应关注图像和文本。它并不涵盖所有 ChatGPT 最新的“*观看、听见和说话*”功能，因为后两者使用的是语音转文本和文本转语音模型，并未直接与 LLM 模型连接。

事情变得有点复杂。相比于仅有文本输入，图像和文本的结合更难以解读。因此，ChatGPT-4 在图像中看得见或看不见的物体和人经常产生幻觉。

Gary Marcus 写了一篇关于多模态幻觉的优秀[文章](https://garymarcus.substack.com/p/hello-multimodal-hallucinations)，揭示了不同的案例。一个例子展示了 ChatGPT 如何从图像中错误地读取时间。它在数数厨房照片中的椅子时也遇到了困难，并且无法识别照片中佩戴手表的人。

![多模态如何使 LLM 对齐更具挑战性](img/d7bc069bbd2f12ae82a482f14874603e.png)

图片来源：[`twitter.com/anh_ng8`](https://twitter.com/anh_ng8)

图像作为输入也为对抗性攻击打开了一扇窗。它们可以成为提示注入攻击的一部分，或者用于传递指令以使模型生成有害内容。

Simon Willison 在这篇[文章](https://simonwillison.net/2023/Oct/14/multi-modal-prompt-injection/)中记录了几个图像注入攻击的例子。一个基本的例子涉及上传一张包含你希望 ChatGPT 遵循的新指令的图像。见下例：

![多模态如何使 LLM 对齐更具挑战性](img/732ae41196025f9798323744196d576d.png)

图片来源：[`twitter.com/mn_google/status/1709639072858436064`](https://twitter.com/mn_google/status/1709639072858436064)

同样，照片中的文本也可以被替换为模型生成仇恨言论或有害内容的指令。

# 改善多模态数据的对齐

那么为什么多模态数据更难对齐？与单模态语言模型相比，多模态模型仍处于开发的早期阶段。OpenAI 并未透露 GPT-4 如何实现多模态，但显然他们为其提供了大量带有文本注释的图像。

文本-图像对比纯文本数据更难获取，这类数据集较少，且自然示例在互联网上比简单文本更难找到。

图像-文本对的质量带来了额外的挑战。一张带有一句话文本标签的图像远不如带有详细描述的图像有价值。为了获得后者，我们通常需要[人工注释者](https://toloka.ai/global-crowd/)，他们按照精心设计的指令集提供文本注释。

此外，训练模型遵循指令需要足够数量的实际用户提示，包括图像和文本。由于这种方法的新颖性，有机示例很难获得，训练示例通常需要由人工按需创建。

对齐多模态模型引发了之前甚至无需考虑的伦理问题。模型是否应该能够评论人们的外貌、性别和种族，或者识别他们是谁？它是否应该尝试猜测照片的位置？与仅有文本数据相比，需要对齐的方面更多。

# 总结

多模态性为模型的使用带来了新的可能性，但也给模型开发人员带来了新的挑战，他们需要确保答案的无害性、真实性和有用性。由于多模态性，需要对更多方面进行对齐，为 SFT 和 RLHF 提供优质的训练数据变得更加困难。希望构建或微调多模态模型的人需要为这些新的挑战做好准备，采用包含高质量人类反馈的开发流程。

**[](https://www.aboutdatablog.com/about)**[Magdalena Konkiewicz](https://www.aboutdatablog.com/about) 是 Toloka 的数据推广专家，这是一家支持快速且可扩展的 AI 开发的全球公司。她拥有爱丁堡大学的人工智能硕士学位，曾在欧洲和美国的企业担任 NLP 工程师、开发人员和数据科学家。她还参与了数据科学家的教学和指导，并定期向数据科学和机器学习出版物做出贡献。

### 更多相关话题

+   [12 个最具挑战的数据科学面试问题](https://www.kdnuggets.com/2022/07/12-challenging-data-science-interview-questions.html)

+   [Web LLM：将 LLM 聊天机器人带到浏览器](https://www.kdnuggets.com/2023/05/webllm-bring-llm-chatbots-browser.html)

+   [是什么让 Python 成为初创企业的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [什么使可视化效果良好？](https://www.kdnuggets.com/2022/10/sphere-makes-visualization-good.html)

+   [Interview Kickstart 数据科学面试课程——它的独特之处](https://www.kdnuggets.com/2022/10/interview-kickstart-data-science-interview-course-makes-different.html)

+   [7 种 ChatGPT 让你编程更高效的方式](https://www.kdnuggets.com/2023/06/7-ways-chatgpt-makes-code-better-faster.html)
