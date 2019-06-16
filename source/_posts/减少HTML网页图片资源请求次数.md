---
title: 减少HTML网页图片资源请求次数
date: 2017-05-22 19:25:04
tags: [HTML, CSS]
categories:
- HTML/CSS
- 优化
---

> 最近学习前端，老师讲过一个问题，虽然<a href="http://www.qq.com">QQ</a>网页很好看，但是加载时间过长，因为将每一张图片都分离开来，一张图片对应用户的一次请求，所以我们一般讲网站用的图标都集中到一张图上，然后通过移动图片位置和确定图片大小来锁定我们要的图片，会让我们网页的次数减少一个很大的层次，算是一个经验！！！

## 优点：
1. 这里放了两张图，其中第一张是网页所用到的图片，都集中到一张图片上了，第二张我们要实现的效果图片，不用专门对图片进行裁剪
2. 这里我们只对图片请求了一次，不需要对图标分别请求，优化了网页

## 缺点：
1. 需要手动定位到我们需要的图标，可能遇到图标对的不齐的问题
<!-- more -->
![所用图标大图](http://on3w7gc9m.bkt.clouddn.com/icon.gif)
![实现的效果图](http://on3w7gc9m.bkt.clouddn.com/%E8%B4%B5%E7%BE%8E%E5%95%86%E5%9F%8E%E5%A4%B4%E9%83%A8%E6%95%88%E6%9E%9C%E5%9B%BE.png)


----------

 源代码（图片路径请注意修改）：
```HTML
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>header</title>
  <style media="screen">
    * {
      margin: 0px;
      padding: 0px;
    }
    #box1 {
      background-image: url(images\\h_bg.jpg);
      background-repeat: no-repeat;
      width: 1920px;
      height: 150px;
    }
    #data-nav {
      padding: 0px;
      /*background-color: red;*/
      width:735px;
      height:30px;
      position: absolute;
      margin-top: 100px;
      margin-left: 25px;
    }
    li{
      float: left;
      list-style: none;
      padding: 0px;
      line-height: 30px;
      text-align: center;
    }
    a {
      text-decoration: none;
    }
    #data-nav li {
      width: 83px;
    }
    #data-nav li:hover{
      background-image: url(images\\nav_bg.gif);
    }
    #data-welcome{
      color:grey;
      letter-spacing: 3px;
      margin-left: 510px;
      position: absolute;
      margin-top: 70px;
      font-size: 14px;
    }
    #data-menu {
      /*background-color: red;*/
      position: absolute;
      margin-top: 10px;
      width: 460px;
      height: 40px;
      margin-left: 540px;
      font-size: 15px;
    }
    .bg{
      background-image: url(images\\icon.gif);
      width: 30px;
      height: 28px;
    }
    .bg1 {
      background-position: -40px 0px;
    }
    .bg2 {
      background-position: -82px 0px;
    }
    .bg3 {
      background-position: -122px 0px;
    }
    #data-menu .icon {
      width: 32px;
    }
  </style>
</head>
<body>
  <div id="box1">
    <div id="data-nav">
      <ul>
        <li><a href="">首页</a></li>
        <li><a href="">家用电器</a></li>
        <li><a href="">手机数码</a></li>
        <li><a href="">日用百货</a></li>
        <li><a href="">书籍</a></li>
        <li><a href="">帮助中心</a></li>
        <li><a href="">免费开店</a></li>
        <li><a href="">全球咨询</a></li>
      </ul>
    </div>
    <div id="data-menu">
      <ul>
        <li class="bg icon"></li>
        <li><a href="">购物车</a></li>
        <li class="bg bg1 icon"></li>
        <li><a href="">帮助中心</a></li>
        <li class="bg bg2 icon"></li>
        <li><a href="">加入收藏</a></li>
        <li class="bg bg3 icon"></li>
        <li><a href="">设为首页</a></li>
      </ul>
    </div>
    <div id="data-welcome">
      你好，欢迎访问贵美商城！117年5月21日16点18分
    </div>
  </div>
</body>
</html>
```
