# MySQL基本操作

###1.建立MySQL数据库

安装与连接：https://www.bilibili.com/video/BV18i4y187i3?p=3

Mysql目录：`C:\Program Files\MySQL\MySQL Server 5.7`

​	bin文件用来放命令
​	include包含头文件
​	lib引用库
​	share字符集编码
​	这里少了一个data数据文件夹，默认不创建，以管理员身份打开cmd，输入以下指令即可创建：

```
cd C:\'Program Files'\MySQL\'MySQL Server 5.7'
mysqld --initialize-insecure --user=root
```

###2.数据库规范

【1】设计原则：（**三大范式**）---核心：消除冗余，保证数据表的完整性，根据实际需求可以违反

- 第一范式：确保每一个字段的数据不能再分了：确保每个字段的原子性
- 第二范式：一个表只描述一件事情，不能扯淡
- 第三范式：消除依赖
  - 例如成绩表，一般情况下不把总分列出来，只需要将成绩表中语数外相加就可以
  - 而高考成绩需要把总分数据列出来，根据具体项目需求确定是否消除传递依赖

【2】细节规范：

- 表名：
  - 和应用名保持一致
  - 不用关键词做表名
  - 表名无复数
- 字段名：
  - 小写字母
  - 用下划线分隔开
  - 避免频繁修改字段

- 索引：
  - 主键以pk_xxx开头，唯一键uk_xxx，idx_xxx

### 3.库操作（CRUD：增加(Create)、检索(Retrieve)、更新(Update)和删除(Delete)）

### DDL语言（data definition language 数据库定义语言）：create、show、alter、drop

创建数据库：

```mysql
create database if not exists `database_name`  charset=utf8; -- 库名和应用名保持一致，指定编码格式utf8
```

查看数据库：

```mysql
show databases; -- 查看全部数据库
show create database database_name; -- 查看创建的数据库
```

修改数据库：

```mysql
alter database database_name charset = gbk; -- 修改库的编码格为gbk
```

删除数据库：

```mysql
drop database if exists `database_name`; 
```

### 4.表操作（CRUD：增加(Create)、检索(Retrieve)、更新(Update)和删除(Delete)）

引用数据库：

```mysql
use `database_name`;   -- 之后表添加进该库
```

创建表：

```mysql
mysql> create table if not exists `table_name`( -- 表名无复数
    -> id bigint unsigned auto_increment comment'主键id', -- id必须为主键，必须为无符号自增bigint类型，不能定义为int，除非是分布式id
    -> name varchar(30) not null comment '老师的名字',
   	-> phone varchar(20) comment '电话号码',
    -> address varchar(100) default '暂时未知' comment '住址',
    -> create_time datetime, -- create_time必须为datatime类型,占用内存比timestamp大，但是无时区调整
    -> update_time datetime, -- create_time必须为datatime类型，占用内存比timestamp大，但是无时区调整
    -> primary key(id)
    -> )engine=innodb charset = utf8;-- 表里必须定义的字段 id ，create_time ，update_time
```

查看表：

```mysql
show tables; -- 查看全部表
show create table table__name; -- 查看创建的表(显示表的创建过程和命令)
desc table__name; -- 查看表的结构（表的字段）
show columns from table__name; -- 查看表中的列属性 
```

修改表：

```mysql
alter table table__name rename to new_table_name; -- 修改表名
```

删除表：

```mysql
drop table if exists table__name ; -- 删除单张表
drop table if exists table_name1,table_name2; -- 删除多张表 
```

- 字符集编码：

```mysql
show variables like 'character_set_%'; -- 查询字符集格式
set character_set_client=utf8; -- 设置为utf8格式（一般在新建database和table时设置好）
```



#### 4.1字段管理（表内字段处理——表操作CRUD）

添加字段：

