# 探索 Python 中的自然排序

> 原文：[https://www.kdnuggets.com/exploring-natural-sorting-in-python](https://www.kdnuggets.com/exploring-natural-sorting-in-python)

![natsort](../Images/d6abe96d6dbba0e1997ba250e5355cd5.png)

作者提供的图片

## 什么是自然排序，我们为什么需要它？

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

在处理 Python 可迭代对象如列表时，排序是一项常见操作。要对列表进行排序，可以使用列表方法 `sort()` 来就地排序，或使用 `sorted()` 函数返回排序后的列表。

`sorted()` 函数在处理数字或包含字母的字符串列表时效果很好。但是，对于包含字母数字字符的字符串，如文件名、目录名、版本号等情况呢？`sorted()` 函数执行的是字典序排序。

看这个简单的例子：

```py
# List of filenames
filenames = ["file10.txt", "file2.txt", "file1.txt"]

sorted_filenames = sorted(filenames)
print(sorted_filenames)
```

你将得到如下输出：

```py
Output >>> ['file1.txt', 'file10.txt', 'file2.txt'] 
```

好吧，'file10.txt' 出现在 'file2.txt' 之前，不是我们所期望的直观排序顺序。这是因为 `sorted()` 函数使用字符的 ASCII 值进行排序，而不是数字值。**引入自然排序**。

自然排序是一种排序技术，它以反映元素自然顺序的方式对元素进行排列，特别是对于字母数字数据。与字典序排序不同，自然排序会解释字符串中的数字值，并据此进行排列，从而得到更有意义和符合预期的序列。

在本教程中，我们将使用 Python 库 [natsort](https://pypi.org/project/natsort/) 探索自然排序。

## 入门

要开始使用，你可以通过 pip 安装 `natsort` 库：

```py
$ pip3 install natsort
```

最佳实践是，在项目中使用虚拟环境安装所需的软件包。由于 natsort 需要 Python 3.7 或更高版本，请确保使用的是最新的 Python 版本，最好是 Python 3.11 或更高版本。要了解如何管理不同的 Python 版本，请阅读 [Python 版本太多了？Pyenv 来拯救你](https://www.kdnuggets.com/too-many-python-versions-to-manage-pyenv-to-the-rescue)。

## 自然排序基础示例

我们将从自然排序有用的简单用例开始：

+   排序文件名：在处理包含数字的文件名时，自然排序确保文件按自然直观的顺序排列。

+   版本排序：自然排序也有助于对版本号字符串进行排序，确保版本根据其数字值而非ASCII值进行排序。这可能不会反映所需的版本顺序。

现在让我们继续编写这些示例代码。

### 排序文件名

现在我们已经安装了natsort库，可以将其导入到我们的Python脚本中，使用该库提供的不同函数。

让我们重新回顾排序文件名的第一个例子（就是我们在教程开始时看到的那个），其中字典序排序的结果并不是我们想要的。

现在让我们使用`natsorted()`函数对相同的列表进行排序，如下所示：

```py
import natsort

# List of filenames
filenames = ["file10.txt", "file2.txt", "file1.txt"]

# Sort filenames naturally
sorted_filenames = natsort.natsorted(filenames)
print(sorted_filenames)
```

在这个例子中，来自natsort库的`natsorted()`函数被用来自然地排序文件名列表。结果是文件名按预期的数字顺序排列：

```py
Output >>> ['file1.txt', 'file2.txt', 'file10.txt']
```

### 排序版本号

让我们再来看一个类似的例子，我们有表示版本的字符串：

```py
import natsort

# List of version numbers
versions = ["v-1.10", "v-1.2", "v-1.5"]

# Sort versions naturally
sorted_versions = natsort.natsorted(versions)

print(sorted_versions) 
```

在这里，`natsorted()`函数被应用于自然地排序版本号列表。结果排序列表保持了版本的正确数字顺序：

```py
Output >>> ['v-1.2', 'v-1.5', 'v-1.10']
```

## 使用键自定义排序

使用内置的`sorted()`函数时，你可能已经使用了`key`参数进行自定义。同样，`sorted()`函数也接受可选的`key`参数，你可以用它来根据特定的标准进行排序。

让我们举一个例子：我们有`file_data`，它是一个元组列表。元组中的第一个元素（索引0处）是文件名，第二个元素（索引1处）是文件大小。

假设我们想按文件大小升序排序。因此，我们将`key`参数设置为`lambda x: x[1]`，这样就使用了索引1处的文件大小作为排序键：

```py
import natsort

# List of tuples containing filename and size
file_data = [
("data_20230101_080000.csv", 100),
("data_20221231_235959.csv", 150),
("data_20230201_120000.csv", 120),
("data_20230115_093000.csv", 80)
]

# Sort file data based on file size
sorted_file_data = natsort.natsorted(file_data, key=lambda x:x[1])

# Print sorted file data
for filename, size in sorted_file_data:
    print(filename, size) 
```

以下是输出结果：

```py
data_20230115_093000.csv 80
data_20230101_080000.csv 100
data_20230201_120000.csv 120
data_20221231_235959.csv 150
```

## 不区分大小写的字符串排序

自然排序还有另一个有用的场景，就是当你需要对字符串进行不区分大小写的排序时。基于ASCII值的字典序排序将不会给出期望的结果。

为了进行不区分大小写的排序，我们可以将`alg`设置为`natsort.ns.IGNORECASE`，这将在排序时忽略大小写。`alg`键控制`natsorted()`使用的算法：

```py
import natsort

# List of strings with mixed case
words = ["apple", "Banana", "cat", "Dog", "Elephant"]

# Sort words naturally with case-insensitivity
sorted_words = natsort.natsorted(words, alg=natsort.ns.IGNORECASE)

print(sorted_words)
```

在这里，包含混合大小写的单词列表被自然地按不区分大小写的方式排序：

```py
Output >>> ['apple', 'Banana', 'cat', 'Dog', 'Elephant']
```

## 总结

就这样！在本教程中，我们回顾了字典序排序的局限性以及自然排序如何成为处理字母数字字符串的良好替代方案。你可以在[GitHub](https://github.com/balapriyac/python-basics/tree/main/natural-sorting)上找到所有代码。

我们从简单的例子开始，也了解了如何基于自定义键排序以及在Python中处理不区分大小写的排序。接下来，你可以探索[natsort库的其他功能](https://natsort.readthedocs.io/en/5.1.0/)。在另一个Python教程中再见。直到那时，继续编程！

**[](https://twitter.com/balawc27)**[Bala Priya C](https://www.kdnuggets.com/wp-content/uploads/bala-priya-author-image-update-230821.jpg)**** 是来自印度的开发者和技术写作人员。她喜欢在数学、编程、数据科学和内容创作的交汇点工作。她的兴趣和专长领域包括DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编码和咖啡！目前，她正在通过编写教程、使用指南、意见文章等与开发者社区分享她的知识。Bala还创建了引人入胜的资源概述和编码教程。

### 更多相关话题

+   [用Python探索数据清理技术](https://www.kdnuggets.com/2023/04/exploring-data-cleaning-techniques-python.html)

+   [探索Python itertools中的无限迭代器](https://www.kdnuggets.com/exploring-infinite-iterators-in-python-itertools)

+   [用Python探索OpenAI API](https://www.kdnuggets.com/exploring-the-openai-api-with-python)

+   [25本免费书籍掌握SQL、Python、数据科学、机器学习……](https://www.kdnuggets.com/25-free-books-to-master-sql-python-data-science-machine-learning-and-natural-language-processing)

+   [探索思维树提示：AI如何通过搜索学习推理……](https://www.kdnuggets.com/2023/07/exploring-tree-of-thought-prompting-ai-learn-reason-through-search.html)

+   [探索GPT-4的力量和局限性](https://www.kdnuggets.com/2023/07/exploring-power-limitations-gpt4.html)
