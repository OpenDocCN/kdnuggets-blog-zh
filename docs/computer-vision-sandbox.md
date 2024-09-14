# 创建你自己的计算机视觉沙箱

> 原文：[https://www.kdnuggets.com/2020/02/computer-vision-sandbox.html](https://www.kdnuggets.com/2020/02/computer-vision-sandbox.html)

[评论](#comments)

**由 [Waun Broderick](https://www.linkedin.com/in/waunbroderick/)，首席技术官，Gyroscopic 联合创始人**

![图示](../Images/a843abceab533ceaa0f49136b79823f1.png)

图片由 [Lorenzo Herrera](https://unsplash.com/@lorenzoherrera?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

难度级别：初级（ ★ ☆ ☆ ☆ ☆）

推荐前提：

+   熟悉 Python

+   CNN 的基础知识

开发成果：

+   一个网络爬虫

+   自动化数据增强工具

+   智能 CNN 吸收管道

+   CNN 模型训练师

我们将一起创建一个 CNN 沙箱，它可以收集图像、增强数据，并轻松改变架构，以便为各种项目快速而灵活地调整。

这个过程是为那些 CNN 构建经验很少的人编写的，因此会抽象掉高级细节，以便能够对一些基本组件有一个宏观的了解。同时，添加了一些非必要的步骤，以便可视化输出并提供过程的透明性。

这些步骤中的每一个都被编写为可以直接过渡到创建完全由你选择的模型，包含任何数量的分类；例如狗与猫、国家旗帜、热狗与非热狗。然而，准确性将大大取决于类别的数量和它们之间的相似性。

[**WaunBroderick/Search-Identify**](https://github.com/WaunBroderick/Search-Identify/blob/master/Search_N_Identify.ipynb)

永久链接 关闭 GitHub 是一个有超过 4000 万开发者共同协作的地方，用于托管和审查代码，管理…

### 数据收集

在构建 CNN 的初期，你可能还没有数据集来开始工作，这没关系！在开发过程中，有一个广泛接受的原则是，如果是手动且单调的过程，我们通常可以将任务自动化！所以，代替不断滚动浏览图片，我们将通过构建一个子程序来开始我们的旅程。

不过请放心，我们并没有在原本不存在的地方创建嵌套问题，而是确保我们能够创建出可以在本次演练中以及未来项目中使用的小段落。

![图](../Images/08fb74ada07424f843e64e38ce2e0ccb.png)

迷茫的男子在做心理数学

要开始这个过程，我们需要一个 Microsoft Azure API 密钥（应用程序编程接口密钥），它允许我们利用他们的服务能力，使这个过程更容易。通过访问[**Bing 图像搜索**](https://azure.microsoft.com/en-us/services/cognitive-services/bing-image-search-api/)页面，你可以注册一个免费试用版，你将获得一个 API 密钥，允许你抓取图像。

下载以下 Python 脚本；

[**WaunBroderick/MSB_ImageScraper**](https://github.com/WaunBroderick/MSB_ImageScraper)

你现在无法执行该操作。你在另一个标签页或窗口中登录。你在另一个标签页或…

打开你电脑上的[bing_image_scraper.py](https://github.com/WaunBroderick/MSB_ImageScraper/blob/master/search_bing_api.py)文件，并用你的 API 密钥、所需的限制进行修改，然后保存。

```py
# for results (maximum of 50 per request)
API_KEY = "{{API KEY}}"
MAX_RESULTS = 250
GROUP_SIZE = 50
```

在命令行/终端中调用文件，并传递**输出目录**和**搜索词**的参数。

```py
python ./bing_image_scraper.py  --query "{{TERM}}" --output {{DIR}}
```

重复这个过程，找到所有你希望为 CNN 模型准备的必要图像类别。接下来的代码将依赖于每个文件夹的名称为一个类别，并且所有图像都放在各自的文件夹中。

![图](../Images/cac712c8a870baf25ffbc01f1ddc912d.png)

建议的照片目录结构

建议你尽力保持每个类别中的照片数量大致相同，以避免模型偏向某一个类别。

最好浏览一下抓取到的图像，并删除那些不符合搜索标准的图像。由于其缺乏严格的规范，网络图像抓取工具常常会获取与搜索主题不直接相关的图像，如果不加以监控，最终会变成‘*垃圾进，垃圾出*’。

![图](../Images/3caf83f335e300f6893ca040d6d7574c.png)

男子在用吸尘器处理火焰

### 数据增强

你现在应该有一系列包含各自图像类别的目录。我们必须确保在构建模型时，数据集足够大，以便为模型提供足够的类别信息。数据样本的大小受许多因素的影响，如特征数量、分类器、图像特性等。然而，为了本教程的目的，我们将按你选择的倍数扩展数据集，而不深入讨论这个问题。

数据增强是可以在计算机视觉管道中使用的一步，它可以为否则同质的数据集添加一些噪声或变异，如：反射、旋转、模糊或扭曲。这一步不仅可以增加你的数据集，还能让你的 CNN 有机会从更多的数据变异中学习。

对于这一步，我们将使用 [*Keras*](https://keras.io/) 和 [*Tensorflow*](https://www.tensorflow.org/) 库（*请参阅 GitHub 页面以获取所有库的导入信息*）。开始时创建一个操作列表并分配目录，我们将指示在完整列表上执行的操作可以在 [这里](https://keras.io/preprocessing/image/) 找到。

```py
#A sereis of operations that were selected to apply to images to create a greater amount of image variation
datagen = ImageDataGenerator(
        rotation_range=40,
        width_shift_range=0.2,
        height_shift_range=0.2,
        rescale=1./255,
        shear_range=0.2,
        zoom_range=0.2,
        horizontal_flip=True,
        fill_mode='nearest')#root directory for data augmentation
rootdir = '/xxx/data/'
```

然后，我们编写如何期望我们的程序遍历我们创建的文件结构的说明。

```py
#Move through the designated root directory and traverse through the individual files inside
for subdir, dirs, files in os.walk(rootdir):
    print("working in " +subdir)
    #Iterate over only the compatible image files
    for file in files:
        if file.endswith(".jpg") or file.endswith(".jpeg") or file.endswith(".png"):
            #Loading the image
            img = load_img(subdir + "/" + file)
            #The Numpy array responsible for shape adhearance
            x = img_to_array(img) 
            x = x.reshape((1,) + x.shape)            # the .flow() command below generates batches of randomly transformed images
            # and saves the results to the `preview/` directory
            i = 0
            for batch in datagen.flow(x, batch_size=1,
                                      save_to_dir= subdir + "/", save_prefix='DA', save_format='jpg'):
                i += 1
                #Set to desired iterations
                if i > 3:
                    #Setting bound to break loop so to avoid indefinite run
                    break
```

运行脚本后，你的文件夹现在应该包含原始抓取的图像，以及具有你选择的不同变换的新图像。

![图示](../Images/23fe7b3872cb00893b748f1ddd19aaa6.png)

相同的蜘蛛侠互相指向对方

我们可以使用此目录文件夹结构和功能来将类别拆分到这些文件夹中，并轻松地将相应的图像链接到这些标签。

```py
#The number of class labels == total number of categories which == length of the folders in .dir
data=load_files(dataDir, load_content= False)
total_categories=len(data.category_names)# input image dimensions
img_rows, img_cols = 64, 64
dataDir= '{{INPUT YOUR TOP LEVEL IMAGE DIR}}'#The function responsible for connecting the image data with the categories created
def build_data(data):
    X = []
    y = []
    #Operations to assign the categories to data structures
    encoder = LabelBinarizer()
    encoded_dict=dict()
    hotcoded_label = encoder.fit_transform(data.category_names)
    #Matching the category target names to the labels
    for i in range(len(data.category_names)):
        encoded_dict[data.category_names[i]]=hotcoded_label[i]
    for country in os.listdir(dataDir):
        label=encoded_dict[country]
        #labeling the images
        for each_flag in os.listdir(dataDir+'/'+country): 
            actual_path = os.path.join(dataDir,country,each_flag)
            img_data = cv2.imread(actual_path)
            img_data = cv2.resize(img_data, (img_rows, img_cols))
            X.append(np.array(img_data))
            y.append(label)
    #Creating a test and training set split with space for declaed variables
    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.1, random_state=36)
    X_train = np.asarray(X_train)
    X_test = np.asarray(X_test)
    y_train = np.asarray(y_train)
    y_test = np.asarray(y_test)#returns the training and testing set
    return [X_train, X_test, y_train, y_test]
```

**模型构建**

然后，我们在其顺序池化和卷积层中创建 CNN 的架构。变量的大小过滤器、层结构、[正则化器](https://keras.io/regularizers/) 或 [核心层](https://keras.io/layers/core/) 也可以根据你的项目要求进行添加和修改。本项目使用了修改版的 VGG-16 网络架构作为基础，但可以更改为你选择或创建的任何架构。

![图示](../Images/a1ecfb8bd42b7caae4c82cf45da51aa0.png)

VGG-16 架构

```py
#epochs = number of passes through entire training set
epochs = 100def create_model(input_shape,classes):
    #Takes the specifications of the image inputted
    img_input = Input(shape=input_shape)
    #The following is an architecture structuring
    x = Conv2D(64, (3, 3), activation='relu', padding='same', name='block1_conv1')(img_input)x = MaxPooling2D((2, 2), strides=(2, 2), name='block1_pool')(x)
    x = Conv2D(128, (3, 3), activation='relu', padding='same', name='block2_conv1')(x)

    x = MaxPooling2D((2, 2), strides=(2, 2), name='block2_pool')(x)
    x = Conv2D(256, (3, 3), activation='relu', padding='same', name='block3_conv1')(x)

    x = MaxPooling2D((2, 2), strides=(2, 2), name='block3_pool')(x)
    x = Conv2D(64, (3, 3), activation='relu', padding='same', name='block4_conv1')(x)

    x = MaxPooling2D((2, 2), strides=(2, 2), name='block4_pool')(x)
    x = Conv2D(128, (3, 3), activation='relu', padding='same', name='block5_conv1')(x)

    x = MaxPooling2D((2, 2), strides=(2, 2), name='block5_pool')(x)

    x = Flatten(name='flatten')(x)
    x = Dense(512, activation='relu', name='fc1')(x)
    x = Dropout(0.2)(x)
    x = Dense(256, activation='relu', name='fc2')(x)
    x = Dropout(0.2)(x)
    x = Dense(classes, activation='softmax', name='final_output')(x)model = Model(img_input, x, name='flag')
    return model
```

然后我们将到目前为止构建的部分组合在一起，并将测试和训练集数据输入到 CNN 架构中。在这个示例中，使用了 [Adams 自适应学习率算法](https://machinelearningmastery.com/adam-optimization-algorithm-for-deep-learning/) 和 [分类交叉熵](https://gombru.github.io/2018/05/23/cross_entropy_loss/) 损失函数。在你自己的项目中，你应该考虑每一个选择。

```py
def train_model(x_train, x_test, y_train, y_test):
    input_shape=(img_rows,img_cols,3)
    model=create_model(input_shape,total_countries)
    adams=optimizers.Adam(lr=1e-4)
    model.compile(loss=’categorical_crossentropy’,
                optimizer= adams,
                metrics=[‘accuracy’])
    model.fit(x = x_train, y = y_train, epochs=epochs)
    score = model.evaluate(x_test, y_test, verbose=1)
    print(‘Test loss:’, score[0])
    print(‘Test accuracy:’, score[1])
    model.save(‘flagFinder.model’)   
    return model
```

恭喜！你已成功创建了第一个 CNN 模型和数据收集管道！

![图示](../Images/ab95188455e55147d3e7f808eadf95d8.png)

女人胜利地欢呼

以下是将构建的模型应用于新一组图像以测试其准确性的示例。还增加了一个额外的步骤，将结果打印到文本文件中以便于查看。

```py
def flag_identify(positiveDir):    #An array to keep the list of countries built from the data ingestion step
    countries = []    #iterate through the ingestion stage names and append them to a list for ease of labeling
    for i in range(len(datas.target_names)):
        i += 1
        countries.append(datas.target_names[i-1])    #Another data preperation step for image ingestion for prediction identification
    def prepare(filepath):
        IMG_SIZE = 64
        img_array = cv2.imread(filepath)
        new_array = cv2.resize(img_array, (IMG_SIZE, IMG_SIZE))
        return new_array.reshape(-1, IMG_SIZE, IMG_SIZE, 3)    #Load in the previously built model
    model = tf.keras.models.load_model("flagFinder.model")    #Traverses through the given directory containing test images
    for filename in os.listdir(positiveDir):
        #Accpets various photo formats that were processed previous
        if filename.endswith(".jpg") or filename.endswith(".jpeg") or filename.endswith(".png"):
            prediction = model.predict([prepare(positiveDir + filename)])
            topCountry = int(np.argmax(prediction, axis=1))
            secondCountry = int(np.argmax(prediction, axis=1)-1)
            #Writes predictions to a set directory for ease of view
            with open("/xxx/flags.txt", "a") as myfile:
                #Generic sentence for output both the filename and the TOP PREDICTION (can be adjusted for other predictions)
                myfile.write("A flag was found in photo: " + filename + ", It is most likely to from the Nation: " + countries[topCountry] +", or ," + countries[secondCountry] +"\n" )
```

现在你可以依次调用这些程序片段来促进管道的不同部分。在相关的 GitHub 示例中，还有一个额外的目标检测段，位于数据增强和 CNN 模型构建之间，以展示系统如何以模块化方式构建。

```py
def build_flag_identifier():
    x_train, x_test, y_train, y_test = build_data(datas)
    train_model(x_train, x_test, y_train, y_test)def main():
    build_flag_identifier()
    flag_identify({{DIR}})if __name__ == "__main__":
    main()
```

如果你将这个代码框架作为未来项目的起点，确保你研究整个过程中的技术决策变异性。尽管每一步都已简化，但每个算法和架构的权衡和决策将大大影响你的准确性和整体项目成功。CNN 不是一刀切的，应该根据每个项目和数据集的规格进行构建。

快乐编程！

**简介：[Waun Broderick](https://www.linkedin.com/in/waunbroderick/)** 是 Gyroscopic 的首席技术官兼联合创始人。他是一位充满热情的应用程序开发者、海军战争官员和社区建设者。

[原创](https://medium.com/swlh/create-your-own-computer-vision-sandbox-b7c6b8662151)。经许可转载。

**相关：**

+   [如何将 RGB 图像转换为灰度图像](/2019/12/convert-rgb-image-grayscale.html)

+   [如何将图片转换为数字](/2020/01/convert-picture-numbers.html)

+   [谷歌开源 MobileNetV3：改进移动计算机视觉模型的新思路](/2019/12/google-open-sources-mobilenetv3-improve-mobile-computer-vision-models.html)

### 更多相关内容

+   [计算机视觉的 TensorFlow - 迁移学习变得简单](https://www.kdnuggets.com/2022/01/tensorflow-computer-vision-transfer-learning-made-easy.html)

+   [探索计算机视觉的世界：介绍 MLM 最新…](https://www.kdnuggets.com/2024/01/mlm-discover-the-world-of-computer-vision-ebook)

+   [计算机视觉的 5 种应用](https://www.kdnuggets.com/2022/03/5-applications-computer-vision.html)

+   [数据管理需要了解的 6 件事及其重要性…](https://www.kdnuggets.com/2022/05/6-things-need-know-data-management-matters-computer-vision.html)

+   [KDnuggets 新闻 2022 年 3 月 9 日：5 步构建机器学习 Web 应用](https://www.kdnuggets.com/2022/n10.html)

+   [DINOv2：Meta AI 自监督计算机视觉模型](https://www.kdnuggets.com/2023/05/dinov2-selfsupervised-computer-vision-models-meta-ai.html)
