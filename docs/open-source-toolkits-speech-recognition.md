# 语音识别的开源工具包

> 原文：[https://www.kdnuggets.com/2017/03/open-source-toolkits-speech-recognition.html](https://www.kdnuggets.com/2017/03/open-source-toolkits-speech-recognition.html)

**作者：Cindi Thompson，[硅谷数据科学](https://www.svds.com/)。**

作为SVDS [深度学习](http://svds.com/getting-started-deep-learning/) R&D团队的成员，我们对比了[递归神经网络](https://en.wikipedia.org/wiki/Recurrent_neural_network)（RNN）和其他语音识别方法。直到几年前，语音识别的最先进技术是基于[语音学](https://en.wikipedia.org/wiki/Phonetics)的方法，包括发音、[声学](https://en.wikipedia.org/wiki/Acoustic_model)和[语言](https://en.wikipedia.org/wiki/Language_model)模型的独立组件。通常，这包括与[隐马尔可夫模型](https://en.wikipedia.org/wiki/Hidden_Markov_model)（HMM）相结合的n-gram语言模型。我们希望从这个基线模型开始，然后探索将其与较新的方法（如百度的[Deep Speech](https://arxiv.org/abs/1412.5567)）相结合的方式。虽然存在解释这些基线语音学模型的总结，但似乎没有易于消化的博客文章或论文来比较不同免费工具的权衡。

这篇文章回顾了使用传统HMM和n-gram语言模型的主要免费语音识别工具包选项。对于操作、通用和面向客户的语音识别，购买如[Dragon](http://www.nuance.com/dragon/index.htm)或[Cortana](https://www.microsoft.com/en-us/mobile/experiences/cortana/)等产品可能更为合适。但在研发环境中，通常需要更灵活和针对性的解决方案，这就是我们决定开发自己的语音识别管道的原因。下面列出了在免费或开源领域中的主要竞争者，并对它们进行了几个特征的评分。

![对比矩阵](../Images/98e57bf6c7f584277f8b9c2b4f5ec19b.png)

开源和免费语音识别工具包的对比。

本分析基于我们的主观经验以及从仓库和工具包网站上获取的信息。这也不是一个详尽的语音识别软件列表，其中大多数软件列在[这里](https://en.wikipedia.org/wiki/List_of_speech_recognition_software)（包括非开源）。2014年，Gaida等人发表了一篇论文，评估了[CMU Sphinx](http://cmusphinx.sourceforge.net/)、[Kaldi](http://kaldi-asr.org/)和[HTK](http://htk.eng.cam.ac.uk/#)的性能。注意，HTK在其通常的解释中并不完全是开源的，因为代码不能被重新分发或用于商业用途。

**编程语言：** 根据你对不同语言的熟悉程度，你可能自然会偏好某个工具包。除 [ISIP](https://www.isip.piconepress.com/projects/speech/) 外，所有列出的选项都有 Python 包装器，通常可以在主站点找到或通过网络搜索快速找到。当然，Python 包装器可能无法暴露工具包核心代码的全部功能。CMU Sphinx 也有多个其他编程语言的包装器。

**开发活动：** 所列项目均源于学术研究。CMU Sphinx，如其名称所示，是卡内基梅隆大学的产物。它以某种形式存在了大约 20 年，现在可在 GitHub（包括[C](https://github.com/cmusphinx/pocketsphinx) 和 [Java](https://github.com/cmusphinx/sphinx4) 版本）和 [SourceForge](https://sourceforge.net/projects/cmusphinx/) 上找到，并且在两个平台上都有近期活动。Java 和 C 版本在 GitHub 上似乎只有一个贡献者，但这并不能反映项目的历史现实（在 SourceForge 代码库上有 9 位管理员和十多位开发者）。Kaldi 的学术根源来自 2009 年的一个研讨会，其代码现在托管在 [GitHub](https://github.com/kaldi-asr/kaldi) 上，拥有 121 位贡献者。HTK 于 1989 年在剑桥大学开始其生命历程，曾商业化一段时间，但现在许可回剑桥，并且不再作为开源软件提供。尽管其最新版本在 2015 年 12 月更新，但之前的版本是在 2009 年。[Julius](http://julius.osdn.jp/en_index.php) 自 1997 年起开发，并于 2016 年 9 月发布了最后一个重大版本，拥有一个稍微活跃的 [GitHub](https://github.com/julius-speech/julius) 代码库，其中有三位贡献者，这可能仍未完全反映现实。ISIP 是第一个最先进的开源语音识别系统，源自密西西比州立大学。它主要在 1996 到 1999 年间开发，最后一次发布是在 2011 年，但在 GitHub 出现之前，该项目基本上已停滞不前。^([1](https://www.svds.com/open-source-toolkits-speech-recognition/#fn1))

![GitHub 比较](../Images/a863a2c6bcca436b4d4d6acc0f627238.png)

**社区：** 在这里，我们考察了邮件列表和讨论列表以及相关的开发者社区。CMU Sphinx 有在线讨论论坛，并且在其代码库中有活跃的关注。然而，我们怀疑 SourceForge 和 GitHub 上的代码库重复是否阻碍了更广泛的贡献。相比之下，Kaldi 拥有论坛和邮件列表以及一个活跃的 GitHub 代码库。HTK 有邮件列表但没有开放的代码库。Julius 网站上的用户论坛链接已损坏，但在日本网站上可能会有更多信息。ISIP 主要面向教育用途，其邮件列表档案已不再有效。

**教程和示例：** CMU Sphinx 拥有非常易读、详尽且易于跟随的文档；Kaldi 的文档也很全面，但在我看来稍显难以跟随。然而，Kaldi 确实涵盖了语音识别的音素和深度学习方法。如果你对语音识别不熟悉，HTK 的教程文档（供注册用户使用）提供了该领域的良好概述，并包括系统的实际设计和使用文档。Julius 项目专注于日语，最新的文档是日语的^([2](https://www.svds.com/open-source-toolkits-speech-recognition/#fn2))，但他们也在积极翻译成英语，并提供这些文档；这里有一些运行语音识别的示例[这里](https://github.com/julius-speech/dictation-kit)。最后，ISIP 项目有一些文档，但导航起来有些困难。

**训练模型：** 尽管使用这些开源或免费工具的主要原因是因为你想训练专门的识别模型，但能够开箱即用地与系统对话是一个很大的优势。CMU Sphinx 包含英语和许多[其他模型](https://sourceforge.net/projects/cmusphinx/files/Acoustic%20and%20Language%20Models/)可供使用，连接到这些模型的 Python 文档直接包含在[GitHub readme](https://github.com/cmusphinx/pocketsphinx-python/blob/master/readme.md)中。Kaldi 对现有模型解码的说明深藏在文档中，但我们最终发现了一个在 `egs/voxforge` 子目录中训练的英文 VoxForge 数据集的模型，并且可以通过运行 `online-data` 子目录中的脚本进行识别。我们没有深入挖掘其他三个包，但它们都至少附带了简单的模型或似乎与[VoxForge](http://www.voxforge.org/home)网站提供的格式兼容，该网站是一个相当活跃的众包语音识别数据和训练模型的库。

未来，我们将讨论如何开始使用 CMU Sphinx。我们还计划在之前的深度学习帖子基础上，进行一个将神经网络应用于语音的帖子，并将神经网络的识别性能与 CMU Sphinx 进行比较。与此同时，我们始终欢迎对我们博客帖子的反馈和问题，因此如果你对这些工具包或其他工具包有更多看法，请告知我们。

**参考文献**

+   [blog.neospeech.com/2016/07/08/top-5-open-source-speech-recognition-toolkits](http://blog.neospeech.com/2016/07/08/top-5-open-source-speech-recognition-toolkits/)

+   Gaida, Christian, 等。“比较开源语音识别工具包。”技术报告，DHBW 斯图加特（2014）。

**个人简介：[辛迪·汤普森](https://www.linkedin.com/in/cindithompson/)** 是一位具有自然合作精神的问题解决者，能够利用强大的沟通和协调技巧弥合技术和业务方面的关切。拥有人工智能博士学位，她带来了机器学习、自然语言理解和研发领域的独特学术与行业经验。

[原文](https://www.svds.com/open-source-toolkits-speech-recognition/?utm_campaign=KDNuggets%20Blog&utm_source=KDNuggets). 经许可转载。

**相关：**

+   [人工智能与聊天机器人语音识别入门](/2017/01/artificial-intelligence-speech-recognition-chatbots-primer.html)

+   [在对话语音识别中实现人类平衡](/2016/12/achieving-human-parity-conversational-speech-recognition.html)

+   [前 20 个 Python 机器学习开源项目，已更新](/2016/11/top-20-python-machine-learning-open-source-updated.html)

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 加速你的网络安全职业发展

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 更多相关内容

+   [语音识别指标的发展](https://www.kdnuggets.com/2022/10/evolution-speech-recognition-metrics.html)

+   [闭源与开源图像注释](https://www.kdnuggets.com/closed-source-vs-open-source-image-annotation)

+   [在 5 分钟内用 Python 构建文本转语音转换器](https://www.kdnuggets.com/2022/09/build-texttospeech-converter-python-5-minutes.html)

+   [图像识别与自然语言处理的迁移学习](https://www.kdnuggets.com/2022/01/transfer-learning-image-recognition-natural-language-processing.html)

+   [5 个需求高但不被重视的 IT 职业](https://www.kdnuggets.com/5-it-jobs-that-are-high-in-demand-but-dont-get-enough-recognition)

+   [介绍 Objectiv：开源产品分析基础设施](https://www.kdnuggets.com/2022/06/objectiv-introducing-objectiv-opensource-product-analytics-infrastructure.html)
