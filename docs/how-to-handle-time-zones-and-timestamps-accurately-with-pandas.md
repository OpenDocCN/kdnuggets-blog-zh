# 如何准确处理 Pandas 中的时区和时间戳

> 原文：[`www.kdnuggets.com/how-to-handle-time-zones-and-timestamps-accurately-with-pandas`](https://www.kdnuggets.com/how-to-handle-time-zones-and-timestamps-accurately-with-pandas)

![如何准确处理 Pandas 中的时区和时间戳](img/6081ef454ebe2c669b65b374aa987d9b.png)

作者提供的图片 | Midjourney

当我们面对不同的时区时，时间数据可能会变得独特。然而，由于这些差异，解释时间戳可能会很困难。本指南将帮助你使用 Python 中的 Pandas 库管理时区和时间戳。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 加速你的网络安全职业生涯

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 方面

* * *

## 准备工作

在本教程中，我们将使用 Pandas 包。我们可以使用以下代码安装该包。

```py
pip install pandas
```

现在，我们将通过实际示例探讨如何在 Pandas 中处理基于时间的数据。

## 使用 Pandas 处理时区和时间戳

时间数据是一个独特的数据集，为事件提供了特定的时间参考。最准确的时间数据是时间戳，它包含了从年份到毫秒的详细时间信息。

我们开始创建一个示例数据集。

```py
import pandas as pd

data = {
    'transaction_id': [1, 2, 3],
    'timestamp': ['2023-06-15 12:00:05', '2024-04-15 15:20:02', '2024-06-15 21:17:43'],
    'amount': [100, 200, 150]
}

df = pd.DataFrame(data)
df['timestamp'] = pd.to_datetime(df['timestamp'])
```

上述示例中的“timestamp”列包含了秒级精度的时间数据。要将此列转换为日期时间格式，我们应该使用`pd.to_datetime`函数。

之后，我们可以使日期时间数据具有时区意识。例如，我们可以将数据转换为协调世界时间（UTC）。

```py
df['timestamp_utc'] = df['timestamp'].dt.tz_localize('UTC')
print(df)
```

```py
Output>> 
  transaction_id           timestamp  amount             timestamp_utc
0               1 2023-06-15 12:00:05     100 2023-06-15 12:00:05+00:00
1               2 2024-04-15 15:20:02     200 2024-04-15 15:20:02+00:00
2               3 2024-06-15 21:17:43     150 2024-06-15 21:17:43+00:00
```

“timestamp_utc”值包含了大量信息，包括时区。我们可以将现有时区转换为另一个时区。例如，我使用了 UTC 列，并将其转换为日本时区。

```py
df['timestamp_japan'] = df['timestamp_utc'].dt.tz_convert('Asia/Tokyo')
print(df)
```

```py
Output>>>
  transaction_id           timestamp  amount             timestamp_utc  \
0               1 2023-06-15 12:00:05     100 2023-06-15 12:00:05+00:00   
1               2 2024-04-15 15:20:02     200 2024-04-15 15:20:02+00:00   
2               3 2024-06-15 21:17:43     150 2024-06-15 21:17:43+00:00   

            timestamp_japan  
0 2023-06-15 21:00:05+09:00  
1 2024-04-16 00:20:02+09:00  
2 2024-06-16 06:17:43+09:00 
```

我们可以根据这个新的时区来筛选数据。例如，我们可以使用日本时间来筛选数据。

```py
start_time_japan = pd.Timestamp('2024-06-15 06:00:00', tz='Asia/Tokyo')
end_time_japan = pd.Timestamp('2024-06-16 07:59:59', tz='Asia/Tokyo')

filtered_df = df[(df['timestamp_japan'] >= start_time_japan) & (df['timestamp_japan'] <= end_time_japan)]

print(filtered_df)
```

```py
Output>>>
  transaction_id           timestamp  amount             timestamp_utc  \
2               3 2024-06-15 21:17:43     150 2024-06-15 21:17:43+00:00   

            timestamp_japan  
2 2024-06-16 06:17:43+09:00 
```

使用时间序列数据可以让我们进行时间序列重采样。让我们看看在数据集中每列进行按小时重采样的例子。

```py
resampled_df = df.set_index('timestamp_japan').resample('H').count()
```

利用 Pandas 的时区数据和时间戳来充分发挥其功能。

## 额外资源

+   [如何识别时间序列数据集中的缺失数据](https://www.kdnuggets.com/how-to-identify-missing-data-in-timeseries-datasets)

+   [时间序列分析：Python 中的 ARIMA 模型](https://www.kdnuggets.com/2023/08/times-series-analysis-arima-models-python.html)

+   [创建时间序列比率分析仪表盘](https://www.kdnuggets.com/2023/06/wolfer-create-time-series-ratio-analysis-dashboard.html)

**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)** 是一名数据科学助理经理和数据撰稿人。在全职工作于 Allianz Indonesia 的同时，他喜欢通过社交媒体和写作媒体分享 Python 和数据技巧。Cornellius 发表了各种人工智能和机器学习主题的文章。

### 更多相关话题

+   [处理不平衡数据的 7 种技巧](https://www.kdnuggets.com/2017/06/7-techniques-handle-imbalanced-data.html)

+   [KDnuggets 新闻，8 月 31 日：完整的数据科学学习路线图……](https://www.kdnuggets.com/2022/n35.html)

+   [如何使用 Scikit-learn 的 Imputer 模块处理缺失数据](https://www.kdnuggets.com/how-to-handle-missing-data-with-scikit-learns-imputer-module)

+   [NumPy 中的掩码数组以处理缺失数据](https://www.kdnuggets.com/masked-arrays-in-numpy-to-handle-missing-data)

+   [重新定义数据可视化：掌握 Pandas 中基于时间的重采样](https://www.kdnuggets.com/revamping-data-visualization-mastering-timebased-resampling-in-pandas)

+   [滴答：使用 Pendulum 轻松管理 Python 中的日期和时间](https://www.kdnuggets.com/tick-tock-using-pendulum-for-easy-date-and-time-management-in-python)
