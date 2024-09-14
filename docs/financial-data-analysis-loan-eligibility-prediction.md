# 财务数据分析 – 数据处理 1：贷款资格预测

> 原文：[`www.kdnuggets.com/2018/09/financial-data-analysis-loan-eligibility-prediction.html`](https://www.kdnuggets.com/2018/09/financial-data-analysis-loan-eligibility-prediction.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**作者 [Sabber Ahamed](https://www.linkedin.com/in/sabber-ahamed/)，计算地球物理学家和机器学习爱好者**

![](img/293b4a75c53681e17ad1aebb8977b940.png)

### 介绍

金融机构/公司已经使用预测分析有一段时间了。最近，由于计算资源的可用性以及在机器学习领域的巨大研究，使得更好的数据分析以及更好的预测成为可能。在这系列文章中，我将解释如何创建一个预测贷款模型，识别那些更有可能被拒绝的申请人。通过一步步的过程，我展示了如何处理原始数据、清理不必要的部分、选择相关特征、进行探索性数据分析，并最终建立模型。

作为一个例子，我使用了 Lending Club 贷款数据集。Lending Club 是全球最大的在线市场，连接借款人和投资者。借贷的一个不可避免的结果是借款人违约。这个教程的目的是创建一个预测模型，以识别那些相对来说风险较大的贷款申请人。为此，我将整个系列分为以下四部分：

+   [**数据处理-1**](https://medium.com/@sabber/financial-data-analysis-80ba39149126)：在这第一部分，我展示了如何清理和移除不必要的特征。数据处理是非常耗时的，但更好的数据将产生更好的模型。因此，需要进行仔细且非常详细的检查，以准备更好的数据。我展示了如何识别常量特征、重复特征、重复行以及具有大量缺失值的特征。

+   [**数据处理-2**](https://medium.com/@sabber/financial-data-analysis-bf4b5e78c45c)：在这一部分，我手动检查了从第一部分中选择的每一个特征。这是最耗时的部分，但为了得到更好的模型是值得的。

+   [**探索性数据分析**](https://medium.com/@sabber/financial-data-analysis-2f86b1341e6e)：在这一部分，我对第一部分和第二部分中选择的特征进行了探索性数据分析（EDA）。良好的 EDA 对于更好地了解领域是必需的。我们需要花费一些时间来找出特征之间的关系。

+   [**创建模型**](https://medium.com/@sabber/financial-data-analysis-51e7275d0ae)：最后，在这最后但并非最末部分中，我创建了模型。创建模型也不是一件容易的事。这也是一个迭代的过程。我展示了如何从一个简单的模型开始，然后逐步增加复杂性以获得更好的性能。

好的，让我们开始第一部分：数据处理、清洗和特征选择。

### **数据处理-1**

```py
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import warnings
warnings.filterwarnings("ignore")
```

在这个项目中，我使用了三年的数据集（2014 年、2015 年和 2017 年（第一至第三季度）），并存储在五个不同的 CSV 文件中。首先让我们读取这些文件：

```py
df1 = pd.read_csv(‘./data/2017Q1.csv’, skiprows=[0])
df2 = pd.read_csv(‘./data/2017Q2.csv’, skiprows=[0])
df3 = pd.read_csv(‘./data/2017Q3.csv’, skiprows=[0])
df4 = pd.read_csv(‘./data/2014.csv’, skiprows=[0])
df5 = pd.read_csv(‘./data/2015.csv’, skiprows=[0])
```

由于数据存储在不同的文件中，我们必须确保每个文件中的特征数量相同。我们可以使用以下代码片段来检查：

```py
columns = np.dstack((list(df1.columns), list(df2.columns), list(df3.columns), list(df4.columns), list(df5.columns)))
coldf = pd.DataFrame(columns[0])
```

上述代码不言而喻，我们首先提取列名，然后使用 Numpy 的 `dstack` 对象将它们堆叠在一起。如果你查看 Github 上的 Jupyter-notebook，你会看到它们是相同的。这对我们来说是好的。我们可以继续下一步。现在是检查数据形状的时候了：

```py
df = pd.concat([df1, df2, df3, df4, df5])
df.shape
```

```py
(981665, 151)
```

我们看到大约有一百万个样本，每个样本包含 151 个特征，包括目标变量。让我们查看特征名称以熟悉数据。了解领域，尤其是特征与目标变量的关系的细节是至关重要的。要彻底了解并不容易，所以需要花费几天或一周的时间来熟悉数据，然后再深入分析。让我们查看特征名称：

```py
print(list(df.columns))
```

```py
['id', 'member_id', 'loan_amnt', 'funded_amnt', 'funded_amnt_inv', 'term', 'int_rate', 'installment', 'grade', 'sub_grade', 'emp_title', 'emp_length', 'home_ownership', 'annual_inc', 'verification_status', 'issue_d', 'loan_status', 'pymnt_plan', 'url', 'desc', 'purpose', 'title', 'zip_code', 'addr_state', 'dti', 'delinq_2yrs', 'earliest_cr_line', 'fico_range_low', 'fico_range_high', 'inq_last_6mths', 'mths_since_last_delinq', 'mths_since_last_record', 'open_acc', 'pub_rec', 'revol_bal', 'revol_util', 'total_acc', 'initial_list_status', 'out_prncp', 'out_prncp_inv', 'total_pymnt', 'total_pymnt_inv', 'total_rec_prncp', 'total_rec_int', 'total_rec_late_fee', 'recoveries', 'collection_recovery_fee', 'last_pymnt_d', 'last_pymnt_amnt', 'next_pymnt_d', 'last_credit_pull_d', 'last_fico_range_high', 'last_fico_range_low', 'collections_12_mths_ex_med', 'mths_since_last_major_derog', 'policy_code', 'application_type', 'annual_inc_joint', 'dti_joint', 'verification_status_joint', 'acc_now_delinq', 'tot_coll_amt', 'tot_cur_bal', 'open_acc_6m', 'open_act_il', 'open_il_12m', 'open_il_24m', 'mths_since_rcnt_il', 'total_bal_il', 'il_util', 'open_rv_12m', 'open_rv_24m', 'max_bal_bc', 'all_util', 'total_rev_hi_lim', 'inq_fi', 'total_cu_tl', 'inq_last_12m', 'acc_open_past_24mths', 'avg_cur_bal', 'bc_open_to_buy', 'bc_util', 'chargeoff_within_12_mths', 'delinq_amnt', 'mo_sin_old_il_acct', 'mo_sin_old_rev_tl_op', 'mo_sin_rcnt_rev_tl_op', 'mo_sin_rcnt_tl', 'mort_acc', 'mths_since_recent_bc', 'mths_since_recent_bc_dlq', 'mths_since_recent_inq', 'mths_since_recent_revol_delinq', 'num_accts_ever_120_pd', 'num_actv_bc_tl', 'num_actv_rev_tl', 'num_bc_sats', 'num_bc_tl', 'num_il_tl', 'num_op_rev_tl', 'num_rev_accts', 'num_rev_tl_bal_gt_0', 'num_sats', 'num_tl_120dpd_2m', 'num_tl_30dpd', 'num_tl_90g_dpd_24m', 'num_tl_op_past_12m', 'pct_tl_nvr_dlq', 'percent_bc_gt_75', 'pub_rec_bankruptcies', 'tax_liens', 'tot_hi_cred_lim', 'total_bal_ex_mort', 'total_bc_limit', 'total_il_high_credit_limit', 'revol_bal_joint', 'sec_app_fico_range_low', 'sec_app_fico_range_high', 'sec_app_earliest_cr_line', 'sec_app_inq_last_6mths', 'sec_app_mort_acc', 'sec_app_open_acc', 'sec_app_revol_util', 'sec_app_open_act_il', 'sec_app_num_rev_accts', 'sec_app_chargeoff_within_12_mths', 'sec_app_collections_12_mths_ex_med', 'sec_app_mths_since_last_major_derog', 'hardship_flag', 'hardship_type', 'hardship_reason', 'hardship_status', 'deferral_term', 'hardship_amount', 'hardship_start_date', 'hardship_end_date', 'payment_plan_start_date', 'hardship_length', 'hardship_dpd', 'hardship_loan_status', 'orig_projected_additional_accrued_interest', 'hardship_payoff_balance_amount', 'hardship_last_payment_amount', 'disbursement_method', 'debt_settlement_flag', 'debt_settlement_flag_date', 'settlement_status', 'settlement_date', 'settlement_amount', 'settlement_percentage', 'settlement_term']
```

看着这些特征，可能一开始会感到害怕。但我们会逐一处理每个特征，然后选择相关特征。让我们从目标特征“loan_status”开始。

```py
df.loan_status.value_counts()
```

```py
Current               500937
Fully Paid            358629
Charged Off            99099
Late (31-120 days)     13203
In Grace Period         6337
Late (16-30 days)       3414
Default                   36
Name: loan_status, dtype: int64
```

我们看到有七种贷款状态。然而，在本教程中，我们对两个类别感兴趣：1）全额偿还：那些已经还清贷款并支付利息的人，2）呆账：那些无法偿还贷款并最终被呆账处理的人。因此，我们选择这两个类别的数据集：

```py
df = df.loc[(df['loan_status'].isin(['Fully Paid', 'Charged Off']))]
```

```py
df.shape
(457728, 151)
```

看着形状，我们看到现在的数据点比原始数据少了一半，但特征数量相同。在进行手动处理和清理之前，让我们先做一些一般的数据处理步骤：

+   移除与 >85% 缺失值相关的特征

+   移除常量特征

+   移除重复的特征

+   移除重复的行

+   移除高度共线的特征（在第三部分 EDA 中）

好的，让我们开始典型的数据处理：

**1. 移除与 90% 缺失值相关的特征：** 在下面的代码中，我首先使用 pandas 的内置方法 `isnull()` 查找与缺失值相关的行。然后，我对它们进行求和以获取每个特征的计数。最后，我按缺失值的数量对特征进行排序，并创建一个数据框以便进一步分析。

在上述结果中，我们看到有 53 个特征缺失了 400000 个值。我使用 pandas 的 drop 方法移除这 53 个特征。注意在此函数中我将“inplace”选项设置为“True”，这会在不返回任何内容的情况下从原始数据框**df**中删除这些特征。

**2\. 移除常量特征：**在这一步，我们移除具有单一唯一值的特征。一个与单一唯一值相关的特征无法帮助模型进行良好的泛化，因为它的方差为零。基于树的模型无法利用这些特征，因为模型无法对这些特征进行拆分。识别具有单一唯一值的特征相对简单：

在上面的代码中，我创建了一个“find_constant_features”函数来识别常量特征。该函数逐个检查每个特征，看看是否具有少于两个唯一值。如果是，这些特征会被添加到常量特征列表中。我们还可以通过查看方差或标准差来找出常量特征。如果特征的方差或标准差为零，我们可以确定该特征具有单一唯一值。打印语句显示五个特征具有单一唯一值，因此我们使用“inplace”选项为 true 移除了它们。

**3\. 移除重复特征：**重复特征是指在多个特征中具有相同值的特征，不论名称是否相同。为了找出这些重复特征，我借用了以下代码，这些代码来自于这个[stackoverflow 链接](https://stackoverflow.com/questions/14984119/python-pandas-remove-duplicate-columns)：

我们只看到一个似乎重复的特征。我不会立即移除该特征，而是等到我们在下一部分进行 EDA 时再处理。

**4\. 移除重复行：**在这一步，我们移除所有重复的行。我使用 pandas 内置的“drop_duplicates(inplace= True)”方法来执行此操作：

```py
df.drop_duplicates(inplace= True)
```

上述四个处理步骤是任何数据科学项目中需要做的基本步骤。让我们看看经过这些步骤后的数据形状：

```py
df.shape

(457728, 93)
```

我们发现经过上述步骤后，我们有 93 个特征。

在这个教程的[next part](https://medium.com/@sabber/financial-data-analysis-bf4b5e78c45c)中，我将逐一检查每个特征，然后进行清理，必要时将其移除。同时，如果你对这一部分有任何问题，请随时在下方写下你的评论。你可以联系我：

```py
Email: sabbers@gmail.com
LinkedIn: https://www.linkedin.com/in/sabber-ahamed/
Github: https://github.com/msahamed
Medium: https://medium.com/@sabber/
```

**个人简介: [Sabber Ahamed](https://www.linkedin.com/in/sabber-ahamed/)** 是 [xoolooloo.com](https://www.xoolooloo.com/) 的创始人，计算地球物理学家及机器学习爱好者。

[原文](https://medium.com/@sabber/financial-data-analysis-80ba39149126)。经许可转载。

**相关：**

+   命令行上的文本挖掘

+   三种技术提升不平衡数据集上的机器学习模型性能

+   文本分类与嵌入可视化，使用 LSTM、CNN 和预训练词向量

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 需求

* * *

### 更多相关主题

+   [机器学习模型可解释性如何加速金融服务领域的人工智能采纳](https://www.kdnuggets.com/2022/07/ml-model-explainability-accelerates-ai-adoption-journey-financial-services.html)

+   [烂番茄电影评分预测的数据科学项目：…](https://www.kdnuggets.com/2023/06/data-science-project-rotten-tomatoes-movie-rating-prediction-first-approach.html)

+   [烂番茄电影评分预测的数据科学项目：…](https://www.kdnuggets.com/2023/07/data-science-project-rotten-tomatoes-movie-rating-prediction-second-approach.html)

+   [使用 BQML 进行多变量时间序列预测](https://www.kdnuggets.com/2023/07/multivariate-timeseries-prediction-bqml.html)

+   [OLAP 与 OLTP：数据处理系统的比较分析](https://www.kdnuggets.com/2023/08/olap-oltp-comparative-analysis-data-processing-systems.html)

+   [自然语言处理任务的数据表示](https://www.kdnuggets.com/2018/11/data-representation-natural-language-processing.html)
