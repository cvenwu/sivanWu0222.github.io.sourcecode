---
title: 实战PyTorch手写数字体识别
tags:
  - PyTorch
  - Deep Learning
share: true
categories:
  - Deep Learning
  - CNN
reward: true
comment: true
top: 2
repo: 'sivanWu0222 | PyTorchDL'
date: 2019-09-29 19:47:51
description:
---

> 开始学习深度学习，想通过被誉为**深度学习"Hello World"的LeNet5**进行实践，奈何从网络架构看不懂到最后编码没看懂，到现在的恍然大悟，这里面有很多坑不实践只知道网络结构永远是不够的，在学习编码LeNet5的过程中，遇到了很多坑，从网上找资料，五音不全的，总感觉少了很多东西，所以自己索性就总结了自己LeNet5的经验以及一些坑

## 网络架构
![LeNet5网络结构图](./CNN.png)


输入的图片是 28*28*1 : **分别表示28像素的高，28像素的宽，1个颜色通道**

第一层：卷积层：卷积核大小为6*28*28, (6表示卷积核的通道数，决定了输出内容的个数，1表示卷积核的通道为1，28)
第二层：池化层
第三层：卷积层
第四层：池化层
第五层：全连接层
第六层：全连接层
第七层：全连接层



## 数据介绍
> 实战LeNet5架构的数据已经由大神Yann LeCun备好，只需要通过PyTorch下载并加载就可以


训练集：提供了60000张28*28像素的黑白图(说明颜色通道为1)，60000个标签
测试集：提供了10000张28*28像素的黑白图，10000个标签

![展示其中的一个数据](./数据图展示.png)

## 一些坑
1. ReLU() 与 torch.relu() 真的不一样，可别搞混，自己当时就是搞混，以为是一样的，卡了半天
2. 



## 步骤
> 任何一个神经网络的实践数据收集都是起点，这里我们使用了经典的MNIST数据集，包括60000 个训练样本和 10000 个测试样本，样本大小都是1 * 28 * 28 (1是指黑白图，也就是只有1个颜色通道)

1. 数据收集
2. 数据处理
3. 网络搭建
4. 针对问题定义损失函数和优化器
5. 训练模型
6. 测试模型准确度


### 数据处理和数据收集
> PyTorch已经内置了MNIST数据集，因此我们只需要导入相应的包来下载数据集

**注意：数据集都是图像，我们在PyTorch中数据的计算都需要将数据转换为计算图的张量(可以理解为是我们现实中的一个常量)**

```python
import torchvision.datasets as datasets
import torchvision.transforms as transforms
from torch.utils.data import dataloader

# 将数据转换为计算图的张量，使用Compose可以将多种变换组合到一起，但是这里我们只将其转换为Tensor
transform = transforms.Compose([
    transforms.ToTensor()
])

# 加载对应的数据集
## root代表数据集存放的目录
## train代表是否是训练集，用来区分训练集和测试集
## download代表是否需要下载，为true代表如果在root中没有找到数据则进行下载
## transform代表对下载后加载进来的数据做哪些预处理操作
train_data = datasets.MNIST(root='./data', train=True, download=True, transform=transform)
test_data = datasets.MNIST(root='./data', train=False, download=True, transform=transform)


# 数据加载(使用MINI-Batch加载)：将上面已经加载的数据存档到一个迭代器中方便训练和测试的时候遍历
## 第1个参数代表加载哪个数据集
## batch_size指示每次加载多少张图
## shuffle指示加载数据的时候是否随机加载(打乱)
train_data_loader = dataloader.DataLoader(train_data, batch_size=BATCH_SIZE, shuffle=True)
test_data_loader = dataloader.DataLoader(test_data, batch_size=BATCH_SIZE, shuffle=True)
```

### 网络搭建
> 这里我们自定义了网络结构

```python
import torch.nn as nn

class MyCNN(nn.Module):
    def __init__(self):
        super(MyCNN, self).__init__()
        self.conv = nn.Sequential(
            # 输入 batch_size, 1, 28, 28
            nn.Conv2d(1, 3, 3, padding=2),
            nn.BatchNorm2d(3),
            nn.LeakyReLU(0.2),
            nn.MaxPool2d(2),
            # batch_size, 3, 15, 15
            nn.Conv2d(3, 16, 3, padding=1),
            nn.BatchNorm2d(16),
            nn.LeakyReLU(0.2),
            nn.MaxPool2d(2),
            # batch_size, 16, 7, 7
            nn.Conv2d(16, 32, 3, padding=1),
            nn.BatchNorm2d(32),
            nn.LeakyReLU(0.2),
            nn.MaxPool2d(2),
            # batch_size, 32, 3, 3
        )
        self.lin = nn.Linear(288, 10)

    def forward(self, input):
        y = self.conv(input)
        y = y.view(input.size(0), -1)
        y = self.lin(y)
        return y
```

