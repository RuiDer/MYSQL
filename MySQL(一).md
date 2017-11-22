库：
====

1. create database 库名; 创建库

2. show databases;查询库所有库的库名，都有哪些库

3. show create databases  库名;查询具体库的具体信息

4. use 库名;使用当前库

5. drop database 库名;删除该库








表：
=======

1. create table 表名（参数及参数类型）;     创建表

use yixiaotongdao;
set default_storage_engine=InnoDB;
create table choose(name char(20),mobile bigint,sex enum('男','女'));



2. show tables;  显示在当前库里的所有表名


3. show create table 表名;  显示具体表的详细信息


4. insert 给表中插入新值
```
insert into choose values('mahede',17629081645,'男');
```


5.select * from 库名;  查询表的所有记录
```
select*from choose;
```

6.drop table 表名; 删除表

