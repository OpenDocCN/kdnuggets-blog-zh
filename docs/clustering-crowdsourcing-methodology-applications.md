# 众包中的聚类：方法论与应用

> 原文：[https://www.kdnuggets.com/2021/11/clustering-crowdsourcing-methodology-applications.html](https://www.kdnuggets.com/2021/11/clustering-crowdsourcing-methodology-applications.html)

[评论](#comments)

**作者：[Daniil Likhobaba](https://www.linkedin.com/in/daniil-likhobaba-4a33201bb/)，Toloka 的分析师**。

聚类是将一组对象分组的任务，使得同一组（称为簇）中的对象彼此之间比与其他组中的对象更相似。在大多数情况下，聚类需要知道对象之间的距离，这通常以距离矩阵的形式呈现。然而，距离的函数通常是未知的，或者聚类规则无法定义。因此，描述标记指令中的过程并利用众包来进行数据聚类更为合理。遗憾的是，目前没有现成的管道用于此目的，而大多数相关论文都限于理论。我们希望能找到一种更实际的解决方案。这就是我们最终得到的结果。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速通道进入网络安全职业

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 部门

* * *

### 配对对象标记

起初，我们重现了 [Larsen 等人（WWW'20）](https://dl.acm.org/doi/10.1145/3366423.3380045) 的一个简单算法，其中所有标记的数据被分为两个簇。为此，我们需要对这两个对象子集进行两次比较，然后使用专门的算法汇总数据。尽管如此，两个簇任务相当少见。此外，进行包含所有对象的配对子集比较是非常昂贵的。最后，该算法提供了一个单一集合及其补集，从技术上讲，这不算真正的聚类。但我们必须从某处开始。

数据来自 [Leipzig 语料库集合](https://wortschatz.uni-leipzig.de/en/download) – 各 360 个英语和德语单词。

![](../Images/e6510c40af2664a3b48d3545f4b708b6.png)

众包执行者必须指示显示的两个词是否属于同一种语言。结果令人惊讶地好：每种语言组中 98% 的词汇被正确识别。这是一个令人鼓舞的第一步。但由于我们希望找到更高效的解决方案，我们继续寻找可行的替代方案。

![](../Images/f94d8e1b7d0e1410110dd96f29457740.png)

### 多次比较

在[Gomes et al. 论文](https://proceedings.neurips.cc/paper/2011/hash/c86a7ee3d8ef0b551ed58e354a836f2b-Abstract.html)[NIPS'11](https://proceedings.neurips.cc/paper/2011/hash/c86a7ee3d8ef0b551ed58e354a836f2b-Abstract.html)[)](https://proceedings.neurips.cc/paper/2011/hash/c86a7ee3d8ef0b551ed58e354a836f2b-Abstract.html)中，作者建议了一种可以处理任意数量图像聚类的方法。其工作原理如下：我们从一个包含图像和颜色调色板的页面开始。执行者选择一种颜色并用它标记相似的图像。然后，执行者选择另一种颜色，根据另一个共享特征将另一组图像分组。如此重复，直到所有图像都被上色标记。

任务是随机创建的。主要参数是每张图像显示的平均页面数量。这个数字必须为每种情况选择，或者根据数据集大小来定义。标记数据与文章中提供的概率模型进行汇总，然后算法随后生成聚类。值得注意的是，这种方法不需要已知的对象之间的距离。

### 按风格对鞋子进行聚类

为了测试上述方法，我们从[SIGMOD/](https://dl.acm.org/doi/abs/10.1145/3318464.3383127)[PODS'20](https://dl.acm.org/doi/abs/10.1145/3318464.3383127) [Toloka教程](https://dl.acm.org/doi/abs/10.1145/3318464.3383127)中获取了一个包含100张穿着衣物的图像的数据集。为了跳过机器学习已经训练过的内容，我们要求众包参与者根据风格对物体进行分组，而不考虑颜色。这样做的逻辑是，大多数人能够区分不同的时尚风格，即使我们无法总是用语言描述任何特定风格的定义特征。指令包含了一些特征和细节，帮助参与者，例如高跟鞋。

![](../Images/58e03041140b4be3bbdfc147557b149d.png)

最终，我们得到了七个易于识别的鞋子聚类。其中包括男鞋、女靴、运动鞋和其他四种。我们很高兴看到女鞋和女凉鞋形成了两个独立的聚类。每个聚类的样本图像如下所示。

![](../Images/009ba0afcdc6cdb6ea9fed6ca25fe407.png)

可以浏览所有子集，亲眼看到聚类的整齐程度，即使是肉眼也能观察到。这种方法对我们证明了有效，但我们决定进一步探讨。

### 按风格对鞋子进行聚类

现在，我们想在更大数据集上测试这种方法。我们从[Feidegger](https://github.com/zalandoresearch/feidegger)数据集中取了2,000张连衣裙的图片，并再次决定按风格对它们进行聚类。然而，与鞋子不同，连衣裙通常包含更多特征，这就是为什么我们需要为完成这项任务的群众表演者制定一套准确但简洁的指示。

主要困难在于我们无法向表演者描述所有可能性和特殊情况，因为我们实际上不知道这个数据集的结构。我们向[Crowd Solutions Architect (CSA)](https://www.kdnuggets.com/2021/06/data-careers-crowd-solutions-architect.html)团队的[Toloka](https://toloka.ai/)同事寻求帮助。他们帮助我们配置了质量控制，创建了一个带有培训计划的实践池，并撰写了适当的指示。结果，任务对表演者变得明确，这反过来提高了标注质量。

这些指示没有包含任何最坏情况或所有分组标准。我们建议表演者专注于确定连衣裙的风格、细节层次（例如，领口的大小但不是其形状），并识别具有总体相似或不同外观的连衣裙的图片。我们想解释可以使用什么逻辑来比较连衣裙，概述基本的聚类方法，但不列出任何具体特征供表演者在实际标注过程中记忆和依赖。

培训过程中难度逐步增加，从两个连衣裙的比较开始，最终增加到八件连衣裙。关键规则在通往最终考试的过程中进行了讲解，最终考试由多张具有明显差异的图片组成。

![](../Images/aa05cc1cd1bc5e514569d402229fac22.png)

![](../Images/6da7585319f8b28ab184124cd3e6d2c1.png)

之后，俄语被添加为界面语言，并为随后的任务设置了每页最多八张图片。这是为了简化任务，将每页的平均处理时间缩短到40秒。

![](../Images/4f8de1915036abe7c4a09cd5f131c176.png)

在任务过程中，有一个方面特别引人关注——负责预期图像数量的页面参数。该参数与数据集大小的关系被确定，这告诉我们成功聚合所需的对象比较数量。我们最终得到了12个易于解释的簇：其中包括蓬松的无袖连衣裙、长袖连衣裙、针织连衣裙等等。下图展示了我们获得的每个簇中的一些对象。

![](../Images/eae10f32dda7c9019b69622dee65b6bf.png)

### 收获

通过这些努力，我们确认了通过众包进行聚类确实是可能的，并且效果非常好。我们获得的质量达到了预期，并且我们制定了一个可复制的流程。此外，我们找到了一种让说明对参与者更清晰的方法，并为任务创建了合适的界面。关键是，这一切都在没有我们最初采用的配对比较的情况下完成。标记时间不长，成本也比预期的便宜。

我们目前正在通过[Chang等（NIPS '11）](https://proceedings.neurips.cc/paper/2009/file/f92586a25bb3145facd64ab20fd554ff-Paper.pdf)的众包评价方法评估结果标记的质量。他们建议使用入侵者，即在聚类中添加一个故意不正确的对象，并要求参与者找到它。参与者越频繁地选择这个明显错误的对象而不是聚类中的其他对象，我们认为质量越高。

我们认为，使用众包进行聚类可能在电子商务、搜索结果排名以及在任何领域调查未知数据集结构时会很有用。

**相关：**

+   [什么是聚类以及它是如何工作的？](https://www.kdnuggets.com/2021/10/clustering-what-is-how-works.html)

+   [k-means聚类的质心初始化方法](https://www.kdnuggets.com/2020/06/centroid-initialization-k-means-clustering.html)

+   [开放创新和机器学习中的众包——从数据中获取高价值](https://www.kdnuggets.com/2017/06/open-innovation-crowdsourcing-machine-learning.html)

### 更多相关话题

+   [聚类解密：理解K均值聚类](https://www.kdnuggets.com/2023/07/clustering-unleashed-understanding-kmeans-clustering.html)

+   [学术界是否过于关注方法论而忽视了真正的见解？](https://www.kdnuggets.com/is-academia-obsessing-over-methodology-at-the-cost-of-true-insights)

+   [K均值聚类是什么，及其算法如何工作？](https://www.kdnuggets.com/2023/05/kmeans-clustering-algorithm-work.html)

+   [使用scikit-learn进行聚类：无监督学习教程](https://www.kdnuggets.com/2023/05/clustering-scikitlearn-tutorial-unsupervised-learning.html)

+   [动手实践无监督学习：K均值聚类](https://www.kdnuggets.com/handson-with-unsupervised-learning-kmeans-clustering)

+   [揭示隐藏模式：层次聚类入门](https://www.kdnuggets.com/unveiling-hidden-patterns-an-introduction-to-hierarchical-clustering)
