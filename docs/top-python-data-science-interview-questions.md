# 顶级 Python 数据科学面试问题

> 原文：[`www.kdnuggets.com/2021/07/top-python-data-science-interview-questions.html`](https://www.kdnuggets.com/2021/07/top-python-data-science-interview-questions.html)

评论![图](img/5a242399c92d2c76c993d13eb80a96d8.png)

图片来源：[JESHOOTS.COM](https://unsplash.com/@jeshoots?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如果你想在数据科学领域有一份职业，掌握 Python 是必须的。Python 是数据科学中最受欢迎的编程语言，尤其是在机器学习和人工智能方面。

* * *

## 我们的前三名课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

为了帮助你在数据科学职业生涯中，我准备了数据科学面试中测试的主要 Python 概念。稍后，我将讨论涵盖这些概念的两种主要面试问题类型，这些都是作为数据科学家所需了解的概念。我还会展示几个示例问题，并提供解决方案，帮助你朝正确方向前进。

### Python 面试问题的技术概念

本指南并非针对特定公司。因此，如果你有一些数据科学面试安排，我强烈建议你将此指南作为面试中可能出现内容的起点。此外，你还应该尝试寻找一些公司特定的问题，并尝试解决它们。了解一般概念并在实际问题上加以练习是成功的组合。

我不会打扰你以理论性问题。这些问题可能会在面试中出现，但它们也涵盖了编码问题中涉及的技术概念。毕竟，如果你知道如何使用我将要讲述的概念，你可能也知道如何解释它们。

数据科学工作面试中测试的 Python 技术概念包括：

1.  数据类型

1.  内置数据结构

1.  用户定义的数据结构

1.  内置函数

1.  循环和条件语句

1.  外部库（Pandas）

### 1\. 数据类型

数据类型是你应该熟悉的概念。这意味着你应该知道 Python 中最常用的数据类型，它们之间的区别，以及何时和如何使用它们。这些数据类型包括整数（int）、浮点数（float）、复数（complex）、字符串（str）、布尔值（bool）、空值（None）。

### 2\. 内置数据结构

这些是列表、字典、元组和集合。了解这四种内置数据结构将帮助你以便于访问和修改的方式组织和存储数据。

### 3\. 用户定义的数据结构

除了使用内置数据结构，你还应该能够定义和使用一些用户定义的数据结构。这些包括数组、栈、队列、树、链表、图、HashMap。

### 4\. 内置函数

Python 有超过 60 个内置函数。你不需要知道所有这些函数，当然，知道尽可能多的函数总是更好。你不能避免的内置函数包括 abs()、isinstance()、len()、list()、min()、max()、pow()、range()、round()、split()、sorted()、type()。

### 5\. 循环和条件语句

循环用于重复任务，当它们重复执行一段代码时，直到条件语句（真/假测试）告诉它们停止为止。

### 6\. 外部库（Pandas）

虽然使用了多种外部库，但 Pandas 可能是最受欢迎的。它专为金融、社会科学、统计和工程中的实际数据分析而设计。

### Python 面试问题类型

这六个技术概念主要通过两种类型的面试问题进行测试。这些问题是：

1.  数据处理和分析

1.  算法

让我们更详细地看看每个问题。

### 1\. 数据处理和分析

这些问题旨在通过解决 ETL（提取、转换和加载数据）问题并进行一些数据分析来测试上述技术概念。

这是一个[来自 Facebook 的示例](https://platform.stratascratch.com/coding-question?id=10291&python=1)：

**问题：** *Facebook 在用户尝试进行 2FA（双因素认证）登录平台时会发送短信。为了成功进行 2FA，他们必须确认收到短信。确认短信仅在发送日期有效。不幸的是，数据库存在 ETL 问题，导致好友请求和无效确认记录被插入到日志中，这些日志存储在‘fb_sms_sends’表中。这些消息类型不应存在于表中。幸运的是，‘fb_confirmers’表包含有效的确认记录，你可以使用该表来识别用户确认的短信。*

*计算 2020 年 8 月 4 日确认的短信文本的百分比。*

**回答：**

```py
import pandas as pd

import numpy as np

df = fb_sms_sends[["ds","type","phone_number"]]

df1 = df[df["type"].isin(['confirmation','friend_request']) == False]

df1_grouped = df1.groupby('ds')['phone_number'].count().reset_index(name='count')

df1_grouped_0804 = df1_grouped[df1_grouped['ds']=='08-04-2020']

df2 = fb_confirmers[["date","phone_number"]]

df3 = pd.merge(df1,df2, how ='left',left_on =["phone_number","ds"], right_on = ["phone_number","date"])

df3_grouped = df3.groupby('date')['phone_number'].count().reset_index(name='confirmed_count')

df3_grouped_0804 = df3_grouped[df3_grouped['date']=='08-04-2020']

result = (float(df3_grouped_0804['confirmed_count'])/df1_grouped_0804['count'])*100
```

测试你数据分析技能的问题之一是[这个来自 Dropbox 的问题](https://platform.stratascratch.com/coding-question?id=10308&python=1)：

**问题：** *编写一个查询，计算市场部和工程部中最高工资之间的差异。只输出工资差异。*

**回答：**

```py
import pandas as pd

import numpy as np

df = pd.merge(db_employee, db_dept, how = 'left',left_on = ['department_id'], right_on=['id'])

df1=df[df["department"]=='engineering']

df_eng = df1.groupby('department')['salary'].max().reset_index(name='eng_salary')

df2=df[df["department"]=='marketing']

df_mkt = df2.groupby('department')['salary'].max().reset_index(name='mkt_salary')

result = pd.DataFrame(df_mkt['mkt_salary'] - df_eng['eng_salary'])

result.columns = ['salary_difference']

result
```

### 2\. 算法

关于 Python 算法面试题，它们测试您使用算法解决问题的能力。由于算法不限于某一种编程语言，这些问题测试您的逻辑思维和编码能力，尤其是 Python。

例如，您可以遇到[这个问题](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)：

***问题：*** *给定一个包含 2-9 的数字的字符串，返回该数字可能代表的所有字母组合。以任何顺序返回答案。*

*下面是数字到字母的映射（就像在电话按钮上）。注意，1 不映射到任何字母。*

**答案：**

```py
class Solution:

    def letterCombinations(self, digits: str) -> List[str]:

        # If the input is empty, immediately return an empty answer array

        if len(digits) == 0: 

            return []

        # Map all the digits to their corresponding letters

        letters = {"2": "abc", "3": "def", "4": "ghi", "5": "jkl", 

                   "6": "mno", "7": "pqrs", "8": "tuv", "9": "wxyz"}

        def backtrack(index, path):

            # If the path is the same length as digits, we have a complete combination

            if len(path) == len(digits):

                combinations.append("".join(path))

                return # Backtrack            

            # Get the letters that the current digit maps to, and loop through them

            possible_letters = letters[digits[index]]

            for letter in possible_letters:

                # Add the letter to our current path

                path.append(letter)

                # Move on to the next digit

                backtrack(index + 1, path)

                # Backtrack by removing the letter before moving onto the next

                path.pop()

        # Initiate backtracking with an empty path and starting index of 0

        combinations = []

        backtrack(0, [])

        return combinations
```

或者它可能会变得更加困难，如[以下问题](https://leetcode.com/problems/sudoku-solver/solution/)所示：

***问题：*** *“编写一个程序，通过填充空单元格来解决数独谜题。数独解必须满足以下所有规则：*

1.  *每个数字 1-9 必须在每一行中各出现一次。*

1.  *每个数字 1-9 必须在每一列中各出现一次。*

1.  *每个数字 1-9 必须在网格的 9 个 3x3 子框中各出现一次。*

*‘.’ 字符表示空单元格。*

***答案：***

```py
from collections import defaultdict

class Solution:

    def solveSudoku(self, board):

        """

        :type board: List[List[str]]

        :rtype: void Do not return anything, modify board in-place instead.

        """

        def could_place(d, row, col):

            """

            Check if one could place a number d in (row, col) cell

            """

            return not (d in rows[row] or d in columns[col] or \

                    d in boxes[box_index(row, col)])        

        def place_number(d, row, col):

            """

            Place a number d in (row, col) cell

            """

            rows[row][d] += 1

            columns[col][d] += 1

            boxes[box_index(row, col)][d] += 1

            board[row][col] = str(d)            

        def remove_number(d, row, col):

            """

            Remove a number which didn't lead 

            to a solution

            """

            del rows[row][d]

            del columns[col][d]

            del boxes[box_index(row, col)][d]

            board[row][col] = '.'                

        def place_next_numbers(row, col):

            """

            Call backtrack function in recursion

            to continue to place numbers

            till the moment we have a solution

            """

            # if we're in the last cell

            # that means we have the solution

            if col == N - 1 and row == N - 1:

                nonlocal sudoku_solved

                sudoku_solved = True

            #if not yet    

            else:

                # if we're in the end of the row

                # go to the next row

                if col == N - 1:

                    backtrack(row + 1, 0)

                # go to the next column

                else:

                    backtrack(row, col + 1)                         

        def backtrack(row = 0, col = 0):

            """

            Backtracking

            """

            # if the cell is empty

            if board[row][col] == '.':

                # iterate over all numbers from 1 to 9

                for d in range(1, 10):

                    if could_place(d, row, col):

                        place_number(d, row, col)

                        place_next_numbers(row, col)

                        # if sudoku is solved, there is no need to backtrack

                        # since the single unique solution is promised

                        if not sudoku_solved:

                            remove_number(d, row, col)

            else:

                place_next_numbers(row, col)                    

        # box size

        n = 3

        # row size

        N = n * n

        # lambda function to compute box index

        box_index = lambda row, col: (row // n ) * n + col // n       

        # init rows, columns and boxes

        rows = [defaultdict(int) for i in range(N)]

        columns = [defaultdict(int) for i in range(N)]

        boxes = [defaultdict(int) for i in range(N)]

        for i in range(N):

            for j in range(N):

                if board[i][j] != '.': 

                    d = int(board[i][j])

                    place_number(d, i, j)

        sudoku_solved = False

        backtrack()
```

这将是一个相当复杂的算法，如果您知道如何解决它，那就太好了！

### 结论

对于数据科学面试，我提到的六个技术概念是必须掌握的。当然，建议您更深入地学习 Python，扩展您的知识。不仅是理论上的，还要通过解决尽可能多的数据处理和分析以及算法问题来进行实践。

对于第一个问题，您可以在[StrataScratch](https://www.stratascratch.com/)上找到大量的示例。您也许可以找到来自您申请工作的公司的问题。而且，当您决定在面试前练习编写 Python 算法时，[LeetCode](https://leetcode.com/)是一个不错的选择。

阅读我关于数据科学的其他文章：[SQL 面试问题及答案](https://cult.honeypot.io/reads/top-sql-interview-concepts-and-questions) 和 [最常见的数据科学面试问题](https://cult.honeypot.io/reads/most-common-data-science-interview-questions)！

**简历：[Nate Rosidi](https://cult.honeypot.io/contributors/nate-rosidi)** 是一名数据科学家兼产品经理。

[原文](https://cult.honeypot.io/reads/top-python-data-science-interview-questions/)。经许可转载。

**相关内容：**

+   如何应对 A/B 测试数据科学面试

+   如何在数百名数据科学候选人中脱颖而出？

+   为数据科学面试准备行为问题

### 更多相关内容

+   [KDnuggets 新闻，5 月 4 日：9 门免费哈佛课程学习数据...] (https://www.kdnuggets.com/2022/n18.html)

+   [你必须知道的 15 个数据科学 Python 编码面试问题](https://www.kdnuggets.com/2022/04/15-python-coding-interview-questions-must-know-data-science.html)

+   [3 个数据科学难度较大的 Python 编码面试问题](https://www.kdnuggets.com/2023/03/3-hard-python-coding-interview-questions-data-science.html)

+   [你必须知道的前 10 个高级数据科学 SQL 面试问题…](https://www.kdnuggets.com/2023/01/top-10-advanced-data-science-sql-interview-questions-must-know-answer.html)

+   [数据分析师的 SQL 和 Python 面试问题](https://www.kdnuggets.com/2023/02/sql-python-interview-questions-data-analysts.html)

+   [5 个 Python 面试问题及答案](https://www.kdnuggets.com/2022/09/5-python-interview-questions-answers.html)
