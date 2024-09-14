# 简单的面向对象编程的混合如何能提升你的深度学习原型

> 原文：[https://www.kdnuggets.com/2019/08/simple-mix-object-oriented-programming-sharpen-deep-learning-prototype.html](https://www.kdnuggets.com/2019/08/simple-mix-object-oriented-programming-sharpen-deep-learning-prototype.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

![](../Images/639dd33fb84f100e7f6a32b3beae256d.png)

### 介绍

本文不适合经验丰富的软件工程师。**本文面向的数据科学家和机器学习（ML）从业者**，像我一样，他们没有软件工程背景。

我们在工作中大量使用Python。为什么？因为它对ML和数据科学社区非常出色。

它正在成为[现代数据驱动分析和人工智能（AI）应用程序中增长最快的主要语言](https://stackoverflow.blog/2017/09/14/python-growing-quickly/?source=post_page---------------------------)。

然而，它也被用于简单的脚本目的，比如[自动化操作](https://automatetheboringstuff.com/?source=post_page---------------------------)、[测试假设](https://machinelearningmastery.com/statistical-hypothesis-tests-in-python-cheat-sheet/?source=post_page---------------------------)、创建[用于头脑风暴的交互式图表](https://mode.com/blog/python-interactive-plot-libraries?source=post_page---------------------------)、[控制实验室仪器](https://hackaday.com/2016/11/16/how-to-control-your-instruments-from-a-computer-its-easier-than-you-think/?source=post_page---------------------------)等。

但问题在于。

**用于软件开发的Python和用于脚本编写的Python并不完全相同——至少在数据科学领域如此。**

> 脚本编程（大多数情况下）是你为自己编写的代码。软件是你（和其他团队成员）为他人编写的代码集合。

明智的做法是承认，当（大多数）数据科学家，他们没有软件工程背景时，编写用于AI/ML模型和统计分析的Python程序，他们往往是为了*自己*。

他们只是想快速找到隐藏在数据中的模式的核心。不加深思地进行，不考虑普通用户的需求——*用户*。

他们编写了一段代码来生成丰富美观的图表。但他们并没有从中创建一个***函数***，以便以后使用。

他们从标准库中导入了大量的***方法***和***类***。但他们并没有通过***继承***创建自己的***子类***并为其添加方法以扩展功能。

***函数、继承、方法、类***——这些是稳健的**面向对象编程（OOP）**的核心，但如果你只是想创建一个包含数据分析和图表的Jupyter笔记本，它们是可以有所回避的。

> 你可以避免使用OOP原则的最初痛苦，但这几乎总是会使你的Notebook代码无法重用和扩展。

简而言之，那段代码只对你有用（直到你忘记了具体编码的逻辑），而对其他人没有任何帮助。

不过，[**可读性（从而可重用性）至关重要**](https://devblogs.microsoft.com/oldnewthing/20070406-00/?p=27343&source=post_page---------------------------)。这是你所生产的代码真正的价值所在。不仅仅是为了自己，而是为了他人。

实际上，数据科学家[Will Koehrsen](https://towardsdatascience.com/u/e2f299e30cb9?source=post_page---------------------------)刚刚写了一篇关于这个理念的精彩文章。

**[《代码大全》中的软件构建笔记](https://towardsdatascience.com/notes-on-software-construction-from-code-complete-8d2a8a959c69?source=post_page---------------------------)**

《代码大全：软件构建实用手册》的经验教训及其在数据科学中的应用。

最重要的是，**数以百计的热门MOOC或在线数据科学和AI/ML课程也没有强调这方面的编码，因为这对年轻的、充满热情的学习者来说感觉是一种负担**。他/她来到这里是为了学习酷炫的算法和神经网络优化，而不是Python中的面向对象编程（OOP）。因此，这一方面仍然被忽视。

那么，你可以做些什么呢？

### 简单的OOP混合可以使你的深度学习（DL）代码更加高效。

我不是软件工程师，也从未在我的生活中担任过这一职务。因此，当我开始探索机器学习和数据科学时，我写了大量粗糙、不可重用的代码。

我正在逐渐改进自己的编码风格，通过简单的增强使其对全世界的任何人更有用。

> 我发现，开始在你的数据科学代码中混合OOP原则并不需要太多努力。

即使你一生中从未上过软件工程课程，一些理念也可能会自然地浮现。**你所需要做的就是站在他人的角度思考，考虑那个人如何以建设性的方式使用你的代码**。

+   如果你的分析中有一个代码块出现了多次（无论是完全相同的形式还是稍有变化的形式），你能将其制作成一个函数吗？

+   当你制作这样的函数时，哪些参数将会被传递？[其中哪些可以是可选的？默认值是什么](https://www.programiz.com/python-programming/function-argument?source=post_page---------------------------)？

+   如果你遇到一种情况，不知道需要传递多少参数，[你是否使用了Python提供的*args、**kwargs](https://www.geeksforgeeks.org/args-kwargs-python/?source=post_page---------------------------)？

+   你是否为那个函数写了[文档字符串](https://www.geeksforgeeks.org/python-docstrings/?source=post_page---------------------------)，以便让其他人知道该函数的功能和期望的参数，或许还包括一个示例？

+   当你收集了一堆这样的实用函数后，你是否还在使用同一个 Notebook，还是 [切换到一个新的、干净的 Notebook 并仅调用](http://web.cs.iastate.edu/~smkautz/cs127f16/notes/chapter07/?source=post_page---------------------------)“from *my_utility_script* import *func1, func2, func3*”（你是否将 *my_utility_script* 创建为一个简单的 Python 文件，而不是 Jupyter Notebook）？

+   你是否将*my_utility_script*放在一个目录中， [在同一目录下放置一个*__init__.py* 文件（即使是空白的），并将其设为 Python 模块](https://timothybramlett.com/How_to_create_a_Python_Package_with___init__py.html?source=post_page---------------------------)以便像 NumPy 或 Pandas 一样可以导入？

+   你是否考虑不仅仅是从像 NumPy 和 TensorFlow 这样的优秀包中导入类和方法，而是 [向它们添加你自己的方法并扩展其功能](https://stackoverflow.com/questions/25458433/how-to-extend-class-method?source=post_page---------------------------)？

我说的这些到底是什么意思？让我们通过一个简单的案例来演示——一个使用[fashion MNIST](https://github.com/zalandoresearch/fashion-mnist?source=post_page---------------------------) 数据集的深度学习图像分类问题。

### 深度学习分类任务的案例说明：

### 方法

[详细的 Notebook 见我的 Github 仓库](https://github.com/tirthajyoti/Deep-learning-with-Python/blob/master/Notebooks/OOP_principle_deep_learning.ipynb?source=post_page---------------------------)。鼓励你浏览它，并为自己的使用和扩展进行分叉。

代码对构建出色的软件至关重要，但未必适合 Medium 文章，因为你阅读这些文章是为了获得见解，而不是进行调试或重构练习。

因此，我将挑选一些代码片段，并尝试指出我在此 Notebook 中如何尝试编码之前详细介绍的一些原则。

### 核心机器学习任务与更高阶的业务问题

核心机器学习任务很简单——为 [**fashion MNIST**](https://github.com/zalandoresearch/fashion-mnist?source=post_page---------------------------) 数据集构建一个深度学习分类器，这是对原始著名 MNIST 手写数字数据集的有趣变体。Fashion MNIST 包含 60,000 张 28 x 28 像素的训练图像——与时尚相关的物体，例如帽子、鞋子、裤子、T 恤、连衣裙等。它还包括 10,000 张用于模型验证和测试的测试图像。

![图示](../Images/70adab23bb60dcdfb43ae1c220a47516.png)

**Fashion MNIST (**[**https://github.com/zalandoresearch/fashion-mnist**](https://github.com/zalandoresearch/fashion-mnist?source=post_page---------------------------)**)**

但是如果围绕这个核心机器学习任务有一个更高阶的优化或视觉分析问题——***模型架构复杂性如何影响达到期望准确度所需的最小训练轮次***？

> 应该让读者明白，我们为什么要关注这样的问题。**因为这与整体业务优化相关**。[训练神经网络不是一个简单的计算问题](https://www.technologyreview.com/s/613630/training-a-single-ai-model-can-emit-as-much-carbon-as-five-cars-in-their-lifetimes/?source=post_page---------------------------)。因此，调查 **为达到目标性能指标所需的最少训练努力，以及架构选择如何影响这一点** 是有意义的。

在这个例子中，我们甚至不会使用卷积网络，因为一个简单的密集连接神经网络可以实现相当高的准确度，实际上，稍微不理想的性能是为了说明我们提出的高阶优化问题的主要点。

### 我们的解决方案

所以，我们必须解决两个问题 -

+   如何确定达到期望准确度目标所需的最小轮次？

+   模型的具体架构如何影响这个数字或训练行为？

为了实现目标，我们将使用两个简单的面向对象原则，

+   创建一个 [从基类对象继承的类](https://www.digitalocean.com/community/tutorials/understanding-class-inheritance-in-python-3?source=post_page---------------------------)

+   创建实用函数并从紧凑的代码块中调用它们，这些代码块可以呈现给外部用户以进行高阶优化和分析。

### 展示良好实践的代码片段

这里我们展示了一些代码片段，以说明如何利用简单的面向对象原则来实现我们的解决方案。这些片段带有注释，便于理解。

*首先，我们继承一个 Keras 类，并编写我们自己的子类，添加一个用于检查训练准确度的方法，并根据该值采取行动。*

![](../Images/bd4010a9987717834876f7f1c5552bfd.png)

这个 [简单回调](http://keras.io/callbacks/?source=post_page---------------------------) 结果是 **动态控制训练轮次**——当准确率达到所需阈值时，训练会自动停止。

![](../Images/257b65d8deecbf013ad98cd406f9182a.png)

我们将 Keras 模型构建代码放在一个实用函数中，以便通过一些函数参数的简单用户输入，**可以生成具有任意层数和架构（只要它们是密集连接的）的模型**。

![](../Images/89ad51c5f979024fb921cffb7c4b97f8.png)

我们甚至可以将编译和训练代码放入一个实用函数中，以 **方便地在高阶优化循环中使用这些超参数**。

![](../Images/30056cdf2206d15d4c48d431e76dbf4c.png)

接下来是可视化的阶段。在这里，我们仍然遵循功能化的实践。通用的绘图函数将原始数据作为输入。然而，如果我们有特定的绘图目的，如绘制训练集准确率的演变并显示它如何与目标进行比较，那么我们的**绘图函数应该仅接受深度学习模型作为输入**并生成所需的图表。

![](../Images/6b64701031704a27e0a3bd626167ddf5.png)

一个典型的结果如下所示，

![](../Images/0bdfd3c2e031522e8a65b82f7a7d4e94.png)

### 最终的分析代码——超级紧凑且简单

现在我们可以利用之前定义的所有函数和类，将它们整合在一起完成更高阶的任务。

因此，我们的最终代码将非常紧凑，但它将生成与上述相同的有趣的损失和准确率图表，针对各种准确率阈值和神经网络架构。

这将使用户能够使用极少量的代码来生成关于性能指标（在此情况下为准确率）和神经网络架构的可视化分析。**这是构建优化机器学习系统的第一步。**

我们生成了一些案例用于调查，

![](../Images/d80a17328c8bc26cc0bc355ca2dce3bb.png)

我们最终的分析/优化代码简洁且易于高层用户理解，**不需要了解 Keras 模型构建或回调类的复杂性**。

> 这就是面向对象编程的核心原则——抽象复杂性层次，我们能够为我们的深度学习任务实现这一点。

请注意，我们如何将`print_msg=False`传递给类实例。在初步检查/调试时我们需要基本的状态打印，但在优化任务中我们应该静默执行分析。如果我们在类定义中没有这个参数，那么我们将没有办法停止打印调试信息。

我们展示了一些代表性结果，这些结果是从执行上面的代码块自动生成的。它清楚地展示了通过极少量的高级代码，我们能够生成可视化分析，以判断各种神经网络架构在不同性能指标水平上的相对性能。这使得用户在不调整底层函数的情况下，能够轻松地根据自己的性能需求判断模型的选择。

![](../Images/d3231e01d1652f36248138eddf1eb779.png)

另请注意每个图表的自定义标题。这些标题清晰地阐明了目标性能和神经网络的复杂性，从而使分析变得简单。

这只是对绘图实用函数的小小补充，但这展示了在创建这些函数时需要细致规划。如果我们没有为该函数规划这样的参数，就无法为每个图生成自定义标题。**这种对 API（应用程序接口）的细致规划是良好面向对象编程的一部分**。

### 最后，将脚本转化为一个简单的 Python 模块。

到目前为止，你可能在使用 Jupyter 笔记本，但你可能希望将这个练习转化为一个整洁的 Python 模块，你可以随时导入。

就像你写“*from matplotlib import pyplot*”一样，你可以在任何地方导入这些实用函数（Keras 模型构建、训练和绘图）。

![](../Images/30ffeea0a36dbac4577c113acf61caec.png)

### 总结与结论

我们展示了一些简单的良好实践，借鉴了面向对象编程，以应用于深度学习分析任务。对于经验丰富的软件开发人员来说，这些做法可能显得微不足道，但**这篇文章是为那些可能没有这种背景但应该理解将这些良好实践融入他们的机器学习工作流程的重要性的初学数据科学家们准备的**。

[笔记本在这里](https://github.com/tirthajyoti/Deep-learning-with-Python/blob/master/Notebooks/OOP_principle_deep_learning.ipynb?source=post_page---------------------------)。

冒着重复自己的风险，让我再次总结一下好的实践，

+   每当有机会时，**将重复的代码块转化为实用函数**。

+   **仔细考虑函数的 API**，即需要哪些最少的参数集以及它们如何服务于更高层次的编程任务。

+   不要忘记**为函数编写文档字符串**，即使它只是一个单行描述。

+   如果你开始积累许多与同一对象相关的实用函数，考虑**将该对象转化为类**，并将这些实用函数作为方法。

+   **每当有机会时扩展类功能**，通过继承来完成复杂的分析。

+   **不要停留在 Jupyter 笔记本上**。将它们转化为**可执行脚本**并放入**小模块**中。养成**模块化工作的习惯**，这样可以方便地被任何人、任何地方重用和扩展。

谁知道呢，当你积累了足够多的有用类和子模块时，你可能会在 Python 包存储库（PyPi 服务器）上发布一个实用包。那时你将有资格炫耀发布一个原创的开源包 :-)

如果你有任何问题或想法，请通过[**tirthajyoti[AT]gmail.com**](mailto:tirthajyoti@gmail.com?source=post_page---------------------------)联系作者。此外，你还可以查看作者的[**GitHub**](https://github.com/tirthajyoti?tab=repositories&source=post_page---------------------------) **代码库**，获取Python、R或MATLAB中的有趣代码片段和机器学习资源。如果你像我一样，对机器学习/数据科学充满热情，请随时[在LinkedIn上添加我](https://www.linkedin.com/in/tirthajyoti-sarkar-2127aa7/?source=post_page---------------------------)或[在Twitter上关注我](https://twitter.com/tirthajyotiS?source=post_page---------------------------)。

**个人简介：[Tirthajyoti Sarkar](https://www.linkedin.com/in/tirthajyoti-sarkar-2127aa7/)** 是ON Semiconductor的高级首席工程师，专注于基于深度学习/机器学习的设计自动化项目。

[原文](https://towardsdatascience.com/how-a-simple-mix-of-object-oriented-programming-can-sharpen-your-deep-learning-prototype-19893bd969bd)。经授权转载。

**相关内容：**

+   [使用Python进行优化：如何用最少的风险赚取最多的财富？](/2019/06/optimization-python-money-risk.html)

+   [如何在Python中检查回归模型的质量？](/2019/07/check-quality-regression-model-python.html)

+   [数学编程——提升数据科学水平的关键习惯](/2019/05/mathematical-programming-key-habit-advancing-data-science.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的IT需求

* * *

### 更多相关主题

+   [机器学习不像你的大脑，第5部分：生物神经元……](https://www.kdnuggets.com/2022/07/machine-learning-like-brain-part-5-biological-neurons-cant-summation-inputs.html)

+   [Python：机器学习的编程语言](https://www.kdnuggets.com/2022/06/mlm-python-programming-language-machine-learning.html)

+   [3种简单方法加速你的Python代码](https://www.kdnuggets.com/2022/10/3-simple-ways-speed-python-code.html)

+   [个性化AI变得简单：你的无代码GPT适配指南](https://www.kdnuggets.com/personalized-ai-made-simple-your-no-code-guide-to-adapting-gpts)

+   [Llama, Llama, Llama：使用你的内容进行本地RAG的3个简单步骤](https://www.kdnuggets.com/3-simple-steps-to-local-rag-with-your-content)

+   [生成式人工智能如何帮助你改善数据可视化图表](https://www.kdnuggets.com/how-generative-ai-can-help-you-improve-your-data-visualization-charts)
