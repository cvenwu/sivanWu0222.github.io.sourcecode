---
title: 深入使用MyBatis
categories:
  - 框架
  - MyBatis
date: 2017-11-19 19:02:52
tags: [MyBatis,框架]
---

## 前言
> 前面几篇文章简要介绍了MyBatis的基本使用，以及一些基本概念，包括我们经常所用到的POJO，还有Dao的两种开发方式，但是我们经常使用的便是VO(View Object)，通过VO我们直接进行业务调用更加容易理解，因为我们每次传递的都是一个VO对象，而不是一个页面传入多个对象，尤其是对于MyBatis中的Sql操作而言，只能传入一个对象，我们可以传入Map，但是不建议这么做，建议使用我们下面所说的VO


-----

## 使用VO进行MyBatis的高级查询

### 输入映射
输入映射就是我们在Mapper配置文件中所配置的parameterType，不仅仅是基本数据类型，还可以是POJO类型，还可以是VO类型！

> 假设有一个页面只是对于用户进行操作，VO便是从页面传过来的对象。使用VO可以给我们带来很大的扩展性，进行任何的操作都可以通过VO来进行，可以用来避免使用Map，


用到的POJO类的编写：
```Java
package com.pojo;

import java.io.Serializable;
import java.sql.Timestamp;

public class Student implements Serializable{

	private Integer id;
	private String name;
	private String sex;
	private Timestamp birthday;
	public Student() {
	}
	public Integer getId() {
		return id;
	}
	public void setId(Integer id) {
		this.id = id;
	}
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
	public Timestamp getBirthday() {
		return birthday;
	}
	public void setBirthday(Timestamp birthday) {
		this.birthday = birthday;
	}
}

```

<!-- more -->


VO类的书写：
```Java

public class StudentVo {

	private Student student;

	public Student getStudent() {
		return student;
	}
	public void setStudent(Student student) {
		this.student = student;
	}
}

```


UserMapper的编写：
```Java
package com.mapper;

import java.util.List;

import com.pojo.Student;
import com.vo.StudentVo;

public interface UserMapper {
	public List<Student> findStudentByNameAndSex(StudentVo student);
}

```

配置文件的编写：注意这里参数的类型是VO类型，传值的时候是POJO.属性(因为POJO已经是VO的属性了)
```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
 <mapper namespace="com.mapper.UserMapper">

	<select id="findStudentByNameAndSex" parameterType="com.vo.StudentVo" resultType="com.pojo.Student">
		select id,name,sex,birthday from student where name like '%${student.name}%' and sex = ${student.sex}
	</select>

 </mapper>

```


测试类的编写：
```Java
import java.util.List;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import com.mapper.UserMapper;
import com.pojo.Student;
import com.vo.StudentVo;

public class StudentVoTest {

	public static void main(String[] args) {

		SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(Student.class.getResourceAsStream("/SqlMapConfig.xml"));
		SqlSession session = factory.openSession();
		UserMapper userMapper = session.getMapper(UserMapper.class);
		Student student = new Student();
		student.setName("吴");
		student.setSex("1");
		StudentVo studentVo = new StudentVo();
		studentVo.setStudent(student);

		List<Student> list = userMapper.findStudentByNameAndSex(studentVo);
		for (Student student2 : list) {
			System.out.println(student2.getId() + "\t" + student2.getName() + "\t" + student2.getBirthday());
		}

		session.close();
	}

}

```

注意事项：由于这里使用了VO，并且VO里面配置了一个POJO属性，并且设置了相应的set以及get方法，之后在POJO的配置文件中编写了具体的配置，并且参数类型是一个VO


-----

### 输出映射
1. 输出映射就是返回的结果集，可以有包装类，基本数据类型
2. 只有在数据库返回的结果为1行1列数据的时候我们才可以使用基本数据类型

#### 聚合数据查询例子

配置文件
```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!-- 编写UserMapper接口的配置文件，
	编写规则（4点）
 -->

 <mapper namespace="com.mapper.UserMapper">

	<!-- 查询学生总人数 -->
	<select id="getStudentCount" resultType="java.lang.Integer">
		select count(*) from student
	</select>

 </mapper>

```

