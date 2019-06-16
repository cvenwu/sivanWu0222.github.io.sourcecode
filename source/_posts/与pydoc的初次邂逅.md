---
title: 与pydoc的初次邂逅
date: 2017-05-11 14:02:58
tags: [Python,pydoc]
categories:
- Python
- pydoc
---


> 对于初学者而言，学习一门新的编程语言，最渴求的便是遇到问题时，希望能有个帮助文档来指点迷津，pydoc就是这样一个例子，不仅仅可以帮助我们获得所需的信息，还进一步根据用户体验来在浏览器呈现帮助文档

> pydoc是Python自带的模块，主要用于从python模块中自动生成文档，这些文档可以基于文本呈现的、也可以生成WEB 页面的，还可以在服务器上以浏览器的方式呈现！

## 学习查看python中的帮助信息
> 对于Python而言，倘若我们学会查看帮助信息，便是学会了如何运用Python，接下来我们只需将Python运用自如即可

### 使用Python自带的Idle来查看帮助信息
> 使用Idle查看帮助信息，我们将会采用help()函数来查看，

```Python
#查看python所有的modules
help("modules")         

#单看python所有的modules中包含指定字符串的modules
help("modules yourstr")

#查看python中常见的topics
help("topics")          

#查看python标准库中的module
import os.path + help("os.path")

#查看python内置的类型
help("list")         

#查看python类型的成员方法
help("str.find")    

#查看python内置函数
help("open")       

```

<!-- more -->

![Idle查看帮助信息](http://on3w7gc9m.bkt.clouddn.com/%E4%B8%8Epython%E7%9A%84%E5%88%9D%E6%AC%A1%E9%82%82%E9%80%851.png)

----------
### 使用Windows命令行界面来查看帮助信息

打开cmd，键入 <code> python -m pydoc <modulename>  </code> ,请将命令中的<modulename> 替换成你需要查找的信息，例如， 将 <modulename>  替换成 print  ，将会显示出print函数的详细信息


![CMD下查看帮助信息](http://on3w7gc9m.bkt.clouddn.com/%E4%B8%8Epython%E7%9A%84%E5%88%9D%E6%AC%A1%E9%82%82%E9%80%852.png)

----------


### 使用浏览器来查看Python帮助信息
> Python 可以通过浏览器来查看 Python的帮助信息，不需要连网，这是我最喜爱的一种方式，不仅提升了用户体验，还更方便

1. 打开cmd，键入 <code> python -m pydoc -b </code>
2. 命令完成后，回车，等待浏览器自动打开
3. 在输入框中键入所要查询的内容即可查询
4. 如果想要关闭服务器，请输入 q 即可

![浏览器下查看帮助信息](http://on3w7gc9m.bkt.clouddn.com/%E4%B8%8Epython%E7%9A%84%E5%88%9D%E6%AC%A1%E9%82%82%E9%80%853.png)



----------
