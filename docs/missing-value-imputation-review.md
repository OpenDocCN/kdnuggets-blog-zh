# 缺失值填补 – 综述

> 原文：[`www.kdnuggets.com/2020/09/missing-value-imputation-review.html`](https://www.kdnuggets.com/2020/09/missing-value-imputation-review.html)

评论

**由 [Kathrin Melcher](https://www.linkedin.com/in/kathrin-melcher-b44542155/?originalSubdomain=de)，KNIME 数据科学家，和 [Rosaria Silipo](https://www.linkedin.com/in/rosaria/?originalSubdomain=ch)，KNIME 首席数据科学家**

缺失值出现在从工业到学术的各种数据集中。它们可以以不同的方式表示 - 有时用问号，或 -999，有时用“n/a”，或用其他专用的数字或字符。正确检测和处理缺失值很重要，因为它们会影响分析结果，并且有些算法无法处理它们。那么正确的方法是什么？

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 如何选择正确的策略

填补缺失值的两种常见方法是用固定值（例如零）或所有可用值的均值替代所有缺失值。哪种方法更好？

我们来看两个不同案例研究的效果：

+   案例研究 1：传感器数据中的基于阈值的异常检测

+   案例研究 2：客户聚合数据报告

**案例研究 1：基于阈值的异常检测中的缺失值填补**

在经典的基于阈值的异常检测解决方案中，从原始数据的均值和方差计算出的阈值会应用于传感器数据以生成警报。如果缺失值用固定值（例如零）填补，这将影响用于阈值定义的均值和方差的计算。这可能会导致警报阈值的错误估计以及一些昂贵的停机时间。

在这里，用可用值的均值填补缺失值是正确的做法。

**案例研究 2：聚合客户数据中的缺失值填补**

在经典的客户数据报告中，需要对每个地理区域的客户数量和总收入进行汇总和可视化，例如通过条形图。客户数据集在业务尚未开始或尚未发展时，有些区域的数据缺失，没有记录客户和业务。在这种情况下，使用可用数字的平均值来填补缺失值，将生成客户和收入的记录，但实际上客户和收入并不存在。

这里正确的做法是用固定值零来填补缺失值。

在这两种情况下，我们对过程的了解可以指导我们填补缺失值的正确方法。在传感器数据的情况下，缺失值是由于测量设备故障，实际数值没有被记录。在客户数据集的情况下，缺失值出现的地方还没有需要测量的内容。

从这两个例子中你已经可以看出，没有一种万能的解决方案来处理所有的缺失值填补问题，我们显然无法回答经典问题：“我的数据集缺失值填补的正确策略是什么？”答案过于依赖于领域和业务知识。

然而，我们可以提供对最常用技术的回顾，以：

+   检测数据集中是否包含缺失值及其类型，

+   填补缺失值。

### 检测缺失值及其类型

在尝试理解缺失值的来源和原因之前，我们需要检测它们。缺失值的常见编码包括 n/a、NA、-99、-999、?、空字符串或任何其他占位符。当你打开一个新的数据集时，没有说明书，你需要识别是否使用了任何这样的占位符来表示缺失值。

直方图是查找占位符字符（如果有的话）的好工具。

+   对于数值型数据，许多数据集使用远离数据分布的值来表示缺失值。经典的例子是正值范围中的-999。在图 1 中，直方图显示大多数数据在[3900-6600]范围内呈现出良好的高斯分布。左侧的-99 附近的小条形与其余数据相比显得很不自然，可能是用来指示缺失值的占位符号码。

+   通常，对于名义数据，识别缺失值的占位符更容易，因为字符串格式允许我们写出某些缺失值的参考，例如“未知”或“N/A”。直方图也可以在这里提供帮助。对于名义数据，具有不匹配值的区间可能是缺失值占位符的一个指示。

![图](img/d1d63a4cf0b0d5b9cc5b174045d54925.png)

*图 1：直方图是检测缺失值占位符字符的好工具。在这个例子中，我们看到大多数值落在 3900 和 6600 之间。值-99 看起来相当偏离，在这种情况下，可能是缺失值的占位符。*

这一步骤，即检测表示缺失值的占位符字符/数字，属于数据探索阶段，在分析开始之前。在检测到这个缺失值的占位符字符后，在真正的分析之前，必须根据所使用的数据工具对缺失值进行适当的格式化。

一个有趣的学术练习是对缺失值的类型进行定性分析。缺失值通常被分类为三种不同类型[1][2]。

+   *完全随机缺失 (MCAR)*

    *定义：* 实例缺失的概率不依赖于已知值或缺失值本身。

    *示例：* 一张数据表被打印出来，没有缺失值，但有人不小心在上面滴了一些墨水，导致一些单元格变得不可读[2]。在这种情况下，我们可以假设缺失值遵循与已知值相同的分布。

+   *随机缺失 (MAR)*

    *定义：* 实例缺失的概率可能依赖于已知值，但不依赖于缺失值本身。

    *传感器示例：* 在温度传感器的情况下，值的缺失并不依赖于温度，但可能依赖于其他因素，例如温度计的电池电量。

    *调查示例：* 无论某人是否回答一个问题——例如年龄——在调查中**并不**取决于答案本身，但可能依赖于另一个问题的答案，即女性性别。

+   *非随机缺失 (NMAR)*

    *定义：* 实例缺失的概率可能依赖于变量本身的值。

    *传感器示例：* 在温度传感器的情况下，当温度低于 5°C 时，传感器无法正常工作。

    *调查示例：* 无论某人是否回答了一个问题——例如病假天数——在调查中**确实**取决于答案本身——因为对一些超重的人来说可能会有所不同。

只有数据收集过程的知识和业务经验才能告诉我们发现的缺失值是 MAR、MCAR 还是 NMAR 类型。

在本文中，我们将仅关注 MAR 或 MCAR 类型的缺失值。填补 NMAR 缺失值更加复杂，因为需要考虑比统计分布和统计参数更多的额外因素。

### 处理缺失值的不同方法

多年来提出的处理缺失值的多种方法可以分为两个主要组：*删除*和*填补*。

### 删除方法

有三种常见的删除方法：列表删除、成对删除和删除特征。

+   **列表删除：** 删除所有存在一个或多个缺失值的行。

+   **配对删除：** 仅删除在用于分析的列中具有缺失值的行。如果缺失数据是 MCAR，则仅推荐使用此方法。

+   **特征删除：** 删除缺失值超过给定阈值（例如 60%）的整个列。

![图](img/d69da6cf803044092b58f525067eac1e.png)

*图 2：左侧是一个包含缺失值的表格，其中只有 F1、F2 和 F3 被用于分析。右侧是应用不同删除方法后的结果表格。*

### 插补方法

插补方法的核心思想是用其他合理的值替代缺失值。由于使用删除方法时会丢失信息（无论是删除样本（行）还是整个特征（列）），插补通常是首选方法。

多种插补技术可以分为两类：*单次插补或多次插补*。

在**单次插补**中，为每个缺失的观察值生成一个单一的插补值。插补值被视为真实值，忽略了没有插补方法可以提供确切值这一事实。因此，单次插补不能反映缺失值的不确定性。

在**多次插补**中，为每个缺失的观察值生成多个插补值。这意味着创建了多个具有不同插补值的完整数据集。对每个数据集进行分析（例如，训练线性回归以预测目标列），然后汇总结果。创建多次插补，相对于单次插补，更能考虑插补中的统计不确定性[3][4]。

### 单次插补

大多数插补方法是单次插补方法，主要有三种策略：用现有值替代、用统计值替代和用预测值替代。根据这些策略中使用的值，我们最终得到只适用于数值型数据的方法和同时适用于数值型和名义型列的方法。这些方法在表格 1 中进行了总结，并在下面进行了详细说明。

| **替代方法：** | **仅数值特征** | **数值和名义特征** |
| --- | --- | --- |
| **现有值** | 最小值 / 最大值 | 先前 / 后续 / 固定 |
| **统计值** | （四舍五入的）均值 / 中位数 / 移动平均，线性 / 平均插值 | 最频繁值 |
| **预测值** | 回归算法 | 回归和分类算法，k 最近邻 |

*表 1：仅对数值特征和对数值及名义特征的单次插补方法，基于现有值、统计测量和预测值。*

***固定值***

固定值填补是一种适用于所有数据类型的一般方法，包含将缺失值替代为固定值。我们在本文开头提到的聚合客户示例中使用了固定值填补来处理数值。作为在名义特征上使用固定值填补的示例，你可以在调查中用“未回答”填补缺失值。

***最小 / 最大值***

如果你知道数据必须符合给定范围[最小值，最大值]，并且从数据收集过程中知道测量系统在超出这些边界之一时停止记录且信号饱和，你可以使用范围的最小值或最大值作为缺失值的替代值。例如，如果在货币兑换中已达到最低价格且兑换过程已停止，则可以用法律兑换边界的最小值替代缺失的货币兑换价格。

***(四舍五入的)均值 / 中位数 / 移动平均***

其他常见的数值特征填补方法包括均值、四舍五入均值或中位数填补。在这种情况下，该方法使用整个数据集中该特征计算出的均值、四舍五入均值或中位数值来替代缺失值。在数据集中异常值数量较多的情况下，建议使用中位数而不是均值。

***最频繁值***

另一种适用于数值和名义特征的常见方法是使用列中最频繁的值来替代缺失值。

***前一个 / 下一个值***

有专门针对时间序列或有序数据的填补方法。这些方法考虑了数据集的排序特性，其中接近的值可能比远离的值更相似。时间序列中填补缺失值的一种常见方法是用时间序列中的下一个或上一个值替代缺失值。这种方法适用于数值和名义值。

***线性 / 平均插值***

类似于前一个/下一个值填补，但仅适用于数值，是线性或平均插值，该方法计算前一个和下一个可用值之间的插值，并替代缺失值。当然，对于有序数据的所有操作，提前正确排序数据是重要的，例如在时间序列数据中根据时间戳排序。

***K 最近邻***

这里的想法是寻找数据集中 k 个最近的样本，其中相应特征的值不缺失，并将该组中最常出现的特征值作为缺失值的替代。

***缺失值预测***

另一种常见的单次插补选项是训练机器学习模型，根据其他特征预测特征 x 的插补值。特征 x 中没有缺失值的行用作训练集，模型根据其他列的值进行训练。这里可以使用任何分类或回归模型，具体取决于特征的数据类型。训练后，将模型应用于所有缺失值的样本，以预测其最可能的值。

如果有多个特征列存在缺失值，所有缺失值首先用基本插补方法临时填补，例如均值。然后将一列的值重新设为缺失。然后训练模型并应用于填补缺失值。通过这种方式，为每个有缺失值的特征训练一个模型，直到所有缺失值都通过模型插补完毕。

### 多重插补

多重插补是一种源自统计学的插补方法。单次插补方法的缺点是没有考虑插补值的不确定性。这意味着它们将插补值视为实际值，而没有考虑标准误差，这会导致结果偏差[3][4]。

解决此问题的一种方法是多重插补，即为每个缺失值创建多个插补，而不是一个。这意味着多次填补缺失值，创建多个“完整”的数据集[3][4]。

已经开发出多种算法用于多重插补。其中一个著名的算法是链式方程的多重插补（MICE）。

***链式方程的多重插补（MICE）***

多重插补（Multiple Imputation by Chained Equations，简称 MICE）是一种处理数据集中缺失值的强大而信息丰富的方法。MICE 基于缺失数据是随机缺失（Missing At Random，MAR）或完全随机缺失（Missing Completely At Random，MCAR）的假设[3]。

该过程是“缺失值预测”单次插补过程的扩展（见上文）：这就是第 1 步。然而，MICE 程序中还有两个额外的步骤。

第 1 步：这是在原始数据子集上进行“缺失值预测”插补过程的步骤。训练一个模型来预测一个特征中的缺失值，使用数据行中的其他特征作为模型的自变量。这一步为所有特征重复进行。这是一个循环或迭代。

第 2 步：重复第 1 步 k 次，每次使用最新的插补值作为自变量，直到收敛为止。通常情况下，k=10 次循环已经足够。

第 3 步：整个过程在 N 个不同的随机子集上重复 N 次。得到的 N 个模型会略有不同，并为每个缺失值产生 N 个略有不同的预测值。

分析，例如，为目标变量训练线性回归，现在在每一个最终的数据集上进行。最后，结果被合并，这通常也称为汇总。

这比单一填补提供了更稳健的结果。当然，这种稳健性的缺点是计算复杂度的增加。

### 比较填补技术

许多填补技术。选择哪一种？

有时候我们应该已经知道最佳的填补方法是什么，基于我们对业务和数据收集过程的了解。然而，有时候我们没有线索，只能尝试几种不同的选项，看看哪种效果最好。

要定义“最佳”，我们需要一个任务。对我们指定任务表现最佳的程序就是“最佳”程序。这正是我们在这篇文章中尝试做的：定义一个任务，为任务定义一个成功的度量，尝试几种不同的缺失值填补程序，并比较结果以找到最合适的方案。

让我们将研究范围限定在分类任务上。成功的度量标准将是模型预测的准确度和 Cohen’s Kappa。准确度是平衡类数据集中任务成功的明确度量。然而，[Cohen’s Kappa](https://www.knime.com/blog/cohens-kappa-an-overview)，尽管不易读解，但对于不平衡类数据集来说，代表了更好的成功度量。

我们实施了两个分类任务，每个任务在一个专用的数据集上：

+   在客户流失预测数据集上进行流失预测（3333 行，21 列）

+   在[Census 收入数据集](https://archive.ics.uci.edu/ml/datasets/Census+Income)上进行收入预测（32561 行，15 列）

对于这两个分类任务，我们选择了一个简单的决策树，训练数据使用原始数据的 80%，测试数据使用剩余的 20%。这里的重点是通过观察在使用某一种填补方法而非另一种时模型性能的可能改善来比较不同填补方法的效果。

我们重复了每个分类任务四次：在原始数据集上，以及在引入 10%、20%和 25% MCAR 类型缺失值后。这意味着我们随机删除数据集中的值，并将其转换为缺失值。

每次，我们都尝试了四种不同的缺失值填补技术（见图 3）。

+   **删除**：逐行删除（蓝色）

+   **0 填补**：用零进行固定值填补（橙色）

+   **均值 - 最频繁**：对数值进行均值填补，对名义值进行最频繁值填补（绿色）

+   **线性回归 - kNN**：用线性回归预测数值的缺失值，用 kNN 预测名义值的缺失值（红色）

图 3 比较了在对原始数据集和人工插入缺失值版本应用四种选择的插补方法后的决策树的准确性和 Cohen's Kappa 值。

![图](img/61935d7013950b976d88e09e14cf6666.png)

*图 3：应用四种选择的插补方法到原始数据集及其随机插入缺失值的变体后，决策树模型在两个不同分类任务上的准确性（左侧）和 Cohen's Kappa 值（右侧）。*

对于四种插补方法中的三种，我们可以看到一个普遍趋势，即缺失值比例越高，准确性和 Cohen's Kappa 值通常越低。唯一的例外是删除方法（蓝线）。

此外，我们无法看到明确的最佳方法。这支持了我们关于最佳插补方法取决于使用案例和数据的观点。

流失数据集是一个类别流失不平衡的数据集，其中类别 0（未流失）远多于类别 1（流失）。列表删除在这里导致非常小的数据集，使得无法训练出有意义的模型。在这个例子中，我们在测试集中只剩下一行数据，正好被预测正确（蓝线）。这解释了 100%的准确率和缺失的 Cohen's Kappa 值。

所有其他插补技术在所有数据集变体上的决策树性能在准确性和 Cohen's Kappa 值方面都差不多。然而，最好的结果是通过缺失值预测方法获得的，使用线性回归和 kNN。

人口普查收入数据集相较于流失预测数据集更大，其中两个收入类别<=50K 和>50K 也是不平衡的。图 3 中的图示显示，均值和最频繁插补方法在准确性和 Cohen's Kappa 值方面优于缺失值预测方法以及 0 插补方法。在删除方法的情况下，人口普查数据集的结果不稳定，并且依赖于由列表删除产生的子集。在这里使用的设置中，删除（蓝线）对于小比例缺失值的性能有所提升，但对于 25%或更多缺失值的性能较差。

### 比较应用程序

用于比较所有描述的技术并生成图 3 中的图表的应用程序是使用[KNIME Analytics Platform](http://link)开发的（见图 4）。

在这里，一个循环遍历数据集的四个变体：0%、10%、20%和 25%缺失值。在每次迭代中，循环中的两个分支之一实现两个分类任务之一：流失预测或收入预测。特别是每个分支：

+   读取数据集并在此循环迭代设置的百分比上撒入缺失数据。

+   随机将数据划分为 80%-20%的比例，分别用于训练和测试所选任务的决策树

+   根据四种选定的方法填充缺失值，并训练和测试决策树

+   计算不同模型的准确性和 Cohen 的 Kappa 系数。

之后，两个循环分支被合并，Loop End 节点收集不同迭代的性能结果，然后通过“可视化结果”组件进行可视化

+   [你可以从 KNIME Hub 下载工作流“比较缺失值处理方法”。](https://kni.me/w/bCTrFr60t7fWWlDi)

![图](img/a18d495e96c3ddba6275570908bf03cd.png)

*图 4：该工作流使用循环随机替换 10%、20%和 25%的值为缺失值。在每次迭代中，使用四种不同的方法对缺失值进行填充。之后，训练并应用决策树于每个数据集变体，最后对不同迭代的性能进行可视化。*

这里关注的组件是“填充缺失值并训练和应用模型”。其内容如图 5 所示：四个分支，每个分支对应一种填充技术。

前三条分支实现了逐列表删除（“删除”）、用零填充值（“0 填充”）、使用均值填充数值特征和使用最频繁值填充名义特征的统计度量（“均值 - 最频繁”）。

最后一条分支实现了缺失值预测填充，使用线性回归处理数值特征，kNN 处理名义特征（“线性回归 - kNN”）。

![图](img/091a89df823826036cabc10ae60d6991.png)

*图 5：“填充缺失值并训练和应用模型”组件内部，使用强大的缺失值节点进行填充，同时包括线性回归、kNN 和 MICE。*

让我们用几句话来描述一下[缺失值节点](https://kni.me/n/uVmaGQkzUOFCTqUe)，简单而有效。缺失值节点提供了大部分介绍的单一填充技术（仅 kNN 和预测模型方法不可用）。在这里，你可以根据选定的策略对所有数据集或逐列（特征）填充缺失值。

![图](img/d09a2279e215d9e5d4beee3fe6c51fc2.png)

*图 6：缺失值节点的配置窗口。在第一个标签页中，可以为整个数据集定义每种数据类型的默认填充方法，在第二个标签页中可以为每一列定义。*

### KNIME Analytics Platform 中的多重填充

在上述工作流中，[比较缺失值处理方法](https://kni.me/w/bCTrFr60t7fWWlDi)，我们看到了如何在 KNIME Analytics Platform 中应用不同的单次插补方法。而且明显可以建立一个循环来实现使用 MICE 算法的多重插补方法。KNIME Analytics Platform 的一个优势是我们不必重新发明轮子，可以轻松集成 Python 和 R 中可用的算法。

R 中的“mice”包允许你插补混合的连续、二元、无序分类和有序分类数据，并从许多不同的算法中进行选择，创建多个完整的数据集。[5]

在 Python 中，“IterativeImputar”函数受到了 MICE 算法的启发。它以相同的轮回方式多次迭代不同的列，但只创建一个插补数据集。通过使用不同的随机种子，可以创建多个完整的数据集。[6]

总体而言，在用户不关心因缺失值导致的不确定性的情况下，单次插补与多重插补在预测和分类中的有效性仍然是一个未解问题。

图 7 中的工作流，[缺失值的多重插补](https://kni.me/w/FMVnse1-jvF_jZRw)，展示了如何使用 R 的“mice”包进行多重插补，创建五个完整的数据集。

![图](img/2e9b3d394d56d2014cb1c7fadaf17f85.png)

*图 7.**这个工作流使用 R 的“mice”包来执行多重插补。然后在每个完整的数据集上使用 KNIME Analytics Platform 进行分析。*

工作流读取了输入特征的 25% 值被替换为缺失值后的普查数据集。在 R 片段节点中，加载并应用了 R 的“mice”包以创建五个完整的数据集。此外，为每一行添加了一个索引，用于标识不同的完整数据集。

+   [你可以从 KNIME Hub 下载工作流“缺失值的多重插补”](https://kni.me/w/FMVnse1-jvF_jZRw)。

在下一步骤中，一个循环处理不同的完整数据集，通过在每次迭代中训练和应用决策树。在工作流的最后部分，通过统计每个类别被预测的频率来汇总预测结果，并提取最多数预测的类别。最后，使用 Scorer 节点评估结果。在 Iris “mice”插补的数据集上，模型达到了 83.867% 的准确率。相比之下，单次插补方法在缺失值为 25% 的数据集上达到了 77% 到 80% 的准确率。

### 总结

所有数据集都有缺失值。需要知道如何处理这些缺失值。我们是应该完全删除数据行，还是用某个合理的值来替代缺失值？

在这篇博客文章中，我们描述了一些常见的删除和插补缺失值的技术。我们随后实现了四种最具代表性的技术，并在两个不同的分类问题上比较了它们在处理逐渐增加的缺失值时的表现效果。

总结一下，我们可以得出以下结论。

使用全列表删除（“删除”）时需谨慎，特别是在小数据集上。删除数据时，你是在删除信息。并非所有数据集都有多余的信息可以舍弃！我们在流失预测任务中见证了这种显著的效果。

使用固定值插补时，你需要了解该固定值在数据领域和业务问题中的含义。在这里，你将任意信息注入到数据中，这可能会影响最终模型的预测。

如果你想在没有先验知识的情况下插补缺失值，很难说哪种插补方法效果最佳，因为这高度依赖于数据本身。

最后需要说明的一点是，所有结果仅适用于这两个简单任务、相对简单的决策树和小数据集。对于更复杂的情况，相同的结果可能不适用。

最终，没有什么比对任务和数据收集过程的先验知识更重要！

**参考文献：**

[1] Peter Schmitt, Jonas Mandel 和 Mickael Guedj，“六种缺失数据插补方法的比较”，《生物统计与生物测定学》

[2] M.R. Berthold, C. Borgelt, F. Höppner, F. Klawonn, R. Silipo，“智能数据科学指南”，Springer，2020

[3] lissa J. Azur, Elizabeth A. Stuart, Constantine Frangakis 和 Philip J. Leaf 1，“链式方程的多重插补：它是什么，它是如何工作的？”链接：[`www.ncbi.nlm.nih.gov/pmc/articles/PMC3074241/`](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3074241/)

[4] Shahidul Islam Khan, Abu Sayed Md Latiful Hoque，“SICE：一种改进的缺失数据插补技术”，链接：[`link.springer.com/content/pdf/10.1186/s40537-020-00313-w.pdf`](https://link.springer.com/content/pdf/10.1186/s40537-020-00313-w.pdf)

[5] Python 文档。链接：[`scikit-learn.org/stable/modules/impute.html`](https://scikit-learn.org/stable/modules/impute.html)

[6] 人口普查收入数据集：[`archive.ics.uci.edu/ml/datasets/Census+Income`](https://archive.ics.uci.edu/ml/datasets/Census+Income)

**相关：**

+   Python 中的数据预处理简易指南

+   自动化机器学习：究竟有多大？

+   如何处理数据集中的缺失值

### 更多相关主题

+   [使用 Datawig，AWS 的深度学习库进行缺失值插补](https://www.kdnuggets.com/2021/12/datawig-aws-deep-learning-library-missing-value-imputation.html)

+   [三种数据插补方法](https://www.kdnuggets.com/2022/12/3-approaches-data-imputation.html)

+   [数据插补方法](https://www.kdnuggets.com/2023/01/approaches-data-imputation.html)

+   [语义层是 AI 驱动分析的缺失环节](https://www.kdnuggets.com/2024/02/cube-semantic-layers-missing-piece-ai-enabled-analytics)

+   [如何使用 Pandas 中的插值技术处理缺失数据](https://www.kdnuggets.com/how-to-deal-with-missing-data-using-interpolation-techniques-in-pandas)

+   [NumPy 中的掩码数组处理缺失数据](https://www.kdnuggets.com/masked-arrays-in-numpy-to-handle-missing-data)
