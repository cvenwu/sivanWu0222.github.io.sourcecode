---
title: 理解Numpy中np.meshgeid函数
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
repo: ai-distill | UsePyToDataAnal
date: 2019-04-16 10:18:51

---



## 前言

> 在学习利用Python进行数据分析的时候，遇到介绍的np.meshgrid函数，书上给的解释是：接受两个一维数组，并产生两个二维矩阵(分别对应于两个数组中所有的(x,y)对)

## 

```python
#coding:utf-8
import numpy as np
# 坐标向量
a = np.array([1,2,3])
# 坐标向量
b = np.array([7,8])
# 从坐标向量中返回坐标矩阵
# 返回list,有两个元素,第一个元素是X轴的取值,第二个元素是Y轴的取值
res = np.meshgrid(a,b)
#返回结果: [array([ [1,2,3] [1,2,3] ]), array([ [7,7,7] [8,8,8] ])]
```



[参考](<https://blog.csdn.net/littlehaes/article/details/83543459>)











<!--more-->







