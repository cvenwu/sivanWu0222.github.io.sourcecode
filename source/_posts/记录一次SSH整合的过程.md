---
title: 记录一次SSH整合的过程
date: 2017-10-21 19:11:32
tags: [Java,SSH]
categories:
- 框架
- 整合
---

> 最近做的这个项目采用了SSH框架，由于时间隔了很久，框架搭建忘记了很多，所以以此记录自己现在搭建整个框架的流程，留作参考！

<strong>本次搭建采用了 MyEclipse 2017 IDE + Hibernate 3.3 + Spring 3.0 作为案例</strong>

## 搭建Hibernate框架
> 这次搭建框架的首要步骤便是搭建Hibernate框架


### 步骤
1. 新建一个Web Project,并点击生成web.xml，点击Finish
2. 对要搭建框架的项目进行右击，选中Configure Facets... --> 点击Install Hibernate Facet
3. 选择版本号(3.3) --> 进行hibernate.cfg.xml的创建 --> 进行 Hibernate 数据库连接配置 --> 选择Hibernate导入jar包
4. 修改hibernate.cfg.xml配置文件：创建一个 show_sql ，值为 true

<img src="http://on3w7gc9m.bkt.clouddn.com/1.png" />
<img src="http://on3w7gc9m.bkt.clouddn.com/2.png" />

<!-- more -->

<img src="http://on3w7gc9m.bkt.clouddn.com/3.png" />
<img src="http://on3w7gc9m.bkt.clouddn.com/4.png" />

-----

## 搭建Spring框架

### 步骤
1. 对要搭建框架的项目进行右击，选中Configure Facets... --> 点击Install Hibernate Facet
2. 选择Spring版本号 --> 添加Spring配置文件 --> 添加Spring 开发jar包

<img src="http://on3w7gc9m.bkt.clouddn.com/5.png" />
<img src="http://on3w7gc9m.bkt.clouddn.com/6.png" />
<img src="http://on3w7gc9m.bkt.clouddn.com/7.png" />

3. 进行逆向工程，生成对应的 POJO 以及 映射文件 和 DAO

4. src目录下 新建 com.dao 文件夹，将生成的DAO文件全部移动到com.dao中


<img src="http://on3w7gc9m.bkt.clouddn.com/8.png" />
<img src="http://on3w7gc9m.bkt.clouddn.com/9.png" />
------


## 配置 web.xml
1. 在Context Parameters 栏目中，添加如下值：
```XML
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:app*.xml</param-value>
</context-param>
```

2. 在Listeners栏目下新建一个ListenerClass
```XML
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```
3. 配置springMVC中的 servlet，配置如下，(于旁边 JSP File 栏目报错，可以随便填入几个字符，再删除并保存就可以避免错误！)
```XML
<servlet>
    <servlet-name>springMVC</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>springMVC</servlet-name>
    <url-pattern>*.do</url-pattern>
</servlet-mapping>
```


------

## 配置applicationContext的XML配置文件
1. 更换xml文件头，改为如下：
```XML
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:util="http://www.springframework.org/schema/util"
	xmlns:p="http://www.springframework.org/schema/p" xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:tx="http://www.springframework.org/schema/tx" xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="
	http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
	http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-3.0.xsd
	http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-3.0.xsd
	http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-3.0.xsd
	http://www.springframework.org/schema/context
  http://www.springframework.org/schema/context/spring-context-3.0.xsd">

```

2. 配置 scan 扫描
```XML
<context:component-scan base-package="com.service"></context:component-scan>
```

