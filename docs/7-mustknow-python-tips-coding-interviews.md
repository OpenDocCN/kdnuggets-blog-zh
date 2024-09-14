# 7 个编程面试必知的 Python 技巧

> 原文：[https://www.kdnuggets.com/2023/03/7-mustknow-python-tips-coding-interviews.html](https://www.kdnuggets.com/2023/03/7-mustknow-python-tips-coding-interviews.html)

![7 个编程面试必知的 Python 技巧](../Images/2e1736bec8cafa934e833c784efe7d3f.png)

图片由作者提供

几乎所有的数据科学职位面试都包括多达两轮的编程测试，以考察候选人的问题解决能力。因此，即使你有令人印象深刻的项目组合，你仍需通过初始编程面试轮次才能进一步晋级。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

选择像 Python 这样的语言来应对编程面试，可以非常有帮助，因为它比 C++ 和 Java 等语言更容易学习和使用。在本指南中，我们将介绍有用的 Python 面试技巧。

我们将讨论反转和排序数组、自定义数组排序、列表和字典推导式、解包可迭代对象等内容。让我们深入了解吧！

# 1\. 原地反转数组

在任何编程面试中，你都会遇到关于数组的问题。在 Python 中，列表提供了与数组相同的功能。你可以进行如下操作：

+   在常数时间内查找特定索引处的元素。

+   在列表末尾添加元素，并在常数时间内从列表末尾移除项目。

+   在 O(n) 时间内在特定索引处插入元素。

当你需要在原地反转列表而不创建新列表时，可以调用 `reverse()` 方法。在这里，我们初始化 `nums` 列表并调用 `reverse()` 方法。

```py
nums = [90,23,19,45,33,54]
nums.reverse()
print(nums)
```

我们看到原始列表已被反转：

```py
Output >> [54, 33, 45, 19, 23, 90]
```

# 2\. 排序数组并自定义排序

另一个常见的数组操作是排序。当你需要在原地排序列表时，可以调用列表对象的 `sort()` 方法。

在这里，`nums` 也是一个数字列表，调用 `sort()` 方法会在原地按升序对 `nums` 进行排序：

```py
nums = [23,67,12,78,94,113,47]
nums.sort()
print(nums)
```

```py
Output >> [12, 23, 47, 67, 78, 94, 113]
```

如图所示，`sort()` 方法默认按**升序**对列表进行排序。

要按降序排序列表，可以在 `sort()` 方法调用中将 `reverse` 设置为 `True`：

```py
nums.sort(reverse=True)
print(nums)
```

```py
Output >> [113, 94, 78, 67, 47, 23, 12]
```

**注意**：对于字符串列表（如 ["plums","cherries","grapes"]），默认排序为字母顺序。将 `reverse = True` 设置为反向字母顺序排序字符串列表。

## 使用 Lambda 自定义排序

有时候，你需要的排序方式不仅仅是简单的升序或降序。你可以通过将`key`参数设置为一个可调用对象来自定义`sort()`方法。

作为一个示例，让我们根据除以7的余数对`nums`列表进行排序。

```py
nums.sort(key=lambda num:num%7)
print(nums)
```

```py
Output >> [113, 78, 23, 94, 67, 47, 12]
```

为了检查输出是否正确，让我们创建一个余数列表`rem_list`：

```py
rem_list = [num%7 for num in nums]
print(rem_list)
```

```py
Output >> [1, 1, 2, 3, 4, 5, 5]
```

我们看到`rem_list`中1和5出现了两次。让我们解析一下这意味着什么：

+   13和78在除以7时都留下一个余数。但13在排序列表中排在78之前，因为在原始`nums`列表中它也排在78之前。

+   同样，47%7和12%7的结果都是5。47在排序列表中排在12之前，因为在原始列表中它也排在12之前。

+   因此，`sort()`方法执行稳定排序，即在原始列表中，如果两个或更多元素在给定排序标准下相等，它们的顺序会被保留。在此情况下，标准是`num%7`。

你还可以自定义字符串列表的排序。在这里，我们根据'p'的出现次数对`str_list`进行排序：

```py
str_list = ["puppet","trumpet","carpet","reset"]
str_list.sort(key=lambda x:x.count('p'))
print(str_list)
```

```py
Output >> ['reset', 'trumpet', 'carpet', 'puppet']
```

# 3\. 列表和字典推导式

推导式是Python的一项强大功能，允许你编写更具惯用性的代码。它们允许你从现有的可迭代对象创建新的可迭代对象，通常是*一种简洁的替代方案*来代替for循环。

## 列表推导示例

假设我们有`nums`列表。我们现在想要获取`nums`中所有能被3整除的数字列表。为此，我们可以使用形式为`[output for item in iterable if condition]`的列表推导表达式。

在这里，我们根据条件`num%3==0`过滤`nums`列表，以获得`div_by_3`列表：

```py
nums = [15,12,90,27,10,34,26,77]
div_by_3 = [num for num in nums if num%3==0]
print(div_by_3)
```

```py
Output >> [15, 12, 90, 27]
```

## 字典推导示例

字典推导式在你需要从现有的可迭代对象创建新的字典时非常有用，而不是使用for循环。

这些表达式通常是这样的形式：`{key:value for key in some_iterable}`。这意味着我们可以在遍历过程中访问键并创建值（从键开始）！

假设我们想创建一个包含1到10的数字作为键以及这些数字的平方作为值的Python字典。我们可以使用字典推导式来做到这一点：

```py
squares_dict = {i:i**2 for i in range(1,11)}
print(squares_dict)
```

我们看到我们有了数字和这些数字的平方作为键值对：

```py
Output >> 
{1: 1, 2: 4, 3: 9, 4: 16, 5: 25, 6: 36, 7: 49, 8: 64, 9: 81, 10: 100}
```

这是另一个例子。从`strings`列表中，我们构建了`str_len`字典，其中键是字符串，值是这些字符串的长度：

```py
strings = ["hello","coding","blue","work"]
str_len = {string:len(string) for string in strings}
print(str_len)
```

```py
Output >> {'hello': 5, 'coding': 6, 'blue': 4, 'work': 4}
```

# 4\. 解包可迭代对象

在Python中，你可以将可迭代对象解包到一个或多个变量中，具体取决于你想如何使用它们。这在你只需要使用元素的一个子集进行进一步处理时特别有用。

让我们创建一个列表`list1`：

```py
list1 = [i*2 for i in range(4)]
print(list1)
```

```py
Output >> [0, 2, 4, 6]
```

如果我们想使用`list1`中的所有四个元素，我们可以简单地将它们分配给四个不同的变量，如下所示：

```py
num1, num2, num3, num4 = list1

print(num1)
Output >> 0

print(num2)
Output >> 2

print(num3)
Output >> 4

print(num4)
Output >> 6
```

如果你想将列表中的前几个元素存储在一个变量中，比如`num1`，将其余元素（子列表）存储在另一个变量中，比如`num2`，你可以在变量名前使用*，这样它会捕捉`list1`中的剩余元素：

```py
num1, *num2 = list1

print(num1)
Output >> 0

print(num2)
Output >> [2, 4, 6]
```

同样，如果你想要列表中的第一个和最后一个元素，你可以像下面这样解包。元素2和4被包含在变量`num2`中：

```py
num1, *num2, num3 = list1

print(num1)
Output >> 0

print(num2)
Output >> [2, 4]

print(num3)
Output >> 6
```

# 5\. 用分隔符连接字符串列表

假设你有一个字符串列表，任务是用分隔符将它们连接成一个字符串。

你可以使用`join()`字符串方法，像这样：`separator.join(list)`将使用分隔符连接列表中的元素。

这里有几个例子。考虑下面的`fruits`列表：

```py
fruits = ["apples","grapes","berries","oranges","melons"]
```

要使用 **--** 作为分隔符来连接`fruits`列表中的字符串，请指定'--'作为分隔符字符串：

```py
print('--'.join(fruits))
Output >> 'apples--grapes--berries--oranges--melons'
```

要在没有任何空白的情况下连接字符串，请使用空字符串作为分隔符：

```py
print(''.join(fruits))
Output >> 'applesgrapesberriesorangesmelons'
```

要使用一个空格来连接字符串，请指定' '作为分隔符：

```py
print(' '.join(fruits))
Output >> 'apples grapes berries oranges melons'
```

# 6\. 使用 enumerate() 循环

让我们使用之前的`fruits`列表：

```py
fruits = ["apples","grapes","berries","oranges","melons"]
```

你可以使用`range`函数和列表索引来同时获取项目和索引：

然而，使用`enumerate()`函数会更方便。你可以使用`enumerate()`函数同时循环访问索引和索引处的项目：

```py
for idx,fruit in enumerate(fruits):
    print(f"At index {idx}: {fruit}")
```

我们看到我们得到了索引0到4，以及那些索引的元素：

```py
Output >>
At index 0: apples
At index 1: grapes
At index 2: berries
At index 3: oranges
At index 4: melons
```

默认情况下，索引从零开始。有时你可能希望从非零索引开始。要做到这一点，你可以在`enumerate()`函数调用中将起始索引指定为第二个位置参数：

```py
for idx,fruit in enumerate(fruits,1):
    print(f"At index {idx}: {fruit}")
```

现在索引从1开始，而不是从零开始：

```py
Output >>
At index 1: apples
At index 2: grapes
At index 3: berries
At index 4: oranges
At index 5: melons
```

因为`enumerate()`允许你循环遍历可迭代对象，所以你可以在列表和字典推导式中使用它。

例如，你可以使用`enumerate()`函数在字典推导式中将索引和项目作为键和值存储在字典中：

```py
idx_dict = {idx:fruit for idx,fruit in enumerate(fruits)}
print(idx_dict)

Output >> {0: 'apples', 1: 'grapes', 2: 'berries', 3: 'oranges', 4: 'melons'}
```

# 7\. 有用的数学函数

Python 内置的 math 模块提供了对常见数学操作的开箱即用支持。以下是一些有用的函数：

## math.ceil() 和 math.floor()

你通常需要将数字四舍五入到最接近的整数；内置 math 模块中的`floor()`和`ceil()`函数可以帮助你做到这一点：

+   `floor()`函数将给定数字向下舍入到小于或等于给定数字的最大整数。

+   `ceil()`函数将给定数字四舍五入到大于或等于给定数字的最小整数。

这里是一个示例：

![7 个必知的 Python 编程面试技巧](../Images/3c1ad7374ba4967b3093d11508a86469.png)

图片来源：作者

因为3是大于2.47的最小整数，所以`ceil(2.47)`的结果是3：

```py
import math
num1 = 2.47
print(math.ceil(num1))
Output >> 3
```

因为3是小于3.97的最大整数，所以`floor(3.97)`的结果也是3：

```py
num2 = 3.97
print(math.floor(num2))
Output >> 3
```

## math.sqrt() 和列表推导式

要获取一个数字的平方根，你可以使用来自数学模块的`sqrt()`函数。你可以将它与列表推导式结合使用，以扩展功能。

在这里，我们在列表推导式表达式中使用`sqrt()`函数来生成平方根

```py
sqrt_nums = [math.sqrt(num) for num in range(1,11)]
print(sqrt_nums)

Output >> [1.0, 1.4142135623730951, 1.7320508075688772, 2.0, 2.23606797749979, 2.449489742783178, 2.6457513110645907, 2.8284271247461903, 3.0, 3.1622776601683795]
```

为了获得更易读的输出，让我们使用内置的`round()`函数将数字四舍五入到小数点后两位：

```py
sqrt_nums = [round(math.sqrt(num),2) for num in range(1,11)]
print(sqrt_nums)

Output >> [1.0, 1.41, 1.73, 2.0, 2.24, 2.45, 2.65, 2.83, 3.0, 3.16]
```

# 总结

到此为止！希望你找到了一些有用的技巧来丰富你的 Python 工具箱。

如果你想学习和练习 Python，并且有兴趣将 ChatGPT 集成到你的学习工作流程中，请查看这个关于如何使用[ChatGPT 作为 Python 编程助手](/2023/01/chatgpt-python-programming-assistant.html)的指南。

**[Bala Priya C](https://www.linkedin.com/in/bala-priya/)** 是一位技术作家，喜欢创作长篇内容。她的兴趣领域包括数学、编程和数据科学。她通过编写教程、操作指南等，分享她的学习经验与开发者社区。

### 更多相关主题

+   [5 门免费的大学课程助你在编码面试中脱颖而出](https://www.kdnuggets.com/5-free-university-courses-to-ace-coding-interviews)

+   [成功应对初级数据科学职位面试的技巧](https://www.kdnuggets.com/tips-for-successfully-navigating-beginner-data-science-job-interviews)

+   [更多成功应对初级数据科学职位面试的技巧](https://www.kdnuggets.com/more-tips-for-successfully-navigating-beginner-data-science-job-interviews)

+   [在数据科学面试中进行精彩演讲](https://www.kdnuggets.com/2022/01/deliver-killer-presentation-data-science-interviews.html)

+   [破解机器学习工程师面试的秘诀](https://www.kdnuggets.com/2022/10/interview-kickstart-crack-machine-learning-engineer-interviews.html)

+   [SQL 面试准备资料资源](https://www.kdnuggets.com/2023/02/sql-interviews-preparations-material-resources.html)
