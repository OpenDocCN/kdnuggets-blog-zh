# 如何在 Python 中找到集合差集

> 原文：[https://www.kdnuggets.com/2023/03/find-set-difference-python.html](https://www.kdnuggets.com/2023/03/find-set-difference-python.html)

![如何在 Python 中找到集合差集](../Images/de9966d30430ba2c37df59fae6fc0161.png)

作者提供的图片

在 Python 中，集合是内置的数据结构，存储的是无序的、不重复的和不可变的元素集合。你可以对 Python 集合执行集合论中的常见操作，如并集、交集和集合差集。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

本教程将教你如何在 Python 中计算集合差集。你将学习如何使用内置集合方法 difference() 和 - 运算符来找到集合差集，以及如何在过程中调试常见错误。

让我们开始吧。

# 什么是集合差集？

在计算 Python 集合的差集之前，让我们快速回顾一下集合差集操作。

给定两个集合 A 和 B，我们可以定义如下：

+   **A - B**:（读作 A 差 B）是指存在于集合 A 中但不在集合 B 中的所有元素的集合。

+   **B - A**:（读作 B 差 A）是指存在于集合 B 中但不在集合 A 中的所有元素的集合。

集合差集 *不是* 交换律操作。因此，A - B 不同于 B - A，除非集合 A 和 B 是相等的，即 A 和 B 包含相同的元素。

我们可以从下面的简单示例中看到这一点：

![什么是集合差集？](../Images/c8feb81cac0c0cb9aad39b6c1f4fed8b.png)

作者提供的图片

在这个示例中：

+   集合 A: {2,1,3,12,7}

+   集合 B: {21,3,12,10}

因此，A - B 是 {1,2,7}，是仅存在于 A 中的元素集合。而 B - A 是 {21,10}，是仅存在于 B 中的元素集合。

# 使用 difference() 方法进行集合差集

让我们定义 `fruits` 和 `to_eat`，这是两个包含字符串作为单独元素的 Python 集合。

![使用 difference() 方法的集合差集](../Images/8d1ef0303b3f73ea8a2d54c648866b98.png)

作者提供的图片

```py
fruits = {"apples","oranges","berries","cherries"}
to_eat = {"apples","cereals","berries","bread"}
```

现在要找到 **fruits - to_eat**，让我们在 `fruits` 集合上调用 `difference()` 方法，将 `to_eat` 作为参数：

```py
print(fruits.difference(to_eat))
Output >> {'cherries', 'oranges'}
```

同样，要找到 **to_eat - fruits**，让我们在 `to_eat` 集合上调用 `difference()` 方法，如下所示：

```py
print(to_eat.difference(fruits))
Output >> {'cereals', 'bread'}
```

# 使用差集运算符 (-) 进行集合差集

我们也可以使用 **差集运算符** (-) 来找到集合差集。

让我们回到相同的例子：对于集合`fruits`和`to_eat`，使用差集运算符(-)执行等效操作并返回相同结果：

```py
print(fruits - to_eat)
Output >> {'cherries', 'oranges'}
```

```py
print(to_eat - fruits)
Output >> {'cereals', 'bread'}
```

# 调试常见的集合差集错误

在我们迄今为止编写的示例中，我们计算了两个Python集合之间的差集。但没有强调`difference()`方法如何与差集运算符不同。

你可以在任何有效的集合对象上调用`difference()`方法。但是，你可以传入一个或多个集合或其他Python可迭代对象。以下是一个示例。

让我们定义`set1`、`list1`、`list2`和`list3`：

```py
set1 = {1,2,3,4,5}
list1 = [2,4,6]
list2 = [3,6,9]
list3 = [10,11]
```

现在我们可以在`set1`上调用`difference()`方法，并在方法调用中传入`list1`、`list2`和`list3`。

```py
print(set1.difference(list1,list2,list3))
Output >> {1, 5}
```

在这种情况下，`difference`方法返回集合中仅存在于`set1`而不在`list1`、`list2`和`list3`中的元素{1,5}。

但是，如果你尝试使用差集运算符，你会遇到`TypeError`异常。因为-是一个二元运算符，它操作于两个操作数，所以我们先将三个列表连接起来，然后尝试计算集合差集：

```py
>>> set1 - (list1 + list2 + list3)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unsupported operand type(s) for -: 'set' and 'list'</module></stdin>
```

如上所示，与`difference()`方法不同，**-**运算符仅适用于Python集合。因此，你必须在计算差集之前将其他可迭代对象转换为Python集合，如下所示：

```py
print(set1 - set(list1 + list2 + list3))
Output >> {1, 5}
```

# 结论

让我们快速回顾一下在本教程中学到的内容。要找到集合差集，我们可以使用Python集合上的`difference()`方法或-运算符。虽然`difference()`方法作用于集合并接受一个或多个Python可迭代对象作为参数，而-运算符仅允许在两个Python集合之间执行集合差集操作。如果你想学习Python，可以查看这个[免费资源列表](/2022/11/9-free-resources-master-python.html)。

**[Bala Priya C](https://www.linkedin.com/in/bala-priya/)** 是一位技术作家，喜欢创作长篇内容。她的兴趣领域包括数学、编程和数据科学。她通过编写教程、操作指南等与开发者社区分享她的学习成果。

### 更多相关话题

+   [停止学习数据科学，找到目标，寻找目标来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [初级与高级数据科学家薪资：有什么区别？](https://www.kdnuggets.com/2022/03/junior-senior-data-scientist-salary-difference.html)

+   [如何找到最佳的数据科学远程职位](https://www.kdnuggets.com/2022/12/find-best-data-science-remote-jobs.html)

+   [数据分析师与数据科学家有什么区别？](https://www.kdnuggets.com/2022/03/difference-data-analysts-data-scientists.html)

+   [快速指南：找到合适的标注人才](https://www.kdnuggets.com/2022/04/quick-guide-find-right-minds-annotation.html)

+   [NLP、NLU和NLG：有什么区别？全面指南](https://www.kdnuggets.com/2022/06/nlp-nlu-nlg-difference-comprehensive-guide.html)
