---
title: '[Java]-3'
date: 2016-12-14 10:54:47
tags:
categories: 
- Java
- 基础
---


## 数组

> 数组作为最常使用的引用数据类型，在项目中使用较为广泛，这里进行一些相关介绍

### 数组的创建

1. 首先声明数组
2. 创建数组
3. 数组赋初值（不赋初值，系统会默认赋值，byte short int long 系统赋初值0 ，float double系统赋初值0.0 boolean系统赋初值false）

> 需要注意的是：数组在定义长度和赋初值同时操作会报错，同时数组长度一旦确定将不能发生改变

## 类中的方法

### 形参与实参的区别

1. 形参只是一个形式参数，最主要的是方法中的参数的数据类型，而不是形参本身
2. 实参是一个实际参数，指定了使用方法时所需传递的数据

<!--more-->

### 方法的调用本质条件

1. 方法名字
2. 参数(参数要分顺序)

### 方法的返回值类型与void的使用

1. 方法返回值类型不是void时，则必须加上return 和相对应的返回类型。
2. 方法返回值是void类型的时候，加上return语句也可以，(**例如下面的代码**)

```Java
public class Person {	
	private String name;
	
	public String getName() 
	{
		return name;
	}	
	
	public void setName(String name)
	{
		this.name = name;
		return;
	}			
}
```

> break : 强制退出当次循环
continue : 进入下一次循环
return : 跳出当前方法

### 方法的重载

#### 条件
1. 发生在同一个类中
2. 方法名字相同
3. 参数不同（参数的类型，参数的顺序，参数的个数）


#### 注意

根据方法名与参数只能找到一个与其对应的方法，则编译正确


> 例如：（代码将会报错）

```Java
public class Test{	
	public static String add(String a, int b)
	{
		return a;
		
	}
	
	public static int add(String a, int b ){
		return b;
	}
	
}
```


----------


## 运算符
### 三元运算符
著名的三元运算符 ？ ：

```Java
	
System.out.println(5 > 3 ? 5 : 3);
```

> 注意：**?号前面的运算结果必须为boolean类型**，不可以为其他类型

### 逻辑运算符

> 逻辑运算符中，包含有 && || & | ^ !

#### ####详解 & 与 &&

> &&只可以做逻辑运算，&则既可以做逻辑运算，也可以做位运算
> 
> 1. && 与 & 做逻辑运算规则相同
> 2. & 做位运算，规则是 将两个数字转换为二进制，对比每个二进制位，有0的便为0，全为1便为1 | 与 || 也同理
> 3. &&在做逻辑运算的时候会部分短路，（在假与真或的时候）