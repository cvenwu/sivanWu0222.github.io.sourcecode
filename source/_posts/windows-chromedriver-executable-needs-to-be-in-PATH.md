---
title: windows chromedriver' executable needs to be in PATH
tags:
  - Python
  - selenium
share: true
categories:
  - Python
  - selenium
reward: true
comment: true
top: 2
repo: sivanWu0222 | PythonNote
date: 2019-02-04 08:32:58
description:
---

# 前言
使用Python的selenium模拟控制chrome浏览器，报出如下错误
> windows chromedriver' executable needs to be in PATH

# 解决方案

## 步骤
1. 更新Chrome浏览器到最新(72.0.3626.81) 64位
2. 下载ChromeDriver, 需下载与Chrome对应的版本
[Download](https://npm.taobao.org/mirrors/chromedriver/)
3. 将下载后的ChromeDriver解压到Chrome的安装目录中的Application
4. 将Chrome安装目录的Application添加到系统的PATH环境变量中

<!--more-->