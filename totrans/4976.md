# 使用 Iris 数据集的简单 XGBoost 教程

> 原文：[https://www.kdnuggets.com/2017/03/simple-xgboost-tutorial-iris-dataset.html](https://www.kdnuggets.com/2017/03/simple-xgboost-tutorial-iris-dataset.html)

**作者：Ieva Zarina，软件开发员，Nordigen。**

我有机会开始使用 [xgboost](https://xgboost.readthedocs.io/en/latest/) 机器学习算法，它快速且显示出 [良好的结果](https://github.com/dmlc/xgboost/tree/master/demo#usecases)。在这里，我将使用 [iris 数据集](https://en.wikipedia.org/wiki/Iris_flower_data_set) 进行多分类预测，数据集来自 [scikit-learn](http://scikit-learn.org/stable/modules/generated/sklearn.datasets.load_iris.html)。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT 工作

* * *

![XGBoost](../Images/6ecd41b989b70db02e5ee3380e71a510.png)

XGBoost 算法 ([source](https://www.slideshare.net/JaroslawSzymczak1/xgboost-the-algorithm-that-wins-every-competition))。

### 安装 Anaconda 和 xgboost

为了处理数据，我需要安装各种 Python 科学库。我发现最好的方法是使用 [Anaconda](https://www.continuum.io/downloads)。它可以简单地安装所有库，并帮助 [安装新的库](http://conda.pydata.org/docs/using/pkgs.html#install-a-package)。你可以下载适用于 Windows 的安装程序，但如果你想在 Linux 服务器上安装，只需将以下内容复制粘贴到终端：

```py

wget http://repo.continuum.io/archive/Anaconda2-4.0.0-Linux-x86_64.sh
bash Anaconda2-4.0.0-Linux-x86_64.sh -b -p $HOME/anaconda
echo 'export PATH="$HOME/anaconda/bin:$PATH"' >> ~/.bashrc
bash

```

在此之后，**使用 conda 安装 pip**，你将需要它来安装 xgboost。重要的是使用 Anaconda（在 Anaconda 的目录中）来安装它，以便 pip 也能在那里安装其他库：

```py

conda install -y pip

```

现在，一个非常重要的步骤：**预先安装 [xgboost Python 包](https://github.com/dmlc/xgboost/tree/master/python-package) 的依赖项**。根据经验，我会安装这些依赖项：

```py

sudo apt-get install -y make g++ build-essential gfortran libatlas-base-dev liblapacke-dev python-dev python-setuptools libsm6 libxrender1

```

我升级了我的 Python 虚拟环境，以避免 [trouble](https://github.com/dmlc/xgboost/issues/463) 与 Python 版本相关的问题：

```py
pip install --upgrade virtualenv

```

最后，我可以用 pip 安装 xgboost（祈祷好运）：

```py
pip install xgboost

```

此命令安装最新版本的 xgboost，但如果你想使用之前的版本，只需指定：

```py

pip install xgboost==0.4a30

```

现在测试一下是否一切正常 – 在终端中输入 **python** 并尝试导入 xgboost：

```py
import xgboost as xgb

```

如果没有看到错误 – 完美。

### Xgboost 与 Iris 数据集的演示

在这里，我将使用 Iris 数据集来展示如何使用 Xgboost 的简单示例。

首先，你**加载数据集**来自 sklearn，其中 X 是数据，y 是类别标签：

```py
from sklearn import datasets

iris = datasets.load_iris()
X = iris.data
y = iris.target

```

然后你将数据**拆分**成80-20%的训练集和测试集，[拆分](http://scikit-learn.org/stable/modules/generated/sklearn.cross_validation.train_test_split.html)：

```py
from sklearn.cross_validation import train_test_split

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

```

接下来，你需要从numpy数组创建Xgboost特定的**[DMatrix](http://xgboost.readthedocs.io/en/latest/python/python_intro.html)**数据格式。Xgboost可以直接处理numpy数组，加载svmlignt文件及其他格式。以下是如何处理**numpy数组**：

```py
import xgboost as xgb

dtrain = xgb.DMatrix(X_train, label=y_train)
dtest = xgb.DMatrix(X_test, label=y_test)

```

如果你想使用svmlight以减少内存消耗，首先**[导出](http://scikit-learn.org/stable/modules/generated/sklearn.datasets.dump_svmlight_file.html)**numpy数组到**svmlight格式**，然后只需将文件名传递给DMatrix：

```py
import xgboost as xgb
from sklearn.datasets import dump_svmlight_file

dump_svmlight_file(X_train, y_train, 'dtrain.svm', zero_based=True)
dump_svmlight_file(X_test, y_test, 'dtest.svm', zero_based=True)
dtrain_svm = xgb.DMatrix('dtrain.svm')
dtest_svm = xgb.DMatrix('dtest.svm')

```

现在为了让Xgboost工作，你需要设置**[参数](https://github.com/dmlc/xgboost/blob/master/doc/parameter.md)**：

```py
param = {
    'max_depth': 3,  # the maximum depth of each tree
    'eta': 0.3,  # the training step for each iteration
    'silent': 1,  # logging mode - quiet
    'objective': 'multi:softprob',  # error evaluation for multiclass training
    'num_class': 3}  # the number of classes that exist in this datset
num_round = 20  # the number of training iterations

```

不同的数据集在不同的参数下表现不同。一个参数组合的结果可能很低，而另一个可能非常好。你可以查看[这个Kaggle脚本](https://www.kaggle.com/tanitter/introducing-kaggle-scripts/grid-search-xgboost-with-scikit-learn)了解如何寻找最佳参数。通常尝试eta 0.1、0.2、0.3，max_depth在2到10的范围内，num_round在几百左右。

### 训练

最终训练可以开始。你只需输入：

```py
bst = xgb.train(param, dtrain, num_round)

```

要查看模型的样子，你也可以将其[导出](http://xgboost.readthedocs.io/en/latest/python/python_intro.html#training)为人类可读的形式：

```py
bst.dump_model('dump.raw.txt')

```

它看起来像这样（f0、f1、f2是特征）：

```py
booster[0]:
0:[f2<2.45] yes=1,no=2,missing=1
    1:leaf=0.426036
    2:leaf=-0.218845
booster[1]:
0:[f2<2.45] yes=1,no=2,missing=1
    1:leaf=-0.213018
    2:[f3<1.75] yes=3,no=4,missing=3
        3:[f2<4.95] yes=5,no=6,missing=5
            5:leaf=0.409091
            6:leaf=-9.75349e-009
        4:[f2<4.85] yes=7,no=8,missing=7
            7:leaf=-7.66345e-009
            8:leaf=-0.210219
....

```

你可以看到每棵树的深度不超过设置的3层。

使用模型来[**预测类别**](http://xgboost.readthedocs.io/en/latest/python/python_intro.html#prediction)测试集的类别：

```py
preds = bst.predict(dtest)

```

但预测结果看起来像这样：

```py
[[ 0.00563804 0.97755206 0.01680986]
 [ 0.98254657 0.01395847 0.00349498]
 [ 0.0036375 0.00615226 0.99021029]
 [ 0.00564738 0.97917044 0.0151822 ]
 [ 0.00540075 0.93640935 0.0581899 ]
....

```

在这里，每一列代表类别0、1或2。对于每一行，你需要选择**概率最高**的那一列：

```py
import numpy as np
best_preds = np.asarray([np.argmax(line) for line in preds])

```

现在你会得到一个包含**预测类别**的漂亮列表：

```py
[1, 0, 2, 1, 1, ...]

```

确定此预测的[**准确率**](http://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)：

```py
from sklearn.metrics import precision_score

print precision_score(y_test, best_preds, average='macro')
# >> 1.0

```

完美！现在**[保存](http://scikit-learn.org/stable/modules/model_persistence.html)**模型以备后用：

```py
from sklearn.externals import joblib

joblib.dump(bst, 'bst_model.pkl', compress=True)
# bst = joblib.load('bst_model.pkl') # load it later

```

现在你有一个保存下来的工作模型，并准备进行更多预测。

查看完整代码在[github](https://gist.github.com/IevaZarina/ef63197e089169a9ea9f3109058a9679)或下方：

**简介: [Ieva Zarina](https://www.linkedin.com/in/ieva-zarina-60515a72/)** 是Nordigen的软件开发人员。

[原始链接](http://ieva.rocks/2016/08/25/iris_dataset_and_xgboost_simple_tutorial/)。经许可转载。

**相关：**

+   [掌握Python机器学习的7个额外步骤](/2017/03/seven-more-steps-machine-learning-python.html)

+   [我在Python中从头实现分类器的学习经历](/2017/02/learned-implementing-classifier-scratch-python.html)

+   [XGBoost：在 Spark 和 Flink 中实现获胜的 Kaggle 算法](/2016/03/xgboost-implementing-winningest-kaggle-algorithm-spark-flink.html)

### 更多相关主题

+   [如何加速 XGBoost 模型训练](https://www.kdnuggets.com/2021/12/speed-xgboost-model-training.html)

+   [XGBoost 的假设是什么？](https://www.kdnuggets.com/2022/08/assumptions-xgboost.html)

+   [调整 XGBoost 超参数](https://www.kdnuggets.com/2022/08/tuning-xgboost-hyperparameters.html)

+   [利用 XGBoost 进行时间序列预测](https://www.kdnuggets.com/2023/08/leveraging-xgboost-timeseries-forecasting.html)

+   [GBM 和 XGBoost 之间有什么区别？](https://www.kdnuggets.com/wtf-is-the-difference-between-gbm-and-xgboost)

+   [Pydantic 教程：Python 数据验证简化版](https://www.kdnuggets.com/pydantic-tutorial-data-validation-in-python-made-simple)
