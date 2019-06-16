title: 手动搭建Hibernate开发环境
categories:
  - 框架
  - Hibernate
date: 2017-11-12 21:40:57
tags: [Java,Hibernate]
---

## 前言
> Hibernate框架是一个开源的轻量级框架，与传统的EJB（Sun 公司提供的J2EE开发规范，重量级的一个框架，重量级是因为EJB框架依赖jar包较大，使用很麻烦）完全不同

Hibernate的核心便是ORM（Object Relation Mapping）,也就是我们传统Java中的面向对象的关系应用到了数据库中（并不是使得数据库成为面向对象数据库，而是通过一种XML的配置文件，将我们的操作转变为Hibernate所提及的面向对象操作）
1. Object：特指我们常说的Java中的对象类
2. Relation：也就是我们常说的表的结构
3. Mapping：XML配置文件，作为两者（Object与Relation）的中间桥梁

### 优点
1. Hibernate对于不同的数据库都可以使用Hibernate中特有的hql语句进行操作，只是配置文件相差而已！
2. Hibernate封装了我们之前的JDBC的操作，极大程度的避免了重复繁琐的代码
3. Hibernate性能较前任EJB相比有极大的提高，映射文件也很灵活，支持多种关系映射，例如一对多，多对多等

> 相比Mybatis来说，Hibernate封装更为彻底，所以相比Mybatis来说，灵活性比较欠缺

------

## 手动搭建Hibernate开发环境
1. 将数据库驱动包导入到WEB-INF下面的lib里面，（很多人可能会问Hibernate不是已经可以用面向对象的思维来操作数据库了么，但是为什么还是需要导入数据库jar包呢，原因很简单，因为Hibernate是跨数据库的，它无法识别我们使用的是哪个数据库来进行开发，所以需要我们收到导入相对应数据库的jar包）
2. 导入Hibernate开发所必须的core开发包，<a href="https://github.com/sivanWu0222/HibernateDoc/tree/master/jar/required">点击下载</a>
3. 导入Hibernate所依赖的jar包(1个是log4j规范接口包，1个实现包，另一个Hibernate规范接口包) <a href="https://github.com/sivanWu0222/HibernateDoc/tree/master/jar/dependency/log4j"></a>
4. 手动编写一个JavaBean（<strong>JavaBean中的所有属性全部使用包装类，而不是基本类型，例如当我们插入一个顾客，如果年龄忘记赋值，则为0，但是包装类型则为null，因此建议使用包装类</strong>）：
```SQL
SET FOREIGN_KEY_CHECKS=0;
-- ----------------------------
-- Table structure for `cst_customer`
-- ----------------------------
DROP TABLE IF EXISTS `cst_customer`;
CREATE TABLE `cst_customer` (
  `cust_id` bigint(32) NOT NULL AUTO_INCREMENT,
  `cust_name` varchar(32) NOT NULL,
  `cust_user_id` bigint(32) DEFAULT NULL,
  `cust_create_id` bigint(32) DEFAULT NULL,
  `cust_source` varchar(32) DEFAULT NULL,
  `cust_industry` varchar(32) DEFAULT NULL,
  `cust_level` varchar(32) DEFAULT NULL,
  `cust_linkman` varchar(64) DEFAULT NULL,
  `cust_phone` varchar(64) DEFAULT NULL,
  `cust_mobile` varchar(16) DEFAULT NULL,
  PRIMARY KEY (`cust_id`)
) ENGINE=InnoDB AUTO_INCREMENT=94 DEFAULT CHARSET=utf8;
```

<!-- more -->

