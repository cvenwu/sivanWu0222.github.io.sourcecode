---
title: dom4j与XStream的使用
tags: [Java, XML]
categories:
  - Java
  - XML
date: 2017-11-24 16:31:11
---

> 对于XML的处理，这里简单的使用开源框架Dom4j和XStream来实现

## Dom4j
> Dom4j是一个优秀的Java XML API ,主要用于读写XML格式的数据，Dom4j具有性能优异，功能强大，易于使用等特点，同时也是一个开源的软件

```Java
/**
 * @date 2017年11月23日 下午3:54:48
 * @author SiVan
 */
package com.test;

import java.util.List;

import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.DocumentHelper;
import org.dom4j.Element;

/**
 * @author SiVan
 * @time 2017年11月23日 下午3:54:48
 * @TODO TODO
 */
public class Dom4jTest {

	public static void main(String[] args) throws DocumentException {
		StringBuffer sb = new StringBuffer();
		sb.append("<?xml version=\"1.0\" encoding=\"utf-8\"?>")
		  .append("<person>")
		  .append("<name>吴晓文</name>")
		  .append("<sex>男</sex>")
		  .append("<address>山西省朔州市</address>")
		  .append("</person>");


		/* 通过解析XML字符串创建Document对象 */
		Document document = DocumentHelper.parseText(sb.toString());
		Element root = document.getRootElement();
		/* 得到根元素下面的所有子节点 */
		List<Element> elementList = root.elements();
		/* 遍历所有子节点 */
		for (Element element : elementList) {
			System.out.println(element.getName() + "=> " + element.getText());
		}

	}

}

```

<!-- more -->

-----

## XStream
> XStream是Thoughtworks公司发布的一个开源Java类库，能够实现XML与Java对象之间的转换。使用非常简单，不需要预先生成其他辅助类，也不需要依赖于任何映射文件，还具有很强大的扩展功能！


```Java
/**
 * @date 2017年11月23日 下午4:12:49
 * @author SiVan
 */
package com.test;

import com.thoughtworks.xstream.XStream;
import com.thoughtworks.xstream.io.xml.DomDriver;

/**
 * @author SiVan
 * @time 2017年11月23日 下午4:12:49
 * @TODO TODO	XStream的使用示例
 */
public class XStreamTest {

	static class Person {
		private String name;
		private String sex;
		private String address;
		public String getName() {
			return name;
		}
		public void setName(String name) {
			this.name = name;
		}
		public String getSex() {
			return sex;
		}
		public void setSex(String sex) {
			this.sex = sex;
		}
		public String getAddress() {
			return address;
		}
		public void setAddress(String address) {
			this.address = address;
		}


	}

	/**
	 * Java对象转换为XML
	 * @param person	Java对象
	 * @return
	 */
	public static String javaObject2Xml(Person person) {
		XStream xs = new XStream(new DomDriver());
		/*给Person类定义别名*/
		xs.alias("person", person.getClass());
		return xs.toXML(person);
	}

	/**
	 * XML对象转换为Java对象
	 * @param xml
	 * @return
	 */
	public static Object xml2JavaObject(String xml) {
		XStream xs = new XStream(new DomDriver());
		/*给Person类定义别名*/
		xs.alias("person", Person.class);
		Person person = (Person) xs.fromXML(xml);
		return person;
	}

	public static void main(String[] args) {

		/*创建Person对象*/
		Person p1 = new Person();
		p1.setName("吴晓文");
		p1.setSex("男");
		p1.setAddress("山西省朔州市");
		/*将p1对象转换为XML字符串*/
		System.out.println(javaObject2Xml(p1));

		/*构造XML字符串*/
		String xml = "<person><name>路遥</name><sex>男</sex><address>贵州贵阳</address></person>";
		Person p2 = (Person) xml2JavaObject(xml);
		System.out.println(p2.getName() + "\t" + p2.getSex() + "\t" + p2.getAddress());

	}

}

```

注意：默认情况下，使用XStream转换Java对象得到的XML文档根节点为完整的Java类名，但当我们使用了如下代码设置别名的时候将会返回一个Person根节点                      
> xs.alias("person", person.getClass);

当我们使用内部类的时候，需要注意，如果报出如下错误（因为我们在静态方法中创建了动态内部类）
 > No enclosing instance of type AA is accessible. Must qualify the allocation with an enclosing instance of type SimpleTh
