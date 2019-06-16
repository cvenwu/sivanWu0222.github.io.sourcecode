---
title: Spring中使用注解
date: 2017-10-26 10:51:29
tags: [Spring,Java]
categories:
- 框架
- Spring
---


## 使用注解进行实例Bean
```Java
import org.springframework.stereotype.Component;

@Component(value="user1")
/*类似于<bean id="user1" class="com.pojo.User"></bean>*/
public class User {

	public void add() {
		System.out.println("user----------add--------");
	}

}
```

### 测试
```Java
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * 使用注解进行装载Bean
 * @author SiVan
 *
 */
public class Test {
	@org.junit.Test
	public void add() {
		ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");
		User user = (User)context.getBean("user1");
		System.out.println(user);
		user.add();
	}

}
```
-----

## 使用注解创建单实例/多实例

```Java
import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Component;

@Component(value="user1")
@Scope(value="prototype")     /*指令多实例*/
/*指定单实例 @Scope(value="singleton")*/
public class User {

	public void add() {
		System.out.println("user----------add--------");
	}

}
```


------
## 注意事项

### NoSuchBeanDefinitionException
> 由于配置文件的包名路径没有写对，所以没有搜索到，确保配置文件类的路径正确

<!-- more -->

### 使用注解的时候出现如下错误：
> The prefix "context" for element "context:component-scan" is not bound.

#### 解决办法
更换Spring总配置文件的头约束，更改为
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context" xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
        <!-- 开启注解说明，会自动扫描该包，扫描包下面的类，方法是否有注解 -->
	<context:component-scan base-package="com.pojo"></context:component-scan>

  <!-- 多个注解可以更改为如下 -->
  <context:component-scan base-package="com.pojo,com.annotation"></context:component-scan>

  <!-- 也可以直接更改为如下配置 -->
  <context:component-scan base-package="com"></context:component-scan>


  <!-- 只会扫描属性上的注解 -->
  <context:annotation-config></context:annotation-config>
</beans>
```