```Java
public class Customer {
	/*使用包装类，因为默认值是null*/
	private Long cust_id;
	private String cust_name;
	private Long cust_user_id;
	private Long cust_create_id;
	private String cust_source;
	private String cust_industry;
	private String cust_level;
	private String cust_linkman;
	private String cust_phone;
	private String cust_mobile;
	public Long getCust_id() {
		return cust_id;
	}
	public void setCust_id(Long cust_id) {
		this.cust_id = cust_id;
	}
	public String getCust_name() {
		return cust_name;
	}
	public void setCust_name(String cust_name) {
		this.cust_name = cust_name;
	}
	public Long getCust_user_id() {
		return cust_user_id;
	}
	public void setCust_user_id(Long cust_user_id) {
		this.cust_user_id = cust_user_id;
	}
	public Long getCust_create_id() {
		return cust_create_id;
	}
	public void setCust_create_id(Long cust_create_id) {
		this.cust_create_id = cust_create_id;
	}
	public String getCust_source() {
		return cust_source;
	}
	public void setCust_source(String cust_source) {
		this.cust_source = cust_source;
	}
	public String getCust_industry() {
		return cust_industry;
	}
	public void setCust_industry(String cust_industry) {
		this.cust_industry = cust_industry;
	}
	public String getCust_level() {
		return cust_level;
	}
	public void setCust_level(String cust_level) {
		this.cust_level = cust_level;
	}
	public String getCust_linkman() {
		return cust_linkman;
	}
	public void setCust_linkman(String cust_linkman) {
		this.cust_linkman = cust_linkman;
	}
	public String getCust_phone() {
		return cust_phone;
	}
	public void setCust_phone(String cust_phone) {
		this.cust_phone = cust_phone;
	}
	public String getCust_mobile() {
		return cust_mobile;
	}
	public void setCust_mobile(String cust_mobile) {
		this.cust_mobile = cust_mobile;
	}
}
```
5. 编写JavaBean所对应的XML映射文件（新建一个XML文件，也就是将Java中的类与数据库中的表进行关联），默认文件名字为 <strong>类名.hbm.xml</strong>，并且文件默认与JavaBean在同一个包下
6. 为新生成的XML映射文件导入约束(hibernate是dtd约束)，打开\lib\hibernate-core-5.0.7.Final.jar/org/hibernate/hibernate-mapping-3.0.dtd，选择第10-13行代码，也就是我们要找的约束，代码如下
```XML
<!DOCTYPE hibernate-mapping PUBLIC
    "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
    "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
```
7. 编写XML配置文件，如果没有出现提示（可能是网络不通），可以配置本地的dtd约束，步骤如下
> 1.  <a href="https://github.com/sivanWu0222/HibernateDoc/tree/master/dtd">下载dtd约束文件</a>
> 2.  打开Eclipse，点击Window->Preferences->XML->XML Catalog->选中User Specified Entries->add
> 3.  Location 位置处点击File System(浏览本地下载好的dtd文件，上面有下载链接),key type中选择URI,Key填写 <strong>http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd</strong>

```XML
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-mapping PUBLIC
    "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
    "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">

<!-- 编写类与表的对应关系 -->    
<hibernate-mapping>
	<class name="com.itheima.domain.Customer" table="cst_customer"><!-- catalog属性用来配置数据库，但是我们在核心配置文件中已经指定了数据库，所以这里不需要写 -->

		<id name="cust_id" column="cust_id">
			<!-- 配置主键生成的策略，native意思就是使用数据库的生成策略 -->
			<generator class="native"></generator>
		</id>

		<!-- 对于name与column相同的属性，可以省略column，此时默认为name -->
		<property name="cust_name" />
		<property name="cust_user_id" />
		<property name="cust_create_id" />
		<property name="cust_source" />
		<property name="cust_industry" />
		<property name="cust_level" />
		<property name="cust_linkman" />
		<property name="cust_phone" />
		<property name="cust_mobile" />
	</class>
</hibernate-mapping>    
```
8. 编写hibernate核心配置文件(不会配置的可以<a href="">参考</a>)，默认文件名为<strong>hibernate.cfg.xml（存放到src目录下）</strong>按照如同上面的方法新建XML文件，并且引入约束
> 配置内容有 5个必须参数  +  可选参数   +   映射配置文件

