# 让 Python 程序闪电般快速

> 原文：[https://www.kdnuggets.com/2020/09/making-python-programs-blazingly-fast.html](https://www.kdnuggets.com/2020/09/making-python-programs-blazingly-fast.html)

[评论](#comments)

**由 [Martin Heinz](https://www.linkedin.com/in/heinz-martin/)，IBM 的 DevOps 工程师**

*Python* 反对者总是说，他们不想使用它的原因之一是它*慢*。好吧，是否特定程序——无论使用何种编程语言——快还是慢，很大程度上取决于编写它的开发人员及其编写*优化*和*快速*程序的技能和能力。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 在 IT 方面支持你的组织

* * *

那么，让我们证明一些人的观点是错误的，看看如何提高我们*Python* 程序的性能，让它们真正变快！

![图](../Images/8e2b22b053bf334d3c9ae781110804fe.png)

by [@veri_ivanova](https://unsplash.com/@veri_ivanova) 在 unsplash

### 计时和剖析

在我们开始优化任何东西之前，我们首先需要找出代码中哪些部分实际上拖慢了整个程序。有时程序的瓶颈可能很明显，但如果你不知道它在哪里，那么你可以尝试以下选项来找出：

*注意：这是我将用于演示目的的程序，它计算 *`*e*`* 的 *`*X*`* 次方（取自 Python 文档）：*

### 最懒的“剖析”

首先，最简单的、老实说非常懒的解决方案——Unix `time` 命令：

如果你只是想计时整个程序，这可能会有效，但通常这还不够……

### 最详细的剖析

另一端的选择是 `cProfile`，它会给你*过多*的信息：

在这里，我们使用了 `cProfile` 模块和 `time` 参数来运行测试脚本，因此行按内部时间（`cumtime`）排序。这给我们提供了*大量*的信息，您上面看到的行只是实际输出的约 10%。从中，我们可以看到 `exp` 函数是罪魁祸首（*惊讶，惊讶*），现在我们可以更具体地进行计时和剖析……

### 计时特定函数

现在我们知道了要关注的地方，我们可能想要对慢函数进行计时，而不测量其余的代码。为此，我们可以使用简单的装饰器：

这个装饰器可以像这样应用于被测试的函数：

这会给我们如下输出：

一件需要考虑的事情是*我们实际上（想要）测量*的时间类型。`time`包提供了`time.perf_counter`和`time.process_time`。它们的区别在于，`perf_counter`返回的是绝对值，包括你 Python 程序未运行时的时间，因此可能会受到机器负载的影响。另一方面，`process_time`仅返回*用户时间*（不包括*系统时间*），这是你程序的运行时间。

### 加速

现在，来点有趣的。让我们让你的 Python 程序运行得更快。我（主要）不会给你展示一些黑客技巧、窍门和代码片段，这些东西能神奇地解决你的性能问题。这更多是关于一般的想法和策略，这些想法和策略在使用时可以对性能产生巨大影响，在某些情况下甚至可以提高30%的速度。

### 使用内置数据类型

这一点很明显。内置数据类型非常快，尤其是与我们的自定义类型如树或链表相比。主要是因为内置类型是用*C*实现的，而我们在用 Python 编写代码时无法匹敌这种速度。

### 使用 lru_cache 进行缓存/记忆化

我已经在之前的博客文章中[这里](https://martinheinz.dev/blog/4)展示过这个内容，但我认为用简单的例子重复一遍是值得的：

上面的函数使用`time.sleep`模拟了重计算。当第一次用参数`1`调用时，它会等待2秒钟，然后才返回结果。当再次调用时，结果已经被缓存，所以它跳过了函数体，立即返回结果。更多的*实际生活*例子请参见之前的博客文章[这里](https://martinheinz.dev/blog/4)。

### 使用局部变量

这与每个作用域中变量查找的速度有关。我说*每个作用域*是因为这不仅仅是关于局部变量与全局变量的使用。实际上，即使是在函数内部的局部变量（最快）、类级属性（例如`self.name` - 较慢）和全局变量（例如导入的函数如`time.time` - 最慢）之间，查找速度也存在差异。

你可以通过使用看似不必要（完全没用的）赋值来提高性能，例如：

### 使用函数

这可能看起来有些反直觉，因为调用函数会将更多的东西放到栈上，并且创建函数返回的开销，但这与前一点相关。如果你把整个代码放在一个文件里而不放入函数中，它会因为全局变量而变得更慢。因此，你可以通过将整个代码包裹在`main`函数中，并一次调用它来加快你的代码，如下所示：

### 不要访问属性

另一个可能会减慢程序速度的因素是*点操作符*（`.`），它在访问对象属性时使用。这个操作符触发了使用`__getattribute__`的字典查找，这会在你的代码中创建额外的开销。那么，我们如何实际避免（限制）使用它呢？

### 注意字符串

当在循环中使用 *取模* (`%s`) 或 `.format()` 时，字符串操作可能变得非常慢。我们还有什么更好的选择？根据最近的 [Raymond Hettinger 推文](https://twitter.com/raymondh/status/1205969258800275456)，我们应该使用的唯一方法是 *f-string*，它最具可读性、简洁且最快。因此，根据该推文，这是你可以使用的方法列表——从最快到最慢：

生成器本身并不比其他方法快，因为它们是为了实现惰性计算而设计的，从而节省内存而非时间。然而，节省的内存可以使你的程序实际运行得更快。如何做到这一点？如果你有一个大型数据集且不使用生成器（迭代器），数据可能会溢出 CPU 的 *L1 缓存*，这会显著减慢内存中的值查找速度。

说到性能，CPU 能将其处理的数据尽可能接近地保存在缓存中是非常重要的。你可以观看[Raymond Hettinger 的讲座](https://www.youtube.com/watch?v=OSGv2VnC0go&t=8m17s)，他在其中提到了这些问题。

### 结论

优化的首要原则是 *不要优化*。但如果你真的必须这样做，希望这些小贴士能帮助你。然而，优化代码时要谨慎，因为它可能使代码难以阅读，从而难以维护，这可能会抵消优化的好处。

*注：这最初发布在* [*martinheinz.dev*](https://martinheinz.dev/blog/13)

**简历： [马丁·海因茨](https://www.linkedin.com/in/heinz-martin/)** 是 IBM 的 DevOps 工程师。作为一名软件开发人员，马丁对计算机安全、隐私和加密充满热情，专注于云计算和无服务器计算，并始终准备迎接新的挑战。

[原文](https://towardsdatascience.com/making-python-programs-blazingly-fast-c1cd79bd1b32)。经授权转载。

**相关：**

+   [自动化你的 Python 项目的每个方面](/2020/09/automating-every-aspect-python-project.html)

+   [MIT 免费课程：Python 计算机科学与编程导论](/2020/09/free-mit-intro-computer-science-programming-python.html)

+   [用一行代码进行统计和可视化探索数据分析](/2020/09/statistical-visual-exploratory-data-analysis-one-line-code.html)

### 更多相关主题

+   [托马斯·米勒博士，探索西北大学的在线…](https://www.kdnuggets.com/2024/05/nwu/thomas-miller-phd-explores-northwestern-universitys-online-graduate-programs-in-data-science)

+   [通过《快速 Python 数据科学》提升你的 Python 技能！](https://www.kdnuggets.com/2022/06/manning-step-python-game-fast-python-data-science.html)

+   [如何让 Python 代码运行得非常快](https://www.kdnuggets.com/2021/06/make-python-code-run-incredibly-fast.html)

+   [预测的实现：Python 中线性回归的初学者指南](https://www.kdnuggets.com/2023/06/making-predictions-beginner-guide-linear-regression-python.html)

+   [BERT在稀疏性下能有多快？](https://www.kdnuggets.com/2022/04/fast-bert-go-sparsity.html)

+   [使用快速克里金（FKR）加速机器学习](https://www.kdnuggets.com/2022/06/vmc-speed-machine-learning-fast-kriging.html)
