# Python 字符串处理备忘单

> 原文：[https://www.kdnuggets.com/2020/01/python-string-processing-primer.html](https://www.kdnuggets.com/2020/01/python-string-processing-primer.html)

# Python 中的字符串处理

自然语言处理和文本分析目前是研究和应用的热门领域。这些领域包含了各种具体的技能和概念，需要彻底理解后才能进行有意义的实践。然而，在达到这一点之前，基本的字符串操作和处理是必须的。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 方面

* * *

我认为需要讨论两种广泛的计算字符串处理技能。第一种是 [正则表达式](https://en.wikipedia.org/wiki/Regular_expression)，一种基于模式的文本匹配方法。可以寻找许多关于正则表达式的优秀介绍，但视觉学习者可能会喜欢 [fast.ai 的代码优先自然语言处理课程视频](https://youtu.be/Q1zLqfnEXdw?list=PLtmWHNX-gukKocXQOkQjuVxglSDYWsSh9&t=630)。

另一种显著的计算字符串处理技能是能够利用给定编程语言的标准库进行基本字符串操作。因此，本文是一个简短的 Python 字符串处理入门。

请注意，有意义的文本分析远远超出了字符串处理，这些更高级技术的核心可能不需要你经常手动操作文本。然而，文本数据处理是成功的文本分析项目中重要且耗时的部分，以上提到的字符串处理技能在这里将非常宝贵。基本水平上理解文本的计算处理对理解更高级的文本分析技术也是概念上非常重要的。

以下许多示例使用了 Python 标准库中的 [string 模块](https://docs.python.org/2/library/string.html)，因此手头有它作为参考是个好主意。

这个实用的备忘单包含了所有的代码在 [**这个可下载的 PDF 文件**](https://www.kdnuggets.com/publications/sheets/Python-String-Processing-Cheatsheet-KDnuggets.pdf) 中。

[![Python 字符串处理备忘单](../Images/1829cc006761b1f6e785213a4bd45a2e.png)](https://www.kdnuggets.com/publications/sheets/Python-String-Processing-Cheatsheet-KDnuggets.pdf)

# 去除空白

[去除空白](https://docs.python.org/3/library/stdtypes.html#str.lstrip)是基本的字符串处理需求。你可以使用 `lstrip()` 方法去除前导空白（左侧），使用 `rstrip()` 去除尾随空白（右侧），使用 `strip()` 去除前导和尾随空白。

```py
s = '   This is a sentence with whitespace.       \n'

print('Strip leading whitespace: {}'.format(s.lstrip()))
print('Strip trailing whitespace: {}'.format(s.rstrip()))
print('Strip all whitespace: {}'.format(s.strip()))
```

```py
Strip leading whitespace: This is a sentence with whitespace.       

Strip trailing whitespace:    This is a sentence with whitespace.
Strip all whitespace: This is a sentence with whitespace.
```

对于去除空白之外的字符感兴趣？相同的方法也很有用，通过传递你希望去除的字符（们）来使用它们。

```py
s = 'This is a sentence with unwanted characters.AAAAAAAA'

print('Strip unwanted characters: {}'.format(s.rstrip('A')))
```

```py
Strip unwanted characters: This is a sentence with unwanted characters.
```

如果需要，请不要忘记查看字符串[`format()`](https://docs.python.org/3/library/stdtypes.html#str.format)的文档。

# 拆分字符串

将字符串拆分成更小的子字符串列表在 Python 中通常是有用的，可以通过[`split()`](https://docs.python.org/3/library/stdtypes.html#str.split)方法轻松完成。

```py
s = 'KDnuggets is a fantastic resource'

print(s.split())
```

```py
['KDnuggets', 'is', 'a', 'fantastic', 'resource']
```

默认情况下，`split()` 以空白字符进行拆分，但也可以传递其他字符（们）序列。

```py
s = 'these,words,are,separated,by,comma'
print('\',\' separated split -> {}'.format(s.split(',')))

s = 'abacbdebfgbhhgbabddba'
print('\'b\' separated split -> {}'.format(s.split('b')))
```

```py
',' separated split -> ['these', 'words', 'are', 'separated', 'by', 'comma']
'b' separated split -> ['a', 'ac', 'de', 'fg', 'hhg', 'a', 'dd', 'a']
```

# 将列表元素合并成一个字符串

需要以上操作的相反操作吗？你可以使用[`join()`](https://docs.python.org/3/library/stdtypes.html#str.join)方法将列表元素字符串连接成一个单一字符串。

```py
s = ['KDnuggets', 'is', 'a', 'fantastic', 'resource']

print(' '.join(s))
```

```py
KDnuggets is a fantastic resource
```

这倒是真的！如果你想用空白之外的内容连接列表元素呢？这个可能会有点奇怪，但也很容易实现。

```py
s = ['Eleven', 'Mike', 'Dustin', 'Lucas', 'Will']

print(' and '.join(s))
```

```py
Eleven and Mike and Dustin and Lucas and Will
```

# 反转字符串

Python 并没有内置的字符串反转方法。然而，鉴于字符串可以像列表一样[切片](https://docs.python.org/3/reference/expressions.html?highlight=slice#slicings)，反转一个字符串可以以与反转列表元素相同的简洁方式完成。

```py
s = 'KDnuggets'

print('The reverse of KDnuggets is {}'.format(s[::-1]))
```

```py
The reverse of KDnuggets is: steggunDK
```

# 转换大写和小写

转换大小写可以通过[`upper()`](https://docs.python.org/3/library/stdtypes.html#str.upper)、[`lower()`](https://docs.python.org/3/library/stdtypes.html#str.lower) 和 [`swapcase()`](https://docs.python.org/3/library/stdtypes.html#str.swapcase) 方法完成。

```py
s = 'KDnuggets'

print('\'KDnuggets\' as uppercase: {}'.format(s.upper()))
print('\'KDnuggets\' as lowercase: {}'.format(s.lower()))
print('\'KDnuggets\' as swapped case: {}'.format(s.swapcase()))
```

```py
'KDnuggets' as uppercase: KDNUGGETS
'KDnuggets' as lowercase: kdnuggets
'KDnuggets' as swapped case: kdNUGGETS
```

# 检查字符串成员

在 Python 中检查字符串成员的最简单方法是使用 `in` 运算符。语法非常自然语言化。

```py
s1 = 'perpendicular'
s2 = 'pen'
s3 = 'pep'

print('\'pen\' in \'perpendicular\' -> {}'.format(s2 in s1))
print('\'pep\' in \'perpendicular\' -> {}'.format(s3 in s1))
```

```py
'pen' in 'perpendicular' -> True
'pep' in 'perpendicular' -> False
```

如果你更感兴趣于在字符串中查找子字符串的位置（而不是仅仅检查子字符串是否包含在其中），`find()` 字符串方法可能会更有帮助。

```py
s = 'Does this string contain a substring?'

print('\'string\' location -> {}'.format(s.find('string')))
print('\'spring\' location -> {}'.format(s.find('spring')))
```

```py
'string' location -> 10
'spring' location -> -1
```

默认情况下，`find()` 返回子字符串第一次出现的第一个字符的索引，如果未找到子字符串，则返回 `-1`。查看文档以获取对默认行为的可用调整。

# 替换子字符串

如果你想要*替换*子字符串，而不仅仅是查找它们呢？Python 的 [`replace()`](https://docs.python.org/3/library/stdtypes.html#str.replace) 字符串方法可以处理这个需求。

```py
s1 = 'The theory of data science is of the utmost importance.'
s2 = 'practice'

print('The new sentence: {}'.format(s1.replace('theory', s2)))
```

```py
The new sentence: The practice of data science is of the utmost importance.
```

一个可选的计数参数可以指定在相同子字符串出现多次时，进行的连续替换的最大次数。

# 合并多个列表的输出

有多个字符串列表想要按元素组合在一起？使用[`zip()`](https://docs.python.org/3/library/functions.html#zip)函数没有问题。

```py
countries = ['USA', 'Canada', 'UK', 'Australia']
cities = ['Washington', 'Ottawa', 'London', 'Canberra']

for x, y in zip(countries, cities):
  print('The capital of {} is {}.'.format(x, y))
```

```py
The capital of USA is Washington.
The capital of Canada is Ottawa.
The capital of UK is London.
The capital of Australia is Canberra.
```

# 检查变位词

想要检查一对字符串是否是变位词吗？在算法上，我们只需统计每个字符串中每个字母的出现次数，并检查这些计数是否相等。使用[`Counter`类](https://docs.python.org/3/library/collections.html#collections.Counter)来完成这个任务非常简单。

```py
from collections import Counter
def is_anagram(s1, s2):
  return Counter(s1) == Counter(s2)

s1 = 'listen'
s2 = 'silent'
s3 = 'runner'
s4 = 'neuron'

print('\'listen\' is an anagram of \'silent\' -> {}'.format(is_anagram(s1, s2)))
print('\'runner\' is an anagram of \'neuron\' -> {}'.format(is_anagram(s3, s4)))
```

```py
'listen' an anagram of 'silent' -> True
'runner' an anagram of 'neuron' -> False
```

# 检查回文

如果你想检查一个单词是否是回文怎么办？在算法上，我们需要创建该单词的反向，然后使用`==`运算符检查这两个字符串（原始字符串和反向字符串）是否相等。

```py
def is_palindrome(s):
  reverse = s[::-1]
  if (s == reverse):
    return True
  return False

s1 = 'racecar'
s2 = 'hippopotamus'

print('\'racecar\' a palindrome -> {}'.format(is_palindrome(s1)))
print('\'hippopotamus\' a palindrome -> {}'.format(is_palindrome(s2)))
```

```py
'racecar' is a palindrome -> True
'hippopotamus' is a palindrome -> False
```

确保你下载了[Python字符串处理备忘单](https://www.kdnuggets.com/publications/sheets/Python-String-Processing-Cheatsheet-KDnuggets.pdf)。

### 更多相关主题

+   [Python字符串方法](https://www.kdnuggets.com/2022/12/python-string-methods.html)

+   [Python字符串匹配无需复杂的正则表达式语法](https://www.kdnuggets.com/2023/02/python-string-matching-without-complex-regex-syntax.html)

+   [将字节转换为Python中的字符串：初学者教程](https://www.kdnuggets.com/convert-bytes-to-string-in-python-a-tutorial-for-beginners)

+   [SQL备忘单的数据准备](https://www.kdnuggets.com/2021/05/data-preparation-sql-cheat-sheet.html)

+   [KDnuggets新闻，11月30日：什么是切比雪夫定理以及如何…](https://www.kdnuggets.com/2022/n46.html)

+   [R中的数据准备备忘单](https://www.kdnuggets.com/2021/10/data-preparation-r-dplyr-cheat-sheet.html)
