# 深度学习中的过拟合问题

> 原文：[https://www.kdnuggets.com/2019/12/fighting-overfitting-deep-learning.html](https://www.kdnuggets.com/2019/12/fighting-overfitting-deep-learning.html)

[评论](#comments)

**由 [ActiveWizards](https://activewizards.com/) 提供**

![图示](../Images/ed0cde5a50a91862ae85dce52e2095cb.png)

### 问题

在训练模型时，我们希望根据选择的指标获得最佳结果。同时，我们还希望在新数据上保持类似的结果。残酷的现实是，我们无法获得100%的准确性。即使我们做到了，结果仍然难免有错误。测试情况太少，难以发现它们。你可能会问，这有什么关系？

错误有两种类型：可减少的和不可减少的。不可减少的错误是由于数据不足产生的。例如，除了类型、时长、演员之外，人们的心情和观看电影时的氛围也会影响评分。但我们无法预测未来人的心情。另一个原因是数据质量。

我们的目标是减少可减少的错误，这些错误又分为偏差和方差。

### 偏差

偏差出现在我们试图用简单模型描述复杂过程时，结果产生了错误。例如，用线性函数描述非线性互动是不可行的。

### 方差

方差描述了预测的稳健性，即当数据变化时预测会变化多少。理想情况下，数据有小的变化时，预测也会有轻微的变化。

![图示](../Images/78746f1bfe3c49494312fdd94c8060d0.png)

偏差和方差对预测的影响

这些错误是相互关联的，导致一个减少时另一个增加。这个问题被称为偏差-方差权衡。

模型越复杂（灵活），对目标变量的方差解释得越好，偏差就越小。但是，尽管如此，模型越复杂，对训练数据的适应性越强，方差也会增加。在某个时点，模型将开始寻找在新数据上不会重复的随机模式，从而降低模型的泛化能力，并增加测试数据上的错误。

有一张图描述了偏差-方差权衡。红线是损失函数（例如 MSE - 均方误差）。蓝线是偏差，橙线是方差。正如你所见，最佳解决方案会出现在这些线交点的某处。

![图示](../Images/8ad6f1fff405cfe68a39b0b9affbc63b.png)

错误函数的分解

你也可以认识到，当损失开始增加的时刻就是模型过拟合的时刻。

结果是：我们需要控制错误，以防止不仅仅是过拟合，还包括欠拟合。

### 我们如何解决这个问题？

正则化是一种技术，用于避免学习更复杂或灵活的模型，以降低过拟合的风险。

注：

> 问题是，在训练数据中表现最佳的模型不能保证在测试数据中也是最佳的。因此，我们可以通过交叉验证选择超参数。

### 正则化

![图](../Images/ef95db2d8a50a5a51c548cc02b25523c.png)

或者，换句话说：

*成本函数 = 损失 + 正则化项*

损失函数通常是一个在数据点、预测和标签上定义的函数，并衡量惩罚。成本函数通常更为一般。它可能是训练集上的损失函数的总和加上一些模型复杂性惩罚（正则化项）****。

通常，过拟合的模型具有不同符号的大权重。这样，它们在总和上互相抵消，得到结果。改善评分的方法是减少权重。

每个神经元可以表示为

![图](../Images/f215f4ce311ca65ee2e1082564e19b21.png)

其中 f 是激活函数，*w* 是权重，X 是数据。

系数向量的“长度”可以通过向量的范数来描述。为了控制我们将减少系数的长度（惩罚模型的复杂性，从而防止其过拟合或过度调整数据），我们将通过将其乘以 λ 来加权范数。选择 λ 没有分析方法，因此可以通过网格搜索来选择。

****L2 范数（L2 正则化，岭回归）****

![图](../Images/b70e3f5187a63ae32403856a7e04deed.png)

如果损失是 MSE，那么具有 L2 范数的成本函数可以通过解析方法求解。

![图](../Images/8ce8dbe63c2dc8a7af0dd7f41e0957ef.png)

你可以看到，我们只是添加了一个乘以 λ 的单位矩阵（岭），以获得一个非奇异矩阵，并增加问题的收敛性。

![图](../Images/efacff284bf0087323f5a99baa0c62c2.png)

通过添加单位矩阵，将多重共线性的 XTX 矩阵转换为非奇异矩阵。

换句话说，我们需要最小化成本函数，因此我们最小化损失和向量的速率（在这种情况下，这是权重平方的总和）。因此，权重会更接近零。

让我们计算：

w^T 是权重的向量行。

正则化之前：

![图](../Images/c84979bf97f86f93cd5d251224df7aa9.png)

数据的微小变化

![图](../Images/2fbbd8f9fe43ab51fcc9e8a90c7f91c0.png)

正则化后，我们将在测试数据上得到相同的结果，

![图](../Images/6ac186edf099c845a012f3abf9282217.png)

但，数据的微小变化会导致结果的微小变化。

![图](../Images/92881580f84db5d3409f297884f479b7.png)

****示例****

```py
from keras import regularizers
...
lambda = 0.01
model.add(Dense(64, input_dim=64,
                kernel_regularizer=regularizers.l2(lambda)))
```

****L1 范数（L1 正则化，Lasso）****

L1 范数意味着我们使用权重的绝对值而不是平方值。没有解析方法来解决这个问题。

![图](../Images/950fc3ae7822523377aaded861a145f0.png)

这种回归将一些权重置为零。在我们尝试压缩模型时，它非常有用。

****示例****

```py
from keras import regularizers
...
lambda = 0.01 
model.add(Dense(64, input_dim=64,
                kernel_regularizer=regularizers.l1(lambda)))
```

我们来看一个有两个参数（B1 和 B2）的简单损失函数，并绘制一个三维图。

![图](../Images/343a9fd77c2c8aea5d0077dd1c9cb24d.png)

在三维空间中的梯度下降与等高线表示 | [来源](https://towardsdatascience.com/regularization-in-machine-learning-connecting-the-dots-c6e030bfaddd)

正如你所看到的，损失函数可能在不同参数下具有相同的值。我们将其投影到参数表面上，其中同一曲线上的每一点具有相同的函数值。这条线称为等高线。

设 Lambda = 1。L2 正则化对于两个参数的函数是 B12 + B22，这在图上形成一个圆。L1 正则化是 |B1| + |B2|，这在图上形成一个菱形。

![图](../Images/66b2320cd4352efa1a1178346deb522b.png)

L1 和 L2 范数与不同的成本函数 | [来源](https://towardsdatascience.com/regularization-in-machine-learning-connecting-the-dots-c6e030bfaddd)

现在，让我们绘制不同的损失函数以及蓝色菱形（L1）和黑色圆圈（L2）正则化项（Lambda = 1）。实际情况是，成本函数在红色圆圈与黑色正则化曲线（L2）的交点处达到最小值，而在蓝色菱形与等高线交点处达到最小值（L1）。

### **Dropout**（随机失活）

想象一个简单的网络。例如，黑客马拉松中的开发团队。团队中有经验丰富或更聪明的开发者，他们承担了整个开发的主要工作，而其他人只做一些小帮助。如果这种情况持续下去，经验丰富的开发者会变得更有经验，而其他人则很难得到训练。神经元也是如此。

![图](../Images/e9dc1415db6283be40232cec7c534fbb.png)

**全连接网络**

但想象一下，在每次产品开发迭代中，我们随机断开一些开发者的连接。那么其余的开发者不得不更加投入，大家学习得更好。

![图](../Images/3dfdcfb6d929ddbb227c06b64bce4db6.png)

随机丢失节点的网络

可能会发生什么问题？如果你关闭了太多的神经元，那么剩下的神经元可能无法应对它们的工作，结果只会变得更糟。

这也可以被看作是机器学习中的一种集成技术。记住，强学习者的集成比单一模型表现更好，因为它们能捕捉更多的随机性，且不容易过拟合。但是，弱学习者的集成比原始模型更容易过拟合。

因此，**dropout** 只在大型神经网络中使用。

****示例****

```py
from keras.layers.core import Dropout
…
percent_of_dropped_neurons = 0.25
model = Sequential([
   Dense(32, activation='relu', input_shape=(10,)),
   Dropout(percent_of_dropped_neurons)
   Dense(32, activation='relu',),
   Dense(1, activation='linear')
])
```

### 数据增强

减少过拟合的最简单方法是增加训练数据的规模。在机器学习中，由于成本高昂，增加数据量是困难的。

那么，图像处理呢？在这种情况下，有几种方法可以增加训练数据的规模——旋转图像、翻转、缩放、平移等。我们还可以添加没有目标类的图像，以教会网络如何区分目标与噪声。

****示例****

```py
from keras.preprocessing.image import ImageDataGenerator
datagen = ImageDataGenerator(
       rotation_range=40,
       width_shift_range=0.2,
       height_shift_range=0.2,
       shear_range=0.2,
       zoom_range=0.2,
       horizontal_flip=True,
       fill_mode='nearest')
img = load_img('983794168.jpg')    # load image from file system
x = img_to_array(img)              # turn image to array and reshape
x = x.reshape((1,) + x.shape)
i = 0
# the .flow() command below generates batches of randomly transformed images
# and saves the results to the `test_data_augmentation` directory
for batch in datagen.flow(x, batch_size=1,
                         save_to_dir='test_data_augmentation', save_prefix='data', save_format='jpeg'):
   i += 1
   if i > 20:
       break   # generate 20 images and stop after
```

****原始图像：****

![图](../Images/9c61094028e19f432db482e6933ff0a5.png)

****增强图像：****

![图](../Images/8249bc4d5b185a0dda76243edf1b8859.png)![图](../Images/01dee9199a92378367caad7d25114b0a.png)![图](../Images/01dee9199a92378367caad7d25114b0a.png)![图](../Images/a3420c9ad50b4452112de6744f1d936e.png)![图](../Images/10a73410076a8d297c4f20f864d92a40.png)![图](../Images/2202c647c9cf80fbd279a67fadafc005.png)

### 提前停止

提前停止是一种交叉验证策略，其中我们将训练集的一部分作为验证集。当我们看到验证集上的性能变差时，我们会立即停止模型的训练。

****示例****

```py
from keras.callbacks import EarlyStopping

es = EarlyStopping(monitor='val_loss', mode='min')
```

这就是我们所需的最简单形式的提前停止。当选定的性能测量指标不再改善时，训练将停止。要发现训练在哪个时期停止，可以将“verbose”参数设置为1。

通常，第一次没有进一步改善的迹象可能不是停止训练的最佳时机。这是因为模型可能会在改善之前进入一个没有改进的平稳期，甚至可能稍微变得更差。

我们可以通过在触发器中添加一个延迟来考虑这一点，以便我们希望看到没有改进的纪元数。这可以通过设置“patience”参数来完成。

```py
es = EarlyStopping(monitor='val_loss', mode='min', verbose=1, patience=50)
```

精确的耐心值将在模型和问题之间有所不同。

默认情况下，性能测量指标的任何变化，无论多么微小，都将被视为改善。你可能需要考虑一个特定的增量，例如均方误差的1单位或准确率的1%。这可以通过“min_delta”参数指定。

```py
es = EarlyStopping(monitor='val_accuracy', mode='max', min_delta=1)
```

EarlyStopping回调可以与其他回调一起应用，例如tensorboard。

```py
model.fit(
X_train,
y_train, 
epochs=2000, 
batch_size=32, 
validation_data=(X_val, y_val), 
callbacks=[get_tensorboard_callback('baseline_pca(45)_log', True), es])
```

了解更多信息：[这里](https://keras.io/callbacks/)

也可以附加Tensorboard并手动监控变化。每5分钟，模型将保存到日志目录中，因此你可以随时检查更好的版本。

### 神经网络架构

众所周知，深度为2的足够大的网络已经可以在[0,1]d上以任意精度逼近任何连续目标函数（Cybenko,1989；Hornik, 1991）。另一方面，较深的网络通常比浅层网络表现更好，这一点早已显而易见。

[最近的分析](http://proceedings.mlr.press/v49/eldan16.pdf)表明，“深度——即使增加1——对于标准前馈神经网络的价值可能是宽度的指数倍”。

你可以认为每一层新层提取了一个新的特征，从而增加了非线性。

记住，增加深度意味着你的模型更复杂，优化函数可能无法找到最佳的权重集。

想象一下，你有一个具有2层隐藏层的神经网络，每层5个神经元。高度 = 2，宽度 = 5。让我们在每层添加一个神经元并计算连接数：（5+1）*（5+1）= 36个连接。现在，让我们在原始网络中添加一层并计算连接数：5*5*5 = 125个连接。因此，每层将显著增加连接数和执行时间。

但与此同时，这也会增加过拟合的机会。

非常宽的神经网络擅长记忆数据，因此你不应该构建非常宽的网络。尽量构建尽可能小的网络来解决问题。记住，网络越大越复杂，过拟合的机会就越高。

你可以在[这里](https://pdfs.semanticscholar.org/69b4/68459daa2b2e5c096c0cf6da8735dba26a4a.pdf)找到更多信息。

### 迁移学习

为什么我们总是从头开始？我们可以利用预训练模型的权重并仅对其进行优化。问题是我们没有一个在我们数据上预训练的模型。但我们可以使用在大量数据上预训练的网络。然后，我们用相对较小的数据来微调预训练的模型。常见做法是冻结除最后几层以外的所有层。

迁移学习的主要优点是它缓解了训练数据不足的问题。正如你所记得的，这是导致过拟合的原因之一。

迁移学习只有在深度学习中如果模型在第一个任务中学到的特征是通用的才有效。使用预训练模型进行图像处理（[这里](https://github.com/BVLC/caffe/wiki/Model-Zoo)）和文本处理非常流行，例如[google word2vec](https://code.google.com/archive/p/word2vec/)。

另一个好处是迁移学习可以提高生产力并减少训练时间：

![图](../Images/86a67d6d2409c24bb3985ea500815371.png)

衡量函数

### 批量归一化

[批量归一化](https://arxiv.org/abs/1502.03167?context=cs)不仅可以作为正则化器，还可以通过增加学习率来减少训练时间。问题是，在训练过程中，每一层的分布都会发生变化。因此，我们需要减少学习率，这会减慢梯度下降优化的速度。但是，如果我们对每个训练小批量应用归一化，我们可以增加学习率并更快地找到最小值。

****示例****

```py
from keras.layers.normalization import BatchNormalization
...
model = Sequential([
   Dense(32, activation='relu', input_shape=(10,)),
   BatchNormalization(),
   Dense(32, activation='relu'),
   Dense(1, activation='linear')
])
```

### 其他

还有一些其他不太流行的深度神经网络防止过拟合的方法。它们并不一定有效。如果你已经尝试过所有其他方法并且想尝试其他东西，你可以在这里阅读更多内容：小[批量大小](https://arxiv.org/pdf/1705.08741.pdf)，[权重噪声](https://www.semanticscholar.org/paper/On-Weight-Noise-Injection-Training-Ho-Leung/1023043d2a5a76a07388c3e17c1284018937dbfc)。

### 结论

过拟合出现在我们拥有过于复杂的模型时。我们的模型开始识别噪声或随机关系，这些关系在新数据中不会再出现。

这种情况的一个特点是神经元中存在不同符号的大权重。一个直接解决方案是 L1 和 L2 正则化，可以分别应用于每一层。

另一种方法是对大型神经网络应用 dropout，或者通过数据增强等方法增加数据量。你也可以配置早停回调，以检测模型何时开始过拟合。

此外，尽量构建尽可能小的神经网络。谨慎选择深度和宽度。

不要忘记，你总是可以使用预训练模型来提高模型生产力。至少，你可以应用批量归一化来提高学习速率并同时减少过拟合。

这些方法的不同组合将给你一个结果，并帮助你解决任务。

**[ActiveWizards](https://activewizards.com/)** 是一个专注于数据项目（大数据、数据科学、机器学习、数据可视化）的数据科学家和工程师团队。核心专长领域包括数据科学（研究、机器学习算法、可视化和工程）、数据可视化（d3.js、Tableau 等）、大数据工程（Hadoop、Spark、Kafka、Cassandra、HBase、MongoDB 等）和数据密集型 Web 应用开发（RESTful API、Flask、Django、Meteor）。

[原文](https://activewizards.com/blog/fighting-overfitting-in-deep-learning/)。经授权转载。

**相关：**

+   [开启深度学习革命](/2019/12/enabling-deep-learning-revolution.html)

+   [使用更少数据进行图像分类的深度学习](/2019/11/deep-learning-image-classification-less-data.html)

+   [2019 年热门深度学习课程](/2019/12/deep-learning-courses.html)

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

### 更多相关话题

+   [用 AI 对抗 AI：深伪应用的欺诈监测](https://www.kdnuggets.com/2023/05/fighting-ai-ai-fraud-monitoring-deepfake-applications.html)

+   [如何避免过拟合](https://www.kdnuggets.com/2022/08/avoid-overfitting.html)

+   [KDnuggets 新闻，8 月 24 日：在 Python 中实现 DBSCAN • 如何…](https://www.kdnuggets.com/2022/n34.html)

+   [学习数据科学、机器学习和深度学习的可靠计划](https://www.kdnuggets.com/2023/01/mwiti-solid-plan-learning-data-science-machine-learning-deep-learning.html)

+   [人工智能、分析、机器学习、数据科学、深度学习……](https://www.kdnuggets.com/2021/12/developments-predictions-ai-machine-learning-data-science-research.html)

+   [15本免费机器学习和深度学习书籍](https://www.kdnuggets.com/2022/10/15-free-machine-learning-deep-learning-books.html)
