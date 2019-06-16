---
title: js中取消事件冒泡的两种方式
tags: [JavaScript]
categories:
  - JavaScript
date: 2017-11-30 13:56:02
---


## 事件冒泡的由来
  当我们在给写好的控件绑定一个事件之前，它是没有绑定事件的，因此便需要将自己的相应的事件与父容器的相应的事件对应，从而就产生了事件冒泡，甚至于当我们在子容器谢了一个点击事件之后，点击子容器，子容器的点击事件执行，父容器的点击事件也会执行，但是子容器的点击事件最先执行，但是往往我们只需要子容器的点击事件，因此我们需要阻止事件冒泡！


页面上有好多事件，也可以多个元素响应一个事件.假如:

```html
<BODY onclick="alert('aaa');">
<div onclick="alert('bbb');">
 <a href="#" class="cooltip" title="这是我的超链接提示1。" onclick="alert('ddd');">
   提示
  </a>
</div>
</BODY>



```

上面这段代码一共有三个事件,body，div，a都分别绑定了单击事件。在页面中当单击a标签会连续弹出3个提示框。这就是事件冒泡引起的现象。事件冒 泡的过程是：a --> div --> body 。a冒泡到div冒泡到body。


本来在上面的代码中只想触发a元素的onclick事件，然而div,body事件也同时 触发了。因此我们必须要对事件的作用范围进行限制。当单击a元素的onclick事件时只触发a本身的事件。由于IE- DOM和标准DOM实现事件对象的方法各不相同，导致在不同浏览器中获取事件的对象变得比较困难。如果想阻止事件的传递，我们可以用 event.stopPropagation()阻止事件的传递行为.

<!-- more -->

## 阻止事件冒泡

### 方式一：
> 在子容器的方法最后添加一句(注意，这个event是事件参数，也就是我们传递过来的) event.stopPropagation()

```html
<div id="mydiv" style="border: 1px solid red ; width: 200px ; height: 200px;position: absolute;">
	<input id="mybtn" type="button" value="click me">
</div>


<script>

	onload = function(){
		$("#mydiv").bind("click" , function(){alert(123);});
		$("#mybtn").bind("click" , function(event){
      alert(456);
      /*用来阻止事件冒泡*/
      event.stopPropagation();
    });
	}


</script>
```


### 方式二：
> 在子容器的代码中 可以将event.stopPropagation() 替换为 return false;

```html
<div id="mydiv" style="border: 1px solid red ; width: 200px ; height: 200px;position: absolute;">
	<input id="mybtn" type="button" value="click me">
</div>


<script>

	onload = function(){
		$("#mydiv").bind("click" , function(){
        alert(123);
      });
		$("#mybtn").bind("click" , function(event){
			   alert(456);
			   return false;
			});
	}


</script>



```

参照：<a href="https://baike.baidu.com/item/%E4%BA%8B%E4%BB%B6%E5%86%92%E6%B3%A1/4211429?fr=aladdin#4">百度百科</a>
