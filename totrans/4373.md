# tensorflow + dalex = :) ，或者如何解释一个 TensorFlow 模型

> 原文：[https://www.kdnuggets.com/2020/11/dalex-explain-tensorflow-model.html](https://www.kdnuggets.com/2020/11/dalex-explain-tensorflow-model.html)

[评论](#comments)

**作者 [Hubert Baniecki](https://www.linkedin.com/in/hbaniecki/)，MI2DataLab 的研究软件工程师**。

![](../Images/4720771e886fb515e514fe95ea57e380.png)

我将展示如何使用 [*tensorflow*](https://github.com/tensorflow/tensorflow) 预测模型，并利用 [*dalex*](https://github.com/ModelOriented/DALEX) Python 包进行解释。这一主题的介绍可以在 [解释模型分析：探索、解释和检查预测模型](https://pbiecek.github.io/ema) 中找到。

对于这个示例，我们将使用 [**世界幸福报告**](https://www.kaggle.com/unsdsn/world-happiness) 的数据，并预测任何给定国家根据经济生产、社会支持等的幸福评分。

![](../Images/f28a71d8c49b7c64d571da44cba97e8c.png)

*数据来源于世界幸福报告 (Kaggle.com)。*

首先训练基础的 *tensorflow* 模型，并加入实验归一化层以获得更好的拟合。

下一步是创建一个 *dalex* 解释器对象，该对象以模型和数据作为输入。

现在，我们准备通过各种方法来解释模型：模型级别的方法解释全球行为，而预测级别的方法则专注于数据中的单个观察。我们可以从评估模型性能开始。

![](../Images/6b8924bd43808999533c296fb6caea3a.png)

*幸福回归任务的模型性能。*

哪些特征最重要？让我们对比两种方法，其中一种在 [*shap*](https://github.com/slundberg/shap) 包中实现。

![](../Images/d6d3af74cd16fd7e1cb794bbba6daf8a.png)

![](../Images/a32150ddef86516a92387691a5d94831.png)

*对比两种特征重要性方法，其中一种在 shap 包中实现。*

变量与预测之间的连续关系是什么？我们使用部分依赖分析，指出并非总是越多越好。

![](../Images/fbff01f8116061da3251c44267997e59.png)

*部分依赖分析（Partial Dependence profiles）展示了变量与预测之间的连续关系。*

残差如何？这些图表对于可视化模型错误的地方非常有用。

![](../Images/bd8ae98e8cefa28a4b4a13444268b7e7.png)

*残差诊断可以帮助评估我们模型的弱点。*

对于特定国家的变量归因，可以更加好奇，

![](../Images/27e0e02a57bfb992d481a4307c9e49dd.png)

*对波兰的变量归因。*

或多个国家进行比较结果。

![](../Images/6dfa9c5504e6d5eec65e85825d15b8bd.png)

*多个国家的变量归因。*

如果你对替代逼近感兴趣，可以通过统一接口生成[*lime*](https://github.com/marcotcr/lime)包的解释。

![](../Images/4061aa60bb17269089a03f2df22281c9.png)

*替代逼近 - 来自 lime 包的解释。*

最后，如果需要一个可解释的模型，我们可以用易于理解的决策树来逼近黑箱模型。

![](../Images/f3eb1731bad1471e3c60817c4fb64392.png)

*基于黑箱模型预测值训练的决策树。*

我希望这段旅程给你带来了一些快乐，因为如今解释预测模型变得更加易于访问和用户友好。当然，在 *dalex* 工具包中还有更多的解释、结果和图示。我们准备了各种资源，列在 [包的 README](https://github.com/ModelOriented/DALEX/tree/master/python/dalex) 中。

这部分的代码可以在 [http://dalex.drwhy.ai/python-dalex-tensorflow.html](https://dalex.drwhy.ai/python-dalex-tensorflow.html) 找到。

```py
pip install tensorflow dalex shap statsmodels lime scikit-learn

```

:)

[原文](https://medium.com/responsibleml/tensorflow-dalex-929ca520d3d9)。已获许可转载。

**简介：** [Hubert Baniecki](https://www.linkedin.com/in/hbaniecki/) 是一名研究软件工程师，开发用于可解释 AI 的 R 和 Python 工具，并在解释性和人机交互的背景下研究机器学习。

**相关：**

+   [可解释性、解释性和机器学习——数据科学家需要知道什么](https://www.kdnuggets.com/2020/11/interpretability-explainability-machine-learning.html)

+   [解释可解释 AI: 二阶段方法](https://www.kdnuggets.com/2020/10/explaining-explainable-ai.html)

+   [解释性：破解黑箱，第 1 部分](https://www.kdnuggets.com/2019/12/explainability-black-box-part1.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 方面的工作

* * *

### 更多相关话题

+   [SHAP：在 Python 中解释任何机器学习模型](https://www.kdnuggets.com/2022/11/shap-explain-machine-learning-model-python.html)

+   [用 LIME 解释 NLP 模型](https://www.kdnuggets.com/2022/01/explain-nlp-models-lime.html)

+   [使用 Tensorflow 训练图像分类模型的指南](https://www.kdnuggets.com/2022/12/guide-train-image-classification-model-tensorflow.html)

+   [Segment Anything Model: 基于图像分割的基础模型](https://www.kdnuggets.com/2023/07/segment-anything-model-foundation-model-image-segmentation.html)

+   [计算机视觉中的 TensorFlow - 轻松掌握迁移学习](https://www.kdnuggets.com/2022/01/tensorflow-computer-vision-transfer-learning-made-easy.html)

+   [PyTorch 还是 TensorFlow？比较流行的机器学习框架](https://www.kdnuggets.com/2022/02/packt-pytorch-tensorflow-comparing-popular-machine-learning-frameworks.html)
