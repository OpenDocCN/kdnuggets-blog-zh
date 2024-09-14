# 让我听听你的声音，我会告诉你你的感受

> 原文：[https://www.kdnuggets.com/2016/05/voice-tone-analysis-emotion-detection.html](https://www.kdnuggets.com/2016/05/voice-tone-analysis-emotion-detection.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者：卡洛斯·阿尔盖塔，灵魂黑客实验室**。

创建情感感知技术在近年来变得非常流行。有一[广泛的公司](http://nordicapis.com/20-emotion-recognition-apis-that-will-leave-you-impressed-and-concerned/)试图从你写的内容、你的语音语调或你面部的表情中检测你的情感。所有这些公司都通过基于云的编程接口（API）在线提供他们的技术。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全认证](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业认证](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业认证](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在IT方面

* * *

![正中下怀](../Images/995b62ccdbc769e89f628329662c8434.png)

作为我的离线情感感知硬件（Project Jammin）的一部分，我已经构建了早期的[面部表情](https://medium.com/@kidargueta/detecting-emotion-in-faces-using-geometric-features-a9a7febe024f#.3jqslnqq9)和[语音内容](https://medium.com/@kidargueta/offline-emotion-specific-speech-to-text-in-low-end-devices-62b9cc195713#.fje08wd1w)识别原型，用于情感检测。在这篇简短的文章中，我描述了缺失的部分——一个语音语调分析器。

为了构建一个语调分析器，需要研究语音波形的特性（声音的二维表示）。波形也被称为声音的时间域表示，因为它们是表示强度随时间变化的表现形式。有关波形的更多详细信息，请参阅这个[有趣的页面](http://clas.mq.edu.au/speech/acoustics/waveforms/speech_waveforms.html)。

![字母波形](../Images/89bcd3e1e9010ef7077ac8eed9062527.png)

*英语语言中四个不同字母的波形。*

使用专门设计用于分析语音的软件，目的是提取波形的某些特征，这些特征可以用作训练机器学习分类器的特征。给定一组带有情感标签的语音录音，我们可以使用提取的特征构建每个录音的向量表示。

从语音中提取情感所使用的特征因研究而异，有时甚至取决于分析的语言。一般来说，许多研究和应用工作使用了音高、梅尔频率倒谱系数（MFCC）和语音的共振峰的组合。

![惊讶波形](../Images/6015d6c6f0967832741d6e6a0e30bae9.png)

*上方为表达惊讶的语音波形。

蓝色下方为音高，黄色为强度，红色为共振峰。*

一旦提取了特征并构建了语音的向量表示，就会训练一个分类器来检测情感。在以前的工作中使用了几种类型的分类器。其中最受欢迎的是支持向量机（SVM）、逻辑回归（Logit）、隐马尔可夫模型（HMM）和神经网络（NN）。

作为一个早期原型，我实现了一个简化版的情感检测分类器。与其检测快乐、悲伤、愤怒等多种情感，我的情感分析器进行二元分类，以检测用户的激发水平。高激发水平与快乐、惊讶和愤怒等情感相关，而低激发水平与悲伤和无聊等情感相关。以下视频展示了我的情感分析器在树莓派上的运行。请欣赏！

**简历：[卡洛斯·阿尔盖塔](http://www.soulhackerslabs.com/)** 是一位居住在台湾的洪都拉斯企业家。他拥有信息系统与应用领域的博士学位，并且是Veryfast Inc. 和 Soul Hackers Labs的联合创始人。

**相关**：

+   [深度学习中的深层感受](/2016/01/deep-feelings-deep-learning.html)

+   [2016年及之后深度学习的预期](/2016/01/deep-learning-2016-beyond.html)

+   [关于情感分析的11件事](/2015/08/11-things-about-sentiment-analysis.html)

### 更多相关内容

+   [在多语言语音技术中克服障碍：前五大挑战及创新解决方案…](https://www.kdnuggets.com/2023/08/overcoming-barriers-multilingual-voice-technology-top-5-challenges-innovative-solutions.html)

+   [适合您的文本分类任务的最佳架构：基准测试…](https://www.kdnuggets.com/2023/04/best-architecture-text-classification-task-benchmarking-options.html)

+   [建立供应链管道所需的6种数据科学技术](https://www.kdnuggets.com/2022/01/6-data-science-technologies-need-build-supply-chain-pipeline.html)

+   [想用您的数据技能解决全球问题？请看这里…](https://www.kdnuggets.com/2022/04/jhu-want-data-skills-solve-global-problems.html)

+   [您下一次面试可能会遇到的24个SQL问题](https://www.kdnuggets.com/2022/06/24-sql-questions-might-see-next-interview.html)

+   [您晋升所需的数据分析师技能](https://www.kdnuggets.com/2022/09/data-analyst-skills-need-next-promotion.html)