3. 在com.dao下面新建HqlDAO类
```Java
public class HqlDAO extends HibernateDaoSupport{
	public List query(String hql,Object ...p)
	{
		return super.getHibernateTemplate().find(hql,p);
	}
	public List findByHQL(final String hql,final Object ...p)
	{
		List list=getHibernateTemplate().executeFind(new HibernateCallback() {

			public Object doInHibernate(Session session) throws HibernateException,
					SQLException {
				/* TODO Auto-generated method stub */
				Query query=session.createQuery(hql);
				for (int i = 0; i < p.length; i++) {
					query.setParameter(i, p[i]);
				}
				return query.list();
			}

		});
		return list;
	}
	public int zsg(String hql, Object ...p)
	{
		return super.getHibernateTemplate().bulkUpdate(hql,p);
	}
	public Session getHibernateSession()
	{
		Session session=super.getHibernateTemplate().execute(new HibernateCallback<Session>() {

			public Session doInHibernate(Session s)
					throws HibernateException, SQLException {
				/* TODO Auto-generated method stub */
				return s;
			}
		});
		return session;
	}
	public List pageQuery(String hql, int size,int page , Object ...p)
	{
		try {
			Session session=getHibernateSession();
			Query query=session.createQuery(hql);
			if(p!=null)
			{
				for (int i = 0; i < p.length; i++) {
					query.setParameter(i, p[i]);
				}
			}
			query.setFirstResult((page-1)*size).setMaxResults(size);
			List list=query.list();
			return list;
		} catch (HibernateException e) {
			/* TODO Auto-generated catch block */
			e.printStackTrace();
		}
		return new ArrayList();
	}
	public void bulkUpdate(String hql, Object... p) {
		getHibernateTemplate().bulkUpdate(hql, p);
	}
	 public float unique(final String hql ,final Object...p) {

		 List list = query(hql, p);

			if (list.size()>0)
			{
				Object obj = list.get(0);
				if (obj!=null){
					return Float.parseFloat(obj.toString());
				}
			}
			return 0;
	 }
	 public List sqlPageCreateQuery(String sql, int page, int size, Object... p) {
			Session session = getHibernateSession();
			Query query = session.createSQLQuery(sql);
			if (p != null) {
				for (int i = 0; i < p.length; i++) {
					query.setParameter(i, p[i]);
				}
			}
			query.setFirstResult((page - 1) * size).setMaxResults(size);
			List list = query.list();
			return list;
		}

		public List sqlCreateQuery(String sql, Object... p) {
			Session session = getHibernateSession();
			Query query = session.createSQLQuery(sql);
			if (p != null) {
				for (int i = 0; i < p.length; i++) {
					query.setParameter(i, p[i]);
				}
			}
			List list = query.list();
			return list;
		}
}
```

4. 配置hqlDAO
```XML
<bean id="hqlDAO" class="com.dao.HqlDAO">
	  <property name="sessionFactory" ref="sessionFactory"></property>
</bean>
```  

5. 配置事务
```XML
<bean id="transactionManager" class="org.springframework.orm.hibernate3.HibernateTransactionManager">
    	<property name="sessionFactory" ref="sessionFactory"></property>
</bean>

<tx:advice id="mytx" transaction-manager="transactionManager">
	<tx:attributes>
		<tx:method name="*" propagation="REQUIRED"/>
	</tx:attributes>
</tx:advice>

<aop:config>
	<aop:advisor advice-ref="mytx" pointcut="execution(* com.service.*.*(..))"/>
</aop:config>
```
6. WEB-INF文件夹下 新建 springMVC-servlet.xml ：用来处理中文
```XML
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:util="http://www.springframework.org/schema/util"
	xmlns:p="http://www.springframework.org/schema/p" xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:tx="http://www.springframework.org/schema/tx" xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="
	http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
	http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-3.0.xsd
	http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-3.0.xsd
	http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-3.0.xsd
	http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context-3.0.xsd">

<context:component-scan base-package="com.action"></context:component-scan>
<!-- 中文处理 要在context下面-->
	<bean
		class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter">
		<property name="messageConverters">
			<list>
				<bean
      class="org.springframework.http.converter.StringHttpMessageConverter">
					<property name="supportedMediaTypes">
						<list>
							<bean class="org.springframework.http.MediaType">
								<constructor-arg index="0" value="text" />
								<constructor-arg index="1" value="plain" />
								<constructor-arg index="2" value="UTF-8" />
							</bean>
						</list>
					</property>
				</bean>
			</list>
		</property>
	</bean>
</beans>
```

7. src下创建文件：log4j.properties（内容如下）：
```properties
#to console#
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss}  %m%n
#to file#
log4j.appender.file=org.apache.log4j.FileAppender
log4j.appender.file.File=sunjob.log
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss}  %l  %m%n
#error/warn/info/debug#
log4j.rootLogger=info, stdout, file
```
------

## 测试：
> 将搭建好的项目部署到tomcat服务器，并启动，如果服务器没有报错，并且页面可以访问，说明搭建成功

---------------
总结：这次搭建SSH框架非常流畅，虽然之前搭建过，但是忘记了很多，因此这次的搭建确实刻骨铭心！
