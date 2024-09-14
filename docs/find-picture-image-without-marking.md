# 在图像中找到未标记的图片

> 原文：[https://www.kdnuggets.com/2022/09/find-picture-image-without-marking.html](https://www.kdnuggets.com/2022/09/find-picture-image-without-marking.html)

![在图像中找到未标记的图片](../Images/c8e88b597a7b216d33b872bae4f8a214.png)

作者提供的图片 | 编辑者编辑

我们经常看到图像中的图片：例如，漫画将几张图片合并成一张。如果你有一个娱乐应用程序，用户在其中发布迷因，就像我们的iFunny，你会经常遇到这种情况。神经网络已经能够找到动物、人或其他物体，但如果我们需要在图像中找到另一张图片怎么办？让我们更详细地了解我们的算法，以便你可以在Google Colaboratory中使用[一个笔记本](https://colab.research.google.com/drive/1u42OGNz4OEZCb7RakFhw6TETWpLCG3sb?usp=sharing)进行测试，甚至在你的项目中实现它。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全领域

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT需求

* * *

所以，迷因。许多迷因由图片及其标题组成，前者作为语言笑话的背景：

![在图像中找到未标记的图片](../Images/9aa85b245aa2501e310b9c303a932d6e.png)

一张迷因可能包含多个带有文字的图片：

![在图像中找到未标记的图片](../Images/221b84c117a986122e16da9fc4b989b1.png)

一种流行的迷因类型是社交网络帖子截图，例如来自Twitter。图片在其中占据了相对较小的区域。

![在图像中找到未标记的图片](../Images/faab49e34222707e3e662b92e57e6c00.png)

最初，寻找图片的任务出现在我们发送推送通知的时候。在Android全面页面推送消息出现之前，这些消息中的所有图片都非常小。文字难以阅读（那用户需要它干嘛？），通知图片中的所有对象几乎没有信息量。它们也不够吸引人。

我们决定押注于迷因中的图片。为了增强视觉效果，我们制作了一个算法，可以裁剪图片周围的所有框架，无论上面显示了什么。

## 让我们通过几个例子来看看算法的实际效果

以下是算法如何处理黑色背景和应用程序水印的方式：

![在图像中查找图片而不标记](../Images/9aa85b245aa2501e310b9c303a932d6e.png) ![在图像中查找图片而不标记](../Images/6f80913f6e05d01a8f9123ed967679d6.png)

在以下示例中，尽管它们之间有一条大线，但算法并未将这些图像分开。

![在图像中查找图片而不标记](../Images/af311ed5f6a73eea5c7fe0dd7d1dccc1.png) ![在图像中查找图片而不标记](../Images/181c8402915f6cdc65961e90522a56ea.png)

该方法也适用于占据整个图像约50%面积的大型竖直文本。

![在图像中查找图片而不标记](../Images/221b84c117a986122e16da9fc4b989b1.png) ![在图像中查找图片而不标记](../Images/1f277aa6d4cc39e6d06c80e8feb20a62.png)

*然而，算法未能识别应用水印（仍需改进）。*

算法在处理以下示例时表现出色，即使目标图像在整个表情包中所占比例相对较小。算法还修剪了侧边的白色边框。

![在图像中查找图片而不标记](../Images/faab49e34222707e3e662b92e57e6c00.png) ![在图像中查找图片而不标记](../Images/83034fad8066afe3ff6b39e0a915cfb5.png)

在下一个示例中，图像中同时有两个背景（白色和黑色），但算法成功处理了这一情况。然而，它保留了侧边的白色边框。

![在图像中查找图片而不标记](../Images/70bdcaa9b72cff3264fadf36ae2bce1f.png) ![在图像中查找图片而不标记](../Images/2ec933a697142f461d77b86f81f04bd9.png)

最后一个例子特别有趣，因为图像本身包含许多线条。但即便如此，也未能困扰我们的算法。上边界比我们期望的稍大，但对于如此困难的情况来说，这是一个很好的结果！

![在图像中查找图片而不标记](../Images/5431b4496b7a25f47b04adebff729b34.png) ![在图像中查找图片而不标记](../Images/15a5f532b42c120e31830db9b0100f5b.png)

请回忆一下，这些结果是在没有标记的情况下取得的。

# 算法

跳过代码中无关紧要的部分。你可以在算法的完整[实现](https://colab.research.google.com/drive/1u42OGNz4OEZCb7RakFhw6TETWpLCG3sb?usp=sharing)中找到并运行这些部分。在这篇文章中，我们将重点关注主要思想。

以这张小狗的图片为例。

![在图像中查找图片而不标记](../Images/2dd41777378e3e81cab6cffd74e02b75.png)

它在顶部有文本和一条白色背景的小条纹，以及底部的应用水印。

为了仅获取小狗的图片，我们需要去除上下两个水平矩形。基本思路是使用[霍夫变换](https://en.wikipedia.org/wiki/Hough_transform)识别图像中的所有直线，然后选择那些与白色或黑色单色区域相邻的直线，我们将这些区域视为背景。

## 第1步。将图像转换为灰度图像

图像处理的第一步是将其转换为单色格式。有很多方法可以实现这一点，但我更喜欢获取[Lab](https://en.wikipedia.org/wiki/CIELAB_color_space)颜色空间中的L通道（亮度）。这种方法在大多数情况下被证明是最好的。不过，你可以尝试其他方法，比如[HSV](https://en.wikipedia.org/wiki/HSL_and_HSV)颜色空间中的V通道（值）。

为了消除图像中产生不必要线条的强对比度差异，我们对生成的黑白图像应用高斯滤波器。

```py
img_gray = cv2.cvtColor(img, cv2.COLOR_RGB2LAB)[:, :, 0]

img_blur = cv2.GaussianBlur(img_gray, (9, 9), 0)
```

这里我们使用了OpenCV库中的函数。除了图像外，cv2.GaussianBlur函数接受高斯内核大小（宽度，高度）和X轴上的标准差（默认情况下，Y轴为0）。当使用0作为标准差时，它会根据内核的大小进行[计算](https://docs.opencv.org/4.x/d4/d86/group__imgproc__filter.html#gac05a120c1ae92a6060dd0db190a61afa)。我们使用的参数基于主观经验。

![在图像中查找图片而不标记](../Images/99abc872fafeeb532d2a0308fe09852a.png)

这是我们目前得到的结果。

你可以从[scikit-image](https://scikit-image.org)库中找到并使用类似的方法，但要小心，因为它们在结果和输入参数上有所不同。[skimage.color.rgb2lab](https://scikit-image.org/docs/stable/api/skimage.color.html#skimage.color.rgb2lab)函数的亮度通道结果在[0,100]范围内，高斯滤波器[skimage.filters.gaussian](https://scikit-image.org/docs/stable/api/skimage.filters.html#skimage.filters.gaussian)对内核参数更为敏感，这会影响最终结果。

## 第2步。检测边缘

霍夫变换是我们算法的基础，只能处理由背景和边缘组成、值为0和1的二值图像。

我们使用[边缘检测算法](https://opencv24-python-tutorials.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_canny/py_canny.html)来创建二值图像。简而言之，该算法计算像素坐标的强度函数的梯度，并找到其局部最大值，这些区域被定义为所需的边缘。

![在图像中查找图片而不标记](../Images/6e20048a154222c90e73f8908e79705e.png)

变换结果

我们使用来自[scikit-image](https://scikit-image.org)库的 Feature 模块中的 Canny 函数来执行变换。你可以在 OpenCV 库中找到类似的函数，但它的实现包括一个 5x5 的高斯滤波器平滑，这使得控制情况有点困难。

```py
img_canny = feature.canny(img_blur) * 1

```

## 第 3 步：Hough 变换

现在我们需要在找到的所有线条中检测直线。Hough 的直线变换在这方面表现出色。算法在原点的给定距离 ? 处和 X 轴的某个角度 ? 绘制一条直线，如下所示。

![在图像中找到未标记的图片](../Images/b25bfa629d23e33535a3584b20ed8e4b.png)

对于每条这样的线，它计算位于绘制直线上的二值图像的亮像素数量。这个过程会对选定范围内的所有 ? 和 ? 值重复进行。结果是 (?, ?) 坐标系中的强度图。因此，当绘制的直线与图像中的线条重合时，结果图中的极大值将会出现。![在图像中找到未标记的图片](../Images/b01b061c663c9c41883a58d77b69e3cc.png)

右图中是对左侧图中两条黑线的 Hough 变换的示意图。

在我们的算法中，我们使用[这个](https://scikit-image.org/docs/stable/auto_examples/edges/plot_line_hough_transform.html)来自 scikit-image 的实现，因为它更易于配置。此外，这个库提供了一个方便的方法来选择最类似于直线的局部极大值。OpenCV 的[实现](https://opencv24-python-tutorials.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_houghlines/py_houghlines.html)没有类似的功能。

```py
tested_angles = np.linspace(-1.6, 1.6, 410)
h, theta, d = transform.hough_line(img_canny, theta=tested_angles)
_, angles, dists = transform.hough_line_peaks(h, theta, d)
```

变换输出了找到的线条的角度和距离。绘制这些线条比我们习惯的直线要复杂一些，因此这里有一个额外的代码片段。

```py
plt.figure(figsize=(10, 10))
plt.imshow(img_gray, cmap="gray")

x = np.array((0, img_gray.shape[1]))
ys = []
for angle, dist in zip(angles, dists):    
    y0, y1 = (dist - x * np.cos(angle)) / np.sin(angle)
    y = int(np.mean([y0, y1]))
    ys += [y]
    plt.plot(x, [y, y])

ys = np.array(ys)
plt.axis("off");
```

计算得到的距离是斜边，而直线在 y 轴上的位置是直角边，因此我们将距离值除以所得角度的正弦值。

![在图像中找到未标记的图片](../Images/9e192008b469877c437bb925df6a153a.png)

运行此代码后，你将看到以下内容：

我们的算法找到了所有必要的线条！应用水印的边界也被检测到。

## 第 4 步：直线分类

最后，我们需要确定哪一条是底部线，哪一条是顶部线，同时去除不必要的线条，例如黑条上方的那一条。

![在图像中找到未标记的图片](../Images/e5ee01f7e7c83b4e391967bfbaf01842.png)

最终结果

为了解决这个问题，我们添加了对找到的线条上下区域的检查。由于我们处理的是表情包及其字幕，我们期望图像外的背景是白色的，而文字是黑色的。或者反过来：黑色背景和白色文字。

因此，我们的算法基于检查分离区域中白色和黑色像素的百分比。

```py
props = {
    "white_limit": 230,
    "black_limit": 35,
    "percent_limit": 0.8
}

areas_h = []
for y in ys:
  percent_up = np.sum((img[:y] > props['white_limit']) + (img[:y] < props['black_limit'])) / (y * img.shape[1] * 3)
  percent_down = np.sum((img[y:] > props['white_limit']) + (img[y:] < props['black_limit'])) / ((img.shape[0] - y) * img.shape[1] * 3)
  areas_h += [-1 * (percent_up > props['percent_limit']) + (percent_down > props['percent_limit']) * 1]
```

在这种方法中，我们使用了3个变量：

+   white_limit是定义白色的下限。所有值在(230; 256)范围内的像素将被视为白色。

+   black_limit是定义黑色的上限。所有值在(0; 35)范围内的像素将被视为黑色。

+   percent_limit是将区域视为带有文本的背景时白色/黑色像素的最小百分比。

此外，比较在每个颜色通道中独立进行，这是一种通用的条件，允许你正确处理稀有的例外情况。

接下来的几行代码选择了上限中的最低线和下限中的最高线。我们还添加了一个关于所选区域大小的条件。如果结果的垂直线裁剪掉了图像高度的超过60%，我们将其视为错误，并返回原始图像的顶部或底部边框。这个参数是基于对表情包图像占用区域的假设来选择的。

```py
cut_threshold = 0.6
y0 = np.max(ys[areas_h == -1])
if y0 > cut_threshold * img.shape[0]:
    y0 = 0
y1 = np.min(ys[areas_h == 1])
if y1 < (1 - cut_threshold) * img.shape[0]:
    y1 = img.shape[0]
```

最终算法，你可以在[这里](https://colab.research.google.com/drive/1u42OGNz4OEZCb7RakFhw6TETWpLCG3sb?usp=sharing)的Functions部分找到，还包括了寻找和调整垂直边框的功能，让你能够去除图像两侧的边框。

# 接下来是什么？

为了改善结果，你需要学习如何测量质量，而这需要标记。如果你想创建一个训练数据集，我们的算法将使你的手动标记更容易，从而使你能更快地收集到所需数量的样本。

为了使我们的方法适用于更多情况，你可以将算法中的背景色检查替换为单色检查。这样，无论背景色是什么，方法都可以使用。

[**亚罗斯拉夫·穆尔扎耶夫**](https://funcorp.dev/blog)是Funcorp的数据科学家。

### 更多相关内容

+   [停止学习数据科学以寻找目标并寻找目标…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [如何在没有任何工作经验的情况下获得你的第一份数据科学工作](https://www.kdnuggets.com/2021/02/first-job-data-science-without-work-experience.html)

+   [Python 字符串匹配无复杂 RegEx 语法](https://www.kdnuggets.com/2023/02/python-string-matching-without-complex-regex-syntax.html)

+   [如何像老板一样进行 MLOps：无泪的机器学习指南](https://www.kdnuggets.com/2023/06/mlops-like-boss-guide-machine-learning-without-tears.html)

+   [如何找到最佳的数据科学远程工作](https://www.kdnuggets.com/2022/12/find-best-data-science-remote-jobs.html)

+   [快速指南：如何找到合适的标注人才](https://www.kdnuggets.com/2022/04/quick-guide-find-right-minds-annotation.html)
