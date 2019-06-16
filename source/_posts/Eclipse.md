---
title: Eclipse工具讲解（中文语言包的安装，活用workspace，添加新的JDK环境）
date: 2017-03-17  10:55:40
tags: [Eclipse,Tools]
categories: Tools
photos:
- https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1490613337&di=df2b2469e26db5ccf65d7d8cba8f789d&imgtype=jpg&er=1&src=http://cms.csdnimg.cn/article/201303/08/5139976ab7cec.jpg 
---


## 中文语言包的安装


### 准备工作

> 1. 安装好的Eclipse，可以去网上下载，免费，下载[Eclipse](https://www.eclipse.org/downloads/)
> 2. 有网络环境

<!--more-->
### 下载中文语言包

1. 进入[语言包下载](http://www.eclipse.org/babel/downloads.php)
2. 找到自己所对应的Eclipse版本，并且点击，进入语言包界面
3. 找到Language: Chinese (Simplified) 栏目
4. 下载该栏目下所有文件

### 解压缩中文语言包

1. 将所下载的压缩包解压到一个文件中
2. 解压后的文件有两个子目录，features 和 plugins ，复制到Eclipse的安装目录，提示是否覆盖，选择是，打开Eclipse即可看到中文界面的Eclipse


----------


## 添加新的JDK环境

> Eclipse的成功运行需要Java运行环境的支持，才能够启动运行，并且Eclipse启动后，会把当前Java执行环境当作Eclipse的默认开发环境


1. 选择 Window–>Preferences–>Java选项卡–>Installed JREs –>Add
2. 选择 Standard VM
3. 弹出的新界面，点击Directory，找到JDK安装路径，点击finish

注意：这里也可以点击 Variables 设置JRE环境的内存参数，在缺省VM参数的编辑框，写入 “-xmx200M”参数就可以为虚拟机添加内存，可以避免大型应用程序因为内存不足而无法运行


----------

## 为项目添加类库

> 1. 一个大型项目的开发，需要几个或多个JAR文件的支持，比如数据库连接的类库等等
> 2. 构建路径就是把各个类库设置到CLASSPATH类路径当中，是一个环境变量，这样，在编译源文件时才能够找到引用的其他JAR文件中的API

### 方法一：

自己新建一个文件夹 lib ，然后把所有用到的JAR文件复制粘贴过来

### 方法二：

>对于重复使用的类库，不需要每次添加，直接添加到用户的类库中就可以

1. 选择Window –> Preferences–> Java –> Build Path –> user libraries
2. 选择import ，点击自己所存放类库的位置就可以


----------


## 安装界面设计器（WindowBuilder插件）


> 由于Java Swing开发桌面应用程序比较灵活，由于API众多，因此开发难度增大，而且Swing为了实现跨平台的应用程序，使得程序员不得不边运行程序，边进行界面设计，因此这里介绍一个插件，帮助大家快速开发桌面应用程序

### 离线安装
1. 进入 [WindowBuilder下载界面](http://www.eclipse.org/windowbuilder/download.php)
2. 找到对应版本的Eclipse，选择 Release Version下的 Zipped Update Site （离线安装）
3. 将压缩包中的文件解压，并覆盖Eclipse文件夹下的同名文件夹

### 在线安装


1. 进入 [WindowBuilder下载界面](http://www.eclipse.org/windowbuilder/download.php)
2. 找到对应版本的Eclipse，选择 Release Version下的Update Site 的link选项，点击，复制进入新界面的网址
3. 打开Eclipse
4. 点击 Help–>Install new software–> Add –>输入名称（WindowBuilder），输入位置 （把复制的网址粘贴）
5. 点击确定，然后选择出来的WindowBuilder，然后一路next