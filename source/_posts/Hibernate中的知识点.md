---
title: Hibernate中的知识点
tags: [Java,Hibernate]
categories:
  - 框架
  - Hibernate
date: 2017-11-13 16:49:12
---

## 持久化类(Persistent Object 简称PO)
> 持久化类就是一个Java类，当这个类与表建立了映射关系(通过XML配置文件)

PO = POJO(JavaBean) + XML配置文件

### JavaBean规范：
1. 必须有构造方法，如果没有无参的需要自己加上一个无参构造方法
2. 属性必须私有，并且必须提供set以及get方法

### PO编写规则
1. 提供一个无参的public修饰符的构造器（因为底层要通过反射创建对象）
2. 提供一个标识属性，映射表的主键字段（唯一标识ID,也就是所说的OId）
3. 所有属性提供public访问控制符的set以及get方法
4. 标识属性应尽量使用基本类型的包装类

### 持久化类的三种状态
1. 瞬时态
> 刚创建的对象，没有被持久化到数据库（没有持久化表示OId,OId就是数据库中的主键，也就是唯一区分JavaBean对象的属性），没有被纳入Session管理（没有缓存到Session中）



2. 持久态
> 存在于数据库中（有唯一标识OId），并且被Session进行管理（缓存到了Session中）



注意事项：
持久态的对象有更新数据库的能力，示例如下(正常情况下，我们需要执行session.update()方法进行修改，但是这里是由于session的一级缓存，所以持久态对象在不执行session.update()方法的时候依然具有更新数据库的能力)
>

```Java

import org.hibernate.Session;
import org.hibernate.Transaction;
import org.junit.Test;

import com.itheima.domain.User;
import com.itheima.util.HibernateUtils;

public class Demo2 {

	@Test
	public void run() {
		Session session = HibernateUtils.getSession();
		Transaction transaction = session.beginTransaction();

		User user = session.get(User.class, 18);
		user.setName("wxwddd");

		transaction.commit();
		session.close();

	}

}

```


<!-- more -->

3. 脱管态
> 存在于数据库中（有唯一标识OId），没有被Session进行管理（没有被Session进行缓存）

下面代码的注意事项：
> 当我们session执行save方法的时候，其实session已经从连接那里取得了id值，因此当我们执行save方法的时候，user对象将会得到一个session获取到的id，并且缓存到session中，尽管事务没有提交


```Java
import java.io.Serializable;

import org.hibernate.Hibernate;
import org.hibernate.Session;
import org.hibernate.Transaction;
import org.junit.Test;

import com.itheima.domain.User;
import com.itheima.util.HibernateUtils;

public class Demo1 {

	@Test
	public void run() {

		Session session = HibernateUtils.getSession();
		Transaction transaction = session.beginTransaction();
		User user = new User();
		user.setAge(29);
		user.setName("wxw");
		/*
      这里其实已经获取到了id值，因为session已经与数据库连接上了
      当session执行save方法的时候，将会把user对象缓存到session中，尽管这时还没有提交事务
    */
		Serializable s = session.save(user);
    /*此时user已经成为了持久态，既进入了缓存，又得到了OId*/

		System.out.println(s);
		System.out.println(user.getId());
		transaction.commit();
		session.close();

    /*此时user依然存在，但是session不存在，此时变成了脱管态*/

	}
}





````

> 三个状态的转换

![](http://on3w7gc9m.bkt.clouddn.com/01-%E4%B8%89%E4%B8%AA%E7%8A%B6%E6%80%81%E8%BD%AC%E6%8D%A2.bmp)

-------
## 主键

### 自然主键
> 当我们创建表结构的时候，有的时候表结构中就有唯一可以确定某个记录的时候，则该列称为自然主键，但是我们在开发中一般不会采用自然主键，因为自然主键可以能随着规则被打破而受到影响

### 代理主键
> 既然自然主键不可以用来充当主键，所以在表结构定义的时候，我们一般加上一列用来唯一区分每一条记录，称为代理主键！

### 主键的生成策略
1. increment 适合于short int long 作为主键，不是使用数据库的自动增长机制，一般不建议使用
> 先找数据库中id的最大值，然后给最大值+1，作为下一条记录的主键，Hibernate执行代码如下

```SQL
Hibernate:
    select
        max(id)
    from
        User
Hibernate:
    insert
    into
        User
        (name, age, id)
    values
        (?, ?, ?)

