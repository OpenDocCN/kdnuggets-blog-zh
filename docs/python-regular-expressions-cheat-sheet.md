# Python 正则表达式备忘单

> 原文：[`www.kdnuggets.com/2018/04/python-regular-expressions-cheat-sheet.html`](https://www.kdnuggets.com/2018/04/python-regular-expressions-cheat-sheet.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**由 [Alex Yang](https://twitter.com/alexalexyang) 提供，Dataquest**

这个备忘单基于 Python 3 的 [正则表达式文档](https://docs.python.org/3/library/re.html)。如果你有兴趣学习 Python，我们有一个免费的 [Python 编程：初学者](https://www.dataquest.io/course/python-programming-beginner) 课程供你尝试。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

![python-regular-expressions-cheatsheet_pic](https://www.dataquest.io/blog/large_files/python-regular-expressions-cheat-sheet.pdf)

[在这里下载备忘单](https://www.dataquest.io/blog/large_files/python-regular-expressions-cheat-sheet.pdf)

### 特殊字符

`^` | 匹配其右侧的表达式在字符串的开头。它匹配每个字符串中每个`\n`之前的此类实例。

`$` | 匹配其左侧的表达式在字符串的末尾。它匹配每个字符串中每个`\n`之前的此类实例。

`.` | 匹配除行结束符（如`\n`）之外的任何字符。

`\` | 转义特殊字符或表示字符类。

`A|B` | 匹配表达式`A`或`B`。如果`A`首先匹配，则`B`将不会被尝试。

`+` | 贪婪地匹配其左侧的表达式 1 次或多次。

`*` | 贪婪地匹配其左侧的表达式 0 次或多次。

`?` | 贪婪地匹配其左侧的表达式 0 次或 1 次。但是，如果`?`被添加到限定符（`+`、`*`和`?`本身）中，它将以非贪婪的方式进行匹配。

`{m}` | 匹配其左侧的表达式`m`次，并不少于。

`{m,n}` | 匹配其左侧的表达式`m`到`n`次，并不少于。

`{m,n}?` | 匹配其左侧的表达式`m`次，并忽略`n`。参见上面的`?`。

### 字符类（即特殊序列）

`\w` | 匹配字母数字字符，即`a-z`、`A-Z`和`0-9`。它还匹配下划线`_`。

`\d` | 匹配数字，即`0-9`。

`\D` | 匹配任何非数字字符。

`\s` | 匹配空白字符，包括`\t`、`\n`、`\r`和空格字符。

`\S` | 匹配非空白字符。

`\b` | 匹配单词的起始和结束处的边界（或空字符串），即 `\w` 和 `\W` 之间。

`\B` | 匹配 `\b` 不匹配的地方，即 `\w` 字符的边界。

`\A` | 匹配其右侧的表达式在字符串的绝对开始处，无论是单行模式还是多行模式。

`\Z` | 匹配其左侧的表达式在字符串的绝对结束处，无论是单行模式还是多行模式。

### 集合

`[ ]` | 包含一个字符集合以进行匹配。

`[amk]` | 匹配 `a`、`m` 或 `k`。它不匹配 `amk`。

`[a-z]` | 匹配从 `a` 到 `z` 的任何字母。

`[a\-z]` | 匹配 `a`、`-` 或 `z`。它匹配 `-` 因为 `\` 对其进行转义。

`[a-]` | 匹配 `a` 或 `-`，因为 `-` 没有用来表示字符系列。

`[-a]` | 同上，匹配 `a` 或 `-`。

`[a-z0-9]` | 匹配从 `a` 到 `z` 和从 `0` 到 `9` 的字符。

`[(+*)]` | 特殊字符在集合内变为字面量，因此这匹配 `(`、`+`、`*` 和 `)`。

`[^ab5]` | 添加 `^` 排除集合中的任何字符。这里，它匹配不是 `a`、`b` 或 `5` 的字符。

### 组

`( )` | 匹配括号内的表达式并将其分组。

`(? )` | 在这样的括号内，`?` 作为扩展符号。其含义取决于其右侧的字符。

`(?PAB)` | 匹配表达式 `AB`，并可以通过组名访问。

`(?aiLmsux)` | 这里，`a`、`i`、`L`、`m`、`s`、`u` 和 `x` 是标志：

+   `a` — 仅匹配 ASCII

+   `i` — 忽略大小写

+   `L` — 区域依赖

+   `m` — 多行

+   `s` — 匹配所有

+   `u` — 匹配 Unicode

+   `x` — 详细

`(?:A)` | 匹配由 `A` 表示的表达式，但与 `(?PAB)` 不同，它无法在之后被检索。

`(?#...)` | 注释。内容供我们阅读，不用于匹配。

`A(?=B)` | 向前断言。只有在 `B` 之后，才匹配表达式 `A`。

`A(?!B)` | 向前否定断言。只有在不跟随 `B` 的情况下，才匹配表达式 `A`。

`(?<=B)A` | 正向回顾断言。只有在 `B` 紧接其左侧时，才匹配表达式 `A`。这只能匹配固定长度的表达式。

`(?<!B)A` | 向后否定断言。只有在 `B` 不紧接其左侧时，才匹配表达式 `A`。这只能匹配固定长度的表达式。

`(?P=name)` | 匹配由早期名为“name”的组匹配的表达式。

`(...)\1` | 数字 `1` 对应第一个匹配的组。如果我们想匹配相同表达式的更多实例，只需使用其数字而不是重新书写整个表达式。我们可以使用从 `1` 到 `99` 的这些组及其对应的数字。

### 流行的 Python re 模块函数

`re.findall(A, B)` | 匹配字符串 `B` 中表达式 `A` 的所有实例，并将它们以列表形式返回。

`re.search(A, B)` | 匹配字符串 `B` 中表达式 `A` 的第一个实例，并返回它作为 re 匹配对象。

`re.split(A, B)` | 使用分隔符 `A` 将字符串 B 拆分成列表。

`re.sub(A, B, C)` | 在字符串`C`中将`A`替换为`B`。

**Python 用户的有用正则表达式网站**

+   [Python 3 re 模块文档](https://docs.python.org/3/library/re.html)

+   [在线正则表达式测试器和调试器](https://regex101.com/)

**简介: [Alex Yang](https://twitter.com/alexalexyang)** 是一位对代码能做的事情充满兴趣的作家。他还喜欢公民科学和新媒体艺术。

[原文](https://www.dataquest.io/blog/regex-cheatsheet/?utm_source=kdnuggets&utm_medium=blog)。转载经许可。

**相关：**

+   30 份重要的数据科学、机器学习与深度学习备忘单

+   Python 中的函数式编程介绍

+   文本数据预处理：Python 中的操作指南

### 更多相关主题

+   [用 Python 掌握正则表达式](https://www.kdnuggets.com/2023/08/mastering-regular-expressions-python.html)

+   [数据清洗中使用正则表达式的 5 个技巧](https://www.kdnuggets.com/5-tips-for-using-regular-expressions-in-data-cleaning)

+   [成为优秀数据科学家所需的 5 项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应掌握的 6 种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021 年最佳 ETL 工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [使用管道编写干净的 Python 代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)
