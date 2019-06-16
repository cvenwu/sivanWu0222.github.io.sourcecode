---
title: Ubuntu下配置Git的SSH
tags:
  - Ubuntu
  - SSH
  - git
  - GitHub
share: true
categories:
  - git
  - ssh
reward: true
comment: true
top: 2
date: 2017-12-01 20:26:12
---

[官方文档](<https://help.github.com/en/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent>)



> 
>
> 经常使用GitHub的童鞋一定知道，当我们每次push的时候，都会遇到一个很尴尬的问题，输入用户名和密码，一次也就够了，但是当我们每天push次数很多的时候，就已经很不耐烦了，所以为了避免这个问题，我们可以创建一个SSH秘钥，然后与GitHub进行配置，从此以后我们不需要输入任何密码！

本次创建和配置我们是基于Linux进行的

## 创建

生成一对秘钥，运行如下命令（注意输入自己的邮箱）

> ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

输入命令之后，系统会让我们输入秘钥存放的位置，这里直接回车，也就是使用默认位置
回车之后，输入密码，这里也是进行回车，因为密码是可选项，

------

## 注册

当我们在本机创建了SSH密钥的时候，并不能够代表我们就可以使用了，我们需要在GitHub上进行注册，使用如下命令

启动SSH代理应用并重定向使用Bourne

> [root@localhost /]# eval "$(ssh-agent -s)"

使用代理注册SSH密钥

> [root@localhost /]# ssh-add ~/.ssh/id_rsa

<!-- more -->

------

## 获得SSH密钥

1. 因为我们采用SSH密钥，因此我们需要一个公钥，一个私钥，所以当代码托管系统向我们询问 "SSH公钥"的时候，我们需要id_rsa.pub文件中的内容，通常存储在home的一个隐藏文件夹中     

> [root@localhost /]# cat ~/.ssh/id_rsa.pub

2. 接下来，我们需要复制屏幕上输出的所有文本
3. 粘贴到我们使用代码托管系统的设置页面中



<!-- more -->