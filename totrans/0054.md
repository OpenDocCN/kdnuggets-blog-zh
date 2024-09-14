# 使用稳定扩散生成超现实面孔的三种方法

> 原文：[https://www.kdnuggets.com/3-ways-to-generate-hyper-realistic-faces-using-stable-diffusion](https://www.kdnuggets.com/3-ways-to-generate-hyper-realistic-faces-using-stable-diffusion)

![使用稳定扩散生成超现实面孔的三种方法](../Images/c5a3321c0b0fb249f21321a0baaf5fb6.png)

作者提供的图片

你是否曾经想知道人们是如何使用AI图像生成技术生成如此超现实的面孔，而你的尝试却总是充满了缺陷和伪影，看起来明显是假的？你已经尝试调整提示和设置，但仍然无法匹配你看到的其他人生产的质量。你究竟做错了什么？

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT

* * *

在这篇博客文章中，我将带你了解三种关键技巧，以开始使用稳定扩散生成超现实的人脸。首先，我们将涵盖提示工程的基础，以帮助你使用基本模型生成图像。接下来，我们将探索如何通过升级到稳定扩散XL模型，通过更多的参数和训练显著提高图像质量。最后，我将向你介绍一个专门为生成高质量肖像而微调的自定义模型。

# 1\. 提示工程

首先，我们将学习如何编写正面和负面提示以生成逼真的面孔。我们将使用Hugging Face Spaces上可用的稳定扩散2.1版本演示。它是免费的，你可以在不设置任何东西的情况下开始。

**链接：** hf.co/spaces/stabilityai/stable-diffusion

创建正面提示时，请确保包含所有必要的细节和图像风格。在这种情况下，我们希望生成一张年轻女性在街上走路的图像。我们将使用一个通用的负面提示，但你可以添加额外的关键词以避免图像中的重复错误。

**正面提示：** “一个二十多岁的年轻女性，走在街上，直视镜头，自信友好的表情，随意穿着现代时尚的服饰，城市街景背景，明亮的阳光照射，鲜艳的色彩”

**负面提示：** “变形的，丑陋的，糟糕的，不成熟的，卡通的，动漫的，3d的，画作的，黑白的，卡通的，画作的，插图的，最差质量，低质量”

![使用稳定扩散生成超现实面孔的三种方法](../Images/420a40bc90832d1df9b5e7f92ee2b138.png)

![使用Stable Diffusion生成超现实面孔的三种方法](../Images/4d26b18b4304ce69293a004bc43f03dd.png)

我们取得了一个良好的开端。图像准确，但质量还可以更好。你可以尝试不同的提示，但这是基础模型能提供的最佳效果。

# 2\. Stable Diffusion XL

我们将使用Stable Diffusion XL（SDXL）模型生成高质量的图像。它通过使用基础模式生成潜在图像，然后通过精化器处理，以生成详细且准确的图像。

**链接：** hf.co/spaces/hysts/SD-XL

在生成图像之前，我们将向下滚动并打开“高级选项”。我们将添加负面提示，设置种子，并应用精化器以获得最佳图像质量。

![使用Stable Diffusion生成超现实面孔的三种方法](../Images/d1cacd0ec92a2311d5a5c978b383bd0a.png)

然后，我们将编写与之前相同的提示，并做出小的更改。我们将生成一位年轻印度女性的图像，而不是普通的年轻女性。

![使用Stable Diffusion生成超现实面孔的三种方法](../Images/5ed114cb449ec52146acd71c123011ec.png)

这是一个显著改进的结果。面部特征完美。让我们尝试生成其他种族的图像，以检查偏差并比较结果。

![使用Stable Diffusion生成超现实面孔的三种方法](../Images/a76ea72c725f83e0cbaf555c9fb275dc.png)

我们得到了逼真的面孔，但所有图像都有Instagram滤镜。通常，真实生活中的皮肤不会这么光滑，常常有痤疮、标记、雀斑和皱纹。

# 3\. CivitAI: RealVisXL V2.0

在这一部分，我们将生成具有标记和逼真皮肤的详细面孔。为此，我们将使用来自CivitAI的定制模型（RealVisXL V2.0），该模型经过微调以获得高质量的肖像。

**链接：** civitai.com/models/139562/realvisxl-v20

你可以点击“创建”按钮在线使用模型，或者下载以便在本地通过Stable Diffusion WebUI使用。

![使用Stable Diffusion生成超现实面孔的三种方法](../Images/138b1c24815f3ade8300ab1b84f835a3.png)

首先，下载模型并将文件移动到Stable Diffusion WebUI模型目录：C:\WebUI\webui\models\Stable-diffusion。

要在WebUI上显示模型，你需要按下刷新按钮，然后选择“realvisxl20…”模型检查点。

![使用Stable Diffusion生成超现实面孔的三种方法](../Images/ca4f7396e588ba86529cdb101478bdac.png)

我们将开始编写相同的正面和负面提示，并生成高质量的1024X1024图像。

![使用Stable Diffusion生成超现实面孔的三种方法](../Images/10ce765be34ff78475da378604ff1f1f.png)

图像看起来很完美。为了充分利用定制模型，我们需要更改我们的提示。

![使用Stable Diffusion生成超现实面孔的三种方法](../Images/4c1e2ec2dd12ae27bf76eed2eabb5250.png)

新的积极和消极提示可以通过滚动模型页面并点击你喜欢的逼真图像来获得。CivitAI上的图像附带了积极和消极提示以及高级引导。

**积极提示：** “一个专注、果断、超现实的印度年轻女性的图像，动态姿势，超高分辨率，清晰纹理，高细节RAW照片，详细的面部，浅景深，锐利的眼睛，（逼真的皮肤纹理：1.2），浅色皮肤，单反，相机颗粒”

**消极提示：** “（最差质量，低质量，插图，3d，2d，画画，漫画，素描），张嘴”

![使用稳定扩散生成超逼真面孔的三种方法](../Images/43b0fed10fe92e5324a6a04cde82bdc9.png)

我们有一张详细的印度女性的图像，皮肤逼真。与基础的SDXL模型相比，这是一个改进版本。

![使用稳定扩散生成超逼真面孔的三种方法](../Images/718a8db49d84eba473832ab493fa9bd5.png)

我们生成了三张更多的图像以比较不同的民族。这些结果非常出色，包含了皮肤标记、多孔皮肤和准确的特征。

# 结论

生成艺术的进步将很快达到一个我们难以区分真实和合成图像的水平。这预示着一个可持续的未来，在这个未来中，任何人都可以利用基于多样化真实世界数据训练的自定义模型，从简单的文本提示生成高度逼真的媒体。快速的发展暗示着激动人心的潜力——也许有一天，生成一个逼真的视频来复制你自己的相貌和语言模式将像键入描述性提示一样简单。

在这篇文章中，我们学习了提示工程、高级稳定设计模型以及用于生成高精度和逼真面孔的定制微调模型。如果你想要更好的结果，我建议你探索civitai.com上各种高质量的模型。

[](https://www.polywork.com/kingabzpro)****[Abid Ali Awan](https://www.polywork.com/kingabzpro)**** ([@1abidaliawan](https://www.linkedin.com/in/1abidaliawan)) 是一位认证的数据科学专业人士，热爱构建机器学习模型。目前，他专注于内容创作，并撰写有关机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是利用图神经网络为面临心理健康困扰的学生构建一个AI产品。

### 更多相关内容

+   [生成AI游乐场：文本到图像稳定扩散与……](https://www.kdnuggets.com/2024/02/intel-generative-ai-playground-text-to-image-stable-diffusion)

+   [使用Phraser和稳定扩散成为AI艺术家](https://www.kdnuggets.com/2022/09/become-ai-artist-phraser-stable-diffusion.html)

+   [稳定扩散：生成AI背后的基本直觉](https://www.kdnuggets.com/2023/06/stable-diffusion-basic-intuition-behind-generative-ai.html)

+   [使用 ChatGPT 生成被动收入的 4 种方法](https://www.kdnuggets.com/2023/03/4-ways-generate-passive-income-chatgpt.html)

+   [使用 Google MusicLM 从文本生成音乐](https://www.kdnuggets.com/2023/06/generate-music-text-google-musiclm.html)

+   [如何生成合成表格数据集](https://www.kdnuggets.com/2022/03/generate-tabular-synthetic-dataset.html)
