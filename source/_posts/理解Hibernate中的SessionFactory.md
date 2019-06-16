title: 理解Hibernate中的SessionFactory与Session
categories:
  - 框架
  - Hibernate
date: 2017-11-13 10:08:27
tags: [Java,Hibernate]
---

## 前言：
 SessionFactory接口负责初始化Hibernate。它充当数据存储源的代理，并负责创建Session对象。这里用到了工厂模式。需要注意的是SessionFactory并不是轻量级的，因为一般情况下，一个项目通常只需要一个SessionFactory就够，当需要操作多个数据库时，可以为每个数据库指定一个SessionFactory。
                    -- 摘自百度百科

------

## SessionFactory内部构造
> sessionFactory 内部分为两个部分

1. 内部机构：用来存放当前数据库的配置文件，以及映射文件和预定义的SQL语句
2. 外部结构：用来存放每个session缓存的内容(session是一级缓存，每次都会将内容同步到二级缓存中，也就是SessionFactory就是二级缓存)

<img src="http://on3w7gc9m.bkt.clouddn.com/QQ%E5%9B%BE%E7%89%8720171113102044.png"></img>

### SessionFactory的线程安全性
  相比Session来说，因为SessionFactory采用了工厂模式，也就是单例，并且使用了synchronized，在每个项目中只有1个，所以SessionFactory是线程安全的，但是Session则是线程不安全的，对于同一个Session，如果执行了多个操作，那么多个线程同时访问同一个Session，将会使得Session中的内容的逻辑异常混乱！
  > 因此创建的Session实例必须在本地存取空上运行，使之总与当前的线程相关。这里就需要用到ThreadLocal,在很多种Session 管理方案中都用到了它.ThreadLocal 是Java中一种较为特殊的线程绑定机制，通过ThreadLocal存取的数据， 总是与当前线程相关， 也就是说，JVM 为每个运行的线程，绑定了私有的本地实例存取空间，从而为多线程环境常出现的并发访问问题提供了一种隔离机制,ThreadLocal并不是线程本地化的实现,而是线程局部变量。也就是说每个使用该变量的线程都必须为该变量提供一个副本,每个线程改变该变量的值仅仅是改变该副本的值,而不会影响其他线程的该变量的值,ThreadLocal是隔离多个线程的数据共享，不存在多个线程之间共享资源,因此不再需要对线程同步


------

## SessionFactory 的特点
1. 由Configuration通过加载配置文件而创建
2. SessionFactory对象中保存了当前的数据库配置信息以及所有映射关系和预定义的SQL语句，同时SessionFactory负责维护Hibernate的二级缓存
3. 一个SessionFactory对应一个数据库，并且应用可以通过SessionFactory来获取Session实例
4. SessionFactory是线程安全的，也就是说可以被多个线程所共享
5. SessionFactory是重量级的，创建以及销毁格外消耗时间
6. SessionFactory需要一个较大的缓存，用来存放预定义的SQL语句及实体的映射信息。另外可以配置一个缓存插件，这个插件被称之为Hibernate的二级缓存，被多线程所共享

一个建议的HibernateUtils类（封装了HibernateSessionFactory）
```Java
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

public class HibernateUtils {

	private  static final Configuration CONFIG;
	private  static final SessionFactory FACTORY;

	static {
		CONFIG = new Configuration().configure();
		FACTORY = CONFIG.buildSessionFactory();
	}

	public Session getSession() {
		return FACTORY.openSession();
	}

}
```

------

## Session常用的方法
1. save() 将数据持久化到数据库中
2. update() 对查出来的数据进行修改，根据id值进行修改
3. get() 根据相应的反射类以及id值进行查询
4. delete() 虽然传递的是一个对象类型的参数，但是实际上是根据id值进行删除
5. saveOrUpdate() 如果数据存在，则进行修改，反之将数据持久化到数据库中
6. clear() 将session中的缓存全部清空
7. evict(Object object) 将缓存中的指定对象从缓存中清除
8. flush() 刷出缓存，与Session快照机制相吻合，如果快照区与缓存区域一致，则进行修改；也就是说与数据库的内容进行同步

> 注意：如果数据库表中的主键采用了自增策略，我们不可以对id值进行设置，如果设置将会报错，因为id值已经交给Hibernate进行管理，无需我们自己进行设值


<!-- more -->

------

## 绑定本地Session


![](http://on3w7gc9m.bkt.clouddn.com/QQ%E5%9B%BE%E7%89%8720171116175231.png)

### ThreadLocal
> 底层是Map集合，键是当前的线程，值（存什么就是什么）
