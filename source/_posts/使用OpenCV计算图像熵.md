---
title: 使用OpenCV计算一维和二维图像熵
tags:
  - OpenCV
  - Python
share: true
categories:
  - OpenCV
  - Image Entropy
reward: true
comment: true
top: 2
repo: 'sivanWu0222 | sivanWu0222 | OpenCV_Python'
date: 2020-01-20 12:33:46
description:
---

# 图像熵
熵用来衡量混乱情况，在图像中我们可以理解为图像可以取值的像素种类越多，则该图像熵越大。

## 一维图像熵的计算

### 一维灰度图像熵的计算

参考：https://blog.csdn.net/marleylee/article/details/78813630

> 图像熵是一种特征的统计形式，它反映了图像中平均信息量的多少。图像的一维熵表示图像中灰度分布的聚集特征所包含的信息量，令Pi 表示图像中灰度值为i的像素所占的比例，则定义灰度图像的一元灰度熵为：

**$H = -\sum\limits_{i=0}^{255}p_i * \log_2p_i$**

```python
def calc_entropy_gray_image(image):
    """
    计算灰度图的熵，使用最简单的熵计算公式
    :param image: 使用opencv读取的灰度图
    :return:
    """
    hist = cv2.calcHist([image], [0], None, [256], [0, 256])
    hist = hist[1:]
    probablity = hist / np.sum(hist)
    print('sum of probability:', np.sum(probablity))
    result = np.sum(np.matmul(probablity.T, np.log2(probablity)))
    return -result
```



### 各个颜色空间的图像熵的计算

**每个分量的熵计算公式都采用上面所描述的公式**

> 返回的result是一个列表，包含每个颜色空间分量的熵，例如在HSV颜色空间中，将会返回一个包含3个元素的列表，包括H对应的熵，S对应的熵，V对应的熵

```python
def calc_image_entropy_HSV(image):
    """
    :param image:
    :return:
    """
    image_HSV = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)
    size = image_HSV.shape
    result = []
    for i in range(size[len(size) - 1]):
        hist = cv2.calcHist([image_HSV], [i], None, [256], [0, 256])
        hist = hist[hist > 0]
        # print(hist)
        probablity = hist / np.sum(hist)
        print('sum of probability:', np.sum(probablity))
        result.append(-(np.matmul(probablity.T, np.log2(probablity))))
    print(result)
    return result


def calc_image_entropy_YCrCb(image):
    """
    :param image:
    :return:
    """
    image_YCrCb = cv2.cvtColor(image, cv2.COLOR_BGR2YCrCb)
    size = image_YCrCb.shape
    result = []
    for i in range(size[len(size) - 1]):
        hist = cv2.calcHist([image_YCrCb], [i], None, [256], [0, 256])
        hist = hist[hist > 0]
        # print(hist)
        probablity = hist / np.sum(hist)
        print('sum of probability:', np.sum(probablity))
        result.append(-(np.matmul(probablity.T, np.log2(probablity))))
    print(result)
    return result


def calc_image_entropy_HLS(image):
    """
    :param image:
    :return:
    """
    image_HLS = cv2.cvtColor(image, cv2.COLOR_BGR2HLS)
    size = image_HLS.shape
    result = []
    for i in range(size[len(size) - 1]):
        hist = cv2.calcHist([image_HLS], [i], None, [256], [0, 256])
        hist = hist[hist > 0]
        # print(hist)
        probablity = hist / np.sum(hist)
        print('sum of probability:', np.sum(probablity))
        result.append(-(np.matmul(probablity.T, np.log2(probablity))))
    print(result)
    return result
```




## 二维图像熵的计算

### 二维图像熵

图像的一维熵可以表示图像灰度分布的聚集特征，却不能反映图像灰度分布的空间特征，为了表征这种空间特征，可以在一维熵的基础上引入能够反映灰度分布空间特征的特征量来组成图像的二维熵。选择图像的邻域灰度均值作为灰度分布的空间特征量，与图像的像素灰度组成特征二元组，记为( i, j )，其中i 表示像素的灰度值(0 <= i <= 255)，j 表示邻域灰度均值(0 <= j <= 255)：

上式能反应某像素位置上的灰度值与其周围像素灰度分布的综合特征，其中f(i, j)为特征二元组(i, j)出现的频数，N 为图像的尺度。

参考：https://blog.csdn.net/marleylee/article/details/78813630

公式：**$H = -\sum\limits_{i}\sum\limits_{j}p_{(i,j)} * \log_2p_{(i,j)}$**


**其中$p(i, j) = f(i, j) / N ^ 2$**


代码对应的原理实现：
1. 对于一幅已知的图像我们在第一行，第一列，最后一行和最后一列填充0，经过一个3*3的平均滤波器核得到一个矩阵，此时矩阵宽度和高度等于原始图像的宽度和高度
2. 计算f(i, j) 并得到probability
3. 根据熵公式计算熵

