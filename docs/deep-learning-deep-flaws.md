# 深度学习有深层缺陷吗？

> 原文：[https://www.kdnuggets.com/2014/06/deep-learning-deep-flaws.html](https://www.kdnuggets.com/2014/06/deep-learning-deep-flaws.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

这里有三对图像。你能找到左列和右列之间的差异吗？

[![对抗样本1](../Images/dd38fdc46049d50f9adfbacc887f8e0b.png)](/wp-content/uploads/adversarial1.png)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的IT需求

* * *

我想不行。它们在肉眼看来完全相同，但对深度神经网络而言却不然。实际上，深度神经网络只能正确识别左侧的图像，而不能识别右侧的图像。有趣，不是吗？

[最近的一项研究](http://cs.nyu.edu/~zaremba/docs/understanding.pdf)由谷歌、纽约大学和蒙特利尔大学的研究人员进行，发现了几乎所有深度神经网络中的这一缺陷。

介绍了深度神经网络的两个反直觉特性。

***1\. 在神经网络的高层中，包含语义信息的是空间，而不是单个单元。*** 这意味着原始图像的随机失真也可以被正确分类。

下图比较了在MNIST上训练的卷积神经网络中自然基和随机基的表现，使用ImageNet数据集作为验证集。

[![自然基](../Images/cbc3d1a7dac44545b71375eaa3c10135.png)](/wp-content/uploads/natural-basis.png)

[![随机基](../Images/ffa882c9949b22747a15ed629d8067f5.png)](/wp-content/uploads/random-basis.png)

对于自然基（上图），它将隐藏单元的激活视为特征，并寻找最大化该单一特征激活值的输入图像。我们可以将这些特征解释为输入域中的有意义的变化。然而，实验表明这种可解释的语义也适用于任何随机方向（下图）。

以白色花朵识别为例。传统上，具有黑色圆圈和扇形白色区域的图像最有可能被分类为白色花朵。然而，事实证明，如果我们选择一组随机基，响应的图像也可以以类似的方式进行语义解释。

***2. 在研究人员应用某种难以察觉的扰动后，网络可能会错误分类一张图像。*** 这些扰动是通过调整像素值以最大化预测错误来找到的。

> 对于我们研究的所有网络（MNIST、QuocNet、AlexNet），对于每个样本，我们总能生成非常接近、在视觉上难以区分的对抗样本，而这些样本会被原始网络错误分类。

以下示例中，左侧为正确预测样本，右侧为对抗样本，中间为它们之间差异的10倍放大。对于人类而言，这两列是相同的，而对神经网络却完全不同。

[![对抗样本2](../Images/cea6e4e34db4614c979ecbd9acdf9c5e.png)](/wp-content/uploads/adversarial2.png)

> 这是一个显著的发现。难道它们不应该对小扰动免疫吗？深度神经网络的连续性和稳定性受到质疑。平滑性假设不再适用于深度神经网络。

更令人惊讶的是，相同的扰动可以导致一个在不同训练数据集上训练的网络对相同图像进行错误分类。这意味着对抗样本在某种程度上是普遍存在的。

这个结果可能会改变我们管理训练/验证数据集和边缘案例的方式。实验已经支持，如果我们保留一组对抗样本并将其混入原始训练集中，泛化能力会有所提高。对抗样本对于高层次的作用似乎比低层次的更为有效。

这些影响不仅限于此。

作者声称，

> 尽管对抗负样本的集合很密集，但概率极低，因此在测试集中很少被观察到。

但“罕见”并不意味着从未发生。想象一下，如果带有这种盲点的AI系统被应用于犯罪检测、安全设备或银行系统。如果我们不断深入挖掘，是否在我们的大脑中也存在这样的对抗图像？很可怕，不是吗？

![Ran Bi](../Images/9ed3515a9880a5fad72f74278778cc7f.png)

**Ran Bi** 是纽约大学数据科学项目的硕士生。在NYU学习期间，她在机器学习、深度学习以及大数据分析方面完成了多个项目。凭借本科阶段金融工程的背景，她对商业分析也很感兴趣。

**相关：**

+   [在哪里学习深度学习 – 课程、教程、软件](/2014/05/learn-deep-learning-courses-tutorials-overviews.html)

+   [深度学习分析如何模拟大脑](/2014/03/how-deep-learning-analytics-mimic-mind.html)

+   [深度学习在Kaggle的狗猫竞赛中获胜](/2014/02/deep-learning-wins-dogs-vs-cats-competition.html)

### 主题相关内容

+   [ETL与机器学习有什么关系？](https://www.kdnuggets.com/2022/08/etl-machine-learning.html)

+   [ChatGPT 是否有潜力成为新的国际象棋超级特级大师？](https://www.kdnuggets.com/does-chatgpt-have-the-potential-to-become-a-new-chess-super-grandmaster)

+   [机器学习为什么没有为我的业务创造价值？](https://www.kdnuggets.com/2021/12/machine-learning-produce-value-business.html)

+   [数据科学家做什么？](https://www.kdnuggets.com/2021/12/what-does-a-data-scientist-do.html)

+   [随机森林算法是否需要标准化？](https://www.kdnuggets.com/2022/07/random-forest-algorithm-need-normalization.html)

+   [数据从哪里来？](https://www.kdnuggets.com/2022/08/data-come.html)
