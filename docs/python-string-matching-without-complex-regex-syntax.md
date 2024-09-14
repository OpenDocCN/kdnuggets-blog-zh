# 没有复杂正则表达式语法的 Python 字符串匹配

> 原文：[https://www.kdnuggets.com/2023/02/python-string-matching-without-complex-regex-syntax.html](https://www.kdnuggets.com/2023/02/python-string-matching-without-complex-regex-syntax.html)

![没有复杂正则表达式语法的 Python 字符串匹配](../Images/5943947bd74ec9e16ea6fb4b391f5096.png)

图片由作者提供

我对正则表达式（RegEx）有一种爱恨交织的关系，尤其是在 Python 中。我喜欢你可以在不编写多个逻辑函数的情况下提取或匹配字符串。这比字符串搜索功能更好。

我不喜欢的是学习和理解 RegEx 模式的难度。我可以处理简单的字符串匹配，例如提取所有字母数字字符并清理 NLP 任务的文本。当涉及到从垃圾文本中提取 IP 地址、电子邮件和 ID 时，事情变得更困难。你必须编写复杂的 RegEx 字符串模式来提取所需的项。

为了简化复杂的 RegEx 任务，我们将学习一个名为 [pregex](https://pypi.org/project/pregex/) 的简单 Python 包。此外，我们还将查看一些从长文本字符串中提取日期和电子邮件的示例。

# 入门 PRegEx

[Pregex](https://pypi.org/project/pregex/) 是建立在 `re` 模块之上的高级 API。它是一种没有复杂正则表达式模式的正则表达式，使任何程序员都容易理解和记住正则表达式。此外，你不必对模式进行分组或转义元字符，它是模块化的。

你可以简单地使用 PIP 安装这个库。

```py
pip install pregex
```

为了测试 PRegEx 强大的功能，我们将使用来自 [文档](https://pregex.readthedocs.io/en/latest/introduction.html) 的修改版示例代码。

在下面的示例中，我们提取 HTTP URL 或带端口号的 IPv4 地址。我们无需为此创建复杂的逻辑。我们可以使用内置函数 `HttpUrl` 和 `IPv4`。

1.  使用 AnyDigit() 创建端口号。端口的第一位数字不能为零，接下来的三位数字可以是任何数字。

1.  使用 Either() 来添加多个逻辑，以提取 HTTP URL 或带端口号的 IP 地址。

```py
from pregex.core.pre import Pregex
from pregex.core.classes import AnyDigit
from pregex.core.operators import Either
from pregex.meta.essentials import HttpUrl, IPv4

port_number = (AnyDigit() - '0') + 3 * AnyDigit()

pre = Either(
    HttpUrl(capture_domain=True, is_extensible=True),
    IPv4(is_extensible=True) + ':' + port_number
)
```

我们将使用包含字符和描述的长文本字符串。

```py
text = """IPV4--192.168.1.1:8000--
        address--https://www.abid.works--
        website--https://www.kdnuggets.com--text"""
```

在提取匹配的字符串之前，让我们看一下 RegEx 模式。

```py
regex_pattren = pre.get_pattern()
print(regex_pattren)
```

**输出**

如我们所见，这很难阅读，甚至难以理解发生了什么。这正是 PRegEx 发挥作用的地方。它为执行复杂的正则表达式任务提供了一个用户友好的 API。

```py
(?:https?:\/\/)?(?:www\.)?(?:[a-z\dA-Z][a-z\-\dA-Z]{,61}[a-z\dA-Z]\.)*([a-z\dA-Z][a-z\-\dA-Z]{,61}[a-z\dA-Z])\.[a-z]{2,6}(?::\d{1,4})?(?:\/[!-.0-~]+)*\/?(?:(?<=[!-\/\[-`{-~:-@])|(?<=\w))|(?:(?:\d|[1-9]\d|1\d{2}|2(?:[0-4]\d|5[0-5]))\.){3}(?:\d|[1-9]\d|1\d{2}|2(?:[0-4]\d|5[0-5])):[1-9]\d{3}
```

就像 `re.match` 一样，我们将使用 `.get_matches(text)` 来提取所需的字符串。

```py
results = pre.get_matches(text)
print(results)
```

**输出**

我们已经提取了带端口号的 IP 地址和两个网页 URL。

```py
['192.168.1.1:8000', 'https://www.abid.works', 'https://www.kdnuggets.com']
```

# 示例 1：日期格式

让我们看几个示例，了解 PRegEx 的全部潜力。

在这个示例中，我们将从下面的文本中提取某些类型的日期模式。

```py
text = """
    04-15-2023
    2023-08-15
    06-20-2023
    06/24/2023
"""
```

通过使用 Exactly() 和 AnyDigit()，我们将创建日期的日、月和年。日和月是两位数字，而年是四位数字。它们由“ - ”破折号分隔。

在创建模式后，我们将运行 `get_match` 以提取匹配的字符串。

```py
from pregex.core.classes import AnyDigit
from pregex.core.quantifiers import Exactly

day_or_month = Exactly(AnyDigit(), 2) 
year = Exactly(AnyDigit(), 4)

pre = (
    day_or_month +
    "-" +
    day_or_month +
    "-" +
    year
)

results = pre.get_matches(text)
print(results)
```

**输出**

```py
['04-15-2023', '06-20-2023']
```

让我们通过使用 `get_pattern()` 函数来查看 RegEx 模式。

```py
regex_pattren = pre.get_pattern()
print(regex_pattren)
```

**输出**

如我们所见，它具有简单的 RegEx 语法。

```py
\d{2}-\d{2}-\d{4}
```

# 示例 2: 电子邮件提取

第二个示例有点复杂，我们将从垃圾文本中提取有效的电子邮件地址。

```py
text = """
    user1@abid.works
    editorial@@kdnuggets.com
    lover@python.gg.
    editorial1@kdnuggets.com
    """
```

+   使用 `OneOrMore()` 创建一个**用户**模式。我们将使用 `AnyButFrom()` 从逻辑中去除“@”和空格。

+   类似于**用户**模式，我们通过从逻辑中去除额外的字符“.”来创建一个**公司**模式。

+   对于**域名**，我们将使用 `MatchAtLineEnd()` 从行尾开始搜索，匹配任意两个或更多字符，除了“@”、空格和句点。

+   将三者结合以创建最终模式：**user@company.domain**。

```py
from pregex.core.classes import AnyButFrom
from pregex.core.quantifiers import OneOrMore, AtLeast
from pregex.core.assertions import MatchAtLineEnd

user = OneOrMore(AnyButFrom("@", ' '))
company = OneOrMore(AnyButFrom("@", ' ', '.'))
domain = MatchAtLineEnd(AtLeast(AnyButFrom("@", ' ', '.'), 2))

pre = (
    user +
    "@" +
    company +
    '.' +
    domain
)

results = pre.get_matches(text)
print(results)
```

**输出**

如我们所见，PRegEx 已经识别出两个有效的电子邮件地址。

```py
['user1@abid.works', 'editorial1@kdnuggets.com']
```

> **注意：** 两个代码示例是 [The PyCoach](https://towardsdatascience.com/pregex-regular-expressions-in-plain-english-in-python-4670425d0eb5) 工作的修改版本。

# 结论

如果你是数据科学家、分析师或 NLP 爱好者，你应该使用 PRegEx 来清理文本并创建简单的逻辑。这将减少你对 NLP 框架的依赖，因为大部分匹配可以通过简单的 API 完成。

在这个迷你教程中，我们学习了 Python 包 PRegEx 及其用例。你可以通过阅读官方 [文档](https://pregex.readthedocs.io/en/latest/introduction.html) 或使用可编程正则表达式解决一个 [wordle](https://pregex.readthedocs.io/en/latest/introduction.html#solving-wordle-with-pregex) 问题来了解更多。

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan)) 是一位认证的数据科学专家，热爱构建机器学习模型。目前，他专注于内容创作和撰写关于机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是使用图神经网络为遭受心理疾病困扰的学生开发一个 AI 产品。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在的组织的 IT

* * *

### 更多相关主题

+   [Python 字符串处理速查表](https://www.kdnuggets.com/2020/01/python-string-processing-primer.html)

+   [Python 字符串方法](https://www.kdnuggets.com/2022/12/python-string-methods.html)

+   [将字节转换为 Python 字符串：初学者教程](https://www.kdnuggets.com/convert-bytes-to-string-in-python-a-tutorial-for-beginners)

+   [Python 基础：语法、数据类型和控制结构](https://www.kdnuggets.com/python-basics-syntax-data-types-and-control-structures)

+   [如何在没有任何工作经验的情况下获得数据科学的第一份工作](https://www.kdnuggets.com/2021/02/first-job-data-science-without-work-experience.html)

+   [在图像中找到一张图片而不做标记](https://www.kdnuggets.com/2022/09/find-picture-image-without-marking.html)
