# 当贝叶斯、奥卡姆和香农聚在一起定义机器学习时

> 原文：[https://www.kdnuggets.com/2018/09/when-bayes-ockham-shannon-come-together-define-machine-learning.html](https://www.kdnuggets.com/2018/09/when-bayes-ockham-shannon-come-together-define-machine-learning.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

![图片](../Images/e31348f9367564a9e3a230f5e90e2944.png)

### **介绍**

在所有机器学习的热门词汇中，令人有些惊讶的是，我们很少听到一个短语，它将统计学习、信息理论和自然哲学的一些核心概念融合成一个三字组合。

此外，这不仅仅是一个晦涩而迂腐的短语，仅适用于机器学习（ML）博士和理论家。它对任何有兴趣探索的人都有明确且易于理解的意义，并且对机器学习和数据科学的从业者有实际的回报。

我讲的是***最小描述长度***。你可能会想这究竟是什么…

让我们剥开层层迷雾，看看它有多有用…

### **贝叶斯及其定理**

我们从[托马斯·贝叶斯牧师](https://en.wikipedia.org/wiki/Thomas_Bayes)开始（虽然不是按时间顺序），他没有发表过关于如何进行统计推断的想法，但后来因其同名定理而被永载史册。

![](../Images/d47d81d21a96aaecdf618a5f8bbb2221.png)

这是18世纪下半叶，当时还没有叫做“概率论”的数学科学分支。它仅以相当古怪的“[*机会论*](https://en.wikipedia.org/wiki/The_Doctrine_of_Chances)”之名存在——这个名字源自[亚伯拉罕·德·莫夫雷](https://en.wikipedia.org/wiki/Abraham_de_Moivre)的一本书。一篇名为“[*解决机会论中的一个问题的论文*](http://rstl.royalsocietypublishing.org/content/53/370)”的文章，最初由贝叶斯提出，但由他的朋友[理查德·普莱斯](https://en.wikipedia.org/wiki/Richard_Price)编辑和修订，于1763年提交给皇家学会并发表在*《皇家学会哲学年刊》*中。在这篇论文中，贝叶斯描述了——*以一种相当频率主义的方式*——关于联合概率的简单定理，从而产生了逆概率的计算，即贝叶斯定理。

自那以后，统计科学的两个对立派别——贝叶斯派和频率派——之间发生了许多争论。但为了本文的目的，让我们暂时忽略历史，专注于贝叶斯推断机制的简单解释。要了解一个非常直观的介绍，请参见[布兰登·罗赫](https://brohrer.github.io/blog.html)的[这个精彩教程](https://brohrer.github.io/how_bayesian_inference_works.html)。我将只集中在方程上。

![](../Images/5efdf115cf9573c2c7ca9c6f5c58f863.png)

这基本上表明，你在看到数据/证据（*似然*）后更新你的信念（*先验概率*），并将更新后的信念赋值给术语 *后验概率*。你可以从一个信念开始，但每一个数据点都会加强或削弱这个信念，你会不断更新你的假设。

听起来简单而直观？很好。

我在段落的最后一句话中做了一个小把戏。你注意到了吗？我插入了一个词“***假设***”。这不是普通的英语。这是正式的术语 :-)

在统计推断的世界里，假设是一种信念。它是对过程的真实本质的信念（我们无法观察到），这个过程生成了一个随机变量（我们可以观察或测量，尽管不是没有噪音）。在统计学中，它通常定义为一个概率分布。但在机器学习的背景下，它可以被认为是任何一套规则（或逻辑或过程），我们相信这些规则可以产生我们用来学习这个神秘过程隐藏本质的*示例*或训练数据。

那么，让我们尝试用不同的符号重新表述贝叶斯定理——即数据科学相关的符号。让我们用***D***表示数据，用***h***表示假设。这意味着我们应用贝叶斯公式来尝试确定***给定数据，数据来自哪个假设***。我们将定理重写为，

![](../Images/6f982332bcccdcf238ceb7a6018fd0c6.png)

现在，一般来说，我们有一个大的（通常是无限的）假设空间，即许多假设可供选择。贝叶斯推断的本质在于我们想要检查数据，以最大化一个假设的概率，这个假设最有可能导致观察到的数据。我们基本上想要确定***argmax***的***P***(***h***|***D***)，也就是说我们想知道对于哪个***h***，观察到的***D***最为可能。为此，我们可以安全地忽略分母中的*P*(***D***)，因为它不依赖于假设。这个方案有一个颇为绕口的名字，叫做[最大后验估计 (MAP)](https://en.wikipedia.org/wiki/Maximum_a_posteriori_estimation)。

现在，我们应用以下数学技巧，

+   最大化在对数下的效果与在原始函数下相似，即取对数不会改变最大化问题。

+   积的对数是各个对数的和

+   最大化一个量等同于最小化负的量

![](../Images/c63444e4619e40c2c9290cd6ffc7d560.png)

*越来越奇怪*和*越来越奇怪……*那些负对数为2的术语看起来很熟悉……来自**信息论**！

**克劳德·香农**登场。

### **香农**

描述克劳德·香农的天才与奇特生活将需要[many a volume](https://www.amazon.com/Mind-Play-Shannon-Invented-Information/dp/1476766681)，他几乎独自奠定了信息理论的基础，引领我们进入了现代高速通信和信息交换的时代。

[香农的麻省理工学院硕士论文](https://en.wikipedia.org/wiki/A_Symbolic_Analysis_of_Relay_and_Switching_Circuits)在电气工程领域被称为20世纪最重要的硕士论文：在这篇论文中，22岁的香农展示了如何使用继电器和开关的电子电路来实现19世纪数学家乔治·布尔的逻辑代数。这一最基本的数字计算机设计特征——将“真”和“假”，“0”和“1”表示为开关的开与关，以及使用电子逻辑门进行决策和算术运算——可以追溯到香农论文中的洞见。

但这还不是他最大的成就。

在1941年，香农来到贝尔实验室，在那里他从事战争事务，包括密码学。他还在研究信息和通信背后的原创理论。在1948年，这项工作在贝尔实验室的研究期刊上以一篇[广受赞誉的论文](https://en.wikipedia.org/wiki/A_Mathematical_Theory_of_Communication)的形式发表。

香农通过一个[**类似于定义物理学中热力学熵的方程**](https://en.wikipedia.org/wiki/Entropy_%28information_theory%29)来定义源产生的信息量——例如，消息中的信息量。在最基本的术语中，香农的信息熵是编码消息所需的二进制数字的数量。对于具有概率***p***的消息或事件，最有效（即紧凑）的编码将需要**-*log2(p)***位。

这正是那些出现在*最大后验*表达式中的术语的性质，它们是从贝叶斯定理中推导出来的！

因此，我们可以说，在贝叶斯推理的世界里，最可能的假设取决于两个唤起长度感的术语——**而是最小长度。**

![](../Images/a4466aed82e499c65c8b06a5b444ae49.png)

> 那么，这些术语中的**长度概念**是什么呢？

### **长度（h）：奥卡姆剃刀**

[威廉·奥卡姆](https://en.wikipedia.org/wiki/William_of_Ockham)（*约*1287–1347）是一位英格兰方济各会修士和[神学家](https://en.wikipedia.org/wiki/Theologian)，也是一位具有影响力的中世纪[哲学家](https://en.wikipedia.org/wiki/Philosopher)。他作为伟大逻辑学家的名声主要归功于他所提出的原则，即[奥卡姆剃刀](https://en.wikipedia.org/wiki/Occam%27s_razor)。术语*剃刀*指的是通过“剃除”不必要的假设或拆分两个类似结论来区分两个假设。

归于他的精确词语是：*entia non sunt multiplicanda praeter necessitatem*（实体不应被超出必要性地增加）。用统计术语来说，这意味着我们必须努力使用最简单的假设来令人满意地解释所有数据。

其他杰出人物也有类似的原则。

艾萨克·牛顿：：“*我们应当不引入更多自然现象的原因，除非这些原因既真实又足以解释其表现。*”

伯特兰·罗素：“*只要可能，就用已知实体的构造来替代对未知实体的推测。*”

> **总是优先选择较短的假设**。

需要一个关于***假设长度***的例子吗？

以下哪棵决策树的长度更*小*？**A**还是**B**？

![](../Images/1032427501fc3b073d171d191abd11dd.png)

即使没有对假设‘长度’的精确定义，我相信你也会认为左边（A）的树看起来*更小*或*更短*。当然，你会是对的。因此，一个*较短*的假设是那些具有更少自由参数，或较简单的决策边界（对于分类问题），或者这些**可以表示其简洁性的特征**的假设。

### **‘Length(D|h)’ 怎么样？**

它是给定假设下的数据长度。这意味着什么？

从直觉上看，它与假设的正确性或表征能力有关。它决定了在给定假设的情况下，数据的“推断”效果如何。**如果假设能够很好地生成数据，并且我们可以无误地测量数据，那么我们根本不需要数据。**

想想[牛顿运动定律](https://en.wikipedia.org/wiki/Newton%27s_laws_of_motion)。

当它们首次出现在[*Principia*](https://en.wikipedia.org/wiki/Philosophi%C3%A6_Naturalis_Principia_Mathematica)中时，并没有严格的数学证明支持。它们不是定理。它们更像是基于自然物体运动观察的假设。但它们非常准确地描述了数据。因此，它们成为了物理定律。

这就是为什么你不需要维护和记住一个所有可能的加速度数值与施加于物体上的力的函数的表格。你只需信任那个紧凑的假设，即法则***F=ma***，并相信你需要的所有数字都可以在必要时从中计算出来。这使得***Length(D|h)*** 非常小。

但如果数据与紧凑假设的偏差很大，那么你需要有一个**详细**的描述，说明这些偏差是什么，可能的解释等。

> 因此，***Length(D|h)*** 简洁地捕捉了“**数据与给定假设的契合程度**”的概念。

本质上，它是指错误分类或错误率。对于一个完美的假设，长度很短，极限情况下为零。对于一个不能完全拟合数据的假设，它通常比较长。

> 这就是权衡的问题所在。

如果你用大号的奥卡姆剃刀削减你的假设，你可能会得到一个简单的模型，它不能拟合所有的数据。因此，你需要提供更多的数据以获得更好的信心。另一方面，如果你创建一个复杂（且长）的假设，你可能能够非常好地拟合你的训练数据，但这实际上可能不是正确的假设，因为它与MAP原则相违背，即假设具有小熵。

> 听起来像是偏差-方差权衡？是的，也确实如此 :-)

![](../Images/6d4b5e2452836abb9a9a557aefe5f198.png)

[图片来源](https://www.reddit.com/r/mlclass/comments/mmlfu/a_nice_alternative_explanation_of_bias_and/)

### **综合考虑**

因此，贝叶斯推断告诉我们**最佳假设是最小化两个项的总和：假设的长度和错误率**。

> 在这一句深刻的话中，它几乎捕捉了所有的（监督）机器学习。

想想它的影响，

+   **线性模型**的模型复杂度——选择哪个次数的多项式，如何减少平方残差和

+   **神经网络**的架构选择——如何避免过拟合训练数据并实现良好的验证准确性，同时减少分类错误。

+   **支持向量机**正则化和核选择——在软边界与硬边界之间的平衡，即在准确性与决策边界非线性之间的权衡。

### **我们究竟应得出什么结论？**

从对最小描述长度的分析中我们应该得出什么结论

长度（MDL）原则？

> 这是否一劳永逸地证明了短假设是最佳的？

不。

MDL显示的是，如果选择一种假设表示，使得假设h的大小为——log2 P(***h***)，并且如果选择一种例外（错误）的表示，使得在给定h的情况下D的编码长度等于-log2 P(***D***|***h***), 那么MDL原则将产生MAP假设。

然而，要展示我们有这样的表示，我们必须知道所有的先验概率P(***h***)，以及P(***D***|***h***）。没有理由相信相对于假设和错误/分类错误的任意编码，MDL假设应该被优先考虑。

> 对于实际的机器学习，有时**为一个人类设计者指定一个能捕捉假设相对概率的表示形式**可能比完全指定每个假设的概率更容易。

这就是**知识表示和领域专业知识**变得至关重要的地方。它绕过了（通常）无限大的假设空间，引导我们朝着一个高度可能的假设集前进，我们可以最优地编码并努力找到其中的MAP假设集。

### **总结与反思**

一个美妙的事实是，简单的数学操作在概率论的基本恒等式上，可以产生对监督学习的基本限制和目标的深刻而简洁的描述。有关这些问题的简明处理，读者可以参考这篇名为[“为什么机器学习有效”的卡内基梅隆大学博士论文](http://www.cs.cmu.edu/~gmontane/montanez_dissertation.pdf)。同时，也值得思考这些内容如何与[无免费午餐定理](https://en.wikipedia.org/wiki/No_free_lunch_theorem)相联系。

**如果你对这一领域有更深入的阅读兴趣**

1.  “[无免费午餐与最小描述长度](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.160.798&rep=rep1&type=pdf)”

1.  “ [监督学习中的无免费午餐与奥卡姆剃刀](https://pdfs.semanticscholar.org/83cd/86c2c7e507e8ebba9563a9efaba7c966a1b3.pdf)”

1.  “[无免费午餐与问题描述长度](http://www.no-free-lunch.org/ScVW01.pdf)”

如果你有任何问题或想法要分享，请通过[**tirthajyoti[AT]gmail.com**](mailto:tirthajyoti@gmail.com)联系作者。此外，你还可以查看作者的[**GitHub 仓库**](https://github.com/tirthajyoti?tab=repositories)，了解其他有趣的Python、R或MATLAB代码片段以及机器学习资源。如果你和我一样，对机器学习/数据科学充满热情，请随时[在LinkedIn上添加我](https://www.linkedin.com/in/tirthajyoti-sarkar-2127aa7/)或[在Twitter上关注我](https://twitter.com/tirthajyotiS)。

**简介：[Tirthajyoti Sarkar](https://www.linkedin.com/in/tirthajyoti-sarkar-2127aa7/)** 是一名半导体技术专家、机器学习/数据科学爱好者、电子工程博士、博主和作家。

[原文](https://towardsdatascience.com/when-bayes-ockham-and-shannon-come-together-to-define-machine-learning-96422729a1ad?sk=4272ee34deb65dd8749cd5197b3fe77d)。经许可转载。

**相关：**

+   [数据科学的基础数学：‘为什么’和‘如何’](/2018/09/essential-math-data-science.html)

+   [IT工程师需要学习多少数学才能进入数据科学领域？](/2017/12/mathematics-needed-learn-data-science-machine-learning.html)

+   [为什么你应该忘记数据科学代码中的‘for-loop’，而拥抱向量化](/2017/11/forget-for-loop-data-science-code-vectorization.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速通道进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT工作

* * *

### 更多相关内容

+   [数据来自哪里？](https://www.kdnuggets.com/2022/08/data-come.html)

+   [数据科学家与数据工程师如何协同工作？](https://www.kdnuggets.com/2022/08/data-scientists-data-engineers-work-together.html)

+   [将人类与人工智能代理结合以提升客户体验](https://www.kdnuggets.com/2024/06/softweb/bringing-human-and-ai-agents-together-for-enhanced-customer-experience)

+   [朴素贝叶斯算法：你需要知道的一切](https://www.kdnuggets.com/2020/06/naive-bayes-algorithm-everything.html)

+   [理解贝叶斯定理的3种方法，将提升你的数据科学能力](https://www.kdnuggets.com/2022/06/3-ways-understanding-bayes-theorem-improve-data-science.html)

+   [高斯朴素贝叶斯算法解释](https://www.kdnuggets.com/2023/03/gaussian-naive-bayes-explained.html)
