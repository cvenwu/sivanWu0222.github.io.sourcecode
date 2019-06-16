---
title: 浅谈SQL中的数据过滤
date: 2017-05-12 10:19:32
tags: [Java,Algorithm]
categories:
- Algorithm
- 策略
---

# SQL中的WHERE子句
> 对于数据库中的表，我们经常使用SQL语句进行操作，对于数据量爆炸的今天，倘若需要对用户数据进行验证，如果不加过滤，去索引表中的所有行，
无疑拉低了用户的体验！如今，我们经常使用WHERE子句对数据进行过滤，以便得到我们所需要的数据


## 不采用过滤的缺点
1. 倘若我们不使用WHERE子句对数据进行过滤，将会检索大量无用的数据，并且增加服务器的负载
2. 我们通常可以从服务器端获得我们所需要的数据，如果将全部数据(未过滤)传给用户，并让用户进行过滤，不仅仅大大的增加了网络的带宽，还给用户
的体验带来了极大的不方便之处



### 值得自己思考的问题：
1. 如果数据在用户层过滤，增加了用户的不方便和网络带宽
2. 如果数据在服务器上进行过滤，面对的用户很多，数据量又很大，服务器如果全部处理数据，难免会崩掉！！！
综上所述，在用户层和服务器层过滤数据各有好坏，那么现在的数据过滤如何解决，值得自己探索和深思！！！

<!--more-->

----------

# 数据过滤

## BETWEEN AND 子句

> 开始学习使用 BETWEEN AND 关键字：如果在BETWEEN AND 关键字中使用字符串，我们需要注意，
BETWEEN "A1" AND "b1" 意思是，从 A1(对于那些前面为A1(报错A1本身)后面有其他的将会被选择到，例如A11) 开始
匹配到 b1(对于那些前面为b1的不会被选择到(不包括b1))

```SQL
SELECT USER.id, USER.U, USER.P
FROM USER
WHERE USER.U BETWEEN "A1" AND "b1";
```
----------

## IN操作符
> 其实SQL语句中对数据进行过滤除了可以使用已有的AND,OR运算符，其实IN操作符可以替换OR操作符

IN操作符用来指定条件范围，范围中的每个条件都可以进行匹配，IN取合法值由逗号分隔的清单，全都括在圆括号中

```SQL
SELECT USER.id, USER.U, USER.P
FROM USER
WHERE USER.id  IN (5, 6, 7)
```

上面这句SQL语句等同于下面这句
```SQL
SELECT USER.id, USER.U, USER.P
FROM USER
WHERE USER.id  = 5 or USER.id  = 6 or USER.id  = 7;
```

### 使用IN操作符的好处
1. 在使用长的合法选项清单时， IN操作符的语法更清楚且更直观
2. 在使用IN时，计算的次序更容易管理（因为使用的操作符更少）
3. <font color="red">IN操作符一般比OR操作符执行更快</font>
4. IN的最大有点是可以包含SELECT语句，使得能够更动态的创立WHERE子句！

----------


## 采用 NOT 操作符
>WHERE子句中的NOT操作符有且只有一个功能，那就是否定它之后所有跟的任何条件

- MySQL中的NOT：MySQL支持使用NOT对IN,BETWEEN 和 EXISTS子句取反
- 在与IN操作符联合使用的时候，NOT使找出与条件列表不匹配的行非常简单

----------

## 采用 LIKE 操作符

1. % 操作符（任何字符出现任意次数），如果写成 '%HET', 是会区分大小写的，要注意的是 % 也可以匹配0个字符

```SQL
SELECT userinfo.U, userinfo.P
FROM userinfo
WHERE userinfo.U LIKE '%A1%';		 -- 也会找到字段U值为A1的
```

```SQL
SELECT userinfo.U, userinfo.P
FROM userinfo
WHERE userinfo.P LIKE '%'				-- 这里将不会匹配 USERINFO.P 值为null的，尽管使用了通配符
```

2. _ 操作符  只能匹配一个字符

### 通配符的使用技巧：
1. 不要过度使用通配符，如果其他操作符能达到相同的目的，应该使用其他操作符
2. 在确实需要使用通配符时，除非绝对有必要，否则不要把他们用在搜索模式的开始处，把通配符至于搜索模式的开始处，搜索起来是最慢的
3. 注意通配符的位置，如果放错地方，可能不会返回想要的数据

----------


## 采用 DISTINCT 子句
> 对于重复的数据，如果我们想返回唯一不同的值，可以使用DISTINCT关键字来进行检索

### 注意事项(不能部分使用 DISTINCT 关键字)：
使用DISTINCT关键字必须放在列名开头，请注意DISTINCT关键字应用于所有列，而不仅是前置它的列，下面这句代码的意思是:对于要检索的字段，除非每行所对应记录的字段都相同，否则都会被检索出来，也就是如果要检索的字段（两行完全相同，将只有一行输出，如果部分相同将全部输出）
-- 不能部分使用DISTINCT关键字

```SQL
SELECT  DISTINCT   U,  P
FROM USER
```
----------


## 空值检查
> 创建表的时候，我们将会指定其中的列是否可以包含值，如果一个列不包含值，称其为包含空值NULL

NULL : 无值，它与字段包含0，空字符串或仅仅包含空格不同

检索USER 表的 P字段是否为空
```SQL
SELECT USER.id, USER.U, USER.P
FROM USER
WHERE USER.P IS NULL;
```

### NULL与不匹配
> 在我们通过过滤选择出不具有特定值的行的时候，我们可能希望返回具有NULL值的行，但是，不行，因为未知具有特殊的含义，数据库不知道他们是否匹配，所以在匹配过滤或不不匹配过滤的时候将不会返回

```SQL
SELECT USER.ID, USER.U, USER.P
FROM USER
WHERE USER.P != '5'  # 这里将不会返回P字段为NULL值的行
```

<font color = "red">过滤数据的时候，我们一定要验证返回数据中确实给出了被过滤列具有NULL的行</font>，将SQL语句改为如下
```SQL
SELECT USER.ID, USER.U, USER.P
FROM USER
WHERE USER.P != '5' OR USER.P IS NULL # 这里将会返回P字段为NULL值的行
```
