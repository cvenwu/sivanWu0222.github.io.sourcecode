---
title: OpenCV-图像篇
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
date: 2020-01-12 10:51:30
description: 
---

## OpenCV基础函数讲解
> 学习 使用 opencv in python 读取图像，显示图像，保存图像的功能，同时还学习了最简单的matplotlib绘图的功能(有点小瑕疵，后面讲)

**注意：彩色图像使用 OpenCV 加载时是 BGR 模式。但是 Matplotib 是 RGB 模式。所以彩色图像如果已经被 OpenCV 读取，那它将不会被 Matplotib 正确显示**

### waitKey() 函数讲解
waitKey(delay): 表示在delay毫秒内等待用户按键，若用户没有按键则每经过delay毫秒将会刷新一次

**功能是不断刷新图像，频率时间为delay，单位为ms，返回值为当前键盘按键值**

- waitKey()–是在一个给定的时间内(单位ms)等待用户按键触发; 如果用户没有按下键,则接续等待(循环)

常见：设置waitKey(0),则表示程序会无限制的等待用户的按键事件

一般在imgshow的时候，如果设置waitKey(0),代表按任意键继续

- 显示视频时，延迟时间需要设置为 大于0的参数

`delay > 0时，延迟”delay”ms，在显示视频时这个函数是有用的，用于设置在显示完一帧图像后程序等待”delay”ms再显示下一帧视频；如果使用waitKey(0)则只会显示第一帧视频, 显示视频时，延迟时间需要设置为大于0的参数, delay>0时，延迟”delay”ms，在显示视频时这个函数是有用的，用于设置在显示完一帧图像后程序等待”delay”ms再显示下一帧视频；如果使用waitKey(0)则只会显示第一帧视频`


