# 如何在3个简单步骤中对任何 Python 脚本进行超参数调优

> 原文：[https://www.kdnuggets.com/2020/04/hyperparameter-tuning-python.html](https://www.kdnuggets.com/2020/04/hyperparameter-tuning-python.html)

[评论](#comments)

**作者：[Jakub Czakon](https://www.linkedin.com/in/jakub-czakon-2b797b69/?originalSubdomain=pl)，Neptune.ai**。

![Python](../Images/48653af31024defdcd657420253f346a.png)

你写了一个训练和评估机器学习模型的 Python 脚本。现在，你希望自动调整超参数以提高其性能吗？

我明白了！

在这篇文章中，我将向你展示如何将你的脚本转换为一个目标函数，该函数可以使用任何超参数优化库进行优化。

![](../Images/e0074c461140259d5cecec92bd0d7474.png)

只需3个步骤，你就可以像没有明天一样调整模型参数。

准备好了吗？

开始吧！

我猜你的 ***main.py*** 脚本大致如下：

```py
import pandas as pd
import lightgbm as lgb
from sklearn.model_selection import train_test_split

data = pd.read_csv('data/train.csv', nrows=10000)
X = data.drop(['ID_code', 'target'], axis=1)
y = data['target']
(X_train, X_valid, 
y_train, y_valid )= train_test_split(X, y, test_size=0.2, random_state=1234)

train_data = lgb.Dataset(X_train, label=y_train)
valid_data = lgb.Dataset(X_valid, label=y_valid, reference=train_data)

params = {'objective': 'binary',
          'metric': 'auc',
          'learning_rate': 0.4,
          'max_depth': 15,
          'num_leaves': 20,
          'feature_fraction': 0.8,
          'subsample': 0.2}

model = lgb.train(params, train_data,
                  num_boost_round=300,
                  early_stopping_rounds=30,
                  valid_sets=[valid_data],
                  valid_names=['valid'])

score = model.best_score['valid']['auc']
print('validation AUC:', score)

```

### 第一步：将搜索参数与代码解耦

将你想要调整的参数放在脚本顶部的一个字典中。通过这样做，你有效地将搜索参数与代码的其余部分解耦。

```py
import pandas as pd
import lightgbm as lgb
from sklearn.model_selection import train_test_split

SEARCH_PARAMS = {'learning_rate': 0.4,
                 'max_depth': 15,
                 'num_leaves': 20,
                 'feature_fraction': 0.8,
                 'subsample': 0.2}

data = pd.read_csv('../data/train.csv', nrows=10000)
X = data.drop(['ID_code', 'target'], axis=1)
y = data['target']
X_train, X_valid, y_train, y_valid = train_test_split(X, y, test_size=0.2, random_state=1234)

train_data = lgb.Dataset(X_train, label=y_train)
valid_data = lgb.Dataset(X_valid, label=y_valid, reference=train_data)

params = {'objective': 'binary',
          'metric': 'auc',
          **SEARCH_PARAMS}

model = lgb.train(params, train_data,
                  num_boost_round=300,
                  early_stopping_rounds=30,
                  valid_sets=[valid_data],
                  valid_names=['valid'])

score = model.best_score['valid']['auc']
print('validation AUC:', score) 

```

### 第二步：将训练和评估包装成一个函数

现在，你可以将整个训练和评估逻辑放入一个 ***train_evaluate*** 函数中。这个函数接受参数作为输入，并输出验证分数。

```py
import pandas as pd
import lightgbm as lgb
from sklearn.model_selection import train_test_split

SEARCH_PARAMS = {'learning_rate': 0.4,
                 'max_depth': 15,
                 'num_leaves': 20,
                 'feature_fraction': 0.8,
                 'subsample': 0.2}

def train_evaluate(search_params):
    data = pd.read_csv('../data/train.csv', nrows=10000)
    X = data.drop(['ID_code', 'target'], axis=1)
    y = data['target']
    X_train, X_valid, y_train, y_valid = train_test_split(X, y, test_size=0.2, random_state=1234)

    train_data = lgb.Dataset(X_train, label=y_train)
    valid_data = lgb.Dataset(X_valid, label=y_valid, reference=train_data)

    params = {'objective': 'binary',
              'metric': 'auc',
              **search_params}

    model = lgb.train(params, train_data,
                      num_boost_round=300,
                      early_stopping_rounds=30,
                      valid_sets=[valid_data],
                      valid_names=['valid'])

    score = model.best_score['valid']['auc']
    return score

if __name__ == '__main__':
    score = train_evaluate(SEARCH_PARAMS)
    print('validation AUC:', score)

```

### 第三步：运行超参数调优脚本

我们快到了。

现在你需要做的就是将这个 ***train_evaluate*** 函数用作你选择的黑箱优化库的目标。

我将使用 [Scikit Optimize](https://neptune.ai/blog/scikit-optimize)，我在另一篇文章中详细描述了它，但你可以使用任何超参数优化库。

简而言之，我：

+   定义搜索 ***SPACE***，

+   创建需要最小化的 ***objective*** 函数，

+   通过 ***forest_minimize*** 函数运行优化。

在这个例子中，我尝试了 **100** 种不同的配置，从 **10** 组随机选择的参数集开始。

```py
import skopt

from script_step2 import train_evaluate

SPACE = [
    skopt.space.Real(0.01, 0.5, name='learning_rate', prior='log-uniform'),
    skopt.space.Integer(1, 30, name='max_depth'),
    skopt.space.Integer(2, 100, name='num_leaves'),
    skopt.space.Real(0.1, 1.0, name='feature_fraction', prior='uniform'),
    skopt.space.Real(0.1, 1.0, name='subsample', prior='uniform')]

@skopt.utils.use_named_args(SPACE)
def objective(**params):
    return -1.0 * train_evaluate(params)

results = skopt.forest_minimize(objective, SPACE, n_calls=30, n_random_starts=10)
best_auc = -1.0 * results.fun
best_params = results.x

print('best result: ', best_auc)
print('best parameters: ', best_params)

```

就这些了。

 ***results*** 对象包含关于 **最佳分数** **和参数** 的信息。

**注意：**

如果你想在训练完成后**可视化你的训练并保存诊断图表**，那么你可以添加一个回调和一个函数调用，**记录每次超参数搜索**到 Neptune。只需使用这个 [来自 neptune-contrib 的帮助函数](https://neptune-contrib.readthedocs.io/_modules/neptunecontrib/monitoring/skopt.html#NeptuneMonitor) 库。

```py
import neptune
import neptunecontrib.monitoring.skopt as sk_utils
import skopt

from script_step2 import train_evaluate

neptune.init('jakub-czakon/blog-hpo')
neptune.create_experiment('hpo-on-any-script', upload_source_files=['*.py'])

SPACE = [
    skopt.space.Real(0.01, 0.5, name='learning_rate', prior='log-uniform'),
    skopt.space.Integer(1, 30, name='max_depth'),
    skopt.space.Integer(2, 100, name='num_leaves'),
    skopt.space.Real(0.1, 1.0, name='feature_fraction', prior='uniform'),
    skopt.space.Real(0.1, 1.0, name='subsample', prior='uniform')]

@skopt.utils.use_named_args(SPACE)
def objective(**params):
    return -1.0 * train_evaluate(params)

monitor = sk_utils.NeptuneMonitor()
results = skopt.forest_minimize(objective, SPACE, n_calls=100, n_random_starts=10, callback=[monitor])
sk_utils.log_results(results)

neptune.stop()

```

现在，当你运行参数扫描时，你会看到以下内容：

![](../Images/d3998fa6921a2e0309b148132c5981aa.png)

查看 [skopt 超参数扫描实验](https://ui.neptune.ai/jakub-czakon/blog-hpo/e/BLOG-369/charts) ，包含所有代码、图表和结果。

### 最后思考

在这篇文章中，你已经学会了如何在仅仅3个步骤中优化几乎任何 Python 脚本的超参数。

希望通过这些知识，您能以更少的努力构建更好的机器学习模型。

祝学习愉快！

[原文](https://neptune.ai/blog/hyperparameter-tuning-on-any-python-script)。转载已获许可。

**相关：**

+   [实用的超参数优化](https://www.kdnuggets.com/2020/02/practical-hyperparameter-optimization.html)

+   [如何自动化超参数优化](https://www.kdnuggets.com/2019/06/automate-hyperparameter-optimization.html)

+   [在 Google Colab 中使用 Hyperas 进行 Keras 超参数调整](https://www.kdnuggets.com/2018/12/keras-hyperparameter-tuning-google-colab-hyperas.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织进行 IT 相关工作

* * *

### 更多相关主题

+   [每个数据科学家都应该了解的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [是什么让 Python 成为初创企业理想的编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [停止学习数据科学以寻找目标，寻找目标来……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [90 亿美元 AI 失败的调查](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [学习数据科学统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的五大特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)
