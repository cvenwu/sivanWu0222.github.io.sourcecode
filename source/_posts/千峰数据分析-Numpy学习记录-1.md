---
title: '千峰数据分析-Numpy学习记录[1]'
tags:
  - Numpy
  - 数据分析
  - 数据挖掘
  - Data Analysis
  - Data Mining
share: true
categories:
  - 数据分析
  - Numpy
reward: true
comment: true
top: 2
repo: ai-distill | QF-Numpy
date: 2019-05-05 20:02:03
description: 
---

## 查看Numpy版本

```Python
import numpy as np
print(np.__version__)
```

## 使用Numpy创建ndarray对象

### 通过列表转换为ndarray对象 
```Python
import numpy as np
n1 = np.array([3, 1, 4, 5])
```
### 使用Numpy自带函数进行填充
np.ones((10, 8), dtype=int)  第1个参数是一个元祖，指定了创建ndarray的形状，这里表示10行8列

np.zeros((4, 4))  创建一个4行4列的形状的全0数组
np.full((10, 10), fill_value=1024) 创建一个10行10列的数组，使用1024对值进行填充
np.eye(10) 创建一个维度为10的方阵，并且主对角线全为1，也就是满秩
np.linspace(0, 100, 20)   从0开始到100，分为20个点，也就有19个空隙，每个空隙的大小为100/19
np.arange(0, 100, 2) 生成从0到100的随机数，但是不包括100，步长为2
np.random.randint(10) 生成10以内的随机数
np.random.randint(1, 10) 生成1-10以内的随机数
np.random.randint(1, 10, 5) 生成5个1-10以内的随机数
np.random.randn(100) 生成一个标准正态分布
np.random.normal(loc=175, scale=0, size=100) 生成一个自己自定义的一个正态分布，scale越大波动越大，这里取值为0时，将会生成100个175
np.random.random(100) 生成100个0-1的随机数
np.random.random([200, 300, 3]) 生成行200，列300，高3的三维0-1的随机数

## Numpy中关于JPG与PNG图像的读取
JPG与PNG是两种不同的图片格式，Numpy将JPG图用3维数组来进行表示，每个元素的取值都在0-255之间；PNG也被Numpy用3维数组来表示，与JPG不同的是每个元素的取值在0-1之间

```Python
import matplotlib.pyplot as plt
cat = plt.imread('D:/git仓库/QF-Numpy/cat.jpg')
cat
"""
结果如下：
array([[[231, 186, 131],
        [232, 187, 132],
        [233, 188, 133],
        ...,
        [100,  54,  54],
        [ 92,  48,  47],
        [ 85,  43,  44]],

       [[232, 187, 132],
        [232, 187, 132],
        [233, 188, 133],
        ...,
        [100,  54,  54],
        [ 92,  48,  47],
        [ 84,  42,  43]],

       [[232, 187, 132],
        [233, 188, 133],
        [233, 188, 133],
        ...,
        [ 99,  53,  53],
        [ 91,  47,  46],
        [ 83,  41,  42]],

       ...,

       [[199, 119,  82],
        [199, 119,  82],
        [200, 120,  83],
        ...,
        [189,  99,  65],
        [187,  97,  63],
        [187,  97,  63]],

       [[199, 119,  82],
        [199, 119,  82],
        [199, 119,  82],
        ...,
        [188,  98,  64],
        [186,  96,  62],
        [188,  95,  62]],

       [[199, 119,  82],
        [199, 119,  82],
        [199, 119,  82],
        ...,
        [188,  98,  64],
        [188,  95,  62],
        [188,  95,  62]]], dtype=uint8)
"""
plt.imread('./成绩.png')
"""
结果如下：
array([[[1., 1., 1.],
        [1., 1., 1.],
        [1., 1., 1.],
        ...,
        [1., 1., 1.],
        [1., 1., 1.],
        [1., 1., 1.]],

       [[1., 1., 1.],
        [1., 1., 1.],
        [1., 1., 1.],
        ...,
        [1., 1., 1.],
        [1., 1., 1.],
        [1., 1., 1.]],

       [[1., 1., 1.],
        [1., 1., 1.],
        [1., 1., 1.],
        ...,
        [1., 1., 1.],
        [1., 1., 1.],
        [1., 1., 1.]],

       ...,

       [[1., 1., 1.],
        [1., 1., 1.],
        [1., 1., 1.],
        ...,
        [1., 1., 1.],
        [1., 1., 1.],
        [1., 1., 1.]],

       [[1., 1., 1.],
        [1., 1., 1.],
        [1., 1., 1.],
        ...,
        [1., 1., 1.],
        [1., 1., 1.],
        [1., 1., 1.]],

       [[1., 1., 1.],
        [1., 1., 1.],
        [1., 1., 1.],
        ...,
        [1., 1., 1.],
        [1., 1., 1.],
        [1., 1., 1.]]], dtype=float32)
"""
```
<!--more-->