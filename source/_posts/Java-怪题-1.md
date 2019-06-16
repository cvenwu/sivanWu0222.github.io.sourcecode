---
title: '[Java]-怪题-1'
date: 2016-12-22 10:57:19
tags:
categories:
- Java
- 基础
---


## 例题一：


不使用中间变量的情况下将两个数字进行交换并成功输出：

```Java
public class Exchange
 {
  public static void main(String[] args)
  {
   int a = 20;
   int b = 10;

      /*方法一：通过加减法进行交换*/
   a = a + b;
   b = a - b;
   a = a - b;
   System.out.println("a:" + a + " b:" + b);/*a:10 b:20*/
      /*方法二：通过乘除法进行交换*/
	 a = a * b;
   b = a / b;
   a = a / b;
	 System.out.println("a:" + a + " b:" + b);  /*a:20 b:10*/
      /*方法三：通过位运算进行交换*/
      /*20：1 0100*/
      /*10：  1010*/
  	a = a>>1;                  /*右移1位*/
  	b = b<<1;                  /*左移1位*/
  	System.out.println("a:" + a + " b:" + b);/*a:10 b:20*/
      /*方法四：通过异或运算进行交换*/
      /*原理：一个数异或本身等于0，而且异或运算符合交换律*/
  	a = a ^ b;
  	b = a ^ b;
  	a = a ^ b;
  	System.out.println(a);
  	System.out.println(b);
    }
   }
```
<!--more-->

----------


## 例题二：

```Java
public class Exchange
    {
	    public static void main(String[] args)
	    {
			double num = 1.0/0;          /*计算的时候将0转换成0.0*/
			System.out.println(num);     /*输出infinity*/
			}
    }
/*
	特别注意：  这里计算num的 时候将除数的0 转换成0.0
*/
```


----------

## 例题三：

关于下面这个代码快

```Java
public class Test{

		public static void main(String[] args)
		{
			int millSeconds = 365 * 24 * 60 * 60 * 1000 * 1000;
			int seconds = 365 * 24 * 60 * 60 * 1000;
			System.out.println(millSeconds);
			System.out.println(seconds);
			System.out.println(millSeconds/seconds);
			System.out.println(millSeconds + Integer.MIN_VALUE - Integer.MAX_VALUE - 1);
		}
}
```

### 结果

> //-1944854528
> //1471228928
> //-1
> //-1944854528

###原因分析：

> 首先millSeconds变量产生了溢出，对于溢出的变量，如果达到了最大值，那么将继续从最小值开始，补足剩余的差值，因此最后输出的数字是-1944854528
