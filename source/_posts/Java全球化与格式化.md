---
title: Java全球化与格式化
date: 2017-07-19 22:38:55
tags: [Java, Internationalization]
categories:
- Java
- Internationalization
---

> 最近几年，随着软件蔓延全球的趋势，全球化的脚步不断前进，全球化就意味着我们必须根据不同用户所在的不同地域使用不同的语言，其中最简单的就是根据用户所在地区进行相应语言的设置，也就是本地化，因此全球化离不开本地化！

## Java中的全球化
> 最近几年，随着JDK版本的的深入，Java也不断升级，提供了对不同国家和不同地区的支持！

### 思路
将程序中的标签，提示等信息放在资源文件中，根据用户所在的地区进行相应的选择，其中每个资源文件采用key----value形式进行配对，每个资源文件中的key不变，随着地区进行value的改变

### 用到的类
1. java.util.Locale         加载国家、资源语言包
2. java.util.ResourceBundle   封装特定国家、地区环境
3. java.text.MessageFormat    对带占位符的字符串进行格式化

### 资源文件命名规范
> 对于不同语言资源文件的配置，我们也有一套规范

1. 值通常采用 Key ----- value 形式进行使用
2. 资源文件通常采用 propertyname_language.properties 的文件名形式进行命名

### 获取Java支持的国家和语言

```Java
/**
 * 事实上JAVA不可能支持所有国家和地区，如果需要获取JAVA支持的国家和地区，
 * 则可以调用Locale类的getAvailableLocales()方法，返回一个Locale数组
 * 包含了支持的国家和地区，
 */

public class GetSupportClassAndArea {
  public static void main(String[] args) {
    Locale[] locales = Locale.getAvailableLocales();
    for (Locale locale : locales) {
      System.out.println(locale.getDisplayCountry() + "=" +   locale.getCountry() + "\t" + locale.getLanguage() + "=" + locale.getDisplayLanguage());
    }
  }
}
```

<!-- more -->

### 编写一个简单的国际化程序
> 这里进行一个简单的国际化程序的演示

1. 编写简单的源代码程序

```Java
public class Hello {
	public static void main(String[] args) {
		System.out.println("Hello World!");
	}
}
```
2. 编写配置文件1 mess.properties
```properties
hello = 你好!
World = 世界!
```

3. 将配置文件编译成配置中文properties文件
采用如下命令进行配置
>  native2ascii 源配置文件 目的配置文件

4. 编写如下Java程序进行测试
```Java

public class Hello {
	public static void main(String[] args) {


		/*取得系统默认的国家/语言环境*/
		Locale myLocale = Locale.getDefault(Locale.Category.FORMAT);

		/*根据指定国家/语言环境加载资源配置文件*/
		ResourceBundle bundle = ResourceBundle.getBundle("mess", myLocale);

		/*打印从资源文件中获得的信息*/
		System.out.println(bundle.getString("hello"));
	}

}

```
