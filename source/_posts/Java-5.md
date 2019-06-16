---
title: '[Java]-5'
date: 2016-12-23  10:54:55
tags: [OOP,static,final]
categories: 
- Java
- 基础
---





**注意: 在JAVA方法中，不允许再定义方法，只可以调用**

## 封装

<table><thead><tr><th>访问修饰符</th><th style="text-align:right">所有类</th><th style="text-align:center">当前包以及不同包的子类</th><th>同包的类</th><th>当前类</th></thead><tr align="center"><td>public</td><td style="text-align:right">Y</td><td style="text-align:center">Y</td><td>Y</td><td>Y</td></tr><tr align="center"><td>protected</td><td style="text-align:right">N</td><td style="text-align:center">Y</td><td>Y</td><td>Y</td></tr><tr align="center"><td>默认(不写)</td><td style="text-align:right">N</td><td style="text-align:center">N</td><td>Y</td><td>Y</td></tr><tr align="center"><td>private</td><td style="text-align:right">N</td><td style="text-align:center">N</td><td>N</td><td>N</td></tr></table>


> 其实访问修饰符就是帮助JAVA实现封装，封装就是我们生活中的包装，例如食物不包装会腐烂，汽车不包装会生锈，但是在JAVA中，我们封装可以提高安全性，可以避免恶意程序的调用，当然完全封装也不利于我们对功能的实现，所以我们在封装的时候，会留下接口，其实接口就是我们在封装所设置的set以及get方法

```
注意：JAVA中方法的重写时，子类方法的访问修饰符   >=   父类的访问修饰符 
```

<!--more-->
### 例如：

1. 首先创建一个Animal类
2. 创建一个Tiger类
3. 创建一个Test类

```Java
//Animal类
public class Animal {
	String name;
	int age;
	protected void sayHello()
	{
	
	}
}
```

```Java
//Tiger类
public class Tiger extends Animal{
	 void sayHello()
	{
		
	}
}
```

```Java
//Test类
public class Test {
	public static void main(String[] args)
	{
		Tiger tiger = new Tiger();
		tiger.sayHello();
	}
}
```

> 结果：编译报错，因为子类访问权限低于父类访问权限


----------


## 多态

> 方法的重载和覆盖都属于多态


----------

## 接口

> 接口在我们做一些小型项目的时候，不会常用到，但是在做一些大型项目的时候，我们经常会用到，尤其是接口的应用，一定程度上决定了项目的好坏，以及延展性，这里我们将对接口进行一些介绍

- 接口只是一个规范，只要求要做什么，没有要求我们如何去做（得到的一个结论就是扩展性强，从而一定程度上弥补了JAVA中单继承的缺点）
- JAVA中的接口与接口之间是多继承的关系，也就是一个接口可以有多个父接口，但是类与类之间是单继承
- 接口中的方法不能有方法体
- **通过文件定义接口的时候，注意接口名字与文件名前缀相同**
- 接口的作用：被类实现，该类叫做接口的实现类，实现类也会获取接口中的方法和属性
- **非抽象类实现接口，必须对接口中的抽象方法进行实现**
- 关于接口中的变量以及方法，看下面代码


```Java
//源代码
public interface Test {
	int foot = 4;
	void say();
}
//进行反编译后的代码
public interface Test {
	public static final int foot;
	public abstract void say();
}
```


> 结论：JAVA中的接口，变量前面自动加上了public static final 修饰符，方法加上了public abstract 修饰符，并且我们不写修饰符的时候也会自动加上！


----------


## 常用关键字

### static关键字

- 修饰属性 和 方法：可以通过类直接调用
- 修饰类: 只能是内部类
- 静态块：在类加载的时候执行（也就是在主方法执行前执行）
- 静态成员(包括属性和方法)不能访问非静态成员(包括属性和方法)，因为静态成员优先于非静态成- 员分配内存
- 静态成员在内存中公共（所有对象都可以对其进行操作，并且结果保留），并且唯一（在内存中只有一份，被大家一起所有，而不是像其他属性，对象才有）

### final关键字

- 修饰类：该类不能有子类
- 修饰属性：表示属性不能被重新赋值，也就是所说的常量
- 修饰方法：该方法不能被重写（覆盖）

### abstract关键字

- 修饰类：该类是抽象类，不能被实例化
- 修饰方法：该方法是抽象方法，不能有方法体，例如 public abstract void setAge(int age);

> - 有抽象方法的类一定是抽象类，抽象类不一定有抽象方法
> - 抽象类作用就在于可以被继承
> - abstract 和 final关键字不可以共存


#### instanceof 关键字使用:(返回 true 或者 false)

> 基本语法：对象 instanceof 类名

```Java
 public class Test
{
	public static void main(String[] args)
	{
	Test t1 = new Test();
	System.out.println(t1 instanceof Test);
	}
	//打印结果为true
}
```


----------


## 约定名称书写规范

> - 全部大写：常量名字
> - 全部小写：工程名字 包名字
> - Pascal命名法： 每个单词首字母大写 类名
> - 驼峰命名法: 第一个单词首字母小写，其他单词首字母全部大写 方法名字，以及类的属性


----------

## 类成员执行优先级顺序：

- **static 优先于 非static**
- **同级别 属性>块>主方法>普通方法 构造方法的话得看他调用的先后**

```Java
public class Test
{
	static int a;
	public static void main(String[] args)
	{
		Test t1 = new Test();
		t1.a=500;
		System.out.println("a:"+t1.a);			//500
	}
	
	class b
	{
	}
}
	class c
	{
	}
```

