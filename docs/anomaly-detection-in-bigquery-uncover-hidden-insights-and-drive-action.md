# BigQuery 中的异常检测：揭示隐藏洞察并驱动行动

> 原文：[`www.kdnuggets.com/anomaly-detection-in-bigquery-uncover-hidden-insights-and-drive-action`](https://www.kdnuggets.com/anomaly-detection-in-bigquery-uncover-hidden-insights-and-drive-action)

![BigQuery 中的异常检测：揭示隐藏洞察并驱动行动](img/4678b39cad81c7ac5ce342d3bce83e15.png)

图片来源于 [starline on Freepik](https://www.freepik.com/free-vector/cloud-computing-polygonal-wireframe-technology-concept_12071198.htm#fromView=search&page=1&position=25&uuid=c85b9e72-78ba-43df-97cf-1432b47a234f)

在大数据和 AI 的时代，异常——与常规的意外偏差——包含了有价值的信息。识别和处理这些异常至关重要。无论是网站流量的突然激增、销售额的异常下跌，还是可疑的交易，检测异常可以让你及早发现问题或机会。

* * *

## 我们的前三推荐课程

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

谷歌云 BigQuery，结合其强大的工具和集成，提供了一个稳健的异常检测平台。[BigQuery](https://cloud.google.com/bigquery/docs/introduction)是一个完全托管的企业数据仓库，帮助你管理和分析数据，内置机器学习、地理空间分析和商业智能等功能。BigQuery 的无服务器架构让你可以使用 SQL 查询来回答组织的重大问题，无需基础设施管理。

让我们探索如何利用 BigQuery 的能力，并深入了解异常检测在行业应用中所带来的实际变化。

## 使用 BigQuery 揭示数据中的异常

+   [**BigQuery ML (BQML)**](https://cloud.google.com/bigquery/docs/bqml-introduction)：这个集成的机器学习服务在 BigQuery 中简化了异常检测。你可以使用像 ARIMA_PLUS 这样的预构建模型进行时间序列数据的分析，或使用 k-means 聚类进行无监督异常检测。只需几行 SQL 代码，你就可以训练模型并获得预测结果。

+   **可视化：** BigQuery 与数据可视化工具如 [Looker Studio](https://cloud.google.com/looker-studio?e=48754805&hl=en)（前身为 Data Studio）无缝集成，使你能够创建仪表板和警报，实时突出显示异常。

## 示例：使用 ARIMA_PLUS 进行时间序列异常检测

假设你正在监测网站流量。流量的突然激增或下降可能表明问题或机会。我们将使用 [BQML 的 ARIMA_PLUS 模型](https://cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-create-time-series)，该模型专为时间序列数据量身定制：

**1. 数据准备：** 确保你的时间序列数据（例如，每小时网站流量）在 BigQuery 表中组织，并且有时间戳列。

**2. 模型训练：** 使用以下 SQL 查询创建和训练你的 ARIMA_PLUS 模型：

```py
CREATE OR REPLACE MODEL `your_project.your_dataset.website_traffic_model`
OPTIONS(model_type='ARIMA_PLUS') AS
SELECT
  DATETIME_TRUNC(timestamp, HOUR) AS timestamp,
  traffic 
FROM `your_project.your_dataset.website_traffic_table`;
```

**3. 异常检测：** 使用你训练的模型，现在可以使用 [ML.DETECT_ANOMALIES](https://cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-detect-anomalies) 函数检测异常。该函数将 [输出](https://cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-detect-anomalies#output) 一个包含异常分数的表，指示数据点为异常的可能性：

```py
SELECT * 
FROM ML.DETECT_ANOMALIES(MODEL `your_project.your_dataset.website_traffic_model`,
                         STRUCT(0.95 AS anomaly_prob_threshold))
```

**4. 可视化和警报：** 使用如 Looker Studio 等工具可视化结果，并设置警报以在出现异常时通知你。

## 异常检测的行业应用

+   **金融服务：**

    +   **欺诈检测：** 识别可能表明欺诈活动的异常交易。

    +   **风险管理：** 检测市场数据中的异常，以管理投资风险。

    +   **反洗钱（AML）：** 发现金融交易中的可疑模式。

    ****电子商务：**

    +   **库存管理：** 监控产品需求和供应链异常，以优化库存水平。

    +   **定价优化：** 识别定价差异或竞争对手定价的突然变化。

    +   **客户行为分析：** 检测客户浏览或购买行为中的异常模式。

    **制造业：**

    +   **预测性维护：** 分析传感器数据，以检测表明设备即将故障的异常。

    +   **质量控制：** 在缺陷影响客户之前识别产品或过程中的缺陷。

    **医疗保健：**

    +   **疾病爆发检测：** 监测公共健康数据，发现疾病爆发的早期迹象。

    +   **病人监测：** 检测生命体征或医疗设备数据中的异常，以提醒医疗服务提供者。

    **IT 运营：**

    +   **网络监控：** 识别可能表明安全威胁或网络问题的异常流量模式。

    +   **系统性能优化：** 检测服务器或应用日志中的异常，以提高系统性能。

    **BigQuery 中异常检测的最佳实践**

    +   **选择正确的算法：** 最佳的异常检测算法取决于你的数据类型（时间序列、分类等）和具体的使用案例。

    +   **数据准备：** 确保你的数据在训练模型前是干净、一致并且格式正确的。

    +   **模型评估：** 持续评估和优化你的异常检测模型，以保持准确性和相关性。

    +   **可操作的警报：** 定义明确的阈值和触发条件，以确保异常情况得到及时处理。

    ## 拥抱异常检测的力量

    异常检测不仅仅是识别离群值；它是揭示隐藏的洞察力，从而推动更好的决策和主动响应。通过利用 BigQuery 强大的功能，你可以将数据转化为有价值的资产，帮助你走在前沿。立即开始探索你所在行业中异常检测的潜力，释放你数据的力量！

    **[尼维达·库玛里](https://www.linkedin.com/in/nivedita-kumari/)** 是一位经验丰富的数据分析和人工智能专家，拥有超过 8 年的经验。在她目前的职位中，作为谷歌的数据分析客户工程师，她不断与 C 级高管互动，帮助他们设计数据解决方案，并指导他们在谷歌云上构建数据和机器学习解决方案的最佳实践。尼维达在伊利诺伊大学厄本那-香槟分校获得了技术管理硕士学位，专注于数据分析。她希望普及机器学习和人工智能，打破技术壁垒，让每个人都能参与这项变革性技术。她通过创建教程、指南、观点文章和编码演示，与开发者社区分享她的知识和经验。[在 LinkedIn 上与尼维达联系](https://www.linkedin.com/in/nivedita-kumari/)。

    ### 更多相关主题

    +   [数据科学中的异常检测技术初学者指南](https://www.kdnuggets.com/2023/05/beginner-guide-anomaly-detection-techniques-data-science.html)

    +   [ChatGPT 驱动的数据探索：解锁数据集中的隐藏洞察](https://www.kdnuggets.com/2023/07/chatgptpowered-data-exploration-unlock-hidden-insights-dataset.html)

    +   [学习数据科学和商业分析，推动创新和增长](https://www.kdnuggets.com/2023/08/learn-data-science-business-analytics-drive-innovation-growth.html)

    +   [数据科学方法推动商业成功](https://www.kdnuggets.com/2023/10/nwu-data-science-methods-drive-business-success)

    +   [Kubernetes 实战：第二版](https://www.kdnuggets.com/2022/03/manning-kubernetes-action-second-edition.html)

    +   [将你的深度学习技能与 R 语言付诸实践！](https://www.kdnuggets.com/2022/08/manning-deep-learning-skills-r-action.html)
