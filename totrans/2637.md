# 支持向量机用于手写字母识别（R语言）

> 原文：[https://www.kdnuggets.com/2021/01/support-vector-machine-hand-written-alphabet-r.html](https://www.kdnuggets.com/2021/01/support-vector-machine-hand-written-alphabet-r.html)

[评论](#comments)

**作者：[Mohan Rai](https://www.linkedin.com/in/mohan-rai-83499924/)，Simple & Real Analytics的主任**

撰写这篇文章的目的是使用一种非常粗略的方法进行图像分类，这里指的是手写文本的图像。虽然我们可以从头开始使用卷积神经网络模型或使用在MNIST数据集上训练好的模型，但它们更适合这个工作。我们使用迁移学习，在这个过程中，作为学生的我错过了非常基础的内容。这就像我在驾驶一辆自动挡汽车，我知道动力传动系统、离合器、加速器和刹车的作用，但不知道这些之外的东西。我们尝试将手写字母图像识别的问题分解成一个简单的过程，而不是使用复杂的软件包。这是一个创建数据并使用支持向量机进行分类的尝试。

### 数据准备

我们将手动准备数据，而不是从网上下载。这将使我们能够从初始阶段理解数据。我们将手动在白纸上写几个字母，并用手机相机拍摄。然后将其传输到硬盘上。由于这是一个实验，我不想在初始阶段花费太多时间，我会创建可能两个或三个不同字母的数据以作演示。建议你尝试这种机制来处理所有字母，查看效率。添加更多字母类别时可能需要修改代码，但这就是学习的开始。我们目前只是处于训练阶段。

### 数据存储结构

我们可以在白纸上写字母，然后用手机相机提取，或者直接使用像Paint这样的图形工具，用笔工具书写。我创建了两个文件夹：train和test。在train文件夹中，我们可以保存按字母命名的子文件夹，而在test文件夹中，我们可以放入要最终分类的图像。训练子文件夹的命名旨在作为训练标签。测试文件夹没有保持相同的形式，因为我们打算进行分类。

[![](../Images/d62488d42fefce1dbbc5833562a889ee.png)](/wp-content/uploads/svm-hand-written-r-data-strucutre.jpg)

如果你想下载我使用的数据，右击这个 [下载数据](http://www.imurgence.com//uploads/thumbnails/sample_data/alphabet_folder.zip) 链接，在新标签页或窗口中打开。然后解压文件夹，你应该能在下载文件夹中看到与上述相同的结构和数据。

之后你应该创建自己的数据并重复整个过程。这将使你接触到完整的周期。

### 在 RStudio 中下载依赖包

我们将使用 R 中的 jpeg 包进行图像处理，使用 kernlab 包中的支持向量机实现。这将是一次性安装。同时我们确保图像数据的尺寸为 200 x 200 像素，水平和垂直分辨率为 120dpi。你可以在当前形式实现后尝试不同的颜色通道和分辨率的变体。

```py
# install package "jpeg"
  install.packages("jpeg", dependencies = TRUE)

# install the "kernlab" package for building the model using support vector machines
  install.packages("kernlab", dependencies  = TRUE) 
```

### 加载训练数据集

```py
# load the "jpeg" package for reading the JPEG format files
  library(jpeg)
# set the working directory for reading the training image data set
  setwd("C:/Users/mohan/Desktop/alphabet_folder/Train")

# extract the directory names for using as image labels 
  f_train<-list.files()

# Create an empty data frame to store the image data labels and the extracted new features in training environment
  df_train<- data.frame(matrix(nrow=0,ncol=5)) 
```

### 特征工程

由于我们的意图是避免使用典型的 CNN 方法，我们将使用白色、灰色和黑色像素值来创建新特征。我们打算使用图像实例的所有像素值的总和，并将其保存为名为“sum”的特征，所有像素值为零的计数为“zero”，所有像素值为“ones”的计数，以及像素值为零以外和一以外值的总和作为“in_between”。“label”特征从文件夹名称中提取。

```py
 # names the columns of the data frame as per the feature name schema
  names(df_train)<- c("sum","zero","one","in_between","label")

# loop to compute as per the logic  
  counter<-1

  for(i in 1:length(f_train))
  {
    setwd(paste("C:/Users/mohan/Desktop/alphabet_folder/Train/",f_train[i],sep=""))

    data_list<-list.files()

    for(j in 1:length(data_list))
    {
      temp<- readJPEG(data_list[j])
      df_train[counter,1]<- sum(temp)
      df_train[counter,2]<- sum(temp==0)
      df_train[counter,3]<- sum(temp==1)
      df_train[counter,4]<- sum(temp > 0 & temp < 1)
      df_train[counter,5]<- f_train[i]
      counter=counter+1
    }
  }

# Convert the labels from text to factor form
  df_train$label<- factor(df_train$label) 
```

### 构建支持向量机模型

```py
# load the "kernlab" package for accessing the support vector machine implementation
  library(kernlab)

# build the model using the training data
  image_classifier <- ksvm(label~.,data=df_train) 
```

### 加载测试数据集

```py
# set the working directory for reading the testing image data set
  setwd("C:/Users/mohan/Desktop/alphabet_folder/Test")

# extract the directory names for using as image labels
  f_test <- list.files()

# Create an empty data frame to store the image data labels and the extracted new features in training environment
  df_test<- data.frame(matrix(nrow=0,ncol=5))

# Repeat of feature extraction in test data
  names(df_test)<- c("sum","zero","one","in_between","label")

# loop to compute as per the logic  
  for(i in 1:length(f_test))
    {
      temp<- readJPEG(f_test[i])
      df_test[i,1]<- sum(temp)
      df_test[i,2]<- sum(temp==0)
      df_test[i,3]<- sum(temp==1)
      df_test[i,4]<- sum(temp > 0 & temp < 1)
      df_test[i,5]<- strsplit(x = f_test[i],split = "[.]")[[1]][1]
    }

# Use the classifier named "image_classifier" built in train environment to predict the outcomes on features in Test environment
  df_test$label_predicted<- predict(image_classifier,df_test)

# Cross Tab for Classification Metric evaluation
  table(Actual_data=df_test$label,Predicted_data=df_test$label_predicted) 
```

我鼓励你通过点击此链接了解支持向量机的概念，这在本文中没有完全探讨，[免费数据科学和机器学习视频课程](https://www.imurgence.com/home/courses)。虽然我们使用了 kernlab 包并创建了分类器，但涉及了从向量空间到核技巧的许多数学知识。我们已经完成了分类器的实现，但你肯定要学习支持向量机的概念部分以及其他有趣的算法。

**简介: [Mohan Rai](https://www.linkedin.com/in/mohan-rai-83499924/)** 是 Simple & Real Analytics 的总监，负责咨询部门客户的分析交付。Simple & Real Analytics 专注于分析咨询和产品开发、机器学习和企业大数据解决方案。Simple & Real Analytics 的客户包括金融、生命科学、物流、数据获取、市场研究、制造和银行领域。除此之外，Mohan 还为他的教育科技创业公司 [Imurgence](https://www.imurgence.com/) 做贡献，并为一些其他公司提供咨询服务。

[原始内容](https://datascienceplus.com/support-vector-machine-for-hand-written-alphabet-recognition/)。经许可转载。

**相关：**

+   [顶级机器学习算法解析：支持向量机 (SVM)](/2020/03/machine-learning-algorithm-svm-explained.html)

+   [R 中简单直观的集成学习](/2020/12/simple-intuitive-meta-learning-r.html)

+   [支持向量机的友好介绍](/2019/09/friendly-introduction-support-vector-machines.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在的组织进行 IT 管理

* * *

### 更多相关主题

+   [今天 90% 的代码是为了防止失败，这成为了问题](https://www.kdnuggets.com/2022/07/90-today-code-written-prevent-failure-problem.html)

+   [支持向量机：直观方法](https://www.kdnuggets.com/2022/08/support-vector-machines-intuitive-approach.html)

+   [支持向量机的温和介绍](https://www.kdnuggets.com/2023/07/gentle-introduction-support-vector-machines.html)

+   [语义向量搜索如何改变客户支持互动](https://www.kdnuggets.com/how-semantic-vector-search-transforms-customer-support-interactions)

+   [Python 向量数据库和向量索引：构建大语言模型应用](https://www.kdnuggets.com/2023/08/python-vector-databases-vector-indexes-architecting-llm-apps.html)

+   [图像识别和自然语言处理中的迁移学习](https://www.kdnuggets.com/2022/01/transfer-learning-image-recognition-natural-language-processing.html)
