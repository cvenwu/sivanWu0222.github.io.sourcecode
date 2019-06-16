---
title: Python异常处理机制
date: 2019-01-25 13:52:37
tags:
  - Python
  - Basic
share: true
categories:
  - Python
  - Basic
reward: true
comment: true
top: 2
repo: sivanWu0222 | PythonNote
description:

---

> 引言：

## 异常


### 类型
1. AssertionError:	断言语句(assert)失败，当assert这个关键字后边的条件为假的时候，程序将会停止并抛出AssertionError异常
2. AttributeError:	尝试访问未知的对象属性
3. IndexError:	索引超出序列的范围
4. KeyError:	字典中查找一个不存在的关键字
5. NameError:	尝试访问一个不存在的变量
6. OSError:		操作系统产生的异常
7. SyntaxError:	Python的语法错误
8. TypeError:	不同类型间的无效操作
9. ZeroDivisionError:	除数为0

<!--more-->