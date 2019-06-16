---
title: GitHub+Hexo+Appveyor 保存博客源代码并自动部署
tags:
  - CI
  - Hexo
  - 自动部署
  - Appveyor
  - 博客源码
share: true
categories:
  - 自动部署
  - Hexo博客
reward: true
comment: true
top: 2
repo: 'sivanWu0222 | MasterPythonWebCrawler#用户名 | 仓库名'
src: 'i.loli.net/2017/12/12/5a2fd18a74471.jpg'
date: 2019-06-16 17:51:34
description:
---

[参考](https://www.jianshu.com/p/db9a95078b16)

## 前言


### 痛点
其实自己之前就有换过电脑的经历，每次换电脑之后都需要重新安装开发环境，博客之前的文章用u盘拷贝过来，很是担心u盘丢失之后自己的学习记录无法保存，于是网上看到了使用Appveyor对博客进行自动部署，在自动部署的过程中，源代码也会自动托管到github的一个存放源代码的仓库中，我们在源代码仓库发生的变动appveyor将会自动对变动进行重新渲染到我们博客(~~不是源代码对应的仓库哦~~)存放的目录，不仅仅保存了博客的源代码，还可以自动部署！

### 步骤
步骤如下：
1. appveyor 添加项目
2. 在自己的项目本地文件中**添加appveyor.yml配置文件**，同时配置**密钥**
3. 在appveyor中配置项目的环境变量
4. 将变动push到对应的github的仓库中，appveyor探查到仓库变动之后，将会自动渲染并部署变动到STATIC_SITE_REPO 指定的仓库中


## 设置AppVeyor
> 添加好appveyor.yml之后，再到Appveyor portal设置以下四个变量。STATIC_SITE_REPO就是Content Repo的地址，TARGET_BRANCH就是你Content Repo的branch，一般默认就是master，GIT_USER_EMAIL和GIT_USER_NAME就是你GitHub账号的信息。


STATIC_SITE_REPO ： 就是hexo渲染后存放的仓库。一般都是username.github.io
TARGET_BRANCH: 就是存放源代码的仓库的分支。默认是master
GIT_USER_EMAIL: GitHub的邮箱地址
GIT_USER_NAME: GitHub的用户名



## appveyor.yml配置文件

```
clone_depth: 5

environment:
  access_token:
    secure: 7AzX+wI/I7gTU6LmboXo1zyCb7Qw+1IGwUUrFW31ayFsTlvN/aVh+k8FWWI1PlNB


install:
  - node --version
  - npm --version
  - npm install
  - npm install hexo-cli -g

build_script:
  - hexo generate

artifacts:
  - path: public

on_success:
  - git config --global credential.helper store
  - ps: Add-Content "$env:USERPROFILE\.git-credentials" "https://$($env:access_token):x-oauth-basic@github.com`n"
  - git config --global user.email "%GIT_USER_EMAIL%"
  - git config --global user.name "%GIT_USER_NAME%"
  - git clone --depth 5 -q --branch=%TARGET_BRANCH% %STATIC_SITE_REPO% %TEMP%\static-site
  - cd %TEMP%\static-site
  - del * /f /q
  - for /d %%p IN (*) do rmdir "%%p" /s /q
  - SETLOCAL EnableDelayedExpansion & robocopy "%APPVEYOR_BUILD_FOLDER%\public" "%TEMP%\static-site" /e & IF !ERRORLEVEL! EQU 1 (exit 0) ELSE (IF !ERRORLEVEL! EQU 3 (exit 0) ELSE (exit 1))
  - git add -A
  - if "%APPVEYOR_REPO_BRANCH%"=="master" if not defined APPVEYOR_PULL_REQUEST_NUMBER (git diff --quiet --exit-code --cached || git commit -m "Update Static Site" && git push origin %TARGET_BRANCH% && appveyor AddMessage "Static Site Updated")
```

**注意把自己的secure对应的密钥换成自己的密钥**


<!-- more -->