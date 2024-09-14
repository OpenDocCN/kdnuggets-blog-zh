# 欺诈检测系统简介

> 原文：[https://www.kdnuggets.com/2018/08/introduction-fraud-detection-systems.html](https://www.kdnuggets.com/2018/08/introduction-fraud-detection-systems.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由[米格尔·冈萨雷斯-费罗](https://www.linkedin.com/in/miguelgfierro/)，微软**。

欺诈检测是银行和金融机构的首要任务之一，可以通过机器学习来解决。根据[a report published by Nilson](https://nilsonreport.com/upload/content_promo/The_Nilson_Report_Issue_1118.pdf)，2017年全球因卡片欺诈案件造成的损失达到228亿美元。预计这一问题在未来几年将进一步恶化，到2021年，卡片欺诈的损失预计将达到329.6亿美元。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织IT需求

* * *

在本教程中，我们将使用[Kaggle上的信用卡欺诈检测数据集](https://www.kaggle.com/mlg-ulb/creditcardfraud)来识别欺诈案例。我们将使用[梯度提升树](https://blogs.technet.microsoft.com/machinelearning/2017/07/25/lessons-learned-benchmarking-fast-machine-learning-algorithms/)作为机器学习算法。最后，我们将创建一个简单的API来使模型投入实际使用。

我们将使用梯度提升库[LightGBM](https://github.com/Microsoft/LightGBM)，它最近已成为[Kaggle竞赛](https://github.com/Microsoft/LightGBM/tree/a39c848e6456d473d2043dff3f5159945a36b567/examples)顶级参与者中最受欢迎的库之一。

欺诈检测问题以极其不平衡而著称。[Boosting](https://en.wikipedia.org/wiki/Boosting_(machine_learning)) 是一种通常适用于这类数据集的技术。它通过迭代创建弱分类器（决策树），加权实例以提高性能。在第一个子集中，训练和测试一个弱分类器，在所有训练数据上测试表现差的实例权重被提高，以便在下一个数据子集中出现更多。最终，所有分类器都通过加权平均其预测结果进行集成。

在LightGBM中，有一个[参数](https://github.com/Microsoft/LightGBM/blob/master/docs/Parameters.rst#objective-parameters)叫做`is_unbalanced`，可以自动帮助你控制这个问题。

LightGBM 可以在有或没有 [GPU](https://lightgbm.readthedocs.io/en/latest/GPU-Performance.html)的情况下使用。对于像我们这里使用的小数据集，使用CPU更快，因为IO开销较大。然而，我想展示GPU的替代方案，虽然安装更复杂，以便有兴趣的人可以尝试更大的数据集。

在Linux中安装依赖项：

```py
$ sudo apt-get update
$ sudo apt-get install cmake build-essential libboost-all-dev -y
$ conda env create -n fraud -f conda.yaml
$ source activate fraud
(fraud)$ python -m ipykernel install --user --name fraud --display-name "Python (fraud)"
```

```py

import numpy as np
import sys
import os
import json
import pandas as pd
from collections import Counter
import requests
from IPython.core.display import display, HTML
import lightgbm as lgb
import sklearn
import aiohttp
import asyncio
from utils import (split_train_test, classification_metrics_binary, classification_metrics_binary_prob,
                   binarize_prediction, plot_confusion_matrix, run_load_test, read_from_sqlite)
from utils import BASELINE_MODEL, PORT, TABLE_FRAUD, TABLE_LOCATIONS, DATABASE_FILE

print("System version: {}".format(sys.version))
print("Numpy version: {}".format(np.__version__))
print("Pandas version: {}".format(pd.__version__))
print("LightGBM version: {}".format(lgb.__version__))
print("Sklearn version: {}".format(sklearn.__version__))

%load_ext autoreload
%autoreload 2

```

```py

System version: 3.6.0 |Continuum Analytics, Inc.| (default, Dec 23 2016, 13:19:00) [GCC 4.2.1 Compatible Apple LLVM 6.0 (clang-600.0.57)]

Numpy version: 1.13.3

Pandas version: 0.22.0

LightGBM version: 2.1.1

Sklearn version: 0.19.1

```

### 数据集

第一步是加载数据集并分析它。

在继续之前，**你必须运行笔记本 [data_prep.ipynb](https://render.githubusercontent.com/view/data_prep.ipynb)**，这将生成SQLite数据库。

```py

query = 'SELECT * FROM ' + TABLE_FRAUD
df = read_from_sqlite(DATABASE_FILE, query)

```

```py

print("Shape: {}".format(df.shape))
df.head() 

```

```py

Shape: (284807, 31)

```

|  | 时间 | V1 | V2 | V3 | V4 | V5 | V6 | V7 | V8 | V9 | ... | V21 | V22 | V23 | V24 | V25 | V26 | V27 | V28 | 金额 | 类别 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | 0.0 | -1.359807 | -0.072781 | 2.536347 | 1.378155 | -0.338321 | 0.462388 | 0.239599 | 0.098698 | 0.363787 | ... | -0.018307 | 0.277838 | -0.110474 | 0.066928 | 0.128539 | -0.189115 | 0.133558 | -0.021053 | 149.62 | 0 |
| 1 | 0.0 | 1.191857 | 0.266151 | 0.166480 | 0.448154 | 0.060018 | -0.082361 | -0.078803 | 0.085102 | -0.255425 | ... | -0.225775 | -0.638672 | 0.101288 | -0.339846 | 0.167170 | 0.125895 | -0.008983 | 0.014724 | 2.69 | 0 |
| 2 | 1.0 | -1.358354 | -1.340163 | 1.773209 | 0.379780 | -0.503198 | 1.800499 | 0.791461 | 0.247676 | -1.514654 | ... | 0.247998 | 0.771679 | 0.909412 | -0.689281 | -0.327642 | -0.139097 | -0.055353 | -0.059752 | 378.66 | 0 |
| 3 | 1.0 | -0.966272 | -0.185226 | 1.792993 | -0.863291 | -0.010309 | 1.247203 | 0.237609 | 0.377436 | -1.387024 | ... | -0.108300 | 0.005274 | -0.190321 | -1.175575 | 0.647376 | -0.221929 | 0.062723 | 0.061458 | 123.50 | 0 |
| 4 | 2.0 | -1.158233 | 0.877737 | 1.548718 | 0.403034 | -0.407193 | 0.095921 | 0.592941 | -0.270533 | 0.817739 | ... | -0.009431 | 0.798278 | -0.137458 | 0.141267 | -0.206010 | 0.502292 | 0.219422 | 0.215153 | 69.99 | 0 |

5行 × 31列

如我们所见，数据集极为不平衡。少数类别约占0.002%的样本。

```py

df['Class'].value_counts()

```

```py

0    284315
1       492
Name: Class, dtype: int64

```

```py

df['Class'].value_counts(normalize=True)
```

```py

0    0.998273
1    0.001727
Name: Class, dtype: float64

```

下一步是将数据集拆分为训练集和测试集。

```py
X_train, X_test, y_train, y_test = split_train_test(df.drop('Class', axis=1), df['Class'], test_size=0.2)
print(X_train.shape)
print(X_test.shape)
print(y_train.shape)
print(y_test.shape)
```

```py

(227845, 30)
(56962, 30)
(227845,)
(56962,)

```

```py

print(y_train.value_counts())
print(y_train.value_counts(normalize=True))
print(y_test.value_counts())
print(y_test.value_counts(normalize=True))

```

```py

0    227451
1       394
Name: Class, dtype: int64
0    0.998271
1    0.001729
Name: Class, dtype: float64
0    56864
1       98
Name: Class, dtype: int64
0    0.99828
1    0.00172
Name: Class, dtype: float64

```

### 使用LightGBM进行训练 - 基准

对于这个任务，我们使用了一组简单的参数来训练模型。我们只想创建一个基准模型，因此这里没有进行交叉验证或参数调优。

LightGBM的不同参数细节可以在 [文档](https://github.com/Microsoft/LightGBM/blob/master/docs/Parameters.rst) 中找到。此外，作者提供了 [一些建议](https://github.com/Microsoft/LightGBM/blob/master/docs/Parameters-Tuning.rst) 来调整参数并防止过拟合。

```py

lgb_train = lgb.Dataset(X_train, y_train, free_raw_data=False)
lgb_test = lgb.Dataset(X_test, y_test, reference=lgb_train, free_raw_data=False)

```

```py
parameters = {'num_leaves': 2**8,
              'learning_rate': 0.1,
              'is_unbalance': True,
              'min_split_gain': 0.1,
              'min_child_weight': 1,
              'reg_lambda': 1,
              'subsample': 1,
              'objective':'binary',
              #'device': 'gpu', # comment this line if you are not using GPU
              'task': 'train'
              }
num_rounds = 300
```

```py

%%time
clf = lgb.train(parameters, lgb_train, num_boost_round=num_rounds)

```

```py

CPU times: user 45.1 s, sys: 7.68 s, total: 52.8 s
Wall time: 11.9 s

```

一旦我们拥有了训练好的模型，我们可以获得一些指标。

```py

y_prob = clf.predict(X_test)
y_pred = binarize_prediction(y_prob, threshold=0.5)

```

```py

metricsmetrics  ==  classification_metrics_binaryclassifi (y_test, y_pred)
metrics2 = classification_metrics_binary_prob(y_test, y_prob)
metrics.update(metrics2)
cm = metrics['Confusion Matrix']
metrics.pop('Confusion Matrix', None)

```

```py

array([[55773,  1091],
       [   11,    87]])

```

```py

print(json.dumps(metrics, indent=4, sort_keys=True))
plot_confusion_matrix(cm, ['no fraud (negative class)', 'fraud (positive class)'])

```

```py

{
    "AUC": 0.9322482105532139,
    "Accuracy": 0.980653769179453,
    "F1": 0.13636363636363638,
    "Log loss": 0.6375216445628125,
    "Precision": 0.07385398981324279,
    "Recall": 0.8877551020408163
}

```

![混淆矩阵](../Images/9d43e51cb4429246863d109770aa14ee.png)

从商业角度来看，如果系统将一个正常的交易误分类为欺诈（假阳性），银行将会进行调查，可能需要人工干预。根据[2015年Javelin策略报告](https://www.javelinstrategy.com/press-release/false-positive-card-declines-push-consumers-abandon-issuers-and-merchants#)，15%的持卡人在前一年中至少有一笔交易被错误地拒绝，总金额近1180亿美元。近4成被拒绝的持卡人表示，他们在被错误拒绝后放弃了他们的卡。

然而，如果欺诈交易未被检测到，实际上意味着分类器预测交易是正常的，而实际是欺诈性的（假阴性），那么银行就会亏损，而犯罪分子则逃脱了。

在这些预测中使用商业规则的一个常见方法是控制预测的阈值或操作点。这可以通过在`binarize_prediction(y_prob, threshold=0.5)`中更改阈值来控制。通常会从0.1到0.9进行循环，评估不同的商业结果。

```py

clf.save_model(BASELINE_MODEL)

```

### 使用Flask和Websockets进行操作化

下一步是将机器学习模型投入使用。为此，我们将使用[Flask](http://flask.pocoo.org/)来创建一个RESTful API。API的输入将是一个交易（由其特征定义），输出将是模型的预测。

此外，我们设计了一个[websocket服务](https://miguelgfierro.com/blog/2018/demystifying-websockets-for-real-time-web-communication/)来在地图上可视化欺诈交易。系统使用库[flask-socketio](https://github.com/miguelgrinberg/Flask-SocketIO)实时工作。

当新交易被发送到API时，LightGBM模型会预测交易是正常还是欺诈。如果交易是欺诈性的，服务器会向一个web客户端发送信号，客户端渲染出一个世界地图，显示欺诈交易的位置。地图使用[javascript](http://amcharts.com/)和之前创建的SQLite数据库中的位置数据制作。

要启动API，请在conda环境中执行`(fraud)$ python api.py`。

```py

# You can also run the api from inside the notebook (even though I find it more difficult for debugging).
# To do it, just uncomment the next two lines:
#%%bash --bg --proc bg_proc
#python api.py

```

首先，我们确保API正在运行

```py

#server_name = 'http://the-name-of-your-server'
server_name = 'http://localhost'
root_url = '{}:{}'.format(server_name, PORT)
res = requests.get(root_url)
display(HTML(res.text))

```

### 欺诈警察在监视你

![](../Images/f39cf9728096f014c1e8b578120251fb.png)

现在，我们将选择一个值并预测输出。

```py

vals = y_test[y_test == 1].index.values
X_target = X_test.loc[vals[0]]
dict_query = X_target.to_dict()
print(dict_query)

```

```py
{'Time': 57007.0, 'V1': -1.2712441917143702, 'V2': 2.46267526851135, 'V3': -2.85139500331783, 'V4': 2.3244800653477995, 'V5': -1.37224488981369, 'V6': -0.948195686538643, 'V7': -3.06523436172054, 'V8': 1.1669269478721105, 'V9': -2.2687705884481297, 'V10': -4.88114292689057, 'V11': 2.2551474887046297, 'V12': -4.68638689759229, 'V13': 0.652374668512965, 'V14': -6.17428834800643, 'V15': 0.594379608016446, 'V16': -4.8496923870965185, 'V17': -6.53652073527011, 'V18': -3.11909388163881, 'V19': 1.71549441975915, 'V20': 0.560478075726644, 'V21': 0.652941051330455, 'V22': 0.0819309763507574, 'V23': -0.22134783119833895, 'V24': -0.5235821592333061, 'V25': 0.224228161862968, 'V26': 0.756334522703558, 'V27': 0.632800477330469, 'V28': 0.25018709275719697, 'Amount': 0.01}
```

```py

headers = {'Content-type':'application/json'}
end_point = root_url + '/predict'
res = requests.post(end_point, data=json.dumps(dict_query), headers=headers)
print(res.ok)
print(json.dumps(res.json(), indent=2))

```

```py

True
{
  "fraud": 1.0
}

```

### 欺诈交易可视化

现在我们知道API的主要端点工作正常，我们将尝试/predict_map端点。它使用websockets创建一个实时可视化系统来展示欺诈交易。

WebSocket 是一种旨在进行实时通信的协议，为 HTML5 规范开发。它创建了一个持久的、低延迟的连接，可以支持由客户端或服务器发起的事务。[在这篇文章中](https://miguelgfierro.com/blog/2018/demystifying-websockets-for-real-time-web-communication/)，你可以找到关于 WebSocket 和其他相关技术的详细解释。

![](../Images/aa05baa6de9f9f3c3b73020d78f30e87.png)对于我们的案例，每当用户向端点`/predict_map`发出请求时，机器学习模型会评估交易详情并进行预测。如果预测被分类为欺诈，服务器会使用`socketio.emit('map_update', location)`发送信号。该信号仅包含一个名为`location`的字典，包含一个模拟的名称和欺诈交易发生的位置。信号在`index.html`中显示，该文件仅渲染一些通过`id="chartdiv"`引用的 JavaScript 代码。

JavaScript 代码定义在文件`frauddetection.js`中。WebSocket 部分如下：

```py

var mapLocations = [];
// Location updated emitted by the server via websockets
socket.on("map_update", function (msg) {
    var message = "New event in " + msg.title + " (" + msg.latitude
        + "," + msg.longitude + ")";
    console.log(message);
    var newLocation = new Location(msg.title, msg.latitude, msg.longitude);
    mapLocations.push(newLocation);

    clear the markers before redrawing
    mapLocations.forEach(function (location) {
      if (location.externalElement) {
        location.externalElement = undefined;
      }
    });

    map.dataProvider.images = mapLocations;
    map.validateData(); //call to redraw the map with new data
});

```

当服务器在 Python 中发出新的信号时，JavaScript 代码会接收并处理它。它创建了一个名为`newLocation`的新变量，包含要保存到名为`mapLocations`的全局数组中的位置信息。这个变量包含了自会话开始以来出现的所有欺诈位置。接下来，进行一个清理过程，以便 amCharts 能够在地图上绘制新信息，最后将数组存储在`map.dataProvider.images`中，这实际上是用新点刷新地图。变量`map`在代码中较早定义，它是负责定义地图的 amCharts 对象。

要向可视化端点发起查询：

```py

headers = {'Content-type':'application/json'}
end_point_map = root_url + '/predict_map'
res = requests.post(end_point_map, data=json.dumps(dict_query), headers=headers)
print(res.text)

```

```py

True
{
  "fraud": 1.0
}

```

现在你可以访问地图 URL（在本地是 http://localhost:5000/map），查看每次执行前一个单元格时地图如何更新为新的欺诈位置。你应该会看到类似下面的地图：

![](../Images/e4766c0703951537c3484955db166ee2.png)

### 负载测试

一旦我们拥有了 API，我们就可以测试它的可扩展性和响应时间。

这里你可以找到一个简单的负载测试来评估你的 API 性能。请记住，在这种情况下，由于客户端和服务器位于同一台计算机上，因此没有请求开销。

10 个请求的响应时间大约为 300 毫秒，因此一个请求的响应时间为 30 毫秒。

```py

num = 10
concurrent = 2
verbose = True
payload_list = [dict_query]*num

```

```py

%%time
with aiohttp.ClientSession() as session:  # We create a persistent connection
    loop = asyncio.get_event_loop()
    calc_routes = loop.run_until_complete(run_load_test(end_point, payload_list, session, concurrent, verbose))

```

```py
ERROR:asyncio:Creating a client session outside of coroutine
client_session: aiohttp.client.ClientSession object at 0x7f16847333c8

```

```py

Response status: 200
{'fraud': 7.284115783035928e-06}
Response status: 200
{'fraud': 7.284115783035928e-06}
Response status: 200
{'fraud': 7.284115783035928e-06}
Response status: 200
{'fraud': 7.284115783035928e-06}
Response status: 200
Response status: 200
{'fraud': 7.284115783035928e-06}
{'fraud': 7.284115783035928e-06}
Response status: 200
Response status: 200
{'fraud': 7.284115783035928e-06}
{'fraud': 7.284115783035928e-06}
Response status: 200
{'fraud': 7.284115783035928e-06}
Response status: 200
{'fraud': 7.284115783035928e-06}
CPU times: user 14.8 ms, sys: 15.8 ms, total: 30.6 ms
Wall time: 296 ms

```

```py

*# If you run the API from the notebook, you can uncomment the following two lines to kill the process
#%%bash
#ps aux | grep 'api.py' | grep -v 'grep' | awk '{print $2}' | xargs kill*

```

### 企业级欺诈检测参考架构

在本教程中，我们已经看到如何创建一个基线欺诈检测模型。然而，对于大公司来说，这还不够。

在下图中，我们可以看到一个欺诈检测的参考架构，这个架构应根据客户的具体情况进行调整。所有服务都基于 Azure。

1) 客户的两个主要数据源：实时数据和静态信息。

2) 用于存储数据的一般数据库部分。由于这是一个参考架构，并且没有更多数据，我将几个选项组合在一起（[SQL 数据库](https://azure.microsoft.com/en-gb/services/sql-database/)，[CosmosDB](https://azure.microsoft.com/en-gb/services/cosmos-db/)，[SQL 数据仓库](https://azure.microsoft.com/en-gb/services/sql-data-warehouse/)，等）无论是云端还是本地。

3) 使用[Azure ML](https://azure.microsoft.com/en-gb/overview/machine-learning/)进行模型实验，再次使用通用计算目标，如[DSVM](https://azure.microsoft.com/en-gb/services/virtual-machines/data-science-virtual-machines/)，[BatchAI](https://azure.microsoft.com/en-gb/services/batch-ai/)，[Databricks](https://azure.microsoft.com/en-gb/services/databricks/)或[HDInsight](https://azure.microsoft.com/en-gb/services/hdinsight/)。

4) 使用新数据进行模型再训练，并从[模型管理](https://docs.microsoft.com/en-gb/azure/machine-learning/desktop-workbench/model-management-overview)中获得模型。

5) 运营层使用一个[Kubernetes 集群](https://azure.microsoft.com/en-gb/services/container-service/kubernetes/)，将最佳模型投入生产。

6) 报告层以展示结果。

![](../Images/e9a4672215422ba64b766a7a91cb2cd4.png)

[原文](https://github.com/miguelgfierro/sciblog_support/blob/master/Intro_to_Fraud_Detection/fraud_detection.ipynb)。转载经许可。

**相关：**

+   [使用 GRAKN.AI 检测信用卡欺诈数据中的模式](https://www.kdnuggets.com/2017/08/grakn-ai-detect-patterns-credit-fraud-data.html)

+   [用于欺诈检测的 AI —— 万事达卡是如何做到的？了解全球领导者如何使用 AI](https://www.kdnuggets.com/2018/07/rework-ai-fraud-detection-mastercard-global-leaders.html)

+   [渐进增强学习指南与梯度提升](https://www.kdnuggets.com/2018/07/intuitive-ensemble-learning-guide-gradient-boosting.html)

### 更多相关话题

+   [数据科学如何推动欺诈预防](https://www.kdnuggets.com/2022/09/data-science-fuels-fraud-prevention.html)

+   [用 AI 对抗 AI：深度伪造应用的欺诈监控](https://www.kdnuggets.com/2023/05/fighting-ai-ai-fraud-monitoring-deepfake-applications.html)

+   [人工智能系统中的不确定性量化](https://www.kdnuggets.com/2022/04/uncertainty-quantification-artificial-intelligencebased-systems.html)

+   [在业务中实施推荐系统的十个关键经验教训](https://www.kdnuggets.com/2022/07/ten-key-lessons-implementing-recommendation-systems-business.html)

+   [Chip Huyen 分享实施 ML 系统的框架和案例研究](https://www.kdnuggets.com/2023/02/sphere-chip-huyen-shares-frameworks-case-studies-implementing-ml-systems.html)

+   [OLAP 与 OLTP：数据处理系统的比较分析](https://www.kdnuggets.com/2023/08/olap-oltp-comparative-analysis-data-processing-systems.html)
