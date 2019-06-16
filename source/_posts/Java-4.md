---
title: '[Java]-4'
date: 2016-12-17  10:54:51
tags: [OOP]
categories:
- Java
- 基础
---


## 面向过程 与 面向对象

1. 面向过程：通过事件发生的过程来进行分析
2. 面向对象：通过事件的参与者来进行分析

> 面向过程就是分析出解决问题所需要的步骤，然后用函数把这些步骤一步一步实现，使用的时候一个一个依次调用就可以了。
>
> 面向对象是把构成问题事务分解成各个对象，建立对象的目的不是为了完成一个步骤，而是为了描叙某个事物在整个解决问题的步骤中的行为。


----------
## 定义一个类

### 如何描述一个JAVA中的类：
1. 属性
2. 方法

> cmd下的JAVA命令，只可以运行有主方法的类
> Javac 进行编译含有主方法类的时候，会自动将里面所涉及到的类自动编译，不需要手动对其他类进行编译

```Java
public class Test {
	String name;
	int age;
	public static void main(String[] args)
	{
			Animal animal = new Animal();
			/*这里的animal只是一个名字，不是指新建出来的对象*/
			/*Head First Java中将其形象的说成了遥控器*/
		}

}
```
<!--more-->

### 类中特殊的方法-构造方法

> 作为JAVA中的最特殊方法—构造方法，并非浪得虚名，通过构造方法，我们可以给属性进行赋值，同时产生一个对象，**当一个子类继承了一个父类的时候，子类的构造方法的第一句一定是通过super调用父类的构造方法。**

#### 注意事项：

1. 与类同名
2. 无返回类型（与void不同，void是返回为空）
3. 不能通过对象调用，只在new的时候调用
4. **如果用户没有定义，系统提供一个默认的无参构造方法**
5. 构造方法不可以被继承
6. 不可以被重写，只可以被重载


> 这里我们可以先对类进行编译，编译之后通过dos下的javap 类名 即可进行反编译

```Java
public class Test {
	public static void main(String[] args)
	{

		Animal1 animal = new Animal1(10);
		/*animal.age = 10;*/
		System.out.println(animal.age);

	}
}
class Animal1 {

	int age;

	public Animal1(int age)
	{
		age = age;
	}

}
```


结果：输出0
原因分析：所有方法中，局部变量优先于全局变量，在这里就是 把局部变量的 age = 10;

### 父类引用指向子类对象

[移步](http://blog.csdn.net/kaiwii/article/details/8042488%20%E7%A7%BB%E6%AD%A5)


----------

## OOP四大特性

1. 抽象：描述一个类，只是通过我们在业务中所用到的一些方法和属性进行描述
2. 继承：JAVA中的继承包括两种，类与类之间的继承是单继承（子类继承父类的属性和方法（**这句话局部正确，毕竟构造方法肯定不可以**）），类与接口的继承是多继承
3. 封装
4. 多态：通过重载与覆盖表现出来

> 关于继承的时候：一般都是一对多，此时我们需要由多的一方进行维护，这和我们在数据库中所涉及到的是一样的


----------

## this 与 super

this：一般常用于方法中，哪个对象调用就指的是这个对象，特指当前对象

```Java
public class Animal {

	int age;

	public Animal(int age)
	{
		this.age = age;/*在这里就是该对象的age属性被赋值成age(所谓的局部变量)*/
	}

}
```

super：只是一个关键字，并不是父类对象，但是我们可以通过super来调用父类的属性和方法


----------

注意：

```Java
public class Test {
	public static void main(String[] args)
	{

		Animal1 animal = new Animal1(10);
		/*animal.age = 10;*/
		System.out.println(animal.age);
		System.out.println(animal);/*这里输出   类名@哈希码  通过哈希码我们可以判断是不是同一个对象，相同则是同一个对象*/
	}
}
```