```mysql
alter table table__name add 字段名 字段类型; -- alter table student add namBe varchar(30);
alter table table__name add 字段A 字段类型 after 字段B; -- alter table student add age int after id; 在字段B后插入字段A
alter table table__name add add 字段 字段类型 first; -- 首行插入字段
```

修改字段：

```mysql
alter table table__name change 字段名 新的字段名 字段类型; -- alter table student change age phone varchar(20); 全修改（名，类型）
alter table table__name modify 字段名 要改成的字段类型; -- alter table student modify phone int(20); 修改字段类型
```

删除字段：

```mysql
alter table table__name drop 字段; -- 从表内删除字段
```

### 5.数据操作（CRUD：增加(Create)、检索(Retrieve)、更新(Update)和删除(Delete)）

### DML语言（data manipiation language 数据操纵语言）：insert、select、update、delete

插入数据（增加数据）：

```mysql
insert into table__name (`字段`) values (插入值); -- insert into teacher (id, name, phone, address) values(1,'Frank', '188888888', 'ShangHai'); 字段可以省略，但插入值需要和字段一一对应
insert into table__name (`字段`) values (插入值),(插入值)...; -- 插入多个值
```

查询数据：

```mysql
select * from table__name; -- 查看全部字段数据; 
select 要查询的字段名 from table__name; -- 查看字段数据
select 要查询的字段名,要查询的字段名,要查询的字段名... from table__name; -- 查看多个字段数据
desc table_name; -- 查看表的结构（表的字段）
```

更新数据：

```mysql
update table__name set 要修改的字段名=要修改成的数据 where 定位字段名=相应数据; -- 根据字段名更新对应数据
update table__name set 要修改的字段名=要修改成的数据，要修改的字段名=要修改成的数据，··· where 定位字段名=相应数据; -- 根据字段名更新多条数据
update table__name set 要修改的字段名=要修改成的数据 where 定位字段名=相应数据 or 定位字段名=相应数据; -- 根据条件更新多条数据
-- 没有where 全表更新 导致SQL注入
```

删除数据：

```mysql
delete from table__name where 选定的字段名称=你要删除的字段相应信息; -- 如果有重复字段会一起删掉
delete from table__name where 某条件; -- 符合某条件删除

truncate table table__name; -- 清空表中所有数据
```

### 6.数据类型(标注宽度)

- int类型：tinyint（-2^4^~2^4^）/ smallint / mediumint / int（-2^16^~2^16^）/ bigint（-2^32^~2^32^） (unsigned放在int类型后)
- decimal类型：小数基本都用，保证精度 --DECIMAL(P,D)：P为精度，D保留小数点位数
- varchar类型：变长字符串，多余字符空间自动回收
- bool类型：0和1
- enum类型：每一个选择对应数字，方便选择--插入数据时可以填数字代表对应选项（1---选择1，2---选择2...）
- set类型：与enum类型区别在于能够选择多个选项
- datetime类型：记录表的各个操作的时间

###7.键操作

#### 7.1超键：在关系中能唯一标识元组的属性集

例：学生表中学号和身份证号能唯一确定一名学生，因此所有“学号+其他属性”和“身份证号+其他属性”均属于超键，如（学号）、（学号，姓名）

注意：除此之外能唯一确定一行的属性组也属于超键，例如（姓名，性别）//假设同名不同性别 也属于超键，但单一属性不是超键

#### 7.2候选键：不含有多余属性的超键（在超键的基础上最小唯一属性组合）

例：单一属性超键必是候选键，如（学号）、（身份证号），（学号、性别）是超键，但删除性别后也能区分，所以不是候选键

注意：（姓名、性别）是候选键，（姓名，性别，年龄）不是候选键

####7.3主键：不为空且在表中的数据是唯一的，一般是id，设为auto_increment（从候选键中选择出的）

增加主键：

```mysql
1.设置字段的时候设置：primary key ; -- id bigint unsigned auto_increment primary key
2.设置表的时候设置：primary key (字段名); -- primary key(id)
3.语句增加主键：
alter table table__name add primary key (字段名); 
```

