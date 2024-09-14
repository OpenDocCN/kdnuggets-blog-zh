# DIY 深度学习项目

> 原文：[https://www.kdnuggets.com/2018/06/diy-deep-learning-projects.html](https://www.kdnuggets.com/2018/06/diy-deep-learning-projects.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

![图像](../Images/17f2247c67478f353b74fc6c0d17296d.png)

### LinkedIn 数据科学社区

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT

* * *

![](../Images/338973683142cf09dec12d6f65a55655.png)

阿克沙伊·巴哈杜尔（Akshay Bahadur）是 LinkedIn 数据科学社区中的优秀例子之一。还有许多其他平台上的优秀人士，如 Quora、StackOverflow、YouTube 和众多论坛，他们在科学、哲学、数学、语言以及数据科学及其相关领域相互帮助。

![](../Images/835c6f3c51ede047eadc1d6ca2ac2318.png)

阿克沙伊·巴哈杜尔（Akshay Bahadur）。

但我认为在过去的 ~3 年里，LinkedIn 社区在数据科学领域分享了很多精彩内容，从分享经验到详细的帖子，介绍了如何在现实世界中进行机器学习或深度学习。我总是推荐进入这个领域的人加入一个社区，LinkedIn 是最好的之一，你会一直在那里找到我 :).

### 从深度学习和计算机视觉开始

![](../Images/eb810539c6ccac2761f884c014417c71.png)

