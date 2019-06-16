---
title: 一个简单的Spring核心机制和其他方面的认知
categories:
  - 框架
  - Spring
date: 2017-11-06 17:41:03
tags: [Spring, Java, XML]
---

## 前言
> 最近开始了解Spring，很多人都在告诉我需要使用Spring的时候，我们需要配置很多东西，很大一部分是在XML里面完成，而且还发现Spring可以通过我们配置的XML文件来创建对象，事实上就是这么做的，就好比如我们需要一把刀的时候，只需要去市场就可以买到，而不需要我们自己造了！感觉瞬间现代化了！

-----

## 一个简单的Spring核心机制
> Spring里面融合的思想有很多，最核心的便是IOC, 也就是 Inverse of control，通过Spring自己带的一个容器，将我们所需要的对象创造出来，而不需要我们手工创造，极大的减少了代码的耦合！

### 手工实现一个简单的Spring核心机制
> Spring 如何帮助我们创建对象呢？答案便是我们通过配置XML文件，Spring会帮助我们通过配置好的XML文件，进行对象的创建！

1. 手动配置XML文件
2. 通过我们自己写的一个SpringUtils来对XML文件进行解析，创建对象

```XML
<?xml version="1.0" encoding="UTF-8"?>
<beans>
	<bean id="student" name="com.pojo.Student"></bean>
</beans>
```

```Java

import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

public class SpringUtils {


	public static Object getElement(String id) throws DocumentException, ClassNotFoundException, InstantiationException, IllegalAccessException {
		SAXReader reader = new SAXReader();
		Document document = reader.read("src/beans.xml");
		Element element = (Element) document.selectSingleNode("beans/bean[@id='" + id + "']");
		String name = element.attributeValue("name");
		Class c = Class.forName(name);
		return c.newInstance();

		/*上面的代码等效为如下代码
		return Class.forName(((Element)new SAXReader().read("src/beans.xml").selectSingleNode("beans/bean[@id='"+ id + "']")).attributeValue("name")).newInstance();
    */
	}

}
```

<!-- more -->

------

## Spring的配置
> 当我们通过Spring进行配置文件的时候，有时候需要给具体的某个POJO类配置相应的属性为 数组或者Map集合，可以采用如下配置


### 属性的配置方式

1. 构造注入
```XML
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:util="http://www.springframework.org/schema/util"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="
	http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-2.0.xsd
	http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-2.0.xsd
	http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-2.0.xsd
	http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-2.0.xsd
	http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context-2.5.xsd">


    <bean id="student" class="com.pojo.Student">
    		<!-- 这里将会自动匹配两个参数的构造方法，如果构造方法不存在将会报错 -->
 	      <constructor-arg value="小王小马"></constructor-arg>
 	      <constructor-arg value="1"></constructor-arg>
    </bean>

</beans>

```

2. set注入

> 我们不仅仅可以通过构造方法进行注入，而且还可以通过set注入，例子如下

```XML
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:util="http://www.springframework.org/schema/util"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="
	http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-2.0.xsd
	http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-2.0.xsd
	http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-2.0.xsd
	http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-2.0.xsd
	http://www.springframework.org/schema/context
  http://www.springframework.org/schema/context/spring-context-2.5.xsd">

	<bean id="Student" class="com.pojo.Student">
		<property name="stuid" value="3"></property>
		<property name="stuname" value="李四"></property>
  </bean>
</beans>

```

> 通过这种XML配置方式，当创建好对象之后，给对象进行赋值的时候将会使用属性对应的set方法，如果没有属性对应的set方法，将会报错


#### 配置数组属性以及Map集合

```XML
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:util="http://www.springframework.org/schema/util"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="
	http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-2.0.xsd
	http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-2.0.xsd
	http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-2.0.xsd
	http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-2.0.xsd
	http://www.springframework.org/schema/context
  http://www.springframework.org/schema/context/spring-context-2.5.xsd">


	<bean id="Student" class="com.pojo.Student">
		<property name="stuid" value="3"></property>
		<property name="stuname" value="李四"></property>
    <!-- 这里我们给数组属性进行配置 -->
		<property name="loves">
			<list>
				<value>看书</value>
				<value>写博客</value>
				<value>玩游戏</value>
				<value>听歌</value>
			</list>
		</property>

    <!-- 这里我们配置Map集合 -->
		<property name="others">
			<map>
				<entry key="age" value="18"></entry>
				<entry key="height" value="180"></entry>
			</map>
		</property>
	</bean>
</beans>
```


#### 属性也是一个对象
> 有的时候我们给属性赋值的时候，属性也是一个对象，这个时候我们就必须配置ref, 通过配置ref指向的类，我们便可以使用该属性，如果属性不是一个对象而是一个固定值，我们需要使用value进行配置
```XML
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:util="http://www.springframework.org/schema/util"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="
	http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-2.0.xsd
	http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-2.0.xsd
	http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-2.0.xsd
	http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-2.0.xsd
	http://www.springframework.org/schema/context
  http://www.springframework.org/schema/context/spring-context-2.5.xsd">

  <bean id="StudentDAO" class="com.dao.StudentDAO"></bean>

  <bean id="StudentService" class="com.service.StudentService">
    <!--
          当我们所配置的对象依赖于另一个对象的时候，可以配置ref属性，给定我们所以依赖对象的链接
          如果是一个固定的值，我们可以配置 value 属性
    -->
    <property name="studentDAO" ref="StudentDAO"></property>
  </bean>
</beans>
```

------

## 总结：
> Spring配置文件很强大，如果能够熟练应用Spring的配置文件，Spring的运用就会更加娴熟！
