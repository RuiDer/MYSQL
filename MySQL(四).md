# 记录的操作

## 1. 添加记录
------------------------
语法：（1）.`insert into <table_name>(各个参数名称) values(参数对应值)`
      （2）. `insert into <table_name> <(字段列表..)> select * from <table_name> <(字段列表)>`

                   //向已有的表中添加记录

例如：


```
create database choose;
use choose;
create table choose(
course_no int not null unique auto_increment,
course_name varchar(20) not null,
up_limit int not null default 60,
description varchar(100) default '暂无',
status enum('已审核','未审核') not null default'未审核',
techer_no int not null
);


alter table choose 
change techer_no 
teacher_no int not null;

**insert into choose(course_name,teacher_no) values('MySQL数据库',01)**;
select * from choose\G;

#设置auto_increment=160101101
alter table choose auto_increment=1601011;

#删除记录
delete from choose;

#设置teacher_no为unique
alter table choose 
change teacher_no
teacher_no int not null unique;

insert into choose(course_name,teacher_no) values('MySQL数据库', 01);
insert into choose(course_name,teacher_no) values('Java语言程序设计',02);
insert into choose(course_name,teacher_no) values('C语言', 89);
select * from choose\G;
```

运行结果为：

```
mysql> select * from choose;
+-----------+------------------------+----------+-------------+-----------+------------+
| course_no | course_name            | up_limit | description | status    | teacher_no |
+-----------+------------------------+----------+-------------+-----------+------------+
|   1601011 | MySQL数据库            |       60 | 暂无        | 未审核    |          1 |
|   1601012 | Java语言程序设计       |       60 | 暂无        | 未审核    |          2 |
|   1601013 | C语言                  |      150 | 暂无        | 未审核    |         89 |
+-----------+------------------------+----------+-------------+-----------+------------+
```
----------------------------


## 2. 删除记录**delete**,**truncate**
--------------------------
> 语法:`delete from <table_name> where <条件>`

例如：

```
use choose;
insert into choose(course_name,teacher_no) values('MySQL数据库', 01);
insert into choose(course_name,teacher_no) values('MySQL数据库', 01);
insert into choose(course_name,teacher_no) values('Java语言程序设计',02);
insert into choose(course_name,teacher_no) values('C语言', 89);
select * from choose\G;

delete from choose where course_name='C语言';
select * from choose;
```

运行结果如下：
```
mysql> select * from choosde;
ERROR 1146 (42S02): Table 'choose.choosde' doesn't exist
mysql> delete from choose where course_name='C语言';
Query OK, 0 rows affected (0.00 sec)

mysql> select * from choose;
+-----------+------------------------+----------+-------------+-----------+------------+
| course_no | course_name            | up_limit | description | status    | teacher_no |
+-----------+------------------------+----------+-------------+-----------+------------+
|   1601011 | MySQL数据库            |       60 | 暂无        | 未审核    |          1 |
|   1601012 | Java语言程序设计       |       60 | 暂无        | 未审核    |          2 |
+-----------+------------------------+----------+-------------+-----------+------------+
```
> 接着执行下面语句：
```
insert into choose(course_name,teacher_no) values('C语言', 89);
select * from choose;
```
运行结果如下：

```
mysql> insert into choose(course_name,teacher_no) values('C语言', 89);
Query OK, 1 row affected (0.05 sec)

mysql> select * from choose;
+-----------+------------------------+----------+-------------+-----------+------------+
| course_no | course_name            | up_limit | description | status    | teacher_no |
+-----------+------------------------+----------+-------------+-----------+------------+
|   1601011 | MySQL数据库            |       60 | 暂无        | 未审核    |          1 |
|   1601012 | Java语言程序设计       |       60 | 暂无        | 未审核    |          2 |
|   1601014 | C语言                  |       60 | 暂无        | 未审核    |         89 |
+-----------+------------------------+----------+-------------+-----------+------------+
```

### 这里的course_no自增型将'C语言'设置为1601014！！！！！！！！！！出现断层！！！

## 总结：删除记录**delete。。truncate**

----------------------------
> 语法：`delete from <table_name> where <条件>`         //不用where**清空**所有记录
                                                       //用where**删除**某些记录，
                                                      //会使自增值出现断层

