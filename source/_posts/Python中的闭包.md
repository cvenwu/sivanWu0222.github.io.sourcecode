---
title: Python中的闭包
date: 2019-01-25 10:58:06
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
---

## 闭包(closure)

> 引言：闭包是函数式编程的一个重要的语法结构，函数式编程是一种编程范式，不同编程语言实现闭包方式不同

> Python中的闭包：从表现形式上定义为，如果一个内部函数里，对在外部作用域(但不是全局作用域)的变量进行引用，那么内部函数就被认为是闭包

```Python
def funX(x):
    def funY(y):
        return x * y
    return funY
	
# 闭包函数的使用
i = funX(8)
print(i(5))


# 也可以直接写 funX(8)(5)
```

> 例如上面的代码中，在内部函数funY()中引用了外部作用域的变量y，此时funY()就被认为是闭包

> 注意：
1. 不可以在外部函数以外的地方对内部函数进行调用，下面做法是错误的
2. 在闭包中，外部函数的局部变量对应内部函数的局部变量，事实上相当于之前讲的全局变量跟局部变量的对应关系(如果要修改全局变量需要使用global关键字),在内部函数中，只能对外部函数的局部变量进行访问，如果需要进行修改，Python3中可以使用nonlocal关键字



> * 适用于在某种情况下，我们并不方便使用全局变量，所以灵活的使用闭包可以实现替代全局变量。

<!--more-->

> * 例如以下的游戏开发中，我们需要将游戏中角色的移动位置保护起来，不希望被其他函数轻易可以修改到，所以我们选择使用闭包操作，参考代码及注释如下：
```Python
origin = (0, 0)         # 原点
legal_x = [-100, 100]   # x轴的移动范围
legal_y = [-100, 100]   # y轴的移动范围

def create(pos_x=0, pos_y=0):
# 初始化位于原点为主    
    def moving(direction, step):
    # direction参数设置方向，1为向右（向上），-1为向左（向下），0为不移动
    # step参数设置移动的距离
        nonlocal pos_x, pos_y
        new_x = pos_x + direction[0] * step
        new_y = pos_y + direction[1] * step
        # 检查移动后是否超出x轴边界
        if new_x < legal_x[0]:
            pos_x = legal_x[0] - (new_x - legal_x[0])
        elif new_x > legal_x[1]:
            pos_x = legal_x[1] - (new_x - legal_x[1])
        else:            
            pos_x = new_x
        # 检查移动后是否超出y轴边界
        if new_y < legal_y[0]:
            pos_y = legal_y[0] - (new_y - legal_y[0])
        elif new_y > legal_y[1]:
            pos_y = legal_y[1] - (new_y - legal_y[1])
        else:            
            pos_y = new_y
        return pos_x, pos_y
    return moving
    
move = create()
print('向右移动10步后，位置是：', move([1, 0], 10))
print('向上移动130步后，位置是：', move([0, 1], 130))
print('向左移动10步后，位置是：', move([-1, 0], 10))

```
[参考](https://fishc.com.cn/thread-42656-1-1.html)