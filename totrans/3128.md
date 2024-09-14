# 高级API是否让机器学习变得愚蠢？

> 原文：[https://www.kdnuggets.com/2018/04/high-level-apis-dumbing-down-machine-learning.html](https://www.kdnuggets.com/2018/04/high-level-apis-dumbing-down-machine-learning.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

Google的研究科学家David Ha（[@hardmaru](https://twitter.com/hardmaru)），最近（[2018年2月9日](https://twitter.com/hardmaru/status/961841773298335745?ref_src=twsrc%5Etfw&ref_url=https%3A%2F%2Fkdnuggets.com%2F2018%2F04%2Fhigh-level-apis-dumbing-down-machine-learning.html)）发推道：

> 从头实现全连接网络、卷积网络、RNN、反向传播和SGD（使用纯Python、numpy，甚至是JS）并在小数据集上训练这些模型，是学习神经网络如何工作的一个极好的方法。在跳入框架之前，花时间获得宝贵的直觉。[https://t.co/biP02iWsjd](https://t.co/biP02iWsjd)
> 
> — hardmaru (@hardmaru) [2018年2月9日](https://twitter.com/hardmaru/status/961841773298335745?ref_src=twsrc%5Etfw)

这引发了Keras的创造者François Chollet（[@fchollet](https://github.com/fchollet)）的一系列回应推文，综合考虑这些回应，呈现出不同的观点。以下是几条突出的回应：

> 研究生在2000年就知道如何用C语言实现神经网络。那时他们对神经网络的直觉并不强。到2018年，一个高中生玩转神经网络框架，可以在几天内建立更强的神经网络理解——这全靠更好的应用背景。
> 
> — François Chollet (@fchollet) [2018年3月7日](https://twitter.com/fchollet/status/971394492468158465?ref_src=twsrc%5Etfw)
> 
> 下一代在抽象层次上不断进步是一件好事，在我看来，让学生从头开始并不是一种好的学习策略。
> 
> — François Chollet (@fchollet) [2018年3月7日](https://twitter.com/fchollet/status/971417998270513153?ref_src=twsrc%5Etfw)

一些其他受人尊敬的机器学习研究人员和工程师也参与了讨论，包括[Jeremy Howard](https://twitter.com/jeremyphoward)和[Emmanuel Ameisen](https://twitter.com/EmmanuelAmeisen)等（值得一提的是，Ha的原始推文实际上是对[Denny Britz的推文](https://twitter.com/dennybritz/status/961829329985400839)的回应）。双方在这种“争论”的“观点”上都提出了很好的观点。

‏

高级机器学习API——特别是[Keras](https://keras.io/)、[TensorFlow Layers](https://www.tensorflow.org/api_docs/python/tf/layers)、[Scikit Flow](https://terrytangyuan.github.io/2016/03/14/scikit-flow-intro/)、[TensorFlow Estimators](https://www.tensorflow.org/programmers_guide/estimators)等——将抽象提升到一个新的高度。与之相比，Theano和普通的TensorFlow显得低层次得多。我们甚至可以进一步扩大范围，考虑像Scikit-learn这样的通用库；不久前，拥有一个包含各种有用实现的全方位算法工具包，并且提供一致的API，在同一平台下，这几乎是不可想象的，更不用说它便利的抽象层次了。

![抽象者？](../Images/55c34cd12e3b083cf3836c47d56f09a2.png)

使用这些库来实现机器学习模型变得更容易。然而，Scikit-learn的标准fit/predict API或用Keras堆叠序列层的简单性是否将抽象层级提升得过高，以至于新手无法真正理解算法的基础理论？或者这些工具是否让从业者免于编写他们可能（或可能不会）完全理解的功能？

关于技术抽象的讨论同样适用于编程语言、机器学习，甚至更广泛的抽象概念。的确，[汇编语言](https://en.wikipedia.org/wiki/Assembly_language)如今不是编程人员的首选武器，程序员们通常使用更高层次的抽象工具，并且什么构成[高级语言](https://en.wikipedia.org/wiki/High-level_programming_language)的概念随着时间不断演变。

然而，我认为，自己在没有计算机科学基础的情况下随意编写Javascript的情况与具有扎实计算机科学基础的研究生（或者没有正式计算机科学教育但对其基础知识有牢固把握的人）使用相同语言实现他们的想法是不同的。我不认为这是Chollet提出的相同观点（他从未说过没有神经网络理解的人能够有效地编码神经网络），但他几乎肯定是在推动抽象层级的稳步上升作为更有效的学习体验。

虽然“最佳”学习方法，尤其是它们的不同细微差别，永远无法完全普遍适用，并且在一定程度上依赖于学习者，但似乎合理的是，将理论和实践相结合的渐进混合方法，不能被排除在作为学习神经网络及其实现的首选方法之外。同样，细节很重要，但保持理论不干扰实践以及避免理论前置的课程阻碍学习者早期看到（和做）深度学习的动机，似乎是有利的。这种方法似乎类似于 [fast.ai](http://www.fast.ai/) 采取的方法，这也可能确认像 Keras 这样的项目在早期作为尝试的有效性，以及在你建立了一些更强的理解后如何完成任务。

仅仅是一个人的观点……你怎么看？

**相关**：

+   [比较深度学习框架：罗塞塔石碑方法](/2018/03/deep-learning-frameworks.html)

+   [今天我在午休时间用 Keras 构建了一个神经网络](/2017/12/today-built-neural-network-during-lunch-break-keras.html)

+   [谷歌 Tensorflow 目标检测 API 是实现图像识别的最简单方法吗？](/2018/03/google-tensorflow-object-detection-api-the-easiest-way-implement-image-recognition.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

### 更多相关话题

+   [通过集成 Jupyter 和 KNIME 缩短实施时间](https://www.kdnuggets.com/2021/12/cutting-implementation-time-integrating-jupyter-knime.html)

+   [解析 AutoGPT](https://www.kdnuggets.com/2023/05/breaking-autogpt.html)

+   [解析量子计算：对数据科学和 AI 的影响](https://www.kdnuggets.com/breaking-down-quantum-computing-implications-for-data-science-and-ai)

+   [深入解析 DENSE_RANK()：SQL 爱好者的逐步指南](https://www.kdnuggets.com/breaking-down-denserank-a-step-by-step-guide-for-sql-enthusiasts)

+   [OpenAI 新推出的 ChatGPT 和 Whisper API](https://www.kdnuggets.com/2023/03/new-chatgpt-whisper-apis-openai.html)

+   [FastAPI 教程：在几分钟内用 Python 构建 API](https://www.kdnuggets.com/fastapi-tutorial-build-apis-with-python-in-minutes)
