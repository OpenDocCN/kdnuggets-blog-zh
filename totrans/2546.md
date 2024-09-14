# 10个机器学习模型训练中的错误

> 原文：[https://www.kdnuggets.com/2021/07/10-machine-learning-model-training-mistakes.html](https://www.kdnuggets.com/2021/07/10-machine-learning-model-training-mistakes.html)

[评论](#comments)

**作者：[Sandeep Uttamchandani, Ph.D.](https://www.linkedin.com/in/sandeepuc/)，既是产品/软件开发者（工程副总裁），也是企业范围内数据/AI项目的领导者（首席数据官）**

![Mistakes header](../Images/70a570ad807374cd4dd6269c8abf20d0.png)

图片来自[Tumisu](https://pixabay.com/users/tumisu-148124/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1966448)的[Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1966448)

机器学习模型训练是整个模型构建过程中最耗时和资源的部分。训练本质上是迭代的，但在某些迭代过程中，错误可能会渗入。在这篇文章中，我分享了机器学习模型训练中的十个致命错误——这些错误是最常见的，也是最容易被忽视的。

## 机器学习模型训练的十个致命错误

### **1\. 在模型未收敛时盲目增加轮次**

在模型训练过程中，有时损失-轮次图表会反复波动，不论轮次多少都似乎无法收敛。没有万能的解决方案，因为需要调查多个根本原因——不良训练样本、缺失的真值、变化的数据分布、过高的学习率。我见过的最常见原因是不良训练样本，涉及异常数据与不正确标签的组合。

### 2. **未对训练数据集进行随机打乱**

有时，模型似乎正在收敛，但突然损失值显著增加，即损失值在减少后突然显著增加。这种损失爆炸有多种原因。我见过的最常见原因是数据中的离群值没有均匀分布/打乱。打乱一般来说是一个重要步骤，包括在损失表现出重复步进函数行为的模式中。

### 3. **在多类别分类中，不优先考虑特定类别的度量准确性**

对于多类别预测问题，除了跟踪总体分类准确性外，通常还需优先考虑特定类别的准确性，并逐步改进模型。例如，在对不同类型的欺诈交易进行分类时，根据业务需求，专注于提高特定类别（如外国交易）的召回率。

### 4. **假设特异性会导致模型准确性降低**

与其构建一个通用模型，不如想象为特定地理区域或特定用户画像构建模型。特定性会使数据更加稀疏，但可能会提高对这些特定问题的准确性。在调优过程中，探索特定性和稀疏性的权衡是很重要的。

### 5. **忽视预测偏差**

预测偏差是预测平均值和数据集中标签平均值之间的差异。预测偏差是模型问题的早期指标。较大的非零预测偏差表明模型中存在某个地方的错误。关于广告点击率的一个有趣的 [Facebook 论文](https://research.fb.com/wp-content/uploads/2016/11/practical-lessons-from-predicting-clicks-on-ads-at-facebook.pdf)。通常，偏差在预测桶之间的测量是有用的。

### 6. **仅仅依靠模型准确率就称之为成功**

95% 的准确率意味着 100 次预测中有 95 次是正确的。在数据集中存在类别不平衡的情况下，准确率是一个有缺陷的指标。应该深入调查诸如精准度/召回率等指标，以及它们如何与整体用户指标（如垃圾邮件检测、肿瘤分类等）相关联。

### 7. **不了解正则化 λ 的影响**

λ 是在简单性和训练数据拟合之间取得平衡的关键参数。高 λ → 简单模型 → 可能欠拟合。低 λ → 复杂模型 → 可能对数据过拟合（无法推广到新数据）。理想的 λ 值是能够很好地推广到以前未见过的数据的值：依赖数据并需要分析。

### 8. 重复使用相同的测试集

使用相同数据进行参数和超参数设置的次数越多，对结果实际推广能力的信心就越小。重要的是收集更多的数据，并不断增加测试和验证集。

### **9. 未关注神经网络中的初始化值**

鉴于神经网络中的非凸优化，[初始化很重要](https://www.deeplearning.ai/ai-notes/initialization/)。

### 10. 假设错误标签总是需要修复

当发现错误标签时，可能会很想立即修复它们。首先分析误分类示例的根本原因是很重要的。通常，由于标签错误引起的错误可能只占很小的比例。可能存在更大的机会来更好地训练针对特定数据片段的模型，这些数据片段可能是主要的根本原因。

总结来说，避免这些错误可以让你在大多数其他团队中脱颖而出。将这些作为你的流程检查清单。

**简介: [Sandeep Uttamchandani, Ph.D.](https://www.linkedin.com/in/sandeepuc/)**: 数据 + 人工智能/机器学习 -- 既是产品/软件构建者（工程副总裁）也是企业范围内数据/人工智能项目的领导者（首席数据官） | O'Reilly 图书作者 | DataForHumanity 创始人（非营利组织）

[原文](https://betterprogramming.pub/10-deadly-sins-of-ml-model-training-a5046c1f5094)。经授权转载。

**相关：**

+   [如何判断你的机器学习模型是否过拟合](/2021/05/how-determine-machine-learning-model-overtrained.html)

+   [使用PyCaret编写和训练你自己的自定义机器学习模型](/2021/05/pycaret-write-train-custom-machine-learning-models.html)

+   [如何在20天内破坏一个模型——关于生产模型分析的教程](/2021/03/break-model-20-days-tutorial-production-model-analytics.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT需求

* * *

### 更多相关话题

+   [如何使用合成数据克服机器学习中的数据短缺问题](https://www.kdnuggets.com/2022/03/synthetic-data-overcome-data-shortages-machine-learning-model-training.html)

+   [如何加快XGBoost模型训练速度](https://www.kdnuggets.com/2021/12/speed-xgboost-model-training.html)

+   [软件错误与权衡：Tomasz Lelek的新书及…](https://www.kdnuggets.com/2021/12/manning-software-mistakes-tradeoffs-book.html)

+   [新手数据科学家应避免的错误](https://www.kdnuggets.com/2022/06/mistakes-newbie-data-scientists-avoid.html)

+   [可能影响数据分析准确性的3个错误](https://www.kdnuggets.com/2023/03/3-mistakes-could-affecting-accuracy-data-analytics.html)

+   [避免这5个每位AI新手都会犯的常见错误](https://www.kdnuggets.com/avoid-these-5-common-mistakes-every-novice-in-ai-makes)
