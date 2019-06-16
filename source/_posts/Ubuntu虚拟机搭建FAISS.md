---
title: Ubuntu虚拟机搭建FAISS
tags:
  - 毕业设计
  - 毕业设计环境
  - FAISS
  - FAISS安装
  - Anaconda
  - Ubuntu
share: true
categories:
  - 本科毕业设计
  - 环境
reward: true
comment: true
top: 2
repo: sivanWu0222 | GraduationProject
date: 2019-04-25 12:55:56
description:
---

## 前言

> 因为使用Anaconda安装FAISS是最简单的一种方法，之前使用Conda(Anaconda的包管理工具)安装，老是曝出如下问题： 


```shell
conda: error: failed to fetch repodata from 'http://repo.continuum.io/pkgs/pro/linux-64/'
```

### 解决办法：
重新去官网下载一个最新的Anaconda安装包
[百度网盘下载](https://pan.baidu.com/s/11pP63oKIAKjZBSI4ycGA5g)
提取码：lebr


## Ubuntu安装Anaconda

[参考](https://www.jianshu.com/p/2645510a39de)



## 安装其他包
[参考](https://www.jianshu.com/p/2645510a39de)

```shell
yirufeng@ubuntu:~/Anaconda$ conda install openblas
WARNING: The conda.compat module is deprecated and will be removed in a future release.
Collecting package metadata: done
Solving environment: done

## Package Plan ##

  environment location: /home/yirufeng/anaconda3

  added / updated specs:
    - openblas

The following packages will be downloaded:

    package                    |            build
    ---------------------------|-----------------
    conda-4.6.14               |           py37_0        2.1 MB
    libgfortran-3.0.0          |                1        281 KB
    openblas-0.2.19            |                0        3.0 MB
    ------------------------------------------------------------
                       Total:         5.4 MB

The following NEW packages will be INSTALLED:

  libgfortran        pkgs/free/linux-64::libgfortran-3.0.0-1
  openblas           pkgs/free/linux-64::openblas-0.2.19-0

The following packages will be UPDATED:

  c
  
  
  
(base) yirufeng@ubuntu:~/Anaconda$ conda install faiss-cpu -c pytorch
Collecting package metadata: done
Solving environment: done

## Package Plan ##

  environment location: /home/yirufeng/anaconda3

  added / updated specs:
    - faiss-cpu


The following packages will be downloaded:

    package                    |            build
    ---------------------------|-----------------
    faiss-cpu-1.5.1            |   py37h6bb024c_1        879 KB  pytorch
    ------------------------------------------------------------
                                    Total:         879 KB

The following NEW packages will be INSTALLED:

  faiss-cpu          pytorch/linux-64::faiss-cpu-1.5.1-py37h6bb024c_1


Proceed ([y]/n)? y


Downloading and Extracting Packages
faiss-cpu-1.5.1      | 879 KB    | ############################################## | 100% 
Preparing transaction: done
Verifying transaction: done
Executing transaction: done
(base) yirufeng@ubuntu:~/Anaconda$ 
```


## 参考

[参考](https://www.jianshu.com/p/2645510a39de)

<!--more-->