[参考](https://blog.csdn.net/a1809032425/article/details/81952952)

<!--more-->

## OpenCV读取图像

`Note: 使用cv2.imread(filepath) ，如果文件路径错误，将不会报错而是返回一个None`

cv2.imread() 后面的几个常用读取模式：**读取图像正常情况下返回一个ndarray**
1. cv2.IMREAD_COLOR ： 正常读取彩色图 **读取的是BGR**
2. cv2.IMREAD_GRAYSCALE : 读取灰度图 
3. cv2.IMREAD_UNCHANGED : 读取包括图像的 alpha 通道的图

```python
"""
@File : 读入图像.py

@Author: sivan Wu

@Date : 2020/1/11 10:26 上午

@Desc : 使用cv2.imread()读取一幅图像
        警告：就算图像的路径是错的，OpenCV 也不会提醒你的， 但是当你使用命 令print img时得到的结果是None
"""

import cv2

IMAGE_PATH = "../data/timg.jpeg"

# 读入彩色图, 第二个参数是cv2.IMREAD_COLOR(默认为该参数)
# 读入一副彩色图像。 图像的透明度会被忽略， 这是默认参数
image = cv2.imread(IMAGE_PATH, cv2.IMREAD_COLOR)  # 返回的类型是<class 'numpy.ndarray'>， cv2.IMREAD_COLOR = 1
print("--------------彩色图像开始--------------")
print(image.shape)
print(type(image))
print("--------------彩色图像开始--------------")
# 读入灰色图像，第二个参数是cv2.IMREAD_GRAYSCALE
image_gray = cv2.imread(IMAGE_PATH, cv2.IMREAD_GRAYSCALE)  # cv2.IMREAD_GRAYSCALE = 0
print("--------------灰色图像开始--------------")
print(image_gray.shape)
print(type(image_gray))
print("--------------灰色图像开始--------------")
# cv2.IMREAD_UNCHANGED：读入一幅图像，并且包括图像的 alpha 通道
image_gray_with_alpha = cv2.imread(IMAGE_PATH, cv2.IMREAD_UNCHANGED)
print("--------------读取带alpha通道的图像开始--------------")
print(image_gray_with_alpha.shape)
print(type(image_gray_with_alpha))
print("--------------读取带alpha通道的图像开始--------------")
```


## OpenCV保存图像

> 如果原来文件存在则将会覆盖

```python
"""
@File : 保存图像.py

@Author: sivan Wu

@Date : 2020/1/11 10:50 上午

@Desc : 使用函数cv2.imwrite(param1, param2) param1是一个字符串的文件名字，param2是一个cv2读取到的ndarray对象

"""

import cv2

IMAGE_PATH = "../data/timg.jpeg"
image = cv2.imread(IMAGE_PATH, cv2.IMREAD_COLOR)  # cv2.IMREAD_GRAYSCALE = 0
cv2.imwrite("大头儿子.png", image)  # 如果原文件夹有该文件将会覆盖
```

## OpenCV显示图像


### 先读取图像再创建窗口
```python
"""
@File : 显示图像.py

@Author: sivan Wu

@Date : 2020/1/11 10:36 上午

@Desc : 显示图像：使用函数 cv2.imshow() 显示图像。窗口会自动调整为图像大小。第一 个参数是窗口的名字
，其次才是我们的图像。你可以创建多个窗口，只要你喜 欢，但是必须给他们不同的名字

"""

import cv2

IMAGE_PATH = "../data/timg.jpeg"

# 读入彩色图, 第二个参数是cv2.IMREAD_COLOR(默认为该参数)
# 读入一副彩色图像。 图像的透明度会被忽略， 这是默认参数
image = cv2.imread(IMAGE_PATH, cv2.IMREAD_COLOR)  # 返回的类型是<class 'numpy.ndarray'>， cv2.IMREAD_COLOR = 1
print("--------------彩色图像开始--------------")
print(image.shape)
print(type(image))
print("--------------彩色图像开始--------------")
# 读入灰色图像，第二个参数是cv2.IMREAD_GRAYSCALE
image_gray = cv2.imread(IMAGE_PATH, cv2.IMREAD_GRAYSCALE)  # cv2.IMREAD_GRAYSCALE = 0
print("--------------灰色图像开始--------------")
print(image_gray.shape)
print(type(image_gray))
print("--------------灰色图像开始--------------")

# 显示图像
print("--------------显示图像开始--------------")
cv2.imshow("灰色图像", image_gray)  #
cv2.imshow("彩色图像", image)  #
cv2.waitKey(0)
cv2.destroyWindow("灰色图像")  # 想删除特定的窗口可以使用 cv2.destroyWindow()，在括号内输入你想删除的窗口名
cv2.destroyAllWindows()  # 可以轻易删除所有我们建立的窗口
print("--------------显示图像结束--------------")

# 另外一种情形：首先创建窗口，之后加载图像
cv2.namedWindow("窗口", cv2.WINDOW_NORMAL)
cv2.imshow("窗口", image_gray)
cv2.waitKey(0)
cv2.destroyWindow("窗口")
```

### 先创建窗口再读取图像

> 一 种 特 殊 的 情 况 是， 你 也 可 以 先 创 建 一 个 窗 口， 之 后 再 加 载 图 像。 这 种 情 况 下，你 可 以 决 定 窗 口 是 否 可 以 调 整 大 小。 使 用 到 的 函 数 是 cv2.namedWindow()。 初 始 设 定 函 数标 签 是 cv2.WINDOW_AUTOSIZE。 但 是 如 果 你 把 标 签 改 成 cv2.WINDOW_NORMAL， 你就可以调整窗口大小了。
当图像维度太大， 或者要添加轨迹条时，调整窗口大小将会很有用

```python
"""
@File : 显示图像情形2.py

@Author: sivan Wu

@Date : 2020/1/11 10:44 上午

@Desc : 显示图像.py中我们首先读取图像之后才将创建窗口将图像送进去，现在我们可以先创建窗口，再将读取的图像送入
"""


import cv2

IMAGE_PATH = "../data/timg.jpeg"
image_gray = cv2.imread(IMAGE_PATH, cv2.IMREAD_GRAYSCALE)  # cv2.IMREAD_GRAYSCALE = 0
cv2.namedWindow("win_name", cv2.WINDOW_AUTOSIZE)
cv2.imshow("win_name", image_gray)
cv2.waitKey(0)
cv2.destroyWindow("win_name")
```



## 补充：使用matplotlib绘图

**注意：彩色图像使用 OpenCV 加载时是 BGR 模式。但是 Matplotib 是 RGB 模式。所以彩色图像如果已经被 OpenCV 读取，那它将不会被 Matplotib 正确显示**



```python

"""
@File : matplotlib绘图.py

@Author: sivan Wu

@Date : 2020/1/11 11:07 上午

@Desc :

"""

import matplotlib.pyplot as plt
import cv2

IMAGE_PATH = "../data/timg.jpeg"
image = cv2.imread(IMAGE_PATH, cv2.IMREAD_COLOR)
plt.imshow(image, cmap='gray', interpolation='bicubic')
plt.xticks([])
plt.yticks([])
plt.show()
```

## 总结


例子：加载一个灰度图，显示图片，按下’s’键保存后退出，或者 按下 ESC 键退出不保存\

**如果你用的是 64 位系统，你需要将 k = cv2.waitKey(0) 这行改成 k = cv2.waitKey(0)&0xFF**
        
```python
"""
@File : 总结一下.py

@Author: sivan Wu

@Date : 2020/1/11 11:00 上午

@Desc : 加载一个灰度图，显示图片，按下’s’键保存后退出，或者 按下 ESC 键退出不保存
        如果你用的是 64 位系统，你需要将 k = cv2.waitKey(0) 这行改成 k = cv2.waitKey(0)&0xFF
"""


import cv2

IMAGE_PATH = "../data/timg.jpeg"
image = cv2.imread(IMAGE_PATH, cv2.IMREAD_GRAYSCALE)
cv2.imshow("gray_image", image)
key = cv2.waitKey(0) & 0xFF  # 必选先点开图之后再按键盘
print(type(key))  # <class 'int'>
print(key)  # 119

if key == ord('s'):
    cv2.imwrite("保存的图像.png", image)
    cv2.destroyWindow("gray_image")
else:
    cv2.destroyWindow("gray_image")
```

### 使用opencv读取图像后用matplotlib进行显示


**注意：彩色图像使用 OpenCV 加载时是 BGR 模式。但是 Matplotib 是 RGB 模式。所以彩色图像如果已经被 OpenCV 读取，那它将不会被 Matplotib 正确显示**

**关键：将BGR转换为RGB**

解释：img[:,:,::-1]中括号中有两个逗号，四个冒号

[：，：，：：-1]

第一个冒号——取遍图像的所有行数

第二个冒号——取遍图像的所有列数

第三个和第四个冒号——取遍图像的所有通道数，-1是反向取值

所以，如果一幅300*100的彩色图像，

执行img[：，：，：：-1]后行列不变，通道数方向，由R、G、B更改为B、G、R，即第二轴反向

若是执行img[1：4，5：10，1：3：-1]后，第1行到第3行，第5列到第9列，第1通道到第2通道上的数据反向，即——第1行到第3行，第5列到第9列由R、G、B更改为R、B、G


> 第一种方法：
 
```python
"""
@File : 练习.py

@Author: sivan Wu

@Date : 2020/1/11 11:26 上午

@Desc : 当你用 OpenCV 加载一个彩色图像，并用 Matplotib 显示它时会遇 到一些困难。请阅读this discussion并且尝试理解它

"""
import numpy as np

import cv2

IMAGE_PATH = "../data/timg.jpeg"
image = cv2.imread(IMAGE_PATH, cv2.IMREAD_COLOR)
img2 = image[:,:,::-1]
import matplotlib.pyplot as plt
plt.imshow(img2)
plt.show()
```


> 第二种方法：使用cvtcolor()函数

功能说明：**cvtcolor()函数是一个颜色空间转换函数，可以实现RGB颜色向HSV，HSI等颜色空间转换。也可以转换为灰度图。**
 
```python
import numpy as np

import cv2

IMAGE_PATH = "../data/timg.jpeg"
image = cv2.imread(IMAGE_PATH, cv2.IMREAD_COLOR)
img2 = image[:,:,::-1]
import matplotlib.pyplot as plt
image = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
plt.imshow(image)
plt.show()
```

