# 如何使用 pivot_table 函数进行高级数据汇总

> 原文：[https://www.kdnuggets.com/how-to-use-the-pivot_table-function-for-advanced-data-summarization-in-pandas](https://www.kdnuggets.com/how-to-use-the-pivot_table-function-for-advanced-data-summarization-in-pandas)

![如何使用 pivot_table 函数进行高级数据汇总](../Images/bdc49066c47305f4041269ca1806327b.png)

图片来源：作者 | Midjourney

让我指导你如何使用 Pandas 的 `pivot_table` 函数进行数据汇总。

## 准备工作

让我们开始安装必要的软件包。

```py
pip install pandas seaborn
```

然后，我们将加载软件包和数据集示例，即 Titanic。

```py
import pandas as pd
import seaborn as sns

titanic = sns.load_dataset('titanic')
```

在成功安装包并加载数据集后，让我们进入下一部分。

## 使用 Pandas 创建数据透视表

Pandas 中的数据透视表允许灵活的数据重组和分析。让我们从简单的应用开始探讨一些实际应用。

```py
pivot = pd.pivot_table(titanic, values='age', index='class', columns='sex', aggfunc='mean')
print(pivot)
```

```py
Output>>>
sex        female       male
class                       
First   34.611765  41.281386
Second  28.722973  30.740707
Third   21.750000  26.507589
```

生成的数据透视表显示了平均年龄，乘客类别在纵轴上，性别类别在顶部。

我们可以更进一步，使用数据透视表来计算票价的均值和总和。

```py
pivot = pd.pivot_table(titanic, values='fare', index='class', columns='sex', aggfunc=['mean', 'sum'])
print(pivot)
```

```py
Output>>>
             mean                   sum           
sex         female       male     female       male
class                                              
First   106.125798  67.226127  9975.8250  8201.5875
Second   21.970121  19.741782  1669.7292  2132.1125
Third    16.118810  12.661633  2321.1086  4393.5865
```

我们可以创建自己的函数。例如，我们创建一个函数，该函数计算数据的最大值和最小值之间的差异，并将其除以二。

```py
def data_div_two(x):
    return (x.max() - x.min())/2

pivot = pd.pivot_table(titanic, values='age', index='class', columns='sex', aggfunc=data_div_two)
print(pivot)
```

```py
Output>>>
sex     female    male
class                 
First   30.500  39.540
Second  27.500  34.665
Third   31.125  36.790
```

最后，你可以添加边际数据，查看总体分组平均值和具体子组之间的差异。

```py
pivot = pd.pivot_table(titanic, values='age', index='class', columns='sex', aggfunc='mean', margins=True)
print(pivot)
```

```py
Output>>>
sex        female       male        All
class                                  
First   34.611765  41.281386  38.233441
Second  28.722973  30.740707  29.877630
Third   21.750000  26.507589  25.140620
All     27.915709  30.726645  29.699118
```

掌握 `pivot_table` 函数将帮助你从数据集中获得洞察。

## 额外资源

+   [掌握 Pandas 和 Python 的 7 个步骤](https://www.kdnuggets.com/7-steps-to-mastering-data-wrangling-with-pandas-and-python)

+   [数据科学家应知道的 10 个必备 Pandas 函数](https://www.kdnuggets.com/10-essential-pandas-functions-every-data-scientist-should-know)

+   [使用 Pandas 处理数据](https://machinelearningmastery.com/massaging-data-using-pandas/)

**[](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**** 是数据科学助理经理和数据撰稿人。在 Allianz 印度尼西亚全职工作时，他喜欢通过社交媒体和写作分享 Python 和数据技巧。Cornellius 涉及多种 AI 和机器学习主题的写作。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

### 了解更多相关主题

+   [初学者的 Pandas Melt 函数指南](https://www.kdnuggets.com/2023/03/beginner-guide-pandas-melt-function.html)

+   [文本总结方法：概述](https://www.kdnuggets.com/2019/01/approaches-text-summarization-overview.html)

+   [自动文本总结入门](https://www.kdnuggets.com/2019/11/getting-started-automated-text-summarization.html)

+   [使用 GPT-3 进行总结](https://www.kdnuggets.com/2022/04/packt-summarization-gpt3.html)

+   [文本总结开发：基于 GPT-3.5 的 Python 教程](https://www.kdnuggets.com/2023/04/text-summarization-development-python-tutorial-gpt35.html)

+   [通过密度链提示解锁 GPT-4 总结功能](https://www.kdnuggets.com/unlocking-gpt-4-summarization-with-chain-of-density-prompting)
