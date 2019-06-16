---
title: MyBatis中的基本概念
tags: [MyBatis,框架]
categories:
- 框架
- MyBatis
date: 2017-11-22 19:25:15
---


## 前言

1. pojo：不按MVC进行分层，只有Java Bean有一些属性，还有get以及set方法
2. domain：不按MVC进行分层，只有Java Bean有一些属性，还有get以及set方法
3. po：用在持久层，相当于 pojo+xml！在页面中进行添加或者修改的时候，直接传入到action中！
	po中的类名等于表名，属性名等于字段名，还有对应的set以及get方法
4. vo：View Object表现层对象，主要用于在高级查询中接收从页面中传过来的参数，好处是扩展性强！
5. bo：用在service层，现在基本不用



pojo,domain.po,vo,bo可以用在各种层面，不会报错！（也就是说po用在表现层，vo用在表现层不报错，
因为都是普通的java bean没有语法错误）最好不用混着用，不利于代码维护！

-----

## MyBatis中的原理

![MyBatis架图](http://on3w7gc9m.bkt.clouddn.com/QQ%E5%9B%BE%E7%89%8720171119093655.png)

输入映射：对于每条Sql语句指定的输入参数的类型（只可以指定Map,基本数据类型(包括String)，以及POJO）
输出映射：返回的结果集类型（只可以指定Map,List,基本数据类型(包括String)，以及POJO）


### MyBatis中的执行器
> 当我们使用MyBatis中的SqlSession执行Sql语句的时候，MyBatis会内部调用Executor接口来执行映射文件中配置好的(也就是通过调用MappedStatement)Sql语句

1. 基本执行器
2. 缓存执行器

### 返回数据库自增主键
数据库中我们可以使用如下代码，来查询刚刚插入的记录的主键值，会返回1个0
```SQL
SELECT LAST_INSERT_ID;
```

但是如果我们在MyBatis中配置了如下代码，将会返回刚刚插入的主键记录的值

```xml
<!-- 方法1 -->
<insert id="addStudent" parameterType="com.pojo.Student" keyProperty="id" useGeneratedKeys="true">
		insert into student(name,sex,birthday) values(#{name}, #{sex}, #{birthday})
</insert>

<!-- 方法2 -->
<insert id="addStudent2" parameterType="com.pojo.Student" keyProperty="id" useGeneratedKeys="true">

		<!--
			keyProperty指定自增主键的值返回到POJO的哪个属性中
			order指相对于下面insert插入语句的执行顺序
		-->
		<selectKey keyProperty="id" order="AFTER" resultType="java.lang.Integer">
			SELECT LAST_INSERT_ID();
		</selectKey>
		insert into student(name,sex,birthday) values(#{name}, #{sex}, #{birthday})
</insert>
```

<!--more-->

### 使用UUID生成主键

> 由于数据库中的主键无法使用字符串进行自动增长，所以当我们使用字符串作为主键的时候，我们需要用到UUID算法，帮助我们生成主键，这里原理就是利用了数据库中的UUID()函数

```xml
<!-- 使用UUID主键方式,uuid必须是先生成，然后传入到student对象里面 -->
	<insert id="addStudent3" parameterType="com.pojo.Student">
		<selectKey keyProperty="id" order="BEFORE">
			<!-- 使用数据库的自动生成策略 -->
			SELECT UUID()
		</selectKey>
		insert into student(id,sex,name,birthday) values(#{id},#{sex},#{name},#{birthday})
	</insert>
```

```java

/**
 * 使用UUID生成主键
 * @author SiVan
 *
 */
public class Demo4 {

	public static void main(String[] args) {

		InputStream in = Student.class.getResourceAsStream("/SqlMapConfig.xml");
		SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
		SqlSession session = factory.openSession();
		Student student = new Student();
		student.setBirthday(Timestamp.valueOf("1997-02-22 10:10:10"));
		student.setName("吴晓文");
		student.setSex("1");
		student.setId(UUID.randomUUID().toString());
		int i = session.insert("com.dao.StudentDao.addStudent3",student);
		session.commit();
		session.close();
		System.out.println(student.getId());
		System.out.println(i);
	}

}
```



----

## 核心配置文件中引入资源文件
> 对于MyBatis,有时候我们需要配置参数在额外的资源文件中，下面以JDBC4个参数例子进行配置

编写资源文件 db.properties
>注意：后面的参数不可以包含空格

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/s59
jdbc.username=root
jdbc.password=root
```

在SqlMapConfig文件中进行配置(引入配置文件)：
```XML
<configuration>
	<!-- 引入配置文件 -->
	<properties resource="db.properties"></properties>

	<environments default="MySql">
		<environment id="MySql">
			<transactionManager type="JDBC"></transactionManager>
			<dataSource type="POOLED">
				<property name="driver" value="${jdbc.driver}"/>
				<property name="username" value="${jdbc.username}"/>
				<property name="password" value="${jdbc.password}"/>
				<property name="url" value="${jdbc.url}"/>
			</dataSource>


		</environment>
	</environments>
<mappers>
		<mapper resource="com/pojo/Student.xml"/>
</mappers>
</configuration>
```

---
