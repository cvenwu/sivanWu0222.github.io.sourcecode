---
title: '[MLA-02]k-近邻算法初探'
date: 2019-06-10 10:37:26
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


## 预备知识

### 归一化特征值
> 有时候以为不同特征值的取值范围差距很大，在我们使用K近邻算法计算距离的时候，取值范围差距大的特征值将会对k近邻算法计算出的结果造成很大的影响，因此我们需要对所有特征值进行归一化


**机器学习实战一书中给出归一化的公式： newValue = (oldValue - min) / (max - min)**

其中 min 和 max 分别是数据集中该特征值对应的最小值和最大值


## 应用

### k近邻算法改进约会网站

事例来自于机器学习实战一书的2.2，


步骤如下：
1. 收集数据
2. 准备数据：对数据进行处理(包括上面提到的归一化处理)
3. 分析数据：使用matplotlib画图
4. 训练算法：此步骤不适用于k近邻算法
5. 测试算法：使用测试数据进行测试
6. 使用算法


测试样本和非测试样本的区别在于：测试样本是已经分类完成的数据，如果预测分类与实际分类不同，则标记为一个错误


数据集解释：
1. 每行数据代表1个顾客，其中有4列，分别对应 每年获得的飞行常客里程数， 玩视频游戏所耗时间百分比， 每周消费的冰激淋公升数， 最终的分类







```python
import operator
from numpy import *


def file2matrix(filename):
    """
    将文本记录转换为Numpy的解析程序
    :param filename:
    :return:
    """
    fr = open(filename)
    arrayOlines = fr.readlines()
    numberOfLines = len(arrayOlines)

    # 得到文件行数
    returnMat = zeros((numberOfLines, 3))
    classLabelVector = []
    index = 0

    # 以下三行用于解析文件到数据列表
    for line in arrayOlines:
        line = line.strip()
        listFromLine = line.split('\t')
        returnMat[index, :] = listFromLine[0:3]
        classLabelVector.append(int(listFromLine[-1]))
        index += 1
    return returnMat, classLabelVector


# 分析数据
def plo():
    import matplotlib
    import matplotlib.pyplot as plt
    fig = plt.figure()
    ax = fig.add_subplot(111)
    datingDataMat, datingLabels = file2matrix('data/datingTestSet2.txt')
    # ax.scatter(datingDataMat[:, 1], datingDataMat[:, 2])
    ax.scatter(datingDataMat[:, 1], datingDataMat[:, 2], 15.0*array(datingLabels), 15.0*array(datingLabels))
    plt.show()


def autoNorm(dataSet):
    # 传入一个0表示按照列取最小值
    minVals = dataSet.min(0)
    # 传入一个0表示按照列取最大值
    maxVals = dataSet.max(0)

    ranges = maxVals - minVals
    normDataSet = zeros(shape(dataSet))
    m = dataSet.shape[0]
    normDataSet = dataSet - tile(minVals, (m, 1))
    # 特征值相除
    normDataSet = normDataSet / tile(ranges, (m, 1))

    # Numpy中矩阵除法需要使用

    return normDataSet, ranges, minVals


def classify(inX, dataSet, labels, k):
    """
    k近邻算法的实现
    :param inX: 输入向量
    :param dataSet: 输入的训练样本集
    :param labels:  标签向量
    :param k:   选择最近邻居的数目
    :return:    返回k近邻算法执行后分类的结果
    """
    dataSetSize = dataSet.shape[0]

    # 进行距离计算
    ## 将其扩展为与标签个数相同的输入数据的矩阵，执行矩阵减操作时，直接对每个对应的元素互相减去
    diffMat = tile(inX, (dataSetSize, 1)) - dataSet
    sqDiffMat = diffMat ** 2
    sqDistances = sqDiffMat.sum(axis=1)
    distances = sqDistances ** 0.5

    # 进行统计
    sortedDistIndicies = distances.argsort()
    classCount = {}

    # 选择距离最小的k个点
    for i in range(k):
        votellabel = labels[sortedDistIndicies[i]]
        classCount[votellabel] = classCount.get(votellabel, 0) + 1

    sortedClassCount = sorted(classCount.items(), key=operator.itemgetter(1), reverse=True)
    return sortedClassCount[0][0]


def datingClassTest():
    hoRatio = 0.10
    datingDataMat, datingLabels = file2matrix('data/datingTestSet2.txt')
    normMat, ranges, minVals = autoNorm(datingDataMat)
    m = normMat.shape[0]
    numTestVecs = int(m * hoRatio)
    errorCount = 0.0
    for i in range(numTestVecs):
        classifierResult = classify(normMat[i, :], normMat[numTestVecs:m, :], datingLabels[numTestVecs:m], 3)
        print("The classifier came back with: %d, the real answer is: %d" % (classifierResult, datingLabels[i]))
        if (classifierResult != datingLabels[i]):
            errorCount += 1.0
    print("The total error rate is %f:" % (errorCount / float(numTestVecs)))


if __name__ == '__main__':
    datingClassTest()

```

