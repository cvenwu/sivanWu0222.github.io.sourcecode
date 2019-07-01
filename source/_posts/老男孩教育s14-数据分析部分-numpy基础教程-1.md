---
title: '老男孩教育s14-数据分析部分-numpy基础教程[1]'
tags:
  - Numpy
  - 老男孩全栈s14
  - 数据分析
  - Python
share: true
categories:
  - 数据分析
  - numpy
reward: true
comment: true
top: 2
repo: sivanWu0222 | OldBoyEduS14
date: 2019-06-23 18:15:31
description:
---


## numpy 数组

### 数组的创建

第一种方式：通过列表创建
第二种方式：通过部分函数进行numpy的创建，例如np.ones(), np.zeros()

> 一维数组的创建

```python
np.array([1, 2, 3])
```

> 二维数组的创建
```python
np.array([[1, 2, 3], [4, 5, 6]])          # 每个元素都为整形
np.array([[1, 2, 3], [4.0, 5, 6]])        # 每个元素都会转换为float
np.array([[1, 2, 3], [4.0, 'a', 6]])      # 每个元素都会转换为一个字符串
np.array([[1, 2, 3], [4.0, 'a']])         # 每行将会对应一个list 的 object



np.ones(img_arr.shape)
np.full((1, 10), fill_value=100)
np.linspace(1, 21, 21)  # 从1到21开始，进行21等分(等分的时候包括两个端点)
np.arange(0, 100, step=2) # 从0开始到100，步伐为2，将会得到0,2,4,...98
np.random.randint(0, 100, size=(5, 10)) # 产生5行10列的二维数组，每个元素都是0到100之间的随机数

# 固定时间种子，产生的随机数就固定下来了
np.random.seed(123)
np.random.randint(0, 100, (2, 10))


# 正态分布
np.random.randn(4, 5) # 返回一个4行5列的基于正态分布的数组
# rand方法会产生0-1的随机数，左闭右开
np.random.rand((3, 4)) # 生成一个3行4列的数组，
```


> 注意：
> - numpy 默认 ndarray 的所有元素类型相同
> - 如果传进来的列表包含不同类型，则统一类型，优先级 str > float > int
> - 产生随机数时，如果希望产生的随机数一直保持不变，可以定义一个随机数种子来确保随机数保持不变

### 属性
ndim 返回ndarray的维度
shape 返回ndarray的形状
dtype 返回ndarray的类型
size 返回总长度

```
arr.dtype
arr.ndim
arr.size
arr.shape
```


### 数组的索引

> 使用数组的索引

```python
""" 
array([[-0.37195736, -0.97033029, -0.80038168, -0.4547916 ],
       [-1.69906423,  1.63460351,  1.        ,  1.90131093],
       [ 1.47817555, -1.09057493,  1.28130256,  0.78534857]])
"""
# 使用索引修改数组内容
arr[1][2] = 1
arr[1, 2] = 2
```

### 数组的切片
> 数组倒序采用[::-1]
```python
import numpy as np
# 切片
arr = np.random.randint(60, 120, size=(6,4))
```

获取数组前2行：arr[:2] 
获取数组前2列：arr[:,:2]
获取数组前2行和前2列：arr[0:2, 0:2]
将数组的行倒序换成最后一行 arr[::-1]
将数组列倒序 arr[, ::-1]


### 其他方法
```python
import matplotlib.pyplot as plt
plt.imread() # 读取指定图片的内容,返回一个多维数组
plt.imshow() # 将返回的多维数组渲染成一个图片
im_arr.shape # 获得ndarray的形状, 返回一个元祖类型
```

