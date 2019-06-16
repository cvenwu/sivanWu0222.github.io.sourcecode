title: 入门Mybatis
categories:
  - 框架
  - Mybatis
date: 2017-11-18 16:28:03
tags: [Java,MyBatis]
---

## 前言：
 Mybatis与Hibernate一样，作为轻量级持久层框架，是现在的主流，并且MyBatis与Hibernate不同，MyBatis的灵活性与Hibernate不可同日而语，当我们能够以SQL语句进行轻便操作的时候，你可知道MyBatis带给我们的便利，虽然没有Hibernate封装的好，但是MyBatis扩展性更好些，也更灵活些！有人曾说Hibernate是全自动的，但是MyBatis是半自动的！


-----

## 搭建基本环境
> 使用任何框架的第一步都是搭建框架的基本环境，当我们环境搭建好之后，所谓的半个成品就出来了233333

1. 引入使用数据库的JDBC驱动jar包
2. 引入MyBatis的核心开发包
3. 引入MyBatis的依赖包，也就是log4j等
4. 创建POJO类
5. 引入log4j的配置文件
6. 引入MyBatis的POJO配置文件
7. 引入MyBatis核心配置文件


具体文件缺失，<a href="https://github.com/sivanWu0222/MyBatisDoc/blob/master/README.md">参考</a>

POJO类文件：
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

POJO的配置文件头约束
```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
```

核心配置文件头约束
```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
```

--------
## 基本使用

1. 创建一个输入流，通过流加载总配置文件
2. 通过加载进来的总配置文件创建SqlSessionFactoryBuilder来创建SessionFactory
3. 创建session
4. 编写具体的业务(增删改查，<strong>MyBatis中自动开启事务，我们只需要操作和提交事务就可以</strong>)
5. 提交事务
6. 关闭session


<!-- more -->


------

## 查询

### 查询所有的学生
```Java

/*
 * 查询所有学生
 * */
public class Demo {

	public static void main(String[] args) {

		InputStream in = User.class.getResourceAsStream("/SqlMapConfig.xml");
		SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
		SqlSession session = factory.openSession();
		List<Student> list = session.selectList("com.dao.StudentDao.findAll");
		for (Student student : list) {
			System.out.println(student.getId() + "\t" + student.getName() + "\t" + student.getSex() + "\t" + student.getBirthday());
		}

		session.close();
	}

}
```

对应的POJO映射文件中添加如下代码：
```XML
<!-- 查询所有学生 ，注意：尽管这里查询出来的是一个集合，但是结果类型依然是学生类型，
		因为我们要往集合里面添加学生类型
    id:sql语句唯一表示
		parameterType：指定传入参数类型
		resultType：返回结果类型
		#{}：占位符，起到占位作用，如果返回的结果是基本类型(string int long ....),则#{}中的变量可以任意写
		 简单的根据ID查询一个用户
		%{}：拼接符，字符串原样拼接，如果传入的参数是基本类型，那么${}中的变量必须是value
	-->
	<select id="findAll" resultType="com.pojo.Student">
		select id,name,sex,birthday from student
	</select>
```

### 通过id查询学生

#### 采用?号赋值
```Java
public class Demo2 {

	public static void main(String[] args) {
		InputStream in = Student.class.getResourceAsStream("/SqlMapConfig.xml");
		SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
		SqlSession session = factory.openSession();
		Student student = (Student) session.selectOne("com.dao.StudentDao.findStudentById",1);
		System.out.println(student.getId() + "\t" + student.getName() + "\t" +  student.getSex() + "\t" + student.getBirthday());
		session.close();
	}

}
```

对应的POJO映射文件中添加如下代码：
```XML
<!-- 根据编号查询学生,采用sql语句中?号的形式进行赋值的方法进行查取 -->
	<select id="findStudentById" parameterType="java.lang.Integer" resultType="com.pojo.Student">
		select id,name,sex,birthday from student where id = #{ss}
	</select>
```

#### 采用拼接字符
```Java
public class Demo3 {

	public static void main(String[] args) {

		InputStream in = Student.class.getResourceAsStream("/SqlMapConfig.xml");
		SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
		SqlSession session = factory.openSession();
		Student student = (Student) session.selectOne("com.dao.StudentDao.findStudentById2",2);
		System.out.println(student.getId() + "\t" + student.getName() + "\t" + student.getSex());
		session.close();

	}

}
```

对应的POJO映射文件中添加如下代码：
```XML
<!-- 根据编号查询学生，采用拼接的方式进行查取，
	这里由于传入的是基本类型，并且采用了拼接，所以只可以传入value -->
	<select id="findStudentById2" parameterType="java.lang.Integer" resultType="com.pojo.Student">
		select id,name,sex,birthday from student where id = ${value}
	</select>
```

