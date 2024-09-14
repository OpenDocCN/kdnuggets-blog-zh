# 仅用两行代码实现强大的探索性数据分析

> 原文：[https://www.kdnuggets.com/2021/02/powerful-exploratory-data-analysis-sweetviz.html](https://www.kdnuggets.com/2021/02/powerful-exploratory-data-analysis-sweetviz.html)

[评论](#comments)

**由[Francois Bertrand](https://medium.com/@fbertrand27)提供，数据可视化和游戏的编码员及设计师**。

![](../Images/951735d5191cb7ce84ba5b2d91af81a3.png)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在IT方面

* * *

探索性数据分析（EDA）是大多数数据科学项目中一个至关重要的早期步骤，它通常包括采取相同的步骤来描述数据集（例如，查找数据类型、缺失信息、值的分布、相关性等）。鉴于这些任务的重复性和相似性，有一些库可以自动化这些任务，并帮助启动过程。

最新的一个工具是一个名为Sweetviz的开源Python库（[GitHub](https://github.com/fbdesignpro/sweetviz)），由几位贡献者和我自己共同创建，正是为了这个目的。它接受pandas数据框，并生成一个自包含的HTML报告。

**它功能强大：** 除了只需两行代码即可创建富有洞察力和美观的可视化外，它还提供了手动生成所需大量时间的分析，包括一些其他库无法如此快速提供的分析，如：

+   **两个数据集的比较**（例如，训练集与测试集）

+   **目标值与所有其他变量的可视化**（例如，“男性与女性的生存率是多少”）

[这是一个由Sweetviz生成的关于著名泰坦尼克号幸存者数据集的报告链接](http://cooltiming.com/SWEETVIZ_REPORT.html)。我们将在本文中分析这个报告。

### EDA变得…有趣？！

能够快速获得如此多关于目标值的信息，并几乎瞬间比较数据集的不同区域，将这一步骤从完全枯燥的工作转变为更快、更有趣，甚至在某种程度上…有趣！（至少对于这个数据爱好者来说！）当然，EDA是一个更长的过程，但至少第一步要顺利得多。让我们看看在著名样本数据集上的效果。

### 分析泰坦尼克号数据集

对于本文，我们将分析你可以在[这里](https://drive.google.com/file/d/11MgXvwics58UwiMjUSWJDAjn6hiflC-W/view?usp=sharing)找到的泰坦尼克号幸存者样本数据集。

安装 Sweetviz 后（使用 *pip install sweetviz*），只需像平常一样加载 pandas 数据框，然后根据需要调用 *analyze()*、*compare()* 或 *compare_intra()*（更多细节见下文）。完整的文档可以在 [GitHub](https://github.com/fbdesignpro/sweetviz) 上找到。现在，让我们从当前的案例开始，加载如下：

```py
import sweetviz
import pandas as pd
train = pd.read_csv("train.csv")
test = pd.read_csv("test.csv")

```

我们现在有两个数据框（训练集和测试集），我们希望分析目标值“Survived”。我想指出的是，在这种情况下，我们事先知道目标列的名称，但指定目标列始终是可选的。我们可以用以下代码生成报告：

```py
my_report = sweetviz.compare([train, "Train"], [test, "Test"], "Survived")

```

运行此命令将执行分析并创建报告对象。要获取输出，只需使用 show_html() 命令：

```py
my_report.show_html("Report.html") # Not providing a filename will default to SWEETVIZ_REPORT.html

```

生成文件后，它将通过默认浏览器打开，应该如下所示：

![](../Images/c3b2b67f43dfab4520eae33fd67c3d5a.png)

有很多内容需要解读，所以我们一步步来吧！

**摘要展示**

![](../Images/25ec3c8d030b6d65fe7afc27fb1fe0de.png)

摘要展示了两个数据框的特征。我们可以立即识别出测试集的大小大约是训练集的一半，但包含相同的特征。底部的图例显示，训练集包含“Survived”目标变量，但测试集不包含。

请注意，Sweetviz 会对每列的数据类型进行最佳猜测，包括数值型、类别/布尔型和文本型。这些可以被覆盖——更多细节见下文。

**关联**

将鼠标悬停在摘要中的“关联”按钮上，会使右侧出现关联图：

![](../Images/4cb0f56bdbb1e6c9d24114bae319c7d3.png)

该图表是 [Drazen Zaric: Better Heatmaps and Correlation Matrix Plots in Python](https://towardsdatascience.com/better-heatmaps-and-correlation-matrix-plots-in-python-41445d0f2bec) 和 [Shaked Zychlinski: The Search for Categorical Correlation](https://towardsdatascience.com/the-search-for-categorical-correlation-a1cf7f1888c9) 的视觉效果合成。

基本上，除了展示传统的数值相关性外，它还将数值相关性、类别-类别的不确定系数以及类别-数值的相关比率统一在一个图表中。方框代表类别特征相关变量，圆圈代表数值-数值相关性。为了清晰起见，简单对角线被留空。

> *重要提示：由不确定系数提供的类别-类别关联是**不对称的**，意味着每一行表示行标题（在左侧）对每一列提供了多少信息。例如，“性别”、“客舱等级”和“票价”是对“生存”提供最多信息的元素。对于泰坦尼克号数据集，这些信息是相当对称的，但情况并不总是如此。*

最后，值得注意的是这些相关性/关联方法不应被视为绝对真理，因为它们对数据的基本分布和关系做出了一些假设。然而，它们可以作为一个非常有用的起点。

**目标变量**

![](../Images/197556be6ef12fbe03dafb26989ffbde.png)

当指定一个目标变量时，它会首先显示在一个特殊的黑色框中。

> *重要提示：目前只有数值和布尔特征可以作为目标变量。*

从这个摘要中我们可以得知，“生存”在训练集中没有缺失数据（891，100%），有2个不同的可能值（占所有值的不到1%），从图表中可以估算出大约60%未能生存。

**详细区域（类别/布尔型）**

当你将鼠标悬停在任何变量上时，右侧区域将展示详细信息。详细信息的内容取决于正在分析的变量类型。在类别（或布尔型）变量的情况下，如目标变量，分析如下：

![](../Images/6deb752bf272697faac3a6eda2cb6068.png)

> *重要提示：目前需要一台“宽屏”显示器才能查看完整的详细区域。*

在这里，我们可以看到每个类别的具体统计数据，其中62%未能生存，38%存活。你还可以看到其他特征的关联详细信息。

**数值数据**

![](../Images/3c529f608464ca911c5a7096bfb1ad92.png)

数值数据在其摘要中展示了更多的信息。在这里，我们可以看到，在这种情况下，大约有20%的数据缺失（测试数据中为21%，非常一致）。

**注意，目标值（在此情况下为“生存”）被绘制为分布图上方的一条线。这可以实现对目标分布相对于其他变量的即时分析。**

有趣的是，从右侧的图表中我们可以看到，生存率在所有年龄段中都相当一致，除了最年轻的年龄段，其生存率较高。这看起来像是“妇女和儿童优先”并非仅仅是空话。

**详细区域（数值）**

![](../Images/f09f1294ae56786f549674c0340163ee.png)

与类别数据类型一样，数值数据类型在其详细区域中显示了一些额外的信息。这里值得注意的是图表上方的按钮。这些按钮会改变图表中显示的“箱”的数量。你可以选择以下选项：

+   自动

+   5

+   15

+   30

**注意：要访问这些按钮，你需要通过点击当前特征来“锁定”它。该特征会有一个红色轮廓，显示它已被锁定，你可以访问详细区域。**

例如，选择“30”会生成一个更详细的图表：

![](../Images/ca321d42041614e31ff8b1188a3b9a3e.png)

**文本数据**

![](../Images/ffacbef9dad440a7af0b9ffcb459f47d.png)

目前，系统未考虑为数值型或类别型的数据将被视为“文本”。文本特征目前只显示计数（百分比）作为统计信息。

### FeatureConfig：强制数据类型，跳过列

在许多情况下，有些“标签”列你可能不想分析（尽管目标分析可以提供基于标签的目标值分布的见解）。在其他情况下，你可能希望强制某些值标记为类别型，即使它们在本质上是数值型的。

要完成所有这些操作，只需创建一个FeatureConfig对象并将其传递给分析/比较函数。你可以在kwargs中指定一个字符串或一个列表，用于*skip*，*force_cat*，和*force_text*：

```py
feature_config = sweetviz.FeatureConfig(skip="PassengerId", force_cat=["Ticket"])
my_report = sweetviz.compare([train, "Train"], [test, "Test"], "Survived", feature_config)

```

### 比较子群体（例如，男性与女性）

即使你只查看单一数据集，研究该数据集中不同子群体的特征也是非常有用的。为此，Sweetviz提供了*compare_intra()*函数。要使用它，你需要提供一个布尔测试来拆分数据集（在这里我们尝试*train["Sex"] == 'male'*，以了解不同性别群体的情况），并为每个子群体指定一个名称。例如：

```py
my_report = sweetviz.compare_intra(train, train["Sex"] == 'male', ["Male", "Female"], 'Survived')
my_report.show_html() # Not providing a filename will default to SWEETVIZ_REPORT.html

```

生成以下分析：（在此截图中，我使用了feature_config来跳过“性别”特征的分析，因为它是多余的）

![](../Images/c3b2b67f43dfab4520eae33fd67c3d5a.png)

注意，目标值（在这种情况下为“生存”）现在被绘制为单独的线条，每个比较的数据集一个（例如，男性用蓝色，女性用橙色）。

### 综合来看

EDA（探索性数据分析）是一个流动的、艺术性的过程，必须根据每个数据集和情况进行独特的调整。然而，像Sweetviz这样的工具可以帮助启动这一过程，并消除许多初步的数据特征工作，从而立即提供见解。让我们一起查看Titanic数据集的所有特征，看看这可能是什么样的。

**单独字段**

***乘客ID***

![](../Images/e6523e600c6d28c1cd226a19fdc87eba.png)

+   ID和生存率的分布均匀且有序，如你所希望/预期的那样，因此没有意外情况。

+   无缺失数据

***性别***

![](../Images/7e5c0bea0910df847577bd87210732f7.png)

+   男性的数量大约是女性的两倍，但……

+   女性的生存概率远高于男性

+   从相关性来看，性别与票价相关，这一点既在意料之中，又在意料之外……

+   训练集和测试集之间的分布相似

+   无缺失数据

***年龄***

![](../Images/cece85536949279157680aa39f468b24.png)

+   20%缺失数据，训练集和测试集之间的缺失数据和分布一致

+   以年轻人为主的人口，但0–70岁的年龄段都有很好的代表性

+   生存率分布相当均匀，除了最年轻年龄段有一个峰值

+   使用详细窗口中的30个区间直方图，你可以看到这个生存率峰值确实是针对最年轻的（大约<=5岁），因为在大约10岁时，生存率非常低。

+   年龄似乎与兄弟姐妹数量、票舱等级和票价相关，令人惊讶的是与登船港口也有些关联

***Name***

![](../Images/0005ddada7edbc6551531b57b7dcda22.png)

+   没有缺失数据，数据看起来很干净

+   所有名字都是独特的，这并不意外

***Pclass***

![](../Images/ed63c35e570d7099e6ec349f293e1b0f.png)

+   生存率与舱位等级紧密相关（头等舱最可能生存，三等舱最不可能生存）

+   训练集和测试集之间的分布类似

+   没有缺失数据

***SibSp***

![](../Images/b501d944f082397b63aec94ee5144c22.png)

+   似乎在1号和2号船舱有生存率的峰值，但（查看未显示的详细面板）在3号及以上的船舱有明显的下降。大家庭可能无法生存或者可能更贫困？

+   训练集和测试集之间的分布类似

+   没有缺失数据

***Parch***

![](../Images/2eb8e569651180083dc6c394aaf46cc6.png)

+   训练集和测试集之间的分布类似

+   没有缺失数据

***Ticket***

+   ~80%的值是独特的，所以平均每5张票中大约有1张是共享的

+   最高频率的票是7，这通常与最大兄弟姐妹数量（8）一致

+   没有缺失数据，数据看起来很干净

***Fare***

![](../Images/b08cd110c82e7784fe3019c40ef7b071.png)

+   正如预期的那样，与Pclass一样，较高的票价生存情况更好（尽管在更高水平上样本量会变得相当薄）

+   “生存”变量的相关系数为0.26，相对较高，因此可能支持这一理论

+   大约30%的独特值感觉有点高，因为你会期望更少的固定价格，但似乎有很多细节，因此这没问题

+   测试集中只有1条缺失记录，数据在训练集和测试集之间比较一致

***Cabin***

![](../Images/052b88ebf3919e9bba86cdea7f86627a.png)

+   大量缺失数据（高达78%），但训练集和测试集之间的一致性很好

+   最高频率是4，这与每个舱位最多4人是合理的

***Embarked***

![](../Images/545d6329b4c4affbc74987888058f77f.png)

+   3个独特的值（S、C、Q）

+   训练数据中只有2行缺失。训练集和测试集之间的数据看起来很一致

+   C类的生存率稍高；这可能是一个富人居住的地区吗？

+   无论如何，“登船港口”对“生存”的不确定系数仅为0.03，因此可能不是很重要

### 一般分析

+   总体而言，大部分数据是存在的，并且看起来一致且合理；没有重大异常值或巨大惊喜

**测试数据与训练数据**

+   测试集的行数大约少了50%

+   训练集和测试集在缺失数据的分布上非常接近

+   训练集和测试集的数据值在各个方面都非常一致

**关联/相关分析**

+   性别、票价和Pclass提供了最多的生还信息。

+   正如预期的那样，票价和Pclass高度相关。

+   年龄似乎能告诉我们关于Pclass、兄弟姐妹以及某种程度上的Fare的信息，这在一定程度上是可以预期的。它对“Embarked”也有很大提示，这有点令人惊讶。

**缺失数据**

+   除了年龄（约20%）和舱位（约77%）的显著缺失数据外，没有其他显著的缺失数据（其他特征中偶尔也有一些缺失）。

### 结论

这些信息都来自两行代码！Sweetviz现在还支持笔记本集成（你可以插入内联报告）和新功能！

使用Sweetviz，当我开始查看新的数据集时，它能迅速帮助我起步。值得指出的是，我还发现它在后续分析过程中非常有用，例如，在特征生成期间，快速概览新特征的表现。我希望你也会发现它在自己的数据分析中是一种有用的工具。

[原文](https://towardsdatascience.com/powerful-eda-exploratory-data-analysis-in-just-two-lines-of-code-using-sweetviz-6c943d32f34)。转载已获许可。

**相关：**

+   [使用一行代码进行统计和视觉探索性数据分析](https://www.kdnuggets.com/2020/09/statistical-visual-exploratory-data-analysis-one-line-code.html)

+   [使用管道进行更干净的数据分析](https://www.kdnuggets.com/2021/01/cleaner-data-analysis-pandas-pipes.html)

+   [强效的探索性数据分析](https://www.kdnuggets.com/2020/07/exploratory-data-analysis-steroids.html)

### 更多相关内容

+   [成为优秀数据科学家所需的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应该掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [使用管道编写干净的Python代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [不到15行代码的多模态深度学习](https://www.kdnuggets.com/2023/01/predibase-multi-modal-deep-learning-less-15-lines-code.html)

+   [停止学习数据科学以找到目的，并为…寻找目的](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)