[https://github.com/facebookresearch/Detectron](https://github.com/facebookresearch/Detectron)

在深度学习领域，研究图像分类、检测以及在“看到”某物时采取行动非常重要，这一领域在这个十年取得了令人惊叹的成果，比如在某些问题上超越了人类水平的表现。

在这篇文章中，我将展示阿克沙伊·巴哈杜尔在计算机视觉（CV）和深度学习（DL）领域所做的每一篇帖子。如果你对这些术语不熟悉，可以在这里了解更多：

[**“奇特”的深度学习介绍**](https://towardsdatascience.com/a-weird-introduction-to-deep-learning-7828803693b0)

*关于深度学习的介绍、课程和博客文章非常丰富。但这是一种不同的介绍。* [towardsdatascience.com](https://towardsdatascience.com/a-weird-introduction-to-deep-learning-7828803693b0)

[**两个月探索深度学习和计算机视觉**](https://towardsdatascience.com/two-months-exploring-deep-learning-and-computer-vision-3dcc84b2457f)

*我决定提高对计算机视觉和机器学习技术的熟悉程度。作为一名网页开发人员，我发现了这一点……* [towardsdatascience.com](https://towardsdatascience.com/two-months-exploring-deep-learning-and-computer-vision-3dcc84b2457f)

[**从神经科学到计算机视觉**](https://towardsdatascience.com/from-neuroscience-to-computer-vision-e86a4dea3574)

*对人类和计算机视觉的 50 年回顾* [towardsdatascience.com](https://towardsdatascience.com/from-neuroscience-to-computer-vision-e86a4dea3574)

[**Andrew Ng 的计算机视觉 - 学到的 11 个课程**](https://towardsdatascience.com/computer-vision-by-andrew-ng-11-lessons-learned-7d05c18a6999)

*我最近完成了 Andrew Ng 的 Coursera 计算机视觉课程。Ng 对许多…* [towardsdatascience.com](https://towardsdatascience.com/computer-vision-by-andrew-ng-11-lessons-learned-7d05c18a6999)

### 1. 使用 OpenCV 进行手部运动

[**akshaybahadur21/HandMovementTracking**](https://github.com/akshaybahadur21/HandMovementTracking)

*通过在 GitHub 上创建一个账户来为 HandMovementTracking 开发做出贡献。* [github.com](https://github.com/akshaybahadur21/HandMovementTracking)

来自 Akshay：

> 要进行视频跟踪，算法分析连续的视频帧并输出目标在帧之间的运动。各种算法各有优缺点。选择哪种算法时需要考虑其预期用途。视觉跟踪系统有两个主要组成部分：目标表示和定位，以及滤波和数据关联。
> 
> 视频跟踪是使用相机随时间定位移动物体（或多个物体）的过程。它有多种用途，包括：人机交互、安全与监控、视频通信与压缩、增强现实、交通控制、医学成像和视频编辑。

![](../Images/fd9958aece7bd34acb9d68b741ace2fd.png)

这是你需要的所有代码来重现它：

```py
import numpy as np
import cv2
import argparse
from collections import deque

cap=cv2.VideoCapture(0)

pts = deque(maxlen=64)

Lower_green = np.array([110,50,50])
Upper_green = np.array([130,255,255])
while True:
	ret, img=cap.read()
	hsv=cv2.cvtColor(img,cv2.COLOR_BGR2HSV)
	kernel=np.ones((5,5),np.uint8)
	mask=cv2.inRange(hsv,Lower_green,Upper_green)
	mask = cv2.erode(mask, kernel, iterations=2)
	mask=cv2.morphologyEx(mask,cv2.MORPH_OPEN,kernel)
	#mask=cv2.morphologyEx(mask,cv2.MORPH_CLOSE,kernel)
	mask = cv2.dilate(mask, kernel, iterations=1)
	res=cv2.bitwise_and(img,img,mask=mask)
	cnts,heir=cv2.findContours(mask.copy(),cv2.RETR_EXTERNAL,cv2.CHAIN_APPROX_SIMPLE)[-2:]
	center = None

	if len(cnts) > 0:
		c = max(cnts, key=cv2.contourArea)
		((x, y), radius) = cv2.minEnclosingCircle(c)
		M = cv2.moments(c)
		center = (int(M["m10"] / M["m00"]), int(M["m01"] / M["m00"]))

		if radius > 5:
			cv2.circle(img, (int(x), int(y)), int(radius),(0, 255, 255), 2)
			cv2.circle(img, center, 5, (0, 0, 255), -1)

	pts.appendleft(center)
	for i in xrange (1,len(pts)):
		if pts[i-1]is None or pts[i] is None:
			continue
		thick = int(np.sqrt(len(pts) / float(i + 1)) * 2.5)
		cv2.line(img, pts[i-1],pts[i],(0,0,225),thick)

	cv2.imshow("Frame", img)
	cv2.imshow("mask",mask)
	cv2.imshow("res",res)

	k=cv2.waitKey(30) & 0xFF
	if k==32:
		break
# cleanup the camera and close any open windows
cap.release()
cv2.destroyAllWindows()
```

是的，54 行代码。很简单，对吧？你需要在电脑上安装 OpenCV，如果你有 Mac，可以查看这个：

[**在 MacOS 上安装 OpenCV 3**](https://www.learnopencv.com/install-opencv3-on-macos/)

*在这篇文章中，我们将提供在 MacOS 和 OSX 上安装 OpenCV 3.3.0（C++ 和 Python）的逐步说明…*

如果你有 Ubuntu：

[**OpenCV：在 Ubuntu 上安装 OpenCV-Python**](https://docs.opencv.org/3.0-beta/doc/py_tutorials/py_setup/py_setup_in_windows/py_setup_in_windows.html)

*现在我们已经有了所有必要的依赖项，接下来安装 OpenCV。安装需要通过 CMake 配置。它…* [docs.opencv.org](https://docs.opencv.org/3.4.1/d2/de6/tutorial_py_setup_in_ubuntu.html)

如果你有 Windows：

[**在 Windows 上安装 OpenCV-Python - OpenCV 3.0.0-dev 文档**](https://docs.opencv.org/3.0-beta/doc/py_tutorials/py_setup/py_setup_in_windows/py_setup_in_windows.html)

*在本教程中，我们将学习如何在 Windows 系统上设置 OpenCV-Python。以下步骤已在 Windows 7-64 上测试过…*

### 2. 疲劳检测 OpenCV

[**akshaybahadur21/Drowsiness_Detection**](https://github.com/akshaybahadur21/Drowsiness_Detection)

*通过在 GitHub 上创建一个账户来为 Drowsiness_Detection 开发做出贡献。* [github.com](https://github.com/akshaybahadur21/Drowsiness_Detection)

这可以用于那些长时间驾驶的骑手，可能会导致事故。该代码可以检测你的眼睛，并在用户打瞌睡时发出警报。

**依赖**

1.  cv2

1.  immutils

1.  dlib

1.  scipy

**算法**

每只眼睛由6个（x，y）坐标表示，从眼睛的左上角开始（就像你在看那个人一样），然后顺时针环绕眼睛：

![](../Images/bbdf9b2032f999fdfb37fe73b2402e4d.png)

**条件**

它检查20个连续的帧，如果眼睛纵横比小于0.25，则会生成警报。

**关系**

![](../Images/e9960701bb337eb9d65d773675f4c540.png)

**总结**

![](../Images/2069cf3ed782cfb6cd957bbbf49fe34b.png)

![](../Images/bf8688f84fe8201275fb8cc0c24cc37e.png)

### 3. 使用Softmax回归进行数字识别

[**akshaybahadur21/Digit-Recognizer**](https://github.com/akshaybahadur21/Digit-Recognizer)

*Digit-Recognizer - 识别数字的机器学习分类器* [github.com](https://github.com/akshaybahadur21/Digit-Recognizer)

这段代码帮助你使用softmax回归分类不同的数字。你可以安装Conda来解决机器学习的所有依赖关系。

**描述**

Softmax回归（同义词：多项逻辑回归、最大熵分类器或多类逻辑回归）是逻辑回归的一种推广，我们可以用于多类分类（假设类别是相互排斥的）。相对而言，在二分类任务中，我们使用（标准）逻辑回归模型。

![](../Images/205b0cabc1ab47ddf01fa63dbb82487f.png)

**Python 实现**

使用的数据集是MNIST，图像大小为28 X 28，计划是使用逻辑回归、浅层网络和深度神经网络来分类0到9的数字。

这里最好的部分之一是他使用Numpy编码了三个模型，包括优化、前向和反向传播以及所有其他内容。

对于逻辑回归：[**查看代码**](https://gist.github.com/FavioVazquez/122e778c15e23d6c18969bc9e70cdbc5)

对于浅层神经网络：[**查看代码**](https://gist.github.com/FavioVazquez/6b4f476c3258a52b6accd842d57a3281)

最终使用深度神经网络：[**查看代码**](https://gist.github.com/FavioVazquez/45f182f79e2afc39ea0536ae2d118370)

**通过网络摄像头写入的执行**

要运行代码，输入`python Dig-Rec.py`

```py
python Dig-Rec.py
```

![](../Images/0bcd81e8fa9f98464832fc19bc07631a.png)

**通过网络摄像头显示图像的执行**

要运行代码，输入`python Digit-Recognizer.py`

```py
python Digit-Recognizer.py
```

![](../Images/c8360c56faba426c0dfada7fc591de13.png)

### Devanagiri 识别

[**akshaybahadur21/Devanagiri-Recognizer**](https://github.com/akshaybahadur21/Devanagiri-Recognizer)

*Devanagiri-Recognizer - 使用卷积网络的印地语字母分类器* [github.com](https://github.com/akshaybahadur21/Devanagiri-Recognizer)

这段代码帮助你使用卷积网络对印地语（Devanagiri）不同字母进行分类。你可以安装Conda来解决机器学习的所有依赖关系。

**使用的技术**

> 我使用了卷积神经网络。我使用Tensorflow作为框架，Keras API提供高级抽象。

**架构**

CONV2D → MAXPOOL → CONV2D → MAXPOOL → FC → Softmax → 分类

**一些额外的点**

1.  你可以添加额外的卷积层。

1.  添加正则化以防止过拟合。

1.  你可以添加额外的图像到训练集中，以提高准确性。

**Python实现**

数据集- DHCD（Devanagari字符数据集），图像大小为32 X 32，并使用卷积网络。

要运行代码，输入`python Dev-Rec.py`

```py
python Dev-Rec.py
```

![](../Images/7bc3ba69048c99f3fb0a415154c37d90.png)

### 4\. 使用FaceNet进行面部识别

[**akshaybahadur21/Facial-Recognition-using-Facenet**](https://github.com/akshaybahadur21/Facial-Recognition-using-Facenet)

[*Facial-Recognition-using-Facenet - 使用facenets实现面部识别。*](https://github.com/akshaybahadur21/Facial-Recognition-using-Facenet)

这段代码帮助进行面部识别，使用facenets（[https://arxiv.org/pdf/1503.03832.pdf](https://arxiv.org/pdf/1503.03832.pdf)）。facenets的概念最初在一篇研究论文中提出。主要概念涉及三元组损失函数，用于比较不同人的图像。这个概念使用了inception网络，源码来自于此，而fr_utils.py来自于deeplearning.ai作为参考。我添加了自己的一些功能，以提供稳定性和更好的检测。

**代码要求**

你可以为Python安装Conda，它解决了所有机器学习的依赖关系，你将需要：

```py
numpy
matplotlib
cv2
keras
dlib
h5py
scipy
```

**描述**

面部识别系统是一种技术，能够通过数字图像或视频源中的视频帧来识别或验证一个人。面部识别系统有多种工作方法，但一般来说，它们通过将选定的面部特征与数据库中的面部进行比较来工作。

**添加的功能**

1.  仅在你的眼睛睁开时检测面部。（安全措施）。

1.  使用dlib的面部对齐功能，以便在实时流媒体中有效预测。

**Python实现**

1.  使用的网络- Inception网络

1.  原始论文——Google的Facenet

**步骤**

1.  如果你想训练网络，运行`Train-inception.py`，然而你不需要这样做，因为我已经训练了模型并将其保存为`face-rec_Google.h5`文件，该文件在运行时会被加载。

1.  现在你需要在你的数据库中有图像。代码会检查`/images`文件夹。你可以将图片粘贴到那里，或者使用网络摄像头拍摄。为此，运行`create-face.py`，图像将存储在`/incept`文件夹中。你需要手动将它们粘贴到`/images`文件夹中。

1.  运行`rec-feat.py`来运行应用程序。

![](../Images/1f71c3e2a0a479ea1676da80444be5ff.png)

### 5\. Emojinator

[**akshaybahadur21/Emojinator**](https://github.com/akshaybahadur21/Emojinator)

[*Emojinator - 一个简单的表情符号分类器。*](https://github.com/akshaybahadur21/Emojinator)

这段代码帮助你识别和分类不同的表情符号。目前，我们仅支持手部表情符号。

**代码要求**

你可以为Python安装Conda，它解决了所有机器学习的依赖关系，你将需要：

```py
numpy
matplotlib
cv2
keras
dlib
h5py
scipy
```

**描述**

表情符号是用于电子消息和网页的表意符号和笑脸。表情符号存在于各种类型中，包括面部表情、常见物品、地点和天气类型以及动物。它们很像表情符号，但表情符号是实际的图片，而不是文字。

**功能**

1.  过滤器以检测手部。

1.  用于训练模型的 CNN。

**Python 实现**

1.  使用的网络 - 卷积神经网络

**过程**

1.  首先，你必须创建一个手势数据库。为此，运行`CreateGest.py`。输入手势名称，你将看到 2 帧显示。查看轮廓帧，并调整你的手，确保捕捉到手部特征。按 'c' 以捕捉图像。它将拍摄 1200 张同一手势的图像。尝试在帧内稍微移动手，以确保你的模型在训练时不会过拟合。

1.  对你想要的所有特征重复此操作。

1.  运行`CreateCSV.py`将图像转换为 CSV 文件

1.  如果你想训练模型，请运行‘TrainEmojinator.py’

1.  最后，运行`Emojinator.py`来通过网络摄像头测试你的模型。

**贡献者**

Akshay Bahadur 和 Raghav Patnecha。

![](../Images/bf56b24d5e89f9afd35f9cca9a979c8c.png)

### 结语

我只能说我对这些项目印象深刻，你可以在你的电脑上运行所有这些项目，或者更简单地在[Deep Cognition](http://deepcognition.ai/)平台上运行，如果你不想安装任何东西，并且它可以在线运行。

我想感谢 Akshay 和他的朋友们为这个伟大的开源贡献做出的努力，也感谢所有将来的贡献者。尝试这些，运行它们，获取灵感。这只是深度学习和计算机视觉能够做的惊人事物的一个小例子，最终由你来将其转化为能够帮助世界变得更美好的东西。

永不放弃，我们需要每个人对许多不同的事物感兴趣。我认为我们可以改变世界，使其变得更好，改善我们的生活、工作方式、思维方式和解决问题的方法。如果我们将所有现有资源集中起来，使这些知识领域为更大的利益而协作，我们可以对世界和我们的生活产生巨大的积极影响。

**我们需要更多感兴趣的人，更多课程，更多专业化，更多热情。我们需要你 :)**

感谢阅读。我希望你在这里找到了一些有趣的东西 :)

如果你有问题，可以在 Twitter 上添加我：

[**Favio Vázquez (@FavioVaz) | Twitter**

*Favio Vázquez (@FavioVaz) 最新的推文。数据科学家。物理学家和计算工程师。我有一…*twitter.com](https://twitter.com/FavioVaz)

和 LinkedIn:

[**Favio Vázquez — 首席数据科学家 — OXXO | LinkedIn**

*查看 Favio Vázquez 在 LinkedIn 的个人资料，全球最大的专业社区。Favio 列出了 15 个工作…*linkedin.com](http://linkedin.com/in/faviovazquez/)

那里见 :)

**简历: [Favio Vazquez](https://www.linkedin.com/in/faviovazquez/)** 是一位物理学家和计算机工程师，专注于数据科学和计算宇宙学。他对科学、哲学、编程和音乐充满热情。目前，他担任 Oxxo 的首席数据科学家，致力于数据科学、机器学习和大数据。他还是 Ciencia y Datos 的创始人，这是一个西班牙语的数据科学出版物。他喜欢接受新挑战，与优秀的团队合作，并解决有趣的问题。他参与了 Apache Spark 的合作，帮助维护 MLlib、核心库和文档。他热衷于将自己的科学知识、数据分析、可视化和自动学习的专长应用于帮助世界变得更美好。

[原文](https://towardsdatascience.com/diy-deep-learning-projects-c2e0fac3274f)。经许可转载。

**相关:**

+   [一个“奇怪”的深度学习入门](//2018/03/weird-introduction-deep-learning.html)

+   [成为数据科学家的两面性](/2018/03/two-sides-getting-job-data-scientist.html)

+   [我深入学习深度学习的历程](/2018/01/journey-into-deep-learning.html)

### 更多相关话题

+   [停止学习数据科学以寻找目的，并寻找目的以……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [一个 90 亿美元的人工智能失败案例分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么让 Python 成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每个数据科学家都应该知道的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
