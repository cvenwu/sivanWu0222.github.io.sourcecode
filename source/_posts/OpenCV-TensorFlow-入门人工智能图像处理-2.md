---
title: 'OpenCV+TensorFlow 入门人工智能图像处理[2]'
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
date: 2019-05-14 12:11:12
description:
---

## TensorFlow理解

### TensorFlow运算的实质
- tf本质 = 张量tensor + 计算图graphs
- 张量不仅指常量还包括变量，其实就是tensorflow中的数据
- 计算图其实就是数据操作的过程，构成了计算图
- 在tensorflow中，所有的计算图都要放入到会话session中来执行
- session可以理解为一个运算的交互环境
- session中所有的变量才可以进行操作，所以有一个init操作
- init操作实质也是一个计算图
- 如果不使用with语句打开session,最后需要手动close

```Python
# 初始化操作，实质也是一个计算图
init = tf.global_variables_initializer()
sess.run(init)
```

### 常量与变量的四则运算

#### 常数之间的四则运算
```Python
import tensorflow as tf
data1 = tf.constant(6)
data2 = tf.constant(2)
data_add = tf.add(data1, data2)
data_mul = tf.multiply(data1, data2)
data_sub = tf.subtract(data1, data2)
data_div = tf.divide(data1, data2)
with tf.Session() as sess:
    print(sess.run(data_add))
    print(sess.run(data_mul))
    print(sess.run(data_sub))
    print(sess.run(data_div))

```
执行结果
```shell
8
12
4
3.0
```

#### 常数与变量的四则运算

```python
import tensorflow as tf
data1 = tf.constant(6)
data2 = tf.Variable(2)
data_add = tf.add(data1, data2)
data_copy = tf.assign(data2, data_add) # 将data_add运算的结果追加到data2中
data_mul = tf.multiply(data1, data2)
data_sub = tf.subtract(data1, data2)
data_div = tf.divide(data1, data2)
init = tf.global_variables_initializer()
with tf.Session() as sess:
    # 所有的变量必须完成初始化才可以操作
    sess.run(init)
    print(sess.run(data_add))
    print(sess.run(data_mul))
    print(sess.run(data_sub))
    print(sess.run(data_div))
```
执行结果
```shell
8
12
4
3.0
```


```python
import tensorflow as tf
data1 = tf.constant(6)
data2 = tf.Variable(2)
data_add = tf.add(data1, data2)
data_copy = tf.assign(data2, data_add) # 将data_add运算的结果追加到data2中
data_mul = tf.multiply(data1, data2)
data_sub = tf.subtract(data1, data2)
data_div = tf.divide(data1, data2)
init = tf.global_variables_initializer()
with tf.Session() as sess:
    # 所有的变量必须完成初始化才可以操作
    sess.run(init)
    print(sess.run(data_add))
    print(sess.run(data_mul))
    print(sess.run(data_sub))
    print(sess.run(data_div))
    print("data2:", sess.run(data2))
    print("sess.run(data_copy) ", sess.run(data_copy)) # 将8赋值给data_copy
    print("data2:", sess.run(data2))
    # 将8+6赋值给data_copy, eval()方法类似于下面的代码，也是会话执行
    print("data_copy.eval()", data_copy.eval())  # 此时data_add已经为14
    print("data2:", sess.run(data2))
    print("tf.get_default_session().run(data_copy)", tf.get_default_session().run(data_copy))
    print("data2:", sess.run(data2))
    # 计算图执行得两种方式：
    # 1. sess.run(图名)
    # 2. 图名.eval()
```
执行结果
```shell
8
12
4
3.0
data2: 2
sess.run(data_copy)  8
data2: 8
data_copy.eval() 14
data2: 14
tf.get_default_session().run(data_copy) 20
data2: 20
```
<!-- more -->