# 表的操作

-------------------
1. 创建表

```
use 库名;
create table 表名(变量名 数据类型 约束条件，.....)
```
例如：

```
use yixiaotongdao;
create table users(身份证号 varchar(20) not null uqique primary key);
```
----------------------
2. 查询表的约束条件：           //容易出错！！！！

```
select constraint_name,constrain-type 
from information_schema.table_constraints 
where table_schema='yixiaotongdao' and table_name='users';
```
----------------------------

3. 查询索引信息
```
select index from users\G;    //“\G”表示1.发送信息  2.结果以垂直方式显示，比如下面：
```

```
mysql> show index from users\G;
*************************** 1. row ***************************
        Table: users
   Non_unique: 0
     Key_name: 手机号
 Seq_in_index: 1
  Column_name: 手机号
    Collation: A
  Cardinality: 1
     Sub_part: NULL
       Packed: NULL
         Null:
   Index_type: BTREE
      Comment:
Index_comment:
*************************** 2. row ***************************
        Table: users
   Non_unique: 0
     Key_name: 账户昵称
 Seq_in_index: 1
  Column_name: 账户昵称
    Collation: A
  Cardinality: 1
     Sub_part: NULL
       Packed: NULL
         Null:
   Index_type: BTREE
      Comment:
Index_comment:
```
--------------------------------------------------------------

3.设置/增加主键和外键


（1）新建主键1

```
create table class(
class_no varchar(10) not null primary key;
); 
```

（2）新建主键2

```
create table stu(
stu_no varchar(10) not null，
stu_name varchar(10) not null,
constraint stu_no_pk primary key(stu_no)    //设置主键
);
```


(3)新建外键

```
create table class(
class_no varchar(10) not null ,
stu_no varchar(10) not null，
constraint class_no_pk primary key(class_no),     //设置主键
constraint stu_no_fk foreign key(stu_no) references stu(stu_no);   //设置外键（将表stu的stu_no设置为表class的外键，注意表class中必须拥有变量stu_no）
); 
```

(4) 如何在已有表中添加外键and添加级联选项
--------------------------
> 语法:alter table 表名 add constraint FK_ID foreign key(你的外键字段名) REFERENCES 外表表名(对应的表的主键字段名)；

例如：

```
alter table tb_active add 
constraint stu_no_fk foreign key(stu_no) 
references class(stu_no) 
on update cascade on delete cascade;
```

> 级联选项有：
> on update cascade on delete cascade;  //随父表同时更新同时删除
> on update set null on delete set null; //主表删除或者修改时，子表为null
> on update no action on delete no action;  //如果父表修改或者删除，将会出现删除失败
> on update restrict on delete restrict;   //与no action 一样
--------------------------

（5）如何在已有表中添加主键
--------------------------

```
alter table stu add constraint stu_no_pk primary key(stu_no);
```



4.修改表的结构及内容
==============================

（1）修改表名  **rename**
------------------------

`alter table table_name rename new_name; `  //修改表名，例如：



```
alter table yixiaotongdao 
rename hello;
```
-------------------------

 (2)修改表的字段名  **change**
---------------------------
语法：`alter table <table_name> change column 字段名 新字段名 字段类型`

```
alter table stu 
change column stu_no 
student_no varchar(10) not null;
```
-----------------------------


 (3)修改表中字段的类型：**modify** 或者 **change**

语法：`alter table <表名> modify column 字段名 + 新的字段类型`；
----------------------------
```
alter table stu modify/change column stu_no not null;
```
----------------------------------



 (4) 删除字段 **drop**
-------------------------------
语法：`alter table <table_name> drop <字段名>`

```
alter table class
drop class_b;
```

 (4)如何在已有表中添加外键
------------------------------------

> 语法:alter table 表名 add constraint FK_ID foreign key(你的外键字段名) REFERENCES 外表表名(对应的表的主键字段名)；

例如：

```
alter table tb_active 
add constraint stu_no_fk foreign key(stu_no)
 references class(stu_no);
```
----------------------------------


 (5) 如何删除已有外键
---------------------------------


> 语法：ALTER TABLE <表名> DROP CONSTRAINT <外键名>

例如：

```
alter table stu 
drop foreign key class_no_fk;
```
---------------------------------


 (6)如何在已建表中增减字段


------------------------------
```
alter table class 
add column class_number int(60);
```

-----------------------------

5.复制表
=============
-------------------------------

（1）复制表的字段以及字段类型和约束条件，不复制记录
------------------

语法：`create table  <table_name> like <table_name>`;

例如：

```
create  table class(
class_no int(60) NOT NULL AUTO_INCREMENT,
class_number int(60) DEFAULT NULL);
insert into class values(33,40);
select * from class;
create table class1 like class;
show create table class1\G;
select * from class1;
```

结果如下：

```
mysql> select * from class;
+----------+--------------+
| class_no | class_number |
+----------+--------------+
|       33 |           40 |
+----------+--------------+
1 row in set (0.00 sec)
mysql> show create table class1\G;
*************************** 1. row ***************************
       Table: class1
Create Table: CREATE TABLE `class1` (
  `class_no` int(60) NOT NULL AUTO_INCREMENT,
  `class_number` int(60) DEFAULT NULL,
  PRIMARY KEY (`class_no`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
1 row in set (0.00 sec)

ERROR:
No query specified

mysql> select * from class1;
Empty set (0.00 sec)
```

---------------------


（2）复制字段，字段类型，约束条件，以及记录
-------------------------------------

语法：`create table <new_table_name> select * from <table_name>`

```
create table class2 select * frorm class;
show create table class2;
show index from class2\G;
select * from class2;
```

结果如下：

```
mysql> create table class2 select * from class;
Query OK, 1 row affected (0.38 sec)
Records: 1  Duplicates: 0  Warnings: 0

mysql> select * from class2;
+----------+--------------+
| class_no | class_number |
+----------+--------------+
|       33 |           40 |
+----------+--------------+
```
