# 使用遗传算法优化递归神经网络

> 原文：[`www.kdnuggets.com/2018/01/genetic-algorithm-optimizing-recurrent-neural-network.html`](https://www.kdnuggets.com/2018/01/genetic-algorithm-optimizing-recurrent-neural-network.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**由 Aaqib Saeed，特温特大学**

最近，自动化机器学习的工作增多了，从选择合适的算法到特征选择和超参数调整。有多个工具（如*AutoML*和*TPOT*），可以帮助用户高效地进行数百次实验。同样，深度神经网络架构通常由专家通过试错方法设计。虽然这种方法在多个领域产生了最先进的模型，但却非常耗时。最近，由于计算能力的增加，研究人员正在使用[强化学习](https://openreview.net/pdf?id=r1Ue8Hcxg)和[进化算法](http://nn.cs.utexas.edu/downloads/papers/stanley.ec02.pdf)来自动搜索最佳神经网络架构。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 工作

* * *

在本教程中，我们将探讨如何应用遗传算法（GA）来寻找基于长短期记忆（LSTM）的递归神经网络（RNN）的最佳窗口大小和单元数量。为此，我们将使用[Keras](https://github.com/fchollet/keras)训练和评估时间序列预测问题的模型。GA 将使用一个名为[DEAP](https://github.com/DEAP/deap)的 Python 包。教程的主要目的是使读者熟悉使用 GA 自动寻找最佳设置；因此，仅探索两个参数。此外，假设读者对 RNN 的理论和应用知识有所了解。如果不熟悉，请参阅以下资源[[1]](http://colah.github.io/posts/2015-08-Understanding-LSTMs/)和[[2]](https://r2rt.com/recurrent-neural-networks-in-tensorflow-i.html)。

带有完整代码的 ipython 笔记本可在以下[链接](https://github.com/aqibsaeed/Genetic-Algorithm-RNN)获取。

### 遗传算法

遗传算法是一种启发式搜索和优化方法，灵感来自自然选择过程。它们广泛用于寻找具有大参数空间的优化问题的近似最优解。通过依赖生物启发的组件（例如交叉）来模拟物种（在我们的例子中是解决方案）的进化过程。此外，由于它不考虑辅助信息（例如导数），因此可以用于离散和连续优化。

使用 GA 需要满足两个前提条件：a）解决方案表示或定义一个染色体；b）一个用于评估生成解决方案的适应度函数。在我们的例子中，二进制数组是解决方案的遗传表示（见 **图 1**），模型在验证集上的均方根误差（RMSE）将作为适应度值。此外，构成 GA 的三种基本操作如下：

1.  **选择**：它定义了哪些解决方案将被保留以供进一步繁殖，例如轮盘赌选择。

1.  **交叉**：它描述了如何从现有的解决方案中生成新的解决方案，例如 n 点交叉。

1.  **突变**：其目的是通过随机交换或关闭解决方案位引入多样性和新颖性，例如二进制突变。

![解决方案的遗传表示](img/bba3790751846c9f5fc72ffb4653617c.png)

有时还使用一种称为“精英主义”的技术，它保留一些最佳解决方案，并传递给下一代。**图 2** 描述了一个完整的遗传算法，其中，初始解决方案（种群）是随机生成的。接下来，它们根据适应度函数进行评估，然后执行选择、交叉和突变。这个过程会重复定义的迭代次数（在 GA 术语中称为代）。最后，选择适应度得分最高的解决方案作为最佳解决方案。要了解更多信息，请查看以下资源 [[3]](http://www.flll.jku.at/div/teaching/Ga/GA-Notes.pdf) 和 [[4]](http://www.cs.cmu.edu/~tom/mlbook.html)。

![遗传算法](img/39049a30d230fa390264432946b6aa1a.png)

### 实现

现在，我们对 GA 的工作原理有了基本了解。接下来，让我们开始编码。

我们将使用风电预测数据，数据可以在以下 [链接](https://www.kaggle.com/c/GEF2012-wind-forecasting/data)中找到。数据包括来自七个风电场的风电测量值（归一化在零和一之间）。为了简化起见，我们将使用第一个风电场的数据（名为 **wp1** 的列），但我鼓励读者尝试并扩展代码，以预测所有七个风电场的能源。

让我们导入所需的包，加载数据集并定义两个辅助函数。第一个方法 `prepare_dataset` 将数据分段以创建用于模型训练的 *X*、*Y* 对。*X* 将是过去的风力值（例如 *1* 到 *t-1*），*Y* 将是时间 *t* 的未来值。第二个方法 `train_evaluate` 完成三件事，1）解码 GA 解以获取窗口大小和单位数量。2）使用 GA 找到的窗口大小准备数据集，并将其分为训练集和验证集，3）训练 LSTM 模型，计算验证集上的 RMSE，并将其作为当前 GA 解的适应度得分返回。

```py
import numpy as np
import pandas as pd
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split as split

from keras.layers import LSTM, Input, Dense
from keras.models import Model

from deap import base, creator, tools, algorithms
from scipy.stats import bernoulli
from bitstring import BitArray

np.random.seed(1120)

```

```py
data = pd.read_csv('train.csv')
data = np.reshape(np.array(data['wp1']),(len(data['wp1']),1))

# Use first 17,257 points as training/validation and rest of the 1500 points as test set.
train_data = data[0:17257]
test_data = data[17257:]

```

```py
def prepare_dataset(data, window_size):
    X, Y = np.empty((0,window_size)), np.empty((0))
    for i in range(len(data)-window_size-1):
        X = np.vstack([X,data[i:(i + window_size),0]])
        Y = np.append(Y,data[i + window_size,0])   
    X = np.reshape(X,(len(X),window_size,1))
    Y = np.reshape(Y,(len(Y),1))
    return X, Y

def train_evaluate(ga_individual_solution):   
    # Decode GA solution to integer for window_size and num_units
    window_size_bits = BitArray(ga_individual_solution[0:6])
    num_units_bits = BitArray(ga_individual_solution[6:]) 
    window_size = window_size_bits.uint
    num_units = num_units_bits.uint
    print('\nWindow Size: ', window_size, ', Num of Units: ', num_units)

    # Return fitness score of 100 if window_size or num_unit is zero
    if window_size == 0 or num_units == 0:
        return 100, 

    # Segment the train_data based on new window_size; split into train and validation (80/20)
    X,Y = prepare_dataset(train_data,window_size)
    X_train, X_val, y_train, y_val = split(X, Y, test_size = 0.20, random_state = 1120)

    # Train LSTM model and predict on validation set
    inputs = Input(shape=(window_size,1))
    x = LSTM(num_units, input_shape=(window_size,1))(inputs)
    predictions = Dense(1, activation='linear')(x)
    model = Model(inputs=inputs, outputs=predictions)
    model.compile(optimizer='adam',loss='mean_squared_error')
    model.fit(X_train, y_train, epochs=5, batch_size=10,shuffle=True)
    y_pred = model.predict(X_val)

    # Calculate the RMSE score as fitness score for GA
    rmse = np.sqrt(mean_squared_error(y_val, y_pred))
    print('Validation RMSE: ', rmse,'\n')

    return rmse,

```

接下来，使用 DEAP 包来定义运行 GA 所需的内容。我们将使用长度为十的二进制表示。它将通过伯努利分布随机初始化。同样，使用有序交叉、洗牌突变和轮盘选择。GA 参数值被任意初始化；我建议你尝试不同的设置。

```py
population_size = 4
num_generations = 4
gene_length = 10

# As we are trying to minimize the RMSE score, that's why using -1.0\. 
# In case, when you want to maximize accuracy for instance, use 1.0
creator.create('FitnessMax', base.Fitness, weights = (-1.0,))
creator.create('Individual', list , fitness = creator.FitnessMax)

toolbox = base.Toolbox()
toolbox.register('binary', bernoulli.rvs, 0.5)
toolbox.register('individual', tools.initRepeat, creator.Individual, toolbox.binary, 
n = gene_length)
toolbox.register('population', tools.initRepeat, list , toolbox.individual)

toolbox.register('mate', tools.cxOrdered)
toolbox.register('mutate', tools.mutShuffleIndexes, indpb = 0.6)
toolbox.register('select', tools.selRoulette)
toolbox.register('evaluate', train_evaluate)

population = toolbox.population(n = population_size)
r = algorithms.eaSimple(population, toolbox, cxpb = 0.4, mutpb = 0.1, 
ngen = num_generations, verbose = False)

```

通过 GA 找到的 K 个最佳解可以通过 `tools.selBest(population,k = 1)` 轻松查看。之后，可以使用最佳配置在完整的训练集上进行训练，并在保留的测试集上进行测试。

```py
# Print top N solutions - (1st only, for now)
best_individuals = tools.selBest(population,k = 1)
best_window_size = None
best_num_units = None

for bi in best_individuals:
    window_size_bits = BitArray(bi[0:6])
    num_units_bits = BitArray(bi[6:]) 
    best_window_size = window_size_bits.uint
    best_num_units = num_units_bits.uint
    print('\nWindow Size: ', best_window_size, ', Num of Units: ', best_num_units)

```

```py
# Train the model using best configuration on complete training set 
#and make predictions on the test set
X_train,y_train = prepare_dataset(train_data,best_window_size)
X_test, y_test = prepare_dataset(test_data,best_window_size)

inputs = Input(shape=(best_window_size,1))
x = LSTM(best_num_units, input_shape=(best_window_size,1))(inputs)
predictions = Dense(1, activation='linear')(x)
model = Model(inputs = inputs, outputs = predictions)
model.compile(optimizer='adam',loss='mean_squared_error')
model.fit(X_train, y_train, epochs=5, batch_size=10,shuffle=True)
y_pred = model.predict(X_test)

rmse = np.sqrt(mean_squared_error(y_test, y_pred))
print('Test RMSE: ', rmse)

```

在本教程中，我们探讨了如何使用遗传算法（GA）自动找到 RNN 中的最佳窗口大小（或回溯期）和单位数量。为了进一步学习，我建议你尝试不同的 GA 参数配置，将遗传表示扩展到包括更多参数进行探索，并在下方评论区分享你的发现和问题。

**参考文献：**

1.  理解 LSTM 网络，作者 Christopher Olah

1.  Tensorflow 中的递归神经网络 I，作者 R2RT

1.  遗传算法：理论与应用，作者 Ulrich Bodenhofer

1.  《机器学习》一书第九章，遗传算法，作者 Tom M. Mitchell

![作者](img/f9c21800dc7ef2fdf98de5b260d7c466.png)**简介： [Aaqib Saeed](http://aqibsaeed.github.io/)** 是荷兰特温特大学（University of Twente）计算机科学（专注于数据科学和智能服务）的研究生。

[原文](http://aqibsaeed.github.io/2017-08-11-genetic-algorithm-for-optimizing-rnn/)。转载已获许可。

**相关：**

+   使用 Tensorflow 中的神经网络进行城市声音分类

+   在 Tensorflow 中实现 CNN 进行人类活动识别

+   今天我在午休时间用 Keras 构建了一个神经网络

### 更多相关话题

+   [用遗传算法优化基因](https://www.kdnuggets.com/2022/04/optimizing-genes-genetic-algorithm.html)

+   [遗传算法关键术语解释](https://www.kdnuggets.com/2018/04/genetic-algorithm-key-terms-explained.html)

+   [使用大型语言模型时优化性能和成本的策略](https://www.kdnuggets.com/strategies-for-optimizing-performance-and-costs-when-using-large-language-models-in-the-cloud)

+   [在使用神经网络之前尝试的 10 个简单方法](https://www.kdnuggets.com/2021/12/10-simple-things-try-neural-networks.html)

+   [可解释的 PyTorch 神经网络](https://www.kdnuggets.com/2022/01/interpretable-neural-networks-pytorch.html)

+   [深度神经网络不会引领我们走向 AGI](https://www.kdnuggets.com/2021/12/deep-neural-networks-not-toward-agi.html)
