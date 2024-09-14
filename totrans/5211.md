# 用 R、SQL 和 Tableau 进行犯罪数据的地理时间序列预测简介

> 原文：[https://www.kdnuggets.com/2020/02/introduction-geographical-time-series-crime-r-sql-tableau.html](https://www.kdnuggets.com/2020/02/introduction-geographical-time-series-crime-r-sql-tableau.html)

[评论](#comments)

**作者：[Jason Wittenauer](https://www.linkedin.com/in/jason-wittenauer-28026110/)，Huron Consulting Group 首席数据科学家**

![图示](../Images/bc2c323f6a4f8e743055941894d91e80.png)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 方面

* * *

### 在本教程中，你将学习如何为时间序列预测准备地理数据。

在审查地理数据时，准备数据进行分析可能会很困难。虽然可以使用特定的算法，但它们的功能可能有限。如果我们花一些时间将数据分割成网格，然后为每个网格点添加一个时间点，这将为建模算法和其他可添加的功能打开更多可能性。然而，这也会造成数据集大小的问题。

五年的犯罪数据集可能包含 250,000 条记录。一旦将其外推到整个城市的时间序列网格中，它可能轻松达到 7500 万个数据点。处理如此规模的数据时，使用数据库来清理数据在将其发送到建模脚本之前是很有帮助的。我们将遵循的步骤如下：

+   将数据导入 SQL Server 数据库。

+   清理和分组数据到地图网格中。

+   为网格数据集添加时间数据点，并填补没有发生犯罪的空白。

+   将数据导入 R

+   运行 XGBoost 模型来确定特定日期犯罪的发生地点

最后，我们将讨论如何在 Tableau 等 BI 工具中使预测结果对最终用户更具可用性。

### 前提条件

在开始本教程之前，你需要：

+   安装 SQL Server Express

+   使用 SQL Management Studio 或类似 IDE 来与 SQL Server 接口

+   已安装 R

+   R Studio、Jupyter notebook 或其他 IDE 来与 R 接口

+   SQL 和 R 的一般工作知识

### 数据流概览

目前我们将保持数据库和数据流的简单结构。数据库本身将非常扁平，最终的数据交接将通过文本文件在 Tableau 仪表板中执行。在更具生产力的版本中，我们会将预测结果保存回数据库，并从数据库中提取数据到报告仪表板中。对于这个示例，我们并不试图完美架构一切，而是了解地理时间序列预测的基本概念。

我们的数据流如下：

![](../Images/bfe4230ca0e6a8e92a27fc5e1955cff9.png)

### 设置数据库

我们的预测模型将使用 2012 年至 2017 年巴尔的摩地区的犯罪数据。这些数据位于此仓库中的“Data”文件夹中，文件名为“Baltimore Incident Data.zip”。在导入这些数据之前，您需要按照以下选项之一来设置数据库：

**选项 1**

+   使用位于“Database Objects\Clean Backup\”文件夹中的备份文件恢复 SQL 数据库。

**选项 2**

+   使用“Database Objects”文件夹中的“Tables”和“Procedures”下的脚本手动创建所有对象。

### 导入数据

数据库成功创建后，您现在可以导入数据。这需要您执行以下操作：

+   解压缩“Data”文件夹中的“Baltimore Incident Data.zip”文件。

+   运行“Insert_StagingCrime”过程，并确保它指向正确的导入文件和正确的格式文件（位于“Data”文件夹中的“FormatFile.fmt”）。

```py
EXEC Insert_StagingCrime 
```

该过程将截断 Staging_Crime 表并通过 BULK INSERT 将文件中的数据直接插入其中。临时表本身具有所有 VARCHAR(MAX) 数据类型，我们将在导入过程的下一阶段将其转换为更好的数据类型。下面是过程的代码片段。

```py
BULK INSERT Staging_Crime 
FROM 'C:\Projects\Crime Prediction\Data\Baltimore Incident Data.csv'
WITH (FIRSTROW = 2, FORMATFILE = 'C:\Projects\Crime Prediction\Data\FormatFile.fmt') 
```

### 复审数据。

数据已导入到临时表中后，您可以在 SQL Management Studio 中运行以下代码查看它：

```py
SELECT TOP 10 *
FROM [dbo].[Staging_Crime] 
```

给你以下结果。

| CrimeDate | CrimeTime | CrimeCode | Address | Description | InsideOutside | Weapon | Post | District | Neighborhood | Location | Premise | TotalIncidents |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 02/28/2017 | 23:50:00 | 6D | 2400 KEYWORTH AVE | LARCENY FROM AUTO | O | NULL | 533 | NORTHERN | Greenspring | (39.3348700000, -76.6590200000) | STREET | 1 |
| 02/28/2017 | 23:36:00 | 4D | 200 DIENER PL | AGG. ASSAULT | I | HANDS | 843 | SOUTHWESTERN | Irvington | (39.2830300000, -76.6878200000) | APT/CONDO | 1 |
| 02/28/2017 | 23:02:00 | 4E | 1800 N MOUNT ST | COMMON ASSAULT | I | HANDS | 742 | WESTERN | Sandtown-Winchester | (39.3092400000, -76.6449800000) | ROW/TOWNHO | 1 |
| 02/28/2017 | 23:00:00 | 6D | 200 S CLINTON ST | LARCENY FROM AUTO | O | NULL | 231 | SOUTHEASTERN | Highlandtown | (39.2894600000, -76.5701900000) | STREET | 1 |
| 02/28/2017 | 22:00:00 | 6E | 1300 TOWSON ST | 偷窃 | O | NULL | 943 | SOUTHERN | Locust Point | (39.2707100000, -76.5911800000) | 街道 | 1 |
| 02/28/2017 | 21:40:00 | 6J | 1000 WILMOT CT | 偷窃 | O | NULL | 312 | EASTERN | Oldtown | (39.2993600000, -76.6034100000) | 街道 | 1 |
| 02/28/2017 | 21:40:00 | 6J | 2400 PENNSYLVANIA AVE | 偷窃 | O | NULL | 733 | WESTERN | Penn North | (39.3094200000, -76.6417700000) | 街道 | 1 |
| 02/28/2017 | 21:30:00 | 5D | 1500 STACK ST | 入室盗窃 | I | NULL | 943 | SOUTHERN | Riverside | (39.2721500000, -76.6033600000) | 其他/住宅 | 1 |
| 02/28/2017 | 21:30:00 | 6D | 2100 KOKO LN | 汽车盗窃 | O | NULL | 731 | WESTERN | Panway/Braddish Avenue | (39.3117800000, -76.6633200000) | 街道 | 1 |
| 02/28/2017 | 21:10:00 | 3CF | 800 W LEXINGTON ST | 商业抢劫 | O | 火器 | 712 | WESTERN | Poppleton | (39.2910500000, -76.6310600000) | 街道 | 1 |

注意到这些数据包括了犯罪发生的时间点、经纬度，甚至还有犯罪类别。这些类别对更高级的建模技术和分析非常有用。

### 清洗数据并创建地图网格

接下来，我们可以将数据转移到一个具有正确数据类型的“犯罪”表中。在此步骤中，我们还将位置字段拆分为经度和纬度字段。完成这些步骤的所有逻辑可以通过运行以下执行语句来完成：

```py
EXEC Insert_Crime 
```

现在，我们将数据填充到“犯罪”表中。这将允许我们完成下一步，即在城市中创建一个网格并将每个犯罪分配到一个网格方块中。你会注意到我们创建了两个网格（小的和大的）。这使我们能够创建在犯罪地点及其周围区域的特征，从而基本上给出城市中的犯罪热点。

![](../Images/8d08d7dfe8dbdc5e32007faf6a1c3b15.png)

运行以下代码以创建网格并为“犯罪”表分配 SmallGridID 和 LargeGridID：

```py
EXEC Update_CrimeCoordinates 
```

这个过程分为三个独立的任务：

1.  在 GridSmall 表中创建地图上的小网格（过程：Insert_GridSmall）。

1.  在 GridLarge 表中创建地图上的大网格（过程：Insert_GridLarge）。

1.  将所有犯罪记录分配到地图上的小方块和大方块中。

创建网格方块的两个过程需要变量来确定地图的角落以及我们希望在地图上有多少个方块。这默认为200x200的小网格和100x100的大网格。

你可以通过运行以下命令查看一些网格数据：

```py
SELECT TOP 10 c.CrimeId, gs.*
FROM [dbo].[Crime] c
JOIN GridSmall gs
    ON gs.GridSmallId = c.GridSmallId 
```

| CrimeId | GridSmallId | BotLeftLatitude | TopRightLatitude | BotLeftLongitude | TopRightLongitude |
| --- | --- | --- | --- | --- | --- |
| 1 | 31012 | 39.3341282000001 | 39.3349965000001 | -76.6594308000001 | -76.6585152000001 |
| 2 | 19121 | 39.2828985 | 39.2837668 | -76.68873 | -76.6878144 |
| 3 | 25198 | 39.3089475000001 | 39.3098158000001 | -76.6456968000001 | -76.6447812000001 |
| 4 | 20657 | 39.2889766 | 39.2898449 | -76.5706176000001 | -76.5697020000001 |
| 5 | 16212 | 39.269874 | 39.2707423 | -76.5916764000001 | -76.5907608000001 |
| 6 | 22832 | 39.2985279000001 | 39.2993962000001 | -76.6035792000001 | -76.6026636000001 |
| 7 | 25202 | 39.3089475000001 | 39.3098158000001 | -76.6420344000001 | -76.6411188000001 |
| 8 | 16601 | 39.2716106 | 39.2724789 | -76.6035792000001 | -76.6026636000001 |
| 9 | 25781 | 39.3115524000001 | 39.3124207000001 | -76.6640088 | -76.6630932 |
| 10 | 20992 | 39.2907132 | 39.2915815 | -76.6319628000001 | -76.6310472000001 |

注意到一个方格中计算了两个点，即右上角和左下角的点。尽管在绘制经纬线时真实地图上确实有一些曲线，我们不必计算方格中的所有四个点。当比例缩小到足够小的时候，我们可以假设这些方格大多是直线的。

### 创建犯罪网格和滞后特征

我们需要完成的最后一步是为我们想要评估的所有时间段创建整个地图网格。在我们的案例中，这意味着每天一个网格点，以确定犯罪是否会发生。要完成这一步，需要执行以下过程：

```py
EXEC Insert_CrimeGrid 
```

在这一步中，每个犯罪事件将被分组到地图方格中，以便我们可以确定在数据集中每个日期发生了多少犯罪事件。我们还需要计算每个方格中没有发生犯罪的所有填充日期，以完成数据集。

这个过程运行的时间比较长。在我的笔记本电脑上，它运行了大约 1 小时，随后生成的表格（“CrimeGrid”）包含了大约 7500 万条记录。好的一点是，输出现在已保存到表格中，因此我们不必在 R 脚本中运行它，那里的大型数据操作可能无法像在数据库中那样高效运行。

在这个步骤中，我们还将创建“滞后特征”。这些将是告诉我们在过去的一天、两天、一周、一月等时间段内，网格方格中发生了多少次犯罪的列。这实际上是帮助我们进行数据的“热点”分析，这可以用来查看附近的其他网格方格，以判断犯罪是否局限于我们的单个方格或是集中在所有附近方格中，类似于地震后的余震。这些特征可能会根据你所做的建模类型而有所不同。

![](../Images/17d4f4d7c22d60ce58296c4584453326.png)

### 预测设置

随着所有数据清理和特征工程在数据库端完成，进行预测的代码相当简单。在我们的示例中，我们将使用 R 中的 XGBoost 分析五年的训练数据来预测未来的犯罪事件。

首先，我们加载我们的库。

```py
# Load required libraries
library(RODBC)
library(xgboost)
library(ROCR)
library(caret) 
```

```py
Loading required package: gplots

Attaching package: 'gplots'

The following object is masked from 'package:stats':

    lowess

Loading required package: lattice
Loading required package: ggplot2
Registered S3 methods overwritten by 'ggplot2':
  method         from 
  [.quosures     rlang
  c.quosures     rlang
  print.quosures rlang 
```

然后直接从数据库中将数据导入 R。你也可以将数据导出为文本文件，并以 CSV 数据的形式读取，如果这是你喜欢的方法。提取数据的查询仅仅是用带有一些日期限制的 SQL 语句编写的。这些查询可以重新编写为从带有日期参数的报告存储过程直接提取数据，以获得更具生产力的代码版本。

```py
# Set seed
set.seed(1001)

# Read in data
dbhandle <- odbcDriverConnect('driver={SQL Server};server=DESKTOP-VLN71V7\\SQLEXPRESS;database=crime;trusted_connection=true')

train <- sqlQuery(dbhandle, 'select IncidentOccurred as target, GridSmallId, GridLargeId, DayOfWeek, MonthOfYear, DayOfYear, Year, PriorIncident1Day, PriorIncident2Days, PriorIncident3Days, PriorIncident7Days, PriorIncident14Days, PriorIncident30Days, PriorIncident1Day_Large, PriorIncident2Days_Large, PriorIncident3Days_Large, PriorIncident7Days_Large, PriorIncident14Days_Large, PriorIncident30Days_Large from crimegrid where crimedate <= \'2/20/2017\' and crimedate >= \'6/1/2012\'')
test <- sqlQuery(dbhandle, 'select IncidentOccurred as target, GridSmallId, GridLargeId, DayOfWeek, MonthOfYear, DayOfYear, Year, PriorIncident1Day, PriorIncident2Days, PriorIncident3Days, PriorIncident7Days, PriorIncident14Days, PriorIncident30Days, PriorIncident1Day_Large, PriorIncident2Days_Large, PriorIncident3Days_Large, PriorIncident7Days_Large, PriorIncident14Days_Large, PriorIncident30Days_Large from crimegrid where crimedate >= \'2/21/2017\' and crimedate <= \'2/27/2017\'')

# Convert integers to numeric for DMatrix
train[] <- lapply(train, as.numeric)
test[] <- lapply(test, as.numeric)

head(train) 
```

| 目标 | GridSmallId | GridLargeId | 星期几 | 年月份 | 年中的天数 | 年 | 过去1天事件 | 过去2天事件 | 过去3天事件 | 过去7天事件 | 过去14天事件 | 过去30天事件 | 过去1天事件_大 | 过去2天事件_大 | 过去3天事件_大 | 过去7天事件_大 | 过去14天事件_大 | 过去30天事件_大 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | 3780 | 990 | 5 | 9 | 262 | 2013 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 0 | 3781 | 991 | 5 | 9 | 262 | 2013 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 0 | 3782 | 991 | 5 | 9 | 262 | 2013 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 0 | 3783 | 992 | 5 | 9 | 262 | 2013 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 0 | 3784 | 992 | 5 | 9 | 262 | 2013 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 0 | 3785 | 993 | 5 | 9 | 262 | 2013 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |

注意在测试数据集中，我们一次提取 7 天的数据进行测试，但仍然保持特征输入时仿佛我们知道前一天发生了什么。这只是为了便于我们一次测试 7 天的数据，并在最后的报告中查看多天预测的效果。

另一个需要注意的是，标记为“_Large”的特征是针对较大的网格的，而不是较小的网格。

### 创建特征集和模型参数

我们将特征定义为 SQL 查询中第一个“目标”列之后的所有列。这些特征会作为标签的一部分传递给训练和测试数据集。

```py
# Get feature names (all but first column which is the target)
feature.names <- names(train)[2:ncol(train)]

print(feature.names)

# Make train and test matrices
dtrain <- xgb.DMatrix(data.matrix(train[,feature.names]), label=train$target)
dtest <- xgb.DMatrix(data.matrix(test[,feature.names]), label=test$target) 
```

```py
 [1] "GridSmallId"               "GridLargeId"              
 [3] "DayOfWeek"                 "MonthOfYear"              
 [5] "DayOfYear"                 "Year"                     
 [7] "PriorIncident1Day"         "PriorIncident2Days"       
 [9] "PriorIncident3Days"        "PriorIncident7Days"       
[11] "PriorIncident14Days"       "PriorIncident30Days"      
[13] "PriorIncident1Day_Large"   "PriorIncident2Days_Large" 
[15] "PriorIncident3Days_Large"  "PriorIncident7Days_Large" 
[17] "PriorIncident14Days_Large" "PriorIncident30Days_Large" 
```

接下来，设置模型训练的参数。我们目前保持它们相当基础。确保评估指标是 AUC，以便我们可以尽量提高我们的真正阳性率。这将防止警察不必要地巡逻某些区域。可能还需要额外的工作来用巧妙的巡逻覆盖所有预测区域，但我们在本教程中不涉及这些内容。

```py
# Training parameters
watchlist <- list(eval = dtest, train = dtrain)

param <- list(  objective           = "binary:logistic", 
                booster             = "gbtree",
                eta                 = 0.01,
                max_depth           = 10,
                eval_metric         = "auc"
) 
```

### 运行模型

既然一切都已设置好，我们可以运行模型并查看其预测效果。请记住，这里使用的是一个相当基本的特征集，仅仅是为了演示处理地理时间序列数据的一种方法。虽然数据集可能会非常庞大，但它们易于理解，并且在使用像 AWS 这样的云服务时运行速度相当快。

这个特定的训练数据集有 7000 万行，在我的笔记本电脑上完成 67 轮评估大约花了 30 分钟。

```py
# Run model
clf <- xgb.train(   params                  = param, 
                    data                    = dtrain, 
                    nrounds                 = 100, 
                    verbose                 = 2, 
                    early_stopping_rounds   = 10,
                    watchlist               = watchlist,
                    maximize               = TRUE) 
```

```py
[14:30:32] WARNING: amalgamation/../src/learner.cc:686: Tree method is automatically selected to be 'approx' for faster speed. To use old behavior (exact greedy algorithm on single machine), set tree_method to 'exact'.
[1]	eval-auc:0.858208	train-auc:0.858555 
Multiple eval metrics are present. Will use train_auc for early stopping.
Will train until train_auc hasn't improved in 10 rounds.

[2]	eval-auc:0.858208	train-auc:0.858555 
[3]	eval-auc:0.858208	train-auc:0.858555 
[4]	eval-auc:0.858208	train-auc:0.858556 
[5]	eval-auc:0.858208	train-auc:0.858556 
[6]	eval-auc:0.858208	train-auc:0.858556 
[7]	eval-auc:0.858311	train-auc:0.858997 
[8]	eval-auc:0.858311	train-auc:0.858997 
[9]	eval-auc:0.858311	train-auc:0.858997 
[10]	eval-auc:0.858315	train-auc:0.859000 
[11]	eval-auc:0.858436	train-auc:0.859110 
[12]	eval-auc:0.858436	train-auc:0.859110 
[13]	eval-auc:0.858512	train-auc:0.859157 
[14]	eval-auc:0.858493	train-auc:0.859157 
[15]	eval-auc:0.858496	train-auc:0.859160 
[16]	eval-auc:0.858498	train-auc:0.859160 
[17]	eval-auc:0.858498	train-auc:0.859160 
[18]	eval-auc:0.858342	train-auc:0.859851 
[19]	eval-auc:0.858177	train-auc:0.859907 
[20]	eval-auc:0.858228	train-auc:0.859971 
[21]	eval-auc:0.858231	train-auc:0.859971 
[22]	eval-auc:0.858206	train-auc:0.860695 
[23]	eval-auc:0.858207	train-auc:0.860695 
[24]	eval-auc:0.858731	train-auc:0.860894 
[25]	eval-auc:0.858702	train-auc:0.860844 
[26]	eval-auc:0.858607	train-auc:0.860844 
[27]	eval-auc:0.858574	train-auc:0.860842 
[28]	eval-auc:0.858602	train-auc:0.860892 
[29]	eval-auc:0.858576	train-auc:0.860843 
[30]	eval-auc:0.858574	train-auc:0.860841 
[31]	eval-auc:0.858607	train-auc:0.860893 
[32]	eval-auc:0.858578	train-auc:0.860843 
[33]	eval-auc:0.858611	train-auc:0.860894 
[34]	eval-auc:0.858612	train-auc:0.860895 
[35]	eval-auc:0.858614	train-auc:0.860898 
[36]	eval-auc:0.858615	train-auc:0.860899 
[37]	eval-auc:0.858616	train-auc:0.860897 
[38]	eval-auc:0.858573	train-auc:0.860870 
[39]	eval-auc:0.858546	train-auc:0.860822 
[40]	eval-auc:0.858575	train-auc:0.860872 
[41]	eval-auc:0.858622	train-auc:0.860898 
[42]	eval-auc:0.858578	train-auc:0.860875 
[43]	eval-auc:0.858583	train-auc:0.860870 
[44]	eval-auc:0.859223	train-auc:0.861768 
[45]	eval-auc:0.859220	train-auc:0.861760 
[46]	eval-auc:0.859221	train-auc:0.861760 
[47]	eval-auc:0.859099	train-auc:0.861719 
[48]	eval-auc:0.859112	train-auc:0.861735 
[49]	eval-auc:0.859112	train-auc:0.861735 
[50]	eval-auc:0.859094	train-auc:0.861734 
[51]	eval-auc:0.859125	train-auc:0.861785 
[52]	eval-auc:0.859021	train-auc:0.861771 
[53]	eval-auc:0.859028	train-auc:0.861784 
[54]	eval-auc:0.859029	train-auc:0.861781 
[55]	eval-auc:0.859028	train-auc:0.861784 
[56]	eval-auc:0.859035	train-auc:0.861788 
[57]	eval-auc:0.859037	train-auc:0.861789 
[58]	eval-auc:0.859035	train-auc:0.861775 
[59]	eval-auc:0.859035	train-auc:0.861774 
[60]	eval-auc:0.859010	train-auc:0.861738 
[61]	eval-auc:0.859011	train-auc:0.861739 
[62]	eval-auc:0.859039	train-auc:0.861778 
[63]	eval-auc:0.859016	train-auc:0.861739 
[64]	eval-auc:0.859017	train-auc:0.861741 
[65]	eval-auc:0.859018	train-auc:0.861746 
[66]	eval-auc:0.859019	train-auc:0.861747 
[67]	eval-auc:0.859024	train-auc:0.861755 
Stopping. Best iteration:
[57]	eval-auc:0.859037	train-auc:0.861789 
```

模型很快找到了模式，每轮改进不大。这可以通过更好的模型参数和更多特征来提升。

### 审查重要性矩阵

现在是分析我们的特征，看看哪些在模型中评分较高的时候了，通过查看重要性矩阵。这可以帮助我们确定新增的特征在大型数据集上是否真的值得（较大数据集 = 每增加一个特征的额外处理时间）。

```py
# Compute feature importance matrix
importance_matrix <- xgb.importance(feature.names, model = clf)

# Graph important features
xgb.plot.importance(importance_matrix[1:10,]) 
```

![](../Images/b1cbdda4a2704fd96dfb44254024086d.png)

看起来模型使用了小网格和大网格特征的组合来判断当前日是否会发生事件。有趣的是，长期特征似乎更重要，表明该区域及其周边地区存在犯罪活动历史。

### 在测试集上预测并检查ROC曲线

最后一步是将我们的预测与测试数据集进行对比，看看效果如何。我们总是希望ROC曲线会快速而高地跃升，但这并非总是如此。在我们的例子中，只有基本特征包含在模型中，我们得到了一个不错的得分。这确实表明我们可以进行预测，并且应该投入更多时间来提高预测的准确性。

```py
# Predict on test data
preds <- predict(clf, dtest)

# Graph AUC curve
xgb.pred <- prediction(preds, test$target)
xgb.perf <- performance(xgb.pred, "tpr", "fpr")

plot(xgb.perf,
     avg="threshold",
     colorize=TRUE,
     lwd=1,
     main="ROC Curve w/ Thresholds",
     print.cutoffs.at=seq(0, 1, by=0.05),
     text.adj=c(-0.5, 0.5),
     text.cex=0.5)
grid(col="lightgray")
axis(1, at=seq(0, 1, by=0.1))
axis(2, at=seq(0, 1, by=0.1))
abline(v=c(0.1, 0.3, 0.5, 0.7, 0.9), col="lightgray", lty="dotted")
abline(h=c(0.1, 0.3, 0.5, 0.7, 0.9), col="lightgray", lty="dotted")
lines(x=c(0, 1), y=c(0, 1), col="black", lty="dotted") 
```

![](../Images/8aed21f0ee9d475c63d428d2786b4f2a.png)

### 审查混淆矩阵

我们知道有一个不错的AUC得分，但让我们看看我们预测的结果与实际发生的情况。最简单的方法是审查混淆矩阵。我们希望左上角和右下角的框（正确预测）较大，其他框（错误预测）较小。

```py
# Set our cutoff threshold
preds.resp <- ifelse(preds >= 0.5, 1, 0)

# Create the confusion matrix
confusionMatrix(as.factor(preds.resp), as.factor(test$target), positive = "1") 
```

```py
Confusion Matrix and Statistics

          Reference
Prediction      0      1
         0 280581    454
         1     62    367

               Accuracy : 0.9982         
                 95% CI : (0.998, 0.9983)
    No Information Rate : 0.9971         
    P-Value [Acc > NIR] : < 2.2e-16      

                  Kappa : 0.5864         

 Mcnemar's Test P-Value : < 2.2e-16      

            Sensitivity : 0.447016       
            Specificity : 0.999779       
         Pos Pred Value : 0.855478       
         Neg Pred Value : 0.998385       
             Prevalence : 0.002917       
         Detection Rate : 0.001304       
   Detection Prevalence : 0.001524       
      Balanced Accuracy : 0.723397       

       'Positive' Class : 1 
```

在7天内，发生了821起事件，我们的模型正确预测了367起。另一个关键点是我们错误预测了62起事件，这基本上是将警察派往我们认为可能发生犯罪的地区，但实际上没有发生。从表面上看，这似乎还不算太糟，但我们需要查看这对犯罪活动响应时间的影响与当前响应时间的对比。目标是使警察在足够接近犯罪发生的区域，以便他们可以通过存在预防犯罪或快速响应尽可能减少伤害。

### 准备报告数据

接下来，我们可以为测试数据集添加经度和纬度坐标，并将其导出为CSV文件。这将使我们能够在像Tableau这样的BI工具中查看实际预测结果。

```py
# Read in the grid coordinates
gridsmall <- sqlQuery(dbhandle, 'select * from gridsmall')

# Merge the predictions with the test data
results <- cbind(test, preds)

# Merge the grid coordinates with the test data
results <- merge(results, gridsmall, by="GridSmallId")

head(results)

# Save to file
write.csv(results,"Data\\CrimePredictions.csv", row.names = TRUE) 
```

| GridSmallId | 目标 | GridLargeId.x | 星期几 | 月份 | 年中的天数 | 年份 | 前一天事件 | 前两天事件 | 前三天事件 | ... | 前三天事件_大 | 前七天事件_大 | 前十四天事件_大 | 前三十天事件_大 | 预测值 | 左下纬度 | 右上纬度 | 左下经度 | 右上经度 | GridLargeId.y |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | 0 | 1 | 3 | 2 | 52 | 2017 | 0 | 0 | 0 | ... | 0 | 0 | 0 | 0 | 0.2822689 | 39.20041 | 39.20128 | -76.71162 | -76.7107 | 1 |
| 1 | 0 | 1 | 5 | 2 | 54 | 2017 | 0 | 0 | 0 | ... | 0 | 0 | 0 | 0 | 0.2822689 | 39.20041 | 39.20128 | -76.71162 | -76.7107 | 1 |
| 1 | 0 | 1 | 7 | 2 | 56 | 2017 | 0 | 0 | 0 | ... | 0 | 0 | 0 | 0 | 0.2822689 | 39.20041 | 39.20128 | -76.71162 | -76.7107 | 1 |
| 1 | 0 | 1 | 4 | 2 | 53 | 2017 | 0 | 0 | 0 | ... | 0 | 0 | 0 | 0 | 0.2822689 | 39.20041 | 39.20128 | -76.71162 | -76.7107 | 1 |
| 1 | 0 | 1 | 2 | 2 | 58 | 2017 | 0 | 0 | 0 | ... | 0 | 0 | 0 | 0 | 0.2822689 | 39.20041 | 39.20128 | -76.71162 | -76.7107 | 1 |
| 1 | 0 | 1 | 1 | 2 | 57 | 2017 | 0 | 0 | 0 | ... | 0 | 0 | 0 | 0 | 0.2822689 | 39.20041 | 39.20128 | -76.71162 | -76.7107 | 1 |

### 预测可视化

现在是时候查看这些预测在7天内实际的情况了。Tableau仪表板可以在这里找到：

+   [犯罪预测仪表板](https://public.tableau.com/profile/jason.wittenauer#!/vizhome/CrimePrediction_15793180506290/CrimeMap)

在查看错误预测（红点）时，它们看起来非常接近正确预测。这将使警察处于犯罪发生的一般附近。所以，错误预测可能对整体结果的影响不大。下面是显示正确预测（蓝色）和错误预测（红色）的示例截图。

**错误预测**

![](../Images/216313171b15f9150960b9cb675e7d6d.png)

**错误和正确预测**

![](../Images/189e5980bb99f292bce1fae6eaed223e.png)

总体来看情况似乎还不错，但我们需要更多的特征和/或更多的数据来捕捉所有缺失的预测。此外，我们可能还可以做更多的工作，专注于发生的特定犯罪类型，并针对每种类型进行具体的预测建模。

### 下一步是什么？

本教程让我们开始使用犯罪数据进行地理时间序列预测。我们可以看到预测确实有效，但在创建特征方面还有更多工作要做。我们可能需要添加一些其他特征，检查更大范围的犯罪发生情况。另一个有用的步骤是将预测频率改为每小时，并按时间段绘制巡逻车路线。即使预测不完美，只要你将警察放在犯罪的附近，这比现有的巡逻方法要好，他们可以更快响应，甚至通过他们的存在防止犯罪的发生。

其他值得考虑的想法：

+   去除警察局、消防局、医院等附近的犯罪数据，因为这些数据可能对报告提交者有偏见。

+   添加与人口普查信息相关的统计特征。

+   将犯罪映射到社区，而不是方格网格，以进行预测。

希望你喜欢这个教程！

**简介： [Jason Wittenauer](https://www.linkedin.com/in/jason-wittenauer-28026110/)** 是一位数据科学家，专注于通过 R、Python、Microsoft SQL Server、Tableau、TIBCO Spotfire 和 .NET 网页编程提升医院收入。他在医疗保健领域的关注点包括业务运营、收入改善和费用减少。

[原文](https://github.com/jasonwi1202/Crime-Prediction/blob/master/Crime%20Prediction.ipynb)。经许可转载。

**相关：**

+   [使用时间序列分析进行股票市场预测](/2020/01/stock-market-forecasting-time-series-analysis.html)

+   [你需要知道的：现代开源数据科学/机器学习生态系统](/2019/06/top-data-science-machine-learning-tools.html)

+   [正义不能盲目：如何通过预测警务对抗偏见](/2018/02/fight-bias-predictive-policing.html)

### 更多相关话题

+   [使用 BQML 进行多变量时间序列预测](https://www.kdnuggets.com/2023/07/multivariate-timeseries-prediction-bqml.html)

+   [用 SQL 处理时间序列中的缺失值](https://www.kdnuggets.com/2022/09/handling-missing-values-timeseries-sql.html)

+   [使用 Tableau 创建高效的联合数据源](https://www.kdnuggets.com/2022/05/create-efficient-combined-data-sources-tableau.html)

+   [为有效的 Tableau 和 Power BI 仪表板准备数据](https://www.kdnuggets.com/2022/06/prepare-data-effective-tableau-power-bi-dashboards.html)

+   [最常用的 10 个 Tableau 函数](https://www.kdnuggets.com/2022/08/10-used-tableau-functions.html)

+   [KDnuggets 新闻，8月3日：最常用的 10 个 Tableau 函数 • 是…](https://www.kdnuggets.com/2022/n31.html)
