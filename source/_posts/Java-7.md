---
title: '[Java]-7'
date: 2016-12-30 10:55:06
tags:
categories:
- Java
- 基础
---



## Java中类与类之间的转换

> **类与类之间的转换如果要强转，则必须要有继承关系**。否则，编译器会报出，ClassCastException异常

### 包装类之间的转换

> 思路是：将包装类进行拆箱，转换成基础数据类型之后，再进行转换，最后进行装箱

例如：Integer与Double的转换


```Java
public class Test {
	public static void main(String[] args)
	{
		Integer a = 10;
		int b = a;
		double m = b;
		Double c = m;
		System.out.println(c);
	}


}
```
<!--more-->

----------

## String类详解


> 1. String类也是Java中的lang包中的一个类，众所周知，lang包中的所有类都不需要导入就可以使用，**String类与System,Scanner类一样，都是Java中的一个预定义类型，而且String类是一个引用类型，任何Java类型都可以将变量表示为引用类型。使用引用类型声明的变量都是引用变量，引用在一个对象里。**
> 2. String 类中 hashcode根据内容而生成（**生成方式不同，即使生成内容相同，也不能说地址相同）**
> 3. 字节是计算机存储信息的基本单位，1 个字节等于 8 位， gbk 编码中 1 个汉字字符存储需要 2 个字节，1 个英文字符存储需要 1 个字节。所以我们看到上面的程序运行结果中，每个汉字对应两个字节值，如“学”对应 “-47 -89” ，而英文字母 “J” 对应 “74” 。同时，我们还发现汉字对应的字节值为负数，原因在于每个字节是 8 位，最大值不能超过 127，而汉字转换为字节后超过127，如果超过就会溢出，以负数的形式显示。

例如 ： 代码如下

```Java
public class Test {
	public static void main(String[] args) {
		Object object = new Object();
		System.out.println(object.hashCode());
		Integer integer = new Integer(7051261);
		System.out.println(integer.hashCode());



	}
}
```

### 创建的两种形式

1. 常量池中创建（内容唯一，不允许重复出现，在编译的时候就可以确定）
2. 堆中创建（在JVM中只存在一个堆，但是在运行时才可以确定）
**在堆中创建的时候，一般做两件事：第一，检查常量池是否存在与其相同的。第二，如果有则直接复制过来，如果没有，在堆中新建，并且保存常量池**