### 定义损失函数和优化器
> Adam: 一种计算每个参数的自适应学习率的方法
> 分类问题常用的损失函数为交叉熵（Cross Entropy Loss。交叉熵描述了两个概率分布之间的距离，交叉熵越小说明两者之间越接近。

```python
# 定义损失函数以及优化器
loss_func = torch.nn.CrossEntropyLoss()  # 采用交叉熵损失
optimizer = torch.optim.Adam(net.parameters(), lr=LEARNING_RATE)  #采用学习率自己可以调节的Adam优化器
```

### 训练模型
```python
index_value = 0
# 训练集
for epoch in range(NUM_EPOCHS):
    for index, (data, label) in enumerate(train_data_loader):
        predict_label = net(data)
        loss = loss_func(predict_label, label)
        loss_value = loss.item()
        print("epoch:{}, index:{}, loss:{}".format(epoch, index, loss_value))
        opt = {
            'title': 'LeNet5_loss',
            'xlabel': 'index',
            'ylabel': 'loss'
        }
        vis.line(Y=np.array([loss_value]), X=np.array([index_value]), win='lenet5_loss', update='append', opts=opt)
        optimizer.zero_grad()
        loss.backward()
        index_value += 1
        optimizer.step()
```



### 测试模型

几个注意的地方：
1. **进行测试的时候一定要关闭求梯度，以及dropout(使用net.eval())**
2. 训练之后的网络，使用net(data)将会得到预测的输出, 例如在该任务中会输出大小为(batch_size, 10)的二维tensor, 每个batch_size对应的10个数字的可能性大小，数字最大的代表预测为该数字
3. 使用net(data).argmax()求得的是二维tensor的最大值，但我们想要的是(batch_size, 10)大小的tensor中每行的最大值对应的下标，因为就是我们预测的值，因此可以采用net(data).argmax(dim=1)
4. label == net(data).argmax(dim=1) 来判断是否与我们预测的一致，将会返回一个bool组成的tensor
5. (label == net(data).argmax(dim=1)).sum() 可以计算我们预测的有多少是准确的，只是局限在该batch_size下


<!--more-->

```python
with torch.no_grad():
    for data, label in test_data_loader:
        net.eval()
        #print(net(data)) # 得到的是一个128维(batch_size) × 10(每个batch_size中的每个tensor分别对应10个数字的几率)的tensor
        #print(max(net(data)[0]) # 得到第一个batch_size最大的值)
        #print(net(data).argmax(dim=1)) # 获得最大值对应的索引，也就是我们想要得到的一个结果
        
        #print(net(data)[0][net(data)[0].argmax(dim=1)])
        
        #print(label)
        
        accur_num = (label == net(data).argmax(dim=1)).sum() #得到有多少是计算准确的
        accur_sum += accur_num
        n += BATCH_SIZE
        print("本轮准确率为：%.6f，累计准确率为：%.6f" % ((accur_num * 1.0/ BATCH_SIZE), (accur_sum * 1.0/ n)))

```


### torch.argmax() 详解
```python
# 以下参考https://blog.csdn.net/qq_27261889/article/details/88613932
m = torch.tensor([[1, 2, 3], [4, 5, 6]])
m.argmax()#找到了全局的最大值
m.argmax(dim=0) #dim=0找意思就是行不要了，因此会找到每一列的最大值对应的序号返回
m.argmax(dim=1) #dim=1找意思就是列不要了，因此会找到每一行的最大值对应的序号返回

```

### 使用visdom进行可视化
> 上面训练网络使用了visdom画出损失函数图进行可视化



**注意使用vis.line()的时候, Y在前面，X在后面，并且需要将其转换为一维的np.array类型**
**如果需要在原图上更新，指定参数update='append'**
**win指定对应pane的名字**
**opts采用键值对的形式指定对应可视化pane的一些属性**


#### 启动
> python -m visdom.server

#### 可视化


```python
import visdom
vis = visdom.Visdom()
opt = {
    'title': 'LeNet5_loss',
    'xlabel': 'index',
    'ylabel': 'loss'
}
vis.line(Y=np.array([loss_value]), X=np.array([index_value]), win='lenet5_loss', update='append', opts=opt)
```


## References
1. [网络架构完全不懂的参考](https://zhuanlan.zhihu.com/p/30117574) 缺点：后面的几层讲解的不够全面
2. [visdom可视化](https://www.pytorchtutorial.com/using-visdom-for-visualization-in-pytorch/)
3. [visdom可视化2](https://blog.csdn.net/u012436149/article/details/69389610?utm_source=gold_browser_extension)
4. [visdom可视化3](https://blog.csdn.net/nanxiaoting/article/details/81395579)