修改主键：

```mysql
alter table table__name drop primary key ,add primary key (字段); -- 先删除，后增加 
alter table 表名 add primary key （字段，字段...）; -- 先删除原表主键，后添加多个字段生成复合主键
```

删除主键：

```mysql
alter table table__name drop primary key; 
```

####7.4唯一键：在一张表中的这个数据不会关联，且数据唯一不重复

增加唯一键：

```mysql
1.创建表字段的时候设置： unique; -- phone varchar(20) unique
2.语句增加主键：
alter table table__name add unique （字段，字段...）;
```

删除唯一键：

```mysql
alter table table__name drop index 字段名； -- 字段名不能使用括号
```

|            主键 primary key            |                 唯一键 unique                  |
| :------------------------------------: | :--------------------------------------------: |
|           可以用来连接各个表           |               不使用来链接各个表               |
|               不可以为空               |                    可以为空                    |
| 如果没有设置唯一键则自增字段必须是主键 | 自增字段不为主键的时候要将这个字段设置成唯一键 |
|      一张表只能有一个或者一个组合      |             可以有多个且不需要组合             |

####7.5外键：外部引用问题，关联两个表，将公共的字段链接在一起，注意区分主从表

增加外键：

```mysql
1.创建表字段的时候设置： foreign key (字段) references link_table_name(字段);
-> foreign key (stuId) references stu(stuId) on delete set null on update cascade;-- 级联
2.alter table table__name add foreign key (字段) references link_table_name(字段);
```

查看外键：

```mysql
show create table table__name;
CONSTRAINT `xxxxxx` FOREIGN KEY (`xxxx`) REFERENCES `xxx (`xx`);
```

删除外键：

```mysql
alter table table__name drop foreign key foreign_key_name;
```

|                   置空                   |                   级联                   |
| :--------------------------------------: | :--------------------------------------: |
| 链接外键的表中，原表删除的数据会成为NULL | 链接外键的表中，原表删除的数据会全部删除 |

###8.查询操作

- 单表查询

```mysql
from \ where \ in \ is (not) null \ between...and... -- 常见语句
sum() \ avg() \ count() -- 聚合函数(字段)
having -- 聚合函数作为筛选条件时，只能用having 
select * from table__name limit 0,2 -- 返回指定的记录数
select distinct 字段 from table__name -- 去重
select * if (expr1, expr2, expr3) as 新字段 from table__name; -- expr1表示的是判断条件,expr1的值为真时，则返回值为expr2；当expr1的值为假时，则返回值为expr3

-- 模糊查询 -- 
select * from table__name where 字段 like 'T%'; -- %代表多个字符
select * from table__name where 字段 like 'T_'; -- _代表单个字符

select * from table__name order by 字段 desc / asc; -- 排序查找 desc降序 / asc升序
select * sum() \ avg() \ count() as 新字段 from table__name group by 字段; -- 分组查询 聚合函数 (where ... group by/ group by ... having ... )关键次：每个、各个——分组
```

- 多表查询

```mysql
-- union --
select 字段 from table1_name union all select 字段 from table2_name; -- 联合查找 all 效率更高，所有记录全部返回，没有all会 自动去重(没有all和or用法一样)

-- join on --
inner join 
select * from table_A inner join table_B on A.字段=B.字段; -- A表与B表中 A.字段=B.字段的情况包含的所有数据

left join
select * from table_A left join table_B on A.字段=B.字段; -- A表与B表中 A.字段=B.字段的情况包含的所有数据和A表剩余数据

right join 
select * from table_A right join table_B on A.字段=B.字段; -- A表与B表中 A.字段=B.字段的情况包含的所有数据和B表剩余数据

natural join
select * from table_A natural join table_B; -- 自动寻找A表与B表中数据类型和列名都相同的字段

