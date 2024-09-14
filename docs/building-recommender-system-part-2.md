# 构建推荐系统，第 2 部分

> 原文：[https://www.kdnuggets.com/2019/07/building-recommender-system-part-2.html](https://www.kdnuggets.com/2019/07/building-recommender-system-part-2.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者：Matthew Mahowald，[开放数据集团](https://www.opendatagroup.com/)**

在上一篇文章中，我们探讨了基于邻域的方法来构建推荐系统。本文探讨了一种使用潜在因子模型的协同过滤替代技术。我们将使用的技术自然可以推广到深度学习方法（如自编码器），因此我们还将使用 Tensorflow 和 Keras 实现我们的方法。

![影院门](../Images/62ea85463bb6b53abb44cb4d031ff0c4.png)

### 数据集

本文将重用我们上次用于协同过滤模型的 MovieLens 数据集。[GroupLens](https://grouplens.org) [在这里提供了数据集](https://grouplens.org/datasets/movielens/)。

首先，让我们加载这些数据：

```py
import pandas as pd
import numpy as np

np.random.seed(42)

ratings = pd.read_csv(RATING_DATA_FILE,
 sep='::',
 engine='python',
 encoding='latin-1',
 names=['userid', 'movieid', 'rating', 'timestamp'])

movies = pd.read_csv(os.path.join(MOVIELENS_DIR, MOVIE_DATA_FILE),
 sep='::',
 engine='python',
 encoding='latin-1',
 names=['movieid', 'title', 'genre']).set_index("movieid")
```

让我们快速查看一下前 20 个最受欢迎的文件：

|  | 标题 | 类型 |
| --- | --- | --- |
| movieid |  |  |
| --- | --- | --- |
| 2858 | 美国丽人 (1999) | 喜剧&#124;剧情 |
| 260 | 星球大战：新希望 (1977) | 动作&#124;冒险&#124;奇幻&#124;科幻 |
| 1196 | 星球大战：帝国反击战… | 动作&#124;冒险&#124;剧情&#124;科幻&#124;战争 |
| 1210 | 星球大战：绝地归来 (1983) | 动作&#124;冒险&#124;浪漫&#124;科幻&#124;战争 |
| 480 | 侏罗纪公园 (1993) | 动作&#124;冒险&#124;科幻 |
| 2028 | 拯救大兵瑞恩 (1998) | 动作&#124;剧情&#124;战争 |
| 589 | 终结者 2：审判日 (1991) | 动作&#124;科幻&#124;惊悚 |
| 2571 | 黑客帝国 (1999) | 动作&#124;科幻&#124;惊悚 |
| 1270 | 回到未来 (1985) | 喜剧&#124;科幻 |
| 593 | 沉默的羔羊 (1991) | 剧情&#124;惊悚 |
| 1580 | 黑衣人 (1997) | 动作&#124;冒险&#124;喜剧&#124;科幻 |
| 1198 | 夺宝奇兵 (1981) | 动作&#124;冒险 |
| 608 | 冰血暴 (1996) | 犯罪&#124;剧情&#124;惊悚 |
| 2762 | 第六感 (1999) | 惊悚 |
| 110 | 布雷夫哈特 (1995) | 动作&#124;剧情&#124;战争 |
| 2396 | 罗密欧与朱丽叶 (1998) | 喜剧&#124;浪漫 |
| 1197 | 公主新娘 (1987) | 动作&#124;冒险&#124;喜剧&#124;浪漫 |
| 527 | 辛德勒的名单 (1993) | 剧情&#124;战争 |
| 1617 | 洛杉矶机密 (1997) | 犯罪&#124;黑色电影&#124;悬疑&#124;惊悚 |
| 1265 | 土拨鼠日 (1993) | 喜剧&#124;浪漫 |

### 预处理

协同过滤模型通常在每个项目有相当数量的评分时效果最佳。我们将限制为仅使用 500 部最受欢迎的电影（按评分数量确定）。我们还将按 `movieid` 和 `userid` 重新索引：

```py
rating_counts = ratings.groupby("movieid")["rating"].count().sort_values(ascending=False)

# only the 500 most popular movies
pop_ratings = ratings[ratings["movieid"].isin((rating_counts).index[0:500])]
pop_ratings = pop_ratings.set_index(["movieid", "userid"])
```

接下来，[如前一篇文章中提到的](/2019/04/building-recommender-system.html)，我们应该规范化我们的评分数据。我们通过减去总体均值评分、每个项目的均值评分，然后减去每个用户的均值评分来创建一个调整后的评分。

这产生了一个“偏好评分” ![\tilde{r}_{u,i}](../Images/52a9c49297042d77a17cce1bba397ac4.png)，定义如下

![

\tilde{r}_{u,i} := r_{u,i} - \bar{r} - \bar{r}_{i} - \bar{r}_{u}

](../Images/7e596563eff0b2daefe4be7079d610a7.png)

对于 ![\tilde{r}](../Images/15faa8d8c7bbf110f5cdc90f3df75b49.png) 的直觉是， ![\tilde{r} = 0](../Images/b0fdf67af6503f0804a125e9413b19a8.png) 表示用户 ![u](../Images/c1155b10039d460302206caf78e70b84.png) 对项目 ![i](../Images/7e670de46a6672b7c7196f23e4711b0b.png) 的评分正是我们如果只知道平均总体评分、项目评分和用户评分时的预测。任何高于或低于0的值表示相对于这个基准的偏好偏差。为了区分 ![\tilde{r}](../Images/15faa8d8c7bbf110f5cdc90f3df75b49.png) 和原始评分 ![r](../Images/dc16d9309e95f2dd60bba8a2d99d78b4.png)，我将前者称为用户对项目 ![i](../Images/7e670de46a6672b7c7196f23e4711b0b.png) 的*偏好*，后者称为用户对项目 ![i](../Images/7e670de46a6672b7c7196f23e4711b0b.png) 的*评分*。

让我们使用对500部最受欢迎电影的评分来构建偏好数据：

```py
prefs = pop_ratings["rating"]

mean_0 = pop_ratings["rating"].mean()
prefs = prefs - mean_0

mean_i = prefs.groupby("movieid").mean()
prefs = prefs - mean_i

mean_u = prefs.groupby("userid").mean()
prefs = prefs - mean_u

pref_matrix = prefs.reset_index()[["userid", "movieid", "rating"]].pivot(index="userid", columns="movieid", values="rating")
```

这段代码的输出是两个对象：`prefs`，它是一个按`movieid`和`userid`索引的偏好数据框；以及`pref_matrix`，它是一个矩阵，其中的 ![(i,j)](../Images/eb0b8c3c59dc87afe6d3a1ddaa4dd520.png) 项对应于用户 ![i](../Images/7e670de46a6672b7c7196f23e4711b0b.png) 对电影 ![j](../Images/4b5e5503442fdfa2029fcc9208d6ca1a.png) 的评分（即列是电影，每行是用户）。如果用户没有对某个项目进行评分，这个矩阵将包含 `NaN`。

数据中的最大和最小偏好分别是3.923和-4.643。接下来，我们将构建一个实际的模型。

### 潜在因子协同过滤

在这一阶段，我们已经构建了一个矩阵 ![P](../Images/7a36f354985d6a6caf213c0934bdc243.png)（在上面的Python代码中称为`pref_matrix`）。潜在因子协同过滤模型的思想是，每个用户的偏好可以通过少量的潜在因子来预测（通常远小于可用项目的总数）：

![

\tilde{r}_{u,i} \approx f_{i}(\lambda_{1}(u), \lambda_{2}(u), \ldots, \lambda_{n}(u))

](../Images/95ea1e92b71a45cf8e1b2d3317fd764b.png)

潜在因子模型因此需要回答两个相关的问题：

1.  对于给定的用户 ![u](../Images/c1155b10039d460302206caf78e70b84.png)，相应的潜在因子是什么 ![\lambda_{k}(u)](../Images/7182e3b55bb891245c408f09bac9c180.png)？

1.  对于给定的潜在因子集合，函数 ![f_{i}](../Images/f105567a69855b0e0e3fc7a24547c4e4.png) 是什么？即，潜在因子与用户对每个项目的偏好之间的关系是什么？

解决这个问题的一种方法是尝试求解 ![f_{i}](../Images/f105567a69855b0e0e3fc7a24547c4e4.png) 和 ![\lambda_{k}](../Images/f5faabc2a6741e3731804ea32acf6217.png)，通过简化假设每个函数都是线性的：

![

\lambda_{k}(u) = \sum_{i} a_{i} \tilde{r}_{u,i}

](../Images/c1594024dae526ec375179d67d522f5d.png)

![

f_{i} = \sum_{k} b_{k} \lambda_{k}

](../Images/f8b09f43d5e1f4139f75aaacc54cd15b.png)

在所有项目和用户中，这可以被重新写为线性代数问题：找出矩阵 ![F](../Images/39efd788e124a66b0f98d992a7cb4f9e.png) 和 ![\Lambda](../Images/41bc440d6829aeae8408d80f760a18d3.png) 使得

![ P \approx F  \Lambda  P, ](../Images/03898dea92e52495175d8b468b0cf8a4.png)

其中 ![P](../Images/7a36f354985d6a6caf213c0934bdc243.png) 是偏好矩阵，![\Lambda](../Images/41bc440d6829aeae8408d80f760a18d3.png) 是将用户的偏好投影到潜在变量空间的线性变换，而 ![F](../Images/39efd788e124a66b0f98d992a7cb4f9e.png) 是从用户在潜在变量空间中的表示中重建用户评分的线性变换。

这个产品 ![F \Lambda](../Images/57fc7a9b4718945cec840bfd8965af63.png) 将是一个方阵。然而，通过选择的潜在变量数量严格少于项目数量，这个产品必然不是满秩的。从本质上讲，我们是在求解 ![F](../Images/39efd788e124a66b0f98d992a7cb4f9e.png) 和 ![\Lambda](../Images/41bc440d6829aeae8408d80f760a18d3.png)，使得产品 ![F \Lambda](../Images/57fc7a9b4718945cec840bfd8965af63.png) 最好地逼近身份变换 *在偏好矩阵上* ![P](../Images/7a36f354985d6a6caf213c0934bdc243.png)。我们的直觉（和希望）是，这将重建每个用户的准确偏好。（我们将调整我们的损失函数以确保确实如此。）

### 模型实现

如广告所示，我们将使用 Keras + Tensorflow 构建我们的模型，以便我们为任何未来的深度学习方法的推广做好准备。这也是解决我们所处理问题的自然方法：表达式

![

P \approx F \Lambda P

](../Images/a7661198c171c6721f2751bc69e02d3c.png)

可以被认为是描述一个两层密集神经网络的，其中层由 ![F](../Images/39efd788e124a66b0f98d992a7cb4f9e.png) 和 ![\Lambda](../Images/41bc440d6829aeae8408d80f760a18d3.png) 定义，并且其激活函数就是身份映射（即函数 ![\sigma(x) = x](../Images/80b0e6e0e12ab684d6e97147cadeac6b.png)）。

首先，让我们导入我们需要的包，并设置我们为这个模型所需的编码维度（潜在变量的数量）。

```py
import tensorflow as tf

from keras.layers import Input, Dense, Lambda
from keras.models import Model, load_model as keras_load_model
from keras import losses
from keras.callbacks import EarlyStopping

ENCODING_DIM = 25
ITEM_COUNT = 500
```

接下来，将模型本身定义为“编码”层（投影到潜在变量空间）和“解码”层（从潜在变量表示中恢复偏好）的组合。推荐模型本身只是这两层的组合。

```py
# ~~~ build recommender ~~~ #
input_layer = Input(shape=(ITEM_COUNT, ))
# compress to low dimension
encoded = Dense(ENCODING_DIM, activation="linear", use_bias=False)(input_layer)
# blow up to large dimension
decoded = Dense(ITEM_COUNT, activation="linear", use_bias=False)(encoded) 

# define subsets of the model:
# 1\. the recommender itself
recommender = Model(input_layer, decoded)

# 2\. the encoder
encoder = Model(input_layer, encoded)

# 3\. the decoder
encoded_input = Input(shape=(ENCODING_DIM, ))
decoder = Model(encoded_input, recommender.layers[-1](encoded_input))
```

### 自定义损失函数

此时，我们可以直接训练我们的模型以仅重现其输入（这本质上是一个非常简单的自编码器）。然而，我们实际上感兴趣的是选择 ![F](../Images/39efd788e124a66b0f98d992a7cb4f9e.png) 和 ![\Lambda](../Images/41bc440d6829aeae8408d80f760a18d3.png) 来正确填充 *缺失* 的值。我们可以通过仔细应用掩盖和自定义损失函数来做到这一点。

记住，`prefs_matrix` 目前主要由 NaNs 组成——实际上，整个数据集中只有一个零值：

```py
prefs[prefs == 0]
# movieid  userid
# 2664     2204      0.0
```

在 `prefs_matrix` 中，我们可以用零填充任何缺失的值。这是一个合理的选择，因为我们已经对评分进行了某种归一化处理，因此 0 代表我们对用户对特定项目偏好的天真猜测。然后，为了创建训练数据，使用 `prefs_matrix` 作为目标，并有选择性地掩盖 `prefs_matrix` 中的非零元素来创建输入（“遗忘”特定用户-项目的偏好）。然后，我们可以构建一个损失函数，该函数会强烈惩罚错误猜测“遗忘”的值，即训练以从已知评分构建新评分的损失函数。以下是我们的函数：

```py
def lambda_mse(frac=0.8):
 """
 Specialized loss function for recommender model.

 :param frac: Proportion of weight to give to novel ratings.
 :return: A loss function for use in a Lambda layer.
 """
 def lossfunc(xarray):
 x_in, y_true, y_pred = xarray
 zeros = tf.zeros_like(y_true)

 novel_mask = tf.not_equal(x_in, y_true)
 known_mask = tf.not_equal(x_in, zeros)

 y_true_1 = tf.boolean_mask(y_true, novel_mask)
 y_pred_1 = tf.boolean_mask(y_pred, novel_mask)

 y_true_2 = tf.boolean_mask(y_true, known_mask)
 y_pred_2 = tf.boolean_mask(y_pred, known_mask)

 unknown_loss = losses.mean_squared_error(y_true_1, y_pred_1)
 known_loss = losses.mean_squared_error(y_true_2, y_pred_2)

 # remove nans
 unknown_loss = tf.where(tf.is_nan(unknown_loss), 0.0, unknown_loss)

 return frac*unknown_loss + (1.0 - frac)*known_loss
 return lossfunc
```

默认情况下，返回的损失是整体 MSE 和仅缺失评分的 MSE 的 20%-80% 加权和。这个损失函数需要输入（带有缺失偏好）、预测的偏好和真实的偏好。

至少截至本文发布之日，Keras 和 TensorFlow 目前不支持具有三个输入的自定义损失函数（其他框架，如 PyTorch，支持）。我们可以通过引入“虚拟”损失函数和简单的包装模型来绕过这个事实。Keras 中的损失函数只需要两个输入，因此这个虚拟函数将忽略“真实”值。

```py
def final_loss(y_true, y_pred):
 """
 Dummy loss function for wrapper model.
 :param y_true: true value (not used, but required by Keras)
 :param y_pred: predicted value
 :return: y_pred
 """
 return y_pred
```

接下来是我们的包装模型。这里的想法是使用 lambda 层（‘`loss`’）来应用我们的自定义损失函数（`'lambda_mse'`），然后使用我们的自定义损失函数进行实际优化。使用 Keras 的功能 API 可以非常容易地将我们已经定义的推荐器用这个简单的包装模型进行包装。

```py
original_inputs = recommender.input
y_true_inputs = Input(shape=(ITEM_COUNT, ))
original_outputs = recommender.output
# give 80% of the weight to guessing the missings, 20% to reproducing the knowns
loss = Lambda(lambda_mse(0.8))([original_inputs, y_true_inputs, original_outputs])

wrapper_model = Model(inputs=[original_inputs, y_true_inputs], outputs=[loss])
wrapper_model.compile(optimizer='adadelta', loss=final_loss)
```

### 训练

为了生成我们的模型训练数据，我们将从偏好矩阵 `pref_matrix` 开始，并随机掩盖（即设置为 0）每个用户已知评分的某个比例。将其结构化为生成器允许我们创建一个本质上无限的训练数据集合（尽管在每种情况下，输出受限于从相同固定已知评分集中提取）。以下是生成器函数：

```py
def generate(pref_matrix, batch_size=64, mask_fraction=0.2):
 """
 Generate training triplets from this dataset.

 :param batch_size: Size of each training data batch.
 :param mask_fraction: Fraction of ratings in training data input to mask. 0.2 = hide 20% of input ratings.
 :param repeat: Steps between shuffles.
 :return: A generator that returns tuples of the form ([X, y], zeros) where X, y, and zeros all have
 shape[0] = batch_size. X, y are training inputs for the recommender.
 """

 def select_and_mask(frac):
 def applier(row):
 row = row.copy()
 idx = np.where(row != 0)[0]
 if len(idx) > 0:
 masked = np.random.choice(idx, size=(int)(frac*len(idx)), replace=False)
 row[masked] = 0
 return row
 return applier

 indices = np.arange(pref_matrix.shape[0])
 batches_per_epoch = int(np.floor(len(indices)/batch_size))
 while True:
 np.random.shuffle(indices)

 for batch in range(0, batches_per_epoch):
 idx = indices[batch*batch_size:(batch+1)*batch_size]

 y = np.array(pref_matrix[idx,:])
 X = np.apply_along_axis(select_and_mask(frac=mask_fraction), axis=1, arr=y)

 yield [X, y], np.zeros(batch_size)
```

让我们检查一下这个生成器的掩盖功能是否正常工作：

```py
[X, y], _ = next(generate(pref_matrix.fillna(0).values))
len(X[X != 0])/len(y[y != 0])
# returns 0.8040994014148377
```

为了完成故事，我们将定义一个训练函数，调用此生成器并允许我们设置其他一些参数（训练轮数、早期停止等）：

```py
def fit(wrapper_model, pref_matrix, batch_size=64, mask_fraction=0.2, epochs=1, verbose=1, patience=0):
 stopper = EarlyStopping(monitor="loss", min_delta=0.00001, patience=patience, verbose=verbose)
 batches_per_epoch = int(np.floor(pref_matrix.shape[0]/batch_size))

 generator = generate(pref_matrix, batch_size, mask_fraction)

 history = wrapper_model.fit_generator(
 generator,
 steps_per_epoch=batches_per_epoch,
 epochs=epochs,
 callbacks = [stopper] if patience > 0 else []
 )

 return history
```

记住 ![\Lambda](../Images/41bc440d6829aeae8408d80f760a18d3.png) 和 ![F](../Images/39efd788e124a66b0f98d992a7cb4f9e.png) 是 ![500 \times 25](../Images/d6a778a96473fa10c2db7ceb09d870b7.png) 和 ![25 \times 500](../Images/580ce03d74e594872245387cc94bd042.png) 维矩阵，因此这个模型有 ![2 \times 25 \times 500 = 25000](../Images/6eef3c56cc0d237518afec6f983426c7.png) 个参数。对于线性模型，一个好的经验法则是每个参数至少要有10个观测值，这意味着我们希望在训练期间看到250,000个用户评分向量。然而，我们的用户数量远远不够，因此在本教程中，我们将大大减少数量——最多使用12,500个观测值（如果损失没有改善则提前停止模型）。

```py
# stop after 3 epochs with no improvement
fit(wrapper_model, pref_matrix.fillna(0).values, batch_size=125, epochs=100, patience=3)
# Loss of 0.6321
```

这个训练过程的输出（至少在我的机器上）给出了0.6321的损失，这意味着我们在没有见过的用户真正偏好上平均相差约0.7901单位（请记住，这个损失中80%来自未知偏好，20%来自已知偏好）。我们的数据中的偏好范围从-4.64到3.92，所以这也不算太差！

### 预测评分

要使用我们的模型生成预测，我们必须在沿各个维度标准化评分后调用之前训练的`recommender`模型。假设我们预测函数的输入是一个以（`movieid`，`userid`）索引的 dataframe，并且有一个名为`"rating"`的列。

```py
def predict(ratings, recommender, mean_0, mean_i, movies):
 # add a dummy user that's seen all the movies so when we generate
 # the ratings matrix, it has the appropriate columns
 dummy_user = movies.reset_index()[["movieid"]].copy()
 dummy_user["userid"] = -99999
 dummy_user["rating"] = 0
 dummy_user = dummy_user.set_index(["movieid", "userid"])

 ratings = ratings["rating"]

 ratings = ratings - mean_0
 ratings = ratings - mean_i
 mean_u = ratings.groupby("userid").mean()
 ratings = ratings - mean_u

 ratings = ratings.append(dummy_user["rating"])

 pref_mat = ratings.reset_index()[["userid", "movieid", "rating"]].pivot(index="userid", columns="movieid", values="rating")
 X = pref_mat.fillna(0).values
 y = recommender.predict(X)

 output = pd.DataFrame(y, index=pref_mat.index, columns=pref_mat.columns)
 output = output.iloc[1:] # drop the bad user

 output = output.add(mean_u, axis=0)
 output = output.add(mean_i, axis=1)
 output = output.add(mean_0)

 return output
```

让我们试试看！这里有一些单个虚拟用户的示例评分，他非常喜欢《星球大战》和《侏罗纪公园》，而对其他的电影不太感兴趣：

```py
sample_ratings = pd.DataFrame([
 {"userid": 1, "movieid": 2858, "rating": 1}, # american beauty
 {"userid": 1, "movieid": 260, "rating": 5},  # star wars
 {"userid": 1, "movieid": 480, "rating": 5},  # jurassic park
 {"userid": 1, "movieid": 593, "rating": 2},  # silence of the lambs
 {"userid": 1, "movieid": 2396, "rating": 2}, # shakespeare in love
 {"userid": 1, "movieid": 1197, "rating": 5}  # princess bride
]).set_index(["movieid", "userid"])

# predict and print the top 10 ratings for this user
y = predict(sample_ratings, recommender, mean_0, mean_i, movies.loc[(rating_counts).index[0:500]]).transpose()
preds = y.sort_values(by=1, ascending=False).head(10)

preds["title"] = movies.loc[preds.index]["title"]
preds
```

| userid | 1 | title |
| --- | --- | --- |
| movieid |  |  |
| --- | --- | --- |
| 260 | 4.008329 | 星球大战：新希望 (1977) |
| 1198 | 3.942005 | 夺宝奇兵 (1981) |
| 1196 | 3.860034 | 星球大战：帝国反击战… |
| 1148 | 3.716259 | 错误裤子 (1993) |
| 904 | 3.683811 | 后窗 (1954) |
| 2019 | 3.654374 | 七武士（七侠镇）(Shichin… |
| 913 | 3.639756 | 马耳他之鹰 (1941) |
| 318 | 3.637150 | 肖申克的救赎 (1994) |
| 745 | 3.619762 | 接近剃刀 (1995) |
| 908 | 3.608473 | 西北偏北 (1959) |

有趣的是，即使用户给*星球大战*的评分为5，模型预测的*星球大战*的评分也只有4.08。不过它确实推荐了*帝国反击战*和*夺宝奇兵*，这些推荐似乎符合这些偏好。

现在让我们逆转这个用户对《星球大战》和《侏罗纪公园》的评分，看看评分如何变化：

```py
sample_ratings2 = pd.DataFrame([
 {"userid": 1, "movieid": 2858, "rating": 5}, # american beauty
 {"userid": 1, "movieid": 260, "rating": 1},  # star wars
 {"userid": 1, "movieid": 480, "rating": 1},  # jurassic park
 {"userid": 1, "movieid": 593, "rating": 1},  # silence of the lambs
 {"userid": 1, "movieid": 2396, "rating": 5}, # shakespeare in love
 {"userid": 1, "movieid": 1197, "rating": 5}  # princess bride
]).set_index(["movieid", "userid"])

y = predict(sample_ratings2, recommender, mean_0, mean_i, movies.loc[(rating_counts).index[0:500]]).transpose()
preds = y.sort_values(by=1, ascending=False).head(10)

preds["title"] = movies.loc[preds.index]["title"]
preds
```

| userid | 1 | title |
| --- | --- | --- |
| movieid |  |  |
| --- | --- | --- |
| 2019 | 3.532214 | 七武士（七侠镇）(Shichin… |
| 50 | 3.489284 | 通常的嫌疑犯 (1995) |
| 2858 | 3.480124 | *美国美人*(1999) |
| 745 | 3.466157 | 《理发师的故事》(1995) |
| 1148 | 3.415981 | 《错误的裤子》(1993) |
| 1197 | 3.415527 | 《公主新娘》(1987) |
| 527 | 3.386785 | 《辛德勒的名单》(1993) |
| 750 | 3.342154 | 《奇爱博士》 |
| 1252 | 3.338330 | 《唐人街探案》(1974) |
| 1207 | 3.335204 | 《杀死一只知更鸟》(1962) |

注意到*七武士*在两个列表中都显著出现。事实上，*七武士*在这个数据集中具有最高的平均评分（为4.56），查看用户推荐的前20或前50部电影时，还会发现更多非常高评分的共同电影。

### 结论与进一步阅读

我们构建的潜在因子表示也可以被视为将*项目*嵌入到某个低维空间中，而不是将*用户*嵌入。这让我们可以做一些有趣的事情，例如，我们可以比较每个项目向量表示之间的距离，以了解两部电影的相似或不同。让我们将*星球大战*与*帝国反击战*和*美国美人*进行比较：

```py
starwars = decoder.get_weights()[0][:,33]
esb = decoder.get_weights()[0][:,144]
americanbeauty = decoder.get_weights()[0][:,401]
```

注意到 33 是对应于*星球大战*的列索引（不同于其`movieid`为260），144 是对应于*帝国反击战*的列索引，401 是对应于*美国美人*的列索引。

```py
np.sqrt(((starwars - esb)**2).sum())
# 0.209578

np.sqrt(((starwars - americanbeauty)**2).sum())
# 0.613659
```

比较这些距离，我们看到*星球大战*和*帝国反击战*在潜在因子空间中的距离为0.209578，比*星球大战*和*美国美人*的距离要近得多。

通过进一步的工作，还可以在潜在因子空间中回答其他问题，例如“哪部电影与*星球大战*最不相似？”

这种类型的技术的变体导致了基于自编码器的推荐系统。有关进一步阅读，还有一类相关模型，称为矩阵分解模型，它可以同时包含项目和用户特征以及原始评分。

**相关：**

+   [构建推荐系统](/2019/04/building-recommender-system.html)

+   [K-Means 聚类：用于推荐系统的无监督学习](/2019/04/k-means-clustering-unsupervised-learning-recommender-systems.html)

+   [使用 Azure 机器学习服务构建推荐系统](/2019/05/recommender-systems-azure-machine-learning.html)

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

### 更多相关话题

+   [使用 Python 为亚马逊产品构建推荐系统](https://www.kdnuggets.com/2023/02/building-recommender-system-amazon-products-python.html)

+   [使用 Hugging Face Transformers 构建推荐系统](https://www.kdnuggets.com/building-a-recommendation-system-with-hugging-face-transformers)

+   [构建视觉搜索引擎 - 第 1 部分：数据探索](https://www.kdnuggets.com/2022/02/building-visual-search-engine-part-1.html)

+   [构建视觉搜索引擎 - 第 2 部分：搜索引擎](https://www.kdnuggets.com/2022/02/building-visual-search-engine-part-2.html)

+   [等级系统如何帮助预测 AI 成本](https://www.kdnuggets.com/2022/03/level-system-help-forecast-ai-costs.html)

+   [学习系统设计：五大必读书单](https://www.kdnuggets.com/learning-system-design-top-5-essential-reads)
