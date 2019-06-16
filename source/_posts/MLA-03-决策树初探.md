---
title: MLA-03-决策树初探
tags: [ML, Machine Learning, 决策树] #标签(同级)
share: true  #分享信息
categories: #分类(分层)
    - ML
    - 决策树
reward: true
comment: true
top: 2
repo: ai-distill | MachineLearningPractice
src: '//i.loli.net/2017/12/12/5a2fd18a74471.jpg'
date: 2019-06-10 18:53:00
description:
---

## 决策树背景知识了解
> 引用机器学习实战书上："决策树的一个重要任务是为了数据中所蕴含的知识信息"


### 决策树的一般流程

1. 收集数据：可以使用任何方法
2. 准备数据：树构造算法只适用于标称型数据，因此数值型数据必须离散化
3. 分析数据：可以使用任何方法，构造树完成之后，应该检查图形是否符合预期
4. 训练算法：构造树的数据结构
5. 测试算法：使用经验树计算错误概率
6. 使用算法：此步骤可以适用于任何监督学习算法，而使用决策树可以更好的理解数据的内在含义

