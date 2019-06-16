---
title: $(document).ready() 与 window.onload() 方法区别
date: 2017-10-18 19:14:09
tags: [JavaScript,jQuery]
categories:
- jQuery
- 事件
---

> 浏览器装载完文档之后,浏览器会通过JS为document元素添加事件，JS中通常使用window.onload 方法，而在jQuery中，通常会使用$(document).ready()方法

# 区别

## 执行时机的区别
1. window.onload 方法在等待网页中的所有元素全部加载(和元素有关联的文件)之后才会执行。JS此时才会访问网页中的元素。
2. jQuery 中的 $(document).ready()方法注册的事件处理程序，在Dom完全就绪的时候就可以调用，此时网页的所有元素都可以进行访问，但并不是这些元素关联的文件都已经下载完毕。

> 注意： 当DOM数解析完成之后，图片还没有加载完毕，图片高度和宽度这样的属性此时不一定有效

## 多次使用
> 网页中的代码如下，但是多次使用 window.onload 之后，只会弹出一次

```JavaScript
window.onload = function(){
  alert("1");
}

window.onload = function(){
  alert("2");
}

```

> 而jQuery中的 $(document).ready() 则可以写多次，且都会执行

```jQuery
$(document).ready(function(){
  alert("1");
  });
$(document).ready(function(){
  alert("2");
  });
```

总结：
1. onload 事件一次只可以保存一个函数的引用，会自动用后面的函数覆盖前面的函数，因此不可以在现有的行为上继续添加行为
2. $(document).ready()方法能够将会很好的处理这些情况，每次调用都会在现有的行为上追加新的行为，并按照注册的顺序依次执行

<!-- more -->
