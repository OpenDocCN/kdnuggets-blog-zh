# 在进行机器学习和数据科学时常见的错误

> 原文：[https://www.kdnuggets.com/2018/12/common-mistakes-data-science.html](https://www.kdnuggets.com/2018/12/common-mistakes-data-science.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由 [Jekaterina Kokatjuhha](https://www.linkedin.com/in/jekaterina-kokatjuhha/)，Zalando 公司的研究工程师撰写。**

这是本系列的第二部分，第一部分请参见 - [**如何从零开始构建数据科学项目**](https://www.kdnuggets.com/2018/12/build-data-science-project-from-scratch.html)。

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

在抓取或获取数据之后，还有许多步骤需要完成**在**应用机器学习模型之前。

你需要可视化每个变量，以查看分布，找到异常值，并理解为何会有这些异常值。

你可以对某些特征中的缺失值做些什么？

将分类特征转换为数值特征的最佳方法是什么？

有许多类似的问题，但我会详细说明大多数初学者遇到错误的那些问题。

#### 1\. 可视化

首先，你应该可视化连续特征的分布，以便了解是否有许多异常值，分布情况如何，以及是否合理。

可视化的方式有很多，例如 [箱线图](https://www.khanacademy.org/math/statistics-probability/summarizing-quantitative-data/box-whisker-plots/a/box-plot-review)、[直方图](https://en.wikipedia.org/wiki/Histogram)、[累计分布函数](https://en.wikipedia.org/wiki/Cumulative_distribution_function) 和 [小提琴图](https://en.wikipedia.org/wiki/Violin_plot)。然而，应该选择能提供关于数据最多信息的图表。

要查看分布（是否为 [正态分布](https://en.wikipedia.org/wiki/Normal_distribution) 或 [双峰分布](https://en.wikipedia.org/wiki/Multimodal_distribution)），直方图将是最有用的。虽然直方图是一个很好的起点，但箱线图在识别异常值的数量和查看中位数四分位数方面可能更优。

根据图表，最有趣的问题是：**你是否看到了你预期的结果？**回答这个问题将帮助你发现见解或找出数据中的错误。

为了获得灵感并了解哪种图表会带来最大价值，我经常参考 [Python 的 seaborn 画廊](https://seaborn.pydata.org/examples/index.html)。另一个很好的可视化和洞察发现的灵感来源是 Kaggle 上的 Kernels。[这是我的 Kaggle Kernel](https://www.kaggle.com/jkokatjuhha/in-depth-visualisations-simple-methods)，用于深入可视化 Titanic 数据集。

在租金的背景下，我绘制了每个连续特征的直方图，并预计租金（不含账单）和总面积的分布会有较长的右尾。

![](../Images/c89bc5e86afd341fed4b789799b00190.png)

**连续特征的直方图**

箱线图帮助我查看了每个特征的异常值数量。实际上，大多数基于租金（不含账单）的异常公寓要么是面积超过200m²的小商铺要么是租金非常低的学生宿舍。

![](../Images/404bdbb751bfd3f901a5b02fdb627afd.png)

**连续特征的箱线图**

#### 2\. 我是否根据整个数据集来填补缺失值？

有时会因为各种原因出现缺失值。如果我们排除每个至少有一个缺失值的观测值，可能会导致数据集非常缩减。

有 [许多填补方法](https://www.iriseekhout.com/missing-data/missing-data-methods/imputation-methods/) ，例如均值或中位数。如何进行填补是你自己的选择，**但** 要确保只在训练数据上计算填补统计，以避免 [**数据泄露**](https://machinelearningmastery.com/data-leakage-machine-learning/)。

在租赁数据中，我还提取了公寓的描述。每当质量、条件或公寓类型缺失时，我会从描述中填补这些信息，如果描述中包含这些信息的话。

#### 3\. 我如何转换分类变量？

一些算法根据实现方式，可能无法直接处理分类数据，因此需要以某种方式将其转换为数值型。

将分类变量转换为数值特征的方法有很多，例如 [Label Encoder、One Hot Encoding、bin encoding](http://pbpython.com/categorical-encoding.html) 和哈希编码。然而，大多数人错误地使用 Label Encoding，而应该使用 One Hot Encoding。

假设在我们的租赁数据中，有一个公寓类型列，值为：[ground floor, loft, maisonette, loft, loft, ground floor]。LabelEncoder 可以将其转换为 [3,2,1,2,2,1]，引入序次，这意味着 ground_floor > loft > maisonette。对于像决策树及其变种这样的算法，这种特征编码方式是可以接受的，但对回归和 SVM 应用可能意义不大。

在租金数据集中，**条件** 编码如下：

+   new:1

+   renovated:2

+   需要翻新: 3

和 **质量** 如下：

+   Luxus:1

+   好于正常: 2

+   正常: 3

+   简单: 4

+   未知: 5

#### 4\. 我需要对变量进行标准化吗？

标准化将所有连续变量缩放到相同的范围，这意味着如果一个变量的值从1K到1M，而另一个变量的值从0.1到1，标准化后它们将具有相同的范围。

[L1或L2正则化](https://towardsdatascience.com/l1-and-l2-regularization-methods-ce25e7fc831c)是减少[过拟合](https://machinelearningmastery.com/overfitting-and-underfitting-with-machine-learning-algorithms/)的常见方法，可以在许多回归算法中使用。然而，在应用L1或L2之前进行特征标准化是很重要的。

租金以欧元为单位，因此拟合的系数会比价格以分为单位时的拟合系数大约大100倍。L1和L2会对较大的系数施加更多惩罚，这意味着它会对较小尺度的特征施加更多惩罚。为了避免这种情况，特征应该在应用L1或L2之前进行标准化。

标准化的另一个原因是，如果你或你的算法使用梯度下降，梯度下降在特征缩放的情况下会收敛得更快。

#### 5\. 我需要对目标变量进行对数变换吗？

我花了一段时间才明白**没有普遍的答案**。

这取决于许多因素：

+   你是想要分数误差还是绝对误差

+   你使用的是哪种算法

+   残差图和指标变化告诉你什么

在回归中，首先[关注残差图](http://docs.statwing.com/interpreting-residual-plots-to-improve-your-regression/)和指标。有时，对目标变量进行对数变换会得到更好的模型，模型的结果仍然容易理解。然而，还有其他的变换可能也值得关注，比如取平方根。

关于这个问题在Stack Overflow上有很多答案，我认为[残差图和原始与对数目标变量的RMSE](https://stats.stackexchange.com/questions/319880/non-linear-regression-residual-plots-and-rmse-on-raw-and-log-target-variable)解释得非常好。

对于租赁数据，我对价格进行了对数变换，因为残差图看起来好一点。

![](../Images/fe1f5cc65c6786934dee94174e02a61f.png)

**对数的残差图（左）和未变换数据（右）不包括账单变量的租金。右侧的图展示了“异方差性”——残差随着预测从小到大而变大。**

#### 其他一些重要内容

一些算法，如回归，会受到数据中的[共线性](https://en.wikipedia.org/wiki/Multicollinearity)的影响，因为系数变得非常不稳定（[更多数学](http://www.stat.cmu.edu/~larry/=stat401/lecture-17.pdf)）。[支持向量机（SVM）](https://en.wikipedia.org/wiki/Support_vector_machine) [可能会或可能不会受到](https://stats.stackexchange.com/questions/149662/is-support-vector-machine-sensitive-to-the-correlation-between-the-attributes)共线性的影响，这取决于核函数的选择。

基于决策的算法不会受到多重共线性的影响，因为它们可以在不同的树中互换使用特征，而不会影响性能。然而，特征重要性的解释变得更困难，因为相关变量可能不会显得像实际那样重要。

### 机器学习

在你熟悉数据并清理了异常值之后，是时候掌握机器学习了。你可以使用许多算法进行这种监督式机器学习。

我想探索三种不同的算法，比较它们的特性，如性能差异和速度。这三种算法分别是不同实现的梯度提升树（XGBoost和LightGBM）、随机森林（FR，scikit-learn）和3层神经网络（NN，Tensorflow）。我选择了RMSLE（均方根对数误差）作为过程优化的指标。我使用RMSLE是因为我对目标变量进行了对数转换。

XGBoost和LightGBM表现相当，RF稍差，而NN表现最差。

![](../Images/baa8f77c7d797cc5f8898e8441cd8cf0.png)

**算法在测试集上的表现（RMSLE）。**

基于决策树的算法非常擅长解释特征。例如，它们会生成特征重要性分数。

#### 特征重要性：寻找租金价格的驱动因素

在拟合决策树模型后，你可以看到哪些特征对于价格预测最为宝贵。

特征重要性提供了一个分数，表示每个特征在模型中构建决策树时的贡献度。计算这个分数的一种方法是统计一个特征在所有树中被用于分割数据的次数。这个分数可以通过[不同的方式](https://datascience.stackexchange.com/questions/12318/how-do-i-interpret-the-output-of-xgboost-importance)来计算。

特征重要性可以揭示关于主要价格驱动因素的其他见解。

对于租金价格预测来说，总面积是价格最重要的驱动因素也不足为奇。有趣的是，一些通过外部API工程化的特征也在最重要的特征之列。

![](../Images/818ee95c8e2f3873c5cbbad1f9d44e12.png)

**通过分割（上图）和增益（下图）计算的特征重要性**

然而，如在[《可解释的机器学习与XGBoost》](https://towardsdatascience.com/interpretable-machine-learning-with-xgboost-9ec80d148d27)中提到的，根据归因选项，特征重要性可能会存在不一致。链接博客的作者和SHAP NIPS论文提出了一种新的特征重要性计算方法，该方法既准确又一致。它使用了[shap Python库](https://github.com/slundberg/shap)。SHAP值表示特征对模型输出变化的责任。

租赁价格数据分析的结果见下图。

![](../Images/ba1d5ac0a74b69d880a8d7afc3cd0aae.png)

**每个公寓在每一行上都有一个点。点的x位置表示该特征对模型预测的影响，点的颜色表示该特征对公寓的值**

该图包含了大量有价值的信息（特征按均值排序（|Tree SHAP|））。小说明：数据来自2018年初；地区可能会有所变化，因此价格相关因素也可能会变化。

+   离市中心的距离（开车到U-Bahn Stadtmitte的公里数以及乘火车到S-Bahn Friedrichstrasse的时间）会提高预测的租赁公寓价格。

+   总面积是租赁价格最重要的驱动因素。

+   如果公寓所有者要求你提供低收入证明（德语中的WBS），则预测价格较低。

+   在这些区域租赁公寓也会提高租金价格：Mitte, Prenzlauer Berg, Wilmersdorf, Charlottenburg, Zehlendorf 和 Friedrichshain。

+   价格较低的地区包括：Spandau, Tempelhof, Wedding 和 Reinickendorf

+   显然，公寓的状态越好——数值越低越好——质量越高——数值越低越好——有家具、内置厨房和电梯的公寓会更贵。

以下特征的影响很有趣：

+   到最近地铁站的时间

+   1公里范围内的车站数量。

**到最近地铁站的时间：**

对于一些公寓，这一特征的高值表示价格较高。原因是这些公寓位于柏林市外的非常富裕的住宅区。

还可以看到，靠近地铁站有两种效果：对某些公寓，它会降低**和**提高价格。原因可能是距离地铁站非常近的公寓也会受到地下噪音或列车震动的影响，但另一方面，这些公寓与公共交通连接良好。然而，仍需深入研究这一特征，因为它仅显示了距离最近的地铁站的距离，而不是电车/公交车站。

**1公里范围内的车站数量：**

对于距离公寓一公里以内的车站数量也是如此。一般来说，附近的地铁站越多，租金价格会越高。然而，它也有负面影响——更多的噪声。

#### 集成平均

在尝试不同模型并比较性能之后，你可以将每个模型的结果进行组合并构建一个集成模型！

Bagging是一种机器学习集成模型，它利用多个算法的预测来计算最终的聚合预测。它旨在防止过拟合并减少算法的方差。

![](../Images/b0acefa672b5eefe0d69f4a84bc74bd4.png)

**使用集成模型的优点：红色模型在左下角的表现更好，而蓝色模型在右上角的表现更好。通过结合这两个模型的预测，可以提升整体性能。图取自 [这里。](https://burakhimmetoglu.com/2016/12/01/stacking-models-for-improved-predictions/)** 

由于我已经有了上述算法的预测，我将所有四个模型以所有可能的方式进行组合，并根据验证集的RMSLE选择了七个最佳的单模型和集成模型。

然后计算了这七个模型在测试集上的RMSLE。

![](../Images/54e522617bfcb8d870e87a119d1cebbc.png)

**算法的测试RMSLE。**

三种基于决策树的算法的集成效果最好，相较于每个单一模型。

你也可以产生一个加权集成模型，给予更好的单一模型更多的权重。其理由是，其他模型只有在它们一致同意一个替代方案时，才可能会推翻最好的模型。

实际上，人们永远不会知道一个平均的集成模型是否比单一模型更好，除非尝试一下。

#### 堆叠模型

平均或加权的集成模型不是唯一的结合不同模型预测的方法。你也可以以非常不同的方式堆叠模型！

堆叠模型的理念是创建多个基础模型，并在基础模型的结果之上构建一个元模型，以产生最终预测。然而，训练元模型并不那么明显，因为它可能会偏向于最好的基础模型。关于如何正确进行堆叠的一个很好的解释可以在帖子 [“堆叠模型以改进预测”](https://burakhimmetoglu.com/2016/12/01/stacking-models-for-improved-predictions/) 中找到。

对于租金价格的情况，堆叠模型并没有改善RMSLE——它们甚至增加了指标。这可能有几个原因——要么是我编码有误 ;) 要么就是堆叠引入了过多噪声。

如果你想探索更多关于集成和堆叠模型的文章，[Kaggle 集成指南](https://mlwave.com/kaggle-ensembling-guide/) 解释了多种不同类型的集成，包括性能比较和如何让这些堆叠模型在Kaggle竞赛中名列前茅的参考资料。

### **最终思考**

+   倾听你周围人的谈话，他们的抱怨可以作为解决重大问题的良好起点

+   通过提供交互式仪表板，让人们发现自己的见解

+   不要将自己限制在常见的特征工程（例如两个变量的乘积）中。尝试寻找额外的数据来源或解释

+   尝试集成模型和堆叠模型，这些方法可能提高性能

**请提供您展示的数据的日期！**

图表来源：

[https://www.pinterest.de/minimalcouture/paris-apartments/](https://www.pinterest.de/minimalcouture/paris-apartments/)

[https://www.theodysseyonline.com/the-struggles-of-moving-into-your-first-apartment](https://www.theodysseyonline.com/the-struggles-of-moving-into-your-first-apartment)

[https://www.fashionbeans.com/content/the-worlds-10-smallest-apartments-are-downright-shocking/](https://www.fashionbeans.com/content/the-worlds-10-smallest-apartments-are-downright-shocking/)

**个人简介**：[耶卡特琳娜·科卡修哈](https://www.linkedin.com/in/jekaterina-kokatjuhha/) 是 Zalando 的研究工程师，专注于可扩展的机器学习用于欺诈预测。

[原文](https://medium.freecodecamp.org/how-to-build-a-data-science-project-from-scratch-dc4f096a62a1)。经授权转载。

**资源：**

+   [在线和基于网络的：分析、数据挖掘、数据科学、机器学习教育](https://www.kdnuggets.com/education/online.html)

+   [分析、数据科学、数据挖掘和机器学习的软件](https://www.kdnuggets.com/software/index.html)

**相关资料：**

+   [雇主希望看到的数据科学项目：如何展示业务影响](https://www.kdnuggets.com/2018/12/data-science-projects-business-impact.html)

+   [导致糟糕数据可视化的5个常见错误](https://www.kdnuggets.com/2017/10/5-common-mistakes-bad-data-visualization.html)

+   [采纳高级分析时要避免的10个错误](https://www.kdnuggets.com/2018/07/tdwi-10-mistakes-avoid-advanced-analytics.html)

### 更多相关主题

+   [成为出色数据科学家所需的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [停止学习数据科学以寻找目标，寻找目标以……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [建立一个稳固的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)
