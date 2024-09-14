# 数据科学家的软件工程最佳实践

> 原文：[https://www.kdnuggets.com/2021/03/software-engineering-best-practices-data-scientists.html](https://www.kdnuggets.com/2021/03/software-engineering-best-practices-data-scientists.html)

[评论](#comments)

**由 [Madison Hunter](https://madison13.medium.com/) 提供，地球科学本科生**

![](../Images/819721409a44c604cc26e9ec52dd53e3.png)

由 [Joonas kääriäinen](https://www.pexels.com/@joonas-kaariainen-67364?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 提供，来自 [Pexels](https://www.pexels.com/photo/bridge-with-lights-at-night-240572/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

在我自己开始编写数据分析代码之前，我真的不明白为什么人们会抱怨数据科学家编写的代码。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的IT

* * *

来自软件开发的教育背景让我觉得自己对编码最佳实践有很好的掌握，并且对编写干净的代码充满信心。然后，当我开始编写第一次数据分析代码时。

你猜对了。

缺乏函数、不清晰的变量名、意大利面条式的代码、没有单元测试的迹象以及风格的严重缺乏，使得我在让代码工作后，面对的代码相当于一锅混乱的早餐。

最后，我终于理解了那些工程师抱怨的原因。

数据科学不是一个自然源于计算机科学的领域，这一点反映在数据科学家拥有的各种不同背景上。许多数据科学家甚至没有计算机科学的学位，他们往往来自其他不相关的领域，包括数学、科学、工程、商业、医学等。

因此，数据科学家不以总是拥有最干净的代码而闻名也就不足为奇了。

我并不是说数据科学家需要能够编写整个复杂的库。相反，数据科学家应该能够生成干净的代码，这些代码可以被更新、调试，并且在生产环境中使用时工程部门不会发出太多脏话。

编写干净代码的重要性不仅仅是为了他人。在许多公司中，预算不足以聘请数据科学家*和*软件工程师，这意味着数据科学家通常负责创建生产就绪的代码。因此，个人的理智也应该作为维护和开发干净、易用和可重用代码的一个因素。

许多人会争论优秀的代码是主观的。然而，通常有四个标准是每个人都能同意的，它们定义了使用最佳实践编写的代码：

1.  优秀的代码应该是高效的——这意味着你需要从你的 Python 代码中挤出每一丝速度和效率，即使在没有的情况下也是如此。

1.  优秀的代码是可维护的——这意味着你可以维护代码，并且其他人也可以轻松理解和维护你的代码。

1.  优秀的代码是可读的且结构良好的——这意味着任何人都应该能够查看你的代码并理解你试图实现的目标，而不需要费太大劲。

1.  优秀的代码是可靠的——干净的代码是好的代码，而好的代码是可靠的，不容易出现错误或随机故障。

查看这些最佳实践，你可以应用它们来达到上述四个标准。

### 使用描述性变量名。

我经常看到数据分析代码使用变量名如 x、y，或者任何数学变量的英文单词。

虽然这在编写数学公式时有效，但在阅读他人的代码时并不那么清晰。

在数据科学方面，我喜欢明确我的变量是什么。例如：

```py
null_hypothesis
standard_deviation_paired_differences_population
degrees_of_freedom
observed_frequency
test_statistic_chi_squared
```

任何修过统计课程的人都会理解我的变量是什么，以及它们在我的方程式中所做的事情。这些基本变量可以在特定的计算或使用时变得更具描述性。

诀窍是尽可能详尽描述情况。当然，有时候计算如此复杂，以至于写出如此描述性的变量没有意义。然而，在这种情况出现之前，尽量做到描述性。

### 使用函数。

函数，无论你采用什么编程方法论（面向对象、函数式等），对于保持代码干净、简洁、可读和 DRY（我稍后会谈到）是至关重要的。

对于非计算机科学专业的数据科学家来说，函数并不总是直观的，因为代码在没有函数的情况下也能正确运行。

然而，最好的程序员被认为是最懒的程序员。为什么？因为他们写的代码最少，并且往往写出最简洁的代码来解决问题。他们尽可能少地工作以使代码运行，并且通常会产生最简洁的解决方案。这涉及到使用函数。

以下是一些编写优秀函数以保持代码整洁的技巧：

+   函数应该只做一件事。不是 10 件事。不是 20 件事。只是做一件事。

+   函数应该尽量小。有些人认为函数的代码行数不应超过20行，但这是一个任意的数字。尽量保持函数简洁明了。

+   函数应该编写得让你能够从上到下阅读和理解其逻辑——就像你读一本书一样。

+   尽量将函数参数的数量保持在最小（少于3个为佳）。如果你要求更多的函数参数，问问自己这个函数是否只完成一个任务。

+   确保函数中的逻辑正确缩进，并且整个函数的代码被适当地分块。这将帮助其他人看到函数内部的代码，以及函数的开始和结束位置。

+   使用描述性名称。

### 使用评论并编写适当的支持文档。

当你在构建改变生活的模型时，这一步很容易被忽视，但事实是，如果没有其他人能够使用它们，那么它们真的能改变生活吗？

我的个人评论规则：

+   在代码文件的顶部写下简要描述代码目标的评论。

+   在每个函数的顶部写下描述其输入、输出和执行逻辑的评论。

+   在我不完全理解的逻辑上方写下评论，以便更好地组织我的思路。这有助于调试和代码重构。

在编写适当的支持文档时，文档不必比READme文件更复杂。最基本的，好的文档应包括：

+   代码的目标。

+   如何安装和使用代码的说明。

+   详细解释代码中棘手的部分，逐行描述代码的具体功能，并可能解释你为什么选择这样编写代码。

+   用于调试和故障排除的截图。

+   有助于进一步描述和解释你的逻辑或代码的外部支持文档的有用链接。

### 使用一致的编码风格，并熟悉你使用的语言的语法惯例。

我承认我并不总是遵循某种语言的正确惯例。

在大学时，我主要学习了C#编程，对其惯例非常熟悉并感到舒适，随后我懒散地将这些惯例应用到我遇到的其他语言中。

不要像我一样。相反，花时间学习每种新语言的语法惯例，并强迫自己正确使用这些惯例。这不仅有助于你更深入地融入语言，还能帮助你编写更清晰的代码，并有助于你与使用相同语言的其他开发者沟通。

这是一个完成相同功能的代码示例，使用两种不同的语言和每种语言的适当惯例（请注意，由于格式问题，可能缺少缩进）：

C#:

```py
// This is a comment in C#. I am going to write some code that prints out a value if the inputted number is correct.int year = 0;
Console.Write("\nEnter the year that C# became a language: ");
year = Convert.ToInt32(Console.ReadLine());if(year == 2000) 
{
Console.Write("That is correct!");
}
else 
{
Console.Write("This is incorrect.");
}
```

Python:

```py
# This is a comment in Python. This code is going to do the same job that the code above just did.year = int(input("Enter the year that Python became a language: "))if year == 1989:
print ("That is correct!")
else:
print ("That is incorrect.")
```

这段傻乎乎的代码其实没做什么，但它让你对每种语言的约定差异有了一个大致的了解。

### 使用库。

使用现有的库可以节省大量时间，特别是在编写数据分析代码时。Python 充满了能够处理数据科学家需求的库。这些库不仅已经为你编写好，而且已经调试并准备好投入生产。

查看这篇文章，概述了 [顶级数据科学库](https://towardsdatascience.com/top-10-python-libraries-for-data-science-cd82294ec266) 及其功能。我在这里总结了我发现最有用的七个库：

### NumPy

+   基本数组操作：添加、乘法、切片、扁平化、重塑、索引。

+   高级数组操作：堆叠数组，拆分成多个部分。

+   线性代数。

### Pandas

+   索引、操作、重命名、排序和合并数据框。

+   对数据框中的列进行基本的 CRUD（更新、添加、删除）功能。

+   输入缺失的文件并处理缺失数据。

+   创建直方图或箱线图。

### Matplotlib

+   所有的数据可视化，包括折线图、散点图、条形图、直方图、饼图、茎叶图和频谱图等。

### TensorFlow

+   语音和声音识别。

+   情感分析。

+   人脸识别。

+   时间序列。

+   视频检测。

### Seaborn

+   确定变量之间的相关性。

+   分析单变量或双变量分布。

+   绘制依赖变量的线性回归模型。

### SciPy

+   常见的科学计算，包括线性代数、插值、统计、微积分和常微分方程。

### Scikit-Learn

+   用于分类、聚类、回归、降维、模型选择和预处理的监督学习和无监督学习算法。

### 使用一个好的 IDE。

Jupyter 并不适合编写生产就绪代码。

尽管有方法将 Jupyter Notebooks 用于创建生产就绪代码，但我发现最好还是在数据探索和分析的早期阶段使用它，然后在适当的 IDE 中开发生产级代码。

IDE 提供了简单的工具来保持代码清晰，如代码检查、自动格式化、语法高亮、查找功能和错误捕获。此外，还有大量的 IDE 扩展可以帮助你在编写代码时节省时间。

一些你可以查看的 IDE 包括：

+   [Visual Studio Code](https://code.visualstudio.com/)（我个人的最爱）

+   [PyCharm](https://www.jetbrains.com/pycharm/)

+   [Atom](https://ide.atom.io/)

+   [Spyder](https://www.spyder-ide.org/)

### 保持代码的 DRY（不要重复自己）。

DRY（不要重复自己）是一个软件工程最佳实践，旨在保持代码清晰、简洁和直接。

目标是不要重复任何代码。这意味着，如果你发现自己重复编写相同的代码行，你需要将这些代码转换为一个只编写一次的函数。然后可以多次调用这个函数。

如果你在保持代码干净方面遇到困难，只需使用 [我最喜欢的规则](https://codeburst.io/clean-code-functions-475fe755c4f1)：

1.  如果这是第一次，编写代码。

1.  如果这是第二次，复制它。

1.  如果这是第三次，将其做成一个函数或类。

### 编写单元测试。

[单元测试](https://www.guru99.com/unit-testing-guide.html) 是一种软件测试类型，涉及测试代码中的各个组件。单元测试是为了确保每一段代码按预期执行。这些测试会检查执行特定功能的小段代码，以确保它准确完成了其工作。被测试的“单元”可以是单独的函数、过程或整个对象。

单元测试的一个很好的例子是测试一个函数在接收到不同参数类型时的响应。这个测试的结果将确保你已考虑到每种可能性，并且函数不会在之后抛出任何无法解释的错误。

查看微软关于如何编写单元测试的这篇文章：

[**编写单元测试的最佳实践 - .NET**](https://docs.microsoft.com/en-us/dotnet/core/testing/unit-testing-best-practices)

编写单元测试有许多好处；它们有助于回归测试，提供文档，并促进…

虽然这篇文章仅描述了如何在 C# 中编写单元测试，但所学的课程和示例适用于任何语言。

### 不要过于执着于一行解决方案。

一行代码的解决方案很棒，如果你想给朋友留下深刻印象或想在 Medium 上写一篇病毒式文章。谁不想用一行代码交易股票呢？

虽然使用一行代码创建解决方案令人印象深刻，但在调试和重构代码时，你也在为自己带来麻烦。

相反，尝试每行保持一到两个函数调用，并确保你的逻辑对任何阅读你代码的人都易于理解。如果你不能用简单的术语解释一行代码在做什么，那可能是这行代码太复杂了。

必须指出的是，开发人员编写的代码行数并不能很好地指示他们的能力或技术水平——不要听信那些告诉你不同意见的人。

### 最后的思考。

本文强调了数据科学家可以实施的最重要的最佳实践，以使他们的代码准备好投入生产。

你的代码是你的责任，因此花时间确保你编写的代码高效、简单且易于理解是有意义的。虽然还有很多其他编写干净代码的技巧和窍门，但上述列出的是你今天就可以应用的最简单的技巧。

**简介：[Madison Hunter](https://madison13.medium.com/)** 是地球科学本科生，软件开发毕业生。Madison 撰写关于数据科学、环境和 STEM 的随笔。

[原文](https://towardsdatascience.com/software-engineering-best-practices-for-data-scientists-4c199ede6e03)。已获许可转载。

**相关内容：**

+   [数据科学的软件工程技巧和最佳实践](/2020/10/software-engineering-best-practices-data-science.html)

+   [我从高效数据科学家那里学到的15个习惯](/2021/03/15-habits-learned-from-highly-effective-data-scientists.html)

+   [数据科学家的软件工程基础](/2020/06/software-engineering-fundamentals-data-scientists.html)

### 更多主题

+   [软件开发者与软件工程师](https://www.kdnuggets.com/2022/05/software-developer-software-engineer.html)

+   [将 ChatGPT 集成到数据科学工作流中：技巧和最佳实践](https://www.kdnuggets.com/2023/05/integrating-chatgpt-data-science-workflows-tips-best-practices.html)

+   [数据仓库和 ETL 最佳实践](https://www.kdnuggets.com/2023/02/data-warehousing-etl-best-practices.html)

+   [云和数据迁移到 AWS 云的11个最佳实践](https://www.kdnuggets.com/2023/04/11-best-practices-cloud-data-migration-aws-cloud.html)

+   [数据可视化最佳实践与有效沟通的资源](https://www.kdnuggets.com/2023/04/data-visualization-best-practices-resources-effective-communication.html)

+   [数据科学团队协作的5个最佳实践](https://www.kdnuggets.com/2023/06/5-best-practices-data-science-team-collaboration.html)