> 1. 从大体上来讲，Java中的内存分为 常量池，堆，以及栈（存放引用，也就是我们常说的遥控器，花名册），关于进一步内存详解，[请移步](http://www.cnblogs.com/xiohao/p/4296088.html)
> 2. 一旦一个字符串在内存中创建，则这个字符串将不可改变。如果需要一个可以改变的字符串，我们可以使用StringBuffer或者StringBuilder（后面章节中会讲到）。


### 区别

> 1. **常量池中创建的 String 对象创建后则不能被修改，是不可变的，所谓的修改其实是创建了新的副本，则会在常量池中产生一个副本，所指向的内存空间不同**
> 2. 另外需要注意的是 ： String类中重写了equals方法，比较的是内容是否相同，而 == 判断的是地址是否相同，也就是哈希码是否相同
> 3. 常量在编译中就可以确定，但是变量在运行时才可以确定

```Java
public class Test {
	public static void main(String[] args) {
		/*编译时就已经创建，并且放在常量池中*/
		String str = "hello";

		/*执行时才会确定，因此放到堆中*/
		String str2 = new String("hello");
		String s = "a" + "bc";/* 等同于 String s = "abc";  //编译的时候这里会进行优化 s = abc*/
		String s4 = s + str ;/*运行时确定的值放堆中 */

		System.out.println(str == str2);       /*false*/
		System.out.println(str.equals(str2));  /*true*/
	}
}
```


### 常用方法

![字符串中的方法](http://img.mukewang.com/53d9f7d200010bb007780366.jpg)



### 使用注意事项

1. 使用方法并没有赋值，因此结果仍然不变
2. 使用charAt 方法的时候不要越界
3. 使用 indexOf 进行字符或字符串查找时，如果匹配返回位置索引；如果没有匹配结果，返回 -1
4. 使用 substring(beginIndex , endIndex) 进行字符串截取时，包括 beginIndex 位置的字符，不包括 endIndex 位置的字符，因此如果只是截取之后的字符，直接使用 subString(int beginIndex)
5. **数组中，length是属性，而String类中，length()是方法，**
6. intern() 方法是将字符串对象放入到常量池中，如果已经存在，则返回池中的字符串。
7. String类中的trim()方法去除的是前后两端的空白字符，包括字符’ ‘,’\t’,’\r’,’\n’



```Java
public class Test {
	public static void main(String[] args) {
		String s = "a";
		s = s.concat("bc");
		System.out.println(s.equals("abc"));
		String s2 = "abc";
		System.out.println(s==s2);

		System.out.println(s2.startsWith("ab"));/*是否以小写ab开头*/
		System.out.println("ABC".equals(s2));
		System.out.println("ABC".equalsIgnoreCase(s2));/*忽略大小写，看是否匹配，具体查看API*/

		String s11 = "hello";
		String s12 = s11.concat(" world");
		System.out.println(s11);/*hello*/
		System.out.println(s12);/*hello world*/
		String s13 = new String("indei");
		s13.intern();
	}
}
```


**这里讲述了转义符，以及replaceAll方法的使用，”.” 号作为通配符，指的是所有的**

```Java
public class Test11 {
	public static void main(String[] args) {
		String s = "http://www.baidu.com";
		String s1 = s;
		s = s.replaceAll("\\.", " ");		
		/*这里使用双\转义，使用两个\，首先对一个\进行转译，然后只有一个\.  再通过\对点号进行转义*/

		System.out.println(s);	/*http://www baidu com*/
		System.out.println(s1); /*http://www.baidu.com*/

		s1 = s1.replace(".", "");
		System.out.println();   /*输出为空*/
	}
}
```

### trim方法

使用trim 方法需要注意，只能去掉首尾两个地方的空格，不可以去掉其他地方的空格

```Java
public class Test {
	public static void main(String[] args) {
		String  s = "  hello world!  ";

		System.out.println(s.trim());
		/*hello world!*/
	}
}
```

### contact方法：拼接字符串


```Java
public class Test16 {
	public static void main(String[] args) {
		String s = "abc";
		s = s + "de";
		s = s + "fg";
		System.out.println(s);
		StringBuffer sb = new StringBuffer("abc");
		sb.append("de");
		System.out.println(sb);

		String s2 = sb.toString();
		System.out.println(s2);

	}
}
```

从代码中我们可以看到，当频繁操作字符串时，就会额外产生很多临时变量。**容易产生很多垃圾内存**。使用 StringBuilder 或 StringBuffer 就可以避免这个问题。至于 StringBuilder 和StringBuffer ，它们基本相似，不同之处，**StringBuffer 是线程安全的，而 StringBuilder 则没有实现线程安全功能，所以性能略高**。因此一般情况下，**如果需要创建一个内容可变的字符串对象，应优先考虑使用 StringBuilder 类。**


----------
练习一：

> 判断邮件格式以及Java源代码文件名是否合法

```Java
public class HelloWorld {
    public static void main(String[] args) {
	        /*Java文件名*/
			String fileName = "HelloWorld.java";
		    /* 邮箱*/
			String email = "laurenyang@imooc.com";

			/* 判断.java文件名是否正确：合法的文件名应该以.java结尾*/
		    /*
		    参考步骤：
		    1、获取文件名中最后一次出现"."号的位置
		    2、根据"."号的位置，获取文件的后缀
		    3、判断"."号位置及文件后缀名
		    */
		    /*获取文件名中最后一次出现"."号的位置*/
			int index = fileName.lastIndexOf('.');

		    /* 获取文件的后缀*/
			String prefix = fileName.substring(index + 1, fileName.length());
		    System.out.println(prefix.equals("java"));
			/* 判断必须包含"."号，且不能出现在首位，同时后缀名为"java"*/
			if (    index != -1 && index != 0 &&     prefix.equals("java")  ) {
				System.out.println("Java文件名正确");
			} else {
				System.out.println("Java文件名无效");
			}

		    /* 判断邮箱格式是否正确：合法的邮箱名中至少要包含"@", 并且"@"是在"."之前*/
		     /*
		    参考步骤：
		    1、获取文件名中"@"符号的位置
		    2、获取邮箱中"."号的位置
		    3、判断必须包含"@"符号，且"@"必须在"."之前
		    */
		    /* 获取邮箱中"@"符号的位置*/
				int index2 = email.indexOf('@');

		    /* 获取邮箱中"."号的位置*/
			int index3 = email.indexOf('.');

			/*判断必须包含"@"符号，且"@"必须在"."之前*/
			if (index2 != -1 && index3 > index2) {
				System.out.println("邮箱格式正确");
			} else {
				System.out.println("邮箱格式无效");
			}
}
}
```


练习二：

> 生成随机验证码

```Java

public class Prac {
	public static void main(String[] args)
	{

		/*方法一：自己写*/
		String s1 = "abcdefghijklmnopqrstuvwxyz0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ";
		for(int i = 0; i < 4; i++)
		{
			System.out.print(s1.charAt((int)(Math.random() * 63 )));
		}

		/*方法二：推荐使用的方法*/
		String s = "abcdefghijklmnopqrstuvwxyz";
		Random random = new Random();
		char[] chs = new char[4];
		for (int i = 0; i < 4; i++) {
			char ch = s.charAt(random.nextInt(26));
			chs[i] = ch;
		}
		String code = new String(chs);
		System.out.println(code);
	}
}
```


注意：方法二将验证码保存了，方法与用户输入进行比对，然而方法一只是完成了生成验证码的任务，没有按照用户体验考虑


## String与基本数据类型之间的转换
###  String与int 类型之间的转换

```Java
public class Test {
	public static void main(String[] args) {

		System.out.println('a' + 'b');/*这里返回的是a与b的Ascii码的和*/
		/*整数转字符串*/
		/*方法一*/
		int a = 10;
		String s = a + "";
		
		/*方法二*/
		Integer i = a;
		String s1 = i.toString();


		/*字符串转整数*/
		int a1 = Integer.parseInt(s);
		System.out.println(a1);

	}
}
```
