---
title: XML创建与解析
categories:
  - Java
  - XML
date: 2017-11-06 08:49:50
tags: [XML, Java]
---

## 前言：
> 这里记录自己之前使用dom进行写XML，使用SAX 进行读取XML

### 两者异同点
1. dom解析是基于文档的，如果解析一个XML文件，将会把文件的所有内容加载到内存中，方便反复查询，虽然效率高，但是占内存，尤其是XML文件很大的时候
2. sax(simple API for xml)解析xml的另一种方法，是一个基于事件流的解析，当我们使用XML进行解析的时候，用到哪里，就读到哪里，如果没有保存，数据将会丢失

-----------

## 创建一个简单的XML文档
1. 生成一个空白的XML文档
2. 添加根节点和属性
3. 根节点下面添加子节点和属性
4. 对在JVM中生成的XML文档保存到本机

```Java

import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.UnsupportedEncodingException;

import org.dom4j.Document;
import org.dom4j.DocumentHelper;
import org.dom4j.Element;
import org.dom4j.io.XMLWriter;

/**
 * 创建XML文档，并且保存到d:/classes.xml
 * @author SiVan
 *
 */
public class CreateDocument {

	public static void main(String[] args) throws IOException {

    /*创建一个XML文档*/
		Document document = DocumentHelper.createDocument();

		/*创建根节点*/
		Element element = document.addElement("classes");
		element.addAttribute("id", "1");
		element.addAttribute("name", "root");
		element.setText("总班级");

		/*创建子节点*/
		Element element2 = element.addElement("class");
		element2.addAttribute("name", "班级1");
		element2.addAttribute("id", "01");
		element2.setText("第1个班级");

		Element element3 = element.addElement("class");
		element3.addAttribute("name", "班级2");
		element3.addAttribute("id", "02");
		element3.setText("第2个班级");

    /*将创建的XML文档保存到本机*/
		XMLWriter writer = new XMLWriter(new FileOutputStream(new File("d:/classes.xml")));
		writer.write(document);

	}

}

```

<!-- more -->

------

## 读取XML文档中的单个节点
> 通常我们需要对XML文档中的某个节点进行读取，可以采用如下方法

1. 找到XML文档，读取到JVM中
2. 找到我们要找的那个节点


```XML
<?xml version="1.0" encoding="UTF-8"?>
<books>
  <book id="11" price="456789">天龙八部</book>
  <book id="2" price="666">武林秘籍</book>
</books>
```

```Java
package com.prac;

import java.io.File;
import java.net.MalformedURLException;

import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

/**
 * 读取Document文档，并且获取单个节点的值
 * @author SiVan
 *
 */
public class Test2 {

	public static void main(String[] args) throws MalformedURLException, DocumentException {

		SAXReader reader = new SAXReader();
		Document document = reader.read(new File("d:/books.xml"));
		Element element = (Element) document.selectSingleNode("books/book[@id=1]");

		System.out.println(element.getName() + "\t" + element.getText() + "\t" + element.attributeValue("id") + "\t" + element.attributeValue("price"));


	}

}

```


-----------

## 获取XML的所有节点
> 有些时候，我们需要获取XML的所有节点，从而形成一个DOM树




XML文档如下

```XML
<?xml version="1.0" encoding="UTF-8"?>
 <contactList>
     <contact id="001" class="style">
         <name>张三</name>
        <age>20</age>
         <phone>134222223333</phone>
         <email>zhangsan@qq.com</email>
         <qq>432221111</qq>
     </contact>
    <contact id="002">
        <name>李四</name>
        <age>20</age>
        <phone>134222225555</phone>
        <email>lisi@qq.com</email>
        <qq>432222222</qq>
    </contact>
    <contactTwo>
        <name>王五</name>
        <age>32</age>
       <phone>465431341</phone>
        <emali>af@qq.com</emali>
        <qq>46164694</qq>
    </contactTwo>
    <test>测试</test>
    <test>其他用途</test>
</contactList>
```

```Java
public class QuerySingleNode {

	public static void main(String[] args) throws MalformedURLException, DocumentException {

    /*采用sax解析*/
		SAXReader reader = new SAXReader();

		Document document = reader.read(new File("src/contact.xml"));

		List list = document.selectNodes("contactList/contact");


		for (Object object : list) {
			Element element = (Element) object;
			System.out.println(element.attributeValue("id") + "\t" + element.attributeValue("class") + "\t" + element.elementText("name") + "\t" + element.elementText("age") + "\t" + element.elementText("email") + "\t" + element.elementText("phone") + "\t" + element.elementText("qq"));
		}

	}

}
```


------

## 修改XML文档中某个节点的属性

> 如果要修改XML文档中某个节点的属性，我们需要通过解析XML文档找到这个节点，然后进行修改！

```Java
package com.prac;

import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.net.MalformedURLException;

import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;
import org.dom4j.io.XMLWriter;

/**
 * 首先读取Document内容，然后修改Document内容中的某一处值
 * @author SiVan
 *
 */
public class Test4 {


	public static void main(String[] args) throws DocumentException, IOException {

		SAXReader reader = new SAXReader();
		Document document = reader.read(new File("d:/books.xml"));
		Element element = (Element) document.selectSingleNode("books/book[@id=1]");

		if( element != null ) {
			element.attribute("id").setValue("11");
			element.attribute("price").setValue("456789");
		}

		XMLWriter writer = new XMLWriter(new FileOutputStream(new File("d:/books.xml")));
		writer.write(document);
		writer.close();

	}

}

```

-----

总结： XML文档解析方法很多，但是常用的便是dom解析和sax解析，dom解析是读取一次，全部载入内存中，虽然效率高，但是很占内存；sax解析则是基于事件流的(用到哪里，读取哪里)，因此我们修改了节点的内容或者属性的时候，如果我们没有再次写到本机中，并不会真正修改本机上的xml文件！
