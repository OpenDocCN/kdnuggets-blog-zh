# 如何将RGB图像转换为灰度图像

> 原文：[https://www.kdnuggets.com/2019/12/convert-rgb-image-grayscale.html](https://www.kdnuggets.com/2019/12/convert-rgb-image-grayscale.html)

有时需要将彩色图像转换为灰度图像。这一需求在加载火星表面拍摄的图像时出现，作为[端到端机器学习课程313，高级神经网络方法](https://end-to-end-machine-learning.teachable.com/p/advanced-neural-network-methods)的一部分。我们处理了混合的彩色和灰度图像，需要将它们转换为统一的格式——全部为灰度图像。我们将使用Python，结合Pillow、Numpy和Matplotlib包。

顺便提一下，本帖子中的所有有趣信息都来自[维基百科上的灰度图](https://en.wikipedia.org/wiki/Grayscale)。（如果你觉得有帮助，[也许可以给他们捐一美元](https://donate.wikimedia.org/w/index.php?title=Special:LandingPage&country=US&uselang=en&utm_medium=sidebar&utm_source=donate&utm_campaign=C13_en.wikipedia.org)。）

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

# 读取彩色图像

该[代码](https://github.com/brohrer/cottonwood_martian_images/blob/master/image_loader.py)用于加载jpeg图像，供自编码器作为输入。这通过使用Pillow和Numpy实现：

```py
from PIL import Image
import numpy as np
color_img = np.asarray(Image.open(img_filename)) / 255
```

这将图像读取并转换为Numpy数组。有关此过程的详细描述，请查看上一篇文章：[如何将图片转换为数字](https://brohrer.github.io/images_to_numbers.html)。对于灰度图像，结果是一个二维数组，行数和列数等于图像的像素行数和列数。较低的数值表示较暗的阴影，较高的数值表示较亮的阴影。像素值的范围通常是0到255。我们将其除以255，以获得0到1的范围。

彩色图像表示为三维Numpy数组——由三个二维数组组成，分别对应红色、绿色和蓝色通道。每个数组像灰度数组一样，每个像素一个值，其范围相同。

![三维图像数组结构](../Images/4ca9846e2120338949d3ec950a8e487e.png)

![图示](../Images/b61919db92ac4e6ae4bab1f098d97892.png)

图片来源：黛安·罗赫

# 简单易行：平均化通道

将彩色图像 3D 数组转换为灰度 2D 数组的直观方法是，对于每个像素，取红色、绿色和蓝色像素值的平均值以获得灰度值。这将每个颜色通道贡献的亮度或 *亮度* 合并成一个合理的灰色近似值。

```py
img = numpy.mean(color_img, axis=2)
```

`axis=2` 参数告诉 `numpy.mean()` 对所有三个颜色通道进行平均。 (`axis=0` 会在像素行之间平均，`axis=1` 会在像素列之间平均。)

![通过平均红色、绿色和蓝色通道得到的灰色](../Images/905b2292724d8aa2c01f659a0670ba6b.png)

# 好吧，实际上……通道相关的亮度感知

在我们眼中，绿色看起来比蓝色亮大约十倍。通过许多精心设计的实验，心理学家已经弄清楚了我们如何感知红色、绿色和蓝色的亮度差异。他们为我们的通道平均提供了一套不同的权重，以获得总亮度。

[![红色、绿色和蓝色通道组合的方程](../Images/07e545e6886030e0ab53c00977c5dc85.png)](https://en.wikipedia.org/wiki/Grayscale#Colorimetric_(perceptual_luminance-preserving)_conversion_to_grayscale)

结果明显不同，对我来说，更加准确。

![通过加权平均红色、绿色和蓝色通道得到的灰色](../Images/5a1bcd7bc191e37523bd1f6dbb96e6f8.png)

# 好吧，实际上……Gamma 压缩

当亮度较低时，我们能够看到细微的差异，但在高亮度水平下，我们对这些差异的敏感度要低得多。为了避免在高亮度下浪费精力表示不可察觉的差异，颜色尺度被扭曲，使其在范围的低端集中更多值，而在高端则分布得更广。这就是 gamma 压缩。

在计算灰度亮度之前，为了撤销 gamma 压缩的效果，有必要应用逆操作，即 gamma 扩展：

[![Gamma 的非线性变换](../Images/2dc74756a003859bbd1e9b226f13bf20.png)](https://en.wikipedia.org/wiki/Grayscale#Colorimetric_(perceptual_luminance-preserving)_conversion_to_grayscale)

![Gamma 压缩函数](../Images/f5e98853566862762b4c0d81947a0c47.png)

Gamma 压缩的好处在于它消除了在平滑变化的暗色中出现的带状效果，比如黄昏时天空的照片。缺点是如果我们要做加法、减法或平均值等操作，我们必须先撤销压缩，将亮度恢复为线性表示。

![考虑 gamma 压缩的灰度](../Images/cba8097bba797dcffc9d6c8c8374fc0b.png)

在考虑 gamma 压缩后，图像的亮度得到增强，使其更接近原始图像的亮度。最终，我们得到高质量的灰度表示。

# 好吧，实际上……线性近似

相比于之前我们使用的加权平均，伽马解压和重新压缩的计算成本非常高。有时，速度比尽可能准确的亮度计算更为重要。对于这种情况，有一种线性近似方法：

![组合红色、绿色和蓝色通道的公式](../Images/07a95fe9ad4c404e73d78bcba841f190.png)

这可以让你得到一个更接近伽马压缩校正版本的结果，但不需要额外的计算时间。

![考虑伽马压缩的灰度线性近似](../Images/3e371291a241ad99f52ca0f9aa662f7f.png)

正如你所看到的，结果一点也不好。它们往往稍显偏暗，尤其是在红色中间值范围，但在大多数实际方面可以说也非常好。

这种亮度计算方法在标准 [ITU-R BT.601 Studio encoding parameters of digital television for standard 4:3 and wide screen 16:9 aspect ratios.](https://www.itu.int/dms_pubrec/itu-r/rec/bt/R-REC-BT.601-7-201103-I!!PDF-E.pdf)中进行了规范，该标准在1983年获得了艾美奖。

# 我该使用哪个？

如果接近就足够好，或者你真的关心速度，可以使用伽马校正的线性近似。这是 [MATLAB](https://www.mathworks.com/help/matlab/ref/rgb2gray.html)、 [Pillow](https://pillow.readthedocs.io/en/3.1.x/reference/Image.html) 和 [OpenCV](https://github.com/opencv/opencv/blob/8c0b0714e76efef4a8ca2a7c410c60e55c5e9829/modules/imgproc/src/color.simd_helpers.hpp) 使用的方法。它包含在我的 [Lodgepole 图像和视频处理工具箱](https://github.com/brohrer/lodgepole/blob/master/lodgepole/image_tools.py)中：

```py
import lodgepole.image_tools as lit
gray_img = lit.rgb2gray_approx(color_img)
```

但如果你确实追求最佳结果，可以花费在整个伽马解压 - 视觉亮度校正 - 伽马重新压缩流程上：

```py
import lodgepole.image_tools as lit
gray_img = lit.rgb2gray(color_img)
```

如果阅读到这里你仍坚持将三个通道直接平均，我会对你做出评判。

现在去制作美丽的灰度图像吧！

[原文](https://brohrer.github.io/convert_rgb_to_grayscale.html)。经允许转载。

[**布兰登·罗赫**](https://www.linkedin.com/in/brohrer/) 是 LinkedIn 的高级机器学习工程师。布兰登的专长是创建算法和计算方法。你可以在 [这里](https://brandonrohrer.com/portfolio) 找到他的工作实例。

### 更多相关主题

+   [如何使用 ChatGPT 将文本转换为 PowerPoint 演示文稿](https://www.kdnuggets.com/2023/08/chatgpt-convert-text-powerpoint-presentation.html)

+   [使用 tfidfvectorizer 将文本文档转换为 TF-IDF 矩阵](https://www.kdnuggets.com/2022/09/convert-text-documents-tfidf-matrix-tfidfvectorizer.html)

+   [将 Python 字典转换为 JSON：初学者教程](https://www.kdnuggets.com/convert-python-dict-to-json-a-tutorial-for-beginners)

+   [如何使用 Pandas 将 JSON 数据转换为 DataFrame](https://www.kdnuggets.com/how-to-convert-json-data-into-a-dataframe-with-pandas)

+   [在 Python 中将字节转换为字符串：初学者教程](https://www.kdnuggets.com/convert-bytes-to-string-in-python-a-tutorial-for-beginners)

+   [图像识别与自然语言处理中的迁移学习](https://www.kdnuggets.com/2022/01/transfer-learning-image-recognition-natural-language-processing.html)
