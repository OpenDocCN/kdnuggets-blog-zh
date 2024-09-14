# 一系列机器学习模型的决策边界

> 原文：[`www.kdnuggets.com/2020/03/decision-boundary-series-machine-learning-models.html`](https://www.kdnuggets.com/2020/03/decision-boundary-series-machine-learning-models.html)

评论

**由[马修·史密斯](https://www.linkedin.com/in/msmithsm14/)，马德里康普顿斯大学**

### 边界上的机器学习：

机器学习模型超越传统计量经济学模型并不新鲜，但我想在我的研究中展示为什么以及如何一些模型进行给定的预测或在这种情况下的分类。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力。

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 管理。

* * *

我想展示我的二分类模型所做出的决策边界。也就是说，我想展示将我的分类划分为每个类别的分区空间。这个问题和代码可以通过一些调整分成多分类问题。

### 初始化：

我首先加载了一系列包，并初始化了一个逻辑函数，用于将对数几率转换为逻辑概率函数。

```py
library(dplyr)
library(patchwork)
library(ggplot2)
library(knitr)
library(kableExtra)
library(purrr)
library(stringr)
library(tidyr)
library(xgboost)
library(lightgbm)
library(keras)
library(tidyquant)
##################### Pre-define some functions ###################################
###################################################################################

logit2prob <- function(logit){
  odds <- exp(logit)
  prob <- odds / (1 + odds)
  return(prob)
}
```

### 数据：

我使用了`iris`数据集，该数据集包含由英国统计学家罗纳德·费舍尔在 1936 年收集的 3 种不同植物变量的信息。数据集由 44 种植物特征组成，这些特征应该能唯一地区分 33 种不同的植物种类（*Setosa*、*Virginica*和*Versicolor*）。然而，我的问题需要的是二分类问题而非多分类问题。在接下来的代码中，我导入了`iris`数据，并去除了一个植物种类`virginica`，将其从多分类问题转换为二分类问题。

```py
###################################################################################
###################################################################################

data(iris)
df <- iris %>% 
  filter(Species != "virginica") %>% 
  mutate(Species = +(Species == "versicolor"))
str(df)
```

```py
## 'data.frame':    100 obs. of  5 variables:
##  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
##  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
##  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
##  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
##  $ Species     : int  0 0 0 0 0 0 0 0 0 0 ...
```

```py
###################################################################################
###################################################################################
```

我通过首先存储`ggplot`对象来绘制数据，只改变每个`plt`中的`x`和`y`变量。

```py
plt1 <- df %>% 
  ggplot(aes(x = Sepal.Width, y = Sepal.Length, color = factor(Species))) +
  geom_point(size = 4) +
  theme_bw(base_size = 15) +
  theme(legend.position = "none")

plt2 <- df %>% 
  ggplot(aes(x = Petal.Length, y = Sepal.Length, color = factor(Species))) +
  geom_point(size = 4) +
  theme_bw(base_size = 15) +
  theme(legend.position = "none")

plt3 <- df %>% 
  ggplot(aes(x = Petal.Width, y = Sepal.Length, color = factor(Species))) +
  geom_point(size = 4) +
  theme_bw(base_size = 15) +
  theme(legend.position = "none")

plt3 <- df %>% 
  ggplot(aes(x = Sepal.Length, y = Sepal.Width, color = factor(Species))) +
  geom_point(size = 4) +
  theme_bw(base_size = 15) +
  theme(legend.position = "none")

plt4 <- df %>% 
  ggplot(aes(x = Petal.Length, y = Sepal.Width, color = factor(Species))) +
  geom_point(size = 4) +
  theme_bw(base_size = 15) +
  theme(legend.position = "none")

plt5 <- df %>% 
  ggplot(aes(x = Petal.Width, y = Sepal.Width, color = factor(Species))) +
  geom_point(size = 4) +
  theme_bw(base_size = 15) +
  theme(legend.position = "none")

plt6 <- df %>% 
  ggplot(aes(x = Petal.Width, y = Sepal.Length, color = factor(Species))) +
  geom_point(size = 4) +
  theme_bw(base_size = 15) +
  theme(legend.position = "none")
```

我还想使用新的`patchwork`包，这使得显示`ggplot`图非常容易。即下面的代码将我们的图形绘制出来（1 个顶部图延展整个网格空间，2 个中间图，另一个单独的图，以及底部的另外 2 个图）。

```py
 (plt1)    /
  (plt2 + plt3)
```

![](img/1b8077e556fd77387eefecc1d37dfdb5.png)

另外，我们可以根据需要重新排列这些图，并以以下方式绘制它们。

```py
 (plt1 + plt2) / 
  (plt5 + plt6)
```

![](img/21bf1cf183f88129d3cac813ff18747a.png)

我认为这看起来很棒。

### 目标

目标是构建一个分类算法，以区分这两类，并计算决策边界，以便更好地了解模型如何做出这些预测。

为了创建每个变量组合的决策边界图，我们需要数据中不同的变量组合。

```py
###################################################################################
###################################################################################

var_combos <- expand.grid(colnames(df[,1:4]), colnames(df[,1:4])) %>% 
  filter(!Var1 == Var2)

###################################################################################
###################################################################################
```

```py
var_combos %>% 
  head() %>% 
  kable(caption = "Variable Combinations", escape = F, align = "c", digits = 2) %>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"), font_size = 9, fixed_thead = T, full_width = F) %>% 
  scroll_box(width = "100%", height = "200px")
```

表 1：变量组合

| 变量 1 | 变量 2 |
| --- | --- |
| 花萼宽度 | 花萼长度 |
| 花瓣长度 | 花萼长度 |
| 花瓣宽度 | 花萼长度 |
| 花萼长度 | 花萼宽度 |
| 花瓣长度 | 花萼宽度 |
| 花瓣宽度 | 花萼宽度 |

接下来，我想使用之前的不同变量组合，创建列表（每个变量组合一个）并用合成数据填充这些列表——或者说从每个变量组合的最小值到最大值的数据。这将作为我们合成创建的测试数据，我们在其上进行预测并构建决策边界。

重要的是要注意，图形最终会是二维的以便于说明，因此我们只对两个变量训练机器学习模型，但对于每个变量组合，这些变量是`boundary_lists`数据框中的前两个变量。

```py
boundary_lists <- map2(
  .x = var_combos$Var1,
  .y = var_combos$Var2,
  ~select(df, .x, .y) %>% 
    summarise(
      minX = min(.[[1]], na.rm = TRUE),
      maxX = max(.[[1]], na.rm = TRUE),
      minY = min(.[[2]], na.rm = TRUE),
      maxY = max(.[[2]], na.rm = TRUE)
    )
) %>% 
  map(.,
      ~tibble(
        x = seq(.x$minX, .x$maxX, length.out = 200),
        y = seq(.x$minY, .x$maxY, length.out = 200),
      )
  ) %>% 
  map(.,
      ~tibble(
        xx = rep(.x$x, each = 200),
        yy = rep(.x$y, time = 200)
      )
  ) %>% 
  map2(.,
       asplit(var_combos, 1), ~ .x %>% 
         set_names(.y))

###################################################################################
###################################################################################
```

我们可以看看前四个观测值和最后两个列表的样子：

```py
boundary_lists %>% 
  map(., ~head(., 4)) %>%  
  head(2)
```

```py
## [[1]]
## # A tibble: 4 x 2
##   Sepal.Width Sepal.Length
##         <dbl>        <dbl>
## 1           2         4.3 
## 2           2         4.31
## 3           2         4.33
## 4           2         4.34
## 
## [[2]]
## # A tibble: 4 x 2
##   Petal.Length Sepal.Length
##          <dbl>        <dbl>
## 1            1         4.3 
## 2            1         4.31
## 3            1         4.33
## 4            1         4.34
```

```py
boundary_lists %>% 
  map(., ~head(., 4)) %>%  
  tail(2)
```

```py
## [[1]]
## # A tibble: 4 x 2
##   Sepal.Width Petal.Width
##         <dbl>       <dbl>
## 1           2       0.1  
## 2           2       0.109
## 3           2       0.117
## 4           2       0.126
## 
## [[2]]
## # A tibble: 4 x 2
##   Petal.Length Petal.Width
##          <dbl>       <dbl>
## 1            1       0.1  
## 2            1       0.109
## 3            1       0.117
## 4            1       0.126
```

### 训练时间：

现在我们已经设置了合成创建的测试数据集，我想用实际观察到的数据来训练模型。我使用上述图中的每个数据点作为训练数据。我应用以下模型：

+   逻辑模型

+   带线性核的支持向量机

+   带多项式核的支持向量机

+   带径向核的支持向量机

+   带 sigmoid 核的支持向量机

+   随机森林

+   具有默认参数的极端梯度提升（XGBoost）模型

+   单层 Keras 神经网络（含线性组件）

+   更深层的 Keras 神经网络（含线性组件）

+   更深层的 Keras 神经网络（含线性组件）

+   具有默认参数的轻量级梯度提升模型（LightGBM）

附注：我不是深度学习/Keras/Tensorflow 的专家，所以我相信更好的模型会产生更好的决策边界，但将不同的机器学习模型适配到`purrr`、`map`调用中是一个有趣的任务。

```py
###################################################################################
###################################################################################
# params_lightGBM <- list(
#   objective = "binary",
#   metric = "auc",
#   min_data = 1
# )

# To install Light GBM try the following in your RStudio terinal

# git clone --recursive https://github.com/microsoft/LightGBM
# cd LightGBM
# Rscript build_r.R

models_list <- var_combos %>%
  mutate(modeln = str_c('mod', row_number()))  %>%
  pmap(~ 
         {

           xname = ..1
           yname = ..2
           modelname = ..3
           df %>%
             select(Species, xname, yname) %>%
             group_by(grp = 'grp') %>%
             nest() %>%
             mutate(models = map(data, ~{

               list(
                 # Logistic Model
                 Model_GLM = {
                   glm(Species ~ ., data = .x, family = binomial(link='logit'))
                 },
                 # Support Vector Machine (linear)
                 Model_SVM_Linear = {
                   e1071::svm(Species ~ ., data = .x,  type = 'C-classification', kernel = 'linear')
                 },
                 # Support Vector Machine (polynomial)
                 Model_SVM_Polynomial = {
                   e1071::svm(Species ~ ., data = .x,  type = 'C-classification', kernel = 'polynomial')
                 },
                 # Support Vector Machine (sigmoid)
                 Model_SVM_radial = {
                   e1071::svm(Species ~ ., data = .x,  type = 'C-classification', kernel = 'sigmoid')
                 },
                 # Support Vector Machine (radial)
                 Model_SVM_radial_Sigmoid = {
                   e1071::svm(Species ~ ., data = .x,  type = 'C-classification', kernel = 'radial')
                 },
                 # Random Forest
                 Model_RF = {
                   randomForest::randomForest(formula = as.factor(Species) ~ ., data = .)
                 },
                 # Extreme Gradient Boosting
                 Model_XGB = {
                   xgboost(
                     objective = 'binary:logistic',
                     eval_metric = 'auc',
                     data = as.matrix(.x[, 2:3]),
                     label = as.matrix(.x$Species), # binary variable
                     nrounds = 10)
                 },
                 # Kera Neural Network
                 Model_Keras = {
                   mod <- keras_model_sequential() %>% 
                     layer_dense(units = 2, activation = 'relu', input_shape = 2) %>% 
                     layer_dense(units = 2, activation = 'sigmoid')

                   mod %>% compile(
                     loss = 'binary_crossentropy',
                     optimizer_sgd(lr = 0.01, momentum = 0.9),
                     metrics = c('accuracy')
                   )
                   fit(mod, 
                       x = as.matrix(.x[, 2:3]),
                       y = to_categorical(.x$Species, 2),
                       epochs = 5,
                       batch_size = 5,
                       validation_split = 0
                   )
                   print(modelname)        
                   assign(modelname, mod)                      

                 },
                 # Kera Neural Network
                 Model_Keras_2 = {
                   mod <- keras_model_sequential() %>% 
                     layer_dense(units = 2, activation = 'relu', input_shape = 2) %>%
                     layer_dense(units = 2, activation = 'linear', input_shape = 2) %>%
                     layer_dense(units = 2, activation = 'sigmoid')

                   mod %>% compile(
                     loss = 'binary_crossentropy',
                     optimizer_sgd(lr = 0.01, momentum = 0.9),
                     metrics = c('accuracy')
                   )
                   fit(mod, 
                       x = as.matrix(.x[, 2:3]),
                       y = to_categorical(.x$Species, 2),
                       epochs = 5,
                       batch_size = 5,
                       validation_split = 0
                   )
                   print(modelname)        
                   assign(modelname, mod)                      

                 },
                 # Kera Neural Network                 
                 Model_Keras_3 = {
                   mod <- keras_model_sequential() %>% 
                     layer_dense(units = 2, activation = 'relu', input_shape = 2) %>% 
                     layer_dense(units = 2, activation = 'relu', input_shape = 2) %>%
                     layer_dense(units = 2, activation = 'linear', input_shape = 2) %>%
                     layer_dense(units = 2, activation = 'sigmoid')

                   mod %>% compile(
                     loss = 'binary_crossentropy',
                     optimizer_sgd(lr = 0.01, momentum = 0.9),
                     metrics = c('accuracy')
                   )
                   fit(mod, 
                       x = as.matrix(.x[, 2:3]),
                       y = to_categorical(.x$Species, 2),
                       epochs = 5,
                       batch_size = 5,
                       validation_split = 0
                   )
                   print(modelname)        
                   assign(modelname, mod)                      

                 },

                 # LightGBM model
                 Model_LightGBM = {
                   lgb.train(
                     data = lgb.Dataset(data = as.matrix(.x[, 2:3]), label = .x$Species),
                     objective = 'binary',
                     metric = 'auc',
                     min_data = 1
                     #params = params_lightGBM,
                     #learning_rate = 0.1
                   )
                 }

               )                      

             }                               
             ))                 

         }) %>% 
  map(
    ., ~unlist(., recursive = FALSE)
  )
```

### 测试时间：

现在模型已经训练好，我们可以开始对我们在`boundary_lists`数据中创建的合成数据进行预测。

```py
models_predict <- map2(models_list, boundary_lists, ~{
  mods <- purrr::pluck(.x, "models")
  dat <- .y
  map(mods, function(x)
    tryCatch({
      if(attr(x, "class")[1] == "glm"){   
        # predict the logistic model
        tibble(
          modelname = attr(x, "class")[1],
          prediction = predict(x, newdata = dat)
        ) %>% 
          mutate(
            prediction = logit2prob(prediction),
            prediction = case_when(
              prediction > 0.5 ~ 1,
              TRUE ~ 0
            )
          )
      }    
      else if(attr(x, "class")[1] == "svm.formula"){ 
        # predict the SVM model
        tibble(
          modelname = attr(x, "class")[1],
          prediction = as.numeric(as.character(predict(x, newdata = dat)))
        )
      }
      else if(attr(x, "class")[1] == "randomForest.formula"){  
        # predict the RF model
        tibble(
          modelname = attr(x, "class")[1],
          prediction = as.numeric(as.character(predict(x, newdata = dat)))
        )
      }    
      else if(attr(x, "class")[1] == "xgb.Booster"){      
        # predict the XGBoost model
        tibble(
          modelname = attr(x, "class")[1], 
          prediction = predict(x, newdata = as.matrix(dat), type = 'prob')
        ) %>% 
          mutate(
            prediction = case_when(
              prediction > 0.5 ~ 1,
              TRUE ~ 0
            )
          )
      }
      else if(attr(x, "class")[1] == "keras.engine.sequential.Sequential"){
        # Keras Single Layer Neural Network
        tibble(
          modelname = attr(x, "class")[1],
          prediction = predict_classes(object = x, x = as.matrix(dat))
        )
      }
      else if(attr(x, "class")[2] == "keras.engine.training.Model"){
        # NOTE:::: This is a very crude hack to have multiple keras NN models
        # Needs fixing such that the models are named better - (not like) - (..., "class")[2], ..., "class")[3]... and so on.  
        # Keras Single Layer Neural Network
        tibble(
          modelname = attr(x, "class")[2], # needs changing also.
          prediction = predict_classes(object = x, x = as.matrix(dat))
        )
      }
      else if(attr(x, "class")[1] == "lgb.Booster"){
        # Light GBM model
        tibble(
          modelname = attr(x, "class")[1],
          prediction = predict(object = x, data = as.matrix(dat), rawscore = FALSE)
        ) %>% 
          mutate(
            prediction = case_when(
              prediction > 0.5 ~ 1,
              TRUE ~ 0
            )
          )
      }
    }, error = function(e){
      print('skipping\n')
    }
    )
  )
}
) %>% 
  map(.,
      ~setNames(.,
                map(.,
                    ~c(.x$modelname[1]
                    )
                )
      )
  ) %>%
  map(.,
      ~map(.,
           ~setNames(.,
                     c(
                       paste0(.x$modelname[1], "_Model"),
                       paste0(.x$modelname[1], "_Prediction")
                     )
           )
      )
  )
```

### 校准数据

现在我们有了训练好的模型，以及预测结果，我们可以将这些预测`map`到数据中，然后使用`ggplot`绘制，再用`patchwork`进行排列！

```py
plot_data <- map2(
  .x = boundary_lists,
  .y = map(
    models_predict,
    ~map(.,
         ~tibble(.)
    )
  ),
  ~bind_cols(.x, .y)
)

names(plot_data) <- map_chr(
  plot_data, ~c(
    paste(
      colnames(.)[1],
      "and",
      colnames(.)[2],
      sep = "_")
  )
)
```

现在我们有了预测结果，可以创建`ggplots`。

```py
ggplot_lists <- plot_data %>%
  map(
    .,
    ~select(
      .,
      -contains("Model")
    ) %>%
      pivot_longer(cols = contains("Prediction"), names_to = "Model", values_to = "Prediction")
  ) %>%
  map(
    .x = .,
    ~ggplot() +
      geom_point(aes(
        x = !!rlang::sym(colnames(.x)[1]),
        y = !!rlang::sym(colnames(.x)[2]),
        color = factor(!!rlang::sym(colnames(.x)[4]))
      ), data = .x) +
      geom_contour(aes(
        x = !!rlang::sym(colnames(.x)[1]),
        y = !!rlang::sym(colnames(.x)[2]),
        z = !!rlang::sym(colnames(.x)[4])
      ), data = .x) +
      geom_point(aes(
        x = !!rlang::sym(colnames(.x)[1]),
        y = !!rlang::sym(colnames(.x)[2]),
        color = factor(!!rlang::sym(colnames(df)[5]))  # this is the status variable
      ), size = 8, data = df) +
      geom_point(aes(
        x = !!rlang::sym(colnames(.x)[1]),
        y = !!rlang::sym(colnames(.x)[2])
      ), size = 8, shape = 1, data = df) +
      facet_wrap(~Model) +
      theme_bw(base_size = 25) +
      theme(legend.position = "none")
  )
```

绘制所有不同的决策边界组合。**注意**：上述代码在您的控制台中效果更好，当我运行代码编译博客时，图形太小了。因此，我提供了模型和变量组合的单独样本图。

我首先需要选择前两列，即感兴趣的变量（花瓣宽度、花瓣长度、萼片宽度和萼片长度）。然后我想对其后的列（即不同的机器学习模型预测）进行随机抽样。

```py
plot_data_sampled <- plot_data %>% 
  map(
    .,
    ~select(
      .,
      -contains("Model")
    ) %>% 
      select(.,
             c(1:2), sample(colnames(.), 2)
             ) %>% 
      pivot_longer(
        cols = contains("Prediction"),
        names_to = "Model",
        values_to = "Prediction")
    ) 
```

接下来我可以通过从列表中随机抽样来制作图表。

```py
plot_data_sampled %>% 
  rlist::list.sample(1) %>% 
  map(
    .x = .,
    ~ggplot() +
      geom_point(aes(
        x = !!rlang::sym(colnames(.x)[1]),
        y = !!rlang::sym(colnames(.x)[2]),
        color = factor(!!rlang::sym(colnames(.x)[4]))
      ), data = .x) + 
      geom_contour(aes(
        x = !!rlang::sym(colnames(.x)[1]),
        y = !!rlang::sym(colnames(.x)[2]),
        z = !!rlang::sym(colnames(.x)[4])
      ), data = .x) +
      geom_point(aes(
        x = !!rlang::sym(colnames(.x)[1]),
        y = !!rlang::sym(colnames(.x)[2]),
        color = factor(!!rlang::sym(colnames(df)[5]))  # this is the status variable
      ), size = 3, data = df) +
      geom_point(aes(
        x = !!rlang::sym(colnames(.x)[1]),
        y = !!rlang::sym(colnames(.x)[2])
      ), size = 3, shape = 1, data = df) +
      facet_wrap(~Model) +
      #coord_flip() +
      theme_tq(base_family = "serif") +
      theme(
        #aspect.ratio = 1,
        axis.line.y = element_blank(),
        axis.ticks.y = element_blank(),
        legend.position = "bottom",
        #legend.title = element_text(size = 20),
        #legend.text = element_text(size = 10),
        axis.title = element_text(size = 20),
        axis.text = element_text(size = "15"),
        strip.text.x = element_text(size = 15),
        plot.title = element_text(size = 30, hjust = 0.5),
        strip.background = element_rect(fill = 'darkred'),
        panel.background = element_blank(),
        panel.grid.major = element_blank(),
        panel.grid.minor = element_blank(),
        #axis.text.x = element_text(angle = 90),
        axis.text.y = element_text(angle = 90, hjust = 0.5),
        #axis.title.x = element_blank()
        legend.title = element_blank(),
        legend.text = element_text(size = 20)
      )
  )
```

```py
## $Sepal.Width_and_Petal.Length
```

```py
## Warning: Row indexes must be between 0 and the number of rows (0). Use `NA` as row index to obtain a row full of `NA` values.
## This warning is displayed once per session.
```

![](img/f0fcc6e0ae7a8ae8efa079465e409ee9.png)

一些其他随机模型：

```py
## $Sepal.Width_and_Sepal.Length
```

![](img/a28b0c6d73afaef699016b5019757ae5.png)

```py
## $Sepal.Width_and_Sepal.Length
```

![](img/0b740f0c10626aa86bb96289baed5b95.png)

```py
## $Petal.Length_and_Sepal.Length
```

![](img/f326ef09871b151259e191ae8e8530ed.png)

```py
## $Petal.Width_and_Petal.Length
```

![](img/2fddfb7ddadda0f1f9a329327673d3d8.png)

```py
## $Petal.Length_and_Petal.Width
```

```py
## Warning in grDevices::contourLines(x = sort(unique(data$x)), y =
## sort(unique(data$y)), : todos los valores de z son iguales
```

```py
## Warning: Not possible to generate contour data
```

![](img/fe78f389f3683f2e013bc223a3c7b6ab.png)

自然地，线性模型形成了线性决策边界。看起来随机森林模型在数据上稍微过拟合了，而 XGBoost 和 LightGBM 模型能够形成更好、更具泛化能力的决策边界。Keras 神经网络表现不佳，因为它们需要更好的训练。

+   `glm` = 逻辑回归模型

+   `keras.engine.sequential.Sequential Prediction...18` = 单层神经网络

+   `keras.engine.sequential.Sequential Prediction...18` = 更深层的神经网络

+   `keras.engine.sequential.Sequential Prediction...22` = 更深层的神经网络

+   `lgb.Booster Prediction` = 轻量级梯度提升模型

+   `randomForest.formula Prediction` = 随机森林模型

+   `svm.formula Prediction...10` = 带有 Sigmoid 核的支持向量机

+   `svm.formula Prediction...12` = 带有径向核的支持向量机

+   `svm.formula Prediction...6` = 带有线性核的支持向量机

+   `svm.formula Prediction...8` = 带有多项式核的支持向量机

+   `xgb.Booster Prediction` = 极端梯度提升模型

在许多组合中，Keras 神经网络模型只是将所有观测值预测为特定类别（这再次是由于我对模型的调整不佳以及神经网络只有 100 个观测值用于学习和 40,000 个观测值用于预测）。也就是说，它将整个背景涂成了蓝色或红色，并且做出了许多误分类。在一些图表中，神经网络成功进行了完美分类，而在其他图表中，它做出了奇怪的决策边界。- 神经网络很有趣。

从图表的简要分析来看，简单的逻辑回归模型几乎完美地进行了分类。考虑到每个变量之间的关系都是线性可分的，这并不令人惊讶。然而，我更偏爱 XGBoost 和 LightGBM 模型，因为它们可以通过在目标函数中引入正则化来处理非线性关系，从而做出更稳健的决策边界。随机森林模型在这里失败了，因此它们的决策边界看起来虽然做得很好，但也略显不稳定和尖锐。

当然，随着更多变量和更高维度的引入，这些决策边界可能会变得显著更复杂和非线性。

```py
for(i in 1:length(plot_data)){
  print(ggplot_lists[[i]])
  }
```

### 结束说明：

我在一个亚马逊 Ubuntu EC2 实例上编写了这个模型，但当我在 Windows 系统上用 R 编译博客文章时遇到了一些问题。这些问题主要是由于安装 `lightgbm` 包和包版本造成的。使用以下包版本（即使用最新包版本）时，代码正常运行没有错误。

```py
sessionInfo()
```

```py
## R version 3.6.1 (2019-07-05)
## Platform: x86_64-w64-mingw32/x64 (64-bit)
## Running under: Windows 10 x64 (build 17763)
## 
## Matrix products: default
## 
## locale:
## [1] LC_COLLATE=Spanish_Spain.1252  LC_CTYPE=Spanish_Spain.1252   
## [3] LC_MONETARY=Spanish_Spain.1252 LC_NUMERIC=C                  
## [5] LC_TIME=Spanish_Spain.1252    
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
##  [1] tidyquant_0.5.7            quantmod_0.4-15           
##  [3] TTR_0.23-6                 PerformanceAnalytics_1.5.3
##  [5] xts_0.11-2                 zoo_1.8-6                 
##  [7] lubridate_1.7.4            keras_2.2.5.0             
##  [9] lightgbm_2.3.2             R6_2.4.1                  
## [11] xgboost_0.90.0.1           tidyr_1.0.0               
## [13] stringr_1.4.0              purrr_0.3.2               
## [15] kableExtra_1.1.0.9000      knitr_1.25.4              
## [17] ggplot2_3.2.1              patchwork_1.0.0           
## [19] dplyr_0.8.99.9000         
## 
## loaded via a namespace (and not attached):
##  [1] Rcpp_1.0.3           lattice_0.20-38      class_7.3-15        
##  [4] utf8_1.1.4           assertthat_0.2.1     zeallot_0.1.0       
##  [7] digest_0.6.24        e1071_1.7-2          evaluate_0.14       
## [10] httr_1.4.1           blogdown_0.15        pillar_1.4.3.9000   
## [13] tfruns_1.4           rlang_0.4.4          lazyeval_0.2.2      
## [16] curl_4.0             rstudioapi_0.10      data.table_1.12.8   
## [19] whisker_0.3-2        Matrix_1.2-17        reticulate_1.14-9001
## [22] rmarkdown_1.14       lobstr_1.1.1         labeling_0.3        
## [25] webshot_0.5.1        readr_1.3.1          munsell_0.5.0       
## [28] compiler_3.6.1       xfun_0.8             pkgconfig_2.0.3     
## [31] base64enc_0.1-3      tensorflow_2.0.0     htmltools_0.3.6     
## [34] tidyselect_1.0.0     tibble_2.99.99.9014  bookdown_0.13       
## [37] quadprog_1.5-7       randomForest_4.6-14  fansi_0.4.1         
## [40] viridisLite_0.3.0    crayon_1.3.4         withr_2.1.2         
## [43] rappdirs_0.3.1       grid_3.6.1           Quandl_2.10.0       
## [46] jsonlite_1.6.1       gtable_0.3.0         lifecycle_0.1.0     
## [49] magrittr_1.5         scales_1.0.0         rlist_0.4.6.1       
## [52] cli_2.0.1            stringi_1.4.3        xml2_1.2.2          
## [55] ellipsis_0.3.0       generics_0.0.2       vctrs_0.2.99.9005   
## [58] tools_3.6.1          glue_1.3.1           hms_0.5.1           
## [61] yaml_2.2.0           colorspace_1.4-1     rvest_0.3.4
```

**简介：[Matthew Smith](https://www.linkedin.com/in/msmithsm14/)** ([@MatthewSmith786](https://twitter.com/MatthewSmith786)) 是马德里康普顿斯大学的博士生。他的研究专注于应用于经济学和金融学的机器学习方法。[他撰写的主题](https://lf0.com/)涵盖了 R、Python 和 C++。

[原始文章](https://lf0.com/post/machine-learning-boundary-conditions/machine-learning-boundary-conditions/)。经许可转载。

**相关：**

+   R 编程入门

+   R 在 Cloud Run 上的无服务器机器学习

+   R 中音频文件处理的基础

### 更多相关话题

+   [时间序列分析：Python 中的 ARIMA 模型](https://www.kdnuggets.com/2023/08/times-series-analysis-arima-models-python.html)

+   [从零开始的机器学习：决策树](https://www.kdnuggets.com/2022/11/machine-learning-scratch-decision-trees.html)

+   [用 Python 和 Scikit-learn 简化决策树的可解释性](https://www.kdnuggets.com/2017/05/simplifying-decision-tree-interpretation-decision-rules-python.html)

+   [决策树算法详解](https://www.kdnuggets.com/2020/01/decision-tree-algorithm-explained.html)

+   [通过实现理解：决策树](https://www.kdnuggets.com/2023/02/understanding-implementing-decision-tree.html)

+   [讲述精彩的数据故事：可视化决策树](https://www.kdnuggets.com/2021/02/telling-great-data-story-visualization-decision-tree.html)
