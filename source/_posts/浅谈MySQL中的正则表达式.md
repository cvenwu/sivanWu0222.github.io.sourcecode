---
title: 浅谈MySQL中的正则表达式
date: 2017-05-14 21:26:19
tags: [SQL,MySQL]
categories:
- MySQL
- 正则表达式
---


> 记得之前自己不熟悉正则表达式的时候，别人问到这个问题，自己一脸懵逼，现在尽管自己对正则表达式有所了解，但还是不够游刃有余，其实自己最感慨
正则表达式的强大之处，可能一个正则表达式可以解决我们一个很复杂的问题，例如如何匹配一个身份证号码，如果没有正则表达式，我们可能很难对数据进行有效的过滤。


# MySQL中的正则表达式
> MySQL中的正则表达式仅支持多数正则表达式实现的一个很小的子集

## LIKE 与 REGEXP 的区别

下面这段SQL代码将匹配 prod_name 为 1000 的记录
```SQL
SELECT prod_name
FROM products
WHERE prod_name LIKE '1000'
ORDER BY prod_name;
```

下面这段SQL代码将匹配 prod_name 中 含有 1000 的记录
```SQL
SELECT prod_name
FROM products
WHERE prod_name REGEXP '1000'
ORDER BY prod_name;
```

1. LIKE 匹配整个列，如果被匹配的文本在列值中出现(而不是列值)，LIKE子句将不会找到
2. REGEXP 则在列值内进行匹配，如果被匹配的文本在列值中出现，REGEXP将会找到
<!-- more -->
> MySQL中的正则表达式自版本（3.23.4后）不区分大小写，如果要去区分大小写，可以使用 BINARY 关键字
```SQL
SELECT userinfo.U, userinfo.P
FROM userinfo
WHERE userinfo.U REGEXP BINARY 'a1';			
-- 正则表达式 会超照 字段U 中包含 a1 的值，这里加了个BINARY 只会找到包含 a1的，不会找到包含A1的
```

----------

## 匹配给定字符的一个
```SQL
SELECT prod_name
FROM products
WHERE prod_name REGEXP '1000|2000'
ORDER BY prod_name;
```
上面这段代码将会匹配 prod_name 中 包含1000 或 2000 的记录


```SQL
SELECT prod_name
FROM products
WHERE prod_name REGEXP '1|2|3 Ton'			
ORDER BY prod_name;
```
上面一段SQL代码意思是找到1 或 2 或 3 Ton的，而不是 1 TON 或 2 Ton 或 3 Ton的

<font color = "red">注意 ： 除非把 | 括在一个集合中，否则它将应用于整个串</font>


----------

## 匹配MySQL中的特殊字符
> 如果我们需要对MySQL中的特殊字符使用正则表达式，我们需要格外注意

例如：采用如下代码匹配 prod_name 中包含 . 号的
```SQL
SELECT prod_name
FROM products
WHERE prod_name REGEXP '.'							
ORDER BY prod_name;
```
其实是错误的，上面这段代码其实是列出表中的所有记录，而并不是我们想要的 包含 . 号的记录，因为 . 号在正则表达式中代表任意字符，所以会列出表中所有记录
> 对于MySQL中的特殊符号，如果我们要采用正则表达式去匹配，需要在特殊符号前面加上 \\

下面的代码就是匹配 prod_name 中包含 . 号的
```SQL
SELECT prod_name
FROM products
WHERE prod_name REGEXP '\\.'							
ORDER BY prod_name;
```

> 注意 ：
1. 为了匹配特殊字符，必须用 \\ 为前导， \\- 表示查找 -  
2. 为了匹配 \ 本身，需要使用 \\\
3. <font color = "red">对于其他语言的正则表达式，如果需要转义特殊字符，我们可能会使用 单个 \ ，但是MySQL要求我们使用两个 \ （MySQL自己解释一个，正则表达式库解释另一个）</font>

----------

## 匹配MySQL中的字符类

| 类    | 说明     |
| :------------- | :------------- |
| [:alnum:]       | 任意字母和数字（同[a-zA-Z0-9]）       |
| [:alpha:]       | 任意字符(同[a-zA-Z])      |
| [:blank:]      | 空格和制表(同[\\t])       |
| [:cntrl:]       |      ASCII控制字符(ASCII 0到31和127)  |
| [:digit:]       | 任意数字(同[0-9])       |
| [:graph:]       | 与[:print:]相同，但不包含空格       |
| [:lower:]       | 任意小写字母(同[a-z])       |
| [:print:]       | 任意可打印字符       |
| [:punct:]       | 既不在[:alnum:]又不在[:cntrl:]中的任意字符       |
| [:space:]       | 包含空格在内的任意空白字符(同[\\f\\n\\r\\t\\v])       |
| [:upper:]       | 任意大写字母(同[A-Z])       |
| [:xdigit:]       | 任意十六进制数字(同[a-fA-F0-9])       |



```SQL
SELECT prod_name
FROM products
WHERE prod_name REGEXP '[[:digit:]\\.]'    -- 或者写成 ^[0-9\\.]
ORDER BY prod_name;
```
找出prod_name 中 数字（包括小数点） 开始的所有产品

----------
## 匹配多个实例
| 元字符    | 说明     |
| :------------- | :------------- |
| *       | 0个或多个匹配       |
| +       | 1个或多个匹配       |
| ?       | 0个或1个匹配       |
| {n}       | 指定数目的匹配       |
| {n,}      | 不少于指定数目的匹配       |
| {n,m}     | 匹配数目的范围       |

```SQL
SELECT prod_name
FROM products
WHERE prod_name REGEXP '\\([0-9] sticks?\\)'		
ORDER BY prod_name;
-- 用来匹配文本 ([0-9] stick(这里sticks也可以，因为是s?,表示s出现0次或1次))
```

----------
## 定位符

| 元字符 |  说明     |
| :------------- | :------------- |
| ^       | 文本的开始       |
| $       | 文本的结尾       |
| [[:<:]]       | 词的开始       |
| [[:>:]]       | 文本的结尾       |
> ^ 有双重用途：可以用来匹配文本开始，也可以放在集合中，例如[^123]，表示不匹配含有 1或2或3的

对于MySQL 我们可以在不使用数据库表的情况下用SELECT 来测试正则表达式，REGEXP 检查总是返回 0(没找到匹配)或1(找到匹配)

```SQL
SELECT 'hello' REGEXP '[0-9]';
```
