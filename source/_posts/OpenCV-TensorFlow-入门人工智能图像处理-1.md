---
title: 'OpenCV+TensorFlow 入门人工智能图像处理[1]'
tags:
  - OpenCV
  - TensorFlow
  - 人工智能
  - CV
  - 
share: true
categories:
  - 计算机视觉
reward: true
comment: true
top: 2
repo: ai-distill | Imooc-OpenCVTensorFlowLearn
date: 2019-05-09 12:11:12
description:
---



## opencv

### opencv的导入

```python
import cv2
```



### 读取图像数据

imread()方法对图片进行数据的读取

1. 有2个参数，第1个参数是文件名
2. 第2个参数是图片的类型，如果当前参数为0，读取的是灰度图片，如果参数为1，读取进来的为彩色图片
3. 该方法完成图片的读取，返回的是当前图片的图像数据
4. 虽然表面上看只是图像读取，但是包含了几个步骤，文件的读取，图像格式的分析，图像数据的解码，数据的加载

<!--more-->

```python
import cv2
img = cv2.imread('./image0.jpg', 1)
(b, g, r) = img[100, 100]
print(b, g, r)
```

**opencv中读取图像后获得的像素点是bgr(blue, green, red)与rgb不同**



### 展示图像

imshow()对图片进行展示

1. 有2个参数，第1个参数是展示出的图像所在的窗体名称
2. 第2个参数是当前窗体展示的内容



```python
cv2.imshow('image', img)
# 防止一瞬间消w失掉后看不到图片
cv2.waitKey(0)
```



因为读取图片之后对图片进行显示的时候将会在一瞬间消失掉，因此我们需要stop，也就是采用了**cv2.waitKey(0)**



### 图像数据的写入

imwrite() 将数据写入到一张图片中

1. 有2个参数，第1个参数是 图片的名称
2. 第2个参数是 当前图片的数组

```python
# 此时的img是解码之后的数据
cv2.imwrite('image1.jpg', img)
type(img) # 将会是一个ndarray数组
```



### 图像质量

> 图像划分为jpg与png，两个采用不同的压缩格式



#### PNG与JPG压缩的区别：

1. jpg图片压缩将会损失图片的质量，也就是有损压缩，png图片压缩是无损压缩
2. jpg无法设置透明度，png可以设置透明度，不仅仅可以修改传统的RGB

```python
# 第3个参数用来描述当前图片的质量,对于jpg图片的压缩可以采用不同的压缩比来对其进行控制，范围是0-100
cv2.imwrite('imageTest2.jpg', img, [cv2.IMWRITE_JPEG_QUALITY, 50]) 

import cv2
img = cv2.imread('./image0.jpg', 1)
cv2.imwrite('imageTest.png', img, [cv2.IMWRITE_PNG_COMPRESSION, 0])
```



在imwrite()方法的第3个参数中：

1. 对于JPG 来说 数字越小，压缩比越高，图片质量越不清晰，压缩范围是0-100
2. 对于PNG 来说，数字越小，压缩比越低，图片质量越清晰 ，压缩范围是0-9 



## 坐标轴

{% image 坐标轴.png 糟了 opencv中的坐标轴%}   

x轴对应垂直高度的变化



## 图像基础知识

- 像素点(像素)：图像是由一个个方块组成，每个方块称为像素点，
- 每种颜色都是由RGB三种颜色分量组合而成。
- 像素点如何存储：在计算机中，每种颜色都可以通过RGB三种颜色进行合成，在8位颜色深度中，每个RGB对应的分量取值都是0-255
- 颜色深度：8位颜色深度，表示的是RGB每个分量中， 都有2^8个可能
- 对于png图片来说，每一个像素点除了RGB之外，还可能有α通道作为透明度
- 经常见到的颜色存储格式：RGB(red, green, blue) 、BGR(blue, green, red)



### 例子：

如果有一个图片，宽为640 高是480 ，表示图片在水平方向上有640个像素点，在竖直方向上有480个像素点，

图片未经压缩的大小计算： 大小 = 宽度 * 高度 * 3 * 颜色深度 (单位是bit)

除以8 转换为B 

首先是宽度*高度，得到多少个像素点，然后因为每个点是由RGB中的3个颜色分量组成，所以 再乘以3，每个颜色分量是8位的颜色深度，所以最后计算而来

<!-- more -->