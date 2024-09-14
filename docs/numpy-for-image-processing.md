# NumPy用于图像处理

> 原文：[https://www.kdnuggets.com/numpy-for-image-processing](https://www.kdnuggets.com/numpy-for-image-processing)

![NumPy用于图像处理](../Images/94513646e5d024dabc980fa9cecef53a.png)

图像来源于 [freepik](https://www.freepik.com/free-photo/combination-facial-features-concept_36029178.htm#fromView=search&page=1&position=19&uuid=e5fa53f4-8eb8-4a51-ac7c-bc74f85aa07d)

NumPy是Python中强大的图像处理工具。它允许你使用数组操作来处理图像。本文探讨了使用NumPy的几种图像处理技术。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行IT工作

* * *

## 导入库

我们必须导入所需的库：PIL、NumPy和Matplotlib。PIL用于打开图像。NumPy允许高效的数组操作和图像处理。Matplotlib用于可视化图像。

```py
import numpy as np
from PIL import Image
import matplotlib.pyplot as plt
```

## 裁剪图像

我们定义坐标以标记要从图像中裁剪的区域。新图像仅包含选定部分，丢弃其余部分。

```py
# Load the image using PIL (Python Imaging Library)
img = Image.open('cat.jpg')

# Convert the image to a NumPy array
img_array = np.array(img)

# Define the cropping coordinates
y1, x1 = 1000, 1000  # Top-left corner of ROI
y2, x2 = 2500, 2000  # Bottom-right corner of ROI
cropped_img = img_array[y1:y2, x1:x2]

# Display the original image and the cropped image
plt.figure(figsize=(10, 5))

# Display the original image
plt.subplot(1, 2, 1)
plt.imshow(img_array)
plt.title('Original Image')
plt.axis('off')

# Display the cropped image
plt.subplot(1, 2, 2)
plt.imshow(cropped_img)
plt.title('Cropped Image')
plt.axis('off')

plt.tight_layout()
plt.show() 
```

![裁剪图像](../Images/2fb66f7dbcc3909e72e85a7c6614a37a.png)

## 旋转图像

我们使用NumPy的**'rot90'**函数将图像数组逆时针旋转90度。

```py
# Load the image using PIL (Python Imaging Library)
img = Image.open('cat.jpg')

# Convert the image to a NumPy array
img_array = np.array(img)

# Rotate the image by 90 degrees counterclockwise
rotated_img = np.rot90(img_array)

# Display the original image and the rotated image
plt.figure(figsize=(10, 5))

# Display the original image
plt.subplot(1, 2, 1)
plt.imshow(img_array)
plt.title('Original Image')
plt.axis('off')

# Display the rotated image
plt.subplot(1, 2, 2)
plt.imshow(rotated_img)
plt.title('Rotated Image (90 degrees)')
plt.axis('off')

plt.tight_layout()
plt.show() 
```

![旋转图像](../Images/a081fe73d36876f650f6c5dc5e6a8bea.png)

## 翻转图像

我们使用NumPy的**'fliplr'**函数水平翻转图像数组。

```py
# Load the image using PIL (Python Imaging Library)
img = Image.open('cat.jpg')

# Convert the image to a NumPy array
img_array = np.array(img)

# Flip the image horizontally
flipped_img = np.fliplr(img_array)

# Display the original image and the flipped image
plt.figure(figsize=(10, 5))

# Display the original image
plt.subplot(1, 2, 1)
plt.imshow(img_array)
plt.title('Original Image')
plt.axis('off')

# Display the flipped image
plt.subplot(1, 2, 2)
plt.imshow(flipped_img)
plt.title('Flipped Image')
plt.axis('off')

plt.tight_layout()
plt.show() 
```

![翻转图像](../Images/1a246e242681af3078cf7e7a27046a65.png)

## 图像的负片

图像的负片是通过反转其像素值来制作的。在灰度图像中，每个像素的值从最大值（8位图像为255）中减去。在彩色图像中，这是为每个颜色通道单独完成的。

```py
# Load the image using PIL (Python Imaging Library)
img = Image.open('cat.jpg')

# Convert the image to a NumPy array
img_array = np.array(img)

# Check if the image is grayscale or RGB
is_grayscale = len(img_array.shape) < 3

# Function to create negative of an image
def create_negative(image):
    if is_grayscale:
        # For grayscale images
        negative_image = 255 - image
    else:
        # For color images (RGB)
        negative_image = 255 - image
    return negative_image

# Create negative of the image
negative_img = create_negative(img_array)

# Display the original and negative images
plt.figure(figsize=(10, 5))
plt.subplot(1, 2, 1)
plt.imshow(img_array)
plt.title('Original Image')
plt.axis('off')

plt.subplot(1, 2, 2)
plt.imshow(negative_img)
plt.title('Negative Image')
plt.axis('off')

plt.tight_layout()
plt.show() 
```

![负片图像](../Images/0e351b24bd7a7621b56296bd353c69e5.png)

## 二值化图像

二值化图像将其转换为黑白图像。每个像素根据阈值标记为黑色或白色。小于阈值的像素变为0（黑色），大于阈值的像素变为255（白色）。

```py
# Load the image using PIL (Python Imaging Library)
img = Image.open('cat.jpg')

# Convert the image to grayscale
img_gray = img.convert('L')

# Convert the grayscale image to a NumPy array
img_array = np.array(img_gray)

# Binarize the image using a threshold
threshold = 128
binary_img = np.where(img_array < threshold, 0, 255).astype(np.uint8)

# Display the original and binarized images
plt.figure(figsize= (10, 5))

plt.subplot(1, 2, 1)
plt.imshow(img_array, cmap='gray')
plt.title('Original Grayscale Image')
plt.axis('off')

plt.subplot(1, 2, 2)
plt.imshow(binary_img, cmap='gray')
plt.title('Binarized Image (Threshold = 128)')
plt.axis('off')

plt.tight_layout()
plt.show() 
```

![二值化图像](../Images/e4b7c0bad77818b6bfd37654d0aa951f.png)

## 颜色空间转换

颜色空间转换将图像从一种颜色模型转换为另一种。这是通过改变像素值的数组来完成的。我们使用RGB通道的加权和将彩色图像转换为灰度图像。

```py
# Load the image using PIL (Python Imaging Library)
img = Image.open('cat.jpg')

# Convert the image to a NumPy array
img_array = np.array(img)

# Grayscale conversion formula: Y = 0.299*R + 0.587*G + 0.114*B
gray_img = np.dot (img_array[..., :3], [0.299, 0.587, 0.114])

# Display the original RGB image
plt.figure(figsize=(10, 5))
plt.subplot(1, 2, 1)
plt.imshow(img_array)
plt.title('Original RGB Image')
plt.axis('off')

# Display the converted grayscale image
plt.subplot(1, 2, 2)
plt.imshow(gray_img, cmap='gray')
plt.title('Grayscale Image')
plt.axis('off')

plt.tight_layout()
plt.show() 
```

![颜色转换](../Images/b401b33889ddb0569abcc086abc5e8bf.png)

## 像素强度直方图

直方图显示了图像中像素值的分布。图像被展平为一维数组以计算直方图。

```py
# Load the image using PIL (Python Imaging Library)
img = Image.open('cat.jpg')

# Convert the image to a NumPy array
img_array = np.array(img)

# Compute the histogram of the image
hist, bins = np.histogram(img_array.flatten(), bins=256, range= (0, 256))

# Plot the histogram
plt.figure(figsize=(10, 5))
plt.hist(img_array.flatten(), bins=256, range= (0, 256), density=True, color='gray')
plt.xlabel('Pixel Intensity')
plt.ylabel('Normalized Frequency')
plt.title('Histogram of Grayscale Image')
plt.grid(True)
plt.show() 
```

![直方图](../Images/48edb31106b04096721b51cf892eb592.png)

## 图像掩膜

掩膜图像意味着根据规则显示或隐藏部分内容。标记为 1 的像素被保留，而标记为 0 的像素被隐藏。

```py
# Load the image using PIL (Python Imaging Library)
img = Image.open('cat.jpg')

# Convert the image to a NumPy array
img_array = np.array(img)

# Create a binary mask
mask = np.zeros_like(img_array[:, :, 0], dtype=np.uint8)
center = (img_array.shape[0] // 2, img_array.shape[1] // 2)
radius = min(img_array.shape[0], img_array.shape[1]) // 2  # Increase radius for a bigger circle
rr, cc = np.meshgrid(np.arange(img_array.shape[0]), np.arange(img_array.shape[1]), indexing='ij')
circle_mask = (rr - center [0]) ** 2 + (cc - center [1]) ** 2 < radius ** 2
mask[circle_mask] = 1

# Apply the mask to the image
masked_img = img_array.copy()
for i in range(img_array.shape[2]):  # Apply to each color channel
    masked_img[:,:,i] = img_array[:,:,i] * mask

# Displaying the original image and the masked image
plt.figure(figsize=(10, 5))

plt.subplot(1, 2, 1)
plt.imshow(img_array)
plt.title('Original Image')
plt.axis('off')

plt.subplot(1, 2, 2)
plt.imshow(masked_img)
plt.title('Masked Image')
plt.axis('off')

plt.tight_layout()
plt.show() 
```

![掩膜图像](../Images/b255bd686a67722dd9d15b6483165418.png)

## 总结

本文展示了使用 NumPy 处理图像的不同方法。我们使用了 PIL、NumPy 和 Matplotlib 来裁剪、旋转、翻转和二值化图像。此外，我们了解了创建图像负片、改变色彩空间、制作直方图以及应用掩膜。

**[Jayita Gulati](https://www.linkedin.com/in/jayitagulati1998/)** 是一位对构建机器学习模型充满热情的机器学习爱好者和技术作家。她拥有利物浦大学的计算机科学硕士学位。

### 更多相关内容

+   [图像识别和自然语言处理的迁移学习](https://www.kdnuggets.com/2022/01/transfer-learning-image-recognition-natural-language-processing.html)

+   [如何将 RGB 图像转换为灰度图像](https://www.kdnuggets.com/2019/12/convert-rgb-image-grayscale.html)

+   [使用卷积神经网络 (CNNs) 进行图像分类](https://www.kdnuggets.com/2022/05/image-classification-convolutional-neural-networks-cnns.html)

+   [在不标记图像的情况下找到图像中的图片](https://www.kdnuggets.com/2022/09/find-picture-image-without-marking.html)

+   [8 种最佳 Python 图像处理工具](https://www.kdnuggets.com/2022/11/8-best-python-image-manipulation-tools.html)

+   [使用 Tensorflow 训练图像分类模型指南](https://www.kdnuggets.com/2022/12/guide-train-image-classification-model-tensorflow.html)
