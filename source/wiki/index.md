---
title: 学习总结
date: 2019-09-20 09:10:31
share: true  #分享信息
reward: true #是否开启打赏功能
comment: true #是否开启评论功能
top: 1 #置顶层级(数字越大，优先级越高)
---


# 常用工具总结


## 语法检查工具
1. Grammarly
2. Chrome 浏览器的扩展ginger
3. 1checker 

## 文献管理工具：
1. Note express
2. Endnote
3. mendeley(lr用)

## 科研画图工具
1. [高大上的网络结构图(在线画) ](http://alexlenail.me/NN-SVG/)


## mac 下安装dlib
> https://www.jianshu.com/p/3e0b7d1ddc56

## dlib库的使用
[dlib库的使用](https://www.cnblogs.com/AdaminXie/p/9032224.html)


## mac下使用不了cmake命令解决办法
> 从官网下载了cmake ，但是使用terminal 说cmake not found
> CMake http://www.cmake.org/ 
> 打开 home 目录下的 .bash_profile 文件加入下面两句：
```
     # Add Cmake Root to Path
     export CMAKE_ROOT=/Applications/CMake.app/Contents/bin/
     export PATH=$CMAKE_ROOT:$PATH
```

    

# 第一周总结(2019.09.16-2019.09.20)
## 本周总结
> 本周着重了解了机器学习的一些基本概念以及3种算法：线性回归(单元和多元)，逻辑回归，神经网络(了解了前向传播和反向传播的基本概念，公式还有待深入理解)，如代价函数，以及求解代价函数的两种方法(梯度下降(目前学习了批量梯度下降方法，了解了梯度下降的局限性，以及梯度下降的优化，其中涉及到了凸优化，还有梯度下降中学习率的选择)，正规方程)，其次学习到了特征缩放和特征归一化(特征缩放和特征归一化都会影响到梯度下降快慢)，其中在学习多元变量的线性回归时了解了决策界限，同时将前面学习到的梯度下降采用向量化方式表达，过拟合问题的处理(正则化：单变量线性回归的正则化，多变量线性回归的正则化，逻辑回归的正则化)

本周总结：http://naotu.baidu.com/file/e4ae17e243701e76784589a027b89f66?token=2233fd10dd9f4dc0

## 下周计划
目标：
1. 学习吴恩达老师的神经网络深度学习
2. PyTorch入门(完成LeNet手写数字识别)
3. 阅读老师给定的论文，同时阅读扩充论文1-2篇
