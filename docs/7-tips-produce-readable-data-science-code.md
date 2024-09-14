# 7个提高数据科学代码可读性的技巧

> 原文：[https://www.kdnuggets.com/2022/11/7-tips-produce-readable-data-science-code.html](https://www.kdnuggets.com/2022/11/7-tips-produce-readable-data-science-code.html)

![7个提高数据科学代码可读性的技巧](../Images/2d92303704dd6bf8eee0b9ed4b0f0f8c.png)

[由svstudioart提供的图片](https://www.freepik.com/free-vector/programmer-working-web-development-code-engineer-programming-python-php-java-script-computer_14723893.htm#query=python%20code&position=4&from_view=search&track=sph) 在Freepik上

编写可读代码的能力被开发者称为一种艺术形式。虽然我部分同意这种说法，但编写代码，尤其是可读代码，是一种可以培养的技能。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 快速通道进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持您的组织进行IT支持

* * *

提高代码可读性的唯一方法是多练习编写优质代码。因此，我建议阅读其他以编写高质量代码著称的开发者的代码。

总的来说，可读代码是一种重要的结果，它随着代码复杂性增加而变得更加重要。尤其是在数据科学中，编写可读代码极为重要，因为数据科学应用可能很难理解，因此，不够清晰的代码所增加的额外复杂性是不可取的。

我认为你同意编写可读代码是至关重要的。那我该如何让我的代码更具可读性呢？

在这篇文章中，我们将讨论一些可以采取的步骤，以生成可读且高质量的代码。

# 在开始编码之前要有一个结构的概念

在打开编辑器并开始编写代码之前，尝试规划一下代码结构。尽可能详细地制定变量、函数、类和模块的结构，并确定它们如何相互连接以解决问题。

这样做会在你实现、扩展和部署代码时节省大量时间。我建议你将这些结构添加到代码文档中，或者如果你计划将代码开源，将其放在GitHub上。

# 给变量起描述性的名称

我知道我们有时会被诱惑给变量命名为X、Y和Z。但当我们在几个月后阅读代码时，却因为无法确定变量X中到底存储了什么而感到困惑！给变量起具有描述性的名称不仅会帮助其他人阅读代码，还会帮助未来的你。

命名变量时，目标是使用准确的名称，而不是简短的名称。例如，如果你计算一个值列表的平均值，不要将变量命名为 ave 或 av；有时使用 average_height 或 average_time。现在，许多代码编辑器提供自动补全功能，因此使用更长的名称不会使你的代码编写过程变慢。

此外，如果你的代码实现了某篇特定论文或书籍中介绍的算法，请保持变量名称与该来源相关。记得在代码文件的顶部包括该来源。

# 明智地使用函数

函数可以是组织和简洁代码的好工具。也就是说，如果使用得当的话。对那些可以封装到函数中的任务使用函数，例如，对不同数据点应用操作或实现算法步骤。当你命名函数时，使用我们在命名变量时讲过的相同逻辑。

将具有相关功能的函数收集在一个代码文件中，并尽可能将其制成模块。这将使查找、扩展和使用函数变得更容易。

尽量明确函数属性的具体类型，并使你的函数安全且可扩展。

# 瞄准清晰而简洁的文档字符串

文档化你的代码是一个重要步骤，无论是完整的文档还是代码内文档（文档字符串）。文档字符串是在代码文件开始处、函数/类定义后出现的字符串，它们告诉读者代码/函数或类的目的。

文档字符串旨在简要提示你的代码是什么以及如何工作。例如，当在函数的开头（函数头下面）使用时，它应包括预期的属性类型及其在函数中的作用、函数的输出以及一两句关于该输出如何计算的描述。

对于类，文档字符串应包含类属性和方法，以及它们如何使用。

# 不要重新发明轮子（除非你能做得更好）

如果你需要的函数已经由支持的包或第三方开发者实现，使用它而不是重新实现。当你使用一个包时，确保了解它包含的所有函数，以节省实现可以使用的功能的时间。

我推荐你自己实现功能的少数场景是当你刚开始编程并尝试了解一切是如何运作的，或当你可以以更少的复杂性更好地实现一个函数。否则，你和其他人使用已经实现的代码会更简单。

# 目标是简单、较长的步骤，而不是短而复杂的步骤

当你尝试实现论文或书籍中提出的想法或算法时，目标应是明确的步骤，而不是试图将多个步骤组合在一起以减少代码长度。

是的，较短的代码可能展示了你使用编程语言惯用法的能力。然而，它也可能使你的代码变得不必要的复杂。复杂到难以阅读、测试、调试和扩展。尤其是当你实现的算法本身就很复杂时，通过将多个步骤组合在一起而增加额外的复杂性，会导致代码变得不够灵活。

# 保持一致

一致性对于代码的可读性至关重要。在规划代码结构时，决定使用一种风格贯穿整个代码。这包括确定命名变量、函数和类的系统。如何使用注释，处理算法中的不同数学步骤，模块化代码以及使用现有的包。

当你的代码风格和模式一致时，跟踪和理解你的代码会更快。

# 最后的思考

作为数据科学家，不可避免的一件事就是使用别人编写的代码。尽管阅读和理解他人编写的代码总是一个耗时的任务，但你可以采取一些步骤，使你的代码更易于跟随和使用。

尽管本文中的技巧可以被任何写代码的人使用，而不仅仅是数据科学家，但在我看来，对于数据科学家来说，编写可读代码尤为重要，以克服由于大多数数据科学算法背后的数学而产生的一些困难。

因此，如果你想开始编写更好、更易读的代码，这篇文章将是一个很好的起点。记住，编写更好的代码是一项技能，就像其他技能一样，随着实践会不断提高。

**[Sara Metwalli](https://www.linkedin.com/in/sara-a-metwalli/)** 是庆应大学的博士候选人，研究测试和调试量子电路的方法。我是IBM的研究实习生和Qiskit倡导者，帮助构建一个更加量子的未来。我还是Medium、Built-in、She Can Code和KDN的作家，撰写关于编程、数据科学和技术主题的文章。我也是国际女性编程Python分会的负责人，一个火车爱好者、旅行者和摄影爱好者。

### 更多相关话题

+   [机器学习未能为我的业务创造价值。为什么？](https://www.kdnuggets.com/2021/12/machine-learning-produce-value-business.html)

+   [ChatGPT代码解释器：几分钟内完成数据科学](https://www.kdnuggets.com/2023/07/chatgpt-code-interpreter-data-science-minutes.html)

+   [你可以用ChatGPT的代码解释器做数据科学的5种方法](https://www.kdnuggets.com/2023/08/5-ways-chatgpt-code-interpreter-data-science.html)

+   [拖拽、放置、分析：无代码数据科学的兴起](https://www.kdnuggets.com/drag-drop-analyze-the-rise-of-nocode-data-science)

+   [作为数据科学家的可重用Python代码管理](https://www.kdnuggets.com/2021/06/managing-reusable-python-code-data-scientist.html)

+   [如何作为数据科学家对你的 Python 代码进行注释](https://www.kdnuggets.com/how-to-comment-your-python-code-as-a-data-scientist)
