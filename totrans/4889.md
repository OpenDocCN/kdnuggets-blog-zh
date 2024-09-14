# 使用 AutoML 生成带 TPOT 的机器学习管道

> 原文：[https://www.kdnuggets.com/2018/01/managing-machine-learning-workflows-scikit-learn-pipelines-part-4.html](https://www.kdnuggets.com/2018/01/managing-machine-learning-workflows-scikit-learn-pipelines-part-4.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

到目前为止，在这一系列文章中，我们已经：

+   [介绍了 Scikit-learn 的管道](/2017/12/managing-machine-learning-workflows-scikit-learn-pipelines-part-1.html)

+   展示了如何将[网格搜索和管道结合起来，成为一个强大的工具](/2018/01/managing-machine-learning-workflows-scikit-learn-pipelines-part-2.html)进行超参数优化

+   演示了[众多模型、管道和网格搜索的组合](/2018/01/managing-machine-learning-workflows-scikit-learn-pipelines-part-3.html)，以及它们的评估和选择

这篇文章将采用一种不同的方法来构建管道。显然，标题揭示了这种不同：我们将不再亲自手动构建管道和进行超参数优化，也不再亲自进行模型选择，而是将自动化这些过程。我们将使用自动化机器学习工具 TPOT 来完成一些繁重的工作。

![头图](../Images/fc517000300a1dc91de487be20edfd12.png)

但首先，[什么是自动化机器学习（AutoML）](https://www.kdnuggets.com/2017/01/current-state-automated-machine-learning.html)？

数据科学家及自动化机器学习的主要倡导者 [Randy Olson](http://www.randalolson.com/) 表示，[有效的机器学习设计要求我们](https://www.kdnuggets.com/2016/05/tpot-python-automating-data-science.html/2):

> +   始终调整我们模型的超参数
> +   
> +   始终尝试许多不同的模型
> +   
> +   始终探索我们数据的多种特征表示

因此，从根本上说，我们可以认为 AutoML 是算法选择、超参数调整、迭代建模和模型评估的任务。

为了了解为什么会这样，可以参考人工智能研究员和斯坦福大学博士生 [S. Zayd Enam](https://twitter.com/zaydenam) 在其精彩博客文章《[为什么机器学习“难”？](http://ai.stanford.edu/~zayd/why-is-machine-learning-hard.html)》中写到的内容：

> 难点在于，机器学习是一个根本上难以调试的问题。机器学习的调试发生在两种情况：1）你的算法不起作用，或者 2）你的算法效果不够好。[...] 极少有算法第一次就能成功，因此这成为了构建算法时花费大部分时间的地方。

基本上，如果一个算法不起作用，或者效果不够好，而选择和优化的过程变得迭代，这就暴露了自动化的机会，因此自动化机器学习应运而生。

我曾[尝试捕捉](https://www.linkedin.com/pulse/case-machine-learning-business-matthew-mayo)AutoML的本质，如下所示：

> 如果，正如[Sebastian Raschka所描述的](https://www.kdnuggets.com/2016/05/explain-machine-learning-software-engineer.html)，计算机编程是关于自动化的，而机器学习是“关于自动化自动化的”，那么自动化机器学习就是“自动化自动化的自动化”。跟随我，在这里：编程通过管理机械任务来减轻我们的负担；机器学习允许计算机学习如何最好地执行这些机械任务；自动化机器学习允许计算机学习如何优化学习如何执行这些机械操作的结果。
> 
> 这是一个非常强大的想法；虽然我们以前必须担心调整参数和超参数，但自动化机器学习系统可以通过多种不同的方法学习调整这些参数以获得最佳结果。

AutoML的基本原理来自这个想法：如果必须构建众多机器学习模型，使用各种算法和不同的超参数配置，那么这个模型构建过程可以自动化，模型性能和准确性的比较也可以自动化。

那么我们实际如何做到这一点？[TPOT](https://github.com/EpistasisLab/tpot)是一个Python工具，它“自动创建和优化机器学习管道，使用遗传编程。” TPOT与Scikit-learn协同工作，自我描述为Scikit-learn的包装器。TPOT是开源的，用Python编写，旨在通过基于[遗传编程](https://en.wikipedia.org/wiki/Genetic_programming)的AutoML方法简化机器学习过程。最终结果是自动化超参数选择、使用各种算法建模以及探索众多特征表示，所有这些都导致了迭代模型构建和模型评估。

你可以在[这里阅读关于使用TPOT的更多信息](https://www.kdnuggets.com/2016/05/tpot-python-automating-data-science.html)，并且可以在[这里阅读与项目（前）首席开发者的访谈](https://www.kdnuggets.com/2016/11/autoamted-machine-learning-interview-randy-olson-tpot.html)。

![TPOT](../Images/49a6192aa0b8ec1192da08cbbdf0e83e.png)

现在回到我们讨论的话题。我们有兴趣使用TPOT为我们生成优化的机器学习管道。你可能会对这样做的简单性感到惊讶。

代码：

请注意，我们几乎需要的一切都可以通过[这一行](http://epistasislab.github.io/tpot/)完成：

```py

tpot = TPOTClassifier(generations=10, verbosity=2)
```

这开始了基于遗传编程的超参数选择、使用各种算法建模以及特征表示探索。结果应该是一个优化的模型。

完成后，TPOT将显示“最佳”模型（基于测试数据准确性）的超参数，并将管道输出为一个可以执行的Python脚本文件，以便于后续使用和进一步调查。

让我们用这个代码运行一下：

```py
  $ python3 tpot_test.py
```

事情是这样的：

```py
Generation 1 - Current best internal CV score: 0.9833333333333332                                                                         
Generation 2 - Current best internal CV score: 0.9833333333333332                                                                         
Generation 3 - Current best internal CV score: 0.9833333333333332                                                                         
Generation 4 - Current best internal CV score: 0.9833333333333332                                                                         
Generation 5 - Current best internal CV score: 0.9833333333333332                                                                         
Generation 6 - Current best internal CV score: 0.9833333333333332                                                                         
Generation 7 - Current best internal CV score: 0.9833333333333332                                                                         
Generation 8 - Current best internal CV score: 0.9833333333333332                                                                         
Generation 9 - Current best internal CV score: 0.9833333333333334                                                                         
Generation 10 - Current best internal CV score: 0.9833333333333334                                                                        

Best pipeline: XGBClassifier(RobustScaler(PolynomialFeatures(XGBClassifier(LogisticRegression(input_matrix, C=10.0, dual=False, penalty=l2), learning_rate=0.001, max_depth=6, min_child_weight=9, n_estimators=100, nthread=1, subsample=0.9000000000000001), degree=2, include_bias=False, interaction_only=False)), learning_rate=0.01, max_depth=4, min_child_weight=13, n_estimators=100, nthread=1, subsample=0.9000000000000001)
TPOT classifier finished in 409.84891986846924 seconds
Best pipeline test accuracy: 1.000
```

就这样。这个过程花费了不到7分钟，结果基于XGBoost的管道能够准确分类100%的测试数据实例。这显然是一个玩具数据集，遗传过程中的交叉验证分数几乎没有改变，但既然我们已经使用鸢尾花数据进行了一系列管道构建（包括使用TPOT），我们绝对准备好向其他数据迈进了。

上述执行生成了这个Python管道脚本并将其保存为` tpot_iris_pipeline.py`：

记住TPOT使用遗传算法进行优化，因此后续执行会产生不同的结果。不要怪我，怪进化。

这个过程可能引发了更多的问题而非答案，特别是如果你对模型优化的自动化方法感到陌生的话。我们将在未来的文章中重新审视这些概念。目前只需记住，AutoML从一个例子来看，可能似乎是一个强大的模型优化工具。

你可能想通过阅读 [自动化机器学习的当前状态](https://www.kdnuggets.com/2017/01/current-state-automated-machine-learning.html) 来跟进，这是一篇我一年前写的文章，但仍然相关。

**相关：**

+   [使用Scikit-learn管道管理机器学习工作流第1部分：温和的介绍](/2017/12/managing-machine-learning-workflows-scikit-learn-pipelines-part-1.html)

+   [使用Scikit-learn管道管理机器学习工作流第2部分：集成网格搜索](/2018/01/managing-machine-learning-workflows-scikit-learn-pipelines-part-2.html)

+   [使用Scikit-learn管道管理机器学习工作流第3部分：多个模型、管道和网格搜索](/2018/01/managing-machine-learning-workflows-scikit-learn-pipelines-part-3.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT工作

* * *

### 更多相关主题

+   [使用TPOT进行机器学习管道优化](https://www.kdnuggets.com/2021/05/machine-learning-pipeline-optimization-tpot.html)

+   [使用ChatGPT生成被动收入的4种方法](https://www.kdnuggets.com/2023/03/4-ways-generate-passive-income-chatgpt.html)

+   [使用Google MusicLM从文本生成音乐](https://www.kdnuggets.com/2023/06/generate-music-text-google-musiclm.html)

+   [使用稳定扩散生成超现实主义面孔的3种方法](https://www.kdnuggets.com/3-ways-to-generate-hyper-realistic-faces-using-stable-diffusion)

+   [如何生成合成的表格数据集](https://www.kdnuggets.com/2022/03/generate-tabular-synthetic-dataset.html)

+   [使用开源工具生成合成时间序列数据](https://www.kdnuggets.com/2022/06/generate-synthetic-timeseries-data-opensource-tools.html)
