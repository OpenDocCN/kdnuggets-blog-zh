# 作为数据科学家管理可重用的 Python 代码

> 原文：[https://www.kdnuggets.com/2021/06/managing-reusable-python-code-data-scientist.html](https://www.kdnuggets.com/2021/06/managing-reusable-python-code-data-scientist.html)

![图示](../Images/5a1e4b21f5d0290742182eb6a088e3ec.png)

图片由 [Chris Ried](https://unsplash.com/@cdr6934?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，出处 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

管理自己代码的方法有很多，这些方法会根据你的需求、个性、技术水平、角色和其他许多因素有所不同。虽然一位经验丰富的开发者可能会有一种极其规范的代码组织方法，适用于多种语言、项目和用例，但一位很少编写代码的数据分析师可能会因为没有必要而采用更加*临时*和随意的方法。实际上，没有对错之分，这只是看什么对你来说有效——并且合适。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 支持

* * *

具体来说，我所说的“管理代码”是指你如何组织、存储和回忆你自己编写并作为长期补充添加到编程工具箱中的不同代码。编程的核心在于自动化，因此，如果你作为代码编写者发现自己重复执行相似任务，那么自动化回忆与该任务相关的代码是很有意义的。

这就是你为什么已经在使用第三方库的原因。每次想使用时都不需要从头实现一个支持向量机代码库；相反，你可以利用一个库——比如 Scikit-learn——并利用众多专家对某些代码的不断完善。

将这一思想扩展到个人编程领域也是合情合理的。你可能已经在这样做了（我希望你在做），但如果没有，这里有一些我为管理自己可重用的 Python 代码所确定的方法，按代码使用的从最一般到最具体的顺序呈现。

### 完整的库

这是一种最通用的方法，也可以说是最“专业”的方法；然而，这本身并不意味着它总是最合适的选择。

如果你发现自己在多个使用场景中频繁使用相同的功能，并且经常这样做，那么这就是最佳方法。如果你想重用的功能容易参数化，即可以通过编写并调用一个带有每次调用时可以定义的变量的通用函数来反复处理任务，那么这也很合理。

例如，我经常发现需要查找字符串中某个子串的第n次出现，而Python标准库中没有这样的函数。因此，我有一段简单的代码，它接受一个字符串、一个子串和我要查找的第n次出现作为输入，并返回字符串中第n次出现的起始位置（很久以前从[这里](https://stackoverflow.com/questions/1883980/find-the-nth-occurrence-of-substring-in-a-string)借鉴的）。

```py
def find_nth(haystack, needle, n):
    start = haystack.find(needle)
    while start >= 0 and n > 1:
        start = haystack.find(needle, start+len(needle))
        n -= 1
    return start
```

由于我处理大量文本处理工作，我收集了许多我经常使用的文本处理函数，并创建了一个库，就像其他Python库一样存在于我的计算机上，我能够像使用其他库一样导入它。创建库的步骤虽然有点长，但很简单，因此我不会在这里详细说明，但[这篇文章](https://medium.com/analytics-vidhya/how-to-create-a-python-library-7d5aea80cc3f)是众多讲解得很好的文章之一。

现在我有了一个`textproc`库，我可以轻松地导入并使用我的`find_nth`函数，而且可以随意使用，而不必将函数复制并粘贴到每个使用它的程序中。

```py
from textproc import find_nth

segment = line[:find_nth(line, ',', 4)].strip()
```

如果我想扩展库以添加更多功能，或更改现有的`find_nth`代码，我可以在一个地方进行修改，然后重新导入。

### 项目特定的共享脚本

也许你不需要一个全面的库，因为你要重用的代码似乎只在你当前工作的项目中有用，但你确实需要在特定项目中重用它。在这种情况下，你可以将这些函数放在一个脚本中，然后按名称导入该脚本。这是简易版的库，但通常正是所需的。

在我的研究生工作中，我需要编写大量与无监督学习相关的代码，特别是k均值聚类。我编写了一些函数，用于初始化质心、计算数据点与质心之间的距离、重新计算质心等，并使用不同的算法执行这些任务。我很快发现将这些算法函数的副本保存在单独的脚本中并不是最优的，因此将它们移到自己的脚本中以供导入。它的工作方式几乎与库相同，但过程是特定路径的，仅用于该项目。

很快，我有了不同的质心初始化函数和距离计算函数的脚本，还有数据加载和处理函数的脚本。随着这些代码变得越来越参数化和普遍有用，最终这些代码被整合进了一个真正的库中。

这似乎是事物通常的进展方式，至少在我的经验中是这样：你在脚本中编写一个现在需要使用的函数，并且你使用它。项目扩展，或者你转到一个类似的项目，你意识到那个函数现在会很有用。因此，这个函数会被放到一个自己的脚本中，然后你导入它以使用。如果这种有用性在短期内持续，并且你发现这个函数有更普遍和长期的用途，那么这个函数现在会被添加到现有的库中，或者成为新库的基础。

然而，导入简单脚本的另一个具体有用的方面是使用 Jupyter notebooks。鉴于 Jupyter notebooks 中许多操作的临时性、探索性和实验性，我不太喜欢将 notebooks 导入到其他 notebooks 作为模块。如果我发现多个 notebooks 定期使用某些代码片段，那么这些代码会被放入存储在同一文件夹中的脚本中，然后导入到 notebook(s) 中。我觉得这种方法更为合理，并且通过知道一个 notebook 所依赖的另一个 notebook 没有被以有害的方式编辑，提供了更多的稳定性。

### 任务特定的模板

我发现我经常重复执行一些相同的任务，这些任务不适合被参数化，或者是可以参数化的任务，但所需的努力超过了实际的价值。在这种情况下，我采用代码模板或样板代码。这比我在本文开头想要避免的所有情况下的代码复制和粘贴要多，但有时这是正确的选择。

例如，我经常需要将 Pandas DataFrame 的内容“转化为列表”（缺乏更好的词汇），虽然编写一个能够确定列数的函数，能够接受作为输入的列等，通常输出也需要调整，这些都表明编写一个函数是非常耗时的。

在这种情况下，我只需编写一个可以轻松更改的脚本模板，并将其保存在类似模板的文件夹中。以下是 `listify_df` 的一个片段，它将 CSV 文件转换为 Pandas DataFrame，然后转换为所需的 HTML 输出。

```py
import pandas as pd

# Read CSV file into dataframe
csv_file = 'data.csv'
df = pd.read_csv(csv_file)

# Iterate over df, creating numbered list entries
i = 1
for index, row in df.iterrows():
	entry = '<b>' + str(i) + \
			'. <a href="' + \
			row['url'] + \
			'">' + \
			row['title'] + \
			'</a> + \
			'\n\n<blockquote>\n' + \
			row['description'] + \
			'\n</blockquote>\n'
	i += 1
	print(entry)
```

在这种情况下，清晰的文件名和文件夹组织有助于管理这些常用的代码片段。

### 短小的单行代码和块

最后，有很多你可能经常输入的重复代码片段。那么你为什么要这样做呢？

你应该利用文本扩展工具在需要时插入短小的“短语”。我使用[AutoKey](https://github.com/autokey/autokey)来管理这些短语，它们与触发关键词关联，当输入这些关键词时就会插入这些短语。

例如，你是否在所有特定类型的项目中导入大量相同的库？我会。举个例子，你可以通过输入，例如，`#nlpimport` 来设置完成特定任务所需的所有导入，一旦输入，这将被识别为触发关键词并替换为以下内容：

```py
import sys, requests

import numpy as np
import pandas as pd

import texthero
import scattertext as st

import spacy
from spacy.lang.en.stop_words import STOP_WORDS

from datasets import load_metric, list_metrics
from transformers import pipeline
from fastapi import FastAPI
```

需要注意的是，一些 IDE 具备这些功能。我自己通常使用升级版文本编辑器进行编码，因此 AutoKey 对我来说是必需的（且极其有用）。如果你有一个处理这方面的 IDE，那很好。关键是，你不应该需要不断重复输入这些内容。

这已经是关于作为数据科学家管理可重用 Python 代码的概述。我希望你觉得它有用。

**[Matthew Mayo](https://www.linkedin.com/in/mattmayo13/)** ([**@mattmayo13**](https://twitter.com/mattmayo13)) 是数据科学家及 KDnuggets 主编，这是一个开创性的在线数据科学和机器学习资源。他的兴趣在于自然语言处理、算法设计与优化、无监督学习、神经网络以及机器学习的自动化方法。Matthew 拥有计算机科学硕士学位和数据挖掘研究生文凭。他可以通过 editor1 at kdnuggets[dot]com 联系。

### 更多相关内容

+   [使用 Poetry 与 Conda & Pip 管理 Python 依赖](https://www.kdnuggets.com/managing-python-dependencies-with-poetry-vs-conda-pip)

+   [管理数据科学项目的4个步骤](https://www.kdnuggets.com/2022/05/4-steps-managing-data-science-project.html)

+   [管理数据科学团队的5个技巧](https://www.kdnuggets.com/5-tips-for-managing-data-science-teams)

+   [管理深度学习数据集的新方法](https://www.kdnuggets.com/2022/03/new-way-managing-deep-learning-datasets.html)

+   [通过 MLOps 管理生产中的模型漂移](https://www.kdnuggets.com/2023/05/managing-model-drift-production-mlops.html)

+   [金融科技中的 AI：管理未来的金融](https://www.kdnuggets.com/2022/10/ai-fintech-managing-finance-future.html)