### 根据编号区间进行查询
```Java
/**
 * 根据id区间进行查询
 * @author SiVan
 *
 */
public class Demo4 {


	public static void main(String[] args) {

		InputStream in = Student.class.getResourceAsStream("/SqlMapConfig.xml");
		SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
		SqlSession session = factory.openSession();
		Map map = new HashMap();
		map.put("min", 1);
		map.put("max", 2);
		/*通过将最大值与最小值存到Map集合中，我们可以获取map集合中的最大值，与最小值*/
		List<Student> list = session.selectList("com.dao.StudentDao.findStudentBetween", map );
		for (Student student : list) {
			System.out.println(student.getId() + "\t" + student.getName() + "\t" + student.getSex() +
				"\t" + student.getBirthday());
		}
		session.close();

	}

}


```

对应的POJO映射文件中添加如下代码：
```XML
<!-- 根据编号查询学生，采用拼接的方式进行查取，
	这里由于传入的是基本类型，并且采用了拼接，所以只可以传入value -->
	<select id="findStudentById2" parameterType="java.lang.Integer" resultType="com.pojo.Student">
		select id,name,sex,birthday from student where id = ${value}
	</select>
```

### 根据编号大小进行查询
```Java
/**
 * 查询编号小于2号的学生
 * @author SiVan
 *
 */
public class Demo5 {

	public static void main(String[] args) {

		InputStream in = Student.class.getResourceAsStream("/SqlMapConfig.xml");
		SqlSessionFactory factory = new SqlSessionFactoryBuilder()
				.build(in);
		SqlSession session = factory.openSession();
		List<Student> list = session.selectList("com.dao.StudentDao.findLess",2);

		for (Student student : list) {
			System.out.println(student.getId() + "\t" + student.getName() + "\t "+ student.getSex());
		}

		session.close();

	}

}

```

对应的POJO映射文件中添加如下代码：
```XML
<!-- 查编号小于参数的学生 -->
	<select id="findLess" resultType="com.pojo.Student" parameterType="java.lang.Integer">
		select id,name,sex,birthday from student where id &lt; #{id}
	</select>
```


### 动态Where子句进行查询

```Java
/**
 * 使用动态where子句，只可以去掉前面的and而不可以去掉后面的and
 * @author SiVan
 *
 */
public class Demo6 {

	public static void main(String[] args) {
		InputStream in = Student.class.getResourceAsStream("/SqlMapConfig.xml");
		SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
		SqlSession session = factory.openSession();

		Map map = new HashMap();
		map.put("id", 1);
		map.put("name", "李飞");
		List<Student> list = session.selectList("com.dao.StudentDao.dynamicWhere", map);
		for (Student student : list) {
			System.out.println( student.getId() + "\t" + student.getName() + "\t" + student.getSex() + "\t" + student.getBirthday());
		}
		session.close();

	}

}

```

对应的POJO映射文件中添加如下代码：
```XML
<!-- 动态SQL之Where子句，也就是where中的部分条件可以有可以没有 -->
	<select id="dynamicWhere" resultType="com.pojo.Student" parameterType="map">
		select id,name,sex,birthday from student
		<where>
			<if test="id!=null">
				and id = #{id}
			</if>
			<if test="name!=null">
				and name = #{name}
			</if>
			<if test="sex!=null">
				 and sex = #{sex}
			</if>
		</where>
	</select>
```
----

## MyBatis中的物理分页
> MyBatis中的物理分页可以通过RowBounds进行分页，与逻辑分页相比，物理分页效率更加低下，当我们需要编号为23-50的人的数据的时候，其实通过物理分页我们将会查询到50个人的数据，而通过逻辑分页我们只需要查询编号为23-50的人的数据


```XML
<!-- 查所有学生 ：逻辑分页 -->
   <select id="getAll" resultType="stu">
       select <include refid="fields"/> from student
   </select>

    <!-- 查所有学生 ：物理分页 -->
   <select id="fenye" resultType="stu" parameterType="map">
       select <include refid="fields"/> from student limit #{x},#{y}
   </select>
```



```Java
/*这里是使用了物理分页*/
public class fenye {
	public static void main(String[] args) {
		InputStream is = fenye.class.getResourceAsStream("/SqlMapConfig.xml");
		SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(is,"MySQL");

		SqlSession  session = factory.openSession();

		Map map = new HashMap();
		map.put("x", 5);
		map.put("y", 5);
		List<Student> list = session.selectList("com.dao.StudentDAO.fenye", map);
		for (Student s : list) {
			System.out.println(s.getId()+"\t"+s.getSname());
		}
		session.commit();
		session.close();

	}

}

```

----

## 得到主键
> 项目中我们经常会使用到获取刚刚插入数据的主键，但是如果我们没有进行特别的配置，MyBatis将不会给我们返回主键，而是返回一个NULL或者0，所以我们需要进行如下配置来获取刚刚插入数据的键值


主键自动增长的时候
```XML
<!-- 新增学生 -->
   <insert keyProperty="id" useGeneratedKeys="true" id="add" parameterType="com.pojo.Student">

      <!--
        keyProperty指定主键返回的值存入到哪个属性里面，order值的是获取主键的时机，实在插入语句执行前还是执行后，
      -->
        <selectKey keyProperty="id" order="AFTER" resultType="java.lang.Integer">
           select LAST_INSERT_ID()
        </selectKey>

        insert into student(sname,sex,sage) values(#{sname},#{sex},#{sage})
   </insert>

```
