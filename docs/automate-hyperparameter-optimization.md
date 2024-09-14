# 如何自动化超参数优化

> 原文：[https://www.kdnuggets.com/2019/06/automate-hyperparameter-optimization.html](https://www.kdnuggets.com/2019/06/automate-hyperparameter-optimization.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者 [Suleka Helmini](https://www.linkedin.com/in/suleka-helmini-09803b132/detail/recent-activity/)，WSO2**

### 《使用贝叶斯优化和 Scikit-Optimize 的初学者指南》

在机器学习和深度学习范式中，“参数”和“超参数”是两个常用的术语，其中“参数”定义了模型内部的配置变量，这些变量的值可以通过训练数据来估计，而“超参数”定义了模型外部的配置变量，这些变量的值不能从训练数据中估计出来（[什么是参数和超参数的区别？](https://machinelearningmastery.com/difference-between-a-parameter-and-a-hyperparameter/)）。因此，超参数的值需要由实践者手动设置。

我们制作的每个机器学习和深度学习模型都有一组不同的超参数值，这些值需要进行微调才能获得令人满意的结果。与机器学习模型相比，深度学习模型往往有更多的超参数需要优化，以获得期望的预测结果，因为深度学习模型的结构复杂度高于典型的机器学习模型。

手动反复试验不同的值组合，以获得每个超参数的最佳值，可能是一个非常耗时且繁琐的任务，需要良好的直觉、大量的经验和对模型的深刻理解。此外，一些超参数值可能需要连续值，这会有不确定数量的可能性，即使超参数需要离散值，其可能性也非常巨大，因此手动执行这一任务是相当困难的。尽管如此，超参数优化可能看起来令人望而却步，但由于网络上有多个现成的库，这一任务变得更加简单。这些库帮助实现不同的超参数优化算法，减少了工作量。一些这样的库包括 [Scikit-Optimize](https://scikit-optimize.github.io/)、[Scikit-Learn](https://scikit-learn.org/stable/) 和 [Hyperopt](https://github.com/hyperopt/hyperopt)。

多年来经常使用的几种超参数优化算法包括 Grid Search、Random Search 和自动超参数优化方法。Grid Search 和 Random Search 都设置了一个超参数的网格，但在 Grid Search 中，每一种值组合都会被穷尽地探索，以找到提供最佳准确性值的超参数组合，使得这种方法非常低效。另一方面，Random Search 会从网格中重复选择随机组合，直到满足指定的迭代次数，已被证明比 Grid Search 产生更好的结果。然而，即使它能够给出一个好的超参数组合，我们也不能确定这是否是最终的最佳组合。自动超参数优化使用不同的技术，如贝叶斯优化，进行有指导的最佳超参数搜索 ([使用网格和随机搜索的超参数调优](https://www.kaggle.com/willkoehrsen/intro-to-model-tuning-grid-and-random-search))。研究表明，贝叶斯优化能比 Random Search 产生更好的超参数组合 ([贝叶斯优化用于超参数调优](https://arimo.com/data-science/2016/bayesian-optimization-hyperparameter-tuning/))。

在本文中，我们将提供一个逐步指南，通过使用高斯过程的贝叶斯优化，执行深度学习模型的超参数优化任务。我们使用了由 [Scikit-Optimize (skopt)](https://scikit-optimize.github.io/) 库提供的 [gp_minimize](https://scikit-optimize.github.io/#skopt.gp_minimize) 包来执行此任务。我们将在一个使用 [TensorFlow](https://www.tensorflow.org/) 开发的简单股票收盘价格预测模型上进行超参数优化。

### Scikit-Optimize (skopt)

Scikit-Optimize 是一个相较于其他超参数优化库更易于使用的库，同时也有更好的社区支持和文档。该库通过减少昂贵且噪声较大的黑箱函数，实施了几种基于模型的顺序优化方法。

使用高斯过程的贝叶斯优化

贝叶斯优化是 skopt 提供的众多功能之一。贝叶斯优化在参数优化过程中寻找一个后验分布作为要优化的函数，然后使用一个采集函数（例如，期望改进-EI，或其他函数）从该后验分布中进行采样，以找到下一个要探索的参数集。由于贝叶斯优化基于考虑可用数据的更系统方法来决定下一个点，因此与网格搜索和随机搜索等全面的参数优化技术相比，它预计能够更快地实现更好的配置。您可以从 [这里](https://orbi.uliege.be/bitstream/2268/226433/1/PyData%202017_%20Bayesian%20optimization%20with%20Scikit-Optimize.pdf) 阅读更多关于 skopt 的贝叶斯优化器的内容。

### 代码警报！

所以，理论部分足够了，我们开始实际操作吧！

这个示例代码使用了 python 和 TensorFlow。此外，这个超参数优化任务的目标是获得一组可以为我们的深度学习模型提供最低可能均方根误差（RMSE）的超参数值。我们希望这对任何第一次接触的人来说都非常简单。

首先，让我们安装 Scikit-Optimize。您可以通过执行此命令使用 pip 进行安装。

```py

pip install scikit-optimize

```

请注意，您需要对现有的深度学习模型代码做一些调整，以使其能够与优化一起工作。

首先，让我们进行一些必要的导入。

```py

import skopt
from skopt import gp_minimize
from skopt.space import Real, Integer
from skopt.utils import use_named_args
import tensorflow as tf
import numpy as np
import pandas as pd
from math import sqrt
import atexit
from time import time, strftime, localtime
from datetime import timedelta
from sklearn.metrics import mean_squared_error
from skopt.plots import plot_convergence

```

现在我们将设置 TensorFlow 和 Numpy 的种子，以便获得可重复的结果。

```py

randomState = 46
np.random.seed(randomState)
tf.set_random_seed(randomState)

```

下方展示了一些我们声明的重要 python 全局变量。在这些变量中，我们还声明了我们希望优化的超参数（第二组变量）。

```py

input_size=1
features = 2
column_min_max = [[0, 2000],[0,500000000]]
columns = ['Close', 'Volume']

num_steps = None
lstm_size = None
batch_size = None
init_learning_rate = None
learning_rate_decay = None
init_epoch = None
max_epoch = None
dropout_rate = None
```

“input_size” 描述了预测的部分形状。“features” 描述了数据集中的特征数量，而“columns” 列表包含了两个特征的标题名称。“column_min_max” 变量包含了两个特征的上下缩放边界（这是通过检查验证和训练分割得出的）。

在声明了所有这些变量之后，终于到了声明我们希望优化的每个超参数的搜索空间的时间。

```py

lstm_num_steps = Integer(low=2, high=14, name='lstm_num_steps')
size = Integer(low=8, high=200, name='size')
lstm_learning_rate_decay = Real(low=0.7, high=0.99, prior='uniform', name='lstm_learning_rate_decay')
lstm_max_epoch = Integer(low=20, high=200, name='lstm_max_epoch')
lstm_init_epoch = Integer(low=2, high=50, name='lstm_init_epoch')
lstm_batch_size = Integer(low=5, high=100, name='lstm_batch_size')
lstm_dropout_rate = Real(low=0.1, high=0.9, prior='uniform', name='lstm_dropout_rate')
lstm_init_learning_rate = Real(low=1e-4, high=1e-1, prior='log-uniform', name='lstm_init_learning_rate')

```

如果你仔细观察，你会发现我们在 log-uniform 之前声明了 ‘lstm_init_learning_rate’，而不是直接使用 uniform。这是因为，如果你使用 uniform，优化器将从 1e-4（0.0001）到 1e-1（0.1）的均匀分布中进行搜索。但当声明为 log-uniform 时，优化器将在 -4 和 -1 之间搜索，从而使过程更高效。这是 [skopt 库](https://scikit-optimize.github.io/notebooks/hyperparameter-optimization.html) 在为学习率分配搜索空间时所建议的。

你可以使用几种数据类型来定义搜索空间，包括Categorical、Real和Integer。当定义涉及浮点值的搜索空间时，你应该选择“Real”；当涉及整数时，选择“Integer”。如果你的搜索空间涉及如不同激活函数这样的分类值，则应选择“Categorical”类型。

我们现在要将待优化的参数列出在“dimensions”列表中。这个列表稍后将传递给‘gp_minimize’函数。你可以看到我们也声明了‘default_parameters’。这些是我们为每个超参数设置的默认值。****记得按照你在“dimensions”列表中列出超参数的顺序输入默认值。****

```py

dimensions = [lstm_num_steps, size, lstm_init_epoch, lstm_max_epoch,
lstm_learning_rate_decay, lstm_batch_size, lstm_dropout_rate, lstm_init_learning_rate]

default_parameters = [2,128,3,30,0.99,64,0.2,0.001]

```

最重要的一点是，“default_parameters”列表中的超参数将是你优化任务的起始点。贝叶斯优化器将在第一次迭代中使用你声明的默认参数，根据结果，获取函数将决定下一个要探索的点。

可以说，如果你之前多次运行模型并找到了一组不错的超参数值，你可以将它们作为默认超参数值，从那里开始探索。这可能会帮助算法更快地找到最低的RMSE值（减少迭代次数）。然而，请记住这并不总是成立的。此外，分配默认值时请记得赋值在你定义的搜索空间内。

到目前为止，我们完成了超参数优化任务的所有初步工作。接下来我们将专注于深度学习模型的实现。由于本文仅关注超参数优化任务，因此我们不会讨论模型开发过程中的数据预处理。我们将在文章末尾提供完整实现的GitHub链接。

为了提供更多背景，我们将数据集分为训练集、验证集和测试集。训练集用于训练模型，验证集用于超参数优化任务。如前所述，我们使用均方根误差（RMSE）来评估模型并进行优化（最小化RMSE）。

使用验证集评估的准确性不能用来评估模型，因为在超参数优化过程中，最小化RMSE的超参数可能会对验证集过拟合。因此，标准做法是使用未在任何管道中使用的测试集来测量最终模型的准确性。

下面展示了我们深度学习模型的实现：

```py

def setupRNN(inputs, model_dropout_rate):

  cell = tf.contrib.rnn.LSTMCell(lstm_size, state_is_tuple=True, activation=tf.nn.tanh,use_peepholes=True)

  val1, _ = tf.nn.dynamic_rnn(cell, inputs, dtype=tf.float32)

  val = tf.transpose(val1, [1, 0, 2])

  last = tf.gather(val, int(val.get_shape()[0]) -1, name="last_lstm_output")

  dropout = tf.layers.dropout(last, rate=model_dropout_rate, training=True,seed=46)

  weight = tf.Variable(tf.truncated_normal([lstm_size, input_size]))
  bias = tf.Variable(tf.constant(0.1, shape=[input_size]))

  prediction = tf.matmul(dropout, weight) +bias

  return prediction

```

“setupRNN”函数包含我们的深度学习模型。尽管如此，你可能不需要了解这些细节，因为贝叶斯优化将该函数视为一个黑箱，接受某些超参数作为输入，然后输出预测结果。所以，如果你不感兴趣了解我们在那个函数内部做了什么，你可以跳过下一段。

我们的深度学习模型包含一个 LSTM 层、一个 dropout 层和一个输出层。需要传递给这个函数的模型工作所需的信息（在我们的例子中，是输入和 dropout 率）。然后，你可以继续在这个函数内部实现你的深度学习模型。在我们的例子中，我们使用了一个 LSTM 层来识别我们股票数据集的时间依赖关系。

我们将 LSTM 的最后输出传递给 dropout 层进行正则化处理，并通过输出层获得预测结果。最后，记得将这个预测结果（在分类任务中，这可以是你的 logit）返回给将传递给贝叶斯优化的函数（“setupRNN” 将由此函数调用）。

如果你正在为机器学习算法执行超参数优化（使用类似 Scikit-Learn 的库），你将不需要一个单独的函数来实现你的模型，因为模型本身已经由库提供，你只需要编写代码来训练和获取预测。因此，这段代码可以放在将返回给贝叶斯优化的函数内部。

我们现在来到了超参数优化任务中最重要的部分——‘fitness’函数。

```py

@use_named_args(dimensions=dimensions)
def fitness(lstm_num_steps, size, lstm_init_epoch, lstm_max_epoch,
            lstm_learning_rate_decay, lstm_batch_size, lstm_dropout_rate, lstm_init_learning_rate):

    global iteration, num_steps, lstm_size, init_epoch, max_epoch, learning_rate_decay, dropout_rate, init_learning_rate, batch_size

    num_steps = np.int32(lstm_num_steps)
    lstm_size = np.int32(size)
    batch_size = np.int32(lstm_batch_size)
    learning_rate_decay = np.float32(lstm_learning_rate_decay)
    init_epoch = np.int32(lstm_init_epoch)
    max_epoch = np.int32(lstm_max_epoch)
    dropout_rate = np.float32(lstm_dropout_rate)
    init_learning_rate = np.float32(lstm_init_learning_rate)

    tf.reset_default_graph()
    tf.set_random_seed(randomState)
    sess = tf.Session()

    train_X, train_y, val_X, val_y, nonescaled_val_y = pre_process()

    inputs = tf.placeholder(tf.float32, [None, num_steps, features], name="inputs")
    targets = tf.placeholder(tf.float32, [None, input_size], name="targets")
    model_learning_rate = tf.placeholder(tf.float32, None, name="learning_rate")
    model_dropout_rate = tf.placeholder_with_default(0.0, shape=())
    global_step = tf.Variable(0, trainable=False)

    prediction = setupRNN(inputs,model_dropout_rate)

    model_learning_rate = tf.train.exponential_decay(learning_rate=model_learning_rate, global_step=global_step, decay_rate=learning_rate_decay,
                                                decay_steps=init_epoch, staircase=False)

    with tf.name_scope('loss'):
        model_loss = tf.losses.mean_squared_error(targets, prediction)

    with tf.name_scope('adam_optimizer'):
        train_step = tf.train.AdamOptimizer(model_learning_rate).minimize(model_loss,global_step=global_step)

    sess.run(tf.global_variables_initializer())

    for epoch_step in range(max_epoch):

        for batch_X, batch_y in generate_batches(train_X, train_y, batch_size):
            train_data_feed = {
                inputs: batch_X,
                targets: batch_y,
                model_learning_rate: init_learning_rate,
                model_dropout_rate: dropout_rate
            }
            sess.run(train_step, train_data_feed)

    val_data_feed = {
        inputs: val_X,
    }
    vali_pred = sess.run(prediction, val_data_feed)

    vali_pred_vals = rescle(vali_pred)

    vali_pred_vals = np.array(vali_pred_vals)

    vali_pred_vals = vali_pred_vals.flatten()

    vali_pred_vals = vali_pred_vals.tolist()

    vali_nonescaled_y = nonescaled_val_y.flatten()

    vali_nonescaled_y = vali_nonescaled_y.tolist()

    val_error = sqrt(mean_squared_error(vali_nonescaled_y, vali_pred_vals))

    return val_error

```

如上所示，我们将超参数值传递给一个名为“fitness”的函数。“fitness”函数将被传递到贝叶斯超参数优化过程（*gp_minimize*）。注意，在第一次迭代中，传递给该函数的值将是你定义的默认值，从那时起，贝叶斯优化将自动选择超参数值。然后，我们将选择的值分配给我们在开始时声明的 Python 全局变量，以便我们能够在“fitness”函数之外使用最新选择的超参数值。

我们现在进入优化任务中的一个关键点。如果你在阅读本文之前使用过 TensorFlow，你会知道 TensorFlow 是通过创建计算图来操作你所制作的任何深度学习模型的。

在超参数优化过程中，在每次迭代中，我们将重置现有图并构建一个新的图。这一过程是为了最小化图占用的内存并防止图重叠堆积。在重置图后，你需要设置 TensorFlow 随机种子，以获得可重复的结果。在上述过程之后，我们可以最终声明 TensorFlow 会话。

在这一点之后，你可以像平常一样开始添加负责训练和验证深度学习模型的代码。这一部分实际上与优化过程无关，但此后的代码将开始利用贝叶斯优化选择的超参数值。

这里主要需要记住的是****返回****最终度量值（在本例中为 RMSE 值），该值将被返回到贝叶斯优化过程中，并在决定下一组要探索的超参数时使用。

****注意****：如果你处理的是分类问题，你可能需要将准确度设置为负值（例如 -96），因为尽管准确度越高模型越好，贝叶斯函数会不断尝试减少这个值，因为它设计用于找到返回值最****低****的超参数值。

现在让我们列出整个过程的执行点，即 “main” 函数。在 main 函数内部，我们声明了 “gp_minimize” 函数。然后，我们将几个关键参数传递给该函数。

```py

if __name__ == '__main__':

    start = time()

    search_result = gp_minimize(func=fitness,
                                dimensions=dimensions,
                                acq_func='EI', # Expected Improvement.
                                n_calls=11,
                                x0=default_parameters,
                                random_state=46)

print(search_result.x)
print(search_result.fun)
plot = plot_convergence(search_result,yscale="log")

atexit.register(endlog)
logger("Start Program")

```

“func” 参数是你最终想要使用贝叶斯优化器建模的函数。“dimensions” 参数是一组你希望优化的超参数，而 “acq_func” 代表获取函数，是帮助决定下一组应该使用的超参数值的函数。*gp_minimize* 支持 4 种类型的获取函数。它们是：

+   LCB：下置信界限

+   EI：期望改进

+   PI：改进概率

+   gp_hedge：在每次迭代时以概率选择上述三种获取函数之一

上述信息摘自文档。每种获取函数都有其自身的优点，但如果你是贝叶斯优化的初学者，可以尝试使用 “EI” 或 “gp_hedge”，因为 “EI” 是最广泛使用的获取函数，而 “gp_hedge” 会以概率方式选择上述获取函数，因此你不必过于担心这个问题。

请记住，在使用不同的获取函数时，可能还会有其他你需要调整的参数，这些参数会影响你选择的获取函数。有关这些参数的详细信息，请参阅 [文档](https://scikit-optimize.github.io/#skopt.gp_minimize)。

回到其他参数的解释， “n_calls” 参数是你希望运行适应度函数的次数。优化任务将以 “x0” 定义的超参数值，即默认超参数值开始。最后，我们设置了超参数优化器的随机状态，以便获得可重复的结果。

现在，当你运行 *gp_optimize* 函数时，事件流程将是：

适应度函数将使用传递给x0的参数。LSTM将按照指定的epoch进行训练，验证输入将被运行以获得预测的RMSE值。然后根据该值，贝叶斯优化器将决定使用获取函数探索下一组超参数值。

在第2次迭代中，适应度函数将使用贝叶斯优化得出的超参数值进行运行，并且这一过程将重复，直到迭代了“n_call”次。当完整过程结束时，Scikit-Optimize 对象将被分配给“search_result”变量。

我们可以使用这个对象来检索文档中所述的有用信息。

+   x [list]: 最小值的位置。

+   fun [float]: 最小值处的函数值。

+   models: 每次迭代中使用的替代模型。

+   x_iters [list of lists]: 每次迭代的函数评估位置。

+   func_vals [array]: 每次迭代的函数值。

+   space [Space]: 优化空间。

+   specs [dict]`: 调用规格。

+   rng [RandomState 实例]: 最小化结束时的随机状态。

“search_result.x”给出了最佳超参数值，使用“search_result.fun”我们可以获得对应于这些超参数值的验证集的RMSE值（验证集中获得的最低RMSE值）。

下图展示了我们为模型获得的最佳超参数值及验证集的最低RMSE值。如果你发现很难确定在使用“search_result.x”时超参数值的排列顺序，它与在“dimensions”列表中指定超参数的顺序一致。

****超参数值：****

+   lstm_num_steps: 6

+   lstm_size: 171

+   lstm_init_epoch: 3

+   lstm_max_epoch: 58

+   lstm_learning_rate_decay: 0.7518394019565194

+   lstm_batch_size: 24

+   lstm_dropout_rate: 0.21830825193089087

+   lstm_init_learning_rate: 0.0006401363567813549

****最低RMSE:**** 2.73755355221523

### 收敛图

在这张图中产生贝叶斯优化最低点的超参数就是我们得到的最佳超参数值集合。

![figure-name](../Images/bdd75378596d881565195f7e17f34e3b.png)

图表显示了贝叶斯优化和随机搜索中每次迭代（50次迭代）记录的最低RMSE值的比较。我们可以看到贝叶斯优化的收敛效果明显优于随机搜索。然而，在开始阶段，我们可以看到随机搜索比贝叶斯优化器更快地找到了更好的最小值。这可能是因为随机搜索的性质就是随机采样。

我们终于来到了这篇文章的结尾，总结一下，我们希望这篇文章通过展示一种更好的方式来找到最优的超参数集，从而使你的深度学习模型构建任务变得更简单。希望你不再为超参数优化而感到压力。祝编码愉快，亲爱的极客们！

### 实用材料：

+   完整的代码可以通过这个 [链接](https://github.com/suleka96/Bayesian_Hyperparameter_optimization/tree/master) 找到。

+   欲了解有关贝叶斯优化的更多信息，可以参阅这篇 [论文](https://arxiv.org/pdf/1807.02811.pdf)。

+   欲了解有关采集函数的更多信息，请参阅这篇 [论文](https://www.cse.wustl.edu/~garnett/cse515t/spring_2015/files/lecture_notes/12.pdf)。

**简介: [Suleka Helmini](https://www.linkedin.com/in/suleka-helmini-09803b132/)** 是 WS02 的软件工程师。

[原文](https://dzone.com/articles/how-to-automate-hyperparameter-optimization)。经允许转载。

**相关：**

+   [高斯过程的贝叶斯优化直观理解](/2018/10/intuitions-behind-bayesian-optimization-gaussian-processes.html)

+   [在 Google Colab 中使用 Hyperas 进行 Keras 超参数调整](/2018/12/keras-hyperparameter-tuning-google-colab-hyperas.html)

+   [使用 AutoML 生成具有 TPOT 的机器学习管道](/2018/01/managing-machine-learning-workflows-scikit-learn-pipelines-part-4.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

### 更多相关内容

+   [超参数优化：10 大 Python 库](https://www.kdnuggets.com/2023/01/hyperparameter-optimization-10-top-python-libraries.html)

+   [用 Python 自动化的 5 项任务](https://www.kdnuggets.com/2021/06/5-tasks-automate-python.html)

+   [使用 Python 自动化 Microsoft Excel 和 Word](https://www.kdnuggets.com/2021/08/automate-microsoft-excel-word-python.html)

+   [用 GPT-4 和 Python 自动化无聊的工作](https://www.kdnuggets.com/2023/03/automate-boring-stuff-chatgpt-python.html)

+   [用 Promptr 和 GPT 自动化你的代码库](https://www.kdnuggets.com/2023/04/automate-codebase-promptr-gpt.html)

+   [用 ChatGPT Canva 插件自动化图形设计活动](https://www.kdnuggets.com/automate-graphic-design-activity-with-chatgpt-canva-plugin)
