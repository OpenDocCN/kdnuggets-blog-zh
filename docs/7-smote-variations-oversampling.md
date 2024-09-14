# 7 种 SMOTE 变体用于过采样

> 原文：[https://www.kdnuggets.com/2023/01/7-smote-variations-oversampling.html](https://www.kdnuggets.com/2023/01/7-smote-variations-oversampling.html)

![7 种 SMOTE 变体用于过采样](../Images/51405691ec8c00074ee722553f91a123.png)

作者提供的图片

不平衡数据集是数据科学中的一个问题。这个问题发生是因为不平衡通常会导致建模性能问题。为了缓解不平衡问题，我们可以使用过采样方法。过采样是对少数类数据进行重采样以平衡数据。

* * *

## 我们的 Top 3 课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 方面

* * *

过采样有许多方法，其中之一是使用 SMOTE1。让我们深入了解多种 SMOTE 实现，以进一步了解过采样技术。

# SMOTE 变体

在继续之前，我们将使用来自 Kaggle2 的流失数据集来表示不平衡数据集。数据集的目标是“exited”变量，我们将观察 SMOTE 如何根据少数类目标进行过采样。

```py
import pandas as pd

df = pd.read_csv('churn.csv')
df['Exited'].value_counts().plot(kind = 'bar', color = ['blue', 'red'])
```

![7 种 SMOTE 变体用于过采样](../Images/5b16aed043c0b0d2ee7e7c9512c2c565.png)

我们可以看到流失数据集面临不平衡问题。让我们尝试使用 SMOTE 对数据进行过采样。

## 1\. SMOTE

SMOTE 通常用于通过生成人工或合成数据来过采样连续数据以解决机器学习问题。我们使用连续数据是因为开发样本的模型只接受连续数据1。

在我们的示例中，我们将使用数据集中的两个连续变量：“EstimatedSalary”和“Age”。让我们看看这两个变量与数据目标的分布情况。

```py
import seaborn as sns
sns.scatterplot(data =df, x ='EstimatedSalary', y = 'Age', hue = 'Exited')
```

![7 种 SMOTE 变体用于过采样](../Images/247ba76232da66be0420332cbb7251af.png)

我们可以看到少数类大多分布在图的中间部分。让我们尝试用 SMOTE 进行过采样，看看差异如何形成。为了便于 SMOTE 过采样，我们将使用 imblearn Python 包。

```py
pip install imblearn
```

使用 imblearn，我们将对流失数据进行过采样。

```py
from imblearn.over_sampling import SMOTE
smote = SMOTE(random_state = 42)

X, y = smote.fit_resample(df[['EstimatedSalary', 'Age']], df['Exited'])
df_smote = pd.DataFrame(X, columns = ['EstimatedSalary', 'Age'])
df_smote['Exited'] = y
```

Imblearn 包基于 scikit-learn API，使用起来很方便。在上述示例中，我们已经使用 SMOTE 对数据集进行了过采样。让我们看看“Exited”变量的分布。

```py
df_smote['Exited'].value_counts().plot(kind = 'bar', color = ['blue', 'red'])
```

![7 种 SMOTE 变体用于过采样](../Images/d6afa0e2c70679f864ee48eaf4395334.png)

从上述输出中可以看出，目标变量现在具有类似的比例。让我们看看连续变量在新的 SMOTE 过采样数据中的分布情况。

```py
import matplotlib.pyplot as plt

sns.scatterplot(data = df_smote, x ='EstimatedSalary', y = 'Age', hue = 'Exited')
plt.title('SMOTE')
```

![7种SMOTE过采样变体](../Images/5c4060008e7281ac205d75fdfc340fe2.png)

上面的图显示了少数类数据现在比我们过采样前的分布更广泛了。如果我们更详细地查看输出，可以看到少数类数据的分布仍然接近核心，并且比之前扩展得更广。这是因为样本是基于邻居模型的，该模型根据最近邻估算样本。

## 2. SMOTE-NC

SMOTE-NC是针对分类数据的SMOTE。如上所述，SMOTE只适用于连续数据。

为什么我们不直接将分类变量编码为连续变量呢？

问题在于SMOTE基于最近邻创建样本。如果你对分类数据进行编码，比如‘HasCrCard’变量，它包含类0和1，样本结果可能是0.8或0.34等。

从数据的角度来看，这没有意义。这就是为什么我们可以使用SMOTE-NC来确保分类数据的过采样是合理的。

让我们用示例数据试试。对于这个特定的样本，我们会使用变量‘HasCrCard’和‘Age’。首先，我想展示初始的‘HasCrCard’变量分布。

```py
pd.crosstab(df['HasCrCard'], df['Exited'])
```

![7种SMOTE过采样变体](../Images/d39cbb30be66ffae4d58de33496ac4a5.png)

然后让我们看看使用SMOTE-NC进行过采样后的差异。

```py
from imblearn.over_sampling import SMOTENC

smotenc = SMOTENC([1],random_state = 42)
X_os_nc, y_os_nc = smotenc.fit_resample(df[['Age', 'HasCrCard']], df['Exited'])
```

注意上面的代码中，分类变量的位置是基于DataFrame中变量的位置。

让我们看看‘HasCrCard’在过采样后的分布。

```py
pd.crosstab(X_os_nc['HasCrCard'], y_os_nc)
```

![7种SMOTE过采样变体](../Images/fc7ae07f005dacdeb4f240e3e0ee78ff.png)

可以看到，数据的过采样几乎保持了相同的比例。你可以尝试用其他分类变量看看SMOTE-NC的效果。

## 3. Borderline-SMOTE

Borderline-SMOTE是一种基于分类器边界的SMOTE。Borderline-SMOTE会对接近分类器边界的数据进行过采样。这是因为接近边界的样本更容易被误分类，因此更重要需要进行过采样。

Borderline-SMOTE有两种类型：Borderline-SMOTE1和Borderline-SMOTE2。它们的区别在于，Borderline-SMOTE1会对接近边界的两个类都进行过采样，而Borderline-SMOTE2只会对少数类进行过采样。

我们来尝试一下使用Borderline-SMOTE与一个数据集示例。

```py
from imblearn.over_sampling import BorderlineSMOTE
bsmote = BorderlineSMOTE(random_state = 42, kind = 'borderline-1')
X_bd, y_bd = bsmote.fit_resample(df[['EstimatedSalary', 'Age']], df['Exited'])
df_bd = pd.DataFrame(X_bd, columns = ['EstimatedSalary', 'Age'])
df_bd['Exited'] = y_bd
```

让我们看看在启动Borderline-SMOTE后，数据的分布情况。

```py
sns.scatterplot(data = df_bd, x ='EstimatedSalary', y = 'Age', hue = 'Exited')
plt.title('Borderline-SMOTE')
```

![7种SMOTE过采样变体](../Images/c5c02737968d67533432aa0896917e02.png)

如果我们查看上述结果，输出与SMOTE输出类似，但Borderline-SMOTE的过采样结果稍微接近于边界。

## 4. SMOTE-Tomek

SMOTE-Tomek使用了SMOTE和欠采样Tomek链接的组合。Tomek链接是一种清理数据的方法，用于去除与少数类重叠的多数类。

让我们尝试对样本数据集应用SMOTE-TOMEK。

```py
from imblearn.combine import SMOTETomek

s_tomek = SMOTETomek(random_state = 42)
X_st, y_st = s_tomek.fit_resample(df[['EstimatedSalary', 'Age']], df['Exited'])
df_st = pd.DataFrame(X_st, columns = ['EstimatedSalary', 'Age'])
df_st['Exited'] = y_st
```

让我们看看使用SMOTE-Tomek后的目标变量结果。

```py
df_st['Exited'].value_counts().plot(kind = 'bar', color = ['blue', 'red'])
```

![7 SMOTE变体用于过采样](../Images/624fe93e93594333095629bcdc4c3bb8.png)

'Exited' 类别 0 的数量现在约为6000，相比之下，原始数据集接近8000。这是因为SMOTE-Tomek在进行过采样少数类的同时，对类别0进行了欠采样。

让我们看看在使用SMOTE-Tomek进行过采样后的数据分布。

```py
sns.scatterplot(data = df_st, x ='EstimatedSalary', y = 'Age', hue = 'Exited')
plt.title('SMOTE-Tomek')
```

![7 SMOTE变体用于过采样](../Images/781cc7836229a363b2150612220cb883.png)

结果分布仍然类似于之前。但如果我们深入了解，距离数据越远，过采样的少数类样本越少。

## 5\. SMOTE-ENN

类似于SMOTE-Tomek，SMOTE-ENN（编辑最近邻）结合了过采样和欠采样。SMOTE进行了过采样，而ENN进行了欠采样。

编辑最近邻是一种在原始和样本结果数据集中移除多数类样本的方法，当最近邻少数类样本将其错误分类时。它会移除接近边界的多数类样本，这些样本被错误分类了。

让我们尝试使用SMOTE-ENN与示例数据集。

```py
from imblearn.combine import SMOTEENN

s_enn = SMOTEENN(random_state=42)
X_se, y_se = s_enn.fit_resample(df[['EstimatedSalary', 'Age']], df['Exited'])
df_se = pd.DataFrame(X_se, columns = ['EstimatedSalary', 'Age'])
df_se['Exited'] = y_se
```

让我们看看SMOTE-ENN的结果。首先，我们会查看目标变量。

```py
df_se['Exited'].value_counts().plot(kind = 'bar', color = ['blue', 'red'])
```

![7 SMOTE变体用于过采样](../Images/3cd38b1285804752b1707c7b62035c6f.png)

与SMOTE-Tomek相比，SMOTE-ENN的欠采样过程严格得多。从上面的结果来看，超过一半的原始'Exited' 类别 0 被欠采样，仅少数类有所略微增加。

让我们看看应用SMOTE-ENN后的数据分布。

```py
sns.scatterplot(data = df_se, x ='EstimatedSalary', y = 'Age', hue = 'Exited')
plt.title('SMOTE-ENN')
```

![7 SMOTE变体用于过采样](../Images/ada4df12952cd5b1987ab8a11b9b45c4.png)

类别之间的数据分布比之前大得多。然而，我们需要记住，结果数据的数量较少。

## 6\. SMOTE-CUT

SMOTE-CUT或SMOTE-聚类欠采样技术结合了过采样、聚类和欠采样。

SMOTE-CUT 实现了使用 SMOTE 进行过采样，聚类原始数据和结果，并从簇中移除多数类样本。

SMOTE-CUT聚类基于EM（期望最大化）算法，该算法为每个数据分配属于各个簇的概率。聚类结果会引导算法进行过采样或欠采样，从而使数据集分布变得平衡。

让我们尝试使用数据集示例。对于这个例子，我们将使用 crucio Python 包。

```py
pip install crucio 
```

使用crucio包，我们通过以下代码对数据集进行过采样。

```py
from crucio import SCUT
df_sample = df[['EstimatedSalary', 'Age', 'Exited']].copy()

scut = SCUT()
df_scut= scut.balance(df_sample, 'Exited')
```

让我们看看目标数据分布。

```py
df_scut['Exited'].value_counts().plot(kind = 'bar', color = ['blue', 'red'])
```

![7 SMOTE变体用于过采样](../Images/13e2592695372363fc26e179816038d8.png)

尽管欠采样过程相当严格，'Exited' 类别的分布仍然相等。许多'Exited' 类别 0 的样本由于欠采样而被移除。

让我们看看实施SMOTE-CUT后的数据分布。

```py
sns.scatterplot(data = df_scut, x ='EstimatedSalary', y = 'Age', hue = 'Exited')
plt.title('SMOTE-CUT')
```

![7 SMOTE变体用于过采样](../Images/cf9e04ca34b10324a8f6e29428a6dfe5.png)

数据分布更分散，但仍然少于SMOTE-ENN。

## 7. ADASYN

ADASYN或自适应合成采样是一种SMOTE，它试图根据数据密度对少数数据进行过采样。ADASYN会为每个少数样本分配一个加权分布，并优先对更难学习的少数样本进行过采样7。

让我们尝试用示例数据集进行ADASYN。

```py
from crucio import ADASYN
df_sample = df[['EstimatedSalary', 'Age', 'Exited']].copy()

ada = ADASYN()
df_ada= ada.balance(df_sample, 'Exited')
```

让我们看看目标分布结果。

```py
df_ada['Exited'].value_counts().plot(kind = 'bar', color = ['blue', 'red'])
```

![7种SMOTE变体进行过采样](../Images/9e569e33feaf9fb3bab6a836986dacdc.png)

由于ADASYN会关注那些更难学习或密度较低的数据，因此其过采样结果低于其他方法。

让我们看看数据的分布情况。

```py
sns.scatterplot(data = df_ada, x ='EstimatedSalary', y = 'Age', hue = 'Exited')
plt.title('ADASYN')
```

![7种SMOTE变体进行过采样](../Images/f1ab93711ff7a2c1413dbcbfea31b28e.png)

从上面的图像中可以看出，数据的分布更接近核心，但也接近低密度的少数数据。

# 结论

数据不平衡是数据领域中的一个问题。缓解不平衡问题的一种方法是通过SMOTE对数据集进行过采样。随着研究的发展，已经创建了许多我们可以使用的SMOTE方法。

在这篇文章中，我们将探讨7种不同的SMOTE技术，包括

1.  SMOTE

1.  SMOTE-NC

1.  边界线-SMOTE

1.  SMOTE-TOMEK

1.  SMOTE-ENN

1.  SMOTE-CUT

1.  ADASYN

## 参考文献

1.  SMOTE: 合成少数类过采样技术 - [Arxiv.org](https://arxiv.org/abs/1106.1813)

1.  [客户流失建模](https://www.kaggle.com/datasets/shubh0799/churn-modelling)数据集来自Kaggle，许可证为CC0: 公开领域。

1.  边界线-SMOTE: 不平衡数据集学习中的一种新过采样方法 - [Semanticscholar.org](https://www.semanticscholar.org/paper/Borderline-SMOTE:-A-New-Over-Sampling-Method-in-Han-Wang/b618f88ebaab51c4d38182e773419478abe44cf8)

1.  平衡自动标注关键词的训练数据：一个案例研究 - [inf.ufrgs.br](https://www.inf.ufrgs.br/maslab/pergamus/pubs/balancing-training-data-for.pdf)

1.  改进慢性心力衰竭不良结果的风险识别：使用SMOTE+ENN和机器学习 - [dovepress.com](https://www.dovepress.com/improving-risk-identification-of-adverse-outcomes-in-chronic-heart-fai-peer-reviewed-fulltext-article-RMHP#cit0022)

1.  使用Crucio SMOTE和聚类欠采样技术处理不平衡数据集 - [sigmoid.ai](https://www.sigmoidai.org/using-crucio-smote-and-clustered-undersampling-technique-for-unbalanced-datasets/)

1.  ADASYN: 自适应合成采样方法用于不平衡学习 - [ResearchGate](https://www.researchgate.net/publication/224330873_ADASYN_Adaptive_Synthetic_Sampling_Approach_for_Imbalanced_Learning)

**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**是一名数据科学助理经理和数据撰稿人。在全职工作于印尼安联保险期间，他喜欢通过社交媒体和写作媒体分享Python和数据技巧。

### 更多相关话题

+   [SMOTE简介](https://www.kdnuggets.com/2022/11/introduction-smote.html)
