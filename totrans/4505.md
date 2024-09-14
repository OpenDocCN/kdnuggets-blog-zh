# 使用GAN生成逼真的人脸

> 原文：[https://www.kdnuggets.com/2020/03/generate-realistic-human-face-using-gan.html](https://www.kdnuggets.com/2020/03/generate-realistic-human-face-using-gan.html)

[评论](#comments)![图像](../Images/c66cd91eb1e8a0a144f7300a8bfddc33.png)

[致谢](https://www.outerplaces.com/science/item/19172-artificial-intelligence-faces-gan)

> * * *
> 
> ## 我们的前三名课程推荐
> ## 
> ![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道
> 
> ![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能
> 
> ![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT工作
> 
> * * *
> 
> “过去20年中深度学习中最酷的想法。” — [Yann LeCun谈GAN](https://www.linkedin.com/in/yann-lecun-0b999/)

我们听说了关于[**艺术风格迁移**](https://techcrunch.com/tag/style-transfer/)和[**面部交换应用**](https://beebom.com/best-face-swap-apps/)（也称为[深伪技术](https://techcrunch.com/tag/deepfakes/)）、**自然语言生成**（[Google Duplex](https://www.youtube.com/watch?v=D5VN56jQMWM)）、[**音乐合成**](https://openai.com/blog/musenet/)、[**智能回复**](https://www.blog.google/products/gmail/save-time-with-smart-reply-in-gmail/)、[**智能写作**](https://ai.googleblog.com/2018/05/smart-compose-using-neural-networks-to.html)**等。**

这种类型的人工智能背后的技术叫做**GAN**，或**“生成对抗网络”**。GAN采用了不同于其他神经网络的学习方法。GAN的算法架构使用两个神经网络，一个是**生成器**，另一个是**鉴别器**，它们相互“竞争”以创建期望的结果。生成器的任务是创建逼真的假图像，而鉴别器的任务是区分真实图像和假图像。如果两者都能高效运行，结果就是看起来几乎和真实照片一模一样的图像。

自从2014年由Ian J. Goodfellow及其合著者在文章[《生成对抗网络》](https://arxiv.org/abs/1406.2661)中介绍了生成对抗网络以来，这一技术取得了巨大的成功。

### GAN最初为什么会被开发出来？

已经注意到，大多数主流神经网络很容易被欺骗，只需在原始数据中添加少量噪声。令人惊讶的是，添加噪声后的模型对错误预测的信心比正确预测时更高。这种对抗的原因在于，大多数机器学习模型从有限的数据中学习，这是一个巨大的缺陷，因为它容易过拟合。此外，输入和输出之间的映射几乎是线性的。虽然看起来各种类别之间的分界线是线性的，但实际上，它们由线性组成，即使特征空间中的一点小变化也可能导致数据的错误分类。

![图示](../Images/a0bda5e3a7d448a715b36064627855bc.png)

[参考](https://github.com/Puzer/stylegan-encoder/issues/1)

### GAN是如何工作的？

GAN通过将两个神经网络对抗起来来学习数据集的概率分布。

一个神经网络，称为**生成器**，生成新的数据实例，而另一个，称为**判别器**，评估这些实例的真实性；即判别器决定它审查的每个数据实例是否属于实际的训练数据集。

与此同时，生成器正在创建新的合成/假图像，并将其传递给判别器。它这样做是希望这些图像也能被认为是真实的，即使它们是假的。假图像是通过逆卷积生成的，称为转置卷积，来自一个100维的噪声（-1.0到1.0之间的均匀分布）。

生成器的目标是**生成**可通过的图像：即在不被抓住的情况下进行欺骗。判别器的目标是识别来自生成器的图像为假图像。

以下是GAN的步骤：

+   生成器接收随机数字并返回图像。

+   这个生成的图像会与从实际的真实数据集中提取的一系列图像一起输入到判别器中。

+   判别器接收真实和假图像，并返回概率值，一个介于0和1之间的数字，其中1表示对真实性的预测，0表示假图像。

所以你有一个双重反馈循环：

+   判别器与我们已知的图像的真实数据处于反馈循环中。

+   生成器与判别器处于反馈循环中。

![图示](../Images/36942c0052f4f1ea690517fca98283d1.png)

[参考](https://medium.com/sigmoid/a-brief-introduction-to-gans-and-how-to-code-them-2620ee465c30)

### GAN的数学原理

让我们更深入地了解它是如何在数学上工作的。判别器的工作是执行二元分类，以检测真实和虚假的图像，因此其损失函数是二元交叉熵。生成器的工作是密度估计，从噪声到真实数据，并将其输入判别器以欺骗它。

设计中遵循的方法是将其建模为一个[**MiniMax博弈**](https://brilliant.org/wiki/minimax/)。现在让我们来看一下成本函数：

![](../Images/a8c31e39ff2c453cd231791c6ccab6ca.png)

`J(D)`中的第一个项表示将实际数据输入给判别器，判别器希望最大化预测为1的对数概率，表示数据是真实的。第二项表示由`G`生成的样本。在这里，判别器希望最大化预测为0的对数概率，表示数据是假的。另一方面，生成器则尝试最小化判别器正确的对数概率。这个问题的解决方案是博弈的均衡点，即判别器损失的鞍点。

**目标函数**

![图](../Images/2a6524e8ec4c450a0cd45e4e61f4e98f.png)

[来源](https://medium.com/sigmoid/a-brief-introduction-to-gans-and-how-to-code-them-2620ee465c30)

**GANs的架构**

`D()` 给出给定样本来自训练数据X的概率。对于生成器，我们希望最小化`log(1-D(G(z)))`，即当`D(G(z))`的值很高时，`D`将假设`G(z)`不过是X，这使得`1-D(G(z))`非常低，我们希望最小化它，从而使其更低。对于判别器，我们希望最大化`D(X)`和`(1-D(G(z)))`。所以D的最优状态将是`P(x)=0.5`。然而，我们希望训练生成器G，使其生成的结果能让判别器D无法区分z和X。

那么问题是为什么这是一个最小最大函数？这是因为判别器尝试最大化目标，而生成器尝试最小化目标，因此我们得到最小最大项。它们通过交替梯度下降一起学习。

尽管理论上的GAN概念很简单，但构建一个有效的模型是非常困难的。在GAN中，有两个深度网络相互耦合，使得梯度的反向传播变得难度加倍。

[深度卷积GAN（DCGAN）](https://arxiv.org/pdf/1511.06434.pdf%C3%AF%C2%BC%E2%80%B0)是展示如何构建一个可以自学合成新图像的实际GAN模型之一。DCGAN与*GAN*s非常相似，但特别专注于使用深度卷积网络来代替在Vanilla *GAN*s中使用的全连接网络。

卷积网络有助于发现图像中的深层关联，即它们寻找空间相关性。这意味着DCGAN将是图像/视频数据的更好选择，而*GAN*s可以被视为[DCGAN](https://arxiv.org/pdf/1511.06434.pdf%C3%AF%C2%BC%E2%80%B0)及许多其他架构（*CGAN、CycleGAN、StarGAN等*）开发的基础性想法。

### 数据集

这个数据集非常适合用于训练和测试人脸检测模型，特别是识别面部特征，例如找出棕色头发、正在微笑或戴眼镜的人。图像涵盖了大范围的姿势变化、背景杂乱、多样的人群，并且支持大量的图像和丰富的注释。

数据集可以从[Kaggle](https://www.kaggle.com/jessicali9530/celeba-dataset)下载。我们的目标是创建一个能够生成现实中不存在的**真实** **人类**图像的模型。

你没听错！

**让我们加载数据集，看看输入图像的样子：**

```py
 from tqdm import tqdm
import numpy as np
import pandas as pd
import os
from matplotlib import pyplot as plt
PIC_DIR = './drive/img_align_celeba/'
IMAGES_COUNT = 10000
ORIG_WIDTH = 178
ORIG_HEIGHT = 208
diff = (ORIG_HEIGHT - ORIG_WIDTH) // 2
WIDTH = 128
HEIGHT = 128
crop_rect = (0, diff, ORIG_WIDTH, ORIG_HEIGHT - diff)
images = []
for pic_file in tqdm(os.listdir(PIC_DIR)[:IMAGES_COUNT]):
    pic = Image.open(PIC_DIR + pic_file).crop(crop_rect)
    pic.thumbnail((WIDTH, HEIGHT), Image.ANTIALIAS)
    images.append(np.uint8(pic)) #Normalize the images
images = np.array(images) / 255
images.shape #print first 25 images
plt.figure(1, figsize=(10, 10))
for i in range(25):
    plt.subplot(5, 5, i+1)
    plt.imshow(images[i])
    plt.axis('off')
plt.show() 
```

![图示](../Images/2b3cc85ace939295de193e1dd161b6cd.png)

输入图像

**下一步是创建生成器：**

生成器的作用正好相反：它是试图欺骗判别器的艺术家。这个网络由8个卷积层组成。这里首先，我们将输入（称为`gen_input`）送入第一个卷积层。每个卷积层执行卷积操作，然后进行批量归一化和泄漏ReLU处理。最后，我们返回tanh激活函数。

```py
 LATENT_DIM = 32
CHANNELS = 3
def create_generator():
    gen_input = Input(shape=(LATENT_DIM, ))

    x = Dense(128 * 16 * 16)(gen_input)
    x = LeakyReLU()(x)
    x = Reshape((16, 16, 128))(x)

    x = Conv2D(256, 5, padding='same')(x)
    x = LeakyReLU()(x)

    x = Conv2DTranspose(256, 4, strides=2, padding='same')(x)
    x = LeakyReLU()(x)

    x = Conv2DTranspose(256, 4, strides=2, padding='same')(x)
    x = LeakyReLU()(x)

    x = Conv2DTranspose(256, 4, strides=2, padding='same')(x)
    x = LeakyReLU()(x)

    x = Conv2D(512, 5, padding='same')(x)
    x = LeakyReLU()(x)
    x = Conv2D(512, 5, padding='same')(x)
    x = LeakyReLU()(x)
    x = Conv2D(CHANNELS, 7, activation='tanh', padding='same')(x)

    generator = Model(gen_input, x)
    return generator 
```

**接下来，创建判别器：**

判别器网络由与生成器相同的卷积层组成。对于网络的每一层，我们将执行卷积操作，然后进行批量归一化以加快网络速度和提高准确性，最后执行Leaky ReLU。

```py
 def create_discriminator():
    disc_input = Input(shape=(HEIGHT, WIDTH, CHANNELS))

    x = Conv2D(256, 3)(disc_input)
    x = LeakyReLU()(x)

    x = Conv2D(256, 4, strides=2)(x)
    x = LeakyReLU()(x)

    x = Conv2D(256, 4, strides=2)(x)
    x = LeakyReLU()(x)

    x = Conv2D(256, 4, strides=2)(x)
    x = LeakyReLU()(x)

    x = Conv2D(256, 4, strides=2)(x)
    x = LeakyReLU()(x)

    x = Flatten()(x)
    x = Dropout(0.4)(x)

    x = Dense(1, activation='sigmoid')(x)
    discriminator = Model(disc_input, x)

    optimizer = RMSprop(
        lr=.0001,
        clipvalue=1.0,
        decay=1e-8
    )

    discriminator.compile(
        optimizer=optimizer,
        loss='binary_crossentropy'
    )

    return discriminator 
```

**定义一个GAN模型：**

接下来，可以定义一个结合生成器模型和判别器模型的大型GAN模型。这个较大的模型将用于训练生成器中的模型权重，使用判别器模型计算的输出和误差。判别器模型单独训练，因此在这个较大的GAN模型中，模型权重标记为不可训练，以确保只有生成器模型的权重被更新。这个对判别器权重训练性的更改仅在训练组合GAN模型时生效，不在训练判别器模型时生效。

这个较大的GAN模型以潜在空间中的一个点作为输入，使用生成器模型生成图像，然后将其作为输入送入判别器模型，最后输出或分类为真实或伪造。

由于判别器的输出是sigmoid函数，我们使用[**二元交叉熵**](https://towardsdatascience.com/understanding-binary-cross-entropy-log-loss-a-visual-explanation-a3ac6025181a)作为损失函数。[**RMSProp**](https://keras.io/optimizers/)作为优化器在本案例中生成更逼真的伪造图像，相比之下[**Adam**](https://keras.io/optimizers/)效果稍差。学习率为0.0001。权重衰减和裁剪值在训练后期稳定学习。如果你想调整学习率，需要调整衰减值。

GAN试图复制概率分布。因此，我们应该使用能够反映GAN生成的数据分布与真实数据分布之间距离的损失函数。

```py
 generator = create_generator()
discriminator = create_discriminator()
discriminator.trainable = False
gan_input = Input(shape=(LATENT_DIM, ))
gan_output = discriminator(generator(gan_input))
gan = Model(gan_input, gan_output)#Adversarial Model
optimizer = RMSprop(lr=.0001, clipvalue=1.0, decay=1e-8)
gan.compile(optimizer=optimizer, loss='binary_crossentropy') 
```

我们不仅需要一个损失函数，而是需要定义三个：生成器的损失，使用真实图像时鉴别器的损失，以及使用虚假图像时鉴别器的损失。虚假图像和真实图像损失的总和即为总体鉴别器损失。

**训练GAN模型：**

训练是最困难的部分，由于GAN包含两个分别训练的网络，它的训练算法必须解决两个复杂问题：

+   GAN必须兼顾两种不同的训练（生成器和鉴别器）。

+   GAN的收敛性难以识别。

随着生成器的训练进步，鉴别器的性能会变差，因为鉴别器无法轻易区分真实和虚假。如果生成器成功完美，则鉴别器的准确率为50%。实际上，鉴别器像掷硬币一样做出预测。

这种进展给整个GAN的收敛性带来了问题：随着时间的推移，鉴别器的反馈变得越来越没有意义。如果GAN继续训练到鉴别器提供完全随机反馈的地步，那么生成器开始在垃圾反馈上训练，其质量可能会崩溃。

```py
 iters = 20000
batch_size = 16RES_DIR = 'res2'
FILE_PATH = '%s/generated_%d.png'
if not os.path.isdir(RES_DIR):
    os.mkdir(RES_DIR)
CONTROL_SIZE_SQRT = 6
control_vectors = np.random.normal(size=(CONTROL_SIZE_SQRT**2, LATENT_DIM)) / 2
start = 0
d_losses = []
a_losses = []
images_saved = 0
for step in range(iters):
    start_time = time.time()
    latent_vectors = np.random.normal(size=(batch_size, LATENT_DIM))
    generated = generator.predict(latent_vectors)

    real = images[start:start + batch_size]
    combined_images = np.concatenate([generated, real])

    labels = np.concatenate([np.ones((batch_size, 1)), np.zeros((batch_size, 1))])
    labels += .05 * np.random.random(labels.shape)

    d_loss = discriminator.train_on_batch(combined_images, labels)
    d_losses.append(d_loss)

    latent_vectors = np.random.normal(size=(batch_size, LATENT_DIM))
    misleading_targets = np.zeros((batch_size, 1))

    a_loss = gan.train_on_batch(latent_vectors, misleading_targets)
    a_losses.append(a_loss)

    start += batch_size
    if start > images.shape[0] - batch_size:
        start = 0

    if step % 50 == 49:
        gan.save_weights('gan.h5')

        print('%d/%d: d_loss: %.4f,  a_loss: %.4f.  (%.1f sec)' % (step + 1, iters, d_loss, a_loss, time.time() - start_time))

        control_image = np.zeros((WIDTH * CONTROL_SIZE_SQRT, HEIGHT * CONTROL_SIZE_SQRT, CHANNELS))
        control_generated = generator.predict(control_vectors)
        for i in range(CONTROL_SIZE_SQRT ** 2):
            x_off = i % CONTROL_SIZE_SQRT
            y_off = i // CONTROL_SIZE_SQRT
            control_image[x_off * WIDTH:(x_off + 1) * WIDTH, y_off * HEIGHT:(y_off + 1) * HEIGHT, :] = control_generated[i, :, :, :]
        im = Image.fromarray(np.uint8(control_image * 255))
        im.save(FILE_PATH % (RES_DIR, images_saved))
        images_saved += 1 
```

我们还需要生成输出图像的GIF。

```py
 import imageio
import shutil
images_to_gif = []
for filename in os.listdir(RES_DIR):
    images_to_gif.append(imageio.imread(RES_DIR + '/' + filename))
imageio.mimsave('trainnig_visual.gif', images_to_gif)
shutil.rmtree(RES_DIR) 
```

输出

你可以在我的GitHub仓库中获取代码。

**[nageshsinghc4/Face-generation-GAN](https://github.com/nageshsinghc4/Face-generation-GAN)**

我们刚刚看到，如果训练充分，一个模型可以生成几乎像人类一样的面孔。由于计算限制，我已将模型训练了15000个周期。你可以尝试更多的周期以获得更好的结果。

> **“GANs是危险的”**

随着时间的推移，这些存在于我们周围的算法在其工作上会越来越好，这意味着这些生成模型可能会在生成模仿对象方面变得更好。另一个突破性的生成模型很可能就在前方。

这项技术可以用于许多有益的事情。然而，也存在不良的潜力。回想2016年的选举以及随后的许多国际选举，当时虚假新闻文章充斥几乎所有社交媒体平台。想象一下，如果这些文章还包含了伴随的“虚假图像”和“虚假音频”，其影响会有多大。宣传在这样的世界中可能传播得更快。实质上，这些新的生成模型，**只要有足够的时间和数据，它们可以从几乎*任何*分布中生成非常可信的样本。**

你可以访问[ thispersondoesnotexist.com](https://thispersondoesnotexist.com/)，体验GAN模型的强大，每次刷新网站时你都会看到一个不同的人物，这些人物甚至并不存在，而是通过GAN生成的。这确实很令人着迷。

### 结论：GAN的未来

无监督学习是[人工智能的下一个前沿](https://www.cs.cornell.edu/content/unsupervised-learning-next-frontier-ai)，我们正朝着这个方向发展。

GAN和生成模型通常非常有趣且令人困惑。它们代表了朝着一个越来越依赖人工智能的世界迈出的另一大步。GAN在以下情况中有广泛的应用，如***生成图像数据集的示例、生成真实照片、图像到图像的翻译、文本到图像的翻译、语义图像到照片的翻译、面部正面视图生成、生成新的人体姿势、面部衰老、视频预测、3D对象生成等。***

好的，这篇关于GAN的文章到此为止，我们讨论了这一酷炫的AI领域及其实际应用。希望大家喜欢阅读，欢迎在评论区分享你的评论/想法/反馈。

**感谢阅读本文！！！**

**简介：[纳格什·辛格·乔汉](https://www.linkedin.com/in/nagesh-singh-chauhan-6936bb13b/)** 是CirrusLabs的大数据开发人员。他在电信、分析、销售、数据科学等多个领域拥有超过4年的工作经验，并专注于各种大数据组件。

[原文](https://towardsdatascience.com/generate-realistic-human-face-using-gan-e6136876e67c)。转载经许可。

**相关：**

+   [对抗性机器学习和生成对抗网络简介](/2019/10/adversarial-machine-learning-generative-adversarial-networks.html)

+   [使用卷积自编码器重建指纹](/2020/03/recreating-fingerprints-using-convolutional-autoencoders.html)

+   [生成对抗网络的半监督学习](/2020/01/semi-supervised-learning-generative-adversarial-networks.html)

### 更多相关话题

+   [使用稳定扩散生成超现实面孔的3种方法](https://www.kdnuggets.com/3-ways-to-generate-hyper-realistic-faces-using-stable-diffusion)

+   [无监督解缠表示学习（类别不平衡数据集中的弹性Infogan）](https://www.kdnuggets.com/2023/01/unsupervised-disentangled-representation-learning-class-imbalanced-dataset-elastic-infogan.html)

+   [虚假至真：生成真实的合成客户数据集](https://www.kdnuggets.com/2022/01/fake-realistic-synthetic-customer-datasets-projects.html)

+   [利用ChatGPT生成被动收入的4种方式](https://www.kdnuggets.com/2023/03/4-ways-generate-passive-income-chatgpt.html)

+   [使用Google MusicLM从文本生成音乐](https://www.kdnuggets.com/2023/06/generate-music-text-google-musiclm.html)

+   [如何生成合成表格数据集](https://www.kdnuggets.com/2022/03/generate-tabular-synthetic-dataset.html)
