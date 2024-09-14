# 使用回归模型预测加密货币价格

> 原文：[https://www.kdnuggets.com/2022/05/predicting-cryptocurrency-prices-regression-models.html](https://www.kdnuggets.com/2022/05/predicting-cryptocurrency-prices-regression-models.html)

![使用回归模型预测加密货币价格](../Images/3d4f7e9f8bda70d2c337985befa89fb4.png)

来源: [https://unsplash.com/s/photos/cryptocurrency](https://unsplash.com/s/photos/cryptocurrency)

# 介绍

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT 工作

* * *

截至 2022 年 3 月，加密货币市场市值超过 2 万亿美元 [1]，但仍然极其波动，曾在 2021 年 11 月达到 3 万亿美元。个别加密货币也表现出相同的波动性，仅在过去一个月内，以太坊和比特币分别下降了超过 18% 和 19%。新货币进入市场的数量也在增加，截至 2022 年 3 月，已有超过 18,000 种加密货币存在 [2]。

这种波动性使得长期加密货币预测更加困难。本文将介绍如何使用线性回归模型开始进行加密货币预测。我们将查看多个时间间隔的预测，同时使用各种模型特征，如开盘价、最高价、最低价和交易量。本文中讨论的加密货币包括更为成熟的比特币和以太坊，以及仍处于相对早期阶段的 Polkadot 和 Stellar。

# 方法

在本文中，我们将使用多元线性回归。回归模型用于通过拟合一条线来确定变量之间的关系。简单线性回归是用来预测一个依赖变量（例如加密货币的收盘价），使用一个自变量（如开盘价），而多元线性回归则考虑多个自变量。

我们将使用的数据来自 CoinCodex [3]，提供了每日的开盘价、最高价、最低价和收盘价以及交易量和市值。我们实验了各种特征组合，以生成模型，并对每日和每周的间隔进行了预测。使用了 Python 的 sklearn 包来训练模型，并用 R2 值作为模型准确性的指标，其中 1 表示完美模型。

使用的数据和生成的模型已上传至[Layer项目](https://app.layer.ai/ellies/predictingCryptoPrices)，可以下载以供进一步使用和研究。所有结果和相应图表也可以在[Layer项目](https://app.layer.ai/ellies/predictingCryptoPrices)中找到。本文将讨论实现模型所用代码的各个部分。完整代码可在此[collab notebook](https://colab.research.google.com/drive/1YpCQ2_V3hBjrIk_1plTwA3__w76krRYh#scrollTo=tfl6d3mifG4B)中访问。

为了初始化项目并访问数据和模型，您可以运行：

```py
import layer
layer.login()
layer.init("predictingCryptoPrices")
```

数据被上传到Layer项目中，涵盖了4个加密货币数据集（比特币、以太坊、Polkadot和Stellar），通过定义用于读取数据的函数，并用数据集和资源装饰器进行了注释。

```py
@dataset("bitcoin")
@resources(path="./data")
def getBitcoinData():
    return pd.read_csv("data/bitcoin.csv")

layer.run([getBitcoinData])
```

您可以运行以下命令来访问数据集并将其保存到pandas数据框中。

```py
layer.get_dataset("bitcoin").to_pandas()
layer.get_dataset("ethereum").to_pandas()
layer.get_dataset("polkadot").to_pandas()
layer.get_dataset("stellar").to_pandas()
```

# 预测

## 每日预测

第一组多元回归模型是为了预测每日间隔的价格而构建的。首先使用当天的开盘价、最低价和最高价来预测收盘价。使用了一年的每日价格数据，并进行了75%-25%的训练-测试拆分。以下函数用于训练给定数据集的模型。

```py
def runNoVolumePrediction(dataset):

    # Defining the parameters
    # test_size: proportion of data allocated to testing
    # random_state: ensures the same test-train split each time for reproducibility

    parameters = {
      "test_size": 0.25,
      "random_state": 15,
    }

    # Logging the parameters
    # layer.log(parameters)
    # Loading the dataset from Layer
    df = layer.get_dataset(dataset).to_pandas()
    df.dropna(inplace=True)

    # Dropping columns we won't be using for the predictions of closing price
    dfX = df.drop(["Close", "Date", "Volume", "Market Cap"], axis=1)

    # Getting just the closing price column
    dfy = df["Close"]

    # Test train split (with same random state)
    X_train, X_test, y_train, y_test = train_test_split(dfX, dfy, test_size=parameters["test_size"], random_state=parameters["random_state"])

    # Fitting the multiple linear regression model with the training data
    regressor = LinearRegression()
    regressor.fit(X_train,y_train)

    # Making predictions using the testing data
    predict_y = regressor.predict(X_test)

    # .score returns the coefficient of determination R² of the prediction
    layer.log({"Prediction Score :":regressor.score(X_test,y_test)})

    # Logging the coefficient corresponding to each variable
    coeffs = regressor.coef_
    layer.log({"Opening price coeff":coeffs[0]})
    layer.log({"Low price coeff":coeffs[1]})
    layer.log({"High price coeff":coeffs[2]})

    # Plotting the predicted values against the actual values
    plt.plot(y_test,predict_y, "*")
    plt.ylabel('Predicted closing price')
    plt.xlabel('Actual closing price')
    plt.title("{} closing price prediction".format(dataset))
    plt.show()

    #Logging the plot
    layer.log({"plot":plt})
    return regressor
```

函数运行时使用了模型装饰器，以将模型保存到Layer项目中，如下所示。

```py
@model(name='bitcoin_prediction')
def bitcoin_prediction():
    return runNoVolumePrediction("bitcoin")

layer.run([bitcoin_prediction])
```

要访问此预测集中的所有模型，您可以运行以下代码。

```py
layer.get_model("bitcoin_prediction")
layer.get_model("ethereum_prediction")
layer.get_model("polkadot_prediction")
layer.get_model("stellar_prediction")
```

表1总结了使用该模型的4种加密货币的R2统计数据。

![4种加密货币的R2统计数据](../Images/90c4cd775c1228d08c14e3d1081f7655.png)

表1：使用当天的开盘价、最低价和最高价预测加密货币收盘价的准确性

下面的图表显示了最佳和最差表现的货币Polkadot和Stellar（图1a和1b）的预测值与真实值的对比。

![Stellar预测的收盘价](../Images/745296eed6eeecf78472f4cb778dea97.png)

图1a：Stellar预测的收盘价与当天真实收盘价的对比，使用开盘价、最低价和最高价作为特征！[Polkadot预测的收盘价](../Images/264b3a4fefc056f5dd6f625797344b7e.png)

图1b：Polkadot预测的收盘价与当天真实收盘价的对比，使用开盘价、最低价和最高价作为特征

从图表和R2值中，我们可以看到使用开盘价、最低价和最高价的线性回归模型对于所有4种货币都非常准确，无论它们的成熟程度如何。

接下来，我们研究是否添加交易量可以改进模型。交易量对应于当天的总交易笔数。有趣的是，在使用开盘价、最低价、最高价和交易量拟合回归模型后，结果基本相同。你可以在Layer项目中访问bitcoin_prediction_with_volume、ethereum_prediction_with_volume、polkadot_prediction_with_volume和stellar_prediction_with_volume模型。进一步的调查显示，4种加密货币中交易量变量的系数都是0。这表明，加密货币的交易量并不是预测当天价格的良好指标。

## 每周预测

在下一个模型中，我们查看了使用本周一的开盘价以及本周的最低价和最高价预测周末价格。对于这个模型，使用了过去5年的比特币、以太坊和Stellar的数据。对于2020年才发布的Polkadot，则使用了所有历史数据。

下载数据后，计算了每周的最低价和最高价，并记录了本周的开盘价和收盘价。这些数据集也可以通过运行Layer来访问：

```py
layer.get_dataset("bitcoin_5Years").to_pandas()
layer.get_dataset("ethereum_5Years").to_pandas()
layer.get_dataset("polkadot_5Years").to_pandas()
layer.get_dataset("stellar_5Years").to_pandas()
```

再次使用了75%-25%的训练-测试拆分，并利用一周的开盘价、最高价和最低价拟合了多重线性回归模型。这些模型也可以在Layer项目中访问。R2指标再次用于与下表2中显示的结果进行比较。

![ 使用开盘价、最低价和最高价预测加密货币收盘价的准确性](../Images/4701cb01eb8899d8ee5a66bf97e3b073.png)

表2：使用开盘价、最低价和最高价预测加密货币收盘价的准确性

在用一周的数据进行预测时，模型的准确性略低于用一天的数据进行预测，但准确性仍然非常高。即便是表现最差的加密货币Stellar，其R2分数也达到了0.985。

# 结论

总体而言，使用开盘价、最低价和最高价作为特征的线性回归模型在每日和每周间隔上表现都很好。这些模型的自然扩展是预测更远的未来，例如使用本周的数据预测下周的价格。

[1] [https://fortune.com/2022/03/02/crypto-market-cap-2-trillion/#:~:text=As%20of%2010%3A15%20a.m.,when%20it%20topped%20%243%20trillion](https://fortune.com/2022/03/02/crypto-market-cap-2-trillion/#:~:text=As%20of%2010%3A15%20a.m.,when%20it%20topped%20%243%20trillion)。

[2] [https://www.investopedia.com/tech/most-important-cryptocurrencies-other-than-bitcoin/#:~:text=One%20reason%20for%20this%20is,communities%20of%20backers%20and%20investors](https://www.investopedia.com/tech/most-important-cryptocurrencies-other-than-bitcoin/#:~:text=One%20reason%20for%20this%20is,communities%20of%20backers%20and%20investors)。

[3] https://coincodex.com/crypto/stellar/historical-data/

**[Eleonora Shantsila](https://www.linkedin.com/in/eleonora-shantsila-b51b2b124/)** 是一位全栈软件工程师，目前在名为Lounge的活动初创公司工作，之前曾在金融服务领域担任全栈工程师。Eleonora拥有数学（圣安德鲁斯大学本科）和计算科学（哈佛大学硕士）的背景，闲暇时喜欢从事数据科学项目。欢迎在[LinkedIn上连接](https://www.linkedin.com/in/eleonora-shantsila-b51b2b124/)。

### 更多相关话题

+   [您应该使用线性回归模型而不是…的3个理由](https://www.kdnuggets.com/2021/08/3-reasons-linear-regression-instead-neural-networks.html)

+   [构建预测模型：Python中的逻辑回归](https://www.kdnuggets.com/building-predictive-models-logistic-regression-in-python)

+   [如何让大型语言模型与您的软件友好配合…](https://www.kdnuggets.com/how-to-make-large-language-models-play-nice-with-your-software-using-langchain)

+   [使用大型语言模型时优化性能和成本的策略](https://www.kdnuggets.com/strategies-for-optimizing-performance-and-costs-when-using-large-language-models-in-the-cloud)

+   [比较线性回归和逻辑回归](https://www.kdnuggets.com/2022/11/comparing-linear-logistic-regression.html)

+   [逻辑回归概述](https://www.kdnuggets.com/2022/02/overview-logistic-regression.html)
