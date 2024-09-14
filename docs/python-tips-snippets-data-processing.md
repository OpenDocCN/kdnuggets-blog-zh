# 5 个 Python 数据处理技巧与代码片段

> 原文：[`www.kdnuggets.com/2021/07/python-tips-snippets-data-processing.html`](https://www.kdnuggets.com/2021/07/python-tips-snippets-data-processing.html)

评论 ![图片](img/113c4ec2bfab26d5c9db691de1e5d6ba.png)

图片来源：[Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/python-code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

本文包含 5 个有用的 Python 代码片段，初学者可能会发现这些对数据处理非常有帮助。

* * *

## 我们的前 3 名课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 工作

* * *

Python 是一种灵活的通用编程语言，提供了多种方法来处理和完成相同的任务。这些代码片段阐明了一种针对特定情况的方法；你可能会发现它们有用，或者发现你找到了另一种更合适的方法。

### 1\. 合并多个文本文件

让我们从合并多个文本文件开始。如果你有多个文本文件在同一个目录中需要合并为一个文件，这段 Python 代码将完成这一任务。

首先我们获取路径中所有 txt 文件的列表；然后我们读取每个文件并将其内容写入新的输出文件；最后，我们重新读取新文件并将内容打印到屏幕上以进行验证。

```py
import glob

# Load all txt files in path
files = glob.glob('/path/to/files/*.txt')

# Concatenate files to new file
with open('2020_output.txt', 'w') as out_file:
    for file_name in files:
        with open(file_name) as in_file:
            out_file.write(in_file.read())

# Read file and print
with open('2020_output.txt', 'r') as new_file:
    lines = [line.strip() for line in new_file]
for line in lines: print(line)
```

```py
file 1 line 1
file 1 line 2
file 1 line 3
file 2 line 1
file 2 line 2
file 2 line 3
file 3 line 1
file 3 line 2
file 3 line 3
```

### 2\. 将多个 CSV 文件合并为一个数据框

继续探讨文件合并的主题，这次让我们处理将多个逗号分隔值文件合并为一个 Pandas 数据框。

我们首先获取路径中 CSV 文件的列表；然后，对于路径中的每个文件，我们将内容读取到各自的数据框中；之后，我们将所有数据框合并为一个数据框；最后，我们打印结果以检查。

```py
import pandas as pd
import glob

# Load all csv files in path
files = glob.glob('/path/to/files/*.csv')

# Create a list of dataframe, one series per CSV
fruit_list = []
for file_name in files:
    df = pd.read_csv(file_name, index_col=None, header=None)
    fruit_list.append(df)

# Create combined frame out of list of individual frames
fruit_frame = pd.concat(fruit_list, axis=0, ignore_index=True)

print(fruit_frame)
```

```py
            0   1    2
0      grapes   3  5.5
1      banana   7  6.8
2       apple   2  2.3
3      orange   9  7.2
4  blackberry  12  4.3
5   starfruit  13  8.9
6  strawberry   9  8.3
7        kiwi   7  2.7
8   blueberry   2  7.6
```

### 3\. 压缩与解压文件到 Pandas

假设你正在使用一个 Pandas 数据框，例如上面代码片段中的结果数据框，并希望将数据框直接压缩保存到文件中。这个代码片段将实现这一点。

首先我们将创建一个数据框以供示例使用；然后，我们将直接压缩并保存数据框到文件；最后，我们将从压缩文件中直接读取数据框并打印以进行验证。

```py
import pandas as pd

# Create a dataframe to use
df = pd.DataFrame({'col_A': ['kiwi', 'banana', 'apple'],
	           'col_B': ['pineapple', 'grapes', 'grapefruit'],
		   'col_C': ['blueberry', 'grapefruit', 'orange']})

# Compress and save dataframe to file
df.to_csv('sample_dataframe.csv.zip', index=False, compression='zip')
print('Dataframe compressed and saved to file')

# Read compressed zip file into dataframe
df = pd.read_csv('sample_dataframe.csv.zip',)
print(df)
```

```py
Dataframe compressed and saved to file

    col_A       col_B       col_C
0    kiwi   pineapple   blueberry
1  banana      grapes  grapefruit
2   apple  grapefruit      orange
```

### 4\. 展开列表

或许你正面临一个处理嵌套列表的情况，也就是说，一个列表的所有元素都是列表。这个代码片段将把这个嵌套列表展平为一个线性列表。

首先，我们将创建一个列表的列表用于示例；然后我们将使用列表推导式以 Pythonic 的方式展平列表；最后，我们将打印结果列表以进行验证。

```py
# Create of list of lists (a list where all of its elements are lists)
list_of_lists = [['apple', 'pear', 'banana', 'grapes'], 
                 ['zebra', 'donkey', 'elephant', 'cow'],
	         ['vanilla', 'chocolate'], 
                 ['princess', 'prince']]

# Flatten the list of lists into a single list
flat_list = [element for sub_list in list_of_lists for element in sub_list]

# Print both to compare
print(f'List of lists:\n{list_of_lists}')
print(f'Flattened list:\n{flat_list}')
```

```py
List of lists:
[['apple', 'pear', 'banana', 'grapes'], ['zebra', 'donkey', 'elephant', 'cow'], ['vanilla', 'chocolate'], ['princess', 'prince']]

Flattened list:
['apple', 'pear', 'banana', 'grapes', 'zebra', 'donkey', 'elephant', 'cow', 'vanilla', 'chocolate', 'princess', 'prince']
```

### 5\. 排序元组列表

这段代码将探讨根据指定元素对元组进行排序的想法。元组是一个常被忽视的 Python 数据结构，它是存储相关数据片段的好方法，而不需要使用更复杂的结构类型。

在这个示例中，我们将首先创建一个大小为 2 的元组列表，并用数字数据填充它们；接着我们将分别按第一个和第二个元素对这些对进行排序，打印两个排序过程的结果以检查结果；最后，我们将扩展这种排序到混合的字母数字数据元素。

```py
# Some paired data
pairs = [(1, 10.5), (5, 7.), (2, 12.7), (3, 9.2), (7, 11.6)]

# Sort pairs by first entry
sorted_pairs  = sorted(pairs, key=lambda x: x[0])
print(f'Sorted by element 0 (first element):\n{sorted_pairs}')

# Sort pairs by second entry
sorted_pairs  = sorted(pairs, key=lambda x: x[1])
print(f'Sorted by element 1 (second element):\n{sorted_pairs}')

# Extend this to tuples of size n and non-numeric entries
pairs = [('banana', 3), ('apple', 11), ('pear', 1), ('watermelon', 4), ('strawberry', 2), ('kiwi', 12)]
sorted_pairs  = sorted(pairs, key=lambda x: x[0])
print(f'Alphanumeric pairs sorted by element 0 (first element):\n{sorted_pairs}')
```

```py
Sorted by element 0 (first element):
[(1, 10.5), (2, 12.7), (3, 9.2), (5, 7.0), (7, 11.6)]

Sorted by element 1 (second element):
[(5, 7.0), (3, 9.2), (1, 10.5), (7, 11.6), (2, 12.7)]

Alphanumeric pairs sorted by element 0 (first element):
[('apple', 11), ('banana', 3), ('kiwi', 12), ('pear', 1), ('strawberry', 2), ('watermelon', 4)]

```

以上就是 5 个 Python 代码片段，这些片段可能对初学者处理各种数据处理任务有所帮助。

**相关**：

+   SQL 中的数据准备及备忘单!

+   如何在命令行清理文本数据

+   数据科学、数据可视化和机器学习的顶级 Python 库

### 更多关于这个主题

+   [每个数据科学家都应了解的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [是什么让 Python 成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [停止学习数据科学以寻找目标，再通过目标寻找目的…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [一个 90 亿美元的 AI 失败，经过检验](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)
