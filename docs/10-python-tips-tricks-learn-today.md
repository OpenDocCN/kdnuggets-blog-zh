# 今天你应该学习的10个Python技巧和窍门

> 原文：[https://www.kdnuggets.com/2020/01/10-python-tips-tricks-learn-today.html](https://www.kdnuggets.com/2020/01/10-python-tips-tricks-learn-today.html)

[comments](#comments)![Figure](../Images/1ef0f4e1d43dc885d3291909d85ff6ef.png)

照片来自[Rifqi Ali Ridho](https://unsplash.com/@rifqialiridho?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/paintbrush?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

根据Stack Overflow的数据，Python是增长最快的编程语言。最新的[Forbes报告](https://www.whatech.com/development/press-release/442278-why-developers-vote-python-as-the-best-application-programming-language)指出，Python去年增长了456%。Netflix使用Python，IBM使用Python，还有数百家公司也使用Python。不要忘了Dropbox，Dropbox也是用Python创建的。根据[DICE的研究](https://insights.dice.com/2016/02/01/whats-hot-and-not-in-tech-skills/)，Python也是最热门的技能之一，并且是全球最受欢迎的编程语言之一，基于[编程语言流行指数](https://pypl.github.io/PYPL.html)。

与其他编程语言相比，Python提供的一些优势包括：

1.  与主要平台和操作系统兼容

1.  许多开源框架和工具

1.  可读性和可维护性高的代码

1.  强大的标准库

1.  标准的测试驱动开发

### Python技巧和窍门

在这篇文章中，我将介绍10个有用的代码技巧和窍门，帮助你完成日常任务。所以，不再耽搁，让我们开始吧。

### 1\. 连接字符串

当你需要连接一个字符串列表时，可以通过*for循环*逐个添加每个元素。然而，这种方法效率非常低，尤其是当列表很长时。在Python中，字符串是不可变的，因此每对连接都需要将左右字符串复制到新的字符串中。

更好的方法是使用`join()`函数，如下所示：

```py
characters = ['p', 'y', 't', 'h', 'o', 'n']
word = "".join(characters)
print(word) # python
```

### 2\. 使用列表推导式

列表推导式用于从其他可迭代对象创建新列表。由于列表推导式返回列表，它们包含一个括号，括号内是对每个元素执行的表达式，配合`for`循环迭代每个元素。列表推导式更快，因为它经过优化，Python解释器能在循环时识别出可预测的模式。

作为一个示例，让我们使用列表推导式找出前五个整数的平方。

```py
m = [x ** 2 for x in range(5)]
print(m) # [0, 1, 4, 9, 16]
```

现在我们来使用列表推导式找出两个列表中的共同数字

```py
list_a = [1, 2, 3, 4]
list_b = [2, 3, 4, 5]
common_num = [a for a in list_a for b in list_b if a == b]
print(common_num) # [2, 3, 4]
```

### 3\. 使用`enumerate()`迭代

`enumerate()`方法为可迭代对象添加计数器，并以枚举对象的形式返回。

让我们解决一个经典的编码面试题，广为人知的Fizz Buzz问题。

> 编写一个程序，打印列表中的数字，对于‘3’的倍数，打印“fizz”代替数字，对于‘5’的倍数，打印“buzz”，对于3和5的倍数，打印“fizzbuzz”。

```py
numbers = [30, 42, 28, 50, 15]
for i, num in enumerate(numbers):
    if num % 3 == 0 and num % 5 == 0:
       numbers[i] = 'fizzbuzz'
    elif num % 3 == 0:
       numbers[i] = 'fizz'
    elif num % 5 == 0:
       numbers[i] = 'buzz'
print(numbers) # ['fizzbuzz', 'fizz', 28, 'buzz', 'fizzbuzz']
```

### 4\. 在处理列表时使用ZIP

假设你被要求将几个相同长度的列表合并并打印结果？这里有一个利用`zip()`的通用方法来获取所需结果，如下面的代码所示：

```py
countries = ['France', 'Germany', 'Canada']
capitals = ['Paris', 'Berlin', 'Ottawa']
for country, capital in zip(countries,capitals):
    print(country, capital) # France Paris 
                              Germany Berlin
                              Canada Ottawa
```

### 5\. 使用itertools

Python的`itertools`模块是处理迭代器的工具集合。`itertools`有多个工具用于生成可迭代的输入数据序列。这里我将以`itertools.combinations()`为例。`itertools.combinations()`用于构建组合。这些组合也是输入值的所有可能分组。

让我们用一个实际的例子来说明上述观点。

> 假设有四支队伍参加比赛。在联赛阶段，每支队伍都要与其他每支队伍比赛。你的任务是生成所有可能的比赛对阵。

让我们看看下面的代码：

```py
import itertools
friends = ['Team 1', 'Team 2', 'Team 3', 'Team 4']
list(itertools.combinations(friends, r=2)) # [('Team 1', 'Team 2'),      ('Team 1', 'Team 3'),  ('Team 1', 'Team 4'),  ('Team 2', 'Team 3'),  ('Team 2', 'Team 4'),  ('Team 3', 'Team 4')]
```

重要的一点是，值的顺序并不重要。因为`('Team 1', 'Team 2')`和`('Team 2', 'Team 1')`表示的是相同的组合，输出列表中只会包含其中一个。类似地，我们还可以使用`itertools.permutations()`以及模块中的其他函数。作为更完整的参考，请查看[这个精彩的教程](https://medium.com/@jasonrigden/a-guide-to-python-itertools-82e5a306cdf8)。

### 6\. 使用Python集合

Python集合是容器数据类型，包括列表、集合、元组和字典。collections模块提供了高性能的数据类型，可以增强你的代码，使其更加简洁易懂。collections模块提供了很多函数。为了演示，我将使用`Counter()`函数。

`Counter()`函数接受一个可迭代对象，如列表或元组，并返回一个Counter字典。字典的键将是可迭代对象中存在的唯一元素，而每个键的值将是可迭代对象中该元素的计数。

要创建一个`counter`对象，将一个可迭代对象（列表）传递给`Counter()`函数，如下面的代码所示。

```py
from collections import Countercount = Counter(['a','b','c','d','b','c','d','b'])
print(count) # Counter({'b': 3, 'c': 2, 'd': 2, 'a': 1})
```

作为更完整的参考，请查看我的[python collections tutorial](https://towardsdatascience.com/a-hands-on-guide-to-python-collections-aa350cb399e3)。

### 7\. 将两个列表转换为字典

假设我们有两个列表，一个列表包含学生的名字，另一个列表包含他们的分数。让我们看看如何将这两个列表转换为一个字典。使用zip函数，可以通过下面的代码完成：

```py
students = ["Peter", "Julia", "Alex"]
marks = [84, 65, 77]
dictionary = dict(zip(students, marks))
print(dictionary) # {'Peter': 84, 'Julia': 65, 'Alex': 77}
```

### 8\. 使用Python生成器

生成器函数允许你声明一个像迭代器一样工作的函数。它们允许程序员以一种快速、简单、清晰的方式创建迭代器。让我们举一个例子来解释这个概念。

> 假设你需要计算前 100000000 个完美平方数的和，从 1 开始。

看起来很简单，对吧？这可以使用列表推导式轻松完成，但问题是输入数据量很大。例如，我们来看下面的代码：

```py
t1 = time.clock()
sum([i * i for i in range(1, 100000000)])
t2 = time.clock()
time_diff = t2 - t1
print(f"It took {time_diff} Secs to execute this method") # It took 13.197494000000006 Secs to execute this method
```

当我们增加需要求和的完美数字时，我们会发现由于计算时间较长，这种方法不可行。此时，Python 生成器可以派上用场。通过将方括号替换为圆括号，我们将列表推导式改为生成器表达式。现在让我们计算所需的时间：

```py
t1 = time.clock()
sum((i * i for i in range(1, 100000000)))
t2 = time.clock()
time_diff = t2 - t1
print(f"It took {time_diff} Secs to execute this method") # It took 9.53867000000001 Secs to execute this method
```

正如我们所见，所需的时间已大大减少。对于更大的输入，这种效果会更加明显。

欲了解更全面的参考资料，请查看我的文章 [使用生成器减少内存使用并加快 Python 代码速度](https://towardsdatascience.com/reduce-memory-usage-and-make-your-python-code-faster-using-generators-bd79dbfeb4c)。

### 9\. 从函数返回多个值

Python 能够从函数调用中返回多个值，这是许多其他流行编程语言所缺乏的。在这种情况下，返回值应该是一个用逗号分隔的值列表，Python 会构建一个 *元组* 并将其返回给调用者。以下代码为示例：

```py

def multiplication_division(num1, num2):
return num1*num2, num1/num2
product, division = multiplication_division(15, 3)
print("Product=", product, "Quotient =", division) # Product= 45 Quotient = 5.0
```

### 10\. 使用 `sorted()` 函数

在 Python 中，使用内置方法 `sorted()` 排序任何序列非常简单，`sorted()` 会为你完成所有的繁重工作。`sorted()` 可以对任何序列（列表、元组）进行排序，并始终返回一个元素按排序顺序排列的列表。我们来看一个示例，将数字列表按升序排序。

```py
sorted([3,5,2,1,4]) # [1, 2, 3, 4, 5]
```

以另一个示例为例，我们将一个字符串列表按降序排序。

```py
sorted(['france', 'germany', 'canada', 'india', 'china'], reverse=True) # ['india', 'germany', 'france', 'china', 'canada']
```

### 结论

在这篇文章中，我介绍了 10 个 Python 技巧和窍门，可以作为你日常工作的参考。希望你喜欢这篇文章。敬请关注我的下一篇文章，“加速 Python 代码的技巧与窍门”。

### 参考文献/进一步阅读

[30-seconds/30-seconds-of-python](https://github.com/30-seconds/30-seconds-of-python)

精选有用的 Python 代码片段，你可以在 30 秒或更短时间内理解。欢迎贡献…

[50+ Python 3 技巧与窍门](https://medium.com/towards-artificial-intelligence/50-python-3-tips-tricks-e5dbe05212d7)

这些 Python 精华将使你的代码变得美观而优雅

[Python Itertools 指南](https://medium.com/@jasonrigden/a-guide-to-python-itertools-82e5a306cdf8)

这些可迭代对象比你想象的要强大得多。

### 联系方式

如果你想保持更新我的最新文章和项目，请 [在 Medium 上关注我](https://medium.com/@abhinav.sagar)。以下是我的一些联系方式：

+   [个人网站](https://abhinavsagar.github.io/)

+   [Linkedin](https://in.linkedin.com/in/abhinavsagar4)

+   [Medium Profile](https://medium.com/@abhinav.sagar)

+   [GitHub](https://github.com/abhinavsagar)

+   [Kaggle](https://www.kaggle.com/abhinavsagar)

祝阅读愉快，学习愉快，编码愉快！

**个人简介：[Abhinav Sagar](https://www.linkedin.com/in/abhinavsagar4)** 是VIT Vellore的四年级本科生。他对数据科学、机器学习及其在实际问题中的应用感兴趣。

[原创](https://towardsdatascience.com/10-python-tips-and-tricks-you-should-learn-today-a05c23a39dc5)。经授权转载。

**相关内容：**

+   [Python列表和列表操作](/2019/11/python-lists-list-manipulation.html)

+   [Python字典和字典方法](/2019/12/python-dictionary-methods.html)

+   [为什么Python是数据科学中最受欢迎的语言之一？](/2020/01/python-preferred-languages-data-science.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行IT管理

* * *

### 更多相关话题

+   [每个数据科学家都应该了解的三个R库（即使你使用Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [停止学习数据科学以寻找目标，并寻找目标以……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [是什么让Python成为初创企业理想的编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [分析$9B AI失败案例](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [学习数据科学统计的最佳资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的5个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)
