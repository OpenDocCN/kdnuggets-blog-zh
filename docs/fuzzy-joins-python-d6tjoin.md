# 使用 d6tjoin 在 Python 中进行模糊连接

> 原文：[`www.kdnuggets.com/2020/07/fuzzy-joins-python-d6tjoin.html`](https://www.kdnuggets.com/2020/07/fuzzy-joins-python-d6tjoin.html)

评论

**由 [Norman Niemer](https://www.linkedin.com/in/normanniemer/)，首席数据科学家**

### 合并不同的数据源是一项时间消耗大！

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持你所在的组织的 IT

* * *

从不同来源合并数据对数据科学家来说是一个巨大的时间消耗。 [d6tjoin](https://github.com/d6t/d6tjoin) 是一个 Python 库，让你可以快速高效地合并 pandas 数据框。

与 [Haijing Li](https://www.linkedin.com/in/haijing-li-7b50a11b2/) 合著，金融服务数据分析师，哥伦比亚大学商业分析硕士。

### 示例

我编造了这个例子来说明 d6tjoin 的能力。

假设我关注了几家公司的股票一段时间，并且制定了一个策略来对这些公司的表现进行 1-5 分的评分。对历史数据进行回测将帮助我评估股价是否真的反映了这些分数，并找出如何根据这些分数进行交易。我需要的回测信息包含在以下两个数据集中：df_price 包含 2019 年的历史股价，df_score 包含我定期更新的分数。

![df_price](https://github.com/HaijingLi94/d6t-python/blob/master/blogs/d6tjoin-blogs/pic/df_price.png)

![df_score](https://github.com/HaijingLi94/d6t-python/blob/master/blogs/d6tjoin-blogs/pic/df_score.png)

为了准备回测，我需要将“score”列合并到 df_price 中。显然，股票代码和日期应该是合并键。但存在两个问题：1. df_price 和 df_valuation 的“ticker”值不一致；2. 分数是按月记录的，我希望 df_price 中的每一行都分配上最近的分数，假设下一个分数在下一个更新日期之前不会出现。

### 预连接分析

d6tjoin 最好的地方之一是它提供了简单的预连接诊断。这对于检测潜在的数据问题特别有用，即使你并没有打算进行模糊连接。

```py
import d6tjoin
j = d6tjoin.Prejoin([df_price,df_score],['ticker','date'])

try:
    assert j.is_all_matched() # fails
except:
    print('assert fails!') 
```

在此之后，我们知道两个表在合并键上并非完全匹配。当你处理较大的数据集或混乱的字符串标识符时，这将更加有用，因为你无法一眼看出两个数据集是否具有不同的键值。

对于我们的情况，更有用的方法是 `Prejoin.match_quality()`。它总结了每个连接键的匹配/未匹配记录的数量。在我们的案例中，没有 ticker 名称完全匹配，且匹配的日期很少。这就是我们需要 d6tjoin 帮助进行模糊连接的原因。

```py
j.match_quality() 
```

![match_quality()](https://github.com/HaijingLi94/d6t-python/blob/master/blogs/d6tjoin-blogs/pic/match_quality.png)

### 连接具有不对齐的 ID（名称）和日期

d6tjoin 最适合对字符串、日期和数字进行最佳匹配连接。其 `MergeTop1()` 对象在 `d6tjoin.top1` 模块中非常灵活，可以让你定义如何合并：使用默认或自定义的差异函数，对多个键进行精确或模糊匹配。默认情况下，字符串的距离使用 Levenshtein 距离计算。

在我们的示例中，'ticker' 和 'date' 在两个数据集中都不对齐，因此我们需要对这两个字段进行模糊连接。

```py
result=d6tjoin.top1.MergeTop1(df_price,df_score,fuzzy_left_on=['ticker','date'],fuzzy_right_on=['ticker','date']).merge() 
```

结果以字典结构存储，包含两个键： `{'merged': 合并结果的 pandas dataframe; 'top1': 每个连接键的性能统计和总结}`

让我们来看一下结果：

```py
result['top1']['ticker']
result['merged'] 
```

第一行返回关于 'ticker' 键的性能总结。它不仅显示了左右两边匹配的键值，还显示了在 `_top1diff_` 列中计算的差异分数。默认情况下，左表中的每个键值应该与右表中的一个键值匹配。因此，即使 "SRPT-US" 和 "VRTX-US" 没有记录在 df_score 中，它们仍会与某些值匹配。我们可以通过设置 `top_limit=3` 来解决这个问题，这将使任何差异大于 3 的匹配被忽略。![top1](https://github.com/HaijingLi94/d6t-python/blob/master/blogs/d6tjoin-blogs/pic/1attempt_ticker_match_quality.png)

第二行返回合并后的数据集。![merged](https://github.com/HaijingLi94/d6t-python/blob/master/blogs/d6tjoin-blogs/pic/1attempt_result.png)

原始的 df_price 数据集仅包含 1536 行，但为什么合并后变成了 5209 行？这是因为在原始数据集中，我们将 "date" 解析为字符串。d6tjoin 默认使用 Levenshtein 距离计算字符串的差异，因此左边的一个日期可能会与右边的多个日期匹配。这告诉你每次处理日期时应首先检查其数据类型。

将 "date" 数据类型从字符串更改为 datetime 对象，并为 "ticker" 设置 `top_limit=3`。让我们再次检查结果。

```py
import datetime
df_price["date"]=df_price["date"].apply(lambda x: datetime.datetime.strptime(x,'%Y-%m-%d'))
df_score["date"]=df_score["date"].apply(lambda x: datetime.datetime.strptime(x,'%Y-%m-%d')) 
```

```py
result=d6tjoin.top1.MergeTop1(df_price,df_score,fuzzy_left_on=['ticker','date'],fuzzy_right_on=['ticker','date'],top_limit=[3,None]).merge() 
```

```py
result['top1']['ticker']
result['merged'] 
```

![top1](https://github.com/HaijingLi94/d6t-python/blob/master/blogs/d6tjoin-blogs/pic/2attempt_ticker_match_qualtiy.png)

![result](https://github.com/HaijingLi94/d6t-python/blob/master/blogs/d6tjoin-blogs/pic/2attempt_result.png)

看起来不错！左侧的所有股票代码都匹配得很好，左侧的日期与右侧的最接近日期匹配。

### 高级使用选项：传递自定义差异函数

现在我们还有一个最后的问题。记住，我们希望假设分数在分配之前不会被提供。这意味着我们希望 df_price 中的每一行都与最近的分数进行匹配。但默认情况下，d6tjoin 不考虑日期的顺序，只是与前面或后面的最近日期进行匹配。

为了解决这个问题，我们需要编写一个自定义差异函数：如果左侧的日期早于右侧的日期，则计算出的差异应足够大，以便忽略匹配。

```py
import numpy as np

def diff_customized(x,y):
    if np.busday_count(x,y)>0: #x is left_date and y is right_date,np_busday_count>0 means left_date is previous to right date
        diff_= 10000000000   #as large enough
    else:
        diff_=np.busday_count(x,y)    
    return abs(diff_) 
```

现在让我们解析自定义差异函数并查看结果。

```py
result=d6tjoin.top1.MergeTop1(df_price,df_score,fuzzy_left_on=['ticker','date'],fuzzy_right_on=['ticker','date'],fun_diff=[None,diff_customized],top_limit=[3,300]).merge() 
```

```py
result['top1']['date']
result['merged'] 
```

![top1](https://github.com/HaijingLi94/d6t-python/blob/master/blogs/d6tjoin-blogs/pic/3attempt_date_match_quality.png)

![result](https://github.com/HaijingLi94/d6t-python/blob/master/blogs/d6tjoin-blogs/pic/3attempt_result.png)

现在我们有了最终合并的数据集：未分配过分数的价格数据会被忽略，其它数据则各自分配了最近的分数。

### 结论

d6tjoin 提供的功能包括：

+   预连接诊断以识别不匹配的连接键

+   最佳匹配连接，找到最相似的错位 id、名称和日期

+   能够自定义差异函数、设置最大差异及其他高级功能

了解更多 [d6t 库](https://github.com/d6t/d6t-python)。它提供了包括以下内容在内的常见数据科学问题的解决方案：

+   [d6tflow](https://github.com/d6t/d6tflow)：构建数据科学工作流

+   [d6tjoin](https://github.com/d6t/d6tjoin)：快速模糊连接

+   [d6tpipe](https://github.com/d6t/d6tpipe)：快速分享和分发数据

**简介： [Norman Niemer](https://www.linkedin.com/in/normanniemer/)** 是一家大型资产管理公司的首席数据科学家，他提供数据驱动的投资见解。他拥有哥伦比亚大学的金融工程硕士学位和卡斯商学院（伦敦）的银行与金融学士学位。

[原文](https://github.com/HaijingLi94/d6t-python/blob/master/blogs/d6tjoin-blogs/Fuzzy%20joins%20in%20python%20with%20d6tjoin.md)。转载已获许可。

**相关：**

+   解释“黑箱”机器学习模型：SHAP 的实际应用

+   数据科学家常犯的前 10 个编码错误

+   数据科学家常犯的前 10 个统计错误

### 更多相关话题

+   [数据科学中的 SQL：理解和利用连接](https://www.kdnuggets.com/2023/08/sql-data-science-understanding-leveraging-joins.html)

+   [通过《快速 Python 数据科学》提升你的 Python 技能！](https://www.kdnuggets.com/2022/06/manning-step-python-game-fast-python-data-science.html)

+   [优化 Python 代码性能：深入了解 Python 性能分析器](https://www.kdnuggets.com/2023/02/optimizing-python-code-performance-deep-dive-python-profilers.html)

+   [Python 枚举：如何在 Python 中构建枚举](https://www.kdnuggets.com/python-enum-how-to-build-enumerations-in-python)

+   [用 Python 和 Scikit-learn 简化决策树的可解释性](https://www.kdnuggets.com/2017/05/simplifying-decision-tree-interpretation-decision-rules-python.html)

+   [Python 中的稀疏矩阵表示](https://www.kdnuggets.com/2020/05/sparse-matrix-representation-python.html)
