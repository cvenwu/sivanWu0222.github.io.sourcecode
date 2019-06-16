---
title: Ubuntu 18.04 使用科学代理
tags:
  - Ubuntu
  - 软件安装
share: true
categories:
  - Ubuntu
reward: true
comment: true
top: 2
date: 2019-04-09 19:35:48
description:

---





## 前言

由于自己在使用Ubuntu安装FAISS的时候，总是遇到如下问题



```shell
yirufeng@ubuntu:/etc/apt/sources.list.d$ conda install faiss-cpu
conda: error: failed to fetch repodata from 'http://repo.continuum.io/pkgs/pro/linux-64/'
```
通过Google和百度都没有找到解决方案，最后想尝试一下科学代理，但是网上关于Ubuntu18 搭建科学代理的回答很少，大都是基于Ubuntu 14或者Ubuntu 16


## 步骤
> 说明：shadowsocks-qt5是ubuntu上一个可视化的版本

### 安装shadowsocks-qt5
- sudo add-apt-repository ppa:hzwhuang/ss-qt5
- sudo apt-get update
出现如下问题：
```shell
yirufeng@ubuntu:~$ sudo apt-get update
命中:1 http://security.ubuntu.com/ubuntu bionic-security InRelease                      
命中:2 http://us.archive.ubuntu.com/ubuntu bionic InRelease                              
忽略:3 http://ppa.launchpad.net/hzwhuang/ss-qt5/ubuntu bionic InRelease
命中:4 http://us.archive.ubuntu.com/ubuntu bionic-updates InRelease    
命中:5 http://us.archive.ubuntu.com/ubuntu bionic-backports InRelease                  
错误:6 http://ppa.launchpad.net/hzwhuang/ss-qt5/ubuntu bionic Release
404  Not Found [IP: 91.189.95.83 80]
正在读取软件包列表... 完成
E: 仓库 “http://ppa.launchpad.net/hzwhuang/ss-qt5/ubuntu bionic Release” 没有 Release 文件。 
N: 无法安全地用该源进行更新，所以默认禁用该源。
N: 参见 apt-secure(8) 手册以了解仓库创建和用户配置方面的细节。
```
**解决方案**：
$ cd /etc/apt/sources.list.d/hzwhuang-ubuntu-ss-qt5-bionic.list 
修改 hzwhuang-ubuntu-ss-qt5-bionic.list 文件内容为如下：(bionic 是18.04版本代号, xenial 是16.04的版本代号)
```shell
# deb http://ppa.launchpad.net/hzwhuang/ss-qt5/ubuntu bionic main
# deb-src http://ppa.launchpad.net/hzwhuang/ss-qt5/ubuntu bionic main
deb http://ppa.launchpad.net/hzwhuang/ss-qt5/ubuntu xenial main
# deb-src http://ppa.launchpad.net/hzwhuang/ss-qt5/ubuntu bionic main
```
- 执行下面两个命令
```shell
$ sudo apt-get update
$ sudo apt-get install shadowsocks-qt5
```

<!--more-->