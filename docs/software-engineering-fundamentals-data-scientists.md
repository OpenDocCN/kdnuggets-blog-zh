# 数据科学家的软件工程基础

> 原文：[https://www.kdnuggets.com/2020/06/software-engineering-fundamentals-data-scientists.html](https://www.kdnuggets.com/2020/06/software-engineering-fundamentals-data-scientists.html)

[评论](#comments)

![](../Images/b7d920f619a83d06ea12e871e38b632c.png)

*来源： [Chris Ried](https://unsplash.com/photos/ieic5Tq8YMk) @ unsplash。*

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT工作

* * *

数据科学作为一个领域，自从它开始流行以来，就与其他学科发生了争论。统计学家抱怨从业者经常缺乏基本的统计知识，数学家则对在没有扎实理解所应用原理的情况下使用工具提出异议，而软件工程师指出数据科学家在编程时忽视基本原理。说实话，他们都有道理。在统计学和数学方面，确实需要对概率、代数和微积分等概念有扎实的理解。这些知识需要多深？这很大程度上取决于你的角色，但基本知识是不可妥协的。在编程方面也是类似的；如果你的角色涉及编写生产代码，那么你至少需要了解软件工程的基本知识。为什么？原因有很多，但我认为可以根据五个原则总结如下：

+   **代码的完整性**，包括代码编写质量、对错误的韧性、异常捕获、测试以及其他人的审查。

+   **代码的可解释性**，需附有适当的文档。

+   **代码的速度**，以便在实时环境中运行。

+   **脚本和对象的模块化**以便重用，避免重复，并在你的代码类中提高效率。

+   **与团队的慷慨**，让他们尽快审查你的代码，并在未来理解你编写的任何代码。

根据这些要点，在这个故事中我们将看到一些我认为最有用的基础知识，作为一个天生不是程序员的人，我从完全不同的背景进入这个领域。这些知识帮助我编写更好的生产代码，为我节省了时间，并使我的同事在实现我的脚本时更轻松。

### 编写干净代码的重要性

![](../Images/fe1216b521d448a294432b1746174096.png)

*来源：[Oliver Hale](https://unsplash.com/photos/oTvU7Zmteic) @ unsplash.*

从理论上讲，我们将在这个故事中讨论的几乎所有内容都可以被视为编写更干净代码的工具或技巧。然而，在本节中，我们将重点关注“干净”一词的严格定义。正如罗伯特·马丁在他的书籍[《代码整洁之道》](https://www.amazon.co.uk/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)中所说，即使糟糕的代码也能正常运行，但如果代码不够干净，它可以让一个开发组织陷入困境。怎么做到呢？说实话，可能性有很多，但只要想象一下审查写得糟糕的代码所浪费的时间，或者在新角色中发现你需要处理一些几乎无法辨认的旧代码，都会令人感到沮丧。更糟的是，想象一下某个功能失效，导致产品特性停止工作，而在你之前写下这些脏代码的人已经不在公司了。

这些都是相对常见的情况，但我们要少一点戏剧化；谁没有写过一些代码，然后因为更紧急的任务将它搁置一段时间，再回来时却不记得它是如何工作的呢？我知道这曾经发生在我身上。

这些都是让我们多花些心思编写更好代码的有效理由。因此，让我们从基础开始，了解一些编写更清洁脚本的技巧：

+   **在你的代码中使用描述性的名称**。我从大学学习Java课程时学到的一个概念就是：让你的代码具有*助记性*。助记性指的是一种帮助记忆的系统，比如字母、思想或关联的模式。也就是说，编写自解释的名称。

+   尽可能**尝试隐含类型**。例如，对于返回布尔对象的函数，你可以用 is_ 或 has 前缀它。

+   **避免缩写，尤其是单个字母。**

+   另一方面，**避免长名称和长行**。编写长名称并不意味着更具描述性，在行长度方面，[PEP 8 Python代码风格指南](https://www.python.org/dev/peps/pep-0008/)建议每行长度最多为约79个字符。

+   **不要为了保持一致性而牺牲清晰性**。例如，如果你有表示员工的对象和一个包含所有员工的列表，employee_list 和 employee_1 比 employees 和 employee_1 更加清晰。

+   关于空行和缩进，**使你的代码更易读**，通过用空行分隔不同部分并使用一致的缩进来实现。

### 编写模块化代码的重要性

![](../Images/13e65db455b071d50666ec8f4d63901a.png)

*来源：[Sharon McCutcheon](https://www.pexels.com/photo/art-materials-art-supplies-blocks-blur-1148496/) @ pexels.*

我认为这一点对于数据科学家和数据分析师来说是最重要的，并且常常与软件工程师讨论，因为我们习惯于在 Jupyter Notebooks 等工具中编码。这些工具非常适合探索性数据分析，但不适合编写生产代码。事实上，Python 本质上是一种面向对象的编程语言，讨论其详细含义超出了本讨论范围。但简而言之，与程序化编程不同，后者是编写一系列指令让脚本执行，面向对象编程则是构建具有自身特性和操作的模块。以下是一个例子：

![](../Images/a8ede08cba7ff915b06d51bb1e8a7d6e.png)

*来源：由作者创建的图像。*

实际上，这些特性称为属性，操作称为方法。在上面的例子中，计算机和打印机将是独立的类。一个类是一个包含所有该特定类型对象的属性和方法的蓝图。即，我们创建的所有计算机和打印机都将共享相同的属性和方法。这个想法的概念叫做封装。封装意味着你可以将函数和数据组合成一个单一的实体或模块。当你将程序拆分成模块时，不同的模块无需了解如何实现某些功能，只要它们不负责执行这些功能。为什么这有用？不仅是为了代码的可重用性，避免重复，并提高代码类的效率，还使调试变得更容易。

再次强调，如果你只是在 Jupyter Notebook 中进行探索性数据分析，这可能并不重要，但如果你在编写将成为实时环境的一部分的脚本，特别是随着应用程序的增长，将代码拆分成不同的模块是有意义的。在将程序的所有部分合并之前完善每个部分，不仅使在其他程序中重用单个模块变得更容易，还通过能够准确定位错误来源来更容易修复问题。

编写模块化代码的一些进一步提示：

+   DRY：不要重复自己

+   使用函数不仅减少了重复，还通过描述性名称提高了可读性，使理解每个模块的功能变得更容易。

+   最小化实体的数量（函数、类、模块等）

+   单一职责原则：一个类应该有唯一的责任。这比预期的要难。

+   遵循开闭原则，即对象应该对扩展开放，对修改关闭。这个理念是编写代码时，使得你能在不改变现有代码的情况下添加新功能，避免在一个类的变更同时需要调整所有依赖类的情况。[应对这一挑战的方式有很多](https://stackify.com/solid-design-open-closed-principle/)，不过在Python中，使用继承是非常常见的。

+   尽量每个函数使用不超过三个参数。如果参数较多，可以考虑拆分它。对函数长度也有类似标准；理想情况下，一个函数应包含20到50行代码。如果超出这个范围，可能需要将其拆分为多个函数。

+   同样要注意类的长度。如果一个类有超过300行代码，那么它可能需要被拆分成更小的类。

如果你已经在使用Python，但对面向对象编程没有或只有很少的了解，我强烈推荐这两个免费的课程：

+   [Python中的面向对象编程](https://www.datacamp.com/courses/object-oriented-programming-in-python) 见 Datacamp

+   [Python中的面向对象编程（OOP）简介](https://realpython.com/courses/intro-object-oriented-programming-oop-python/) 见 [https://realpython.com/](https://realpython.com/)

### 重构的重要性

![](../Images/bae4178d7b038c4cc46b8108bbaf8fcc.png)

*来源： [RyanMcGuire](https://pixabay.com/photos/car-repair-car-workshop-repair-shop-362150/) @ pixabay。*

维基百科对重构的定义如下：

> *在计算机编程和软件设计中，代码重构是指在不改变其外部行为的情况下重构现有计算机代码的过程。重构旨在改善软件的设计、结构和/或实现，同时保持其功能。重构的潜在优点包括提高代码可读性和减少复杂性；这些都可以提高源代码的可维护性，并创建一个更简单、更清晰或更具表现力的内部架构或对象模型，从而改善可扩展性。*

我认为定义本身就很清楚，但除此之外，我们可以补充一点：重构给我们一个机会，在代码运行后清理和模块化我们的代码。它也给了我们一个提高代码效率的机会。而我到目前为止学到的是，当一个软件工程师谈到高效代码时，他们通常指的是以下其中之一：

1.  减少运行时间

1.  减少内存占用

让我们简要讨论这两个要点……

根据我的经验，**减少代码运行时间** 是你在编写越来越多生产代码的过程中逐渐学会的。当你在 Jupyter Notebook 中进行一些分析时，不论计算这些配对距离花费你两分钟、五分钟还是十分钟都无所谓。你可以让它运行，回答一些 Slack 消息，去洗手间，倒杯咖啡，然后回来查看你的代码是否完成。然而，当有用户在等待时，你不能让他们一直等待你的代码编译，对吧？

在 Python 中，有几种方法可以提高性能。我们快速回顾一下其中一些：

**使用向量操作来加快计算速度**。例如，当检查一个数组的元素是否在另一个数组内时，不必编写 for 循环，你可以使用 NumPy 的 *intersect1d*。你还可以使用向量根据条件搜索元素以执行加法或类似操作。我们来看看一个快速示例，当我们需要 **遍历一个数字列表并根据条件执行操作** 时：

不要使用这种方式：

```py
# random array with 10 million points
a = np.random.normal(500, 30, 10000000)

# iterating and checking for values < 500
t0 = time.time()
total = 0
for each in a:
    if each < 500:
    total += each
t1 = time.time()
print(t1-t0) 

```

时间：3.6942789554595947 秒

```py
# same operation only using numpy
t0 = time.time()
total = a[a<500].sum()
t1 = time.time()
print(t1-t0) 

```

时间：0.06348109245300293 秒

比之前快了 58 倍以上！

我知道 Pandas Dataframes 使用起来非常方便，我们所有 Python 爱好者都喜欢它们。然而，在编写生产代码时，最好还是避免使用它们。我们用 Pandas 执行的大部分操作也可以用 Numpy 完成。我们来看一些其他的例子：

+   **根据条件对矩阵的行进行求和**

```py
# random 2d array with 1m rows and 20 columnn
# we’ll use the same in following examples
a = np.random.random(size=(1000000,20))

# sum all values greater than 0.30
(a * (a>0.30)).sum(axis=1)

```

在上述代码中，乘以布尔数组之所以有效，是因为 True 对应 1，False 对应 0。

+   **根据某些条件添加列**

```py
# obtain the number of columns in the matrix to be used as index of the new one to be placed at the end
new_index = a.shape[1]

# set the new column using the array created in the previous example
a = np.insert(a, new_index, (a * (a>0.30)).sum(axis=1), axis=1)

# check new shape of a
a.shape

```

打印：（1000000，21）| 新列已添加。

+   **根据多个条件筛选表格**

```py
# filter if the new last column is greater than 10 and first column is less than 0.30
b = a[(a[:,0]<0.30)&(a[:,-1]>10)]

b.shape

```

打印：（55183，21）| 55183 行符合条件。

+   **如果满足条件则替换元素**

```py
# change to 100 all values less than 0.30
a[a<0.3] = 100

```

除了上述代码，减少运行时间的另一个好方法是 **并行化**。并行化意味着编写一个脚本来并行处理数据，使用机器上的多个或所有可用处理器。这为何能显著提升速度？因为大多数情况下，我们的脚本是串行计算数据：它们解决一个问题，然后是下一个问题，如此循环。当我们用 Python 编写代码时，通常就是这样。如果我们希望利用并行化，就必须明确这样做。我将很快写一个关于这个主题的独立故事，但如果你迫切想了解更多，在所有可用的并行化库中，到目前为止我最喜欢的是：

+   [Multiprocessing](https://docs.python.org/3/library/multiprocessing.html)

+   [Numba](https://numba.pydata.org/numba-doc/latest/user/5minguide.html)

关于 **减少内存占用**，在 Python 中减少内存使用是困难的，因为 [Python 实际上不会将内存释放回操作系统](http://effbot.org/pyfaq/why-doesnt-python-release-the-memory-when-i-delete-a-large-object.htm)。如果你删除对象，那么内存会被新 Python 对象使用，但不会被释放回系统。此外，正如前面提到的，Pandas 是进行探索性数据分析的好工具，但除了在生产代码中较慢之外，它在内存方面也相当昂贵。然而，我们可以做一些事情来控制内存使用：

+   首先：如果可能，**使用 NumPy 数组而非 Pandas**。即使是字典，如果可能的话，也会比 Dataframe 占用更少的内存。

+   **减少 Pandas Dataframe 的数量**：在修改数据框时，尝试使用参数 inplace=True 修改数据框本身，而不是创建一个新对象，以避免生成副本。

+   **清除历史记录**：每次对数据框进行更改（例如，df + 2）时，Python 会在内存中保存该对象的副本。你可以使用 %reset Out 清除这些历史记录。

+   **注意数据类型**：与数字相比，object 和 string 数据类型在内存方面的开销要大得多。这就是为什么使用 df.info() 检查数据框的数据类型并在可能的情况下使用 df[‘column’] = df[‘columns’].astype(type) 进行类型转换总是很有用的原因。

+   **使用稀疏矩阵**：如果你有一个包含大量空值或空单元格的矩阵，使用稀疏矩阵会更方便，它通常在内存中占用的空间要少得多。

你可以使用 *scipy.sparse.csr_matrix(df.values)* 来实现这一点。

+   **使用生成器而非对象**：生成器允许你声明一个像迭代器一样行为的函数，但使用 *yield* 关键字而不是 *return*。生成器不会创建一个包含所有计算的新对象（即列表或 NumPy 数组），而是生成一个存储在内存中的单一值，并且只有在你请求时才会更新。这被称为惰性求值。更多关于生成器的信息可以在 [Abhinav Sagar](https://towardsdatascience.com/@abhinav.sagar) 在 Towards Data Science 上的精彩文章中找到。

### 测试的重要性

![](../Images/b0fac3f71716a374de4c187c387c41f9.png)

*来源： [Pixabay](https://www.pexels.com/photo/red-and-yellow-hatchback-axa-crash-tests-163016/) 在 @ pexels。*

数据科学中的测试是必要的。其他软件相关领域通常抱怨数据科学家的代码缺乏测试。在其他算法或脚本中，如果出现错误，程序可能会停止工作，但在数据科学中，这更危险，因为程序可能会运行，但由于值编码不正确、特征使用不当或数据破坏了模型实际依赖的假设，最终可能导致错误的洞察和建议。

当我们谈到测试时，有两个主要概念值得讨论：

+   单元测试

+   测试驱动开发

让我们先从前者开始。**单元测试**之所以称为单元测试，是因为它们覆盖了代码的一个小单元。目标是验证代码的每个单独部分是否按设计执行。在面向对象编程语言中，例如 Python，单元测试也可以设计用于评估整个类，但也可以是单个方法或函数。

单元测试可以从头编写。实际上，让我们这样做，以便更好地理解单元测试实际是如何工作的：

假设我有以下函数：

```py
def my_func(a,b):

   c=(a+b)/2*1.5

   return c

```

我想测试以下输入是否返回预期的输出：

+   4 和 2 返回 4.5

+   5 和 5 返回 5.5

+   4 和 8 返回 9.0

我们可以完全写出这样的代码：

```py
def test_func(function, output):
    out = function
    if output == out:
        print(‘Worked as expected!’)
    else:
        print(‘Error! Expected {} output was {}’.format(output,out))

```

然后简单地测试我们的函数：

```py
test_func(my_func(4,2),4.5)

```

打印：*按预期工作！*

然而，当函数变得更加复杂时，尤其是当我们想一次测试多个函数，甚至是一个类时，这会变得更加棘手。一个优秀的单元测试工具是[pytest library](https://docs.pytest.org/en/latest/getting-started.html)。Pytest 要求你创建一个包含要测试的函数的 Python 脚本，并且还有另一组函数来断言输出。文件需要以“test”作为前缀保存，然后像运行其他 Python 脚本一样运行它。Pytest 最初是为了从命令行使用而开发的，但如果你仍处于项目的早期阶段，可以使用 Jupyter Notebook 中的 hacky 方法；你可以使用魔法命令*%%writefile* 创建并保存一个 .py 文件，然后直接从笔记本运行命令行语句来运行脚本。让我们来看一个示例：

```py
import pytest
%%writefile test_function.py

def my_func(a,b):
    c=(a+b)/2*1.5
    return c

def test_func_4_2():
    assert(my_func(4,2)==4.5)

def test_func_5_5():
    assert(my_func(5,5)==7.5)

def test_func_4_8():
    assert(my_func(4,8)==9.0)

```

然后运行脚本：

```py
!pytest test_function.py

```

如果一切按预期运行，你会看到这样的输出：

![](../Images/efa239201e3ca82744ad7f7d84db4362.png)

未来，我会写另一个故事来讨论更复杂的单元测试示例，以及如何测试整个类（如果这是你需要的）。但与此同时，这些示例应该足以帮助你入门并测试一些函数。请注意，在上面的示例中，我测试的是返回的确切数字，但你也可以测试数据框的形状、NumPy 数组的长度、返回对象的类型等。

我们在本章开始时提到的另一个重点是 **测试驱动开发（TDD）**。这种测试方法或方法论包括在开始开发之前编写单元测试。接下来，你将编写尽可能简单和/或快速的代码，以通过最初编写的测试，这将帮助你通过在编写代码之前专注于需求来确保质量。此外，它将强迫你保持代码简单、干净，并可测试，通过将其拆分为与最初编写的测试相符的小块代码。一旦你有了一段实际通过测试的代码，你就可以专注于重构，以提高代码质量或实现更多功能。

![](../Images/d9994ecd4b3ba3238aa4cbe5f036c495.png)

*来源：[https://me.me/](https://me.me/)*

TDD 的一个主要好处是，如果将来需要对代码进行更改而你不再参与该项目，可能是你转到另一家公司或只是度假，了解最初编写的测试将帮助任何接手代码的人确保更改后不会破坏任何东西。

一些值得考虑的进一步点：

+   笔记本：理想用于探索，但不适合 TDD。

+   乒乓 TDD：一个人编写测试，另一个人编写代码。

+   为你的测试设置性能和输出指标。

### 代码审查的重要性

![](../Images/dd9c380898050c5dcffa3a5a687649f4.png)

*来源： [Charles Deluvio](https://unsplash.com/photos/Lks7vei-eAg) @ unsplash。*

代码审查对团队中的每个人都有益，以促进最佳编程实践并为生产准备代码。代码审查的主要目标是捕捉错误。然而，它们也有助于提高可读性，并检查团队中的标准是否得到遵守，以便没有肮脏或慢速的代码进入生产。除此之外，代码审查还非常适合分享知识，因为团队成员可以阅读来自不同背景和风格的人的代码片段。

现如今，用于代码审查的优秀工具是 GitHub 平台的拉取请求。拉取请求是将代码或全新脚本集成到某个代码环境中的请求。之所以称之为拉取请求，是因为提交时意味着要求某人将你编写的代码拉入仓库。

从 [GitHub 的文档](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests) 中，我们可以看到对拉取请求的定义：

> *拉取请求让你能够告诉他人你已推送到 GitHub 上一个仓库分支的更改。一旦打开拉取请求，你可以与协作者讨论和审查潜在更改，并在更改合并到主分支之前添加后续提交。*

拉取请求本身就是一门艺术，如果你有兴趣了解更多，可以参考 [Hugo Dias](https://medium.com/@hugooodias) 的 [这篇文章](https://medium.com/@hugooodias/the-anatomy-of-a-perfect-pull-request-567382bb6067)，标题为《完美拉取请求的解剖》。不过，在审查代码时，你可以问自己以下几个问题：

+   **代码是否干净且模块化？** 查找重复、空白、可读性和模块化。

+   **代码是否高效？** 查看循环、对象、函数结构，是否可以使用多进程？

+   **文档是否有效？** 查找内联注释、[docstrings](https://en.wikipedia.org/wiki/Docstring) 和 readme 文件。

+   **代码是否经过测试？** 查找单元测试。

+   **[**日志记录**](https://www.quora.com/What-is-Logging-in-programming) **是否足够好？** 查找日志消息的清晰度和适当频率。

[原文](https://towardsdatascience.com/software-engineering-fundamentals-for-data-scientists-6c95316d6cc4)。经允许转载。

**相关内容：**

+   [机器学习部署的软件接口](https://www.kdnuggets.com/2020/03/software-interfaces-machine-learning-deployment.html)

+   [如何对机器学习代码进行单元测试](https://www.kdnuggets.com/2017/11/unit-test-machine-learning-code.html)

+   [数据科学家的编码习惯](https://www.kdnuggets.com/2020/05/coding-habits-data-scientists.html)

### 更多相关话题

+   [5 门免费在线课程学习数据工程基础](https://www.kdnuggets.com/5-free-online-courses-to-learn-data-engineering-fundamentals)

+   [数据科学家和分析师的统计学基础](https://www.kdnuggets.com/2023/08/fundamentals-statistics-data-scientists-analysts.html)

+   [软件开发者与软件工程师](https://www.kdnuggets.com/2022/05/software-developer-software-engineer.html)

+   [KDnuggets 新闻 2022 年 3 月 16 日：学习数据科学基础 & 5…](https://www.kdnuggets.com/2022/n11.html)

+   [学习数据科学基础需要多长时间？](https://www.kdnuggets.com/2022/03/long-take-learn-data-science-fundamentals.html)

+   [5 门免费在线课程学习数据科学基础](https://www.kdnuggets.com/5-free-online-courses-to-learn-data-science-fundamentals)