例子： 一幅图像的像素为
```
1   2   3   4 
5   6   7   8
8   10  11  12
13  14  15  16
```
首先填充0，得到的结果为
```
0   0   0   0   0   0
0   1   2   3   4   0
0   5   6   7   8   0
0   9   10  11  12  0
0   13  14  15  16  0
0   0   0   0   0   0
```
经过一个3*3的均值滤波器核，等同于于神经网络中的卷积操作
```
14/9  23/9  30/9  22/9
33/9  54/9  63/9  45/9
57/9  90/9  99/9  69/9
46/9  72/9  78/9  54/9
```
计算f(i, j)
```
14/9对应的中心像素点的像素值为1, 因此f(1, 14/9) = 1
23/9对应的中心像素点的像素值为1, 因此f(2, 23/9) = 1
30/9对应的中心像素点的像素值为1, 因此f(3, 30/9) = 1
22/9对应的中心像素点的像素值为1, 因此f(4, 22/9) = 1
33/9对应的中心像素点的像素值为1, 因此f(5, 33/9) = 1
54/9对应的中心像素点的像素值为1, 因此f(6, 54/9) = 1
63/9对应的中心像素点的像素值为1, 因此f(7, 63/9) = 1
45/9对应的中心像素点的像素值为1, 因此f(8, 45/9) = 1
57/9对应的中心像素点的像素值为1, 因此f(9, 57/9) = 1
90/9对应的中心像素点的像素值为1, 因此f(10, 90/9) = 1
99/9对应的中心像素点的像素值为1, 因此f(11, 99/9) = 1
69/9对应的中心像素点的像素值为1, 因此f(12, 69/9) = 1
46/9对应的中心像素点的像素值为1, 因此f(13, 46/9) = 1
72/9对应的中心像素点的像素值为1, 因此f(14, 72/9) = 1
78/9对应的中心像素点的像素值为1, 因此f(15, 78/9) = 1
54/9对应的中心像素点的像素值为1, 因此f(16, 54/9) = 1
```

得到probability

```
[1/16, 1/16, 1/16, 1/16, 1/16, 1/16, 1/16, 1/16, 1/16, 1/16, 1/16, 1/16, 1/16, 1/16, 1/16, 1/16]
```
根据熵公式计算熵
```
H = - [(1/16) * log2(1/16)] * 16
```

结果：
```
概率之和： 1.0
熵: 4.0
```




代码实现：
```python
def calc_entropy_two_dims_gray(image_path, area=3):
    """
    计算图像空间二维熵：https://blog.csdn.net/marleylee/article/details/78813630
        p(i, j) = f(i, j) / N*N  其中i表示像素的灰度值(0 <= i <= 255)，j 表示邻域灰度均值(0 <= j <= 255)
    上式能反应某像素位置上的灰度值与其周围像素灰度分布的综合特征，其中f(i, j)为特征二元组(i, j)出现的频数，N 为图像的尺度。
        H = -sum_i(sum_j(p(i, j) * log2 p(i, j)))
    :param image: 要计算熵的图像
    :param area: 空间区域
    :return: 返回计算好的熵
    """
    image_gray = cv2.cvtColor(cv2.imread(image_path), cv2.COLOR_BGR2GRAY)
    # image_gray = np.array([[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12], [13, 14, 15, 16]])
    np.random.seed(12)
    original_height, original_width = image_gray.shape
    # 首行增加0
    image_gray = np.insert(image_gray, 0, values=0.0, axis=0)
    # 首列增加0
    image_gray = np.insert(image_gray, 0, values=0.0, axis=1)
    # 最后一行加全0
    image_gray = np.row_stack((image_gray, np.zeros([original_width + 1])))
    # 最后一列加全0
    image_gray = np.column_stack((image_gray, np.zeros([original_height + 2])))

    # 计算f(i, j)
    kernel = np.ones((area, area), np.float32) / (area ** 2)
    dst_image = cv2.filter2D(image_gray, -1, kernel)

    # 根据均值滤波器计算得到的像素均值
    dst_image = dst_image[1:-1, 1:-1]

    # 计算均值滤波器
    height, width = image_gray.shape
    f_i_j = []
    for i in range(height - 2):
        for j in range(width - 2):
            # 存储均值滤波器中心点位于原图的像素
            f_i = image_gray[i + 1, j + 1]
            # 存储均值滤波器计算时候的均值像素
            f_j = dst_image[i, j]
            temp = (f_i, f_j)
            f_i_j.append(temp)

    f_i_j_set = set(f_i_j)
    probability = []
    count = 1
    for ele in f_i_j_set:
        probability.append(f_i_j.count(ele))
        count += 1
    probability = np.array(probability, np.float32) / (original_width * original_height)
    print("概率之和：", np.sum(probability))
    # 统计位于列表中出现特征二元组的次数，同时计算概率 ,参考：https://segmentfault.com/q/1010000016716175?utm_source=tag-newest
    # for i in f_i_j_set:
    #     probability.append(f_i_j.count(i))
    # 根据计算出的概率和熵公式计算熵
    H = -np.matmul(probability, np.log2(probability).T)
    print("熵:", H)
    return H
```

<!--more-->

## 参考
[熵的计算](https://blog.csdn.net/marleylee/article/details/78813630)