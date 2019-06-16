---
title: '[Java]-8'
date: 2017-01-02 10:55:12
tags:
categories:
- Java
- Util基本类的使用
---


## Util包

### Random类

#### nextInt()方法

范围只是包含【0，输入的数字）；生成一个输入数字以内的随机整数

#### Date 类，SimpleDateFormat类，以及Calendar类

> 1.Date 类最主要的作用就是获得当前时间，同时这个类里面也具有设置时间以及一些其他的功能，但是由于本身设计的问题，这些方法却遭到众多批评，不建议使用，更推荐使用
> Calendar 类进行时间和日期的处理。
> 2. 由于Date类中，许多方法已经过时，因此现在我们通常用Date来保存时间，而使用Calendar来修改时间
> 3. 格式化时间通常使用Date类以及SimpleDateFormat类
使用 Date 类的默认无参构造方法创建出的对象就代表当前时间，我们可以直接输出 Date 对象显示当前的时间


```Java
public class Test4 {
	public static void main(String[] args) {
		Date d = new Date();
		System.out.println(d);

		/*输出 Sat Jan 14 19:13:41 CST 2017*/
	}
}
```


Sat 代表 Saturday (星期六)， Jan 代表 January (一月)， 14代表 14 号， CST 代表 China Standard Time (中国标准时间，也就是北京时间，东八区)。

<!--more-->
#### 格式转换
##### 将日期格式化，并以字符串形式输出

```Java
public class Test4 {
	public static void main(String[] args) {
		Date d = new Date();
		System.out.println(d);


		SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd号 HH时mm分ss秒");
		String s = sdf.format(d);
		System.out.println(s);

	}
}
```

> 1. 这里年，月，号，时，分，秒 可以自由换符号表示，可以换成 - 等等，也就是分隔符可以自由切换，但是英文字固定
> 2. **需要注意的是格式化日期的时候 yyyy代表年份，mm代表分钟，MM代表月份，dd代表号，HH代表24小时制度的时，hh代表12进制的时，ss代表秒**

##### 将字符串格式的日期转换为系统默认格式输出

```Java
public class Test {
	public static void main(String[] args) throws ParseException {

		String s = "2016年12月14号 19时28分34秒";
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd号 HH时mm分ss秒");


		Date date = sdf.parse(s);
		System.out.println(date);

	}
}
```

> 将日期格式化系统格式的时候，需要注意声明异常


----------

### Calendar类的使用

> java.util.Calendar 类是一个抽象类，可以**通过调用 getInstance() 静态方法获取一个 Calendar 对象**，此对象已由当前日期时间初始化，即默认代表当前时间，如 **Calendar c = Calendar.getInstance()**;这里很深刻的体现了**Java设计模式中的单例模式**

```Java
public class Test4 {
	public static void main(String[] args) {

		/*1.得到系统当前时间*/
		Date date = new Date();
		Calendar calendar = Calendar.getInstance();
		
		/*2.把要修改的时间给cal,同时进行修改*/
		calendar.setTime(date);
		calendar.set(calendar.YEAR, 2016);
		calendar.set(calendar.MONTH, 11);
		calendar.set(calendar.DATE, 31);

		/*3.修改完成后转换成Date进行保存*/
		date = calendar.getTime();
		System.out.println(date);


	}
}
```

> Calendar修改的时候，需要我们注意几个问题，月份的值是 0-11，而且一个月份的天数超过，则会自动进入下个月


----------

## 集合

> 集合长度随时可以改变，动态数组中，增长因子默认为10，大小改变，元素不变，将原先的数组复制到该数组

![这里写图片描述](http://on3w7gc9m.bkt.clouddn.com/collcetion.png)


Collection 是集合类的根接口，其中下辖子接口 List , Set, Queue.

### 动态数组 与 动态链表 区别
动态数组：增删效率低，查询效率高，**物理（实际的位置）和逻辑（花名册上的位置）均相邻，所以查询效率高**

动态链表：增删效率高，查询效率低，**物理不相邻，逻辑相邻，所以查询效率低**

### ArrayList 与 Vector 区别
ArrayList ： 异步（允许同一时间操作）
Vector ： 同步（不允许同一时间操作）


### 同步与异步区别

同步：一个接一个，效率低，安全性高
异步：一起，效率高，安全性低


----------

### 练习

例题一： 图书馆借阅时间判断合法性

```Java
public class Test3 {
	public static void main(String[] args) throws ParseException {
		Scanner input = new Scanner(System.in);

		System.out.println("请输入借书时间：");
		String s = input.nextLine();/*只能扫描输入一行*/

		System.out.println("请输入还书时间：");
		String s1 = input.nextLine();

		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH-mm-ss");

		Date date = sdf.parse(s);

		Date date1 = sdf.parse(s1);

		if( date.getTime() > date1.getTime() )
			System.out.println("借书成功！");
		else
			System.out.println("借书失败！");

	}
}
```
