# 解决5个复杂SQL问题：棘手的查询解析

> 原文：[https://www.kdnuggets.com/2022/07/5-hardest-things-sql.html](https://www.kdnuggets.com/2022/07/5-hardest-things-sql.html)

![在SQL中最难做的5件事](../Images/fa67cdf461a14281ea03e090313dda28.png)

编辑器提供的图片

我们许多人都体验过将计算集中在云数据仓库中带来的速度和效率的核心优势。虽然这确实如此，但我们也意识到，就像任何事情一样，这种价值也伴随着自己的缺点。

这种方法的主要缺点之一是你必须学习和执行不同语言的查询，特别是SQL。虽然编写SQL比建立一个运行python的辅助基础设施（在你的笔记本电脑或办公室服务器上）更快、更便宜，但它带来了许多复杂性，具体取决于数据分析师希望从云数据仓库中提取什么信息。转向云数据仓库增加了复杂SQL相较于python的实用性。经历了这一过程后，我决定记录那些在SQL中最难学和执行的具体转换，并提供缓解这些痛苦所需的实际SQL。

为了帮助你的工作流程，你会注意到我在转换执行前后提供了数据结构示例，这样你可以跟随并验证你的工作。我还提供了进行这5个最难转换所需的实际SQL。你需要新的SQL来执行多个项目的转换，因为你的数据会变化。我们提供了每个转换的动态SQL链接，以便你可以根据需要继续捕获分析所需的SQL！

# 日期脊

术语“日期脊”来源不明确，但即使是不知道这个术语的人，也可能对它的概念很熟悉。

想象一下你正在分析每日销售数据，它看起来像这样：

| sales_date | product | sales |
| --- | --- | --- |
| 2022-04-14 | A | 46 |
| 2022-04-14 | B | 409 |
| 2022-04-15 | A | 17 |
| 2022-04-15 | B | 480 |
| 2022-04-18 | A | 65 |
| 2022-04-19 | A | 45 |
| 2022-04-19 | B | 411 |

16号和17号没有销售，因此这些行完全缺失。如果我们尝试计算*平均每日销售额*或构建时间序列预测模型，这种格式将是一个主要问题。我们需要做的是插入缺失日期的行。

这是基本概念：

1.  生成或选择唯一日期

1.  生成或选择唯一产品

1.  交叉连接（笛卡尔积）1和2的所有组合

1.  外连接 #3 到你的原始数据

[可定制的日期脊柱SQL](https://app.rasgoml.com/sql?transformName=%22datespine_groups%22&tableState=%7B%22tables%22%3A%5B%7B%22name%22%3A%22My_First_Table%22%2C%22columns%22%3A%5B%7B%22name%22%3A%22sales_date%22%2C%22dataType%22%3A%22date%22%7D%2C%7B%22name%22%3A%22product%22%2C%22dataType%22%3A%22string%22%7D%2C%7B%22name%22%3A%22sales%22%2C%22dataType%22%3A%22number%22%7D%5D%7D%5D%2C%22baseTableName%22%3A%22My_First_Table%22%2C%22ddl%22%3A%22%22%7D&formState=%7B%22arguments%22%3A%7B%22group_by%22%3A%7B%22argType%22%3A%22column_list%22%2C%22cols%22%3A%5B%7B%22id%22%3A1%2C%22columnName%22%3A%22product%22%2C%22displayName%22%3A%22product%22%2C%22dataType%22%3A%22string%22%2C%22dwColumnId%22%3A1%7D%5D%7D%2C%22date_col%22%3A%7B%22argType%22%3A%22column%22%2C%22col%22%3A%7B%22id%22%3A0%2C%22columnName%22%3A%22sales_date%22%2C%22displayName%22%3A%22sales_date%22%2C%22dataType%22%3A%22date%22%2C%22dwColumnId%22%3A0%7D%7D%2C%22start_timestamp%22%3A%7B%22argType%22%3A%22timestamp%22%2C%22value%22%3A%222020-01-01T00%3A00%22%7D%2C%22end_timestamp%22%3A%7B%22argType%22%3A%22timestamp%22%2C%22value%22%3A%222023-01-01T00%3A00%22%7D%2C%22interval_type%22%3A%7B%22argType%22%3A%22date_part%22%2C%22value%22%3A%22day%22%7D%2C%22group_bounds%22%3A%7B%22argType%22%3A%22value%22%2C%22value%22%3A%22mixed%22%7D%7D%2C%22transformName%22%3A%22datespine_groups%22%7D)

最终结果将如下所示：

| sales_date | product | sales |
| --- | --- | --- |
| 2022-04-14 | A | 46 |
| 2022-04-14 | B | 409 |
| 2022-04-15 | A | 17 |
| 2022-04-15 | B | 480 |
| 2022-04-16 | A | 0 |
| 2022-04-16 | B | 0 |
| 2022-04-17 | A | 0 |
| 2022-04-17 | B | 0 |
| 2022-04-18 | A | 65 |
| 2022-04-18 | B | 0 |
| 2022-04-19 | A | 45 |
| 2022-04-19 | B | 411 |

# Pivot / Unpivot

有时候，在进行分析时，你可能需要重新构建表格。例如，我们可能有一个学生、科目和成绩的列表，但我们想将科目拆分到每一列。我们都知道并喜欢Excel的**数据透视表**。但你是否尝试过在SQL中实现它？不仅每个数据库在如何支持PIVOT方面有令人烦恼的差异，而且语法也不直观，容易忘记。

之前：

| 学生 | 科目 | 成绩 |
| --- | --- | --- |
| Jared | 数学 | 61 |
| Jared | 地理 | 94 |
| Jared | 体育 | 98 |
| Patrick | 数学 | 99 |
| Patrick | 地理 | 93 |
| Patrick | 体育 | 4 |

[可自定义 SQL 进行透视](https://app.rasgoml.com/sql?transformName=%22pivot%22&tableState=%7B%22tables%22%3A%5B%7B%22name%22%3A%22My_First_Table%22%2C%22columns%22%3A%5B%7B%22name%22%3A%22Student%22%2C%22dataType%22%3A%22string%22%7D%2C%7B%22name%22%3A%22Subject%22%2C%22dataType%22%3A%22string%22%7D%2C%7B%22name%22%3A%22Grade%22%2C%22dataType%22%3A%22number%22%7D%5D%7D%5D%2C%22baseTableName%22%3A%22My_First_Table%22%2C%22ddl%22%3A%22%22%7D&formState=%7B%22arguments%22%3A%7B%22dimensions%22%3A%7B%22argType%22%3A%22column_list%22%2C%22cols%22%3A%5B%7B%22id%22%3A0%2C%22columnName%22%3A%22Student%22%2C%22displayName%22%3A%22Student%22%2C%22dataType%22%3A%22string%22%2C%22dwColumnId%22%3A0%7D%5D%7D%2C%22pivot_column%22%3A%7B%22argType%22%3A%22column%22%2C%22col%22%3A%7B%22id%22%3A2%2C%22columnName%22%3A%22Grade%22%2C%22displayName%22%3A%22Grade%22%2C%22dataType%22%3A%22number%22%2C%22dwColumnId%22%3A2%7D%7D%2C%22value_column%22%3A%7B%22argType%22%3A%22column%22%2C%22col%22%3A%7B%22id%22%3A1%2C%22columnName%22%3A%22Subject%22%2C%22displayName%22%3A%22Subject%22%2C%22dataType%22%3A%22string%22%2C%22dwColumnId%22%3A1%7D%7D%2C%22agg_method%22%3A%7B%22argType%22%3A%22agg%22%2C%22value%22%3A%22AVG%22%7D%2C%22list_of_vals%22%3A%7B%22argType%22%3A%22string_list%22%2C%22values%22%3A%5B%22Mathematics%22%2C%22Geography%22%2C%22Phys%20Ed%22%5D%7D%7D%2C%22transformName%22%3A%22pivot%22%7D)

结果：

| Student | Mathematics | Geography | Phys Ed |
| --- | --- | --- | --- |
| Jared | 61 | 94 | 98 |
| Patrick | 99 | 93 | 4 |

# 一热编码

这个方法不一定困难，但确实耗时。大多数数据科学家不考虑在 SQL 中做一热编码。虽然语法很简单，他们宁愿将数据从数据仓库中转移出来，也不愿意写 26 行的 CASE 语句。我们不怪他们！

然而，我们建议利用你的数据仓库及其处理能力。这里是一个使用 STATE 作为列进行一热编码的示例。

之前：

| Babyname | State | Qty |
| --- | --- | --- |
| Alice | AL | 156 |
| Alice | AK | 146 |
| Alice | PA | 654 |
| … | … | … |
| Zelda | NY | 417 |
| Zelda | AL | 261 |
| Zelda | CO | 321 |

[可自定义的 SQL 用于独热编码](https://app.rasgoml.com/sql?transformName=%22one_hot_encode%22&tableState=%7B%22tables%22%3A%5B%7B%22name%22%3A%22My_First_Table%22%2C%22columns%22%3A%5B%7B%22name%22%3A%22BABYNAME%22%2C%22dataType%22%3A%22string%22%7D%2C%7B%22name%22%3A%22STATE%22%2C%22dataType%22%3A%22string%22%7D%2C%7B%22name%22%3A%22Qty%22%2C%22dataType%22%3A%22number%22%7D%5D%7D%5D%2C%22baseTableName%22%3A%22My_First_Table%22%2C%22ddl%22%3A%22%22%7D&formState=%7B%22arguments%22%3A%7B%22column%22%3A%7B%22argType%22%3A%22column%22%2C%22col%22%3A%7B%22id%22%3A1%2C%22columnName%22%3A%22STATE%22%2C%22displayName%22%3A%22STATE%22%2C%22dataType%22%3A%22string%22%2C%22dwColumnId%22%3A1%7D%7D%2C%22list_of_vals%22%3A%7B%22argType%22%3A%22string_list%22%2C%22values%22%3A%5B%22AL%22%2C%22AK%22%2C%22AZ%22%2C%22AR%22%2C%22CA%22%2C%22CO%22%2C%22CT%22%2C%22DE%22%2C%22FL%22%2C%22GA%22%2C%22HI%22%2C%22ID%22%2C%22IL%22%2C%22IN%22%2C%22IA%22%2C%22KS%22%2C%22KY%22%2C%22LA%22%2C%22ME%22%2C%22MD%22%2C%22MA%22%2C%22MI%22%2C%22MN%22%2C%22MS%22%2C%22MO%22%2C%22MT%22%2C%22NE%22%2C%22NV%22%2C%22NH%22%2C%22NJ%22%2C%22NM%22%2C%22NY%22%2C%22NC%22%2C%22ND%22%2C%22OH%22%2C%22OK%22%2C%22OR%22%2C%22PA%22%2C%22RI%22%2C%22SC%22%2C%22SD%22%2C%22TN%22%2C%22TX%22%2C%22UT%22%2C%22VT%22%2C%22VA%22%2C%22WA%22%2C%22WV%22%2C%22WI%22%2C%22WY%22%5D%7D%7D%2C%22transformName%22%3A%22one_hot_encode%22%7D)

结果：

| Babyname | State | State_AL | State_AK | … | State_CO | Qty |
| --- | --- | --- | --- | --- | --- | --- |
| Alice | AL | 1 | 0 | … | 0 | 156 |
| Alice | AK | 0 | 1 | … | 0 | 146 |
| Alice | PA | 0 | 0 | … | 0 | 654 |
| … | … |  |  | … |  | … |
| Zelda | NY | 0 | 0 | … | 0 | 417 |
| Zelda | AL | 1 | 0 | … | 0 | 261 |
| Zelda | CO | 0 | 0 | … | 1 | 321 |

# 市场篮分析

在进行市场篮分析或挖掘关联规则时，第一步通常是将数据格式化以将每笔交易汇总为一个记录。这对你的笔记本电脑来说可能是一个挑战，但你的数据仓库旨在高效地处理这些数据。

典型交易数据：

| 销售订单号 | 客户密钥 | 英文产品名称 | 列表价格 | 重量 | 订单日期 |
| --- | --- | --- | --- | --- | --- |
| SO51247 | 11249 | Mountain-200 黑色 | 2294.99 | 23.77 | 1/1/2013 |
| SO51247 | 11249 | 水瓶 - 30 oz. | 4.99 |  | 1/1/2013 |
| SO51247 | 11249 | Mountain 水瓶架 | 9.99 |  | 1/1/2013 |
| SO51246 | 25625 | Sport-100 头盔 | 34.99 |  | 12/31/2012 |
| SO51246 | 25625 | 水瓶 - 30 oz. | 4.99 |  | 12/31/2012 |
| SO51246 | 25625 | Road 水瓶架 | 8.99 |  | 12/31/2012 |
| SO51246 | 25625 | Touring-1000 蓝色 | 2384.07 | 25.42 | 12/31/2012 |

[市场篮子自定义SQL](https://app.rasgoml.com/sql?transformName=%22market_basket%22&tableState=%7B%22tables%22%3A%5B%7B%22name%22%3A%22My_First_Table%22%2C%22columns%22%3A%5B%7B%22name%22%3A%22SALESORDERNUMBER%22%2C%22dataType%22%3A%22string%22%7D%2C%7B%22name%22%3A%22CUSTOMERKEY%22%2C%22dataType%22%3A%22string%22%7D%2C%7B%22name%22%3A%22ENGLISHPRODUCTNAME%22%2C%22dataType%22%3A%22string%22%7D%2C%7B%22name%22%3A%22LISTPRICE%22%2C%22dataType%22%3A%22float%22%7D%2C%7B%22name%22%3A%22WEIGHT%22%2C%22dataType%22%3A%22float%22%7D%2C%7B%22name%22%3A%22ORDERDATE%22%2C%22dataType%22%3A%22date%22%7D%5D%7D%5D%2C%22baseTableName%22%3A%22My_First_Table%22%2C%22ddl%22%3A%22%22%7D&formState=%7B%22arguments%22%3A%7B%22transaction_id%22%3A%7B%22argType%22%3A%22column%22%2C%22col%22%3A%7B%22id%22%3A0%2C%22columnName%22%3A%22SALESORDERNUMBER%22%2C%22displayName%22%3A%22SALESORDERNUMBER%22%2C%22dataType%22%3A%22string%22%2C%22dwColumnId%22%3A0%7D%7D%2C%22sep%22%3A%7B%22argType%22%3A%22value%22%2C%22value%22%3A%22%2C%20%22%7D%2C%22agg_column%22%3A%7B%22argType%22%3A%22column%22%2C%22col%22%3A%7B%22id%22%3A2%2C%22columnName%22%3A%22ENGLISHPRODUCTNAME%22%2C%22displayName%22%3A%22ENGLISHPRODUCTNAME%22%2C%22dataType%22%3A%22string%22%2C%22dwColumnId%22%3A2%7D%7D%7D%2C%22transformName%22%3A%22market_basket%22%7D)

结果：

| NUMTRANSACTIONS | ENGLISHPRODUCTNAME_LISTAGG |
| --- | --- |
| 207 | 山地瓶架，水瓶 - 30 盎司 |
| 200 | 山地轮胎内胎，修补工具包/8片补丁 |
| 142 | LL 公路轮胎，修补工具包/8片补丁 |
| 137 | 修补工具包/8片补丁，公路轮胎内胎 |
| 135 | 修补工具包/8片补丁，旅行轮胎内胎 |
| 132 | HL 山地轮胎，山地轮胎内胎，修补工具包/8片补丁 |

# 时间序列聚合

时间序列聚合不仅被数据科学家使用，也被用于分析。它们难以处理的原因在于窗口函数要求数据格式正确。

例如，如果你想计算过去14天的平均销售金额，窗口函数要求你将所有销售数据分解为每天一行。不幸的是，任何处理过销售数据的人都知道，销售数据通常是以交易级别存储的。这就是时间序列聚合派上用场的地方。你可以在不重新格式化整个数据集的情况下创建聚合的历史指标。如果我们想一次添加多个指标，这也很有用：

+   过去14天的平均销售

+   过去6个月的最大购买

+   过去90天的不同产品类型数量

如果你想使用窗口函数，每个指标都需要独立构建，并经过多个步骤。

更好的处理方式是使用公共表表达式（CTEs）来定义每个历史窗口，并进行预聚合。

例如：

| 交易ID | 客户ID | 产品类型 | 购买金额 | 交易日期 |
| --- | --- | --- | --- | --- |
| 65432 | 101 | 杂货 | 101.14 | 2022-03-01 |
| 65493 | 101 | 杂货 | 98.45 | 2022-04-30 |
| 65494 | 101 | 汽车 | 239.98 | 2022-05-01 |
| 66789 | 101 | 杂货 | 86.55 | 2022-05-22 |
| 66981 | 101 | 药品 | 14 | 2022-06-15 |
| 67145 | 101 | 杂货 | 93.12 | 2022-06-22 |

[可定制的时间序列聚合 SQL](https://app.rasgoml.com/sql?transformName=%22timeseries_agg%22&tableState=%7B%22tables%22%3A%5B%7B%22name%22%3A%22My_First_Table%22%2C%22columns%22%3A%5B%7B%22name%22%3A%22Transaction_ID%22%2C%22dataType%22%3A%22string%22%7D%2C%7B%22name%22%3A%22Customer_ID%22%2C%22dataType%22%3A%22string%22%7D%2C%7B%22name%22%3A%22Product_Type%22%2C%22dataType%22%3A%22string%22%7D%2C%7B%22name%22%3A%22Purchase_Amt%22%2C%22dataType%22%3A%22float%22%7D%2C%7B%22name%22%3A%22Transaction_Date%22%2C%22dataType%22%3A%22date%22%7D%5D%7D%5D%2C%22baseTableName%22%3A%22My_First_Table%22%2C%22ddl%22%3A%22%22%7D&formState=%7B%22arguments%22%3A%7B%22aggregations%22%3A%7B%22argType%22%3A%22agg_dict%22%2C%22aggDict%22%3A%7B%22Purchase_Amt%22%3A%7B%22AVG%22%3Atrue%2C%22MAX%22%3Atrue%7D%2C%22Transaction_ID%22%3A%7B%22COUNT%20DISTINCT%22%3Atrue%7D%7D%7D%2C%22date%22%3A%7B%22argType%22%3A%22column%22%2C%22col%22%3A%7B%22id%22%3A4%2C%22columnName%22%3A%22Transaction_Date%22%2C%22displayName%22%3A%22Transaction_Date%22%2C%22dataType%22%3A%22date%22%2C%22dwColumnId%22%3A4%7D%7D%2C%22offsets%22%3A%7B%22argType%22%3A%22int_list%22%2C%22values%22%3A%5B14%2C90%2C180%5D%7D%2C%22date_part%22%3A%7B%22argType%22%3A%22date_part%22%2C%22value%22%3A%22day%22%7D%2C%22group_by%22%3A%7B%22argType%22%3A%22column_list%22%2C%22cols%22%3A%5B%7B%22id%22%3A1%2C%22columnName%22%3A%22Customer_ID%22%2C%22displayName%22%3A%22Customer_ID%22%2C%22dataType%22%3A%22string%22%2C%22dwColumnId%22%3A1%7D%5D%7D%7D%2C%22transformName%22%3A%22timeseries_agg%22%7D)

结果：

| 交易 ID | 客户 ID | 产品类型 | 购买金额 | 交易日期 | 过去 14 天的平均销售 | 过去 6 个月的最大购买 | 过去 90 天不同产品类型的计数 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 65432 | 101 | 杂货 | 101.14 | 2022-03-01 | 101.14 | 101.14 | 1 |
| 65493 | 101 | 杂货 | 98.45 | 2022-04-30 | 98.45 | 101.14 | 2 |
| 65494 | 101 | 汽车 | 239.98 | 2022-05-01 | 169.21 | 239.98 | 2 |
| 66789 | 101 | 杂货 | 86.55 | 2022-05-22 | 86.55 | 239.98 | 2 |
| 66981 | 101 | 药品 | 14 | 2022-06-15 | 14 | 239.98 | 3 |
| 67145 | 101 | 杂货 | 93.12 | 2022-06-22 | 53.56 | 239.98 | 3 |

# 结论

我希望这篇文章能够帮助揭示数据从业者在现代数据堆栈操作过程中会遇到的各种问题。SQL 在查询云仓库时是把双刃剑。虽然将计算集中在云数据仓库中可以提高速度，但有时也需要一些额外的 SQL 技能。我希望这篇文章能够回答一些问题，并提供解决这些问题所需的语法和背景知识。

**[Josh Berry](https://www.linkedin.com/in/joshberry022/)** （[**@Twitter**](https://mobile.twitter.com/itsamejoshabee)）领导 Rasgo 的客户数据科学工作，自 2008 年以来一直从事数据和分析工作。Josh 曾在 Comcast 工作了 10 年，在那里他建立了数据科学团队，并且是内部开发的 Comcast 特征商店的关键负责人之一——这是市场上首批推出的特征商店之一。离开 Comcast 后，Josh 在 DataRobot 继续担任客户数据科学团队的重要领导。业余时间，Josh 会对如棒球、F1 赛车、房地产市场预测等有趣的话题进行复杂分析。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT

* * *

### 了解更多相关话题

+   [解决的 5 个棘手 SQL 查询](https://www.kdnuggets.com/2020/11/5-tricky-sql-queries-solved.html)

+   [解决 MySQL 中幻读问题的权威指南](https://www.kdnuggets.com/2022/06/definitive-guide-solving-phantom-read-mysql.html)

+   [思想图谱：大型语言模型中的复杂问题解决新范式](https://www.kdnuggets.com/graph-of-thoughts-a-new-paradigm-for-elaborate-problem-solving-in-large-language-models)

+   [数据科学中的 4 个有用中级 SQL 查询](https://www.kdnuggets.com/2022/12/4-useful-intermediate-sql-queries-data-science.html)

+   [KDnuggets 新闻，12 月 7 日：揭示的十大数据科学误区 • 4…](https://www.kdnuggets.com/2022/n47.html)

+   [如何优化 SQL 查询以更快地检索数据](https://www.kdnuggets.com/2023/06/optimize-sql-queries-faster-data-retrieval.html)