> 语法：`rruncate table <table_name>`    //**清空**所有记录，**自增值重置为1**

----------------------------------------------------------------------------------


## 3. 修改记录
----------------------------
语法：`update <table_name> set <要修改的值1>,<要修改的值2>......... where <条件>`

例如：
```
update choose
set up_limit=150 where course_no<=1601012;
```
--------------------------------------------




## ····· 设置自增型 `auto_increment`
-------------------------------
> 自增值类型的在设置正确语句是：
语法：（1） `alter table auto_increment=<数字>`
      （2）`create table <table_name>(参数)engine=InnoDB default_charset=gdk auto_increment=<数值>`

> 下面这句设置是错误的，因为每个表只能含有一个自增值字段，可以直接设置

```
alter table choose
change course_no
course_no int not null unique auto_increment=100;
```
错误！！！！！！


----------------


## 2.在复制表时自增值的不同变化
----------------------------------
1. 用like只是复制表的结构，不复制表的记录，而用select不仅复制了表的结构，还有记录
2. 但是，select会导致新表重新设置**自增值类型为默认值为0**,like自增值初始为**1**.

比如下面例子：

原表：
```
mysql> show create table choose;
+--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table  | Create Table                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
+--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| choose | CREATE TABLE `choose` (
  `course_no` int(11) NOT NULL **AUTO_INCREMENT**,
  `course_name` varchar(20) NOT NULL,
  `up_limit` int(11) NOT NULL DEFAULT '60',
  `description` varchar(100) DEFAULT '暂无',
  `status` enum('已审核','未审核') NOT NULL DEFAULT '未审核',
  `teacher_no` int(11) NOT NULL,
  UNIQUE KEY `course_no` (`course_no`),
  UNIQUE KEY `teacher_no` (`teacher_no`)
) ENGINE=InnoDB **AUTO_INCREMENT=1601015 **DEFAULT CHARSET=utf8            |
```

新表：
--------------------------------------

 用like语句：
```
mysql> create table choose1 like choose;
Query OK, 0 rows affected (0.27 sec)

mysql> show create table choose1;
+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table   | Create Table                                                                                                                                                                                                                                                                                                                                                                                                                           |
+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| choose1 | CREATE TABLE `choose1` (
  `course_no` int(11) NOT NULL AUTO_INCREMENT,
  `course_name` varchar(20) NOT NULL,
  `up_limit` int(11) NOT NULL DEFAULT '60',
  `description` varchar(100) DEFAULT '暂无',
  `status` enum('已审核','未审核') NOT NULL DEFAULT '未审核',
  `teacher_no` int(11) NOT NULL,
  UNIQUE KEY `course_no` (`course_no`),
  UNIQUE KEY `teacher_no` (`teacher_no`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8            |
```
并运行
```
mysql> insert into choose1(course_name,up_limit,teacher_no) values('Java编程语言',150,02);
Query OK, 1 row affected (0.06 sec)

mysql> select * from choose1;
+-----------+------------------+----------+-------------+-----------+------------+
| course_no | course_name      | up_limit | description | status    | teacher_no |
+-----------+------------------+----------+-------------+-----------+------------+
|         1 | Java编程语言     |      150 | 暂无        | 未审核    |          2 |
+-----------+------------------+----------+-------------+-----------+------------+
```
## 注意自增值变为1了！！！！！！

------------------------------------------------------

---------------------------------------------------
用select语句：
```

mysql> create table choose1 select * from choose;
Query OK, 3 rows affected (0.32 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> show create  table choose1;
+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table   | Create Table                                                                                                                                                                                                                                                                                                                                      |
+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| choose1 | CREATE TABLE `choose1` (
  `course_no` int(11) NOT NULL **DEFAULT '0'**,
  `course_name` varchar(20) NOT NULL,
  `up_limit` int(11) NOT NULL DEFAULT '60',
  `description` varchar(100) DEFAULT '暂无',
  `status` enum('已审核','未审核') NOT NULL DEFAULT '未审核',
  `teacher_no` int(11) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8            |
```

注意：这里的**自增值被始终默认为0.**

-------------------------------
