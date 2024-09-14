# 使用命令行中的 TensorFlow 创建您的第一个 GitHub 项目中的 GAN

> 原文：[https://www.kdnuggets.com/2018/05/zimbres-first-github-project-gans.html](https://www.kdnuggets.com/2018/05/zimbres-first-github-project-gans.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由 [Rubens Zimbres](https://www.linkedin.com/in/rubens-zimbres/)，数据科学家**

我从 2016 年开始贡献于 GitHub。自那时起，我的代码库中有超过 100 个不同的文件，包含了我在学习数据科学期间开发的机器学习、深度学习和自然语言处理代码。但它们只是简单的代码库，我没有专注于开发一个 GitHub 项目。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT

* * *

在本文中，我将介绍创建您的第一个 GitHub 项目的步骤。我将以生成对抗网络为例。如果您想了解更多关于 GAN 的信息，可以点击我的 PDF 演示文稿查看：

**[https://github.com/RubensZimbres/Repo-2018/blob/master/Deep%20Learning%20Summer%20School/GANs_Summer_School_15_FINAL.pdf](https://github.com/RubensZimbres/Repo-2018/blob/master/Deep%20Learning%20Summer%20School/GANs_Summer_School_15_FINAL.pdf)**

正如您将在我的项目中看到的那样，有一个 main.py 文件，其中包含 GAN 代码，还有一个包含正确运行代码所需库的文件（requirements.txt）。

文件 main.py 包含 GAN 代码本身和运行笔记本所需的参数。这些参数以这种形式插入到 main.py 中：

```py

import argparse

parser = argparse.ArgumentParser(description='')
parser.add_argument('--epoch', dest='nb_epoch', type=int, default=5000, help='# of epochs')
parser.add_argument('--learning_rate', dest='lr', type=float, default=0.0001, help='# learning rate')
parser.add_argument('--sample_size', dest='sample_size', type=int, default=60, help='# sample size')
parser.add_argument('--gen_hidden', dest='gen_hidden', type=int, default=80, help='# hidden nodes in generator')
parser.add_argument('--disc_hidden', dest='disc_hidden', type=int, default=80, help='# hidden nodes in discriminator')
parser.add_argument('--your_login', dest='your_login', type=str, default='rubens', help='# your login name')

args = vars(parser.parse_args())

```

另一个文件 requirements.txt 包含运行 main.py 文件所需的库，简单形式如下：

tensorflow

numpy

matplotlib

keras

pandas

接下来，我们可以开发一个交互式函数，为用户提供指南，并获取其操作系统，以便代码自定义 Tensorboard 文件存储的文件夹。

```py

var0 = input("Are you using Linux? [y|n]")

if str(var0)=='n':
        logs_path = 'C:/Users/'+args['your_login']+'/Anaconda3/envs/Scripts/plot_1'
    else:
        logs_path = '/home/'+args['your_login']+'/anaconda3/envs/plot_1'

```

之后我们定义一个包含 GAN 的函数：

```py

def GAN(sample_size):

    tf.reset_default_graph() 
    def generator(x, reuse=False):
        with tf.variable_scope('Generator', reuse=reuse):
            x = tf.layers.dense(x, units=6 * 6 * 64)
            x = tf.nn.relu(x)
            x = tf.reshape(x, shape=[-1, 6, 6, 64])
            x = tf.layers.conv2d_transpose(x, 32, 4, strides=2)
            x = tf.layers.conv2d_transpose(x, 1, 2, strides=2)
            x = tf.nn.relu(x)
            x = tf.reshape(x, [n,784])
            return x

    def discriminator(x, reuse=False):
        with tf.variable_scope('Discriminator', reuse=reuse):
            x = tf.reshape(x, [n,28,28,1])
            x = tf.layers.conv2d(x, 32, 5)
            x = tf.nn.relu(x)
            x = tf.layers.average_pooling2d(x, 2, 2,padding='same')
            x = tf.layers.conv2d(x, 64, 5,padding='same')
            x = tf.nn.relu(x)
            x = tf.layers.average_pooling2d(x, 8, 8)
            x = tf.contrib.layers.flatten(x)
            x = tf.layers.dense(x, 784)
            x = tf.nn.sigmoid(x)
        return x
```

… 等等。文章末尾我将提供我代码库的完整链接。

我们还添加了生成 Tensorboard 的代码行，使用 tf.summary：

损失：

```py

tf.summary.scalar("Generator_Loss", gen_loss)
tf.summary.scalar("Discriminator_Loss", disc_loss)

```

由生成器生成并由判别器分类的图像：

```py

tf.summary.image('GenSample', tf.reshape(gen_sample, [-1, 28, 28, 1]), 4)
tf.summary.image('stacked_gan', tf.reshape(stacked_gan, [-1, 28, 28, 1]), 4)

```

还有，权重的直方图：

```py

for i in range(0,11):
    with tf.name_scope('layer'+str(i)):
        pesos=tf.get_collection(tf.GraphKeys.TRAINABLE_VARIABLES)
        tf.summary.histogram('pesos'+str(i), pesos[i])

```

然后我们打开 Tensorflow 会话以运行反向传播并生成图像：

```py

with tf.Session() as sess:
    sess.run(init)
    summary_writer = tf.summary.FileWriter(logs_path, graph=tf.get_default_graph())
    for i in range(1, num_steps+1):
        batch_x, batch_y=next_batch(batch_size, x_train
                                    , x_train_noisy)        
        feed_dict = {real_image_input: batch_x, noise_input: batch_y,
                     disc_target: batch_x, gen_target: batch_y}
        _, _, gl, dl,summary2 = sess.run([train_gen, train_disc, gen_loss, disc_loss,summary],
                                feed_dict=feed_dict)
        g = sess.run([stacked_gan], feed_dict={noise_input: batch_y})
        h = sess.run([gen_sample], feed_dict={noise_input: batch_y})
        summary_writer.add_summary(summary2, i)
        if i % show_steps == 0 or i == 1:
            print('Epoch %i: Generator Loss: %f, Discriminator Loss: %f' % (i, gl, dl))

```

最终：

```py

if __name__ == '__main__':
   GAN(args[‘sample_size’])
```

要运行代码，您必须在 Anaconda 文件夹中运行：

```py

$ git clone https://github.com/RubensZimbres/GAN-Project-2018

$ cd GAN-Project-2018

$ conda install --yes --file requirements.txt

$ python main.py --epoch=4000 --learning_rate=0.0001 –your_login=rubens

```

其中：

```py

Arguments:
--epoch: default=5000
--learning_rate: default=0.0001
--sample_size: default=60
--gen_hidden: # hidden nodes in generator: default=80
--disc_hidden: # hidden nodes in discriminator: default=80
--your_login: your login in your OS: default:rubens

```

这行代码提供的参数将按如下方式自定义您的 GAN：

```py

num_steps = args['nb_epoch']
learning_rate1=args['lr']
image_dim = 784 
gen_hidden_dim = args['gen_hidden']
disc_hidden_dim = args['disc_hidden']

```

这是你预计得到的输出：

![输出图像](../Images/68253cb14b27c6359bdcaec614546170.png)

Tensorboard 将在你关闭弹出窗口后启动（GAN 输出与 MNIST 数字）

关闭 MNIST 弹出窗口后，代码通过 `main.py` 中的以下代码行启动 Tensorboard 服务器：

```py

os.system('tensorboard –logdir='+logs_path)
```

然后，你可以访问 Tensorboard 提供的 URL，在浏览器中查看训练细节，如标量（训练损失）、图形（GAN 结构）、图像（生成和分类）和直方图（权重）。请查看以下图片。

要访问此项目的详细信息，请访问：**[https://github.com/RubensZimbres/GAN-Project-2018](https://github.com/RubensZimbres/GAN-Project-2018)**

![图片](../Images/e1be7b540d7d213fca0e6df6124dc514.png)

![图片](../Images/e1d5d275a464ab0a978dfc1b7582c82b.png)

![图片](../Images/27ed32f2a241752dfa73fd0ae4d4fd61.png)

**个人简介：[Rubens Zimbres](https://www.linkedin.com/today/author/rubens-zimbres)**，博士，是一位战略家和数据科学家，拥有超过 23 年的客户服务、管理和财务规划经验，曾在战略规划和重组、物理和数字营销、社交网络分析、人员管理、客户数据库分析等领域担任首席执行官和数据科学家。他拥有 13 年市场研究专长和 10 年数据分析经验。

**相关：**

+   [生成对抗网络概述](/2018/01/generative-adversarial-networks-overview.html)

+   [前 20 名 Python AI 和机器学习开源项目](/2018/02/top-20-python-ai-machine-learning-open-source-projects.html)

+   [我如何不知不觉地为开源做出贡献](/2018/04/how-contributed-open-source.html)

### 更多相关话题

+   [掌握命令行艺术，这个 GitHub 仓库](https://www.kdnuggets.com/master-the-art-of-command-line-with-this-github-repository)

+   [ChatGPT CLI：将你的命令行界面转变为 ChatGPT](https://www.kdnuggets.com/2023/07/chatgpt-cli-transform-commandline-interface-chatgpt.html)

+   [命令行中的数据科学：免费电子书](https://www.kdnuggets.com/2022/03/data-science-command-line-free-ebook.html)

+   [更多 5 种数据科学的命令行工具](https://www.kdnuggets.com/2023/03/5-command-line-tools-data-science.html)

+   [用 Python 在 7 个简单步骤中构建命令行应用程序](https://www.kdnuggets.com/build-a-command-line-app-with-python-in-7-easy-steps)

+   [使用 TensorFlow 和 Keras 构建和训练你的第一个神经网络](https://www.kdnuggets.com/2023/05/building-training-first-neural-network-tensorflow-keras.html)