UserMapper接口
```Java
package com.mapper;

import java.util.List;

import com.pojo.Student;
import com.vo.StudentVo;

public interface UserMapper {

	public int getStudentCount();
}

```


测试实例
```Java
package com.demo;

import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import com.mapper.UserMapper;
import com.pojo.Student;

public class GetStudentCountTest {

	public static void main(String[] args) {
		SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(Student.class.getResourceAsStream("/SqlMapConfig.xml"));
		SqlSession session = factory.openSession();
		UserMapper userMapper = session.getMapper(UserMapper.class);
		int c = userMapper.getStudentCount();
		System.out.println(c);
	}

}
```

----

## MyBatis中的动态语句

### 动态Where子句
> 之前我们在使用JDBC的时候，需要自己手动一个条件一个条件的进行拼接，但是MyBatis却给了我们极大的便利，以至于我们使用一个标签就可以解决问题！

根据姓名以及性别查询学生，如果输入了姓名和性别，则根据两个条件进行查询；如果输入了姓名，则根据姓名进行查询;如果输入了性别，则根据性别进行查询！


UserMapper.xml配置文件
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
 <mapper namespace="com.mapper.UserMapper">
	<!-- 动态Sql查询语句 -->
	<select id="dynamicWhere" parameterType="com.pojo.Student" resultType="com.pojo.Student">
		select id,name,sex,birthday from student
		<!-- 动态where子句会自动根据需要去掉行首的and -->
		<where>
			<if test="name != null and name != ''">
				and name like '%${name}%'
			</if>
			<if test="sex != null">
				and sex = #{sex}
			</if>
		</where>
	</select>

 </mapper>
```

Mapper代理接口
```Java
package com.mapper;

import java.util.List;

import com.pojo.Student;
import com.vo.StudentVo;

public interface UserMapper {


	public List<Student> dynamicWhere(Student student);
}

```

测试：
```Java
package com.demo;

import java.util.List;

import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import com.mapper.UserMapper;
import com.pojo.Student;

public class DynamicWhereTest {

	public static void main(String[] args) {
		SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(Student.class.getResourceAsStream("/SqlMapConfig.xml"));
		SqlSession session = factory.openSession();
		UserMapper userMapper = session.getMapper(UserMapper.class);

		Student student = new Student();
		student.setName("吴");
		List<Student> list = userMapper.dynamicWhere(student);
		System.out.println(list);
	}

}

```

#### 配置文件中封装SQL语句
> 有时候我们在配置文件中大量书写重复的SQL语句，效率低下，因此MyBatis可以让我们手动配置Sql语句，以便能够重用提高我们的开发效率

配置之后的UserMapper.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

 <mapper namespace="com.mapper.UserMapper">

 	<sql id="user_Where">
	 	<!-- 动态where子句会自动根据需要去掉行首的and -->
			<where>
				<if test="name != null and name != ''">
					and name like '%${name}%'
				</if>
				<if test="sex != null">
					and sex = #{sex}
				</if>
			</where>
 	</sql>

	<!-- 动态Sql查询语句 -->
	<select id="dynamicWhere" parameterType="com.pojo.Student" resultType="com.pojo.Student">
		select id,name,sex,birthday from student
		<!-- 动态where子句会自动根据需要去掉行首的and -->
		<include refid="user_Where"></include>
	</select>

 </mapper>
```


测试：
```Java
public class DynamicWhereTest {

	public static void main(String[] args) {
		//dynamicWhere
		SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(Student.class.getResourceAsStream("/SqlMapConfig.xml"));
		SqlSession session = factory.openSession();
		UserMapper userMapper = session.getMapper(UserMapper.class);

		Student student = new Student();
		student.setName("吴");
		List<Student> list = userMapper.dynamicWhere(student);
		System.out.println(list);
	}

}
```

总结：Dao配置文件中可以自己配置重用性高的SQL语句以便提高我们的开发效率
