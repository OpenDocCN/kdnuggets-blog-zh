# 机器学习中的优化：稳健还是全局最小值？

> 原文：[https://www.kdnuggets.com/2017/06/robust-global-minimum.html](https://www.kdnuggets.com/2017/06/robust-global-minimum.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png)[comments](#comments)

**作者：Nikolaos Vasiloglou**

![Optimization in Machine Learning](../Images/a067afc3bc25ba23a5ea3941a8ccf004.png)

在2014年，Facebook最佳论文奖 @UAI 授予 *“Universal Convexification via Risk-Aversion” b.y* Krishnamurthy Dvijotham; Maryam Fazel; Emanuel Todorov [http://www.auai.org/uai2014/proceedings/individuals/121.pdf](http://www.auai.org/uai2014/proceedings/individuals/121.pdf)。三年后，我们在一次简短的采访中跟进了作者：

**NV:** 首先祝贺你们获奖。我们知道在凸问题中，找到全局最优解要容易得多。

**KD, MF:** 谢谢！我们很感激有机会参与这个讨论。

**1\. NV: 凸化问题是否总是具有与原始问题相同的最小值？**

**KD, MF:** 不，凸化的问题可能具有与原始问题相当不同的最小值。我们论文的动机来源于许多问题（如控制和强化学习）中人们对“稳健”最小值（即当扰动参数时成本不会大幅增加的最小值）的兴趣。我们的方法破坏了非稳健最小值，保留了问题的唯一稳健最小值。

**2\. NV: 我们已经看到很多对分析所有局部最小值等于全局最小值的非凸问题的兴趣。例如A. Anandkumar的团队一直在研究如何保证非凸问题中的最小值。你的工作与这个方向相比如何？**

**KD:** 是的，这些结果非常有趣，并且与我们的工作有潜在的联系。特别是，论文 [https://arxiv.org/abs/1503.03712](https://arxiv.org/abs/1503.03712) 和 [https://arxiv.org/pdf/1610.09322.pdf](https://arxiv.org/pdf/1610.09322.pdf) 研究了平滑非凸函数以消除局部最小值的方法，但采用了逐渐的方法（从大量平滑开始，并逐渐减少，同时跟踪全局最小值）。我们的方法是一次性完成的（添加大量噪声/风险规避）以直接凸化问题，但通过逐渐的方式可能可以改进（在允许较弱假设的意义上，保证一个稳健最小值的位置）。我们期待进一步探索这些联系。

**MF:** 只是补充一下，我们的工作（以及KD上面提到的论文）并不专注于所有局部最小值等同于全局最小值的限制情况。

**3\. NV:** **最大的难题是深度学习。你的论文中有一个MNIST的例子。你们尝试过其他案例吗？**

**KD：** 我们在 MNIST 上的结果相当初步，确实需要更仔细的调查，并与最新的深度学习算法进行比较。不幸的是，我们没有机会进行更详细的实验——在我们撰写这篇论文后不久，我就离开去做博士后，并将重点转向其他主题。我们计划在接下来的几个月里重新研究这个问题，并撰写这篇论文的扩展版本。然而，文献中出现了几篇其他作者的论文，探讨了向神经网络中添加噪声以改善优化和泛化的类似想法（例如，[https://openreview.net/forum?id=B1YfAfcgl](https://openreview.net/forum?id=B1YfAfcgl)）。

**4. NV：深度学习爱好者说 SGD 大多数时候都能工作，并且经过几次重启你就能训练好网络。你对此有何评论？你认为你的方法为何仍然更好？**

**KD：** 我们论文的贡献是一个具体的方法，它精确量化了需要多少噪声/风险厌恶才能使问题凸化。我们不期望这种方法直接与 SGD 竞争，至少对于标准的监督学习问题是这样，尽管它可能会提供一些关于添加多少噪声的见解，以便训练对局部极小值的敏感性更低（**MF：** 最近，噪声梯度下降和 Langevin 动力学因类似目标而受到关注）。

然而，我们开发这个方法的主要动机是在 RL 和控制问题中，其中鲁棒性本身是一种理想的特质（例如，它是一种确保机器人系统在扰动和干扰下不会违反安全约束的方法）。我们的直觉是，找到鲁棒的极小值实际上更容易，论文中提出的理论对此声明提供了一些严格的理由。

**6. NV：你的论文在社区中的反响如何？有没有后续工作？你的方法似乎很直接实现。你认为 TensorFlow 实现（或其他流行的包）会更有助于人们采纳它吗？**

**KD:** 我们收到了来自最近应用类似想法于计算机视觉的论文的一些后续跟进 ([https://link.springer.com/chapter/10.1007/978-3-319-14612-6_4](https://link.springer.com/chapter/10.1007/978-3-319-14612-6_4)) 和一般非凸优化的论文 ([https://arxiv.org/abs/1602.02191](https://arxiv.org/abs/1602.02191))。然而，由于我们没有继续进行这项工作，这篇论文没有得到广泛宣传。我们希望尽快回到这项工作中，改进算法以使其实际可用，并发布一个开源实现。论文中的基本方法是直接实现的。然而，为了在实践中取得良好的性能，我们认为这种方法需要与近年来从事SGD研究的学者们开发的方差减少思想相结合。正如我在前面的问题中所说，我们的观点是这种方法最适用于那些对鲁棒性有直接兴趣的问题，而不是标准的监督学习问题。我们希望在接下来的几个月中，开发一个适用于RL问题的实现，使用OpenAIgym环境 ([https://gym.openai.com/](https://gym.openai.com/)) 并公开发布。

**7\. NV:** 讲述一下在实际中找到全局最小值的其他成功方法。

**KD:** 非凸问题的全局优化通常具有挑战性和困难。在优化社区中，已经开发出几种依赖于分支定界的方法，即构建和改进凸松弛，并结合某种搜索策略（例如 BARON: [http://archimedes.cheme.cmu.edu/?q=baron](http://archimedes.cheme.cmu.edu/?q=baron)）。对于没有额外结构的问题，这些方法是最佳的。然而，这些算法在任何特定问题上的性能可能会有很大的变化（在最坏的情况下，它们可能类似于暴力搜索）。对于多项式优化问题，Lasserre层次结构 ([http://homepages.laas.fr/lasserre/book-flyer.pdf](http://homepages.laas.fr/lasserre/book-flyer.pdf)) 允许通过逐渐增大的半正定程序的层次来计算全局最优解。理论上，需要解决非常大的半正定程序以找到全局最优解，但在实践中，对于一些多项式优化问题，适当大小的半正定程序足以计算全局最优解。

**8\. NV:** 对于非凸问题，寻找凸包是否值得？与凸包相比，你的算法会慢多少？

**KD:** 这是一个很好的问题。构造一般非凸函数的凸包问题仍然是NP难的，除非在特殊情况下，否则很难解决。在那些构造凸包比较容易的特殊情况下，如果你有兴趣找到原始目标函数的最小值，可以最小化这个凸包。然而，正如之前所述，我们的方法的重点是找到一个鲁棒的最小值，而不是全局最小值，因此凸包是否对这个目标有用并不明显。也可能通过另一种称为莫罗包络的包络来解释我们的方法；我们目前正在研究这些联系。

**9\. NV: 在你的实验中，你更强调泛化误差而不是全局最小值的接近程度。这是为什么？你的方法实际上解决了这两个问题吗？**

**KD:** 我们的方法可能会产生一个远离全局最小值的最优解，因为它试图找到一个鲁棒的最小值。我们在实验中测试的假设是，也许鲁棒最小值在泛化误差方面优于全局最小值——期望这样是因为在实验中，我们对SVM（w’x+b）和神经网络的中间神经元的预测添加了噪声。这个想法是，添加这种噪声并尝试最小化噪声上的期望学习损失，试图确保算法的预测不仅对给定输入保持稳定，还对其周围的一系列噪声扰动保持稳定。当然，在我们的论文中，这一点并没有被严格证明，但我们进行的初步实验表明确实如此。我们正在研究将这一点形式化的方法；已知学习算法对扰动的稳定性与泛化能力有关 [http://www.jmlr.org/papers/volume2/bousquet02a/bousquet02a.pdf](http://www.jmlr.org/papers/volume2/bousquet02a/bousquet02a.pdf)。

**10\. NV: 你的方法对输入添加噪声并在期望上进行优化吗？这与贝叶斯方法有联系吗？与加速的蒙特卡洛变体有任何联系吗？另外，你的方法是否提供了学习模型的分布？**

**KD:** 由于我们的方法中注入噪声是有意为之，并非像贝叶斯方法中的概率模型那样产生的，因此没有直接的联系。此外，最后我们没有得到模型参数的后验分布，而只是一个点估计。可能有方法利用蒙特卡洛算法来获得更好的随机梯度样本估计，但我们还没有研究过。我们的方法仅产生模型参数的单一估计，而不是整个分布。

**11\. NV: 你的方法与dropout（在提高泛化能力方面）有任何联系吗？**

**KD,MF:** 我们不知道任何直接的联系。

**12\. NV: ICLR 2017 最佳论文 “**[理解深度学习需要重新思考泛化](https://openreview.net/pdf?id=Sy8gdB9xx)**” 以如下话语结束： “*从我们的实验中得到的另一个见解是，即使得到的模型不能泛化，优化仍然在经验上是容易的。这表明，优化在经验上容易的原因必须与泛化的真实原因不同*”。你似乎同时处理了这两个（优化和泛化）。你的方法是否与这篇论文的发现相矛盾？**

**KD：** 我尚未仔细研究这篇论文，但它确实有一个有趣的观察。正如我之前提到的，我们的方法确实表明，找到稳健的最小值比找到全局最小值更容易，并且有一些证据（来自我们的实验和其他学习理论论文）表明稳健性和泛化是相关的。然而，我们的实验相当初步，并使用了非常小的数据集和神经网络。最近的深度学习工作显示，在大数据和大规模神经网络领域，优化问题（局部最小值等）往往会消失——因此，最近的 ICLR 论文所研究的情况可能实际上呈现了一个不同的状态，其中优化和泛化具有不同的定性特征，而不是少量模型参数和小数据集的设置。我不会说这两项研究相互矛盾，但新的 ICLR 工作令人着迷，因为它违背了关于学习和泛化的传统智慧。

[原文](https://www.linkedin.com/pulse/robust-global-minimum-nikolaos-vasiloglou)。已获得许可转载。

**个人简介：**[Nikolaos Vasiloglou](https://www.linkedin.com/in/vasiloglou/) 是 UAI 2017 的组织委员会成员，并在构建/开发分布式机器学习系统方面有多年经验。

**相关：**

+   [介绍 Dask-SearchCV：使用 Scikit-Learn 进行分布式超参数优化](/2017/05/dask-searchcv-distributed-hyperparameter-optimization-scikit-learn.html)

+   [通过梯度下降学习如何学习](/2017/02/learning-learn-gradient-descent.html)

+   [5 个你不容忽视的机器学习项目，1 月](/2017/01/five-machine-learning-projects-cant-overlook-january.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

### 更多相关话题

+   [数据科学基础：开始时需要掌握的10项关键技能](https://www.kdnuggets.com/2020/10/data-science-minimum-10-essential-skills.html)

+   [使用TPOT进行机器学习管道优化](https://www.kdnuggets.com/2021/05/machine-learning-pipeline-optimization-tpot.html)

+   [想用你的数据技能解决全球问题？这是你需要了解的](https://www.kdnuggets.com/2022/04/jhu-want-data-skills-solve-global-problems.html)

+   [SQL查询优化技术](https://www.kdnuggets.com/2023/03/sql-query-optimization-techniques.html)

+   [数据库优化：探索SQL中的索引](https://www.kdnuggets.com/2023/07/database-optimization-exploring-indexes-sql.html)

+   [梯度下降：山地旅行者的优化指南](https://www.kdnuggets.com/gradient-descent-the-mountain-trekker-guide-to-optimization-with-mathematics)
