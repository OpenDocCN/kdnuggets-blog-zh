# 使用 TensorFlow 和 Streamlit 构建一个生成逼真面孔的应用

> 原文：[https://www.kdnuggets.com/2020/04/app-generate-photorealistic-faces-tensorflow-streamlit.html](https://www.kdnuggets.com/2020/04/app-generate-photorealistic-faces-tensorflow-streamlit.html)

[评论](#comments)

**由 [Adrien Treuille](https://www.linkedin.com/in/adrien-treuille-52215718/)，Streamlit 联合创始人**

![图像](../Images/d3f4b90ea22b7679cfeed5720254753b.png)

[GAN 合成面孔]

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

机器学习模型是黑箱。是的，你可以在测试集上运行它们并绘制华丽的性能曲线，但*仍然*常常很难回答关于它们性能的基本问题。一个出乎意料的强大洞察来源就是**玩弄你的模型**！调整输入。观察输出。让你的同事和经理也来试试。这种互动方式不仅是获得直觉的强大方法，也是让人们对你的工作感到兴奋的绝佳方式。

制作互动模型是启发 [Streamlit](http://streamlit.io/) 的用例之一，这是一个 Python 框架，使得 [编写应用像编写 Python 脚本一样简单](https://towardsdatascience.com/coding-ml-tools-like-you-code-ml-models-ddba3357eace?source=friends_link&sk=f7774c54571148b33cde3ba6c6310086)。本概述将指导你创建一个 Streamlit 应用来玩弄一个最复杂、最黑箱的模型之一：深度*生成对抗网络*（GAN）。在本例中，我们将使用 TensorFlow 可视化 Nvidia 的 [PG-GAN](https://research.nvidia.com/publication/2017-10_Progressive-Growing-of) [1] 来从无中生有合成逼真的人脸。然后，使用 Shaobo Guan 的惊人 [TL-GAN](https://blog.insightdatascience.com/generating-custom-photo-realistic-faces-using-ai-d170b1b59255) [2] 模型，我们将创建一个 [应用](https://github.com/streamlit/demo-face-gan) 使我们能够通过年龄、微笑度、男性相似度和发色等属性来调整 GAN 合成的名人面孔。在教程结束时，你将拥有一个完全参数化的人类模型！（注意我们没有创建这些属性。它们来自于 [CelebA 数据集](http://mmlab.ie.cuhk.edu.hk/projects/CelebA.html) [3]，有些属性可能会有点奇怪……）

### 开始使用 Streamlit

如果你还没有安装 Streamlit，可以通过运行以下命令来进行安装：

```py
pip install streamlit
streamlit hello
```

如果你是经验丰富的Streamlit用户，你需要使用0.57.1或更高版本，确保进行升级！

```py
pip install --upgrade streamlit
```

### 设置你的环境

在我们开始之前，使用下面的命令查看项目的GitHub仓库，并亲自运行[Face GAN demo](https://github.com/streamlit/demo-face-gan)。这个演示依赖于Tensorflow 1，它不支持Python 3.7或3.8，所以你需要使用Python 3.6。在Mac和Linux上，我们推荐使用[pyenv](https://github.com/pyenv/pyenv)来安装Python 3.6并与当前版本共存，然后使用venv或virtualenv设置新的虚拟环境。在Windows上，[Anaconda Navigator](https://docs.anaconda.com/anaconda/navigator/)允许你通过点击界面来[选择你的Python版本](https://docs.anaconda.com/anaconda/navigator/tutorials/use-multiple-python-versions/)。

当你准备好后，打开终端窗口并输入：

```py
git clone [https://github.com/streamlit/demo-face-gan.git](https://github.com/streamlit/demo-face-gan.git)
cd demo-face-gan
pip install -r requirements.txt
streamlit run app.py
```

等待一会儿，让训练好的GAN下载完成，然后尝试调整滑块，探索GAN可以合成的不同面孔。很酷，对吧？

![](../Images/2ba8cf06358eeb8bacc34606a0900451.png)

完整的应用代码是一个大约包含190行代码的文件，其中仅有13行是Streamlit调用的。**没错，以上所有UI都仅仅由这13行代码绘制而成！**

我们来看看应用的结构：

现在你对结构有了大致了解，我们来详细探讨一下以上5个步骤，看看它们是如何工作的。

### 第一步：下载模型和数据文件

这一步会下载我们需要的文件：一个预训练的PG-GAN模型和一个适配它的TL-GAN模型（稍后我们将深入探讨这些！）。

`download_file`工具函数比单纯的下载器更聪明：

+   它会检查文件是否已经存在于本地目录中，因此只在必要时才进行下载。它还会检查下载文件的大小是否符合预期，因此能够修复中断的下载。这是一个很好的模式！

+   它使用`st.progress()`和`st.warning()`来向用户展示一个漂亮的UI，同时文件正在下载。然后在完成后，它调用`.empty()`来隐藏这些UI元素。

### 第二步：将模型加载到内存中

下一步是将这些模型加载到内存中。以下是加载PG-GAN模型的代码：

注意 `@st.cache` 装饰器在 `load_pg_gan_model()` 的开头。通常在 Python 中，你可以直接运行 `load_pg_gan_model()` 并反复重用那个变量。然而，Streamlit 的 [执行模型](https://docs.streamlit.io/main_concepts.html?highlight=execution%20model#data-flow) 是独特的，因为每次用户与 UI 小部件交互时，你的脚本会再次从头到尾完全执行。通过将 `@st.cache` 添加到耗时的模型加载函数中，我们告诉 Streamlit 仅在脚本首次执行时运行这些函数 —— 并且在之后的每次执行中仅重用缓存输出。这是 Streamlit 最基本的功能之一，因为它通过缓存函数调用的结果来高效地运行脚本。这样，大型的 GAN 模型将仅加载到内存中一次；同样，我们的 TensorFlow 会话也将仅创建一次。（参见我们的 [启动文章](https://towardsdatascience.com/coding-ml-tools-like-you-code-ml-models-ddba3357eace?source=friends_link&sk=f7774c54571148b33cde3ba6c6310086) 以了解更多有关 Streamlit 执行模型的内容。）

![图](../Images/434b4264564c81114912b94710a16064.png)

[图 1\. 缓存如何在 Streamlit 执行模型中工作]

不过，有一个问题：TensorFlow 会话对象在我们用它进行不同计算时可能会在内部发生变化。通常，[我们不希望缓存对象发生变化](https://docs.streamlit.io/caching.html#example-6-mutating-cached-values)，因为这可能导致意外的结果。所以当 Streamlit 检测到这样的变化时，会向用户发出警告。然而，在这种情况下我们知道 TensorFlow 会话对象发生变化是可以接受的，因此 [我们绕过警告](https://docs.streamlit.io/troubleshooting/caching_issues.html#how-to-fix-the-cached-object-mutated-warning) 通过设置 `allow_output_mutation=True`。

### 步骤 3\. 绘制侧边栏 UI

如果这是你第一次看到 Streamlit 的小部件绘制 API，以下是 30 秒速成课程：

+   你可以通过调用 API 方法如 `st.slider()` 和 `st.checkbox()` 来添加小部件。

+   这些方法的返回值是 UI 中显示的值。例如，当用户将滑块移动到位置 42 时，你的脚本将重新执行，在那次执行中 `st.slider()` 的返回值将是 42。

+   你可以通过在前面加上 `st.sidebar` 将任何东西放在侧边栏中。例如，`st.sidebar.checkbox()`。

所以，要在侧边栏中添加一个滑块，例如 — 一个允许用户调整 `brown_hair` 参数的滑块，你只需添加：

在我们的应用中，我们希望展示如何轻松地在 Streamlit 中使 UI 本身可修改！我们希望允许用户首先使用多选小部件选择他们想在生成图像中控制的一组特征，这意味着我们的 UI 需要通过编程方式绘制：

![](../Images/a36716267772f577d0c823d074729999.png)

使用Streamlit，相关代码其实非常简单：

### 第4步：合成图像

现在我们有了一组特征，告诉我们要合成什么样的面孔，我们需要进行繁重的面孔合成工作。我们将通过将这些特征传递给TL-GAN来生成PG-GAN潜在空间中的一个向量，然后将该向量输入到PG-GAN中。如果这句话对你来说没有意义，让我们绕个弯子，谈谈我们的两个神经网络是如何工作的。

### **深入了解GAN**

要理解上述应用程序如何根据滑块值生成面孔，你首先需要了解PG-GAN和TL-GAN的工作原理——但不用担心，**你可以跳过这一部分，仍然能在更高层次上理解应用程序的工作原理！**

PG-GAN，像任何GAN一样，根本上是*一对*神经网络，一个生成网络和一个判别网络，它们彼此对抗，永远锁定在生死搏斗中。生成网络负责合成它认为像面孔的图像，而判别网络负责决定这些图像是否确实是面孔。这两个网络迭代地对抗对方的输出，所以每个网络都尽力学会欺骗另一个网络。最终结果是，最终的生成网络能够合成看起来逼真的面孔，即使在训练开始时，它只能合成随机噪声。这真的很惊人！在这种情况下，我们使用的面孔生成GAN是由Karras *等人* 使用他们的[渐进式生成对抗网络](https://github.com/tkarras/progressive_growing_of_gans)（PG-GAN）算法在名人面孔上进行训练的，该算法通过逐渐提高分辨率的图像来训练GAN。[1]

PG-GAN的输入是一个高维向量，属于其所谓的*潜在空间*。潜在空间基本上是网络可以生成的所有可能面孔的空间，因此该空间中的每个随机向量对应一个独特的面孔（或者至少应该是！有时你会得到奇怪的结果……）你通常使用GAN的方式是给它一个随机向量，然后查看合成出什么样的面孔（见图2.a）。

![图](../Images/c71c1eb6611e2080ce5ce1eb3f020a9f.png)

[图2.a]

然而，这听起来有点单调，我们希望对输出有更多的控制。我们希望告诉PG-GAN“生成一个有胡子的男人的图像”，或者“生成一个棕色头发的女人的图像”。这就是TL-GAN发挥作用的地方。

TL-GAN 是另一个神经网络，这个网络通过将随机向量输入 PG-GAN、获取生成的面孔，然后通过分类器检查诸如“看起来年轻”、“有胡须”、“棕色头发”等属性来进行训练。在训练阶段，TL-GAN 使用这些分类器标记 PG-GAN 生成的数千个面孔，并识别与我们关心的标签变化相对应的潜在空间中的方向。因此，TL-GAN 学会了如何将这些类别（即“看起来年轻”、“有胡须”、“棕色头发”）映射到适当的随机向量中，该向量应该输入到 PG-GAN 中以生成具有这些特征的面孔（图 2.b）。

![图](../Images/444c4f6eebf36d5528508f27adccf7f8.png)

[图 2.b]

回到我们的应用程序，此时我们已经下载了预训练的 GAN 模型并将其加载到内存中，同时还从 UI 中获取了特征向量。所以现在我们只需将这些特征输入到 TL-GAN 然后是 PG-GAN 以获得图像：

### 优化性能

上面的`generate_image()`函数可能需要一些时间来执行，特别是在 CPU 上运行时。为了提高应用程序的性能，如果我们能够缓存该函数的输出，将会很好，这样我们在滑动条来回移动时，就不需要重新合成已经看到的面孔。

嗯，如你在上面的代码片段中已经注意到的，解决方案是再次使用 `@st.cache` 装饰器。

但请注意我们在这种情况下传递给 `@st.cache` 的两个参数：`show_spinner=False` 和 `hash_funcs={tf.Session: id}`。这些是用来做什么的？

第一个问题容易解释：默认情况下，`@st.cache` 在 UI 中显示一个状态框，让你知道一个运行缓慢的函数正在执行。我们称之为“加载指示器”。但在这种情况下，我们希望避免显示它，以免 UI 意外跳动。因此，我们将`show_spinner`设置为 False。

下一个问题解决了一个更复杂的问题：TensorFlow 会话对象作为参数传递给 `generate_image()`，通常在这个缓存函数运行之间会被 TensorFlow 的内部机制修改。这意味着`generate_image()`的输入参数将始终不同，我们将永远不会真正获得缓存命中。换句话说，`@st.cache` 装饰器实际上不会起作用！我们如何解决这个问题？

### hash_funcs 的救援

hash_funcs 选项允许我们指定自定义哈希函数，告诉`@st.cache` 在检查是否为缓存命中或缓存未命中时，应该如何解释不同的对象。在这种情况下，我们将使用该选项告诉 Streamlit 通过调用 Python 的 `id()` 函数来对 TensorFlow 会话进行哈希，而不是通过检查其内容：

这对我们有效，因为在我们的情况下，会话对象实际上是所有底层代码执行中的单例，因为它来自 @st.cache 的 `load_pg_gan_model()` 函数。

欲了解有关`hash_funcs`的更多信息，请查看我们关于[高级缓存技术](https://docs.streamlit.io/api.html?highlight=cache#streamlit.cache)的文档。

### 第5步：绘制合成图像

现在我们已经有了输出图像，绘制它就轻而易举了！只需调用Streamlit的`st.image`函数：

```py
st.image(image_out, use_column_width=True)
```

我们完成了！

### 总结

就这样：在一个190行的Streamlit应用中使用TensorFlow进行互动面孔合成，仅需13个Streamlit函数调用！祝你在探索这两个GAN能绘制的面孔领域时玩得开心，非常感谢[Nvidia](https://github.com/tkarras/progressive_growing_of_gans)和[Shaobo Guan](https://github.com/SummitKwan/transparent_latent_gan)让我们能在他们超级酷的演示基础上进行构建。我们希望你像我们一样享受构建应用和玩弄模型的乐趣。????

欲了解更多Streamlit应用示例，请查看我们的画廊[https://www.streamlit.io/gallery](https://www.streamlit.io/gallery)。

*感谢Ash Blum、TC Ricks、Amanda Kelly、Thiago Teixeira、Jonathan Rhone和Tim Conkling对本文的宝贵意见。*

**参考文献：**

[1] T. Karras, T. Aila, S. Laine, J. Lehtinen. *改进质量、稳定性和变化的渐进式生成对抗网络*。国际学习表征会议（ICLR 2018）

[2] S. Guan. *使用新颖的TL-GAN模型进行受控图像合成和编辑*。Insight Data Science博客（2018）

[3] Z. Liu, P. Luo, X. Wang, X. Tang. *深度学习人脸属性*。国际计算机视觉会议（ICCV 2015）

**简介：[Adrien Treuille](https://www.linkedin.com/in/adrien-treuille-52215718/)** 是Streamlit的联合创始人，该框架专注于机器学习工具。Adrien曾是卡内基梅隆大学的计算机科学教授，领导了Google X项目，并曾担任Zoox的副总裁。

[原文](https://towardsdatascience.com/build-an-app-to-synthesize-photorealistic-faces-using-tensorflow-and-streamlit-dd2545828021)。经授权转载。

**相关：**

+   [使用GAN生成逼真的人脸](/2020/03/generate-realistic-human-face-using-gan.html)

+   [12小时机器学习挑战：使用Streamlit和DevOps工具构建和部署应用](/2020/02/machine-learning-challenge-build-deploy-app-streamlit-devops.html)

+   [被遗忘的算法](/2020/02/forgotten-algorithm-monte-carlo-simulation.html)

### 更多相关话题

+   [使用稳定扩散生成超逼真的面孔的3种方法](https://www.kdnuggets.com/3-ways-to-generate-hyper-realistic-faces-using-stable-diffusion)

+   [5分钟内构建机器学习网页应用](https://www.kdnuggets.com/2022/03/build-machine-learning-web-app-5-minutes.html)

+   [KDnuggets新闻2022年3月9日：在5分钟内构建机器学习网页应用…](https://www.kdnuggets.com/2022/n10.html)

+   [用Python在7个简单步骤中构建命令行应用](https://www.kdnuggets.com/build-a-command-line-app-with-python-in-7-easy-steps)

+   [使用 ChatGPT 生成被动收入的 4 种方法](https://www.kdnuggets.com/2023/03/4-ways-generate-passive-income-chatgpt.html)

+   [使用 Google MusicLM 从文本生成音乐](https://www.kdnuggets.com/2023/06/generate-music-text-google-musiclm.html)
