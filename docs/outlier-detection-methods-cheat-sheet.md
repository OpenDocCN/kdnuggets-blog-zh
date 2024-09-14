# 异常值检测方法的直观可视化

> 原文：[`www.kdnuggets.com/2019/02/outlier-detection-methods-cheat-sheet.html`](https://www.kdnuggets.com/2019/02/outlier-detection-methods-cheat-sheet.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

[异常值](https://en.wikipedia.org/wiki/Outlier)是指那些似乎无法很好适应剩余数据或模型的数据实例。虽然许多机器学习算法故意不考虑异常值，或者可以修改为明确地丢弃它们，但有时异常值本身可能就是关键。

在欺诈检测中，这种情况尤为明显，欺诈检测利用异常值来识别欺诈活动。如果你经常在纽约及其周边以及在线使用信用卡，主要用于一些微不足道的消费？今天早上在 Soho 的咖啡店用过，晚上在 Upper West Side 吃过晚饭，但在这期间在巴黎“亲自”花费几千美元购买电子设备？这就是你的异常值，这些会被用各种机器学习技术进行不懈追踪。

这个简单的例子实际上让这看起来过于简单了。实际上，没有普遍适用的异常值定义；你能想象试图定义一个具体规则来界定地理上“过远”而不适用的情况，并且这个规则适用于所有类似上述情况的欺诈检测场景吗？即使我们能达成共识什么是异常值，根据应用的不同，我们可能不希望去除它，只是对其存在保持警觉。

即使我们说我们希望在检测到异常值时收到通知，仍然有各种方法可以实现这一点。想要使用简单的描述性统计方法，比如识别在正态分布中偏离均值特定倍数的低维数据点？很好，如果这对你有效。但也有许多其他方法。

查看这个用于异常值检测方法的可视化，它来自于[**Python 异常值检测（PyOD）**](https://github.com/yzhao062/pyod)的创建者——我鼓励你点击查看以享受高清分辨率的效果：

![异常值检测](https://raw.githubusercontent.com/yzhao062/pyod/master/examples/ALL.png)

点击放大

至少有 12 种异常值检测方法以非常直观的方式进行了可视化。他们在这方面做得非常出色。

当然，虽然这作为了解不同异常值检测方法如何工作的极好备忘单，但实际上这是来自于[之前提到的 Python 项目](https://github.com/yzhao062/pyod)：

> PyOD 是一个全面且可扩展的 Python 工具包，用于检测多变量数据中的异常对象。这个既令人兴奋又具有挑战性的领域通常被称为异常值检测或异常检测。自 2017 年以来，PyOD 已成功应用于各种学术研究和商业产品中。

如果你正在学习异常值检测，PyOD 是一个简单的工具包，具有 Scikit-learn 风格的 API，包括许多检测算法的实现（它的 GitHub 仓库链接到算法的原始论文），且足够直观，可以几乎立即上手，只要你对当代 Python 机器学习生态系统的各个组件有一定了解。你可以在[这里找到项目文档](https://pyod.readthedocs.io/en/latest/)。

**相关**：

+   如何使你的机器学习模型对异常值具有鲁棒性

+   异常值检测的四种技术

+   数据科学基础：从数据中可以挖掘出哪些模式？

### 更多相关话题

+   [停止学习数据科学以找到目的，并通过找到目的来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [一个 90 亿美元的 AI 失败，详细审查](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功的数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么使得 Python 成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每个数据科学家都应该了解的三种 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
