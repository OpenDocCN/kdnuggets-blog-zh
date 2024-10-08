# 最常见的数据科学面试问题及答案

> 原文：[`www.kdnuggets.com/2021/08/common-data-science-interview-questions-answers.html`](https://www.kdnuggets.com/2021/08/common-data-science-interview-questions-answers.html)

评论

![](img/07b0fcf6f1ccae532d7de32f06604751.png)

成为数据科学家被认为是一种令人羡慕的职业。早在 2012 年，《哈佛商业评论》称数据科学家是 21 世纪最性感的工作，行业角色的增长趋势似乎证实了这一说法。为了确认这一“性感”是否仍在持续，Glassdoor 的数据表明，数据科学家在 2021 年是美国第二最佳职业。

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全领域。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 工作

* * *

![](img/b42ee8ad918fbc61e5b65b44b975bd5a.png)

*来源：Glassdoor。*

要获得如此高端的职位，你必须经过严格的面试。数据科学的问题可以非常广泛和复杂。考虑到数据科学家通常涉及多个领域，这种情况是可以预见的。为了帮助你准备数据科学的面试，我审查了所有相关的问题，并将它们分为不同的类别。以下是我的分类方法。

## 分析的描述和方法论

我从各种招聘网站和公司评价平台如 Glassdoor、Indeed、Reddit 和 Blind App 收集了数据。更准确地说，过去四年收集了 903 个问题。

这些问题被分成预定的类别。这些类别是根据专家对面试经历描述的分析结果而得出的。

这些类别包括：

1.  编程

1.  建模

1.  算法

1.  统计

1.  概率

1.  产品

1.  商业案例

1.  系统设计

1.  技术

## 你应该期待什么类型的面试问题？

该图表显示了根据收集的数据，每个类别的问题类型。

![](img/7706db0c4857a84c80250aa66478b5e5.png)

转换为百分比后，图表如下所示：

![](img/1e42af0a3aa9ebde97cdc2bbac9464f0.png)

如你所见，编程和建模问题占据了主导地位。超过一半的问题来自这两个领域。考虑到这一点，也不奇怪。编程和建模可能是数据科学家最重要的两个技能。编程类型的问题很普遍，占所有问题的三分之一以上。其他问题类型，如算法和统计，也相当重要；24%的问题来自这两个类别。其他类别的代表性则较少。考虑到数据科学家的角色性质，我认为这是合理的。

现在，我想引导你了解每个问题类别，并展示一些被问到的问题示例。

## 数据科学面试中最常考察的概念

### 编程

正如你所见，编程问题是数据科学中最重要的话题。这类问题通常需要通过代码对数据进行某种形式的操作，以识别洞察力。这些问题旨在测试编码能力、解决问题的技巧和创造力。你通常会在计算机或白板上完成这些任务。

**编程面试问题示例**

一个来自[微软的示例](https://platform.stratascratch.com/coding-question?id=2028&python=)是这样的：

> **问题**：*“计算新用户和现有用户的比例。输出月份、新用户比例和现有用户比例。新用户定义为在当前月份开始使用服务的用户。现有用户是指在当前月份开始使用服务并且在之前任何月份也使用过服务的用户。假设所有日期都来自 2020 年。”*

你将使用*fact_events*表，样本数据如下：

![](img/468df8dde9cc75f6fd1d30c08fa8a65f.png)

为了获得所需的输出，你应该编写以下代码：

```py
with all_users as (
    SELECT date_part('month', time_id) AS month,
           count(DISTINCT user_id) as all_users
    FROM fact_events
    GROUP BY month),
new_users as (
    SELECT date_part('month', new_user_start_date) AS month,
           count(DISTINCT user_id) as new_users
    FROM
         (SELECT user_id,
           min(time_id) as new_user_start_date
          FROM fact_events
          GROUP BY user_id) sq
    GROUP BY month
)
SELECT
  au.month,
  new_users / all_users::decimal as share_new_users,
  1- (new_users / all_users::decimal) as share_existing_users
FROM all_users au
JOIN new_users nu ON nu.month = au.month

```

编写 SQL 代码是编程中最常考察的概念。这也不奇怪，因为 SQL 是数据科学中使用最广泛的工具。你几乎无法避免的一个概念就是连接（joins）。所以请确保你了解不同连接的区别以及如何使用它们来获得所需的结果。

此外，你可以预期经常使用 GROUP BY 子句对数据进行分组。其他常被询问的概念包括使用 WHERE 和/或 HAVING 子句过滤数据。你还会被要求选择不同的数据。此外，确保你了解聚合函数，如 SUM()、AVG()、COUNT()、MIN()、MAX()。

一些概念出现的频率不高，但值得提及并为这些问题做好准备。例如，公用表表达式（CTEs）就是一个这样的主题。另一个是 CASE()子句。此外，别忘了复习一下字符串数据类型和日期的处理方法。

### 建模

在我们的研究数据中，建模是第二大类别，占所有问题的 20%。这些问题旨在测试你构建统计模型和实现机器学习模型的知识。

**建模面试问题示例**

回归是面试中最常见的技术数据科学概念。考虑到统计建模的性质，这也就不足为奇了。

一个[来自 Galvanise 的示例](https://platform.stratascratch.com/technical/2167-regularization)是如下的：

> **问题**：*“回归中的正则化是什么？”*

下面是你可以回答这个问题的方式：

**回答**：*“正则化是一种特殊的回归类型，其中系数估计值被约束（或正则化）为零。通过这样做，可以减少模型的方差，同时降低采样误差。正则化用于避免或减少过拟合。过拟合发生在模型过于完美地学习训练数据，从而影响模型在新数据上的表现。为了避免过拟合，通常使用 Ridge 或 Lasso 正则化。”*

一些经常测试的概念包括其他回归分析概念，如逻辑回归、贝叶斯逻辑回归和朴素贝叶斯分类器。你也可能会被问到随机森林，以及测试和评估模型。

### 算法

算法相关的问题都是需要通过编程语言解决数学问题的问题。这些问题涉及逐步过程，通常需要调整或计算以得出答案。这些问题测试了问题解决和数据处理的基础知识，这些知识可以应用于工作中的复杂问题。

**算法面试问题示例**

在算法方面，最常测试的技术概念是用编程语言解决数学或语法问题。

这里是[你可以在 Leetcode 上找到的一个示例](https://leetcode.com/problems/add-two-numbers/)：

> **问题**：*“给定两个非空链表，表示两个非负整数。数字以逆序存储，每个节点包含一个数字。将两个数字相加，并将和作为链表返回。”*

数据示例可能如下所示：

![](img/bf8132ea9da182c5312a6864345ae6c2.png)

*来源：Leetcode。*

**回答**：用 Java 编写的代码应该是：

```py
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummyHead = new ListNode(0);
    ListNode p = l1, q = l2, curr = dummyHead;
    int carry = 0;
    while (p != null || q != null) {
        int x = (p != null) ? p.val : 0;
        int y = (q != null) ? q.val : 0;
        int sum = carry + x + y;
        carry = sum / 10;
        curr.next = new ListNode(sum % 10);
        curr = curr.next;
        if (p != null) p = p.next;
        if (q != null) q = q.next;
    }
    if (carry > 0) {
        curr.next = new ListNode(carry);
    }
    return dummyHead.next;
}

```

其他类型问题经常测试的常见概念包括数组、动态规划、字符串、贪心算法、深度优先搜索、树、哈希表和二分搜索。

### 统计

统计面试问题是测试统计理论及其相关原则的知识的问题。这些问题旨在考察你对数据科学基础理论原则的熟悉程度。理解分析所依据的理论和数学背景非常重要。回答好这些问题，每位面试官都会对你刮目相看。

**统计面试题例子**

最常提到的技术概念是抽样和分布。对于数据科学家来说，这是数据科学家每天实施的最常用的统计原则之一。

例如，[IBM 的面试题](https://platform.stratascratch.com/technical/2048-non-gaussian-distribution)问道：

> **问题**：*“一个非高斯分布的数据类型的例子是什么？”*

要回答这个问题，你可以先定义高斯分布。然后可以给出非高斯分布的例子。像这样：

**答案**：*“高斯分布是一个分布，其中在检查从均值的标准差时，可以找到一定已知百分比的数据，也称为正态分布。非高斯分布的一些例子可以是指数分布或二项分布。”*

准备面试时，确保你还涵盖以下主题：方差和标准差、协方差和相关性、p 值、均值和中位数、假设检验和贝叶斯统计。这些都是数据科学家需要掌握的概念，所以在面试中也要有所准备。

### 概率

这些问题只需要对概率概念的理论知识。面试官提出这些问题是为了深入了解你对概率方法和用途的知识，以完成通常在工作场所进行的复杂数据研究。

**概率面试题例子**

很有可能，你得到的问题是计算从一组骰子/卡片中抽到某张卡片/数字的概率。这似乎是我们研究中大多数公司最常问的问题，因为许多公司都提出过这些问题。

这样一个[来自 Facebook 的概率问题](https://platform.stratascratch.com/technical/2003-two-cards-same-suite)的例子：

> **问题**：*“在一副 52 张牌的牌堆中，分别抽取两张牌得到一对的概率是多少？”*

这里是你可以回答的方式：

**答案**：*“你抽的第一张卡片可以是任意一张，所以它不会影响结果，除了在牌堆中少了一张卡片。抽出第一张卡片后，剩下的牌堆中有三张卡片可以组成一对。所以，与第一张卡片匹配成对的几率是 51 张剩余卡片中的 3 张。也就是说，这个事件发生的概率是 3/51 或 5.89%。”*

由于这是一个只涉及概率的“专业”问题，没有其他概念被提问。唯一的区别是问题的想象力。然而，基本上，你总是需要计算某个事件的概率并展示你的思考过程。

### 产品

产品面试问题将要求你通过数据评估产品/服务的表现。这些问题测试你在任何环境中适应和使用数据科学原则的知识，就像日常工作一样。

**产品面试问题示例**

这一类别中最突出的技术概念是从数据科学家的角度识别公司的产品并提出改进建议。产品方面测试的技术概念差异大，可以通过产品问题的性质和回答这些问题所需的更高创造性来解释。

一个来自 [Facebook 的产品问题示例](https://platform.stratascratch.com/technical/2192-favorite-product-or-app) 是：

> **问题**: *“你最喜欢的 Facebook 产品是什么？你会如何改进它？”*

**回答**: *由于问题的性质，我们将让你自己回答这个问题。*

测试的常见概念很大程度上依赖于面试公司的具体情况。只要你熟悉公司的业务和他们的产品（理想情况下，你也是他们的用户），你就会没问题。

### 业务案例

这一类别包括案例研究和与业务相关的通用问题，这些问题将测试数据科学技能。了解如何回答这些问题的意义可能是巨大的，因为一些面试官希望候选人在被聘用之前能知道如何将数据科学原则应用于解决公司特定问题。

**业务案例问题示例**

由于问题类型的性质，我无法确定一个突出的技术概念。由于这里分类的大多数问题都是案例研究，它们在某种程度上都是独特的。

不过，以下是一个来自 [Uber 的业务案例问题示例](https://platform.stratascratch.com/technical/2014-determining-origin-city)：

> **问题**: *“有一组人从两个相近的城市乘坐了 Uber，比如 Menlo Park 和 Palo Alto，你可以收集的任何数据都可以被考虑。你会收集什么数据，以便确定乘客的出发城市？”*

**回答**: *“要确定城市，我们需要访问位置/地理数据。收集的数据可以是 GPS 坐标、经度/纬度和邮政编码。”*

### 系统设计

系统设计问题是所有与设计技术系统相关的问题。提出这些问题是为了分析候选人在解决问题、创建和设计系统以帮助客户/客户的过程中。对于数据科学家来说，了解系统设计非常重要；即使你的角色不是设计系统，你也很可能在已建立的系统中扮演角色，需要知道它是如何工作的，以便完成你的工作。

**系统设计面试问题示例**

这些问题涵盖了不同的主题和任务。但最突出的是构建数据库。数据科学家每天都大量处理数据库，因此询问这个问题以了解你是否能从零开始构建一个数据库是有意义的。

这是一个 [来自 Audible 的问题示例](https://platform.stratascratch.com/technical/2148-build-a-recommendation-system)，在我们的研究中揭示：

> **问题**：“*你能详细说明一下你将如何构建推荐系统吗？*”

**答案**：*由于回答这个问题有各种不同的方法，我们将留给你自己想出构建一个的方法。*

再次强调，为了回答这些问题，了解公司的业务至关重要。考虑一下公司最可能需要的数据库，并在面试前稍微展开你的方法。

### 技术

技术问题是所有询问各种数据科学技术概念解释的问题。技术问题是理论性的，需要了解你将在公司使用的技术。由于其性质，它们可能看起来与编码问题相似。了解你所做事情的理论是非常重要的，因此技术问题在面试中经常被提问。

**技术面试问题示例**

最常被考察的领域是 Python 和 SQL 的理论知识。不足为奇，因为这两种语言在数据科学中占据主导地位，R 语言则补充了 Python。

一个 [来自 Walmart 的实际技术问题示例](https://platform.stratascratch.com/technical/2072-data-structures-in-python) 是：

> **问题**：“*Python 中的数据结构有哪些？*”

**答案**：*“数据结构用于存储数据。Python 中有四种数据结构：列表、字典、元组和集合。这些是内置的数据结构。列表用于创建可以包含不同类型数据的列表。字典基本上是一组键；它们用于存储带有键的值，并使用相同的键获取数据。元组与列表相同。不同之处在于在元组中数据不能更改。集合包含无序的元素且没有重复。除了内置的数据结构，还有用户定义的数据结构。”*

这些是包含所有无法干净地归入其他类别的问题的类别。因此，没有特定的概念出现得更多或更少。

## 结论

本数据科学面试指南旨在支持理解数据科学面试中提出的问题类型的研究。面试问题的数据来自于四年期间几十家公司并进行了分析。这些问题被分类为九种不同的问题类型（算法、商业案例、编码、建模、概率、产品、统计、系统设计和技术问题）。

在分析过程中，我讨论了每种问题类型类别中一些最常见的技术概念。例如，最常被问及的统计问题涉及采样和分布。每个问题类别都有一个实际问题的示例。

本文旨在为你提供重要的面试准备指南或简单地了解更多关于数据科学的知识。希望我能帮助你在数据科学面试过程中更加自信。祝你面试顺利！

[原文](https://cult.honeypot.io/reads/most-common-data-science-interview-questions/)。经许可转载。

**相关内容：**

+   [30 个最常被问及的机器学习问题及答案](https://www.kdnuggets.com/2021/08/30-machine-learning-questions-answered.html)

+   [顶级 Python 数据科学面试问题](https://www.kdnuggets.com/2021/07/top-python-data-science-interview-questions.html)

+   [你如何能从数百名数据科学候选人中脱颖而出？](https://www.kdnuggets.com/2021/07/distinguish-yourself-hundreds-other-data-science-candidates.html)

### 更多相关话题

+   [7 个数据分析面试问题及答案](https://www.kdnuggets.com/2022/09/7-data-analytics-interview-questions-answers.html)

+   [5 个 Python 面试问题及答案](https://www.kdnuggets.com/2022/09/5-python-interview-questions-answers.html)

+   [20 个问题（含答案）来识别伪数据科学家：ChatGPT…](https://www.kdnuggets.com/2023/01/20-questions-detect-fake-data-scientists-chatgpt-1.html)

+   [20 个问题（含答案）来识别伪数据科学家：ChatGPT…](https://www.kdnuggets.com/2023/02/20-questions-detect-fake-data-scientists-chatgpt-2.html)

+   [成为优秀数据科学家需要的 5 项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应该掌握的 6 个预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)
