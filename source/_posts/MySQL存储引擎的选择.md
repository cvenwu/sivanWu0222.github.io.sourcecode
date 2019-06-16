---

title: MySQL存储引擎的选择
categories:
  - MySQL
  - SQL
date: 2017-09-20 22:55:35
tags: [数据库,MySQL]

---

## 了解存储引擎
> 存储引擎其实就是数据在数据库中的存放方式，通过什么样的方式存储数据效率更高，对于应用的优化有着举足轻重的作用，尤其是对于我们这样海量数据的时代，能够高效的处理海量数据就已经很不易了！

MySQL5.0 支持如下存储引擎：
1. MyISAM
2. InnoDB
3. BDB
4. MEMORY
5. MERGE
6. EXAMPLE
7. NDB Cluster
8. ARCHIVE
9. CSV
10. BLACKHOLE
11. FEDERATED
> 如此多的存储引擎，却只有InnoDB和BDB提供事务安全表，其他存储引擎都是非事务安全表

### 选择存储引擎
> 当我们在创建表的时候没有指定存储引擎，系统将会使用默认存储引擎，MySQL5.5(包含5.5)之前默认存储引擎为MyISAM,之后便改为了InnoDB,如果要修改存储引擎，可以在创建表的SQL语句中进行设置。


查看当前数据库支持的存储引擎，可以使用如下命令
```SQL
SHOW ENGINES \G
```
或者
```SQL
SHOW VARIABLES LIKE 'have%';
```
![](http://on3w7gc9m.bkt.clouddn.com/QQ%E5%9B%BE%E7%89%8720170913144228.png)
> 图片中显示 value 显示为 "DISABLED" 的记录表示支持该存储引擎，但是数据库启动的时候被禁用

#### 创建表的时候指定存储引擎
> 创建表可以指定存储引擎，例如下面的SQL语句

```SQL
CREATE TABLE ai (
  i BIGINT(20) NOT NULL AUTO_INCREMENT,
  PRIMARY KEY(i)
) ENGINE = MyISAM DEFAULT CHARSET = UTF8;
```
![](http://on3w7gc9m.bkt.clouddn.com/QQ%E5%9B%BE%E7%89%8720170913145307.png)


#### 修改存储引擎
> 也可以使用ALTER TABLE 语句进行存储引擎的修改，将一个已经存在的表修改成其他存储引擎
```SQL
ALTER TABLE ai ENGINE = InnoDB;
```
<!-- more -->

> 结果可以使用
```SQL
SHOW CREATE TABLE ai \G
```
进行查看

<hr />

## 各种存储引擎的特性
![](http://on3w7gc9m.bkt.clouddn.com/QQ%E5%9B%BE%E7%89%8720170920232002.jpg)


### MyISAM
> MyISAM作为MySQL 5.5 之前默认的存储引擎。不支持事务也不支持外键

优势： 访问速度快，对事务完整性没有要求或者以SELECT, INSERT 为主的应用基本上可以使用该引擎来创建表

> 每个MyISAM 在磁盘上存储成3个文件，其文件名和表名相同，但扩展名分别是：
  1. .frm(存储表定义)
  2. .MYD(MYData,存储数据)
  3. .MYI(MYIndex,存储索引)
  <strong> 数据文件和索引文件可以放置在不同目录，平均分布IO，获得更快的速度 </strong>

如果需要指定索引文件和数据文件路径，需要在创建表的时候通过DATA DIRECTORY 和 INDEX DIRECTORY 语句指定，也就是说不同MyISAM 表的索引文件和数据文件可以放置到不同路径下。文件路径需要绝对路径，并且需要访问权限


### InnoDB
> InooDB 存储引擎提供了具有提交，回滚和崩溃恢复能力的事务安全，但是对比MyISAM的存储引擎，InnoDB写的处理效率差一些，并且会占用更多的磁盘空间已保留数据和索引

#### 特点
1. 自动增长列
> InnoDB表的增长列可以手工插入，但是插入的值如果是空或者0，则实际插入的值将是自动增长后的值

2. 外键约束

3. 存储方式

> 1. 使用共享空间存储，这种方式创建的表结构保存在.frm文件中，数据和索引保存在innodb_data_home_dir和innodb_data_file_path定义的表空间，可以是多个文件
> 2. 使用多表空间存储，这种方式创建的表结构依然保存在.frm文件中，但是每个表的数据和索引单独保存在.ibd中。如果是个分区表，则每个分区对应单独的.ibd文件，文件名是“表名 + 分区名”,可以在创建分区的时候指定每个分区数据文件的位置，以此来将表的IO均匀分布在多个磁盘上。





### MEMORY
> MEMPRY存储引擎使用存在于内存中的内容来创建表，每个MEMORY表只实际对应一个磁盘文件，格式是.frm。MEMORY类型的表访问也非常快，因为它的数据位于内存中，并且默认使用HASH索引，但是一旦服务关闭，表中的数据将会丢失。

创建索引的时候可以指定是HASH索引还是BTREE索引
```SQL
CREATE INDEX index_name USING HASH ON table_name(column_name);
```

> 在启动MySQL服务的时候，使用--inint-file选项，把INSERT INTO ... SELECT 或 LOAD DATA INFILE 这样的语句放入到该文件中，就可以在服务启动的时候从持久稳固的数据源装载表

当我们对MEMORY存储引擎的表操纵完成后，应该释放内存，执行DELETE FROM 或 TRUNCATE TABLE 或者 DROP TABLE;

每个MEMORY 表中可以放置的数据量大小，受到max_head_table_size系统变量的约束，这个系统变量初始值为16MB，可以根据需要加大。此外，在定义MEMORY表的时候，可以通过MAX_ROWS子句指定表的最大行数。

MEMORY 类型的存储引擎主要用于内容变化不太频繁，或者作为统计操作的中间结果表，便于高效的对中间结果进行分析并得到最终的统计结果。对存储引擎为MEMORY的表进行更新操作要谨慎，因为数据并没有实际写入到磁盘中，所以如果由于异常或者重启等引起的将会导致数据丢失。


### MERGE
> MERGE 存储引擎是一组MyISAM表的组合，这些MyISAM表必须结构完全相同，MERGE表本身并没有数据，对MERGE类型的表进行查询，更新和删除操作，实际上是对内部MyISAM表进行的。对于MERGE类型表的插入操作，是通过INSERT_METHOD子句定义插入的表，可以有3个不同的值，使用FIRST或LAST值使得插入操作被相应地作用在第一或者最后一个表上，不定义这个子句或者定义为NO，表示不能对这个MERGE表执行插入操作。
> 可以对MERGE表进行DROP操作，仅仅只是删除MERGE，并不会对表的结构有太大影响
> MERGE表在磁盘上保留两个文件，文件名以表的名字开始，一个.frm文件存储表定义，另一个.MYG文件包含组合表信息，包含MERGE表由哪些表组成，插入新的数据时的依据。可以通过修改.MRG文件来修改MERGE表，但是修改后要通过FLUSH TABLES刷新。

MERGE表和分区表的区别：MERGE表并不能智能地将记录写到对应的表中，而分区表则可以。




### ToKuDB
> ToKuDB是第三方存储引擎，并不是MySQL自带的存储引擎，除此之外，还有列式存储引擎Infobright,高写性能高压缩ToKuDB(也就是这里所讲到的)

一个高性能，支持事务处理的MySQL 和 MariaDB 存储引擎，具有高扩展性，高压缩率，高效的写入性能，支持大多数在线DDL操作

#### 特性
1. 使用Fractal 树保证高效的插入性能
2. 优秀的压缩性，比InnoDB 高近10倍
3. Hot Schema Changes 特性支持在线创建索引和添加，删除属性列等DDL操作
4. 使用Bulk Loader达到快速加载大量数据
5. 提供了主从延迟消除技术
6. 支持ACID 和 MVCC

#### 使用场景
1. 日志数据，因为日志插入频繁，且存量大
2. 历史数据，通常不会有写操作，可以利用ToKuDB的高压缩特性进行存储
3. 在线DDL较频繁的场景，可以使用ToKuDB可以大大增加系统可用性

<hr />

## 选择合适的存储引擎

### MyISAM
> MySQL 5.5 之前的默认存储引擎，如果应用以读操作和插入操作为主，只有很少的更新和删除操作，并且对事务的完整性，并发性要求不是很高，选择MyISAM引擎将会非常合适，MyISAM是在web,数据仓储，和其他应用环境下最常使用的存储引擎之一
### InnoDB
> 用于事务处理应用程序，支持外键，如果应用对事务的完整性有比较高的要求，在并发条件下要求数据的一致性，数据操作除了插入和查询以外，还包括很多的更新、删除操作，那么InnoDB存储引擎应该是比较合适的选择。InnoDB存储引擎除了有效地降低由于删除和更新导致的锁定，还可以确保事务的完整提交和回滚，对于类似计费系统或者财务系统等对数据准确性要求较高的系统，InnoD都是较好的选择
### MEMORY
> 将所有数据保存在RAM中，在需要快速定位记录和其他类似数据的环境下，可以提供极快的访问，MEMORY的缺陷是对表的大小有限制，太大的表无法缓存在内存中，其次是要确保表的数据可以恢复，数据库异常终止后表中数据可以恢复。MEMORY表通常用于更新不太频繁的小表，用于快速访问得到结果

### MERGE
<strong>用于将一系列等同的MyISAM表以逻辑方式组合在一起，并作为一个对象引用它们，MERGE表的优点在于可以突破对单个MyISAM表大小的限制,并且通过将不同的表分布在多个磁盘上</strong>，可以有效的改善MERGE表的访问效率，这对于数据仓储等VLDB环境非常适合！
