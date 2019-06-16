---
title: '[Java]-怪题-3'
date: 2016-12-14 10:57:28
tags: [Java]
categories:
- Java
- 基础
---


> 这里介绍一个之前曾经遇到过的一个问题

例题：采用递归的方法求100以内的数字和

方法一：

```Java
public class Test {
	public static void main(String[] args)
	{
		int s = 100;
		System.out.println(getSum(100));
	}

	/*这里自己另外写了一个方法*/
	public static int getSum(int n)
	{

		if(n < 1)
			return n;
		else
			return n + getSum(n-1);
	}
}
```
<!--more-->

方法二：

```Java
public class Test {
	public static void main(String[] args)
	{
		int s = 100;
		System.out.println(sum(100));
	}

	/*这种方法代码量少，而且富有技巧，但是不是自己写的，还是需要加强	*/
	public static int sum(int number)
	{
		int s = 0;
		if(number >= 1)
			s = number-- + sum(number);
		return s;
	}
}
```
