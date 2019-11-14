---
title: python中fire库的基本使用
tags:
  - Python
  - fire
share: true
categories:
  - Python
  - fire
reward: true
comment: true
top: 2
date: 2019-11-14 18:39:11
description:
---

[参考教程](https://www.cnblogs.com/my_captain/p/9574560.html)

## 一个py文件只有一个函数
只有一个函数时,采用如下命令进行调用：

> python test_fire.py 5 2


![Pycharm中直接运行的结果](pycharm_fire.jpg)

![命令行参数运行的结果](terminal_fire.jpg)


```python
import fire
print(fire.__version__)




def add(a, b):
    return a + b

if __name__ == '__main__':
    fire.Fire(add)


```

## 一个py文件有多个函数
有多个函数时,采用如下命令进行调用：
> python test_fire.py 函数名 参数

![Pycharm中直接运行的结果](pycharm_fire2.jpg)

![命令行参数运行的结果](terminal_fire2.jpg)


```python
import fire
print(fire.__version__)




class Calculator(object):
    def add(self, a, b):
        return a + b

    def sub(self, a, b):
        return a - b


if __name__ == '__main__':
    fire.Fire(Calculator)


```

<!--more-->