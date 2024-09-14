# 使用 Tensorflow Object Detection 和 OpenCV 分析足球（足球）比赛

> 原文：[https://www.kdnuggets.com/2018/07/analyze-soccer-game-using-tensorflow-object-detection-opencv.html](https://www.kdnuggets.com/2018/07/analyze-soccer-game-using-tensorflow-object-detection-opencv.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由 [Priyanka Kochhar](https://github.com/priya-dwivedi) 提供，深度学习顾问**

### **介绍**

世界杯赛季来了，并且开始得非常有趣。谁曾想到卫冕冠军德国会在小组赛中被淘汰 :(

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

对于你内心的数据科学家来说，让我们利用这个机会对足球片段进行一些分析。通过深度学习和 OpenCV，我们可以从视频片段中提取有趣的见解。见下图澳大利亚与秘鲁比赛的示例 GIF，我们可以识别所有的球员 + 裁判，足球，并预测球员的球队基于他们球衣的颜色。所有这些都可以实时完成。

![](../Images/07b25fd30b67c19bdd28661a485b9554.png)

球员检测和团队预测

你可以在我的 [Github 仓库](https://github.com/priya-dwivedi/Deep-Learning/blob/master/soccer_team_prediction/soccer_realtime.ipynb) 找到我使用的代码。

### **步骤概览**

Tensorflow Object Detection API 是一个非常强大的工具，用于快速构建物体检测模型。如果你不熟悉这个 API，请查看我以下的博客，这些博客介绍了该 API 并教你如何使用它构建自定义模型。

[Tensorflow Object Detection API 介绍](http://deeplearninganalytics.org/blog/introduction-to-tensorflow-object-detection-api)

[使用 Tensorflow Object Detection API 构建自定义模型](http://deeplearninganalytics.org/blog/building-toy-detector-with-object-detection-api)

该 API 提供了在 COCO 数据集上训练的预训练物体检测模型。COCO 数据集是一个包含 90 种常见对象的数据集。见下图 COCO 数据集的一部分对象。

![](../Images/fd31605da1c1f2ce10a9e0b9985f8da3.png)

coco 对象类别

在这种情况下，我们关心的是类别——人和足球，这些都是 COCO 数据集的一部分。

该 API 还支持大量的模型。见下表作为参考。

![](../Images/35568105aa26885446c3b973afee2ec8.png)

API 支持的模型的小子集

模型在速度和准确性之间有权衡。由于我对实时分析感兴趣，我选择了 SSDLite mobilenet v2。

一旦我们使用对象检测 API 识别出球员，为了预测他们属于哪个队，我们可以使用 OpenCV，它是一个强大的图像处理库。如果你是 OpenCV 的新手，请参见下面的教程：

[OpenCV 教程](http://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_tutorials.html)

OpenCV 允许我们识别特定颜色的掩膜，我们可以利用这一点来识别红色球员和黄色球员。请参见下面的示例，了解 OpenCV 掩膜如何在图像中检测红色。

![](../Images/9da890317f9dbabcd95094c08c7161be.png)

图像中红色区域的预测

### **深入了解主要步骤**

现在让我们详细了解一下代码。

如果你第一次使用 Tensorflow 对象检测 API，请从这个[link](https://github.com/tensorflow/models/tree/master/research/object_detection) 下载 GitHub，并使用这些[instructions](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/installation.md) 安装所有依赖项。

如果你还没有设置 OpenCV，请按照这个[tutorial](https://docs.opencv.org/3.4.1/d2/de6/tutorial_py_setup_in_ubuntu.html) 从源代码构建。

我遵循的主要步骤是（请在我的[Github](https://github.com/priya-dwivedi/Deep-Learning/blob/master/soccer_team_prediction/soccer_realtime.ipynb)上的 jupyter notebook 中跟随）：

+   将 SSDLite mobilenet 模型加载到图形中，并加载 COCO 数据集中的类别列表

+   使用 cv2.VideoCapture(filename) 打开视频，并逐帧读取每一帧

+   对每一帧执行对象检测，使用加载的图形

+   从 SSDLite 返回的结果是每个识别的类别及其置信度分数和边界框预测。因此，现在识别所有置信度 > 0.6 的人员并裁剪他们。

+   现在你已经提取出每个球员。我们需要读取他们球衣的颜色来预测他们是澳大利亚球员还是秘鲁球员。这是通过代码块 detect team 完成的。我们首先定义红色和蓝色的颜色范围。然后使用 cv2.inRange 和 cv2.bitwise 创建该颜色的掩膜。为了检测团队，我计算了检测到的红色像素和黄色像素的数量以及这些像素与裁剪图像总像素数的百分比。

+   最终将所有代码片段组合起来，同时运行并使用 cv2.imshow 显示结果

### **结论和参考文献**

太棒了。现在你可以看到深度学习和 OpenCV 的简单组合如何产生有趣的结果。既然你有了这些数据，有很多方法可以从中挖掘更多的见解：

1.  通过将相机角度对准澳大利亚进球区域，你可以计算出在该区域内的秘鲁球员与澳大利亚球员的数量

1.  你可以绘制每个团队足迹的热图——例如，哪些区域是秘鲁团队的高占用区域

1.  你可以绘制出守门员的路径

对象检测 API 还提供其他更准确但更慢的模型。你也可以尝试那些。

如果你喜欢这篇文章，请给我一个 ❤️ :) 希望你能下载代码并自己试试。

**其他著作**: [http://deeplearninganalytics.org/blog](http://deeplearninganalytics.org/blog)

附言：我拥有自己的深度学习咨询公司，并且喜欢处理有趣的问题。我曾帮助多家初创公司部署创新的人工智能解决方案。查看我们的网站——[http://deeplearninganalytics.org/](http://deeplearninganalytics.org/)。

如果你有一个我们可以合作的项目，请通过我的网站或 email priya.toronto3@gmail.com 联系我

**参考文献**

+   [Tensorflow 对象检测 API](https://github.com/tensorflow/models/tree/master/research/object_detection)

+   一个关于使用 OpenCV 检测颜色的好 [教程](https://www.pyimagesearch.com/2014/08/04/opencv-python-color-detection/)

**个人简介：[Priyanka Kochhar](https://github.com/priya-dwivedi)** 拥有超过 10 年的数据科学经验。她现在拥有自己的深度学习咨询公司，喜欢处理有趣的问题。她曾帮助多家初创公司部署创新的人工智能解决方案。如果你有一个她可以合作的项目，请通过 [priya.toronto3@gmail.com](mailto:priya.toronto3@gmail.com) 联系她。

[原文](https://towardsdatascience.com/analyse-a-soccer-game-using-tensorflow-object-detection-and-opencv-e321c230e8f2)。已获许可转载。

**相关内容：**

+   [Google Tensorflow 对象检测 API 是否是实现图像识别的最简单方法？](/2018/03/google-tensorflow-object-detection-api-the-easiest-way-implement-image-recognition.html)

+   [使用 Tensorflow 对象检测 API 构建玩具探测器](/2018/02/building-toy-detector-tensorflow-object-detection-api.html)

+   [训练和可视化词向量](/2018/01/training-visualising-word-vectors.html)

### 更多相关内容

+   [成为优秀数据科学家所需的 5 项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应该掌握的 6 种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021 年最佳 ETL 工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [如何使用图论来侦查足球比赛](https://www.kdnuggets.com/2022/11/graph-theory-scout-soccer.html)

+   [使用管道编写干净的 Python 代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [停止学习数据科学以寻找目标，并找到目标后再……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)
