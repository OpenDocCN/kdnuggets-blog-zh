# AI 与数据分析师：影响分析未来的六大限制

> 原文：[`www.kdnuggets.com/ai-vs-data-analysts-top-6-limitations-impacting-the-future-of-analytics`](https://www.kdnuggets.com/ai-vs-data-analysts-top-6-limitations-impacting-the-future-of-analytics)

![AI 与数据分析师：影响分析未来的六大限制](img/9ea29a3487853b34aa5063367fe1a0b7.png)

## AI 能做哪些类型的数据分析？

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析水平

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT 工作

* * *

我们已经知道 ChatGPT 是最万能的 AI 工具，它的插件使其能够做几乎所有事情。它可以在 Python、R 以及许多其他语言中[生成可运行的代码](https://medium.com/@tanyamarleytsui/coding-with-chatgpt-b50ab3fcb45f)，以及复杂的 SQL 查询。可以想象，结合这些功能将使您能够将 AI 用于数据分析工作的各个方面。

![AI 与数据分析师：影响分析未来的六大限制](img/97d3b1eb0b74dd410c467dfb03cd4598.png)

用例包括：

+   查询

+   清洗和其他处理

+   可视化

在处理数据时，像 [Julius AI](https://blog.enterprisedna.co/julius-ai/page/2/?et_blog)（用于 csv 文件）或 [BlazeSQL](https://www.blazesql.com)（用于 SQL 数据库）这样的专用工具是为此目的专门设计的。与 ChatGPT 不同，这些工具不需要您每次打开时都上传/连接和解释数据。

ChatGPT 可以对 csv 文件进行一些快速分析，但大多数公司将数据存储在私有网络中的 SQL 数据库中。尽管如此，专用工具可以连接到这些安全的 SQL 数据库，通过查询数据库并可视化结果来回答您的问题。

## AI 如何取代数据分析师？

数据分析的核心是从数据中获得见解，数据分析师和数据科学家拥有提供利益相关者所需见解的技术技能。但情况已发生变化，现在 AI 工具可以成功完成一些以前只能由数据分析师和数据科学家完成的任务。

理论上，一个没有技术技能的业务利益相关者现在可以将他们的数据连接到一个 AI 工具，并发出类似于“获取按产品分组的每月收入，针对年度前三产品”的请求。AI 然后可以抓取数据，甚至可视化。用户只需要花几秒钟写出请求。如果他们问人类同事，可能几天或更长时间才能得到答案。

![AI 与数据分析师：影响分析未来的六大限制](img/4d1c363455ff6d2a5d201a9d26094734.png)

看到这样的图像对数据分析师来说既令人惊叹又令人担忧，但**取代数据分析师和数据科学家并非那么简单**。简单地运行一个 SQL 查询并绘制结果**只是他们工作的一部分**，即使是这些，也不总是可以被 AI 可靠地完成。虽然在上面的截图中可能有效，但如果结果看起来没问题却实际上错误呢？

听起来是时候讨论一些 AI 在处理数据时的限制了。

## 限制 #1: AI 幻觉

大多数使用 ChatGPT 和类似工具的人都听说过“幻觉”这个术语。当你问他们一些他们不知道的事情时，他们有时**会编造信息**。

这些幻觉的原因很简单：LLMs 像是非常先进的自动完成算法。它们基于训练数据返回**最可能的下一个消息**。得益于高质量的数据集和先进的训练技术，这种“自动完成”工作得非常好，这些工具能够以显著的高质量结果满足复杂请求。不幸的是，当它们遇到训练数据没有准备的情况时，**最可能的下一个消息**可能实际上并不太有意义。

如果它生成一些运行的代码，但代码返回了错误的数据怎么办？使用 AI 数据分析师的业务利益相关者可能不知道结果是错误的，但由于他们不理解代码，无法看到错误。

## 限制 #2: 商业信息

通常，当一名新的数据分析师开始在公司工作时，他们需要了解一些列和数值的含义。这是因为数据模型是由业务设计的。你不能仅仅分析数据而不理解数据来源，因为常识不足以理解大多数数据库。

![AI 与数据分析师：影响分析未来的六大限制](img/f4bbdd1fba2ae745393b54f092a440d1.png)

像 BlazeSQL 这样的 AI 工具确实允许你为 AI 提供这些信息，但需要数据分析师或数据科学家来保持这些信息的最新。

## 限制 #3: 有时，AI 就会卡住。也叫“盲点”

你可能见过 ChatGPT 在非常基础的问题上陷入困境的例子。这些问题通常很容易回答，但需要 AI 以它不擅长的方式进行推理。

![AI 与数据分析师：影响分析未来的六大限制](img/3bd1f475e488018d37a7a9791a0645a5.png)

我们可以将这些情况称为“盲点”，它们在编写代码时也存在。例如，AI 在生成 SQL 查询时的一个常见盲点是使用子查询。AI 模型通常会生成尝试从子查询中选择列的查询，即使该列在子查询中不存在。

```py
WITH recent_orders AS (
  SELECT
    customer_id,
    MAX(order_date) AS latest_order_date
  FROM
    orders
  GROUP BY
    customer_id
)
SELECT
  customer_id,
  product_id,  -- (This column is not defined in the subquery)
  latest_order_date
FROM
  recent_orders
```

即使指出了错误，它们在重试时也往往会犯同样的错误。

## 限制 #4：AI 模型过于一致

AI 模型往往会同意你的观点，即使你是错的。当 AI 模型应该扮演专家角色时，这可能是一个巨大的问题，因为专家应该能够在你错误时纠正你。

![AI 与数据分析师：影响分析未来的六大限制](img/4b120c8b45b38b4c752e2a6fda90205d.png)

## 限制 #5：输入长度

人类可能需要花费几个月的时间来了解一个项目和数据库，收集大量重要信息。另一方面，大型语言模型（LLM）通常有一个“令牌限制”，这意味着它只能接受一定量的输入。

![AI 与数据分析师：影响分析未来的六大限制](img/7356d3832de03b794ccb0c242e82146c.png)

这种输入长度（也称为“令牌限制”）在复杂任务中常常具有约束性。你如何将几个月的学习浓缩成几页，并适应到 AI 模型中呢？

广泛使用的 GPT-4 版本限制为 **12 页** 的输入 + 输出。请记住，数据分析师需要参加数小时的会议，阅读文档或报告。所有输出（代码和 GPT-4 的解释）需要从 12 页中扣除，因为限制包括输出，而不仅仅是输入。

这意味着一个需要大量学习和探索的重大数据分析项目根本不可行。

## 限制 #6：软技能

最后但绝对不容忽视的是，ChatGPT 和其他 AI 聊天机器人……只是聊天机器人。人际互动和软技能是从事数据项目的重要组成部分。不管是建立信任、处理办公室政治，还是解读非语言沟通，这些元素对成功与利益相关者合作和完成项目至关重要。

## 接下来是什么？

正如你所看到的，AI 具有许多限制，阻止其成为完全胜任的数据分析师。上述列表只是一些主要的限制，但在真正取代数据专家时，还有许多其他重大障碍。换句话说，**你不需要担心 AI 会取代你**！

尽管如此，**AI 已经对数据分析师和数据科学家产生了重大影响**。它可能不是完美的，但它已经提供了不可思议的价值。

## 使用 AI 提高工作效率

编写代码，无论是 Python、SQL 还是 R，都可能很耗时。这些 AI 工具可能不是 100% 准确，但它们在很多情况下效果很好。快速审查它们生成的内容通常比从头开始做快 10 倍。

![AI 与数据分析师：影响分析未来的前 6 大局限性](img/1ee5de70aaae713538a0bfb9e1cb962d.png)

在人工智能表现不佳或常常出错的情况下，从头开始做可能更快。在其他情况下，大幅度提升的生产力值得偶尔进行调试。关键在于尝试不同的工具，了解它们的优缺点，并根据需要将其整合到你的工作流程中。

## 未来怎么样？

事情进展极快，因此一些当前的局限性可能不会持续很久。特别是现在 AI 工具被如此多人使用，因为它们[从用户那里学习](https://www.pcguide.com/apps/does-chatgpt-learn-from-users/#:~:text=How%20does%20ChatGPT%20learn%20from,question%20or%20make%20a%20statement.)。这些互动用于训练模型，每天都有数百万次互动。

ChatGPT 拥有史上增长最快的用户基础，**它从这些用户基础中学习。**

![AI 与数据分析师：影响分析未来的前 6 大局限性](img/920101688c80e136e181b545d9929860.png)

随着 Claude、Bard 和其他竞争者加入竞争，我们很快就会看到一些重大的改进。

准备这些变化很简单，只需关注新工具并进行尝试。这样你就会了解它们的优缺点，并确保利用最新技术并在其演变过程中适应。

关于这一点，值得关注的几个工具包括：

[BlazeSQL](https://openai.com/blog/introducing-chatgpt-enterprise)（用于 SQL 数据库）

[ChatGPT 高级数据分析](https://openai.com/blog/introducing-chatgpt-enterprise)（用于 csv 和其他文件）

[Pandas AI](https://github.com/gventuri/pandas-ai)（将生成式 AI 添加到 pandas 库中）

**[](https://www.linkedin.com/in/justus-mulli-64551889)**[Justus Mulli](https://www.linkedin.com/in/justus-mulli-64551889)**** 是一位数据科学家和创始人，拥有金融、医疗保健和电子商务领域的经验。他利用在数据科学和 AI 方面的专业知识，在各个行业和职业中实施颠覆性的 AI 解决方案。

### 更多相关话题

+   [预测未来事件：AI 和 ML 的能力与局限性](https://www.kdnuggets.com/2023/06/forecasting-future-events-capabilities-limitations-ai-ml.html)

+   [AI 如何影响 2023 年 STEM 教育的 5 种方式](https://www.kdnuggets.com/2023/04/5-ways-ai-impacting-stem-education-2023.html)

+   [探索 GPT-4 的力量与局限性](https://www.kdnuggets.com/2023/07/exploring-power-limitations-gpt4.html)

+   [数据分析师和数据科学家之间的区别是什么？](https://www.kdnuggets.com/2022/03/difference-data-analysts-data-scientists.html)

+   [数据分析师的 SQL 和 Python 面试问题](https://www.kdnuggets.com/2023/02/sql-python-interview-questions-data-analysts.html)

+   [数据科学家和分析师的统计学基础](https://www.kdnuggets.com/2023/08/fundamentals-statistics-data-scientists-analysts.html)