```

使用increment在并发访问的情况下，由于多个人并发查询最大值的时候极有可能相同，因此在插入的时候将会产生问题！

2. identity 适用于short int long作为主键，并且使用该生成策略的时候，要求数据库的主键必须可以自动增长！因为该策略采用了数据库底层的自动增长机制！所以对于oracle这种不可以自动增长的数据库，是不可以采用这种策略的！
> 不经常使用，因为如果中途更换数据库，这个策略将不会适用于oracle数据库

3. sequence 适用于short int long作为主键，底层采用的是序列增长的方式
> 只支持oracle数据库，也支持DB2数据库，因为oracle自动增长使用的是序列进行自动增长,不支持MySQL数据库

4. uuid 适用于char varchar 数据类型，使用随机的字符串作为主键

5. native 本地策略，根据底层数据库的不同，自动选择适用于该种数据库的生成策略 (适用于 short int long 数据类型)
> 如果本地使用MySQL数据库，将会使用identity，如果使用oracle，将会使用sequence

6. assigned 主键自己进行维护，不使用Hibernate框架进行维护

------

## Hibernate中的缓存

### Session级别的缓存(一级缓存)
> Hibernate内部采用了缓存机制，就是说将数据存放到一块单独的内存空间中，以便增强我们操控数据的效率

1. 一级缓存是Session级别的，不可以卸载的，一级缓存的生命周期与session一致
2. session内部存放了很多集合，用来存放缓存的数据

#### Session中的快照机制
> 正是因为Session中快照机制的存在，所以我们的持久化对象才有操纵数据库的能力
> Session中可以划分为两个区域，一个缓存区域，一个快照区域

当我们执行查询方法的时候，数据将会缓存到Hibernate中的缓存区，和快照区域，但是当我们修改数据的时候，缓存区域的数据进行修改，快照区域的数据不会进行修改，但是当我们使用事务进行提交的时候，事务会自动判别快照区域与内存区域的数据，并对其进行比较，如果两者一致，则不进行修改，如果两者不一致，则进行修改！

### SessionFactory级别的缓存(二级缓存)
> 由于Session生命周期的短暂，所以Hibernate也提供了二级缓存，二级缓存默认没有开启，需要手动配置才可以使用，二级缓存可以在多个session中共享数据，有点类似于我们进程与线程

-------

## Hibernate中的事务

### 事务的4个特性：
1. 原子性（atomicity）：一个事务是一个不可分割的工作单位，事务中包括的诸操作要么都做，要么都不做。
2. 隔离性（isolation）：一个事务的执行不能被其他事务干扰。即一个事务内部的操作及使用的数据对并发的其他事务是隔离的，并发执行的各个事务之间不能互相干扰。
3. 一致性（consistency）：事务必须是使数据库从一个一致性状态变到另一个一致性状态。一致性与原子性是密切相关的。
4. 持久性（durability）：持久性也称永久性（permanence），指一个事务一旦提交，它对数据库中数据的改变就应该是永久性的。接下来的其他操作或故障不应该对其有任何影响。

### 并发事务引发的问题（隔离性所引发的问题）
1. 脏读：一个事务读取了另一个事务未提交的数据
2. 不可重复读：一个事务读到了另一个事务更新之后的数据，导致多次查询结果不一致
3. 虚读（幻读）：一个事务读到了另一个事务新增之后的数据，导致多次查询结果不一致
> 不可重复读与虚读违背了事务的一致性，多个事务读取同一个数据，读取到的数据应该是一致的，也就是事务的一致性！

### 避免并发事务引发的问题
> 通过设置隔离级别来避免事务并发所引起的问题

1. read-uncommited 读未提交 脏读，不可重复读，虚读都有可能发生
2. read-commited 读已提交 避免了脏读，但是不可重复读，虚读都有可能发生
3. repeatable read 重复读  不可重复读，但是虚读有可能发生
4. Serializable 串行化 以上情况都可以避免，但是效率低，有点类似于线程的同步

MySQL 默认隔离级别为 repeatable read ,可以避免脏读和不可以重复读，而幻读则发生几率很低，忽略不计
oracle 默认隔离级别为 read-commited

### Hibernate核心配置文件配置事务隔离级别
>通过设置hibernate.connection.isolation

1. read-uncommited   值为1
2. read-commited 值为2
3. repeatable read 值为4
4. serializable 值为8


----

## 丢失更新问题
> 上面讲的脏读，不可重复读，幻读，都是针对Hibernate中读取数据因数据库不同隔离级别而产生的问题，而丢失更新则是对数据库进行修改所产生的问题

### 丢失更新问题的产生
> 丢失更新问题的产生很简单，就是当我们访问数据库的时候，两个人分别获取到了同一条数据，并且内容一致，但是一个人修改了自己读取到的数据的一个字段，另一个人修改了自己读取到的数据的另一个字段，结果可想而知，也就是修改了一个字段，并没有两个字段都进行修改，也就产生了丢失更新的问题

如图所示：
![](http://on3w7gc9m.bkt.clouddn.com/QQ%E5%9B%BE%E7%89%8720171116161731.png)

### 解决丢失更新问题
1. 悲观锁（不常用），采用数据库的锁机制进行操作，对于我们每次操作，数据库都会在sql语句后面加上for update，操作完成之后，再把锁进行释放，效率很低
2. 乐观锁：JavaBean属性添加一个新的属性(version)
> 数据库首先查询数据，然后进行修改，提交的时候，将会比较版本号是否与查出的版本号一致，如果不一致，将会报错，否则根据版本号与主键值进行修改


#### 悲观锁的使用
```Java
session.get(Customer.class, 1, LockMode.UPGRADE);
```

#### 乐观锁的使用
1. JavaBean中添加version属性，并且设置相应的set以及get方法
2. 在映射文件中 添加 如下代码
> <version name="version" />

-----
