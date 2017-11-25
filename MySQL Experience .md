# MySQL经验：

> 这是笔者在学习MySQL数据库时总结的经验，希望对你有帮助。
--------------------------------------------------------------------------------------
## 中文字符集的配置方法：（解决汉子乱码的方法）

 ### 将mysql字符集默认为utf8，就不会出现问题了
  
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

### 注意：使用汉语必须是字符集默认为utf8后，然后新建库，其对象才能使用汉语，之前建立的库以及库的对象
### 还是不支持汉语
-----------------------------------------------------------------------------------------------

代码易错点;
=========

1. insert into 表名 **values**(参数);   //

2. show variables **like** '变量名';

3. constraint class_no_fk foreign kay(class_no) refererences stu(class_no);  //容易丢掉references stu(class_no)


概念易混淆点：

3. show **create** database 库名;  // **显示库的具体信息**。

4. show databases;      //显示有哪些库

5. show **create** table 表名;    //显示该表的信息，比如存储引擎等

6. show tables;   //显示有哪些表

7. select * from 表名;    //显示表里的具体内容
