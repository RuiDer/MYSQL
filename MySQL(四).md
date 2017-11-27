## 6. 添加记录
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

## 8. 设置自增型 `auto_increment`(每个表只能含有一个自增型字段！！！！)
-------------------------------
> 自增值类型的在设置正确语句是：
语法： `alter table auto_increment=<数字>`

> 下面这句设置是错误的，因为每个表只能含有一个自增值字段，可以直接设置

```
alter table choose
change course_no
course_no int not null unique auto_increment=100;
```
错误！！！！！！


----------------

