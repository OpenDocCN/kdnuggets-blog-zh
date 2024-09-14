# 近期机器学习和深度学习的20篇重要研究论文

> 原文：[https://www.kdnuggets.com/2017/04/top-20-papers-machine-learning.html](https://www.kdnuggets.com/2017/04/top-20-papers-machine-learning.html)

[comments](#comments)

近年来，机器学习，尤其是其子领域深度学习，取得了许多令人惊叹的进展，重要的研究论文可能会导致被数十亿人使用的技术突破。该领域的研究发展迅速，为帮助读者监控进展，我们提供了自2014年以来发表的最重要的科学论文列表。

我们选择这20篇顶级论文的标准是通过引用次数，从三个学术来源中获取数据：[scholar.google.com](https://scholar.google.com); [academic.microsoft.com](https://academic.microsoft.com/); 和 [semanticscholar.org](https://www.semanticscholar.org)。由于引用次数在各个来源中有所不同且是估算的，我们列出了来自 [academic.microsoft.com](https://academic.microsoft.com/) 的结果，这略低于其他来源。

对于每篇论文，我们还提供了发表年份、由 [semanticscholar.org](https://www.semanticscholar.org) 提供的高度影响引用次数（HIC）和引用速度（CV）指标。HIC 展示了出版物如何相互建立联系，是识别有意义引用的结果。CV 是过去3年每年的加权平均引用次数。对于某些引用，CV 为零，表示 [semanticscholar.org](https://www.semanticscholar.org) 未显示或数据为空。

* * *

## 我们的前3个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的IT需求

* * *

这20篇论文中，大多数（但不是全部），包括前8篇，均涉及深度学习。不过，我们看到很强的多样性——只有一位作者（Yoshua Bengio）有2篇论文，这些论文发表在多个不同的期刊上：CoRR（3），ECCV（3），IEEE CVPR（3），NIPS（2），ACM Comp Surveys，ICML，IEEE PAMI，IEEE TKDE，Information Fusion，Int. J. on Computers & EE，JMLR，KDD，和Neural Networks。前两篇论文的引用次数远高于其他论文。请注意，第二篇论文仅在去年发表。阅读（或重读）它们，了解最新进展。

1.  [![](../Images/93c27ad5d0f40b95dbca6736b82cf9e4.png) **丢弃法：防止神经网络过拟合的简单方法**](http://jmlr.org/papers/volume15/srivastava14a/srivastava14a.pdf)，作者：Hinton, G.E., Krizhevsky, A., Srivastava, N., Sutskever, I., & Salakhutdinov, R.（2014年）。《机器学习研究杂志》，15, 1929-1958。（引用2084次，HIC: 142 , CV: 536）。

    概要：关键思想是在训练过程中随机丢弃神经网络中的单元（连同其连接）。这可以防止单元过度共适应。这样显著减少了过拟合，并在其他正则化方法中取得了显著的改进。

1.  [**深度残差学习用于图像识别**](http://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/He_Deep_Residual_Learning_CVPR_2016_paper.pdf)，作者：He, K., Ren, S., Sun, J., & Zhang, X.（2016年）。CoRR, abs/1512.03385。（引用1436次，HIC: 137 , CV: 582）。

    概要：我们提出了一种残差学习框架，以简化训练远比以前使用的网络更深的深度神经网络。我们明确地将层重新定义为相对于层输入的学习残差函数，而不是学习无参考的函数。我们提供了全面的实证证据，显示这些残差网络更易于优化，并且可以从显著增加的深度中获得更高的准确率。

1.  [**批量归一化：通过减少内部协变量偏移来加速深度网络训练**](http://jmlr.org/proceedings/papers/v37/ioffe15.pdf)，作者：Sergey Ioffe, Christian Szegedy（2015年）ICML。（引用946次，HIC: 56 , CV: 0）。

    概要：训练深度神经网络的复杂性在于每一层输入的分布在训练过程中会随着前一层参数的变化而改变。我们将这种现象称为内部协变量偏移，并通过对层输入进行归一化来解决这个问题。应用于最先进的图像分类模型，批量归一化在训练步骤减少14倍的情况下实现了相同的准确率，并显著优于原始模型。

1.  [**大规模视频分类与卷积神经网络**](http://www.cv-foundation.org/openaccess/content_cvpr_2014/papers/Karpathy_Large-scale_Video_Classification_2014_CVPR_paper.pdf)，作者：Fei-Fei, L., Karpathy, A., Leung, T., Shetty, S., Sukthankar, R., & Toderici, G.（2014年）。IEEE计算机视觉与模式识别会议（引用865次，HIC: 24 , CV: 239）

    概要：卷积神经网络（CNNs）已被确立为解决图像识别问题的强大模型。受到这些结果的鼓舞，我们对CNN在大规模视频分类上的表现进行了广泛的实证评估，使用了一个包含487个类别的100万YouTube视频的新数据集。

1.  [**Microsoft COCO: Common Objects in Context**](https://arxiv.org/pdf/1405.0312.pdf)，由Belongie, S.J., Dollár, P., Hays, J., Lin, T., Maire, M., Perona, P., Ramanan, D., & Zitnick, C.L. (2014). ECCV。（引用830次，HIC: 78，CV: 279）概述：我们展示了一个新的数据集，旨在通过将物体识别的问题放在更广泛的场景理解问题的背景下，从而推动物体识别的最先进技术。我们的数据集包含91种物体类型的照片，这些物体对于4岁的孩子来说是容易识别的。最后，我们提供了使用变形部件模型的边界框和分割检测结果的基准性能分析。

1.  [**Learning deep features for scene recognition using places database**](http://places.csail.mit.edu/places_NIPS14.pdf)，由Lapedriza, À., Oliva, A., Torralba, A., Xiao, J., & Zhou, B. (2014). NIPS。（引用644次，HIC: 65，CV: 0）

    概述：我们引入了一个新的以场景为中心的数据库，名为Places，包含超过700万张标记的场景图片。我们提出了比较图像数据集的密度和多样性的新方法，并展示了Places的密度与其他场景数据集相当，并且具有更多的多样性。

1.  [**Generative adversarial nets**](http://datascienceassn.org/sites/default/files/Generative%20Adversarial%20Nets.pdf)，由Bengio, Y., Courville, A.C., Goodfellow, I.J., Mirza, M., Ozair, S., Pouget-Abadie, J., Warde-Farley, D., & Xu, B. (2014) NIPS。（引用463次，HIC: 55，CV: 0）

    概述：我们提出了一种通过对抗过程估计生成模型的新框架，其中我们同时训练两个模型：一个捕捉数据分布的生成模型G和一个估计样本来自训练数据而非G的概率的判别模型D。

1.  [**High-Speed Tracking with Kernelized Correlation Filters**](http://arxiv.org/pdf/1404.7584)，由Batista, J., Caseiro, R., Henriques, J.F., & Martins, P. (2015). CoRR, abs/1404.7584。（引用439次，HIC: 43，CV: 0）

    概述：在大多数现代跟踪器中，为了应对自然图像变化，通常使用平移和缩放的样本补丁训练分类器。我们提出了一个针对数千个平移补丁数据集的分析模型。通过展示得到的数据矩阵是循环的，我们可以使用离散傅里叶变换对其进行对角化，从而减少存储和计算量。

1.  [**A Review on Multi-Label Learning Algorithms**](http://doi.ieeecomputersociety.org/10.1109/TKDE.2013.39)，由Zhang, M., & Zhou, Z. (2014). IEEE TKDE。（引用436次，HIC: 7，CV: 91）

    概述：本文旨在及时回顾多标签学习研究的问题，其中每个示例由一个实例表示，同时与一组标签相关联。

1.  [**深度神经网络中的特征可转移性如何**](http://papers.nips.cc/paper/5347-how-transferable-are-features-in-deep-neural-networks.pdf)，由 Bengio, Y., Clune, J., Lipson, H., & Yosinski, J. (2014) 编写。CoRR, abs/1411.1792。（引用次数：402，HIC：14，CV：0）

    摘要：我们实验性地量化了深度卷积神经网络中每层神经元的一般性与特异性，并报告了一些令人惊讶的结果。转移性受到两个不同问题的负面影响：（1）高层神经元对其原始任务的专业化，从而牺牲了目标任务的性能，这是预期中的；（2）与将网络分割在共同适应的神经元之间相关的优化困难，这是意料之外的。

1.  **[我们是否需要数百个分类器来解决实际分类问题](http://jmlr.org/papers/volume15/delgado14a/delgado14a.pdf)**，由 Amorim, D.G., Barro, S., Cernadas, E., & Delgado, M.F. (2014) 编写。 《机器学习研究杂志》（引用次数：387，HIC：3，CV：0）

    摘要：我们评估了来自17个家族（判别分析、贝叶斯、神经网络、支持向量机、决策树、基于规则的分类器、提升、袋装、堆叠、随机森林及其他集成方法、广义线性模型、最近邻、偏最小二乘和主成分回归、逻辑回归和多项式回归、多重自适应回归样条和其他方法）的179个分类器。我们使用来自UCI数据库的121个数据集来研究分类器行为，不依赖于数据集的集合。获胜者是用R实现的随机森林（RF）版本（通过caret访问）和用C实现的高斯核SVM（使用LibSVM）。

1.  [**知识宝库：一种大规模概率知识融合的方法**](http://www.cs.cmu.edu/~nlao/publication/2014.kdd.pdf)，由 Dong, X., Gabrilovich, E., Heitz, G., Horn, W., Lao, N., Murphy, K., ... & Zhang, W. (2014年8月) 编写。发表于第20届ACM SIGKDD国际会议《知识发现与数据挖掘》ACM。（引用次数：334，HIC：7，CV：107）。

    摘要：我们介绍了知识宝库，一个Web规模的概率知识库，将从Web内容中提取的数据（通过文本分析、表格数据、页面结构和人工注释获得）与现有知识库中的先验知识结合用于构建知识库。我们采用了监督机器学习方法来融合不同的信息源。知识宝库比任何已发布的结构化知识库都要大，并具有一个概率推理系统，计算事实正确性的校准概率。

1.  [**可扩展的高维数据最近邻算法**](http://ieeexplore.ieee.org/document/6809191/)，由 Lowe, D.G., & Muja, M. (2014) 编写。IEEE Trans. Pattern Anal. Mach. Intell.（引用次数：324，HIC：11，CV：69）。

    摘要：我们提出了新的近似最近邻匹配算法，并与之前的算法进行评估和比较。为了扩展到单台机器内存无法容纳的大型数据集，我们提出了一种分布式最近邻匹配框架，可以与文中描述的任何算法配合使用。

1.  [**极限学习机的趋势：综述**](http://www.ntu.edu.sg/home/egbhuang/pdf/ELM-Suvey-Huang-Gao.pdf)，由 Huang, G., Huang, G., Song, S., & You, K.（2015）。Neural Networks，（被引用 323 次，HIC: 0，CV: 0）

    摘要：我们旨在报告极限学习机（ELM）在理论研究和实际进展中的当前状态。除了分类和回归之外，ELM 最近已扩展用于聚类、特征选择、表示学习及许多其他学习任务。由于其卓越的效率、简单性和令人印象深刻的泛化性能，ELM 已应用于生物医学工程、计算机视觉、系统识别、控制和机器人等多个领域。

1.  [**概念漂移适应的调研**](http://www.win.tue.nl/~mpechen/publications/pubs/Gama_ACMCS_AdaptationCD_accepted.pdf)，由 Bifet, A., Bouchachia, A., Gama, J., Pechenizkiy, M., & Zliobaite, I. 编写。ACM Comput. Surv., 2014，（被引用 314 次，HIC: 4，CV: 23）

    摘要：这项工作旨在全面介绍概念漂移适应，这指的是当输入数据与目标变量之间的关系随时间变化时的在线监督学习场景。

1.  [**深度卷积激活特征的多尺度无序池化**](http://slazebni.cs.illinois.edu/publications/yunchao_eccv14_mopcnn.pdf)，由 Gong, Y., Guo, R., Lazebnik, S., & Wang, L.（2014）。ECCV（被引用 293 次，HIC: 23，CV: 95）

    摘要：为了在不降低其区分能力的情况下提高 CNN 激活的稳定性，本文提出了一种简单而有效的方案——多尺度无序池化（MOP-CNN）。

1.  [**同时检测与分割**](http://arxiv.org/pdf/1407.1808)，由 Arbeláez, P.A., Girshick, R.B., Hariharan, B., & Malik, J.（2014）ECCV，（被引用 286 次，HIC: 23，CV: 94）

    摘要：我们旨在检测图像中所有类别的实例，并为每个实例标记属于它的像素。我们称此任务为同时检测与分割（SDS）。

1.  [**特征选择方法综述**](http://www.serc.org.in/admin/pdffiles/2-vol3ijceit.pdf)，由 Chandrashekar, G., & Sahin, F. 编写。Int. J. on Computers & Electrical Engineering，（被引用 279 次，HIC: 1，CV: 58）

    摘要：由于数据中存在数百个变量，导致数据维度非常高，文献中提供了许多特征选择方法。

1.  [**使用回归树集成进行一毫秒面部对齐**](http://www.cv-foundation.org/openaccess/content_cvpr_2014/papers/Kazemi_One_Millisecond_Face_2014_CVPR_paper.pdf)，由 Kazemi, Vahid 和 Josephine Sullivan 发表，IEEE 计算机视觉与模式识别会议 2014 论文集。（被引用 277 次，HIC: 15，CV: 0）

    摘要：本文讨论了单张图像的面部对齐问题。我们展示了如何利用回归树的集成来直接从稀疏的像素强度子集估计面部标志位置，达到了高质量预测的超实时性能。

1.  [**多分类器系统作为混合系统的综述**](http://romisatriawahono.net/lecture/dm/survey/Wozniak%20-%20Multiple%20Classifier%20Systems%20-%202014.pdf)，由 Corchado, E., Graña, M., 和 Wozniak, M. (2014) 发表。信息融合，16，3-17。（被引用 269 次，HIC: 1，CV: 22）

    摘要：模式分类中的一个当前重点研究方向是多个分类器系统的组合，这些系统可以基于相同或不同的模型和/或数据集构建。

### 更多相关话题

+   [建立一个稳固的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)

+   [使用管道编写干净的 Python 代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [停止学习数据科学以寻找目的，并寻找目的以……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [每个初学者数据科学家应该掌握的 6 种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [一项 90 亿美元的 AI 失败，深入分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)
