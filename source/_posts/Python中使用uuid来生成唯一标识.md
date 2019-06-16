---
title: Python中使用uuid来生成唯一标识
tags:
  - Python
  - uuid
share: true
categories:
  - Python
reward: true
comment: true
top: 2
date: 2019-05-08 15:06:48
description:
---



## uuid

```python
import uuid
print(uuid.uuid4()) #将会生成一个带有-分隔的随机
# 09debe4e-895b-4283-afb7-d9bb9e3ecc02
# 使用.hex将其-分隔符去掉
print(uuid.uuid4().hex)
2a6acd34fe4940eeb0f4a76fbad319c1

```



[参考](<https://blog.csdn.net/crazyhacking/article/details/38898721>)
[参考](https://www.cnblogs.com/tangpg/p/9475900.html)

<!--more-->