---
title: Hexo常用命令
date: 2016-12-25 10:55:55
tags: Hexo 
categories: 常用命令
---


这里收录了一些常用的Hexo命令，经常用到

## Hexo

npm install hexo -g #安装
npm update hexo -g #升级
hexo init #初始化

<!--more-->

注意：需要node.js环境


----------

## 新建博客目录

hexo init blog #生成一个blog目录

注解：在 hexo 目录下生成blog文件夹，包含 node_modules、scaffolds、source、themes、.gitignore、_config.yml、db.json、package.json，其中 _config.yml 为全局配置文件


----------

## 简写

hexo n “我的博客” == hexo new “我的博客” #新建文章
hexo p == hexo publish
hexo g == hexo generate#生成
hexo s == hexo server #启动服务预览
hexo d == hexo deploy#部署


----------

## 新建文件夹（或者说是页面）（自动在文件夹下新建index.md）

hexo new page file
hexo new page “file”
INFO Created: D:…\blog\source\file\index.md


----------

## 新建文章

hexo n fileName
hexo new “fileName”
hexo new post fileName
hexo new post “fileName”
INFO Created: D:…\blog\source_posts\fileName.md


----------


## 服务器

hexo server #Hexo 会监视文件变动并自动更新，您无须重启服务器。
hexo server -s #静态模式
hexo server -p 5000 #更改端口
hexo server -i 192.168.1.1 #自定义 IP

备注：4000为默认端口，若站点 _config.yml 中 修改配置 root: /blog/ ，即访问地址对应为 htto://localhost:4000/blog/

hexo clean #清除缓存 网页正常情况下可以忽略此条命令
hexo g #生成静态网页
hexo d #开始部署


----------

## 监视文件变动
hexo generate #使用 Hexo 生成静态文件快速而且简单
hexo generate –watch #监视文件变动
