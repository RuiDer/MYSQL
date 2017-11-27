# 记录的操作

## 1. 添加记录
------------------------
语法：`insert into <table_name>(各个参数名称) values(参数对应值)`

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


接着再运行下面语句：

```
create table choose1 select * from choose;
select * from choose1;
delete from choose1;
insert into choose1(course_name,teacher_no) values('C语言', 01);
select * from choose1;
insert into choose1(course_name,teacher_no) values('Java语言程序设计',02);
select * from choose1;
```
运行结果如下：

```
mysql> select * from choose1;
+-----------+-------------+----------+-------------+-----------+------------+
| course_no | course_name | up_limit | description | status    | teacher_no |
+-----------+-------------+----------+-------------+-----------+------------+
|         0 | C语言       |       60 | 暂无        | 未审核    |          1 |
+-----------+-------------+----------+-------------+-----------+------------+
1 row in set (0.00 sec)

mysql> insert into choose1(course_name,teacher_no) values('Java语言程序设计',02);
Query OK, 1 row affected (0.09 sec)

mysql> select * from choose1;
+-----------+------------------------+----------+-------------+-----------+------------+
| course_no | course_name            | up_limit | description | status    | teacher_no |
+-----------+------------------------+----------+-------------+-----------+------------+
|         0 | C语言                  |       60 | 暂无        | 未审核    |          1 |
|         0 | Java语言程序设计       |       60 | 暂无        | 未审核    |          2 |
+-----------+------------------------+----------+-------------+-----------+------------+
```

### 注意，这里的自增型变为0了！！！！！！！！也就是说自增型不工作了！！！！！！


## ····· 设置自增型 `auto_increment`
=============================
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
