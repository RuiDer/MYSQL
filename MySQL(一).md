# 库：

> 这里是我在学习过程中遇到的问题以及一些经验，希望对你有用。如果笔者的总结对你的学习有帮助，那就点击屏幕右上方的fork，谢谢！！！！

--------------------------------------------------------------------------------------------------
1. create database 库名; 创建库

2. show databases;查询库所有库的库名，都有哪些库

3. show create databases  库名;查询具体库的具体信息

4. use 库名;使用当前库

5. drop database 库名;删除该库


6. **中文字符集的配置方法**：（解决汉子乱码的方法）

##### 将mysql字符集默认为utf8，就不会出现问题了
  
  （1）首先以管理员的身份打开MySQL的虚拟主机（必须以管理员的身份，否则出错）
  （2）用`show variables like 'char%'`查看主机的系统变量的默认值如下操作

![](http://img3.imgtn.bdimg.com/it/u=2412101824,1844181161&fm=27&gp=0.jpg)
    

   (3)然后打开MySQL所在目录，打开`my.ini`在里面找到**[mysqld]**,配置`character_set_server=utf8`


找到**[client]**,配置`default-character-set=utf8`


找到**[mysql]**,配置`default-character-set=utf8`

如下操作：
![](http://jingyan.baidu.com/album/d5a880ebb7a40513f147cc87.html?picindex=3)

![](http://jingyan.baidu.com/album/d5a880ebb7a40513f147cc87.html?picindex=4)

   (4)打开mysql命令行（管理员身份），执行

```
mysqld remove;
mysqld install;

```
然后就可以正常使用了

```
net start mysql;
mysql -u root -p;

```
输入密码；

##### 注意：使用汉语必须是字符集默认为utf8后，然后新建库，其对象才能使用汉语，之前建立的库以及库的对象
##### 还是不支持汉语

------------------------------------------------------------------------------------------------

# 表：

-----------------------------------------------------------------------
1. create table 表名（参数及参数类型）;     创建表

use yixiaotongdao;
set default_storage_engine=InnoDB;
create table choose(name char(20),mobile bigint,sex enum('男','女'));



2. show tables;  显示在当前库里的所有表名


3. show create table 表名;  显示具体表的详细信息


4. insert 给表中插入新值
```insert into choose values('mahede',17629081645,'男');
```


5.select * from 库名;  查询表的所有记录
```
select*from choose;
```

6.drop table 表名; 删除表

--------------------------------------------------------------------------------
