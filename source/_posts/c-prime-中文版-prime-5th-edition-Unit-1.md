---
title: C++ primer 中文版 5th edition Unit 1
tags:
  - C++
  - Grammar
share: true
categories:
  - C++
  - Grammar
reward: true
comment: true
top: 1
repo: sivanWu0222 | CppProgramming
date: 2018-06-26 19:58:23
description: 这里是自己在看 C++ primer中文版第5版第1单元所遇到的一些小知识，这里记录下来以后可以查阅 
---

## 术语
缓冲区：内存中的一个存储数据的区域。IO设施通常会将数据放到缓冲区，我们可以显式的刷新缓冲区来将缓冲区中的数据输出IO设备。默认情况下，>cin会刷新cout>程序非正常终止也会刷新cout
cerr：一个ostream对象，关系到标准错误处理，默认情况下，写入到cerr的数据是不缓冲的。
clog：一个ostream对象，关系到标准错误。默认情况下，写到clog的数据是缓冲的。通常用于一个日志文件中。
.运算符：点运算符，左侧为一个类类型对象，右侧为类类型对象的一个成员名字。
::运算符：作用域运算符，用在访问命名空间中的名字
<<运算符：输出运算符，表示将右侧对象写入到标准输出中
">>"运算符：输入运算符，表示将左侧标准输入到右侧对象中

## 读取数量不定的输入数据

```c++
/**
 * 读取数量不定的输入数据
 * @return [description]
 */
#include<iostream>
int main()
{
  int sum = 0, value = 0;
  //读取数据直到遇到文件尾（可以是正常的文件结束符，也可以是输入错误），计算所有读入的值的和
  //从键盘输入文件结束符：win下是ctrl + z然后按Enter或Return键
  while (  std::cin >> value )
  /*
  //当使用istream对象作为条件时，其效果是检测流的状态，如果流是有效的，
  //当使用那么就会检测成功，当遇到一个错误输入(输入不是整数)或者文件结束符，
  //就会变为无效，将会使得条件为假
  */
    sum += value; //等价于sum = sum + value;

  std::cout << "Sum is:" << sum << std::endl;
  return 0;
}
```

<!--more-->

## 注意事项
1. 大多数系统中，main()的返回值被用来指示状态，返回0表示成功，非0返回的值的含义由系统进行定义，通常用来指出错误类型
2. 使用endl 与 \n 的区别就是：endl会刷新缓冲区，使得栈中的东西重新刷新一次，将会效率低下，endl除了\n还有flush的意思






