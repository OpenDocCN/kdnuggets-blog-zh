# 从初学者到高手：为什么你的 Python 技能在数据科学中至关重要

> 原文：[`www.kdnuggets.com/novice-to-ninja-why-your-python-skills-matter-in-data-science`](https://www.kdnuggets.com/novice-to-ninja-why-your-python-skills-matter-in-data-science)

![从初学者到高手：为什么你的 Python 技能在数据科学中至关重要](img/a4cffc335f288d4f1586a1f59a275653.png)

由作者使用 DALL•E 3 创建的图像

# 介绍

我们知道编程是数据科学家需要具备的有用（必要？）技能。但是需要什么程度的编程技能呢？数据科学家是否应该目标是“足够好”，还是应当期望成为高级程序员？我们是否应该追求成为编程高手？

如果我们要探讨这个话题，我们应该首先了解初学者、中级和高级程序员的情况——或者至少了解他们的 *代码* 看起来如何。

在下面，你将找到 2 个编程任务，每个任务包括 3 个代码片段；分别展示了初学者、中级程序员和高级程序员完成这些任务的方法，并附有一些关于差异的解释。这将为我们讨论编程能力的重要性奠定基础。

请记住，这些都是为了模仿不同级别编程而构造的方法。所有的脚本都是功能性代码并能完成任务，但它们在优雅性、效率和 Python 规范性方面各有不同。

# 任务：求一个数的阶乘

首先，让我们考虑一个简单但可以用多种方式解决的任务：计算给定数的阶乘。我们将为假设中的初学者、中级和高级 Python 程序员实现这个任务，并比较代码中的差异。

## 初学者的方法

初学者可能使用直接的方法，通过 `for` 循环来计算阶乘。以下是他们可能的做法。

```py
n = int(input("Enter a number to find its factorial: "))
factorial = 1

if n < 0:
    print("Factorial does not exist for negative numbers")
elif n == 0:
    print("The factorial of 0 is 1")
else:
    for i in range(1, n + 1):
        factorial *= i
    print(f"The factorial of {n} is {factorial}")
```

## 中级程序员的方法

中级程序员可能使用函数来提高代码的重用性和可读性，并且还会使用 `math` 库进行基本检查。

```py
import math

def factorial(n):
    if n < 0:
        return "Factorial does not exist for negative numbers"
    elif n == 0:
        return 1
    else:
        return math.prod(range(1, n + 1))

n = int(input("Enter a number to find its factorial: "))
result = factorial(n)
print(f"The factorial of {n} is {result}")
```

## 高级程序员的方法

高级程序员可能会使用递归并添加类型提示以便于维护。他们还可能会利用 Python 的简洁且富有表现力的语法。

```py
from typing import Union

def factorial(n: int) -> Union[int, str]:
    return 1 if n == 0 else n * factorial(n - 1) if n > 0 else "Factorial does not exist for negative numbers"

n = int(input("Enter a number to find its factorial: "))
print(f"The factorial of {n} is {factorial(n)}")
```

## 总结

让我们看看不同级别之间的代码差异以及最突出的特点。

+   初学者：使用较长的整体代码，不使用函数或库，逻辑直接

+   中级程序员：使用函数以改善结构，使用 `math.prod` 计算乘积

+   高级程序员：使用递归以保持优雅，添加类型提示，并使用 Python 的条件表达式以提高简洁性

# 任务：生成斐波那契数

作为第二个例子，我们来考虑一个任务：找到前 *n* 个斐波那契数列。以下是不同级别的程序员可能如何解决这个任务。

## 初学者的方法

初学者可能使用基本的 `for` 循环和列表来收集斐波那契数。

```py
n = int(input("How many Fibonacci numbers to generate? "))
fibonacci_sequence = []

if n <= 0:
    print("Please enter a positive integer.")
elif n == 1:
    print([0])
else:
    fibonacci_sequence = [0, 1]
    for i in range(2, n):
        next_number = fibonacci_sequence[-1] + fibonacci_sequence[-2]
        fibonacci_sequence.append(next_number)
    print(fibonacci_sequence)
```

## 中级程序员的方法

中级程序员可能会使用列表推导式和`zip`函数实现更具 Python 风格的处理。

```py
n = int(input("How many Fibonacci numbers to generate? "))

if n <= 0:
    print("Please enter a positive integer.")
else:
    fibonacci_sequence = [0, 1]
    [fibonacci_sequence.append(fibonacci_sequence[-1] + fibonacci_sequence[-2]) for _ in range(n - 2)]
    print(fibonacci_sequence[:n]) 
```

## 专家的方法

专家可能会使用生成器实现更高效的内存管理，并利用 Python 的解包特性在一行中交换变量。

```py
def generate_fibonacci(n: int):
    a, b = 0, 1
    for _ in range(n):
        yield a
        a, b = b, a + b

n = int(input("How many Fibonacci numbers to generate? "))
if n <= 0:
    print("Please enter a positive integer.")
else:
    print(list(generate_fibonacci(n)))
```

## 总结

让我们看看主要的差异是什么，以及哪些编程差异将专业水平区分开来。

+   初学者：使用基本控制结构和列表，简单但略显冗长

+   中级：利用列表推导式和`zip`实现更具 Python 风格和简洁的解决方案

+   专家：使用生成器实现内存高效的解决方案，并使用解包进行优雅的变量交换

# “忍者”编码的好处

如果所有示例代码都能正常工作并最终完成任务，*为什么我们还要努力成为最优秀的程序员？* 很好的问题！

成为熟练的程序员不仅仅是让代码工作。以下是为什么努力成为更好的程序员是有益的几个原因：

## 1\. 效率

+   时间：编写更高效的代码意味着任务完成得更快，这对程序员和使用软件的任何人都是有益的

+   资源利用：高效的代码使用更少的 CPU 和内存，这对在有限资源或大规模运行的应用程序至关重要

## 2\. 可读性和可维护性

+   合作：代码通常由团队编写和维护。干净、结构良好且注释充分的代码更容易让其他人理解和合作

+   长期性：随着项目的发展或演变，可维护的代码更容易扩展、调试和重构，从长远来看节省时间和精力

## 3\. 可重用性

+   模块化：编写解决问题效果好的函数或模块意味着你可以轻松地在其他项目或上下文中重用这些代码

+   社区贡献：高质量的代码可以开源，并惠及更广泛的开发者社区

## 4\. 稳健性和可靠性

+   错误处理：高级程序员通常编写不仅能解决问题而且能优雅地处理错误的代码，使软件更加可靠

+   测试：了解如何编写可测试的代码和实际的测试，确保代码在各种场景中按预期工作

## 5\. 技能认可

+   职业发展：被认定为熟练的程序员可以带来晋升、工作机会和更高的薪水

+   个人满足感：知道自己能够编写高质量代码带来成就感和自豪感

## 6\. 适应性

+   新技术：扎实的基础技能使得适应新语言、库或范式变得更加容易

+   问题解决：对编程概念的深入理解增强了你创造性和有效性地解决问题的能力

## 7\. 成本效益

+   较少的调试：编写良好的代码通常更不容易出错，从而减少调试所花费的时间和资源

+   可扩展性：优秀的代码可以更容易地扩展或缩减，从长远来看更具成本效益

所以，尽管完成工作确实很重要，但完成工作的方式可以对个人发展、团队和组织产生广泛的影响。我们都应该努力成为最好的程序员，这同样适用于数据科学家。

[](https://www.linkedin.com/in/mattmayo13/)****[Matthew Mayo](https://www.kdnuggets.com/wp-content/uploads/./profile-pic.jpg)**** ([**@mattmayo13**](https://twitter.com/mattmayo13)) 拥有计算机科学硕士学位和数据挖掘研究生文凭。作为 [KDnuggets](https://www.kdnuggets.com/) 和 [Statology](https://www.statology.org/) 的主编，以及 [Machine Learning Mastery](https://machinelearningmastery.com/) 的特约编辑，Matthew 旨在使复杂的数据科学概念变得易于理解。他的专业兴趣包括自然语言处理、语言模型、机器学习算法以及探索新兴的 AI。他致力于在数据科学社区中普及知识。Matthew 从 6 岁起就开始编程。

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 方面

* * *

### 更多相关话题

+   [什么是数据血缘，为什么它很重要？](https://www.kdnuggets.com/what-is-data-lineage-and-why-does-it-matter)

+   [为何谦逊会提升你的数据科学技能](https://www.kdnuggets.com/2022/01/humbling-improve-data-science-skills.html)

+   [避免这 5 个每个 AI 新手常犯的错误](https://www.kdnuggets.com/avoid-these-5-common-mistakes-every-novice-in-ai-makes)

+   [顶级数据科学项目来提升你的技能](https://www.kdnuggets.com/2022/04/top-data-science-projects-build-skills.html)

+   [运用你的数据科学技能创建 5 种收入来源](https://www.kdnuggets.com/2023/03/data-science-skills-create-5-streams-income.html)

+   [如何使用 ChatGPT 提升你的数据科学技能](https://www.kdnuggets.com/2023/03/chatgpt-improve-data-science-skills.html)
