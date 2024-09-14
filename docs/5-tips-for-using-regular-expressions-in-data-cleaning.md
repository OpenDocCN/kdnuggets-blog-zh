# 使用正则表达式进行数据清洗的 5 个技巧

> 原文：[https://www.kdnuggets.com/5-tips-for-using-regular-expressions-in-data-cleaning](https://www.kdnuggets.com/5-tips-for-using-regular-expressions-in-data-cleaning)

![5 Tips for Using Regular Expressions in Data Cleaning](../Images/dd0cd3b5a960921fd88cc33160d7c5a8.png)

作者提供的图片 | 使用 Canva 创建

如果你是 Linux 或 Mac 用户，你可能已经在命令行中使用过 [grep](https://www.gnu.org/software/grep/manual/grep.html) 来通过匹配模式搜索文件。正则表达式（regex）允许你基于模式搜索、匹配和操作文本。这使得它们成为处理文本和清洗数据的强大工具。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

对于 Python 中的正则表达式匹配操作，你可以使用内置的 [re 模块](https://docs.python.org/3/library/re.html)。在本教程中，我们将探讨如何使用正则表达式来清洗数据。我们将讨论如何去除不需要的字符、提取特定模式、查找和替换文本等。

## 1\. 去除不需要的字符

在继续之前，我们先导入内置的 re 模块：

```py
import re
```

字符串字段（几乎）总是需要广泛清洗才能进行分析。不需要的字符——通常源于不同的格式——会使你的数据难以分析。正则表达式可以帮助你高效地去除这些字符。

你可以使用 `sub()` 函数从 re 模块中替换或删除模式或特殊字符的所有出现。假设你有包含破折号和括号的电话号码字符串。你可以像下面这样去除它们：

```py
text = "Contact info: (123)-456-7890 and 987-654-3210."
cleaned_text = re.sub(r'[()-]', '', text)
print(cleaned_text) 
```

在这里，**re.sub(pattern, replacement, string)** 将字符串中所有模式的出现替换为替换内容。我们使用 **r'[()-]'** 模式来匹配任何出现的 (, ), 或 -，得到如下输出：

```py
Output >>> Contact info: 1234567890 or 9876543210
```

## 2\. 提取特定模式

从文本字段中提取电子邮件地址、网址或电话号码是常见任务，因为这些都是相关的信息。为了提取所有特定的感兴趣模式，你可以使用 `findall()` 函数。

你可以像这样从文本中提取电子邮件地址：

```py
text = "Please reach out to us at support@example.org or help@example.org."
emails = re.findall(r'\b[\w.-]+?@\w+?\.\w+?\b', text)
print(emails)
```

**re.findall(pattern, string)** 函数找到并返回（作为列表）字符串中所有模式的出现。我们使用模式 **r'\b[\w.-]+?@\w+?\.\w+?\b'** 来匹配所有电子邮件地址：

```py
Output >>> ['support@example.com', 'help@example.org']
```

## 3\. 替换模式

我们已经使用`sub()`函数去除不必要的特殊字符。但你也可以用另一种模式替换原有模式，以便字段适合更一致的分析。

这是一个移除不必要空格的示例：

```py
text = "Using     regular     expressions."
cleaned_text = re.sub(r'\s+', ' ', text)
print(cleaned_text) 
```

**r'\s+'**模式匹配一个或多个空白字符。替换字符串是一个单一的空格，得到的输出是：

```py
Output >>> Using regular expressions.
```

## 4\. 验证数据格式

验证数据格式可以确保数据的一致性和正确性。正则表达式可以验证如电子邮件、电话号码和日期等格式。

这是你如何使用`match()`函数来验证电子邮件地址：

```py
email = "test@example.com"
if re.match(r'^\b[\w.-]+?@\w+?\.\w+?\b$', email):
    print("Valid email")  
else:
    print("Invalid email")
```

在这个例子中，电子邮件字符串是有效的：

```py
Output >>> Valid email
```

## 5\. 按模式分割字符串

有时你可能想根据模式或特定分隔符的出现将一个字符串拆分成多个字符串。你可以使用`split()`函数来实现。

我们来把`text`字符串分割成句子：

```py
text = "This is sentence one. And this is sentence two! Is this sentence three?"
sentences = re.split(r'[.!?]', text)
print(sentences) 
```

在这里，**re.split(pattern, string)** 在模式的所有出现处分割字符串。我们使用**r'[.!?]'**模式来匹配句号、感叹号或问号：

```py
Output >>> ['This is sentence one', ' And this is sentence two', ' Is this sentence three', '']
```

## 使用正则表达式清理Pandas数据框

将正则表达式与pandas结合使用，可以高效地清理数据框。

要从名称中去除非字母字符并在数据框中验证电子邮件地址：

```py
import pandas as pd

data = {
	'names': ['Alice123', 'Bob!@#', 'Charlie$$$'],
	'emails': ['alice@example.com', 'bob_at_example.com', 'charlie@example.com']
}
df = pd.DataFrame(data)

# Remove non-alphabetic characters from names
df['names'] = df['names'].str.replace(r'[^a-zA-Z]', '', regex=True)

# Validate email addresses
df['valid_email'] = df['emails'].apply(lambda x: bool(re.match(r'^\b[\w.-]+?@\w+?\.\w+?\b$', x)))

print(df)
```

在上述代码片段中：

+   `df['names'].str.replace(pattern, replacement, regex=True)` 替换序列中模式的出现。

+   `lambda x: bool(re.match(pattern, x))`：这个lambda函数应用正则表达式匹配并将结果转换为布尔值。

输出结果如下：

```py
 names           	   emails    valid_email
0	  Alice	        alice@example.com     	    True
1  	  Bob          bob_at_example.com    	    False
2         Charlie     charlie@example.com     	    True
```

## 总结

我希望你觉得这个教程有用。让我们回顾一下我们学到的内容：

+   使用`re.sub`去除不必要的字符，如电话号码中的破折号和括号。

+   使用`re.findall`从文本中提取特定模式。

+   使用`re.sub`替换模式，例如将多个空格转换为一个空格。

+   使用`re.match`验证数据格式，以确保数据符合特定格式，如验证电子邮件地址。

+   要根据模式分割字符串，应用`re.split`。

实际操作中，你将结合正则表达式和pandas来高效清理数据框中的文本字段。注释你的正则表达式以解释其目的也是一种良好的实践，能够提高可读性和可维护性。要了解更多关于使用pandas进行数据清理的内容，请阅读 [掌握Python和Pandas数据清理的7个步骤](https://www.kdnuggets.com/7-steps-to-mastering-data-cleaning-with-python-and-pandas)。

**[](https://twitter.com/balawc27)**[Bala Priya C](https://www.kdnuggets.com/wp-content/uploads/bala-priya-author-image-update-230821.jpg)**** 是来自印度的开发者和技术作家。她喜欢在数学、编程、数据科学和内容创作的交汇点上工作。她的兴趣和专长领域包括DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编程和喝咖啡！目前，她正在通过编写教程、操作指南、观点文章等方式向开发者社区学习和分享她的知识。Bala还创建了引人入胜的资源概述和编码教程。

### 更多相关主题

+   [使用Python掌握正则表达式](https://www.kdnuggets.com/2023/08/mastering-regular-expressions-python.html)

+   [关于掌握SQL、Python、数据清理、数据处理和探索性数据分析的指南集合](https://www.kdnuggets.com/collection-of-guides-on-mastering-sql-python-data-cleaning-data-wrangling-and-exploratory-data-analysis)

+   [数据科学中数据清理的重要性](https://www.kdnuggets.com/2023/08/importance-data-cleaning-data-science.html)

+   [通过这本免费的电子书学习数据清理和预处理以进行数据科学](https://www.kdnuggets.com/2023/08/learn-data-cleaning-preprocessing-data-science-free-ebook.html)

+   [SQL中的数据清理：如何准备混乱的数据以进行分析](https://www.kdnuggets.com/data-cleaning-in-sql-how-to-prepare-messy-data-for-analysis)

+   [开始清理数据](https://www.kdnuggets.com/2022/01/getting-started-cleaning-data.html)