### 手写数字识别


> 数据集是每个数字对应成32*32像素的文件，





思路：通过将每个数字对应的文件(32*32像素)转换为(1, 1024)的矩阵，并使用k近邻算法与训练集数据进行计算，选取距离最小的k个元素，并进行统计，得到最终结果。**需要预先对数据进行处理**

```python
import operator
from numpy import *
import os

def classify(inX, dataSet, labels, k):
    """
    k近邻算法的实现
    :param inX: 输入向量
    :param dataSet: 输入的训练样本集
    :param labels:  标签向量
    :param k:   选择最近邻居的数目
    :return:    返回k近邻算法执行后分类的结果
    """
    dataSetSize = dataSet.shape[0]

    # 进行距离计算
    ## 将其扩展为与标签个数相同的输入数据的矩阵，执行矩阵减操作时，直接对每个对应的元素互相减去
    diffMat = tile(inX, (dataSetSize, 1)) - dataSet
    sqDiffMat = diffMat ** 2
    sqDistances = sqDiffMat.sum(axis=1)
    distances = sqDistances ** 0.5

    # 进行统计
    sortedDistIndicies = distances.argsort()
    classCount = {}

    # 选择距离最小的k个点
    for i in range(k):
        votellabel = labels[sortedDistIndicies[i]]
        classCount[votellabel] = classCount.get(votellabel, 0) + 1

    sortedClassCount = sorted(classCount.items(), key=operator.itemgetter(1), reverse=True)
    return sortedClassCount[0][0]


def datingClassTest():
    hoRatio = 0.10
    datingDataMat, datingLabels = file2matrix('data/datingTestSet2.txt')
    normMat, ranges, minVals = autoNorm(datingDataMat)
    m = normMat.shape[0]
    numTestVecs = int(m * hoRatio)
    errorCount = 0.0
    for i in range(numTestVecs):
        classifierResult = classify(normMat[i, :], normMat[numTestVecs:m, :], datingLabels[numTestVecs:m], 3)
        print("The classifier came back with: %d, the real answer is: %d" % (classifierResult, datingLabels[i]))
        if (classifierResult != datingLabels[i]):
            errorCount += 1.0
    print("The total error rate is %f:" % (errorCount / float(numTestVecs)))


def img2vec(imgFile):
    """
    准备数据，将图像文件转换为1*1024的图像
    :param imgFile:
    :return:
    """
    vec = zeros((1, 1024))
    with open(imgFile) as f:
        content = f.readlines()
        line_number = len(content)
        for line_index in range(line_number):
            for index in range(len(content[line_index].strip())):
                vec[0, line_index*32 + index] = content[line_index].strip()[index]
    return vec


def handwritingClassTest():
    hwLabels = []
    trainingFileList = os.listdir('trainingDigits')

    # 获取目录内容
    m = len(trainingFileList)
    trainingMat = zeros((m, 1024))
    for i in range(m):
        # 从文件名解析分类数字
        fileNameStr = trainingFileList[i]
        fileStr = fileNameStr.split('.')[0]
        classNumStr = int(fileStr.split('_')[0])

        hwLabels.append(classNumStr)
        trainingMat[i, :] = img2vec('trainingDigits/%s' % fileNameStr)
        testFileList = os.listdir('testDigits')
        errorCount = 0.0
        mTest = len(testFileList)

    for i in range(mTest):
        fileNameStr = testFileList[i]
        fileStr = fileNameStr.split('.')[0]
        classNumStr = int(fileStr.split('_')[0])
        vectorUnderTest = img2vec('testDigits/%s' % fileNameStr)
        classifierResult = classify(vectorUnderTest, trainingMat, hwLabels, 3)
        print('the classifier came back with: %d' % classNumStr)
        if (classifierResult != classNumStr):
            errorCount += 1.0
    print('the total number of errors is: %d' % errorCount)
    print('the total error rate is: %f' % (errorCount / float(mTest)))


if __name__ == '__main__':
    handwritingClassTest()

```



## 总结：

缺陷：
1. 无法给出任何数据的基础结构信息，也不知道平均实例样本和典型实例样本具有什么特征。
2. k近邻算法必须保存全部数据集，如果训练数据集很大，将会需要大量的存储空间对数据进行保存，并且计算需要耗费大量时间，无法做到实时计算。


<!--more-->

