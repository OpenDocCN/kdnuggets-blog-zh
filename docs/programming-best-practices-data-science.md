# 数据科学编程最佳实践

> 原文：[`www.kdnuggets.com/2018/08/programming-best-practices-data-science.html`](https://www.kdnuggets.com/2018/08/programming-best-practices-data-science.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**作者：Srini Kadamati，[Dataquest.io](https://www.dataquest.io/)**

数据科学生命周期通常包含以下组件：

+   数据检索

+   数据清理

+   数据探索和可视化

+   统计或预测建模

* * *

## 我们的前 3 个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速踏上网络安全职业生涯之路。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

虽然这些组件有助于理解不同的阶段，但它们并没有帮助我们深入*编程*工作流程。

通常，整个数据科学生命周期最终变成了*任意*的笔记本单元格混合体，这些单元格位于 Jupyter Notebook 中或是一个混乱的脚本中。此外，大多数数据科学问题要求我们在数据检索、数据清理、数据探索、数据可视化和统计/预测建模之间切换。

但还有更好的方法！在这篇文章中，我将深入探讨大多数人在进行数据科学编程工作时切换的两种思维方式：**原型**思维和**生产**思维。

| 原型思维优先考虑： | 生产思维优先考虑： |
| --- | --- |
| 小片段代码的迭代速度 | 整个管道的迭代速度 |
| 较少抽象（直接修改代码和数据对象） | 更多抽象（改为修改参数值） |
| 代码结构较少（模块化较少） | 代码结构更多（模块化更多） |
| 帮助你和他人理解代码和数据 | 帮助计算机自动运行代码 |

我个人使用[JupyterLab](http://jupyterlab.readthedocs.io/en/stable/getting_started/installation.html)来完成整个过程（原型设计和生产化）。我推荐至少在原型设计阶段使用 JupyterLab**。

### **Lending Club 数据**

为了更具体地理解原型设计思维和生产思维之间的区别，让我们使用一些真实数据。我们将使用来自点对点借贷网站[Lending Club](https://www.dataquest.io/blog/programming-best-practices-for-data-science/www.lendingclub.com)的借贷数据。与银行不同，Lending Club 并不直接借出资金。Lending Club 实际上是一个市场，供借款人向寻求贷款的个人提供贷款（如家庭维修、婚礼费用等）。我们可以利用这些数据构建模型，预测给定的贷款申请是否会成功。我们不会在这篇文章中深入探讨构建预测机器学习管道，但在我们的[机器学习项目演练课程](https://www.dataquest.io/course/machine-learning-project)中有涉及。

Lending Club 提供详细的历史数据，涵盖已完成的贷款（Lending Club 批准的贷款申请，并找到贷方）和被拒绝的贷款（Lending Club 拒绝的贷款申请，资金未发生变动）。请前往他们的[data download page](https://www.lendingclub.com/info/download-data.action)并在**DOWLNOAD LOAN DATA**下选择**2007-2011**。

![lendingclub](img/5893dcd88190c71f1a9b9914ef8fd8e0.png)

### **原型思维**

在原型思维中，我们关注的是快速迭代，尝试理解数据的一些属性和真相。创建一个新的 Jupyter notebook，并添加一个 Markdown 单元格来解释：

+   您对 Lending Club 进行的任何研究，以更好地了解该平台

+   有关您下载的数据集的任何信息

首先，让我们将 CSV 文件读入 pandas。

```py
import pandas as pd
loans_2007 = pd.read_csv('LoanStats3a.csv')
loans_2007.head(2)

```

我们得到两部分输出，首先是一个警告。

```py
/home/srinify/anaconda3/envs/dq2/lib/python3.6/site-packages/IPython/core/interactiveshell.py:2785: DtypeWarning: Columns (0,1,2,3,4,7,13,18,24,25,27,28,29,30,31,32,34,36,37,38,39,40,41,42,43,44,46,47,49,50,51,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,101,102,103,104,105,106,107,108,109,110,111,112,113,114,115,116,117,118,119,120,121,123,124,125,126,127,128,129,130,131,132,133,134,135,136,142,143,144) have mixed types. Specify dtype option on import or set low_memory=False.
  interactivity=interactivity, compiler=compiler, result=result)

```

然后是数据框的前 5 行，我们在这里避免展示（因为内容较长）。

我们还得到了以下数据框输出：

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 注释由 Prospectus 提供 ([链接](https://www.lendingclub.com/info/prospectus.action)) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| d | member_id | loan_amnt | funded_amnt | funded_amnt_inv | term | int_rate | installment | grade | sub_grade | emp_title | emp_length | home_ownership | annual_inc | verification_status | issue_d | loan_status | pymnt_plan | url | desc | purpose | title | zip_code | addr_state | dti | delinq_2yrs | earliest_cr_line | inq_last_6mths | mths_since_last_delinq | mths_since_last_record | open_acc | pub_rec | revol_bal | revol_util | total_acc | initial_list_status | out_prncp | out_prncp_inv | total_pymnt | total_pymnt_inv | total_rec_prncp | total_rec_int | total_rec_late_fee | recoveries | collection_recovery_fee | last_pymnt_d | last_pymnt_amnt | next_pymnt_d | last_credit_pull_d | collections_12_mths_ex_med | mths_since_last_major_derog | policy_code | application_type | annual_inc_joint | dti_joint | verification_status_joint | acc_now_delinq | tot_coll_amt | tot_cur_bal | open_acc_6m | open_act_il | open_il_12m | open_il_24m | mths_since_rcnt_il | total_bal_il | il_util | open_rv_12m | open_rv_24m | max_bal_bc | all_util | total_rev_hi_lim | inq_fi | total_cu_tl | inq_last_12m | acc_open_past_24mths | avg_cur_bal | bc_open_to_buy | bc_util | chargeoff_within_12_mths | delinq_amnt | mo_sin_old_il_acct | mo_sin_old_rev_tl_op | mo_sin_rcnt_rev_tl_op | mo_sin_rcnt_tl | mort_acc | mths_since_recent_bc | mths_since_recent_bc_dlq | mths_since_recent_inq | mths_since_recent_revol_delinq | num_accts_ever_120_pd | num_actv_bc_tl | num_actv_rev_tl | num_bc_sats | num_bc_tl | num_il_tl | num_op_rev_tl | num_rev_accts | num_rev_tl_bal_gt_0 | num_sats | num_tl_120dpd_2m | num_tl_30dpd | num_tl_90g_dpd_24m | num_tl_op_past_12m | pct_tl_nvr_dlq | percent_bc_gt_75 | pub_rec_bankruptcies | tax_liens | tot_hi_cred_lim | total_bal_ex_mort | total_bc_limit | total_il_high_credit_limit | revol_bal_joint | sec_app_earliest_cr_line | sec_app_inq_last_6mths | sec_app_mort_acc | sec_app_open_acc | sec_app_revol_util | sec_app_open_act_il | sec_app_num_rev_accts | sec_app_chargeoff_within_12_mths | sec_app_collections_12_mths_ex_med | sec_app_mths_since_last_major_derog | hardship_flag | hardship_type | hardship_reason | hardship_status | deferral_term | hardship_amount | hardship_start_date | hardship_end_date | payment_plan_start_date | hardship_length | hardship_dpd | hardship_loan_status | orig_projected_additional_accrued_interest | hardship_payoff_balance_amount | hardship_last_payment_amount | disbursement_method | debt_settlement_flag | debt_settlement_flag_date | settlement_status | settlement_date | settlement_amount | settlement_percentage | settlement_term |

警告提示我们，如果在调用`pandas.read_csv()`时将`low_memory`参数设置为`False`，pandas 对每列的类型推断将会得到改善。

第二个输出更有问题，因为 DataFrame 存储数据的方式存在问题。JupyterLab 内置了一个终端环境，我们可以打开它并使用 bash 命令 `head` 来观察原始文件的前两行：

```py
head -2 LoanStats3a.csv

```

虽然第二行包含我们期望的 CSV 文件中的列名，但看起来第一行在 pandas 尝试解析文件时使 DataFrame 的格式出现了问题：

```py
Notes offered by Prospectus (https://www.lendingclub.com/info/prospectus.action)

```

添加一个 Markdown 单元格，详细描述你的观察结果，并添加一个代码单元格，考虑这些观察结果。

```py
import pandas as pd
loans_2007 = pd.read_csv('LoanStats3a.csv', skiprows=1, low_memory=False)

```

从 [Lending Club 下载页面](https://www.lendingclub.com/info/download-data.action) 阅读数据字典，以了解哪些列不包含对特征有用的信息。`desc` 和 `url` 列似乎立即符合这个标准。

```py
loans_2007 = loans_2007.drop(['desc', 'url'],axis=1)

```

下一步是删除任何具有超过 50% 缺失行的列。使用一个单元格来探索哪些列符合该标准，另一个单元格实际删除这些列。

```py
loans_2007.isnull().sum()/len(loans_2007)

```

```py
loans_2007 = loans_2007.dropna(thresh=half_count, axis=1)

```

因为我们使用 Jupyter notebook 来跟踪我们的想法和代码，我们依赖于环境（通过 IPython 内核）来跟踪状态的变化。这使我们可以自由格式化，移动单元格，重复运行相同的代码等。

通常，原型思维模式下的代码应专注于：

+   可理解性

    +   Markdown 单元格用以描述我们的观察和假设

    +   实际逻辑的小段代码

    +   许多可视化和计数

+   最小化抽象

    +   大多数代码不应该放在函数中（应该感觉更面向对象）

假设我们花了一个小时来探索数据并编写描述数据清洗的 markdown 单元格。然后我们可以切换到生产思维模式，使代码更加健壮。

### **生产思维**

在生产思维模式下，我们要专注于编写可以泛化到更多情况的代码。在我们的例子中，我们希望我们的数据清洗代码适用于 Lending Club 的任何数据集（来自其他时间段）。将代码泛化的最佳方法是将其转化为**数据管道**。数据管道是使用来自 [函数式编程](https://www.dataquest.io/blog/introduction-functional-programming-python/) 的原则设计的，其中数据在函数*内部*被修改，然后在函数*之间*传递。

这是使用单个函数封装数据清洗代码的管道的第一次迭代：

```py
import pandas as pd

def import_clean(file_list):
    frames = []
    for file in file_list:
        loans = pd.read_csv(file, skiprows=1, low_memory=False)
        loans = loans.drop(['desc', 'url'], axis=1)
        half_count = len(loans)/2
        loans = loans.dropna(thresh=half_count, axis=1)
        loans = loans.drop_duplicates()
        # Drop first group of features
        loans = loans.drop(["funded_amnt", "funded_amnt_inv", "grade", "sub_grade", "emp_title", "issue_d"], axis=1)
        # Drop second group of features
        loans = loans.drop(["zip_code", "out_prncp", "out_prncp_inv", "total_pymnt", "total_pymnt_inv", "total_rec_prncp"], axis=1)
        # Drop third group of features
        loans = loans.drop(["total_rec_int", "total_rec_late_fee", "recoveries", "collection_recovery_fee", "last_pymnt_d", "last_pymnt_amnt"], axis=1)
        frames.append(loans)
    return frames

frames = import_clean(['LoanStats3a.csv'])

```

在上述代码中，我们将之前的代码**抽象**为一个单独的函数。该函数的输入是文件名列表，输出是 DataFrame 对象列表。

通常，生产思维模式应专注于：

+   健康的抽象

    +   代码应该进行泛化，以兼容类似的数据源

    +   代码不应过于泛化，以至于难以理解

+   管道稳定性

    +   可靠性应与其运行的频率匹配（每日？每周？每月？）

### **思维模式切换**

假设我们尝试对来自 Lending Club 的所有数据集运行函数，而 Python 返回了错误。一些潜在的错误来源包括：

+   一些文件中列名的方差

+   由于 50% 的缺失值阈值，丢弃的列的方差

+   基于 pandas 类型推断的不同列类型

在这些情况下，我们应该实际切换回我们的原型笔记本并进一步调查。当我们确定需要让我们的管道更灵活并考虑数据中的特定变化时，我们可以将其重新纳入管道逻辑中。

这是一个示例，我们调整了函数以适应不同的丢弃阈值：

```py
import pandas as pd

def import_clean(file_list, threshold=0.5):
    frames = []
    for file in file_list:
        loans = pd.read_csv(file, skiprows=1, low_memory=False)
        loans = loans.drop(['desc', 'url'], axis=1)
        threshold_count = len(loans)*threshold
        loans = loans.dropna(thresh=half_count, axis=1)
        loans = loans.drop_duplicates()
        # Drop first group of features
        loans = loans.drop(["funded_amnt", "funded_amnt_inv", "grade", "sub_grade", "emp_title", "issue_d"], axis=1)
        # Drop second group of features
        loans = loans.drop(["zip_code", "out_prncp", "out_prncp_inv", "total_pymnt", "total_pymnt_inv", "total_rec_prncp"], axis=1)
        # Drop third group of features
        loans = loans.drop(["total_rec_int", "total_rec_late_fee", "recoveries", "collection_recovery_fee", "last_pymnt_d", "last_pymnt_amnt"], axis=1)
        frames.append(loans)
    return frames

frames = import_clean(['LoanStats3a.csv'], threshold=0.7)

```

默认值仍然是`0.5`，但如果需要，我们可以将其覆盖为`0.7`。

这里有几种方式可以使管道更加灵活，按优先级递减：

+   使用可选、位置性和必需的参数

+   在函数内使用 if / then 语句以及布尔输入值

+   使用新的数据结构（字典、列表等）来表示特定数据集的自定义操作

这个管道可以扩展到数据科学工作流程的所有阶段。这里有一些示例代码，预览了它的样子。

```py
import pandas as pd

def import_clean(file_list, threshold=0.5):
    ## Code

def visualize(df_list):
    # Find the most important features and generate pairwise scatter plots
    # Display visualizations and write to file.
    plt.savefig("scatter_plots.png")

def combine(df_list):
    # Combine dataframes and generate train and test sets
    # Drop features all dataframes don't share
    # Return both train and test dataframes
    return train,test

def train(train_df):
    # Train model
    return model

def validate(train_df, test-df):
    # K-fold cross validation
    # Return metrics dictionary
    return metrics_dict

frames = import_clean(['LoanStats3a.csv', 'LoanStats2012.csv'], threshold=0.7)
visualize(frames)
train_df, test_df = combine(frames)
model = train(train_df)
metrics = test(train_df, test_df)
print(metrics)

```

### **下一步**

如果你有兴趣深化理解和进一步实践，我推荐以下步骤：

+   学习如何将管道转换为可以作为模块或从命令行运行的独立脚本： [`docs.python.org/3/library/**main**.html`](https://docs.python.org/3/library/__main__.html)

+   学习如何使用 Luigi 构建可以在云中运行的更复杂的管道： [在 Python 和 Luigi 中构建数据管道](https://marcobonzanini.com/2015/10/24/building-data-pipelines-with-python-and-luigi/)

+   了解更多关于数据工程的内容： [Dataquest 上的数据工程帖子](https://www.dataquest.io/blog/tag/data-engineering/)

**生物：Srini Kadamati** 是 **[Dataquest.io](https://www.dataquest.io/)** 的内容总监。

[原文](https://www.dataquest.io/blog/programming-best-practices-for-data-science/?utm_source=kdnuggets&utm_medium=crosspost&utm_content=text)。转载已获许可。

**相关：**

+   Swiftapply  – 自动高效的 pandas apply 操作

+   与机器学习算法相关的数据结构

+   Python 函数式编程简介

### 更多相关主题

+   [是什么使 Python 成为初创企业的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [停止学习数据科学以寻找目标，并通过寻找目标来...](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的五大特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [每个数据科学家都应该了解的三个 R 语言库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [一个 90 亿美元的 AI 失败案例分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)
