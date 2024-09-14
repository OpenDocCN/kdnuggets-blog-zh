# 使用FLAML + Ray Tune进行快速AutoML

> 原文：[https://www.kdnuggets.com/2021/09/fast-automl-flaml-ray-tune.html](https://www.kdnuggets.com/2021/09/fast-automl-flaml-ray-tune.html)

[评论](#comments)

**由[吴庆云](https://twitter.com/qingyun_wu)、[王驰](https://www.linkedin.com/in/chi-wang-49b15b16/)、[安东尼·鲍姆](https://www.linkedin.com/in/yard1/)、[理查德·廖](https://twitter.com/richliaw)和[迈克尔·加拉尼克](https://twitter.com/GalarnykMichael)**

![Image](../Images/de7c7c129074f49504448e84d6e97174.png)

[FLAML](https://github.com/microsoft/FLAML)是微软研究院推出的轻量级Python库，利用[前沿](https://arxiv.org/abs/2005.01571)算法以高效和经济的方式寻找准确的机器学习模型，这些算法旨在节省资源并易于并行化。FLAML还可以利用[Ray Tune](https://docs.ray.io/en/master/tune/index.html)进行分布式超参数调优，将这些AutoML方法扩展到集群中。

本博客重点介绍：

+   对经济高效AutoML方法的需求

+   经济高效的AutoML与FLAML

+   如何用Ray Tune扩展FLAML的优化算法

## 对经济高效AutoML方法的需求

众所周知，AutoML是一项资源和时间消耗大的操作，因为它涉及通过试错找到性能良好的超参数配置。由于可能的配置值空间通常非常大，因此需要一种经济高效的AutoML方法来更有效地进行搜索。

AutoML中超参数搜索的高资源和时间消耗归结为以下两个因素：

1.  需要大量候选超参数配置（试验）来找到性能良好的配置

1.  每个超参数的高‘评估’成本，因为评估涉及使用给定的训练数据训练和验证机器学习模型。

为了解决这两个因素，微软研究人员开发了[FLAML](https://github.com/microsoft/FLAML)（快速轻量级AutoML）。

### **什么是FLAML？**

FLAML是一个新发布的库，包含最先进的超参数优化算法。FLAML利用搜索空间的结构，同时优化成本和模型性能。它包含微软研究院开发的两种新方法：

+   成本节俭优化（CFO）

+   BlendSearch

成本节俭优化（CFO）是一种以成本感知方式进行搜索过程的方法。搜索方法从低成本初始点开始，逐渐向高成本区域移动，同时优化给定的目标（如模型损失或准确率）。

Blendsearch是CFO的扩展，结合了CFO的节俭性和贝叶斯优化的探索能力。像CFO一样，BlendSearch需要一个低成本的初始点作为输入（如果存在），并从那里开始搜索。然而，与CFO不同的是，BlendSearch不会等待局部搜索完全收敛后再尝试新的起始点。

FLAML中的经济HPO方法灵感来源于两个关键见解：

1.  许多机器学习算法有超参数会导致训练成本的大幅变化。例如，10棵树的XGBoost模型比1000棵树的模型训练速度要快得多。

1.  参数的“成本”通常是‘连续且一致的’——评估树=10的成本低于评估树=100，而评估树=100的成本又低于评估树=500。

这些见解一起提供了关于成本空间中超参数的有用*结构信息*。这些方法，即CFO和BlendSearch，能够有效利用这些见解来减少沿途产生的成本，而不会影响到达最优解的收敛。

### **FLAML有效吗？**

在最新的[AutoML基准](https://openml.github.io/automlbenchmark/)中，[FLAML](https://arxiv.org/pdf/1911.04706.pdf)能够在62%以上的任务中，仅使用10%的计算资源，实现与最先进AutoML解决方案相同或更好的性能。

FLAML的性能归功于其经济优化方法。新的HPO方法（CFO、BlendSearch）利用搜索空间的结构来选择优化的搜索顺序，以兼顾良好的性能和低成本。这在预算限制下可以显著提高搜索效率。

图1显示了从FLAML和最先进的超参数调整库[Optuna](https://github.com/optuna/optuna)中获得的典型结果，用于调整9维超参数的LightGBM。你可以看到FLAML在更短的时间内能够获得更好的解决方案。

![](../Images/6b87b2f4515439e522f2a79d7adf4781.png)

图1\. 在[classification dataset](https://www.openml.org/d/23517)上调整LightGBM的验证损失（1-auc）曲线。线条和阴影区域显示了10次运行中验证损失的均值和标准差。该图中的结果是从使用1个CPU而不进行并行计算的实验中获得的（图像由作者提供）。

以下代码示例展示了如何用仅几行代码开始使用FLAML（假设训练数据集已经提供并保存为`X_train`、`y_train`）。任务是在60秒的时间预算内调整LightGBM模型的超参数。

```py
**from** flaml **import** AutoML
automl = AutoML()
automl.fit(X_train=X_train, y_train=y_train, time_budget=60, estimator_list=['lgbm'])

''' retrieve best model and best configuration found'''
print('Best ML model:', automl.model)
print('Best hyperparameter config:', automl.best_config)
```

在这个示例中，我们在LightGBM的默认搜索空间上进行搜索，该搜索空间已经在FLAML中提供。FLAML提供了丰富的定制选项，涉及到关注的任务，例如学习器类、搜索空间、评估指标等。

## 一个演示示例

现在我们使用一个示例来展示CFO在调整XGBoost的两个超参数（树的数量和叶子的数量）时的成本节约行为。

```py
'''create an XGBoost learner class with a customized search space'''
**from** flaml.model **import** XGBoostSklearnEstimator
**from** flaml **import** tune

**class** **MyXGB**(XGBoostSklearnEstimator):
​​    '''XGBoostSklearnEstimator with a customized search space'''
    @classmethod
    **def** **search_space**(cls, data_size, **params):
        upper = min(2**15, int(data_size))
        **return** {
            'n_estimators': {
                'domain': tune.lograndint(lower=4, upper=upper),
                'low_cost_init_value': 4,
            },
            'max_leaves': {
                'domain': tune.lograndint(lower=4, upper=upper),
                'low_cost_init_value': 4,
            },
        }

'''Use CFO in FLAML to tune XGBoost'''
**from** flaml **import** AutoML
automl = AutoML()
automl.add_learner(learner_name='my_xgboost', learner_class=MyXGB)
automl.fit(X_train=X_train, y_train=y_train, time_budget=15, estimator_list=['my_xgboost'], hpo_method='cfo')
```

## CFO和BlendSearch的工作原理

以下两个GIF分别展示了CFO在损失和评估成本（即评估时间）空间中的搜索轨迹。CFO从一个低成本的初始点（通过`low_cost_init_value`在搜索空间中指定）开始，并根据其随机化的局部搜索策略执行局部更新。采用这种策略，CFO可以快速向低损失区域移动，显示出良好的收敛特性。此外，CFO倾向于避免探索高成本区域，直到必要时才会进行。这种搜索策略通过一个[可证明的收敛速率和期望的有界成本](https://arxiv.org/abs/2005.01571)进一步得到支持。

![](../Images/cd50c94bbee1cf7e9397ad9f8c1d2775.png)

图2展示了CFO在调整XGBoost的叶子数和树数时的表现。两个热力图显示了所有配置的损失和成本分布。黑点是CFO评估的点，黑点通过线连接表示评估时产生更好损失表现的点（图像由作者提供）。

BlendSearch进一步将CFO使用的局部搜索策略与全局搜索结合。它利用CFO的节俭性和全局搜索方法（如贝叶斯优化）的空间探索能力。具体来说，BlendSearch维护一个全局搜索模型，并根据全局模型提出的超参数配置逐渐创建局部搜索线程。它根据实时性能和成本优先考虑全局搜索线程和多个局部搜索线程。这可以进一步提高CFO在复杂搜索空间任务中的效率，例如包含多个不连续子空间的搜索空间。

## FLAML与贝叶斯优化性能

图3展示了FLAML中经济型HPO方法（在此图中CFO标记为`LS`）与用于调整具有11个超参数的XGBoost的贝叶斯优化（BO）方法的典型行为。

从图3(a)中，我们观察到BO提出的配置的评估时间可能非常长。当总资源有限时，例如1个cpu小时（或更少），BO无法给出令人满意的结果（图3(b)）。

FLAML的CFO（标记为LS）和BlendSearch在快速找到良好配置方面具有明显优势：它们能够集中于评估时间较短的配置，同时导航于表现良好的配置，即低损失。

![](../Images/48ce7d9823e8612a6841e19d5028e40d.png)

图 3\. (a) 是不同方法提出的超参数配置的散点图，x 轴和 y 轴分别为评估时间和损失。超参数配置的评估时间是指在训练数据上训练机器学习模型并在验证数据集上验证其性能所花费的时间。损失是验证损失。 (b) 展示了不同方法在墙钟时间上的最佳损失。 ([image source](https://openreview.net/pdf?id=VbLH04pRA3))

## 如何通过 Ray Tune 的分布式调优扩展 CFO 和 BlendSearch

为了加速超参数优化，你可能需要并行化你的超参数搜索。例如，BlendSearch 能够在并行设置中良好运行：它利用多个搜索线程，这些线程可以独立执行而性能不会明显下降。这种理想的属性在现有优化算法（如贝叶斯优化）中并不总是成立。

为了实现并行化，FLAML 与 Ray Tune 集成。Ray Tune 是一个 Python 库，通过允许你利用最前沿的优化算法来加速超参数调优。Ray Tune 还允许你将超参数搜索从你的笔记本电脑扩展到集群，而无需更改代码。你可以在 FLAML 中使用 Ray Tune，或在 Ray Tune 中运行 FLAML 的超参数搜索方法以并行化你的搜索。以下代码示例展示了前一种用法，只需通过配置 `n_concurrent_trials` 参数即可实现。

```py
'''Use BlendSearch for hyperparameter search, and Ray Tune for parallelizing concurrent trials (when n_concurrent_trials > 1) in FLAML to tune XGBoost'''
**from** flaml **import** AutoML
automl = AutoML()
automl.add_learner(learner_name='my_xgboost', learner_class=MyXGB)
automl.fit(X_train=X_train, y_train=y_train, time_budget=15, estimator_list=['my_xgboost'], hpo_method='bs', n_concurrent_trials=8)
```

![](../Images/a14bf0e97837d77213d5398128880542.png)

Logo 来源 ([XGBoost](https://github.com/dmlc/xgboost)、[FLAML](https://github.com/microsoft/FLAML)、[Ray Tune](https://github.com/ray-project/ray))

下面的代码展示了后者用法，即如何在 Ray Tune 中使用 BlendSearch 的端到端示例。

```py
**from** ray **import** tune 
**from** flaml **import** CFO, BlendSearch
**import** time

**def** **training_func**(config):
    '''evaluate a hyperparameter configuration'''
    # we use a toy example with 2 hyperparameters
    metric = (round(config['x'])-85000)**2 - config['x']/config['y']

    # usually the evaluation takes a non-neglible cost
    # and the cost could be related to certain hyperparameters
    # in this example, we assume it's proportional to x
    time.sleep(config['x']/100000)
    # use tune.report to report the metric to optimize    
    tune.report(metric=metric) 

# provide the search space
search_space = {
        'x': tune.lograndint(lower=1, upper=100000),
        'y': tune.randint(lower=1, upper=100000)
    }

# provide the low cost partial config
low_cost_partial_config={'x':1}

# set up BlendSearch
blendsearch = BlendSearch(
    metric="metric", mode="min",
    space=search_space,
    low_cost_partial_config=low_cost_partial_config)

blendsearch.set_search_properties(config={"time_budget_s": 60})

analysis = tune.run(
    training_func,    # the function to evaluate a config
    config=search_space,
    metric='metric',    # the name of the metric used for optimization
    mode='min',         # the optimization mode, 'min' or 'max'
    num_samples=-1,    # the maximal number of configs to try, -1 means infinite
    time_budget_s=60,   # the time budget in seconds
    local_dir='logs/',  # the local directory to store logs
    search_alg=blendsearch  # or cfo
    )

print(analysis.best_trial.last_result)  # the best trial's result
print(analysis.best_config)  # the best config
```

其他关键的 Ray Tune 功能包括：

+   自动集成实验跟踪工具，如 Tensorboard 和 Weights/Biases

+   支持 GPU

+   提前停止

+   一个 scikit-learn API 以便轻松集成 [XGBoost](https://www.anyscale.com/blog/distributed-xgboost-training-with-ray)、[LightGBM](https://www.anyscale.com/blog/introducing-distributed-lightgbm-training-with-ray)、[Scikit-Learn](https://github.com/ray-project/tune-sklearn) 等。

## 基准测试结果

我们进行了一项实验，以检查 BlendSearch 在高度并行化设置中与 Optuna（使用多变量 TPE 采样器）和随机搜索的对比。我们使用了来自 [AutoML Benchmark](https://www.openml.org/s/218) 的 12 个数据集的子集。每次优化运行使用 16 个并行试验进行 20 分钟，采用 3 折交叉验证，使用 ROC-AUC（针对多类数据集的加权一对多）。这些运行重复三次，使用不同的随机种子。重现代码可以在 [这里](https://github.com/Yard1/Blendsearch-on-Ray-benchmark) 找到。

![](../Images/d8580e50635a1d1a0d2d0cdd7c82268e.png)

图片由作者提供

BlendSearch 在 12 个数据集中的 6 个上实现了最佳交叉验证得分。此外，BlendSearch 相比随机搜索平均提高了 2.52%，而 Optuna 则为 1.96%。值得注意的是，BlendSearch 使用了单变量的 Optuna-TPE 作为全局搜索器——使用多变量 TPE 很可能进一步提高得分。

![](../Images/c286e797360d45720a6c4d34881c0612.png)

图片由作者提供

此外，由于其节省成本的方式，BlendSearch 平均在相同时间限制内评估的试验次数是其他搜索器的两倍。这表明，随着时间预算的增加，BlendSearch 与其他算法之间的差距将会扩大。

## 结论

FLAML 是一个新发布的库，包含了 [最先进的](https://arxiv.org/abs/2005.01571) 超参数优化算法，利用搜索空间的结构同时优化成本和模型性能。FLAML 还可以利用 [Ray Tune](https://docs.ray.io/en/latest/tune/index.html) 进行分布式超参数调优，以在集群中扩展这些经济高效的 AutoML 方法。

有关 FLAML 的更多信息，请参见 [GitHub 仓库](https://github.com/microsoft/FLAML) 和 [项目页面](https://aka.ms/flaml)。如果你想及时了解 Ray 的一切，请考虑 [关注 @raydistributed 的 Twitter](https://twitter.com/raydistributed) 并 [订阅新闻通讯](https://anyscale.us5.list-manage.com/subscribe?u=524b25758d03ad7ec4f64105f&id=d94e960a03)。

[原文](https://towardsdatascience.com/fast-automl-with-flaml-ray-tune-64ff4a604d1c)。转载已获许可。

**相关：**

+   [TPOT 机器学习管道优化](/2021/05/machine-learning-pipeline-optimization-tpot.html)

+   [二分类自动机器学习](/2021/05/binary-classification-automated-machine-learning.html)

+   [Hugging Face AutoNLP 概述及示例项目](/2021/06/overview-autonlp-hugging-face-example-project.html)

* * *

## 我们的前三课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

### 更多相关话题

+   [如何微调 ChatGPT 3.5 Turbo](https://www.kdnuggets.com/how-to-finetune-chatgpt-35-turbo)

+   [如何使用 Hugging Face AutoTrain 微调 LLMs](https://www.kdnuggets.com/how-to-use-hugging-face-autotrain-to-finetune-llms)

+   [如何使用 Hugging Face Transformers 对 BERT 进行情感分析的微调](https://www.kdnuggets.com/how-to-fine-tune-bert-sentiment-analysis-hugging-face-transformers)

+   [BERT 在稀疏化条件下能达到多快？](https://www.kdnuggets.com/2022/04/fast-bert-go-sparsity.html)

+   [通过快速克里金（FKR）加速机器学习](https://www.kdnuggets.com/2022/06/vmc-speed-machine-learning-fast-kriging.html)

+   [如何让 Python 代码运行得惊人迅速](https://www.kdnuggets.com/2021/06/make-python-code-run-incredibly-fast.html)