using 与 on -- 如果两个表的关联字段名是一样的，就可以使用Using来建立关系
```

- 子查询

```mysql
-- 将一个查询的结果作为另一个查询的数据来源或判断条件 --
select * from stu where stuId  in (select stuId from eatery where money > 800); -- in为关键词
select * from stu where exists  (select stuId from eatery where money > 900); -- exists为关键词，只要子查询存在，就把主查询所有的列出来
```

###9.事务（Engine=innodb）

事务是必须满足4个条件（ACID）：

- 原子性（Atomicity，或称不可分割性）一个事务中的所有操作，要么全部完成，要么全部不完成，不会结束在中间某个环节

- 一致性（Consistency）：在事务开始之前和事务结束以后，数据库的完整性没有被破坏
- 隔离性（Isolation，又称独立性）：并发事务同时对其数据进行读写和修改的能力
- 持久性（Durability）：对数据的修改就是永久的

```mysql
-- 指定数据库引擎为INNODB才能用 --

start transaction; -- 开始事务

rollback; -- 回滚至最近节点
commit; -- 提交事务

savepoint five; -- 保存节点
rollback to five; -- 回滚至名为five的节点

```

### 10.索引

常见类型：普通索引index、主键索引primary key 、唯一索引unique、联合索引、全文索引、空间索引

优点：帮助你快速查询到数据

缺点：数据设置为索引，增删改查效率低且占空间



添加索引：

```mysql
alter table table__name add primary key (字段); -- 主键索引
alter table table__name add unique (字段); -- 唯一索引
alter table table__name add index index_name (字段); -- 普通索引
alter table table__name add fulltext (字段); -- 全文索引
alter table table__name add index index_name (字段,字段,字段); -- 多列索引
```

查看索引：

```mysql
show index from table__name; 
```



索引约束：

- 一般情况下，有唯一特性的字段要设置唯一索引
- 在实际开发当中不允许多于两个表的查询
- 多表查询的时候，关联的字段也要有索引

###11.视图

一般借助第三方软件，建立一张虚拟表查看数据，隐藏敏感数据与数据结构

###12.零散函数

**存储过程（一般不用）**

创建存储过程：

```mysql
delimiter // -- 设定SQL语句分隔符，可以设置以双斜线为结尾
create procedure proc()
    -> begin
    -> update wallet set balance=balance+50;
    -> update teacher set name='Frank';
    -> end //
delimiter ； -- 设定SQL语句分隔符，用完后还原
```

删除存储过程：

```mysql
drop procedure proc;
```

查看存储过程：

```mysql
show procedure status \G; -- G代表查看所有存储过程
```

**零散函数**

```mysql
-- number --
rand(); -- 随机数
ceil(3.1); -- 向上取整，结果为4
floor(3.1); -- 向下取整，结果为3
round(3.1); -- 四舍五入，结果为3
truncate(3.141592654,2); -- 截取小数点后两位，结果为3.14

-- string --
ucase('abc'); -- 小写变大写
lcase('ABC'); -- 大写变小写
left('ABC',1); -- 从左截取1位
right('ABC',1); -- 从右截取1位
substring('ABC',1,2); -- 从第1位截取2个字符长度
concat('ABC','DEF'); -- 连接多个字符串

-- other --
select year(now()) year, month(now()) month, day(now()) day; -- 设置年月日
select sha("123"); -- 加密字符串，不可逆
select MD5("123"); -- 加密字符串，可逆
select ifnull(null,"frank"); -- 用于判断第一个表达式是否为NULL，如果为NULL则返回第二个参数的值，如果不为NULL则返回第一个参数的值

-- case...when... --
CASE [col_name] WHEN [value1] THEN [result1]…ELSE [default] END

CASE WHEN [expr] THEN [result1]…ELSE [default] END