```XML
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC
	"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
	"http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">

<!-- 编写hibernate核心配置文件 -->
<hibernate-configuration>
	<session-factory>

		<!-- 5个必须的基本参数 -->
		<property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
		<property name="hibernate.connection.url">jdbc:mysql:///hibernate_day01</property>
		<property name="hibernate.connection.username">root</property>
		<property name="hibernate.connection.password">1018222wxw</property>

		<!-- 配置数据库的方言 -->
		<property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>




		<!-- 可选配置 -->
		<!-- 控制台打印SQL语句 -->
		<property name="hibernate.show_sql">true</property>
		<!-- 控制台格式化打印的SQL语句 -->
		<property name="hibernate.format_sql">true</property>
		<!-- 定义表的自动生成策略 -->
		<!-- 先删除原来的表(如果存在)，再创建新表 -->
		<!--  <property name="hibernate.hbm2ddl.auto">create</property>-->
		<!-- 先删除原来的表(如果存在)，再创建新表，插入数据(如果有),用完之后,最后进行删除 -->
		<!-- <property name="hibernate.hbm2ddl.auto">create-drop</property> -->
		<!--
			1.如果原来的表不存在，可以帮我们创建新表，并且插入数据
			2.如果数据库中新增了一列，可以使用update帮助我们在表结构中增加一列（前提是JavaBean已经配置好了新的属性以及set和get方法，并且映射文件也进行了配置）
			3.如果原来的数据库已经存在，则直接添加数据
		 -->
		<!-- <property name="hibernate.hbm2ddl.auto">update</property> --><!-- 开发阶段使用 -->
		<!-- validate:用来对表结构以及对应的映射文件进行验证是否一致，如果不一致，则报错 -->
		<property name="hibernate.hbm2dd.auto">validate</property><!-- 项目上线之后进行使用 -->

		<!-- 编写映射文件 -->
		<mapping resource="com/itheima/domain/Customer.hbm.xml"/>
	</session-factory>
</hibernate-configuration>

```

9. 编写最基本的测试代码：
```Java

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;
import org.junit.Test;

import com.itheima.domain.Customer;


/**
 * 测试保存一个客户
 * @author SiVan
 *
 */
public class Demo1 {

	@Test
	public void testSave() {

		/*1.加载总配置文件:默认加载src目录下的hibernate.cfg.xml文件*/
		Configuration configuration = new Configuration().configure();
    /*
      如果我们在核心文件中没有配置映射文件，那么我们需要手动加载映射文件
      configuration.addResource("com/itheima/domain/Customer.cfg.xml");
    */
    configuration.add("")
		/*2.创建SessionFactory对象，用来生产Session对象*/
		SessionFactory sessionFactory = configuration.buildSessionFactory();
		/*3.创建Session对象(持久化对象)*/
		Session session = sessionFactory.openSession();
		/*4.开启事务*/
		Transaction transaction = session.beginTransaction();
		/*5.编写保存代码*/
		Customer customer = new Customer();
		customer.setCust_name("测试");
		customer.setCust_level("2");
		customer.setCust_phone("110");
		/*保存客户*/
		session.save(customer);
		/*6.提交事务*/
		transaction.commit();
		/*7.释放资源*/
		session.close();
		sessionFactory.close();/*这里只是测试，因此释放sessionFactory*/
	}

}

```

### 注意事项：
> hibernate核心配置文件不仅仅可以使用XML文件，也可以使用property文件，但是使用property文件的时候，我们需要注意，映射配置文件需要我们自己手动加载，并且property文件没有约束，即使写错也没有提示

------
总结：Hibernate虽然核心机制是orm，但是却实现了用面向对象的关系来操作数据库，通过orm技术来实现这一个目的，简化了我们持久化对象的流程，甚至对于那些不会编写SQL语句的Java程序员也是一个巨大的福音！
