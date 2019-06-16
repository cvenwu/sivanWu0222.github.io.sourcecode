---
title: '[MLA-01]k-近邻算法初探'
date: 2019-06-07 10:37:26
tags: [ML, Machine Learning, K近邻算法] #标签(同级)
share: true  #分享信息
categories: #分类(分层)
    - ML
    - k近邻算法
reward: true #是否开启打赏功能
comment: true #是否开启评论功能
top: 2 #置顶层级(数字越大，优先级越高)
repo: ai-distill | MachineLearningPractice
src: //i.loli.net/2017/12/12/5a2fd18a74471.jpg #主页摘要缩略图(外链以及相对资源均可)
---


# 了解

k 邻近算法是《机器学习实战》中第一个讲到的机器学习算法，也是最容易理解的算法，不需要很多背景知识即可理解和上手。

## 概述
> k-近邻算法采用测量不同特征值之间的距离进行分类

优点：精度高，对异常值不敏感，无数据输入确定
缺点：计算复杂度高，空间复杂度高
适用数据范围：数值型和标称型

## 需要了解的一些知识
- k邻近算法只适用于分类算法(预测的值是离散的而不是连续的)
- k邻近算法属于监督学习


# 原理

对于要预测的某个数据点，从已带有标签的数据中选择k个距离该数据点最近的数据，并对其标签进行统计，频率最大的便是该数据点的标签，也就是我们预测的结果

> 书上的解释：存在一个样本数据集合，也称作训练样本集，并且样本集中每个数据都存在标签，即我们知道样本集中每个数据与所属分类的对应关系。输入没有标签的数据后，将新数据的每个特征值与样本集中每个数据特征进行比较，然后算法提取样本集中特征最相似数据(最近邻)的分类标签。一般来说，只选择样本数据集中前k个最相似的数据。最后选择k个最相似数据中出现次数最多的分类，作为新数据的分类。

# 一般流程
1. 收集数据
2. 准备数据：计算距离所需要的数值，最好是结构化的数据格式
3. 分析数据
4. 训练算法：此步骤不适用于k近邻算法
5. 测试算法：计算错误率
6. 使用算法：首先需要输入样本数据和结构化的输出结果，然后运行k近邻算判定输入数据分别属于哪个分类，最后应用对计算出的分类执行后续的处理


# 实现

对每个未知类别属性的数据集中的每个点依次执行如下操作：
1. 计算已知类别数据集中点与当前点之间的距离
2. 按照距离从小到大进行排序
3. 选取与当前点距离最小的k个点
4. 确定前k个点所在类别的出现频率
5. 返回前k个点出现频率最高的类别作为当前点的预测分类


## 准备数据

```python
# 导入科学计算包
from numpy import *
# 导入运算符模块，k近邻算法执行排序操作时将会使用这个模块提供的函数
import operator


def createDataSet():
    group = array([[1.0, 1.1], [1.0, 1.0], [0, 0], [0, 0.1]])
    labels = ['A', 'A', 'B', 'B']
    return group, labels
```

## 实现KNN分类算法

> 采用欧式距离公式计算两个数据点之间的距离，从小到大排序，确定前k个最小距离元素所对应的分类

```python
def classify(inX, dataSet, labels, k):
  """
  根据欧式距离公式计算出的距离从小到大排序，返回前k个元素
  :param inX: 用于分类的输入向量
  :param dataSet: 输入的训练样本集
  :param labels: 标签向量
  :param k: 标签向量元素的个数或训练集中数据的行数
  :return:
  """
  dataSetSize = dataSet.shape[0]
  # 距离计算
  diffMat = tile(inX, (dataSetSize, 1)) - dataSet
  sqDiffMat = diffMat ** 2
  sqDistances = sqDiffMat.sum(axis=1)
  distances = sqDistances ** 0.5
  sortedDistIndicies = distances.argsort()
  classCount = {}
  # 以下两行，选择距离最小的k个点
  for i in range(k):
      votellabel = labels[sortedDistIndicies[i]]
      classCount[votellabel] = classCount.get(votellabel, 0) + 1
      sortedClassCount = sorted(classCount.items(), key=operator.itemgetter(1), reverse=True)
  return sortedClassCount[0][0]
```


### numpy中的tile()方法


> 的功能是重复某个数组。比如tile(A,n)，功能是将数组A重复n次，构成一个新的数组
```python
num = [1, 2, 3]
tile(num, 2) # 返回一个数组，将num重复两次
tile(num, (1, 2)) # 返回一个1行并且将num重复两次的矩阵
tile(num, (2, 3)) # 返回一个两行并且每行将num重复3次的矩阵
```

结果是：

```python
[1, 2, 3, 1, 2, 3]
[[1, 2, 3, 1, 2, 3]]
[[1, 2, 3, 1, 2, 3, 1, 2, 3], [1, 2, 3, 1, 2, 3, 1, 2, 3]]

```






### 字典使用get()方法
```python
    classCount[votellabel] = classCount.get(votellabel, 0) + 1
```

作用：从classCount字典中获取votellabel的元素，如果不存在返回0


### operator模块的itemgetter()方法
> operator模块提供的itemgetter函数用于获取对象的哪些维的数据

```python
sortedClassCount = sorted(classCount.items(), key=operator.itemgetter(1), reverse=True)
```

operator.itemgetter函数~~获取的不是值~~，而是**定义了一个函数**，通过该函数作用到对象上才能获取值。




## 测试分类器

> k近邻算法的错误率在0-1之间， 错误率=错误次数/测试执行的总数

```python
group, labels = createDataSet()
print(classify([0, 0], group, labels, 3))
```


# 总结
上面就是一个简单的分类器的简单实现，采用欧式距离公式计算两个数据点的距离

## k近邻算法的缺点
1. 如果期望k近邻算法能够表现优秀，我们必须有接近实际数据的训练样本集
2. 由于必须对数据集中的每个数据计算距离值，在实际使用中(如需要实时计算或数据量超级大时)可能非常耗时
3. 无法给出任何数据的基础结构信息，因此我们无法知晓平均实例样本和典型实例样本具有什么特征
4. k近邻算法可以很好的完成分类任务，但是最大的缺点就是无法给出数据的内在含义


<!--more-->

