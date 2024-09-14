# 使用时间序列分析预测电力消耗

> 原文：[https://www.kdnuggets.com/2020/01/predict-electricity-consumption-time-series-analysis.html](https://www.kdnuggets.com/2020/01/predict-electricity-consumption-time-series-analysis.html)

[评论](#comments)![图](../Images/c248c9e12ad62e45724654ee5e058261.png)

图片来源：[https://gfycat.com/frailofficialdegus](https://gfycat.com/frailofficialdegus)

### 介绍

> “时间序列模型用于基于定期观察到的事件（以及收集的数据）预测未来事件。”

我们将处理一个小的预测问题，并尝试解决它，过程中学习时间序列预测。

### 什么是时间序列分析

时间序列预测是一种通过时间序列预测事件的技术。该技术在许多研究领域中使用，从地质学到行为学再到经济学。这些技术通过[分析](https://searchbusinessanalytics.techtarget.com/definition/predictive-analytics)过去的趋势来预测未来事件，假设未来趋势将与历史趋势类似。

时间序列预测在各种应用中执行，包括：

+   天气预测

+   地震预测

+   天文学

+   统计学

+   数学金融

+   计量经济学

+   模式识别

+   [信号](https://searchnetworking.techtarget.com/definition/signal)处理

+   控制工程

时间序列预测有时只是专家研究某个领域并提供预测的分析。然而，在许多现代应用中，时间序列预测利用计算机技术，包括：

+   [机器学习](https://searchenterpriseai.techtarget.com/definition/machine-learning-ML)

+   [人工神经网络](https://searchenterpriseai.techtarget.com/definition/neural-network)

+   [支持向量机](https://whatis.techtarget.com/definition/support-vector-machine-SVM)

+   [模糊逻辑](https://searchenterpriseai.techtarget.com/definition/fuzzy-logic)

+   高斯过程

+   隐藏[马尔可夫模型](https://whatis.techtarget.com/definition/Markov-model)

时间序列分析的两个主要目标是：（a）识别由观察序列表示的现象的性质，以及（b）预测（预测时间序列变量的未来值）。这两个目标都要求识别并或多或少地正式描述观察到的时间序列数据的模式。一旦模式建立，我们可以将其与其他数据解释和整合（即，在我们调查现象的理论中使用，例如，季节性商品价格）。无论我们对现象的理解深度和解释（理论）的有效性如何，我们都可以将识别出的模式外推以预测未来事件。

### 时间序列预测的阶段

解决时间序列问题与常规建模任务略有不同。解决时间序列问题的简单/基本过程可以通过以下步骤来演示。我们将了解每个阶段需要执行的任务。我们还将查看我们解决问题的每个阶段的Python实现。

步骤是—

**1\. 可视化时间序列**

在这一步中，我们尝试可视化序列。我们尝试识别与序列相关的所有潜在模式，如趋势和季节性。现在不必担心这些术语，因为我们将在实现过程中讨论它们。可以说，这更像是一种对时间序列数据的探索性分析。

**2\. 时间序列平稳化**

平稳时间序列是指其统计属性（如均值、方差、自相关等）随时间保持不变的序列。大多数统计预测方法基于这样的假设：时间序列可以通过数学变换大致平稳化（即“平稳化”）。平稳化的序列相对容易预测：你只需预测其统计属性在未来与过去相同！另一个尝试平稳化时间序列的原因是能够获得有意义的样本统计量，如均值、方差和与其他变量的相关性。这些统计量仅在序列平稳时才对未来行为有描述意义。例如，如果序列随着时间的推移持续增加，样本均值和方差将随着样本量的增加而增长，并且它们总是会低估未来时期的均值和方差。如果序列的均值和方差不明确，那么它与其他变量的相关性也不明确。

**3\. 为我们的模型寻找最佳参数**

我们需要为预测模型找到最佳参数，当我们拥有一个平稳序列时。这些参数来自ACF和PACF图。因此，这个阶段更多的是关于绘制上述两个图形，并基于它们提取最佳模型参数。不要担心，我们将在下面的实现部分中介绍如何确定这些参数！

**4\. 拟合模型**

一旦我们拥有了最佳模型参数，我们可以拟合一个ARIMA模型来学习序列的模式。始终记住，时间序列算法仅适用于平稳数据，因此使序列平稳是一个重要的方面。

**5\. 预测**

在拟合模型后，我们将在此阶段进行未来预测。由于我们现在对解决时间序列问题的基本流程比较熟悉，让我们进入实现部分。

### 问题陈述

数据集可以从[**这里**](https://drive.google.com/open?id=1051N7h_L7XUXJU1ztSvWTV2VMb6yZfnD)下载。数据集仅包含2列，一列是日期，另一列与消费百分比有关。

它展示了1985年至2018年的电力消耗情况。目标是预测接下来6年的电力消耗，即直到2024年。

加载数据集

```py
import warnings
warnings.filterwarnings('ignore')
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
plt.style.use('fivethirtyeight')
# Above is a special style template for matplotlib, highly useful for visualizing time series data
from pylab import rcParams
rcParams['figure.figsize'] = 10, 7df = pd.read_csv('/Users/.../.../.../Electric_consumption.csv')
```

现在，定义列名，删除空值，将日期转换为DateTime格式，并将日期设置为索引列，因为没有索引就无法绘制图表。

```py
df.columns=['Date', 'Consumption']
df=df.dropna()
df['Date'] = pd.to_datetime(df['Date'])
df.set_index('Date', inplace=True) #set date as index
df.head()
```

![图](../Images/c25cd4ba5e6409f69067c0f9244d3d0b.png)

数据集

现在，让我们开始按照预定义的步骤进行：

**1\. 可视化时间序列。**

```py
plt.xlabel("Date")
plt.ylabel("Consumption")
plt.title("production graph")
plt.plot(df)
```

![图](../Images/dfb3a46d2347daf5c5940c9d90fd36e8.png)

原始时间序列数据点

记住，对于时间序列预测，序列需要是平稳的。序列应具有恒定的均值、方差和协方差。

这里需要注意几个点，均值在这种情况下不是恒定的，因为我们可以清楚地看到上升趋势。

因此，我们已确定我们的序列不是平稳的。我们需要一个平稳的序列来进行时间序列预测。在下一阶段，我们将尝试将其转换为平稳序列。

让我们绘制散点图：

```py
df.plot(style='k.')
plt.show()
```

![图](../Images/d4c4d12bc48e1a6e4810b91ee0f7f97b.png)

时间序列数据点的散点图

我们还可以通过分布来可视化我们序列中的数据。

![](../Images/b20e8147afe05a4d403c25853f451827.png)

我们可以观察到消费值近似正态分布（钟形曲线）。

此外，给定的时间序列被认为包含三个系统性组件，包括水平、趋势、季节性，以及一个非系统性组件称为噪声。

这些组件定义如下：

+   **水平**：序列中的平均值。

+   **趋势**：序列中增加或减少的值。

+   **季节性**：序列中重复的短期周期。

+   **噪声**：序列中的随机变化。

为了进行时间序列分析，我们可能需要从序列中分离出季节性和趋势。通过这一过程，结果序列将变得平稳。

所以，让我们从时间序列中分离趋势和季节性。

```py
from statsmodels.tsa.seasonal import seasonal_decompose
result = seasonal_decompose(df, model='multiplicative')
result.plot()
plt.show()
```

![](../Images/5c0775018ed3c73952f4e52546c471fe.png)

这可以让我们更深入了解数据和现实世界的行为。显然，存在上升趋势和每年电力消费达到最高的重复事件。

**2\. 使时间序列平稳。**

首先，我们需要检查序列是否平稳。

**ADF（增强型Dickey-Fuller）检验**

Dickey-Fuller检验是最受欢迎的统计检验之一。它可以用来确定序列中是否存在单位根，从而帮助我们理解序列是否平稳。该检验的原假设和备择假设如下：

**原假设：** 序列具有单位根（a=1的值）

**备择假设：** 序列没有单位根。

如果我们未能拒绝原假设，则可以说该序列是非平稳的。这意味着该序列可以是线性平稳或差分平稳的（我们将在下一部分了解更多关于差分平稳的内容）。

如果均值和标准差都是平坦的线（均值恒定且方差恒定），则序列变为平稳。

以下函数可以绘制一个序列及其滚动均值和标准差。

```py
from statsmodels.tsa.stattools import adfuller
def test_stationarity(timeseries):
    #Determing rolling statistics
    rolmean = timeseries.rolling(12).mean()
    rolstd = timeseries.rolling(12).std()
    #Plot rolling statistics:
    plt.plot(timeseries, color='blue',label='Original')
    plt.plot(rolmean, color='red', label='Rolling Mean')
    plt.plot(rolstd, color='black', label = 'Rolling Std')
    plt.legend(loc='best')
    plt.title('Rolling Mean and Standard Deviation')
    plt.show(block=False)

    #perform dickey fuller test  
    print("Results of dickey fuller test")
    adft = adfuller(timeseries['Consumption'],autolag='AIC')
    # output for dft will give us without defining what the values are.
    #hence we manually write what values does it explains using a for loop
    output = pd.Series(adft[0:4],index=['Test Statistics','p-value','No. of lags used','Number of observations used'])
    for key,values in adft[4].items():
        output['critical value (%s)'%key] =  values
    print(output)

test_stationarity(df)
```

![](../Images/c3d4b4193d0ec0e53b9286e5e72686b9.png)

通过上述图表，我们可以看到均值和标准差的增加，因此我们的序列不是平稳的。

![图](../Images/6da2e750c73d55a1c29f436a5128f08a.png)

迪基-富勒检验的结果

我们看到 p 值大于 0.05，因此我们不能拒绝**原假设**。此外，测试统计量也大于临界值，因此数据是非平稳的。

为了获得平稳的序列，我们需要从序列中去除趋势和季节性。

我们首先对序列取对数，以减少值的大小并减少序列中的上升趋势。然后在得到序列的对数后，我们计算序列的滚动平均数。滚动平均数是通过输入过去 12 个月的数据，在序列的每个后续点上提供一个均值消耗值来计算的。

```py
df_log = np.log(df)
moving_avg = df_log.rolling(12).mean()
std_dev = df_log.rolling(12).std()
plt.plot(df_log)
plt.plot(moving_avg, color="red")
plt.plot(std_dev, color ="black")
plt.show()
```

![](../Images/cd9d4fa9ba13711cc5cc4f614d4980a8.png)

计算均值后，我们在序列中的每个点上取序列值与均值之间的差异。

通过这种方式，我们去除序列中的趋势，并获得一个更平稳的序列。

```py
df_log_moving_avg_diff = df_log-moving_avg
df_log_moving_avg_diff.dropna(inplace=True)
```

再次执行迪基-富勒检验（ADFT）。每次都需要执行此函数，以检查数据是否平稳。

```py
test_stationarity(df_log_moving_avg_diff)
```

![](../Images/95388987e34b4711450e24fc01e5c434.png)

从上图中，我们观察到数据达到了平稳状态。

当我们得出结论时，一个模块已经完成。我们需要检查加权平均数，以理解时间序列数据的趋势。取之前的对数数据并执行以下操作。

```py
weighted_average = df_log.ewm(halflife=12, min_periods=0,adjust=True).mean()
```

指数加权移动平均数（EMA）是最近 n 个价格的加权平均数，其中权重随着每个之前的价格/周期呈指数递减。换句话说，该公式赋予近期价格比过去价格更多的权重。

![](../Images/962926beaef8ee29c67564bc8f4558c2.png)

之前我们用移动平均数减去了 df_log，现在用相同的 df_log 减去加权平均数，并再次执行迪基-富勒检验（ADFT）。

```py
logScale_weightedMean = df_log-weighted_average
from pylab import rcParams
rcParams['figure.figsize'] = 10,6
test_stationarity(logScale_weightedMean)
```

![](../Images/e1539dc78ec0593da27f1b930de78526.png)

![图](../Images/0746a0afbe809122f8d29d8c1da4b33b.png)

迪基-富勒检验的结果

从上图中，我们观察到数据达到了平稳状态。我们还看到测试统计量和临界值相对接近。

数据中可能会出现高季节性的情况。

在这些情况下，仅去除趋势并没有太大帮助。我们还需要处理序列中的季节性。处理这种情况的一种方法是差分。

差分是一种转换时间序列数据集的方法。

它可以用来去除序列对时间的依赖，即所谓的时间依赖性。这包括趋势和季节性等结构。差分可以通过去除时间序列水平的变化来帮助稳定时间序列的均值，从而消除（或减少）趋势和季节性。

差分是通过从当前观察值中减去前一个观察值来进行的。

再次执行Dickey-Fuller测试（ADFT）。

```py
df_log_diff = df_log - df_log.shift()
plt.title("Shifted timeseries")
plt.xlabel("Date")
plt.ylabel("Consumption")
plt.plot(df_log_diff)#Let us test the stationarity of our resultant series
df_log_diff.dropna(inplace=True)test_stationarity(df_log_diff)
```

![](../Images/454f6ac4acd56560e46bc9552729d75b.png)

下一步是执行分解，这提供了一种结构化的思考时间序列预测问题的方法，既包括建模复杂性的一般性方面，也包括如何在给定模型中最好地捕捉每个组件的具体方面。最后，再次执行Dickey-Fuller测试（ADFT）。

```py
from chart_studio.plotly import plot_mpl
from statsmodels.tsa.seasonal import seasonal_decompose
result = seasonal_decompose(df_log, model='additive', freq = 12)
result.plot()
plt.show()trend = result.trend
trend.dropna(inplace=True)seasonality = result.seasonal
seasonality.dropna(inplace=True)residual = result.resid
residual.dropna(inplace=True)test_stationarity(residual)
```

![](../Images/0eef215bcefd854dcad6dd894590f2cc.png)

在分解之后，如果我们查看残差，那么均值和标准差都是平坦的。我们得到了我们的平稳序列，现在可以继续寻找模型的最佳参数。

**3. 寻找我们模型的最佳参数**

在我们构建预测模型之前，需要确定模型的最佳参数。为这些最佳参数，我们需要ACF和PACF图。

非季节性ARIMA模型被分类为“ARIMA(p,d,q)”模型，其中：

p → 自回归项的数量，

d → 实现平稳性所需的非季节性差分数量，以及

q → 预测方程中滞后的预测误差数量。

p和q的值通过ACF和PACF图得出。所以让我们理解ACF和PACF吧！

### 自相关函数（ACF）

统计相关性总结了两个变量之间关系的强度。皮尔逊相关系数是一个介于-1和1之间的数字，分别描述负相关或正相关。零值表示没有相关性。

我们可以计算时间序列观察值与前一个时间步的相关性，称为滞后。因为时间序列观察值的相关性是与前面时间的同一序列的值计算的，这被称为序列相关性，或自相关性。

时间序列的自相关图按滞后显示，称为**自**相关**函**数，或缩写为ACF。该图有时也称为自相关图或自相关图。

### 偏自相关函数（PACF）

偏自相关是时间序列中某个观察值与前期观察值的关系的总结，剔除了中间观察值的关系。

在滞后k的偏自相关是去除任何因较短滞后项而产生的相关性后的相关性。

观测值和先前时间步的观测值之间的自相关包括直接相关和间接相关。部分自相关函数旨在去除这些间接相关。

以下代码绘制了 ACF 和 PACF 图：

```py
from statsmodels.tsa.stattools import acf,pacf
# we use d value here(data_log_shift)
acf = acf(df_log_diff, nlags=15)
pacf= pacf(df_log_diff, nlags=15,method='ols')#plot PACF
plt.subplot(121)
plt.plot(acf) 
plt.axhline(y=0,linestyle='-',color='blue')
plt.axhline(y=-1.96/np.sqrt(len(df_log_diff)),linestyle='--',color='black')
plt.axhline(y=1.96/np.sqrt(len(df_log_diff)),linestyle='--',color='black')
plt.title('Auto corellation function')
plt.tight_layout()#plot ACF
plt.subplot(122)
plt.plot(pacf) 
plt.axhline(y=0,linestyle='-',color='blue')
plt.axhline(y=-1.96/np.sqrt(len(df_log_diff)),linestyle='--',color='black')
plt.axhline(y=1.96/np.sqrt(len(df_log_diff)),linestyle='--',color='black')
plt.title('Partially auto corellation function')
plt.tight_layout()
```

![](../Images/afd326f6376a540cda25c22f8ed81149.png)

**4\. 拟合模型**

为了从上述图表中找到 p 和 q 值，我们需要检查图表第一次从原点切断或降至零的地方，从图表中 p 和 q 值接近 3，即图表从原点切断的地方（画一条线到 x 轴）。现在我们有了 p、d、q 值。接下来，我们可以代入 ARIMA 模型并查看输出结果。

```py
from statsmodels.tsa.arima_model import ARIMA
model = ARIMA(df_log, order=(3,1,3))
result_AR = model.fit(disp = 0)
plt.plot(df_log_diff)
plt.plot(result_AR.fittedvalues, color='red')
plt.title("sum of squares of residuals")
print('RSS : %f' %sum((result_AR.fittedvalues-df_log_diff["Consumption"])**2))
```

![](../Images/b57540811435e9eea8464ac0c79423c9.png)

RSS 值越小，模型越有效。你可以通过 (2,1,0)、(3,1,1) 等组合来寻找最小的 RSS 值。

**5\. 预测**

以下代码帮助我们预测未来 6 年的洗发水销售量。

```py
result_AR.plot_predict(1,500)
x=result_AR.forecast(steps=200)
```

![](../Images/799347500c89c55c018c68653b1517b9.png)

从上述图表中，我们计算了到 2024 年的未来预测，灰色区域是置信区间，这意味着预测不会越过该区域。

### 结论

最后，我们成功构建了一个 ARIMA 模型，并实际预测了未来的时间段。请注意，这只是一个基本实现，旨在帮助入门时间序列预测。还有很多概念如平滑等，模型如 ARIMAX、prophet 等可以用于构建时间序列模型。

好了，文章到此为止，希望大家阅读愉快，欢迎在评论区分享你的意见/想法/反馈。

![图](../Images/bb5866fe888ae1ac0fd95aaed064e11b.png)

图片来源: [http://mrwgifs.com/grainy-classic-the-end-gif/](http://mrwgifs.com/grainy-classic-the-end-gif/)

你可以在这个 GitHub 链接中找到完整代码：[https://github.com/nageshsinghc4/Time-Series-Analysis](https://github.com/nageshsinghc4/Time-Series-Analysis)

快乐学习 !!!

**简介: [纳盖什·辛格·乔汉](https://www.linkedin.com/in/nagesh-singh-chauhan-6936bb13b/)** 是一位数据科学爱好者。对大数据、Python 和机器学习感兴趣。

原文，已获许可转载。

**相关:**

+   [结合不同方法创建高级时间序列预测](/2016/11/combining-different-methods-create-advanced-time-series-prediction.html)

+   [AutoML 在时序关系数据中的应用：新的前沿](/2019/10/automl-temporal-relational-data.html)

+   [时间序列分析：使用 KNIME 和 Spark 的简单示例](/2019/10/time-series-analysis-simple-example-knime-spark.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的IT

* * *

### 更多相关主题

+   [停止学习数据科学以寻找目标，并寻找目标以...](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [一个90亿美元的AI失败案例分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [数据科学学习统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的5个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么让Python成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每个数据科学家都应了解的三个R语言库（即使你使用Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