```



### 13.开发事项

开发约束：

- 不能用where name = null 来判断是否为空，需要用where name is null
- 并发项目中不要使用并发和级联，一切外键的问题在应用层解决
- 不能使用存储过程
- 子查询中避免in操作
- 编码格式统一utf-8



# 数据库设计优化

数据库选型、存储引擎选型、数据表结构设计、数据库优化

## 1.索引

一些数据结构（哈希、二叉树、多叉树等），一般建立在频繁查询且很少修改的属性上

### 1.1哈希索引

数据库采用合适的哈希算法增加索引，并将索引缓存在内存中，使用哈希需要确认记录，存在一个索引对应多个值的情况

问题：1.只能等值查询；2.哈希碰撞降低查询效率

### 1.2BTree索引

叶节点存储指向具体记录的连接（MyISAM引擎），或者叶节点直接存储数据（InnoDB引擎）

索引稳定，不存在哈希碰撞，且数据排序，支持区间、比较查询

### 1.3位图索引

适用于建立索引的属性只有固定的值（哈希：大量的哈希碰撞；Btree：大量相等的树节点）

对枚举值属性具备较好的索引手段

## 2.数据库锁

**乐观锁**

当前操作的对象大概率不存在并发，不需要被操作对象枷锁（版本号策略）

有利于提升被操作对象的并发，对于操作方而言可能出现写失败的情况，牺牲操作方提升被操作对象并发性能（利他）

适用于经常读取而少修改的对象，由数据库的使用方实现

**悲观锁** 

使得持有锁的一方可以独享被操作对象（利己）

- 共享锁（S锁、读锁）：其他操作方**只能**对被操作对象增加读锁，多个对象可以同时读被操作对象，不能写
- 排他锁（X锁、写锁）：其他操作方**不能**对被操作对象增加**任何**锁，只有加锁操作方能够写数据
- 更新锁（U锁）：避免死锁情况，属于能转换成X锁的S锁，处于读状态时，属于S锁，之后写的时候变为X锁

**死锁**

产生条件：互斥、不剥夺、请求和保持、循环等待

解决方法：打破产生条件即可（银行家算法：提前计算资源分配情况）

## 3.事务

事务的基本属性：ACID

事务并发产生的问题：

- 脏读：一个事务读取了另一个事务尚未提交的数据
- 不可重复读：一个事务在多次读取同一数据得到不同结果（其他事务提交了修改）
- 幻读：事务删除表后发现还有记录（其他事务插入了记录）

事务隔离的级别：

- 读未提交：事务更新某数据前加S锁，事务结束时释放
- 读已提交：更新某数据前加X锁，直到事务结束，在读取数据前增加S锁，读取完毕立即释放
- 可重复读：更新某数据前加X锁，直到事务结束，在读取数据前增加S锁，直到事务结束
- 串行化：更新某数据前在表上加X锁，直到事务结束，在读取数据前在表上增加S锁，直到事务结束

| 隔离级别 |  脏读  | 不可重复读 |  幻读  |
| :------: | :----: | :--------: | :----: |
| 读未提交 |  可能  |    可能    |  可能  |
| 读已提交 | 不可能 |    可能    |  可能  |
| 可重复读 | 不可能 |   不可能   |  可能  |
|  串行化  | 不可能 |   不可能   | 不可能 |

##4.海量数据优化

表分区（对数据表的读写操作进行分流）、分库分表（路由操作、拼接操作）、读写分离（主从数据库、主从复制问题）

## 5.非传统数据库

内存数据库：Redis、FastDB等，放弃或者部分放弃持久化，将数据存入内存，提高读写性能

列存储数据库：HBase等，查询某个属性的所有记录（统计）

面向对象数据库：ObjectDB引入了类、对象、继承等概念，适用于面向对象编程

文档数据库：MongoDB、CouchDB等，适用于不方便拆分成属性进行存储的文件

图数据库：Neo4J等，方便存储图

## 6.数据库中间件

对于集群数据库，需要中间件完成分库分表的相关操作（MyCat）
