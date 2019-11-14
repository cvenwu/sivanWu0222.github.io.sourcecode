---
title: '[Paper_TL]A survery on transfer learning'
tags:
  - Paper
  - notes
share: true
categories:
  - Paper
  - Transfer Learning
reward: true
comment: true
top: 2
date: 2019-11-11 12:33:15
description:
---

## three main research issues of transfer learning
1. what to transfer
> “What to transfer” asks which part of knowledge can be transferred across domains or tasks. Some knowledge is specific for individual domains or tasks, and some knowledge may be common between different domains such that they may help improve performance for the target domain or task.

2. how to transfer
>  After discovering which knowledge can be transferred, learning algorithms need to be developed to transfer the knowledge, which corresponds to the “how to transfer” issue.

3. when to transfer
> Most current work on transfer learning focuses on “What to transfer” and “How to transfer”, by implicitly assuming that the source and target domains be related to each other. However, how to avoid negative transfer is an important open issue that is attracting more and more attention in the future

## 迁移学习怎么来的

The fundamental motivation for Transfer learning in the field of machine learning was
discussed in a NIPS-95 workshop on “Learning to Learn” 1, which focused on the need for lifelong machine-learning methods that retain and reuse previously learned knowledge

## How to transfer?(3种不同的迁移模式, based on the definition of transfer learning)
1. inductive transfer learning  (推导迁移学习)
2. transductive transfer learning (转导迁移学习)
3. unsupervised transfer learning (无监督迁移学习)
> Transfer learning is classified to three different settings: inductive transfer learning, transductive transfer learning and unsupervised transfer learning. Most previous works focused on the former two settings. Unsupervised transfer learning may attract more and more attention in the future.

![根据方式进行分类](根据方式分类.png)

### inductive learning:(任务不同，域相同)
the target task is different from the source task, no matter when the source and target domains are the same or not.

In this case, some labeled data in the target domain are required to induce an objective predictive model fT(·) for use in the target domain. In addition, according to different situations of labeled and unlabeled data in the source domain, we can further categorize the inductive transfer learning setting into two cases.

1. A lot of labeled data in the source domain are available
> In this case, the inductive transfer learning setting is similar to the multi-task learning setting. However, the inductive transfer learning setting only aims at achieving high performance in the target task by transferring knowledge from the source task while multi-task learning tries to learn the target and source task simultaneously
2. No labeled data in the source domain are available.
> In this case, the inductive transfer learning setting is similar to the self-taught learning setting, which is first proposed by Raina et al. [22]. In the self-taught learning setting, the label spaces between the source and target domains may be different, which implies the side information of the source domain cannot be used directly. Thus, it’s similar to the inductive transfer learning setting where the labeled data in the source domain are unavailable.

### transductive transfer learning(任务相同，域不同)
1. The feature spaces between the source and target domains are different,
2. The feature spaces between domains are the same but the marginal probability distributions of the input data are different. 
The latter case of the transductive transfer learning setting is related to domain adaptation for knowledge transfer in text classification [23] and sample selection bias [24] or co-variate shift [25], whose assumptions are similar.




### unsupervised transfer learning(任务不同但是相关)

In this situation, no labeled data in the target domain are available while a lot of labeled data in the source domain are available. In addition, according to different situations between the source and target domains, we can further categorize the transductive transfer learning setting into two cases

**different task but related to the source task**, unsupervised transfer learning focuses on **solving unsupervised learning tasks in the target domain**. Thus, no labeled data available in both source and target domains in training)


## What to transfer?(based on what to transfer 所有的迁移学习根据内容可以归纳为如下四类)
1. instance-transfer approach  
2. feature-representation-transfer approach
3. parameter-transfer approach
4. relational-knowledge-transfer approach

> The former three contexts have an i.i.d assumption on the data while the last context deals with transfer learning on relational data. Most of these approaches assume that the selected source domain is related to the target domain.


## some important research issues(或者当前研究的困境)
1. how to avoid negative transfer

负迁移：迁移之后的效果比之前还差。
现在我们的许多机器学习(包括深度学习)中都假设源域和目标域是相关的，但是如果假设不成立，负迁移就可能会发生。
避免：首先要对源域和目标域的可转移性进行度量，根据度量，我们可以选择相关的源域来提取相关知识，从而学习目标任务。
如何度量：基于距离度量，我们可以对源域或任务进行聚类，这有助于度量可转移性。
另一个任务：当一个完整的域不能用于转移学习时，我们是否还可以转移域的一部分用于目标域的有用学习

2. most existing transfer learning algorithms so far focused on improving generalization across different distributions between source and target domains or tasks.


3. 目前机器学习算法和深度学习算法的一个前提条件：the training and test data are drawn from the same feature space and the same distribution.
