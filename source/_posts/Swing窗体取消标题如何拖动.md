---
title: Swing窗体取消标题如何拖动
date: 2017-05-05 08:24:13
tags: [Java,Swing]
categories:
- Java
- Swing
---
> 众所周知，swing中的窗体默认提供了边框以及标题栏，但是我们设置undecorated为true之后，不会使用Swing中的窗体来修饰，但是默认窗体也将不会随着鼠标的拖动而移动，因此我们需要给我们自己窗体添加一个随着鼠标移动而移动的事件

## 添加鼠标监听器事件
> 窗体移动默认都是鼠标按下之后，鼠标开始移动，窗体也会跟着鼠标开始移动！鼠标移动之前，我们需要记录按下鼠标时相对于窗体的位置，因为移动将不会改变鼠标相对于窗体的位置


```Java
  frame.addMouseListener(new MouseAdapter() {
            @Override
            public void mousePressed(MouseEvent e) {
              pressedPoint = e.getPoint();/*记录鼠标相对于窗体坐标*/
            }
        });
```
<!-- more -->

----------

## 添加鼠标侦听器事件
> 窗体移动是一个实时的事件，并且将会持续一个过程，java为我们提供了一个鼠标侦听器事件的监听器，我们只需要实现侦听器就可以实现窗体跟随鼠标拖动而移动


### 让窗体移动的思路
-   记录下来按下鼠标的时候，鼠标相对于窗体的位置
-   记录窗体位置，鼠标移动了之后，鼠标相对于窗体位置并不会改变，但是在移动过程中，窗体位置将不会改变，鼠标相对于窗体位置会不断改变，因此我们只需要通过 窗体位置（不会动） + 移动过程中鼠标相对于窗体位置的坐标（移动过程中会动） - 鼠标相对于窗体的位置（按下鼠标的时候，不会动） = 窗体移动后的位置

```Java
/*设置窗体移动*/
		frame.addMouseMotionListener(new MouseMotionAdapter() {
	            @Override
	            public void mouseDragged(MouseEvent e) {
	            	Point point = e.getPoint();           /*获取当前坐标,得到的是鼠标位置相对于窗体的坐标*/
	                Point locationPoint = frame.getLocation();/*获取窗体坐标*/
	                int x = locationPoint.x + point.x - pressedPoint.x;/* 计算移动后的新坐标*/
	                int y = locationPoint.y + point.y - pressedPoint.y;
	                frame.setLocation(x, y);/*改变窗体位置*/
	            }
	        });
```
