# 神经网络内部与 Tensorflow 中的交互式代码

> 原文：[https://www.kdnuggets.com/2018/06/inside-mind-neural-network-interactive-code-tensorflow.html/2](https://www.kdnuggets.com/2018/06/inside-mind-neural-network-interactive-code-tensorflow.html/2)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](/2018/06/inside-mind-neural-network-interactive-code-tensorflow.html?page=2#comments)

### **内部梯度 / 正面和负面属性**

![](../Images/df9c2757200f1a14d3619735f68800dd.png)

alpha 值为 1.0

现在让我们使用 [内部梯度](https://towardsdatascience.com/google-iclr-2017-paper-summary-gradients-of-counterfactuals-6306510935f2)，alpha 值为 ….

0.01, 0.01, 0.03, 0.04, 0.1, 0.5, 0.6, 0.7, 0.8 和 1.0（与原始输入相同），以可视化网络的内部工作。

[![图像](../Images/6500908cf499dd7e147d14afb520fefa.png)](https://image.ibb.co/dqbES8/jaedukseo_alphas.jpg)

上述所有图像表示了不同 alpha 值下相对于输入的梯度。我们可以观察到，随着 alpha 值的增加，梯度变得越来越接近 0（黑色），这是由于网络的饱和。我们可以清楚地看到，当我们将所有图像以 gif 格式查看时，梯度变化的差异。

![](../Images/44353a386f8ae7f410ac356c27d2284a.png)

然而，我能够做出一个有趣的观察，随着 alpha 值变小，我的网络无法做出正确的预测。如下面所示，当 alpha 值较小时（0.01），网络预测大多为 3 或 4（猫/鹿）

![](../Images/0c15fd789ec51ae1af897a4e170413c3.png)![](../Images/657845dc01154e828bb72e7179fbb9f1.png)

**红色框** → 模型预测

但随着 alpha 值的增加，最终达到 1.0，我们可以观察到模型的准确性增加。最后，让我们查看每个 alpha 值下梯度的正面和负面属性。

![](../Images/a222dcb85cc7e2d296fc24e584a4bbf3.png)

**蓝色像素 →** 与原始图像叠加的正面属性

**红色像素 →** 与原始图像叠加的负面属性

[![图像](../Images/e3cd81af95d297f575f502e36aeb2f91.png)](https://image.ibb.co/moeeuo/jaedukseo_alphas_2.jpg)

再次，如果我们将这些图像制作成 gif，就更容易查看变化。

![](../Images/a05f316ca73552e1fbc6ab99fe5174b7.png)

### **积分梯度 / 正面和负面属性**

![](../Images/63c5b2df3e30865ad02c2302b1328bd2.png)![](../Images/f8b236395a2e8acd0b5130c7f2ddd299.png)

**左侧图像** → 积分梯度，步长为 3000，用于 [黎曼近似](https://en.wikipedia.org/wiki/Riemann_sum)

**右侧图像** → 正面属性用蓝色标记，负面属性用红色标记

最后，当我们使用积分梯度来可视化梯度时，我们可以观察到如上所示的结果。我发现的一个非常有趣的事实是网络如何预测马（7）、狗（5）和鹿（4）。

![](../Images/0dcba269087e7bbac3dde2ddce079742.png)

首先，让我们看一下原始图像，如上所示，左侧和中间的图像是马，右侧的图像是绵羊。

![](../Images/f1d3f810a13fc6e93ecfc7705c22a87b.png)

当我们可视化积分梯度时，可以看到一些有趣的现象。正如中间图像所示，网络认为马的头部最重要。然而，当仅仅考虑头部时，网络认为这是一张狗的图片。（而且它确实有点像狗）。右侧图像也有类似的故事，当网络仅考虑绵羊的头部时，它预测为狗。

**然而**，正如最左侧的图像所示，当网络接收马的整体形状（包括一些人类的部分）时，它正确地识别了这张图像为马。真是**惊人**！

### **交互式代码**

![](../Images/932b6906c5b23eaa53a6499266270145.png)

*对于 Google Colab，你需要一个谷歌账户来查看代码，另外你不能在 Google Colab 中运行只读脚本，所以请在你的沙盒中复制一份。最后，我永远不会请求访问你 Google Drive 上的文件，仅供参考。祝编程愉快！为了透明度，我已将所有训练日志上传到我的 GitHub。*

要访问此帖子的代码，请 [点击这里](https://colab.research.google.com/drive/1ld8_40udZbnWALV1pU582JphXUZD-npn)，要查看训练 [日志请点击这里。](https://github.com/JaeDukSeo/Daily-Neural-Network-Practice-2/blob/master/Understanding_Concepts/COUNTERFACTUALS/viz/z_viz.txt)

### **最后的话**

由于这一领域仍在发展中，将会有新的方法和发现。（我希望我也能为这些发现做出贡献。）最后，如果你想访问原始论文“[*Axiomatic Attribution for Deep Networks*](https://arxiv.org/abs/1703.01365)”的代码， [请点击这里](https://github.com/ankurtaly/Integrated-Gradients/blob/master/attributions.ipynb)。

如果发现任何错误，请发邮件到 jae.duk.seo@gmail.com。如果你希望查看我所有写作的列表，请 [点击这里查看我的网站](https://jaedukseo.me/)。

同时在我的 Twitter [关注我](https://twitter.com/JaeDukSeo)，访问 [我的网站](https://jaedukseo.me/)，或者我的 [YouTube 频道](https://www.youtube.com/c/JaeDukSeo) 以获取更多内容。我还实现了 [Wide Residual Networks，请点击这里查看博客文章](https://medium.com/@SeoJaeDuk/wide-residual-networks-with-interactive-code-5e190f8f25ec)。

**参考资料**

1.  [http://Axiom/definition/theorem/proof](http://axiom/definition/theorem/proof)。 （2018）。YouTube。检索于 2018 年 6 月 14 日，来自 [https://www.youtube.com/watch?v=OeC5WuZbNMI](https://www.youtube.com/watch?v=OeC5WuZbNMI)

1.  axiom — 词典定义。（2018）。Vocabulary.com。检索于 2018 年 6 月 14 日，来自 [https://www.vocabulary.com/dictionary/axiom](https://www.vocabulary.com/dictionary/axiom)

1.  Axiomatic Attribution for Deep Networks — — Mukund Sundararajan, Ankur Taly, Qiqi Yan. (2017). Vimeo. 取自 [https://vimeo.com/238242575](https://vimeo.com/238242575)

1.  STL-10 数据集。 (2018). Cs.stanford.edu. 取自 [https://cs.stanford.edu/~acoates/stl10/](https://cs.stanford.edu/~acoates/stl10/)

1.  Benenson, R. (2018). 分类数据集结果。 Rodrigob.github.io. 取自 [http://rodrigob.github.io/are_we_there_yet/build/classification_datasets_results.html#53544c2d3130](http://rodrigob.github.io/are_we_there_yet/build/classification_datasets_results.html#53544c2d3130)

1.  [duplicate], H. (2018). 如何在 Matplotlib (python) 中隐藏坐标轴和网格线。 Stack Overflow. 取自 [https://stackoverflow.com/questions/45148704/how-to-hide-axes-and-gridlines-in-matplotlib-python](https://stackoverflow.com/questions/45148704/how-to-hide-axes-and-gridlines-in-matplotlib-python)

1.  VanderPlas, J. (2018). 多个子图 | Python 数据科学手册。 Jakevdp.github.io. 取自 [https://jakevdp.github.io/PythonDataScienceHandbook/04.08-multiple-subplots.html](https://jakevdp.github.io/PythonDataScienceHandbook/04.08-multiple-subplots.html)

1.  matplotlib.pyplot.subplot — Matplotlib 2.2.2 文档。 (2018). Matplotlib.org. 取自 [https://matplotlib.org/api/_as_gen/matplotlib.pyplot.subplot.html](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.subplot.html)

1.  matplotlib, m. (2018). matplotlib 中超过 9 个子图。 Stack Overflow. 取自 [https://stackoverflow.com/questions/4158367/more-than-9-subplots-in-matplotlib](https://stackoverflow.com/questions/4158367/more-than-9-subplots-in-matplotlib)

1.  pylab_examples 示例代码：subplots_demo.py — Matplotlib 2.0.0 文档。 (2018). Matplotlib.org. 取自 [https://matplotlib.org/2.0.0/examples/pylab_examples/subplots_demo.html](https://matplotlib.org/2.0.0/examples/pylab_examples/subplots_demo.html)

1.  matplotlib.pyplot.hist — Matplotlib 2.2.2 文档。 (2018). Matplotlib.org. 取自 [https://matplotlib.org/api/_as_gen/matplotlib.pyplot.hist.html](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.hist.html)

1.  Python?, I. (2018). 是否有一种干净的方法在 Python 中生成折线直方图？ Stack Overflow. 取自 [https://stackoverflow.com/questions/27872723/is-there-a-clean-way-to-generate-a-line-histogram-chart-in-python](https://stackoverflow.com/questions/27872723/is-there-a-clean-way-to-generate-a-line-histogram-chart-in-python)

1.  Pyplot Text — Matplotlib 2.2.2 文档。 (2018). Matplotlib.org. 取自 [https://matplotlib.org/gallery/pyplots/pyplot_text.html#sphx-glr-gallery-pyplots-pyplot-text-py](https://matplotlib.org/gallery/pyplots/pyplot_text.html#sphx-glr-gallery-pyplots-pyplot-text-py)

1.  [ Google / ICLR 2017 / 论文总结 ] 反事实的梯度。 (2018). Towards Data Science. 于2018年6月16日访问，来自 [https://towardsdatascience.com/google-iclr-2017-paper-summary-gradients-of-counterfactuals-6306510935f2](https://towardsdatascience.com/google-iclr-2017-paper-summary-gradients-of-counterfactuals-6306510935f2)

1.  numpy.transpose — NumPy v1.14 手册。 (2018). Docs.scipy.org. 于2018年6月16日访问，来自 [https://docs.scipy.org/doc/numpy-1.14.0/reference/generated/numpy.transpose.html](https://docs.scipy.org/doc/numpy-1.14.0/reference/generated/numpy.transpose.html)

1.  命令，G。 (2018). 在一个命令中执行 git add 和 commit。 Stack Overflow. 于2018年6月16日访问，来自 [https://stackoverflow.com/questions/4298960/git-add-and-commit-in-one-command](https://stackoverflow.com/questions/4298960/git-add-and-commit-in-one-command)

1.  如何将图像切分为红色、绿色和蓝色通道。 (2018). 如何使用 misc.imread 将图像切分为红色、绿色和蓝色通道。 Stack Overflow. 于2018年6月16日访问，来自 [https://stackoverflow.com/questions/37431599/how-to-slice-an-image-into-red-green-and-blue-channels-with-misc-imread](https://stackoverflow.com/questions/37431599/how-to-slice-an-image-into-red-green-and-blue-channels-with-misc-imread)

1.  如何在 Windows 10 上安装 Bash shell 命令行工具。 (2016). Windows Central. 于2018年6月16日访问，来自 [https://www.windowscentral.com/how-install-bash-shell-command-line-windows-10](https://www.windowscentral.com/how-install-bash-shell-command-line-windows-10)

1.  Google Colab 免费 GPU 教程 — Deep Learning Turkey — Medium. (2018). Medium. 于2018年6月16日访问，来自 [https://medium.com/deep-learning-turkey/google-colab-free-gpu-tutorial-e113627b9f5d](https://medium.com/deep-learning-turkey/google-colab-free-gpu-tutorial-e113627b9f5d)

1.  安装 — imgaug 0.2.5 文档。 (2018). Imgaug.readthedocs.io. 于2018年6月16日访问，来自 [http://imgaug.readthedocs.io/en/latest/source/installation.html](http://imgaug.readthedocs.io/en/latest/source/installation.html)

1.  numpy.absolute — NumPy v1.14 手册。 (2018). Docs.scipy.org. 于2018年6月16日访问，来自 [https://docs.scipy.org/doc/numpy-1.14.0/reference/generated/numpy.absolute.html](https://docs.scipy.org/doc/numpy-1.14.0/reference/generated/numpy.absolute.html)

1.  numpy.clip — NumPy v1.10 手册。 (2018). Docs.scipy.org. 于2018年6月16日访问，来自 [https://docs.scipy.org/doc/numpy-1.10.0/reference/generated/numpy.clip.html](https://docs.scipy.org/doc/numpy-1.10.0/reference/generated/numpy.clip.html)

1.  numpy.percentile — NumPy v1.14 手册。 (2018). Docs.scipy.org. 于2018年6月16日访问，来自 [https://docs.scipy.org/doc/numpy/reference/generated/numpy.percentile.html](https://docs.scipy.org/doc/numpy/reference/generated/numpy.percentile.html)

1.  CIFAR-10 和 CIFAR-100 数据集。 (2018). Cs.toronto.edu. 于2018年6月16日访问，来自 [https://www.cs.toronto.edu/~kriz/cifar.html](https://www.cs.toronto.edu/~kriz/cifar.html)

1.  [ICLR 2015] 追求简单性：具有交互式代码的全卷积网络 [手册…. (2018). Towards Data Science. 取自 [https://towardsdatascience.com/iclr-2015-striving-for-simplicity-the-all-convolutional-net-with-interactive-code-manual-b4976e206760](https://towardsdatascience.com/iclr-2015-striving-for-simplicity-the-all-convolutional-net-with-interactive-code-manual-b4976e206760) 于 2018 年 6 月 16 日

1.  EN10/CIFAR. (2018). GitHub. 取自 [https://github.com/EN10/CIFAR](https://github.com/EN10/CIFAR) 于 2018 年 6 月 16 日

1.  深度网络的公理归因 — — Mukund Sundararajan, Ankur Taly, Qiqi Yan. (2017). Vimeo. 取自 [https://vimeo.com/238242575](https://vimeo.com/238242575) 于 2018 年 6 月 16 日

1.  (2018). Arxiv.org. 取自 [https://arxiv.org/pdf/1506.06579.pdf](https://arxiv.org/pdf/1506.06579.pdf) 于 2018 年 6 月 16 日

1.  Riemann 和. (2018). En.wikipedia.org. 取自 [https://en.wikipedia.org/wiki/Riemann_sum](https://en.wikipedia.org/wiki/Riemann_sum) 于 2018 年 6 月 16 日

**简介: [Jae Duk Seo](https://jaedukseo.me/)** 是瑞尔森大学的四年级计算机科学学生。

[原文](https://towardsdatascience.com/inside-the-mind-of-a-neural-network-with-interactive-code-in-tensorflow-histogram-activation-a4dff0963103)。经授权转载。

**相关：**

+   [用 NumPy 从头开始构建卷积神经网络](/2018/04/building-convolutional-neural-network-numpy-scratch.html)

+   [我如何使用 CNN 和 TensorFlow 并在 Kaggle 挑战中失去了银牌](/2018/05/lost-silver-medal-kaggle-mercari-price-suggestion-challenge-cnns-tensorflow.html)

+   [使用 TensorFlow 对象检测进行像素级分类](/2018/03/tensorflow-object-detection-pixel-wise-classification.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 领域

* * *

### 更多相关话题

+   [使用 TensorFlow 和 Keras 构建并训练你的第一个神经网络](https://www.kdnuggets.com/2023/05/building-training-first-neural-network-tensorflow-keras.html)

+   [6 个让人惊叹的 ChatGPT 扩展插件](https://www.kdnuggets.com/2023/04/6-chatgpt-mindblowing-extensions-anywhere.html)

+   [选择下一个数据科学工作时需要牢记的5件事](https://www.kdnuggets.com/2022/01/5-things-keep-mind-selecting-next-job.html)

+   [数据管理：如何保持对客户的关注？](https://www.kdnuggets.com/2022/04/data-management-stay-top-customer-mind.html)

+   [DeepMind在使用深度学习推动数学发展的新努力](https://www.kdnuggets.com/2021/12/inside-deepmind-new-efforts-deep-learning-advance-mathematics.html)

+   [使用AIMET进行神经网络优化](https://www.kdnuggets.com/2022/04/qualcomm-neural-network-optimization-aimet.html)
