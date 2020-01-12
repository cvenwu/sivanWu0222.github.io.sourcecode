---
title: opencv-直方图统计与显示
tags:
  - OpenCV
  - Python
share: true
categories:
  - OpenCV
reward: true
comment: true
top: 2
repo: 'sivanWu0222 | sivanWu0222 | OpenCV_Python'
date: 2020-01-12 19:11:24
description:
---

使用opencv操作直方图需要进行的两个步骤：
1. 统计直方图
2. 显示直方图

# 直方图


什么是直方图呢？通过直方图你可以对整幅图像的灰度分布有一个整体的 了解。直方图的 x 轴是灰度值（0 到 255），y 轴是图片中具有同一个灰度值的 点的数目

直方图的绘制包括最核心的两步：**统计直方图，绘制直方图**

## 使用opencv 统计直方图
**函数 cv2.calcHist 可以帮助我们统计一幅图像 的直方图**

cv2.calcHist(images, channels, mask, histSize, ranges, hist, accumulate)

返回值：返回的hist 是一个 256x1 的数组，每一个值代表了与次灰度值对应的像素点数 目

参数解释：
1. images: 原图像（图像格式为 uint8 或 ﬂoat32）。当传入函数时应该 用中括号 [] 括起来，例如：[img]。

2. channels: 同样需要用中括号括起来，它会告诉函数我们要统计那幅图 像的直方图。如果输入图像是灰度图，它的值就是 [0]；如果是彩色图像 的话，传入的参数可以是 [0]，[1]，[2] 它们分别对应着通道 B，G，R。

3. mask: 掩模图像。要统计整幅图像的直方图就把它设为 None。但是如 果你想统计图像某一部分的直方图的话，你就需要制作一个掩模图像，并 使用它。（后边有例子）

4. histSize:BIN 的数目。也应该用中括号括起来，例如：[256]。

5. ranges: 像素值范围，通常为 [0，256]


**image.ravel() 将图像转换成一维数组**


<!--more-->


```python
"""
@File : opencv统计直方图.py

@Author: sivan Wu

@Date : 2020/1/12 4:07 下午

@Desc :

"""


import cv2
import matplotlib.pyplot as plt
image = cv2.imread("../data/angela.jpeg", cv2.IMREAD_GRAYSCALE)

## 使用opencv 统计直方图函数
## 返回的hist 是一个 256x1 的数组，每一个值代表了与次灰度值对应的像素点数 目
hist = cv2.calcHist([image], [0], None, [256], [0, 256])
print(hist)
plt.plot(hist)
plt.show()
```


## 使用numpy统计直方图

> Numpy 还 有 一 个 函 数 np.bincount()， 它 的 运 行 速 度 是 np.histgram 的 十 倍。 所 以 对 于 一 维 直 方 图， 我 们 最 好 使 用 这 个 函 数。 使 用 np.bincount 时 别 忘 了 设 置 minlength=256。 例 如， hist=np.bincount(img.ravel()，minlength=256)

`注意：OpenCV 的函数要比 np.histgram() 快 40 倍。 所以坚持使用 OpenCV 函数。`



例子：Numpy 中的函数 np.histogram() 也可以帮我们统计直方图

```python
"""
@File : numpy统计直方图.py

@Author: sivan Wu

@Date : 2020/1/12 4:13 下午

@Desc :

"""

import cv2
import numpy as np
import matplotlib.pyplot as plt
image = cv2.imread("../data/angela.jpeg")
hist = np.histogram(image.ravel(), 256, [0, 256])
plt.show()
print(hist)
print(len(hist))
```


## 使用opencv绘制直方图

由于步骤比较繁琐，[参考](https://github.com/Itseez/opencv/raw/master/samples/python2/hist.py)

## 使用matlotlib绘制直方图


### 绘制灰度的直方图

包括如下3部分内容：
1. opencv统计直方图和matplotlib显示
2. numpy.bincount()统计直方图和matplotlib显示
3. numpy.histogram()统计直方图和matplotlib显示


> Numpy 还 有 一 个 函 数 np.bincount()， 它 的 运 行 速 度 是 np.histgram 的 十 倍。 所 以 对 于 一 维 直 方 图， 我 们 最 好 使 用 这 个 函 数。 使 用 np.bincount 时 别 忘 了 设 置 minlength=256。 例 如， hist=np.bincount(img.ravel()，minlength=256)

`注意：OpenCV 的函数要比 np.histgram() 快 40 倍。 所以坚持使用OpenCV函数`



```python
"""
@File : matplotlib绘制灰色图的直方图.py

@Author: sivan Wu

@Date : 2020/1/12 4:20 下午

@Desc :

"""

import cv2
import matplotlib.pyplot as plt

image = cv2.imread("../data/angela.jpeg", cv2.IMREAD_GRAYSCALE)

## 使用opencv 统计直方图函数 和 matplotlib显示
## 返回的hist 是一个 256x1 的数组，每一个值代表了与次灰度值对应的像素点数 目
hist = cv2.calcHist([image], [0], None, [256], [0, 256])
print(hist)
plt.plot(hist)
plt.show()

import numpy as np

## 使用numpy统计直方图函数np.bincount() 和 matplotlib显示
image = cv2.imread("../data/angela.jpeg", cv2.IMREAD_GRAYSCALE)
hist2 = np.bincount(image.ravel(), minlength=256)
print(len(hist2))  # 256
print(type(hist2))  # <class 'numpy.ndarray'>
plt.plot(hist2)
plt.show()

## 使用numpy统计直方图函数np.histogram() 和 matplotlib显示
hist, bins = np.histogram(image.ravel(), 256, [0, 256])
print(type(hist))
print(len(hist))
plt.plot(hist)
plt.show()

```


### 绘制彩色图的直方图
> 绘制彩色图在不同颜色通道下的直方图




例如：绘制彩色图对应BGR三个颜色通道的直方图

```python

"""
@File : matplotlib绘制彩色图的直方图.py

@Author: sivan Wu

@Date : 2020/1/12 4:20 下午

@Desc :

"""

import cv2
import matplotlib.pyplot as plt

image = cv2.imread("../data/angela.jpeg")

color = ['b', 'g', 'r']
# 绘制彩色图不同通道的直方图
for i, col in enumerate(color):
    hist = cv2.calcHist([image], [i], None, [256], [0, 256])
    plt.plot(hist, color=col)
    plt.xlim([0, 256])  # 包括256
plt.show()
```


## 参考
[更多资源](https://www.cambridgeincolour.com/tutorials/histograms1.htm)
