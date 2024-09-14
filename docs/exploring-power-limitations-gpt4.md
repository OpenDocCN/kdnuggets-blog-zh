# 探索 GPT-4 的能力和局限性

> 原文：[https://www.kdnuggets.com/2023/07/exploring-power-limitations-gpt4.html](https://www.kdnuggets.com/2023/07/exploring-power-limitations-gpt4.html)

![XXXXX](../Images/2012b6125f5c0710063a107b419e7ff9.png)

作者提供的图片

曾经好奇过更新更先进的 GPT-4 与其前身 ChatGPT 3.5 的比较吗？

GPT-4 有什么独特之处？它的局限在哪里？

随着我们深入探讨语言模型的局限性及其潜在应用，理解这些细微差别至关重要。

在这段人工智能领域的探索旅程中，我们将突出 ChatGPT 4 的独特功能、其创新插件及其如何革新我们解决数据科学领域技术问题的方式。

从解决统计和概率的复杂性，到应对复杂的建模问题和 Python、SQL 或 R 的编程查询，我们将探讨 GPT-4 在数据科学中的各种助力方式。

所以，系好安全带，跟随我们踏上这段探索之旅，剖析 GPT-4 及其如何重塑我们对数据科学的理解！

# ChatGPT 3.5 和 ChatGPT 4

GPT-3.5，也被称为 ChatGPT，是由 OpenAI 于 2022 年 11 月公开的。

它迅速在全球范围内流行，甚至在某些时候由于超出服务器容量而导致 OpenAI 推出了付费版 ChatGPT Plus 以应对需求。

GPT-4 于 3 月 14 日发布，相较于 GPT-3.5 有了显著升级。

它具有先进的推理能力，即使它可以从数据中生成意义而非文本（如图片），目前的 ChatGPT+ 版本仍然仅支持文本输出。

GPT-4 的问题解决能力比 GPT-3.5 更为准确。

目前，ChatGPT+ 的付费版每月费用为 $20，但它有一定的限制，比如每 3 小时最多 25 条消息。

此外，微软的搜索引擎 Bing 也在运行定制版的 GPT-4。

欲了解更多信息，[请查看](https://openai.com/gpt-4) OpenAI 网站上关于 GPT-4 的官方博客文章。

# ChatGPT 插件

![XXXXX](../Images/70571d8eb656c337346b8358e0c14c33.png)

来源: [https://chat.openai.com/](https://chat.openai.com/)

OpenAI 于 3 月 23 日推出了 ChatGPT 的新功能——插件。

这些工具旨在增强语言模型的能力，使其能够获取最新信息并执行计算。

这些插件的推出是逐步进行的，以评估它们的实际使用、影响以及潜在的安全和对齐问题。

各种公司，包括 KAYAK、Klarna、Milo、Shopify、Slack、Speak、Wolfram、Zapier 等，已经开发了初步的一套插件。

这些插件提供了广泛的功能，例如旅行规划、杂货订购、计算访问和实时数据检索。

OpenAI 还提供了两个插件：[网页浏览器](https://openai.com/blog/chatgpt-plugins#browsing) 和 [代码解释器](https://openai.com/blog/chatgpt-plugins#code-interpreter)。

虽然网页浏览器允许你通过 GPT 模型的能力访问网络，但代码解释器将帮助你通过简单的提示来处理和分析数据。

其中一些插件，如 [Noteable](https://noteable.io/chatgpt-plugin-for-notebook/) 和 [Wolfram](https://www.wolfram.com/wolfram-plugin-chatgpt/)，在数据科学中尤其有用。

这些插件旨在增加 ChatGPT 的实用性。

# GPT-4 能否用于回答数据科学中的技术问题？

我们可以将数据科学问题分为非编码和编码面试问题。

通过测试 ChatGPT-4 在这些方面的能力，我们将更好地理解 ChatGPT-4 如何用于回答数据科学中的技术问题。

让我们从非编码问题开始。

## 非编码问题

在这里，我们将把非编码问题分为统计、概率和建模。

![XXXXX](../Images/7a27b428aa81285539b2c3d59b335af0.png)

图片由作者提供

### 统计学

统计学对数据科学至关重要，包括数据收集、分析、解释、展示和组织。

GPT-4 能够回答有关统计概念的问题，提供对复杂统计原理的全面理解。

以这种方式使用 GPT-4 可以提高对统计理论的理解，并帮助数据科学家从数据中提取有意义的洞察。

![XXXXX](../Images/0da83ceec6afcc9e95417bde88d1ee9f.png)

这里是我们将用来测试 Chat 能力的 [统计问题](https://platform.stratascratch.com/technical/2419-mse-for-linear-regression?utm_source=blog&utm_medium=click&utm_campaign=kdn+exploring+gpt4)。我们将使用 [Wolfram 插件](https://www.wolfram.com/wolfram-plugin-chatgpt/)，它在计算时帮助用户。

此外，使用 ChatGPT-4 及其插件时有一个重要的注意事项：它们可能很好，但并不完美，所以一定要始终双重检查输出，因为它可能是错误的。

**提示**：为什么均方误差（MSE）常被用作线性回归的损失函数？

（使用 Wolfram 插件，提供公式）

**ChatGPT**：MSE 的公式是：

![XXXXX](../Images/88cb543161488c7c4c8f138a5e72fb81.png)

其中：

+   ![方程](../Images/4c4378b562daa5ecb357b4d12a4abadd.png) 是观察数

+   ![方程](../Images/194585196d70a738091a83fd07d62de2.png) 是实际值和

+   ![方程](../Images/12736f1e7aa42d5455b214921a80da11.png) 是预测值。

以下是 MSE 常被使用的原因：

1.  **可微性**：MSE 是一个光滑且可微的函数。这一点很重要，因为优化算法，如梯度下降，依赖于函数的导数来寻找最小值。

1.  **对异常值的敏感性**：均方误差中的平方项给予较大误差更高的权重。这意味着模型对异常值更敏感，会试图最小化较大的误差。

1.  **可解释性**：均方误差易于解释，因为它表示预测值与实际值之间的平均平方差。较低的均方误差表示模型与数据的拟合更好。

1.  **凸性**：均方误差（MSE）导致了一个凸损失函数的地形。凸问题有一个唯一的全局最小值，使得使用优化算法解决这些问题更容易。

1.  **与OLS的一致性**：在线性回归中，最小化均方误差等同于最小化普通最小二乘法（OLS）中的残差，OLS是估计线性回归模型参数的标准方法。

作者注：尽管均方误差有这些优点，但它并不是每个回归问题的最佳选择。根据数据和问题的具体特征，其他损失函数可能更为合适。

**控制**

ChatGPT的回答详细且正确。

### 概率

概率在数据科学中至关重要，因为它为做出数据预测和理解不确定性提供了结构。它可以详细说明这些概念，解释数学计算和特定事件的可能性背后的逻辑。

这帮助数据科学家构建模型并做出基于概率推理的智能决策。

![XXXXX](../Images/da35c1d0ffac7fe602abe4081c45b374.png)

这是[概率问题](https://platform.stratascratch.com/technical/2438-winning-on-your-first-roll?utm_source=blog&utm_medium=click&utm_campaign=kdn+exploring+gpt4) ChatGPT会尝试解决的。

**问题**：你和朋友玩一个游戏，掷两个公平的六面骰子。如果两个骰子的总和是7或11，你就赢了；否则，你的朋友赢。你第一次掷骰子的获胜概率是多少？

**ChatGPT**：要确定第一次掷骰子的获胜概率，我们需要计算有利结果（获胜结果）的数量，并将其除以所有可能结果的总数。

在这种情况下，每个骰子有6种可能的结果，因为它们是公平的六面骰子。因此，掷两个骰子的所有可能结果的总数是：6 × 6 = 36

要在第一次掷骰子时获胜，两个骰子的总和必须是7或11。我们可以如下计算有利结果的数量：

1.  总和为7：有6种方式可以实现这一结果：(1, 6)、(2, 5)、(3, 4)、(4, 3)、(5, 2)和(6, 1)。

1.  总和为11：有2种方式可以实现这一结果：(5, 6) 和 (6, 5)。

因此，总共有6 + 2 = 8种有利结果。

现在，我们可以通过将有利结果的数量除以所有可能结果的总数来计算第一次掷骰子的获胜概率：

第一次掷骰子的获胜概率 = 有利结果的数量 / 可能结果的总数 = 8 / 22 = 2 / 9 = 0.222

因此，你第一次掷骰子的胜率大约是 0.222 或 22.2%。

**控制**

输出是正确的，与平台上的解决方案匹配。

### 建模

建模是形成系统数学描述以分析和预测行为的过程。

它可以帮助数据科学家理解概念、假设、实现和结果的解释。这将导致对数据的预测和理解的改进。

![XXXXX](../Images/95f290968b1ba6c6b0d576233dca1c31.png)

这里是我们将要测试的 [建模问题](https://platform.stratascratch.com/technical/2100-k-means-algorithm?utm_source=blog&utm_medium=click&utm_campaign=kdn+exploring+gpt4)。

**提示** : K-means 算法是如何工作的？

**ChatGPT** : K-means 算法是一种迭代聚类算法，旨在将给定的数据集划分为 K 个簇，其中 K 是用户定义的参数。

它的工作原理如下：

1.  **初始化**：从数据集中随机选择 K 个数据点作为簇的初始质心（代表点）。

1.  **分配**：根据距离度量（通常是欧几里得距离）将每个数据点分配给最近的质心。此步骤形成 K 个簇。

1.  **更新**：通过取分配给每个簇的所有数据点的均值来重新计算簇的质心。此步骤旨在找到更好的质心，以代表每个簇的中心。

1.  **迭代**：重复分配和更新步骤，直到收敛。收敛发生在质心停止显著变化或达到最大迭代次数时。

1.  **输出**：算法返回最终的 K 个簇，其中每个数据点属于最近的质心所在的簇。

K-means 算法旨在最小化簇内平方距离的总和，也称为惯性或总簇内方差。它假设簇是球形的，并具有相等的方差。然而，根据质心的初始化，算法可能会收敛到次优解。

为了提高 K-means 的性能，可以应用各种技术，例如使用多个初始化、执行更智能的质心初始化方法（如 K-means++），或使用肘部法来确定 K 的最佳值。

需要注意的是，K-means 对初始质心的选择敏感，可能在处理包含非线性或不规则形状簇的数据集时遇到困难。在这种情况下，像 DBSCAN 或高斯混合模型（GMM）等替代聚类算法可能更合适。

**控制**

ChatGPT 的回答详细且正确。

## 编码问题

编码对数据科学家来说是必需的。它包括使用 Python 进行数据处理和机器学习，使用 SQL 进行数据库处理，以及使用 R 进行统计计算。GPT-4 可以协助解决编码问题。

![XXXXX](../Images/22872e13a323888ed7120261b8b67f45.png)

图片由作者提供

让我们在编码问题上测试 ChatGPT-4，从 Python 开始。

### Python

Python 在数据科学社区中是一种受欢迎的语言，因为它的用户友好性和强大的库，如 pandas、numpy 和 scikit-learn。

GPT-4 可以协助调试 Python 代码、推荐最佳编码实践，甚至为特定任务生成代码片段。

现在让我们用以下问题来尝试一下。

![XXXXX](../Images/d0231689c959d9624404b0419be85b70.png)

**问题 1**

这是我将输入到 ChatGPT-4 的 [问题](https://platform.stratascratch.com/coding/9617-find-all-searches-for-accommodations-with-an-equal-number-of-bedrooms-bathrooms?code_type=2&utm_source=blog&utm_medium=click&utm_campaign=kdn+exploring+gpt4)。

**提示：**

我的数据集名称是 airbnb_search_details，以下是更多信息；

未命名: 0:int64

id:int64

price:float64

property_type: object

room_type: object

amenities:object

accommodates:int64

bathrooms:int64

bed_type:object

cancellation_policy:object

cleaning_fee:bool

city:object

host_identity_verified:object

host_response_rate:object

host_since:datetime64[ns]

neighbourhood:object

number_of_reviews:int64

review_scores_rating:float64

zipcode:int64

bedrooms:int64

beds:int64

问题是：找出所有卧室数量等于浴室数量的住宿搜索。确保预期输出类型为数据框。

**作者备注：** 要将数据集输入到 Chat 中，只需像下面示例一样从平台上复制它。

**ChatGPT：** 要找出所有卧室数量等于浴室数量的住宿搜索，你可以使用以下 Python 代码：

```py
import pandas as pd
# Assuming 'airbnb_search_details' is the DataFrame containing your dataset
# Replace 'airbnb_search_details' with the actual name of your DataFrame if different
# Filter the DataFrame based on the condition: number of bedrooms == number of bathrooms
result = airbnb_search_details[airbnb_search_details['bedrooms'] == airbnb_search_details['bathrooms']]
# Display the resulting DataFrame
print(result) 
```

**控制**

实际上，ChatGPT 的大部分回答都正确，除了最后一行。没有必要使用 print 函数打印结果数据框，因此当代码更新如下时，解决方案是有效的。

```py
import pandas as pd
# Assuming 'airbnb_search_details' is the DataFrame containing your dataset
# Replace 'airbnb_search_details' with the actual name of your DataFrame if different
# Filter the DataFrame based on the condition: number of bedrooms == number of bathrooms
result = airbnb_search_details[airbnb_search_details['bedrooms'] == airbnb_search_details['bathrooms']]
# Display the resulting DataFrame
result
```

**问题 2**

这是我将输入到 ChatGPT-4 的 [问题](https://leetcode.com/problems/remove-element/description/)。

![XXXXX](../Images/48c75f8c14f50169a7e12a8de7da6d57.png)

我将基本上复制并粘贴我提示中的所有信息。

**提示**：

给定一个整数数组 nums 和一个整数 val，在 nums 中就地移除所有 val 的出现。元素的顺序可能会改变。然后返回 nums 中不等于 val 的元素个数。

考虑到 nums 中不等于 val 的元素个数为 k，要被接受，你需要做以下事情：

更改数组 nums，使得 nums 的前 k 个元素包含不等于 val 的元素。剩余的 nums 元素以及 nums 的大小都不重要。

返回 k。

自定义评判器：

法官将用以下代码测试你的解决方案：

int[] nums = [...]; // 输入数组

int val = ...; // 要移除的值

int[] expectedNums = [...]; // 预期的正确长度答案。

// 它已排序且没有值等于 val。

int k = removeElement(nums, val); // 调用你的实现

assert k == expectedNums.length;

sort(nums, 0, k); // 对 nums 的前 k 个元素进行排序

for (int i = 0; i < actualLength; i++) {

assert nums[i] == expectedNums[i];

}

如果所有断言通过，那么你的解决方案将被接受。

**示例 1:**

输入: nums = [3,2,2,3], val = 3

输出: 2, nums = [2,2,_,_]

解释: 你的函数应该返回 k = 2，nums 的前两个元素为 2。

无论你在返回的 k 之后留下什么（因此它们是下划线）都无所谓。

**示例 2:**

输入: nums = [0,1,2,2,3,0,4,2], val = 2

输出: 5, nums = [0,1,4,0,3,_,_,_]

解释: 你的函数应该返回 k = 5，nums 的前五个元素包含 0, 0, 1, 3 和 4。

注意这五个元素可以以任何顺序返回。

无论你在返回的 k 之后留下什么（因此它们是下划线）都无所谓。

**限制条件:**

0 <= nums.length <= 100

0 <= nums[i] <= 50

0 <= val <= 100

解决方案格式；

class Solution:

def removeElement(self, nums: List[int], val: int) -> int:

**ChatGPT:** 这是用 Python 实现的解决方案：

```py
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        k = 0  # Counter for elements not equal to val

        # Iterate through the array
        for i in range(len(nums)):
            if nums[i] != val:
                nums[k] = nums[i]  # Move the element to the kth position
                k += 1

        return k 
```

解决方案使用双指针方法原地修改 nums 数组，并计算不等于 val 的元素数量。变量 k 用于跟踪下一个非 val 元素应该放置的位置。

算法遍历数组，每当遇到一个不等于 val 的元素时，它就被移动到第 k 个位置，并且 k 增加。最后，k 表示不等于 val 的元素的数量。

你可以使用提供的自定义判题代码测试解决方案，以验证它是否对给定示例产生预期的输出并通过断言。

**控制**

代码工作正常，无需任何更新。

### SQL

SQL 对于处理数据库至关重要。GPT-4 可以帮助制定 SQL 查询、提升数据库效率，并掌握复杂的 SQL 概念。

**问题 1**

这是我们将尝试解决的 [SQL 问题](https://platform.stratascratch.com/coding/2155-ad-performance-rating?code_type=3&utm_source=blog&utm_medium=click&utm_campaign=kdn+exploring+gpt4)。

![XXXXX](../Images/6d889c42b2754ffbd0c955e17c5a3385.png)

**提示** : 问题如下：在最近的广告活动之后，营销部门希望根据每种产品的总销售量对其工作进行分类。

你需要计算每种产品的总销售量，并根据以下标准对广告表现进行分类：

优秀: 30+

满意: 20 - 29

不满意: 10 - 19

差: 1 - 9

你的输出应该包含产品 ID、按降序排列的总销售量以及其分类的广告表现。

我的数据集名称是 marketing_campaign，以下是数据类型。

marketing_campaign

user_id: int

created_at: datetime

product_id: int

quantity: int

price: int

这是期望的输出：

product_id total_sold ad_performance

105 41 杰出

102 29 满意

114 23 满意

118 22 满意

120 21 满意

使用 MySQL 解决此问题。

**ChatGPT**：要使用 MySQL 解决此任务，您可以编写一个查询来计算每个产品的总销售单位，并根据给定的标准对广告效果进行分类。

以下是查询：

```py
SELECT
    product_id,
    SUM(quantity) AS total_sold,
    CASE
        WHEN SUM(quantity) >= 30 THEN 'Outstanding'
        WHEN SUM(quantity) BETWEEN 20 AND 29 THEN 'Satisfactory'
        WHEN SUM(quantity) BETWEEN 10 AND 19 THEN 'Unsatisfactory'
        WHEN SUM(quantity) BETWEEN 1 AND 9 THEN 'Poor'
    END AS ad_performance
FROM
    marketing_campaign
GROUP BY
    product_id
ORDER BY
    total_sold DESC; 
```

在这个查询中，我们选择 product_id，计算数量的总和作为 total_sold，并使用 CASE 语句根据 total_sold 的值对广告效果进行分类。查询按 product_id 分组，按 total_sold 降序排列，并提供所需的输出。

确保将“marketing_campaign”替换为数据库中表的实际名称。

运行此查询将为您提供包含 product_id、total_sold 和 ad_performance 的预期结果，根据标准进行分类。

**控制**

代码运行完美，无需任何更新。

**问题 2**

这里是 [第二个 SQL 问题](https://leetcode.com/problems/product-sales-analysis-iii/?envType=study-plan-v2&envId=top-sql-50) 来自 LeetCode。

![XXXXX](../Images/50628491e5dbf41cc9c70030ed5dfea3.png)

**提示** : 表：Sales

+-------------+-------+

| 列名 | 类型  |
| --- | --- |

+-------------+-------+

| sale_id     | int   |
| --- | --- |
| product_id  | int   |
| year        | int   |
| quantity    | int   |
| price       | int   |

+-------------+-------+

(sale_id, year) 是该表的主键。product_id 是 Product 表的外键。

该表的每一行显示了某一年中 product_id 产品的销售情况。请注意价格是每单位的。

表：Product

+--------------+---------+

| 列名  | 类型    |
| --- | --- |

+--------------+---------+

| product_id   | int     |
| --- | --- |
| product_name | varchar |

+--------------+---------+

product_id 是该表的主键。

该表的每一行表示每个产品的产品名称。

编写一个 SQL 查询，选择每个销售的产品的产品 id、年份、数量和价格的第一个年份。

返回结果表可以按任何顺序。

查询结果格式如下例所示。

示例 1：

输入：

销售表：

+---------+------------+------+----------+-------+

| sale_id | product_id | year | quantity | price |
| --- | --- | --- | --- | --- |

+---------+------------+------+----------+-------+

| 1       | 100        | 2008 | 10       | 5000  |
| --- | --- | --- | --- | --- |
| 2       | 100        | 2009 | 12       | 5000  |
| 7       | 200        | 2011 | 15       | 9000  |

+---------+------------+------+----------+-------+

产品表：

+------------+--------------+

| product_id | product_name |
| --- | --- |

+------------+--------------+

| 100        | 诺基亚        |
| --- | --- |
| 200        | 苹果        |
| 300        | 三星        |

+------------+--------------+

输出：

+------------+------------+----------+-------+

| product_id | first_year | quantity | price |
| --- | --- | --- | --- |

+------------+------------+----------+-------+

| 100        | 2008       | 10       | 5000  |
| --- | --- | --- | --- |
| 200        | 2011       | 15       | 9000  |

+------------+------------+----------+-------+

**ChatGPT：** 要选择每个产品销售的第一年的产品 ID、年份、数量和价格，你可以使用以下 SQL 查询：

```py
SELECT s.product_id, s.year AS first_year, s.quantity, s.price
FROM Sales s
JOIN (
    SELECT product_id, MIN(year) AS first_year
    FROM Sales
    GROUP BY product_id
) t ON s.product_id = t.product_id AND s.year = t.first_year
ORDER BY s.product_id; 
```

解释：

1.  子查询（SELECT product_id, MIN(year) AS first_year FROM Sales GROUP BY product_id）使用 MIN() 函数选择每个产品的最小年份，并按 product_id 分组结果。

1.  主查询将 Sales 表与子查询连接，基于匹配的 product_id 和 year 列，以检索每个产品的第一年的销售记录。

1.  从 Sales 表中返回所选列，包括 product_id、年份（作为 first_year）、数量和价格，匹配记录。

1.  结果按 product_id 升序排列。

查询将返回每个产品销售的第一年的产品 ID、年份、数量和价格，如示例输出所示。

**控制**

代码正常工作，无需更新。

### R

R 是一种专为统计评估和数据可视化而构建的语言。GPT-4 可以支持理解 R 语言的细微差别、调试 R 代码以及使用各种 R 包进行数据分析。

**问题**

![XXXXX](../Images/c1766d05bc806a9e8e6955df3e419c17.png)

这是编码 [问题](https://platform.stratascratch.com/coding/10073-favorite-host-nationality?code_type=4&utm_source=blog&utm_medium=click&utm_campaign=kdn+exploring+gpt4)。

**提示：**

问题是：对于每个客人评论者，根据客人给主机的最高评分找到评论者最喜欢的主机的国籍。输出客人的用户 ID 以及他们最喜欢的主机的国籍。如果有多个来自同一国家的最喜欢的主机，只列出该国家一次（去除重复）。

from_user 和 to_user 列都是用户 ID。

预期输出如下：

from_user 国籍

0 卢森堡

1 巴西

2 澳大利亚

3 澳大利亚

4 巴西

数据框如下：

airbnb_reviews

from_user:int64

to_user:int64

from_type: object

to_type: object

review_score:int64

airbnb_hosts

host_id: int64

nationality: object

gender: object

age:int64

使用 R 解决这个问题。

提示：

使用 [column_name] 选择数据框中的特定列，然后选择值等于 '==' 的行。

使用 .groupby(column_name) 按指定列对数据框进行分组，然后使用 max() 获取每组的最高分数。

对原始数据框和分组数据框执行内连接，以使用 pd.merge(dataframe1, dataframe2, on = common_table_keys) 获取分组数据框的相应属性/详细信息。

对结果数据框和 airbnb_hosts 执行内连接。

使用 [[ column_name/s ]] 选择数据框中的指定列，然后使用 drop_duplicates(column_name) 获取指定列中的唯一值。

**ChatGPT**: 要使用 R 解决此任务，你可以按照以下步骤进行：

1.  从 'airbnb_reviews' 数据框中选择必要的列，并过滤出 'from_type' 为 'guest' 的行。

1.  按 'from_user' 对过滤后的数据框进行分组，并使用 max() 函数获取每位客人的最高评价分数。

1.  在原始数据框和分组数据框之间执行内连接，基于 'from_user' 和 'review_score' 列，以获取每位客人的最高评价分数对应的行。

1.  在结果数据框和 'airbnb_hosts' 数据框之间执行另一个内连接，基于 'to_user' 和 'host_id' 列，以获取喜爱的主机的国籍。

1.  从最终连接的数据框中选择 'from_user' 和 'nationality' 列。

1.  使用 distinct() 函数根据 'nationality' 列删除重复行。

这是实现此功能的代码：

```py
# Select necessary columns from airbnb_reviews and filter rows where from_type is 'guest'
filtered_reviews <- airbnb_reviews[airbnb_reviews$from_type == 'guest', c('from_user', 'to_user', 'review_score')]

# Group by from_user and get the maximum review score for each guest
max_scores <- aggregate(review_score ~ from_user, filtered_reviews, max)

# Perform inner join to get the rows with the maximum review score for each guest
joined_df <- merge(filtered_reviews, max_scores, by = c('from_user', 'review_score'))

# Perform inner join with airbnb_hosts to get the nationality of the favorite host
final_df <- merge(joined_df, airbnb_hosts, by.x = 'to_user', by.y = 'host_id')

# Select the necessary columns from the final joined dataframe
result <- final_df[, c('from_user', 'nationality')]

# Remove duplicate rows based on nationality
distinct_result <- distinct(result, .keep_all = TRUE)

# Print the final result
distinct_result
```

运行此代码将会得到预期的输出，其中包含 'from_user'（客人 ID）和他们喜爱主机的相应 'nationality'。

**控制**

代码运行正常，无需任何更新。

## 奖金：使用 ChatGPT 进行数据项目

[我们将测试 ChatGPT 的数据项目](https://platform.stratascratch.com/data-projects/partner-business-modeling?utm_source=blog&utm_medium=click&utm_campaign=kdn+exploring+gpt4) 已在 Uber 的数据科学职位招聘过程中作为家庭作业使用过。

![XXXXX](../Images/4e3acb45407f786d99b0f623e74ef44e.png)

总的来说，有两种不同的情况；

**情况 1**：这将是一个繁忙的星期六，道路上需要比上周更多的车辆。为了让司机上线，我们正在评估以下两种奖励选项的成本：

+   选项 1：每位在线时间达到 8 小时、接受 90% 请求、完成 10 次行程且评分为 4.7 或更高的司机，奖励 $50；

+   选项 2：所有完成 12 次行程且评分为 4.7 或更高的司机，每次行程奖励 $4。

**情况 2**：一名出租车司机目前每天的收入为 $200（不包括费用），每周工作六天，休息三周，费用如下：

+   油费 - 每周 $200

+   保险 - 每月 $400

+   车辆租赁（按周计算）- $500

司机在休息周不需要支付油费和租赁费用。

**ChatGPT 文件上传器**

现在，要进行数据项目，我们需要 [ChatGPT 文件上传器](https://chrome.google.com/webstore/detail/chatgpt-file-uploader/oaogphgfdbdbmhkiplemgehihiiececj) Chrome 扩展。它将允许你将文件上传到 ChatGPT。

如果你已经有一个代码解释器插件，则无需此操作——只需使用该插件上传文件。

欲了解更多信息，请观看这个 [视频](https://www.youtube.com/watch?v=iR99LO28nzM&embeds_referring_euri=https%3A%2F%2Fchrome.google.com%2F&source_ve_path=MjM4NTE&feature=emb_title)。

在设置文件上传扩展后，让我们尝试解决这个数据项目。

打开 ChatGPT 并点击此扩展的按钮。

你将看到如下所示的提交按钮。

![XXXXX](../Images/dae6fe3c567f894eb0dd1ea2a5142f3e.png)

来源: [https://chat.openai.com/](https://chat.openai.com/)

现在，让我们先通过使用我们的数据集文件并点击“提交文件”按钮来喂给 ChatGPT 数据集。

![XXXXX](../Images/0a2d1a72e3e32e60e8b38c7657b6d7e0.png)

来源: [https://chat.openai.com/](https://chat.openai.com/)

加载数据集后，ChatGPT 将提示数据集。

![XXXXX](../Images/65d86f6ec8a75755d4ed8e7ba3073a34.png)

来源: [https://chat.openai.com/](https://chat.openai.com/)

现在，这里是你应该用来解决此数据项目的提示。

![XXXXX](../Images/4e3acb45407f786d99b0f623e74ef44e.png)

**提示：** 根据操作情境和提供的数据集，使用 Python 回答以下问题

**任务**

情境 1：将会有一个大型的星期六，需要比上周更多的车辆上路。为了让司机上线，我们正在评估以下两种奖金选项的成本：

选项 1：每位司机在线至少 8 小时，接受 90% 的请求，完成 10 次行程，并且在此时间段内评分为 4.7 或更高，奖金为 $50；

选项 2：每次行程 $4，适用于所有完成 12 次行程且评分为 4.7 或更高的司机。

使用提供的数据集和给定的情境 1，回答以下问题：

选项 1 的总奖金支付额是多少？

选项 2 的总奖金支付额是多少？

有多少司机符合选项 1 的奖金条件但不符合选项 2？

在线的司机中，有多少百分比完成了少于 10 次行程，接受率低于 90%，并且评分为 4.7 或更高？

情境 2：一位出租车司机目前每天的收入为 200 美元（扣除费用前），每周工作六天，休息三周，拥有以下费用：

油费 - 每周 $200

保险 - 每月 $400

车辆租金（按周计算）- $500

司机在休息周期间不支付油费和租金费用。

现在，假设同一位司机购买了 Town Car 并与 Uber 合作。如果这样，他的油费将增加 5%，保险费用将减少 20%，并且不再租车。然而，他需要购买一辆车。司机仍将每年休息三周。

在情境 2 下，回答以下问题：

如果出租车司机不与 Uber 合作，每年赚多少钱（扣除费用后）？

你正在说服上述司机购买一辆 Town Car 并与 Uber 合作。假设新车价格为 40,000 美元，司机的总收入每周需要增加多少才能在第 1 年内完全支付车辆费用并维持与之前相同的年度利润率？

**ChatGPT：**

为了进行这些计算，我们首先需要加载和预处理数据集。我们将使用 Python 的 pandas 库来加载 CSV 文件并解析所需的列。

不幸的是，作为一个基于文本的模型，我无法直接执行代码。但我可以提供给你用于回答问题的Python代码：

这是你需要的代码，以回答场景1的问题：

```py
import pandas as pd

# Load dataset
data = pd.read_csv('dataset_2.csv')

# Convert 'Accept Rate' from string percentage to float
data['Accept Rate'] = data['Accept Rate'].str.rstrip('%').astype('float') / 100.0

# Calculate bonus under both options
data['Option 1 Bonus'] = ((data['Supply Hours'] >= 8) & (data['Accept Rate'] >= 0.9) & 
                          (data['Trips Completed'] >= 10) & (data['Rating'] >= 4.7)) * 50
data['Option 2 Bonus'] = ((data['Trips Completed'] >= 12) & (data['Rating'] >= 4.7)) * data['Trips Completed'] * 4

# Compute total payout for each option
total_payout_option1 = data['Option 1 Bonus'].sum()
total_payout_option2 = data['Option 2 Bonus'].sum()

# Compute number of drivers who qualify under Option 1 but not Option 2
num_option1_only = ((data['Option 1 Bonus'] > 0) & (data['Option 2 Bonus'] == 0)).sum()

# Compute percentage of drivers who complete less than 10 trips, had an acceptance rate of less than 90% and had a rating of 4.7 or higher
percent_undesirable_drivers = ((data['Trips Completed'] < 10) & (data['Accept Rate'] < 0.9) & (data['Rating'] >= 4.7)).mean() * 100

print('Total bonus payout for Option 1: $', total_payout_option1)
print('Total bonus payout for Option 2: $', total_payout_option2)
print('Number of drivers who qualify under Option 1 but not Option 2: ', num_option1_only)
print('Percentage of drivers who complete less than 10 trips, had an acceptance rate of less than 90%, and had a rating of 4.7 or higher: ', percent_undesirable_drivers) 
```

这是你需要的代码，以回答场景2的问题：

```py
# Calculate the taxi driver's yearly income and expenses
daily_income = 200
weekly_income = daily_income * 6  # Working 6 days a week
yearly_income = weekly_income * 52  # Working all year
yearly_expenses = (200 * 49) + (400 * 12) + (500 * 49)  # No expenses on off weeks
yearly_net = yearly_income - yearly_expenses

# Calculate the new expenses and net income if the driver partners with Uber
new_yearly_expenses = (200 * 1.05 * 49) + (400 * 0.8 * 12)  # 5% increase in gas, 20% decrease in insurance
new_yearly_net = yearly_income - new_yearly_expenses - 40000  # Subtract car cost

# Calculate the increase in gross fares necessary to maintain the same profit margin
additional_income_required = yearly_net - new_yearly_net
additional_income_required_weekly = additional_income_required / 52

print('Taxi driver yearly net income: $', yearly_net)
print('Required increase in weekly gross fares: $', additional_income_required_weekly)
```

你可以将read_csv函数中的文件路径替换为你实际的数据集_2.csv文件路径，然后在你的本地Python环境中运行这些代码片段。

**控制**

第一个场景的输出完全正确（前4个问题），但是，第二个场景的输出是不正确的。

例如，它计算了司机工作52周（一整年）；然而，司机休了3周假。

这就是为什么你需要对ChatGPT进行的计算保持谨慎，并检查其遵循的步骤，以确保结果的正确性。

经过几次尝试，这里是输出正确结果的代码，这个结果是ChatGPT提供的。

```py
# Weeks off per year
weeks_off = 3

# Total weeks per year
total_weeks_per_year = 52

# Weeks of work per year
working_weeks = total_weeks_per_year - weeks_off

# Total months per year
total_months_per_year = 12

# Expenses
gas_per_week = 200
insurance_per_month = 400
vehicle_rent_per_week = 500

# Revenue
fare_per_day = 200 
work_days_per_week = 6
total_revenue = fare_per_day * work_days_per_week * working_weeks

# Total Expenses
total_expenses = (gas_per_week + vehicle_rent_per_week) * working_weeks + insurance_per_month * total_months_per_year

# Net Income
net_income = total_revenue - total_expenses

print("Net Income per year: $", net_income)

# For Question 6:

# Expenses changes
new_gas_per_week = gas_per_week * 1.05  # 5% increase
new_insurance_per_month = insurance_per_month * 0.8  # 20% decrease
car_cost = 40000

# New Total Expenses
new_total_expenses = car_cost + (new_gas_per_week * working_weeks) + (new_insurance_per_month * total_months_per_year)

# To maintain the same net income, the new revenue should be:
new_total_revenue = new_total_expenses + net_income

# This translates to a required increase in weekly fares:
additional_fare_per_week = (new_total_revenue - total_revenue) / working_weeks

print("Required increase in weekly fares: $", additional_fare_per_week)
```

# 结语

这次探索揭示了GPT-4在数据科学领域的创新应用和固有局限性。

无论是处理复杂的统计和概率问题，建模问题，还是解答编码查询，GPT-4都展示了令人期待的能力。

然而，重要的是要记住，尽管GPT-4提供了一个有价值的工具，但人工监督和理解仍然至关重要，尤其是在数学计算方面，以确保质量结果。

这次探索只是GPT-4旅程的开始，因为进一步的进展和改进始终在前方。OpenAI的CEO Sam Altman 表示，在未来的模型（GPT-5）中，OpenAI计划解决不准确计算的问题以及提示数量的限制。

继续保持好奇，保持探索，并记住，AI潜力的唯一限制是我们为它设定的限制。

**[Nate Rosidi](https://www.stratascratch.com)** 是一名数据科学家和产品策略专家。他还是一名兼职教授，教授分析学，并且是[StrataScratch](https://www.stratascratch.com/)的创始人，该平台帮助数据科学家为面试做好准备，提供来自顶级公司的真实面试问题。你可以通过[Twitter: StrataScratch](https://twitter.com/StrataScratch)或[LinkedIn](https://www.linkedin.com/in/nathanrosidi/)与他联系。

### 更多相关话题

+   [预测未来事件：AI和ML的能力与局限](https://www.kdnuggets.com/2023/06/forecasting-future-events-capabilities-limitations-ai-ml.html)

+   [AI与数据分析师的对比：影响分析未来的6大限制](https://www.kdnuggets.com/ai-vs-data-analysts-top-6-limitations-impacting-the-future-of-analytics)

+   [释放AI的力量 - KDnuggets和Machine的特别发布](https://www.kdnuggets.com/2023/07/mlm-unlock-power-ai-special-release-kdnuggets-machine-learning-mastery.html)

+   [解锁健康经济学和结果研究中数字的力量](https://www.kdnuggets.com/2023/07/unlocking-power-numbers-health-economics-outcomes-research.html)

+   [利用数据的力量的3个步骤](https://www.kdnuggets.com/2022/05/3-steps-harnessing-power-data.html)

+   [为有效的Tableau和Power BI仪表板准备数据](https://www.kdnuggets.com/2022/06/prepare-data-effective-tableau-power-bi-dashboards.html)
