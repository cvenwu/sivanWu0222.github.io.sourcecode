---
title: MyBatis中Dao开发的两种方式
categories:
  - 框架
  - MyBatis
date: 2017-11-20 19:02:52
tags: [MyBatis,框架]
---




## Dao的两种开发方式
1. 原始Dao的开发方法(接口与实现类，手动编写)
2. 动态代理方式(使用Mapper接口代理的方式)

### 原始Dao编写
编写StudentDao接口：
```Java
/**
 * 一个user的持久化接口
 * @author SiVan
 *
 */
public interface UserDao {

	/*查找所有学生*/
	public List<Student> findAll();

	public Student findStudentById(Integer id);

}

```

编写UserDaoImpl(也就是UserDao的实现类)
> 编写实现类的时候，我们注意到每个具体的方法都会有一个session，由于session是线程不安全的，所以session最好的作用范围便是方法之内！

```Java
/**
 * 具体的UserDao实现类
 */
import com.pojo.Student;

public class UserDaoImpl implements UserDao{

	private SqlSessionFactory factory;

	public UserDaoImpl(SqlSessionFactory factory) {
		this.factory = factory;
	}


	@Override
	public List<Student> findAll() {
		/*由于SqlSession是线程不安全的，因此SqlSession最好的作用域便是方法内*/
		SqlSession session = factory.openSession();
		return session.selectList("com.dao.StudentDao.findAll");
	}

	@Override
	public Student findStudentById(Integer id) {
		SqlSession session = factory.openSession();
		return (Student) session.selectOne("com.dao.StudentDao.findStudentById",id);
	}

}

```

编写测试类（需要初始化factory）
```Java
package com.demo;

import java.io.InputStream;
import java.util.List;

import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import com.dao.UserDao;
import com.dao.UserDaoImpl;
import com.pojo.Student;

public class UserDaoTest {

	private static SqlSessionFactory factory;

	/*初始化*/
	public static  void setUp() {
		InputStream in = Student.class.getResourceAsStream("/SqlMapConfig.xml");
		factory = new SqlSessionFactoryBuilder().build(in);
		System.out.println("s");
	}

	public static void main(String[] args) {
		setUp();
		UserDao userDao = new UserDaoImpl(factory);
		System.out.println(factory);
		System.out.println(userDao);
		List list = userDao.findAll();
		System.out.println(list);
	}

}

```


<!--more-->

-----

### 动态代理实现Dao
> MyBatis希望我们能够以1个接口的形式来书写规范，然后会自动根据我们书写的接口自动生成相应的实现类，我们只需要调用相应的方法就可以了

<strong> 注意：动态代理实现Dao需要MyBatis3.2以上的支持！！！</strong>

#### 编写Dao接口
MyBatis严格控制书写对应Mapper的接口规则：
1. 接口名字必须等于Mapper映射文件的namespace名字
2. 接口的方法名字必须等于Mapper映射文件的id名字
3. 接口的方法参数类型必须等于Mapper映射文件的参数类型
4. 接口的方法返回类型必须等于Mapper映射文件的结果集类型

注意事项：
1. Mapper对应的配置文件与我们自己书写的Mapper接口必须在同一个文件夹下
2. Mapper对应的配置文件与我们自己书写的Mapper接口名字必须相同，后缀不同

#### 编写Mapper对应的配置文件
编写Mapper对应的接口
```Java
package com.mapper;

import java.util.List;

import com.pojo.Student;

public interface UserMapper {

	public List findAll();

	public Student findStudentById(Integer id);

}

```



#### 在总配置文件中添加刚刚编写的Mapper对应的配置文件
```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!-- 编写UserMapper接口的配置文件，
	编写规则（4点）
 -->

 <mapper namespace="com.mapper.UserMapper">
 	<!-- 查询所有学生 -->
	<select id="findAll" resultType="com.pojo.Student">
		select id,name,sex,birthday from student
	</select>

	<select id="findStudentById" parameterType="java.lang.Integer" resultType="com.pojo.Student">
		select id,name,sex,birthday from student where id = #{id}
	</select>
 </mapper>
```


#### 编写测试类

```Java
package com.demo;

import java.io.InputStream;
import java.util.List;

import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import com.mapper.UserMapper;
import com.pojo.Student;

public class UserMapperTest {

	public static void main(String[] args) {

		InputStream in = Student.class.getResourceAsStream("/SqlMapConfig.xml");
		SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
		SqlSession session = factory.openSession();
		UserMapper mapper = session.getMapper(UserMapper.class);
		List<Student> list = mapper.findAll();
		System.out.println(list);

	}

}

```
