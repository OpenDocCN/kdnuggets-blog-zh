# 5 个 Python 面试问题与答案

> 原文：[https://www.kdnuggets.com/2022/09/5-python-interview-questions-answers.html](https://www.kdnuggets.com/2022/09/5-python-interview-questions-answers.html)

![5 个 Python 面试问题与答案](../Images/187fb33af80c3f5d46a3763f7fd398e9.png)

图片来源：[ThisIsEngineering](https://www.pexels.com/photo/person-using-gray-laptop-3861964/)

想象你坐在笔记本电脑前，听取招聘经理的指示并阅读编码问题陈述。如果你没有为定时编码面试做好准备，你将会犯错误。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT 方面的需求

* * *

因此，在参加面试之前，复习最常见的问题总是好的。这将为你准备好棘手、条件性和高级的编码问题。

# Q1: 奇数还是偶数？确定一下！

确定“n”个连续数字的和是偶数、奇数还是任意。

**示例：**

+   odd_or_even(3) 应返回“Either”。总和可以是偶数或奇数。

+   odd_or_even(2) 应返回“Odd”。连续数字的和包含一个奇数和一个偶数，因此总和将始终是奇数。

+   odd_or_even(4) 应返回“Even”。连续数字的和包含两个偶数和两个奇数，因此总和将始终是偶数。

**解决方案：**

1.  如果将“n”除以 2 后余数为 1，则返回“Either”。

1.  如果第一个条件失败，请转到第二个条件。

1.  首先，将数字除以 2，然后再除以 2 以检查是否返回余数。如果余数为 1，则返回“Odd”，否则返回“Even”

```py

def odd_or_even(n):
    if n % 2 == 1: 
        return "Either"
    elif (n / 2) % 2 == 1:
        return "Odd"
    else:
        return "Even"

```

为了在第一次尝试中通过此问题，您需要多次阅读描述。答案已经在示例中给出。

这并不是唯一的解决方案。你可以在 Codewars 平台上找到各种解决方案。你甚至可以提出自己独特的解决方案以获得更多积分。

# Q2: 元组和列表有什么区别？

元组和列表都用于存储和收集数据。

列表使用方括号 [1,2,3,..] 声明，而元组使用圆括号 (1,2,3,...) 声明。

**列表**是可变的，主要用于数据插入和删除。它还具有多个内置函数。

**元组**是不可变的。与列表相比，它的迭代速度较快，占用的内存较少。它主要用于快速访问元素。

# Q3: 什么是 Lambda 函数？为什么使用它们？

Lambda函数通常称为匿名函数。它们没有名称。

当表达式适合一行时，它可以作为常规函数的替代。Lambda函数非常简单且轻量。它们提高了代码质量。

要创建一个简单的函数，将20添加到输入参数中，我们将编写lambda并提供两个组件：参数和表达式。

```py
lambda arguments : expression
```

参数是“a”，表达式是“a+20”。可以通过提供输入给“x”变量来激活函数。

就这么简单！

```py
x = lambda a : a + 20
print(x(10))
>>> 30
```

我们还可以向函数提供多个输入参数。在我们的例子中，它将2和5相乘以输出10。

```py
x = lambda a, b : a * b
print(x(2, 5))
>>> 10
```

# Q4：最大69数字

你得到一个正整数“num”。它仅由6和9数字组成。

通过更改一个数字（6或9）来返回最大数字。

**示例：**

+   输入：num = 9669，输出：9969

+   输入：num = 9996，输出：9999

+   输入：num = 9999，输出：9999

**解决方案：**

为了得到最大数字，我们必须始终将6改为9，并且应该是第一个数字（6）。

假设我们有num = 9669

+   更改第一个数字将得到6669。

+   更改第二个数字将得到9969。

+   更改第三个数字将得到9699。

+   更改第四个数字将得到9666。

**9969** 是最高的数字。

我们只需要创建一个简单的函数，在开始时将一个6转换为9。

为此，我们将数字转换为字符串，并使用`.replace`函数将“6”替换为“9”。第三个参数是“count”为1。它只会替换一个数字。替换后，我们将字符串转换为整数。

```py
def maximum69Number(num):
    return int(str(num).replace('6','9',1))
num = 966696696

maximum69Number(num)
>>> 996696696
```

# Q5：谁先获胜？

微软编码面试中的问题：

“*艾米和布拉德轮流掷一个公平的六面骰子。谁先掷到“6”谁就赢。艾米先掷。艾米获胜的概率是多少？*”

在我们进入解决方案部分之前，我希望你再读一遍问题。这是一个模拟问题，我们将找到艾米获胜的概率。

艾米总是先掷骰子，如果她第一次掷到6就赢了。否则，布拉德掷骰子，如果他掷到6就赢了。如果没有，则轮到艾米，直到其中一人获胜。

这个问题完全是关于迭代和逻辑的。

1.  将A_count和B_count初始化为零。我们将使用它们来计算概率。

1.  之后，我们将对“size”进行模拟。在我们的例子中，它是1000。

1.  利用Numpy的random randint函数，我们将生成一个从1到6的数字。randint函数需要低（包含）和高（不包含）数字。你甚至可以使用random库代替Numpy。

1.  如果随机数是6，则A_count（艾米）加一，否则布拉德再掷一次。

1.  如果B_role掷到6，则B_count（布拉德）加一。否则，转到艾米。

1.  这个循环将运行一千次，我们将收集艾米和布兰德的胜利来计算概率。

1.  为了计算艾米获胜的概率，我们将把艾米掷出6的次数除以艾米和布拉德的总获胜次数。

```py
import numpy as np

def win_probability(size):

    A_count = 0 
    B_count = 0 

    for i in range(size): 
        A_role = np.random.randint(1,7)

        if A_role == 6:       
            A_count+=1     
        else:              
            B_role = np.random.randint(1,7)
            if B_role == 6:   
                B_count+=1
    return A_count/(A_count+B_count)
```

为了确保结果的可重复性，我们将使用Numpy的种子。

```py
np.random.seed(125)
win_probability(1000)
>>> 0.5746031746031746
```

正如我们所见，艾米由于先开始，较布拉德更占优势。她在1000次迭代中的胜率为57.5%。

## 参考资料

1.  奇数还是偶数？来确定一下吧！| [Codewars](https://www.codewars.com/kata/619f200fd0ff91000eaf4a08/train/python)

1.  元组和列表有什么区别？ | [Codecademy](https://www.codecademy.com/resources/blog/python-interview-questions-practice/)

1.  什么是lambda函数？它们为什么会被使用？| [Codecademy](https://www.codecademy.com/resources/blog/python-interview-questions-practice/)

1.  最大69数字 | [LeetCode](https://leetcode.com/problems/maximum-69-number/)

1.  谁先赢得比赛？| [Leihua Ye, PhD](https://towardsdatascience.com/5-python-coding-questions-asked-at-faang-59e6cf5ba2a0)

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan)) 是一位认证的数据科学专业人士，热衷于构建机器学习模型。目前，他专注于内容创作和撰写有关机器学习和数据科学技术的技术博客。Abid拥有技术管理硕士学位和电信工程学士学位。他的愿景是利用图神经网络为那些在心理健康方面挣扎的学生构建一个AI产品。

### 进一步了解此主题

+   [7个数据分析面试问题及答案](https://www.kdnuggets.com/2022/09/7-data-analytics-interview-questions-answers.html)

+   [检测虚假数据科学家的20个问题（附答案）：ChatGPT…](https://www.kdnuggets.com/2023/01/20-questions-detect-fake-data-scientists-chatgpt-1.html)

+   [检测虚假数据科学家的20个问题（附答案）：ChatGPT…](https://www.kdnuggets.com/2023/02/20-questions-detect-fake-data-scientists-chatgpt-2.html)

+   [数据科学中你必须知道的15个Python编码面试问题](https://www.kdnuggets.com/2022/04/15-python-coding-interview-questions-must-know-data-science.html)

+   [数据分析师的SQL和Python面试问题](https://www.kdnuggets.com/2023/02/sql-python-interview-questions-data-analysts.html)

+   [数据科学中的3个难题Python编码面试问题](https://www.kdnuggets.com/2023/03/3-hard-python-coding-interview-questions-data-science.html)
