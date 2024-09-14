# 使用 5 行代码更改任何视频的背景

> 原文：[https://www.kdnuggets.com/2020/12/change-background-video-5-lines-code.html](https://www.kdnuggets.com/2020/12/change-background-video-5-lines-code.html)

[评论](#comments)

**由 [Ayoola Olafenwa](https://www.linkedin.com/in/ayoola-olafenwa-003b901a9/)，独立 AI 研究员**

![图示](../Images/c90ef44af509ee9620dd55a0fd935595.png)

作者提供的照片

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

PixelLib 是一个库，旨在实现对象分割的简单实现。PixelLib 支持图像调优，即能够更改任何图像的背景。PixelLib 现在还支持视频调优，即能够更改视频和相机的背景。PixelLib 采用对象分割技术，进行出色的前景和背景分离。PixelLib 使用经过 pascalvoc 数据集训练的 deeplabv3+ 模型，该数据集支持 20 种对象类别。

```py
person,bus,car,aeroplane, bicycle, ,motorbike,bird, boat, bottle,  cat, chair, cow, dinningtable, dog, horse pottedplant, sheep, sofa, train, tv
```

*背景效果支持如下：*

**1** 使用图片更改图像的背景

**2** 为图像和视频的背景分配独特的颜色。

**3** 模糊图像和视频的背景。

**4** 将图像和视频的背景转为灰度。

**5** 为视频创建虚拟背景。

安装 PixelLib 及其依赖项：

使用以下命令安装 Tensorflow：（PixelLib 支持 Tensorflow 2.0 及以上版本）

+   *pip3 install tensorflow*

安装 PixelLib 使用

+   pip3 install pixellib

如果已安装，请使用以下命令升级到最新版本：

+   pip3 install pixellib — 升级

### 目标对象检测

在某些应用中，您可能希望检测图像或视频中的特定对象。deeplab 模型默认检测图像或视频中所有支持的对象。现在可以过滤掉未使用的检测，专注于图像或视频中的特定对象。

*示例图像*

![图示](../Images/3844ccd3c79e2702e0710a8e6bb650ef.png)

来源：[Unsplash.com](https://unsplash.com/photos/D5mCL7Q_6Us) 由 Strvnge Films 提供

我们打算模糊上图的背景。

模糊图像背景的代码。

输出图像

![帖子图片](../Images/dd2a391896609ec6e19d2b2ecc8882b8.png)

我们的目标是完全模糊图像中人物的背景，但对于其他对象的存在感到不满意。因此，需要修改代码以检测目标对象。

代码仍然相同，只是我们在*blur_bg*函数中引入了一个额外的参数***detect***。

```py
change_bg.blur_bg("sample.jpg", extreme = True, output_image_name="output_img.jpg", detect = "person")
```

**detect: **这是确定要检测的目标对象的参数。detect的值设置为***person***。这意味着模型将仅检测图像中的人。

![用于帖子的图像](../Images/af4678c17099777cc6af3f59ac414219.png)

这是仅显示我们目标对象的新图像。

如果我们打算仅显示图像中存在的汽车，只需将**detect**的值从***person***更改为***car***。

```py
change_bg.blur_bg("sample.jpg", extreme = True, output_image_name="output_img.jpg", detect = "car")
```

![用于帖子的图像](../Images/c831882ee424554647b31976a41d2941.png)

我们将目标对象设置为***car***，并且图像中存在的其他对象都与背景一起被模糊。

***目标对象的彩色背景***

可以通过颜色效果进行目标检测。

```py
change_bg.color_bg("sample.jpg", colors = (0,128,0), output_image_name="output_img.jpg", detect = "person")
```

![用于帖子的图像](../Images/b849e912cc6dfe47cfae912cb1d5cbd9.png)

***用新图片更改目标对象的背景***

***背景图像***

![图](../Images/ccef7e7882379916a9c419cb6457e54e.png)

来源：[Unsplash.com by Dawid Zawila](https://unsplash.com/photos/9P2-bzjvIHk)

```py
change_bg.change_bg_img("sample.jpg", "background.jpg", output_image_name="output_img.jpg", detect = "person")
```

![用于帖子的图像](../Images/1602d3c22c22d9579a983ef93eeb1d1f.png)

***将目标对象的背景转换为灰度***

```py
change_bg.gray_bg("sample.jpg", output_image_name="output_img.jpg", detect = "person")
```

![用于帖子的图像](../Images/c08f9107b96b6938bb4eb9e9dfca052b.png)

阅读这篇文章可以全面了解如何使用PixelLib编辑图像背景。

[**用5行代码更改任何图像的背景**](https://towardsdatascience.com/change-the-background-of-any-image-with-5-lines-of-code-23a0ef10ce9a)

### ***PixelLib的视频调节***

视频调节是改变任何视频背景的能力。

**模糊视频背景**

PixelLib使得只需五行代码即可方便地模糊任何视频的背景。

**sample_video**

*模糊视频文件背景的代码*

```py
import pixellib                       
from pixellib.tune_bg import alter_bg  

change_bg = alter_bg(model_type = "pb")
change_bg.load_pascalvoc_model("xception_pascalvoc.pb")
```

我们导入了pixellib，并从pixellib中导入了*alter_bg*类。创建了该类的实例，并在类内添加了一个参数*model_type*并将其设置为***pb***。最后，我们调用了函数来加载模型。

**注意：** PixelLib支持两种类型的deeplabv3+模型，keras和tensorflow模型。keras模型是从tensorflow模型的检查点提取的。tensorflow模型的性能优于从检查点提取的keras模型。我们将使用tensorflow模型。从[这里](https://github.com/ayoolaolafenwa/PixelLib/releases/download/1.1/xception_pascalvoc.pb)下载tensorflow模型。

有三个参数决定背景的模糊程度。

*low:* 当设置为true时，背景会被轻微模糊。

*moderate:* 当设置为true时，背景会被中等程度地模糊。

*extreme:* 当设置为 true 时，背景会被深度模糊。

```py
change_bg.blur_video("sample_video.mp4", extreme = True, frames_per_second=10, output_video_name="blur_video.mp4", detect = "person")
```

这是模糊视频背景的代码行。这个函数接受五个参数：

**video_path:** 这是要模糊其背景的视频文件的路径。

**extreme:** 设置为 true 时，视频背景将被极度模糊。

**frames_per_second:** 这是设置输出视频文件每秒帧数的参数。在这种情况下，它设置为 10，即保存的视频文件将每秒有 10 帧。

**output_video_name:** 这是保存的视频。输出视频将保存在当前工作目录中。

**detect:** 这是选择视频中目标对象的参数。它设置为 ***person***。

**输出视频**

**模糊相机画面的背景**

```py
import cv2capture = cv2.VideoCapture(0)
```

我们导入了 cv2 并包含了捕获相机画面的代码。

```py
change_bg.blur_camera(capture, extreme = True, frames_per_second= 5, output_video_name=”output_video.mp4", show_frames= True,frame_name= “frame”, detect = "person")
```

在模糊相机画面的代码中，我们将视频的文件路径替换为捕获，即我们将处理相机画面的流而不是视频文件。我们添加了额外的参数来显示相机画面：

**show_frames:** 这是处理显示模糊相机画面的参数。

**frame_name:** 这是显示的相机画面的名称。

**输出视频**

哇！PixelLib 成功模糊了视频中的背景。

### **为视频创建虚拟背景**

PixelLib 使为任何视频创建虚拟背景变得非常简单，你可以使用任何图像为视频创建虚拟背景。

**示例视频**

**用作视频背景的图像**

![图像](../Images/c5bdc32590fbb45357cd1a3591bdaa5f.png)

来源: [Unsplash.com](https://unsplash.com/photos/rCbdp8VCYhQ) [由 Handy Holmes](https://unsplash.com/photos/rCbdp8VCYhQ)

```py
change_bg.change_video_bg(“sample_video.mp4”, “bg.jpg”, frames_per_second = 10, output_video_name=”output_video.mp4", detect = “person”)
```

代码仍然相同，只是我们调用了函数 *change_video_bg* 来为视频创建虚拟背景。该函数接受我们想用作视频背景的图像路径。

**输出视频**

> 漂亮的演示！我们成功地为视频创建了虚拟空间背景。

**为相机画面创建虚拟背景**

```py
change_bg.change_camera_bg(cap, “space.jpg”, frames_per_second = 5, show_frames=True, frame_name=”frame”, output_video_name=”output_video.mp4", detect = “person”)
```

这与我们用于模糊相机画面的代码类似。唯一的区别是我们调用了函数 *change_camera_bg*。我们执行了相同的例程，替换了视频的文件路径为捕获，并添加了相同的参数。

**输出视频**

> 哇！PixelLib 成功为我的视频创建了虚拟背景。

**视频背景着色**

PixelLib 使为视频背景分配任意颜色成为可能。

*给视频文件背景着色的代码*

```py
change_bg.color_video("sample_video.mp4", colors =  (0, 128, 0), frames_per_second=15, output_video_name="output_video.mp4", detect = "person")
```

代码还是相同的，只是我们调用了函数 *color_video* 给视频背景上色。函数 *color_bg* 采用参数 *colors*，而颜色的 RGB 值设置为绿色。绿色的 RGB 值是 (0, 128, 0)。

输出视频

```py
change_bg.color_video("sample_video.mp4", colors =  (0, 128, 0), frames_per_second=15, output_video_name="output_video.mp4", detect = "person")
```

同样的视频，背景为白色

**为相机的画面着色背景**

```py
change_bg.color_camera(capture, frames_per_second=10,colors = (0, 128, 0), show_frames = True, frame_name = “frame”, output_video_name=”output_video.mp4", detect = "person")
```

这与我们用于创建相机画面虚拟背景的代码类似。唯一的不同是我们调用了函数 *color_camera*。我们进行了相同的操作，将视频的文件路径替换为捕捉，并添加了相同的参数。

**输出视频**

> 漂亮的演示！我的背景成功地用 PixelLib 变成了绿色。

**灰度视频背景**

*将视频文件的背景转换为灰度代码*

```py
change_bg.gray_video("sample_video.mp4", frames_per_second=10, output_video_name="output_video.mp4", detect = "person")
```

输出视频

**注意:** 视频的背景将被改变，对象保持其原始质量。

**灰度化相机的画面背景**

这与我们用来为相机画面上色的代码类似。唯一的不同是我们调用了函数 *gray_camera*。我们进行了相同的操作，将视频文件路径替换为捕捉，并添加了相同的参数。

> [访问 PixelLib 官方 GitHub 仓库](https://github.com/ayoolaolafenwa/PixelLib)
> 
> [访问 PixelLib 官方文档](https://pixellib.readthedocs.io/en/latest/)

通过以下方式联系我：

电子邮件: [olafenwaayoola@gmail.com](https://mail.google.com/mail/u/0/#inbox)

LinkedIn: [Ayoola Olafenwa](https://www.linkedin.com/in/ayoola-olafenwa-003b901a9/)

推特: [@AyoolaOlafenwa](https://twitter.com/AyoolaOlafenwa)

Facebook: [Ayoola Olafenwa](https://web.facebook.com/ayofen)

查看这些文章，了解如何使用 PixelLib 对图像和视频中的对象进行语义和实例分割。

[**使用 5 行代码进行图像分割**](https://towardsdatascience.com/image-segmentation-with-six-lines-0f-code-acb870a462e8)

使用 PixelLib 进行语义和实例分割。

[**使用 5 行代码进行视频分割**](https://towardsdatascience.com/video-segmentation-with-5-lines-of-code-87f798afb93)

视频的语义和实例分割。

[**使用 5 行代码对 150 类对象进行语义分割**](https://towardsdatascience.com/semantic-segmentation-of-150-classes-of-objects-with-5-lines-of-code-7f244fa96b6c)

使用 PixelLib 对 150 类对象进行语义分割

[**使用 7 行代码进行自定义实例分割训练**](https://towardsdatascience.com/custom-instance-segmentation-training-with-7-lines-of-code-ff340851e99b)

使用 7 行代码训练数据集，实现实例分割和目标检测。

**简介: [Ayoola Olafenwa](https://www.linkedin.com/in/ayoola-olafenwa-003b901a9/)** 是一位独立的人工智能研究员，专注于计算机视觉领域。

[原文](https://towardsdatascience.com/change-the-background-of-any-video-with-5-lines-of-code-7cc847394f5d)。已获授权转载。

**相关:**

+   [用 5 行代码更改任何图像的背景](/2020/11/change-background-image-5-lines-code.html)

+   [计算机视觉路线图](/2020/10/roadmap-computer-vision.html)

+   [使用深度学习自动旋转图像](/2020/07/auto-rotate-images-deep-learning.html)

### 更多相关话题

+   [NExT-GPT介绍：任意对任意的多模态大语言模型](https://www.kdnuggets.com/introduction-to-nextgpt-anytoany-multimodal-large-language-model)

+   [少于15行代码实现多模态深度学习](https://www.kdnuggets.com/2023/01/predibase-multi-modal-deep-learning-less-15-lines-code.html)

+   [如何从不同背景过渡到数据科学领域？](https://www.kdnuggets.com/2023/05/transition-data-science-different-background.html)

+   [SHAP：用Python解释任何机器学习模型](https://www.kdnuggets.com/2022/11/shap-explain-machine-learning-model-python.html)

+   [如何在没有工作经验的情况下获得数据科学的第一份工作](https://www.kdnuggets.com/2021/02/first-job-data-science-without-work-experience.html)

+   [在你参加任何免费数据科学课程之前，请先阅读此文](https://www.kdnuggets.com/read-this-before-you-take-any-free-data-science-course)
