[TOC]

# Mac MySQL使用教程
> [记录使用Homebrew安装Mysql全过程](http://blog.csdn.net/lkxlaz/article/details/54580735)  (亲测有效)  
> [mac安装mysql的两种方法（含配置）](https://www.jianshu.com/p/fd3aae701db9)

# homebrew 安装完初始化

```
    mysql_secure_installation
```

# 找mysql的配置文件方法

```
输入 mysqld --verbose --help | more


Default options are read from the following files in the given order:
/etc/my.cnf /etc/mysql/my.cnf /usr/local/etc/my.cnf ~/.my.cnf 


我用homebrew安装的在/usr/local/etc/my.cnf目录下
```

# 修改密码

```
    修改密码：
    1、
    终端命令：mysqladmin -uroot -p password
    然后会弹出下面提示
    Enter password:   //这里输入上面的旧密码
    New password:    //重新输入新密码
    Confirm new password: //重新输入新密码
    
    2、
    mysqladmin -u root -p password 123456

    123456是我这里设置的密码，换成你自己的，运行后要输入一个密码，这个密码就是前面记录的root用户的初始密码，输入回车就更改密码成功了
```

# 进入mysql数据库
修改完就可以用`mysql -u root -p`命令登录了。  
也可以用`mysql -h 主机名 -u 用户名 -p`命令登录，但一般我们只有在远程登录的时候才会用到-h参数(远程主机的主机名或者IP地址)，如果本地登录也想用主机名填写localhost就可以了。

# 查看已有的数据库
登录mysql后，输入`show databases;`  
**注意后面的databases这个单词后面是有个s的，然后最后面是有个分";"的。**  

```
    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | mysql              |
    | performance_schema |
    | sys                |
    +--------------------+
```

# 查看端口号
登录mysql后，输入`show global variables like 'port';`

```
    +---------------+-------+
    | Variable_name | Value |
    +---------------+-------+
    | port          | 3306  |
    +---------------+-------+
```

# 创建数据库
`create database study_mysql;`

```
    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | mysql              |
    | performance_schema |
    | study_mysql        |
    | sys                |
    +--------------------+
```

# 对你需要操作的数据库进行操作
`use study_mysql;`

```
    Database changed
```

# 在对应的数据库中创建表
`CREATE TABLE 数据库表名;`

```
CREATE TABLE study001
    -> (
    -> id char(10) not null primary key,
    -> name char(16) not null,
    -> sex char(6) not null,
    -> age int not null,
    -> address char(36) not null                                                    -> );                                                                       Query OK, 0 rows affected (0.02 sec)
```

# 查看该数据库下的所有表
`show tables;`

```
    +-----------------------+
    | Tables_in_study_mysql |
    +-----------------------+
    | study001              |
    +-----------------------+
    1 row in set (0.00 sec)
```

# 查看刚创建的数据库表字段是否正确
`DESCRIBE 数据库表名;`

```
    mysql> DESCRIBE study001;
    +---------+----------+------+-----+---------+-------+
    | Field   | Type     | Null | Key | Default | Extra |
    +---------+----------+------+-----+---------+-------+
    | id      | char(10) | NO   | PRI | NULL    |       |
    | name    | char(16) | NO   |     | NULL    |       |
    | sex     | char(6)  | NO   |     | NULL    |       |
    | age     | int(11)  | NO   |     | NULL    |       |
    | address | char(36) | NO   |     | NULL    |       |
    +---------+----------+------+-----+---------+-------+
    5 rows in set (0.00 sec)
```

# 向数据库表中添加数据
`insert into 数据库表名 values(value值1,value值2,.......);`  
这种必须创建了几个列就需要传几个值，少一个就会报错(**Column count doesn't match value count at row 1**)，
`insert into 数据库表名 (列名1,列名2,...） values(value值1,value值2,...);`  
这种可以少传参数，只会赋值那些设置值的字段(前提设置了默认值，如果没有设置也会报错**ERROR 1364 (HY000): Field 'address' doesn't have a default value**)

```
INSERT INTO study001 VALUES(6, '哈哈1', 'nv1', 10, '哈哈1');
Query OK, 1 row affected (0.01 sec)

INSERT INTO study001 (name, sex, age, address) VALUES ('天天1', '男1', 111, '天国1');
```

# 查询表中数据
`select 列名称 from 数据库表名 [查询条件];`  

<p id="操作符">常用操作符</p>

|操作符|描述|
|:----------:|:--:|
|=	          |等于|
|<>	或!=      |不等于|
|>           |大于|
|<	          |小于|
|>=	          |大于等于|
|<=	          |小于等于|
|BETWEEN	   |在某个范围内("11"（包括）和 "3"（不包括）之间的数据)|
|LIKE	      |搜索某种模式|

<p id="通配符">如果使用LIKE，需要使用到下面的通配符：</p>  

|通配符	     |描述|
|:---:      |:---:|
|%	         |替代一个或多个字符|
|_	         |仅替代一个字符|
|[charlist]	| 字符列中的任何单一字符|
|[!charlist]或[\^charlist]	  | 不在字符列中的任何单一字符|


或者

[!charlist]

不在字符列中的任何单一字符

## 查询所有数据
`select * from 表名;`

```
    mysql> select * from study001;
    +----+------------+------+---------+
    | id | name       | sex  | address |
    +----+------------+------+---------+
    | 1  | 吴冬冬     | 男   | 吴国    |
    | 11 | 小哈       | 11   | 哈哈    |
    | 2  | 吴冬冬2    | 男2  | 吴国2   |
    | 3  | 吴冬冬2    | 男2  | 吴国2   |
    | 4  | 小火       | 男2  | 吴国2   |
    | 5  | 小男       | 男2  | 吴国1   |
    +----+------------+------+---------+
```

## 查询指定column数据
`select id,name from study001;`

```
    mysql> select id,name from study001;
    +----+------------+
    | id | name       |
    +----+------------+
    | 1  | 吴冬冬     |
    | 11 | 小哈       |
    | 2  | 吴冬冬2    |
    | 3  | 吴冬冬2    |
    | 4  | 小火       |
    | 5  | 小男       |
    +----+------------+
```

##查询特定条件的表中数据
`select 列名称 from 数据库表名 where 查询条件;`

### 查询一个固定条件
`select 列名称 from 数据库表名 where 某个column=某个值;`

```
    mysql> select * from study001 where name='吴冬冬2';
    +----+------------+------+---------+
    | id | name       | sex  | address |
    +----+------------+------+---------+
    | 2  | 吴冬冬2    | 男2  | 吴国2   |
    | 3  | 吴冬冬2    | 男2  | 吴国2   |
    +----+------------+------+---------+
```

### 查询多个固定条件
- and 提取所有都成立的数据
- or 提取成立一条的数据

```
    mysql> select * from study001 where sex='男2' and address='吴国1';
    +----+--------+------+---------+
    | id | name   | sex  | address |
    +----+--------+------+---------+
    | 5  | 小男   | 男2  | 吴国1   |
    +----+--------+------+---------+
    1 row in set (0.00 sec)
```

### 查询范围条件
需要用到[常用的操作符](#操作符)

```
mysql> select * from study001 where id between '11' and '3';
+----+------------+------+---------+
| id | name       | sex  | address |
+----+------------+------+---------+
| 11 | 小哈       | 11   | 哈哈    |
| 2  | 吴冬冬2    | 男2  | 吴国2   |
| 3  | 吴冬冬2    | 男2  | 吴国2   |
+----+------------+------+---------+
3 rows in set (0.00 sec)
```

### [通配符使用](#通配符)

```
    mysql> select * from study001 where name like '小%';
    +----+--------+------+---------+
    | id | name   | sex  | address |
    +----+--------+------+---------+
    | 11 | 小哈   | 11   | 哈哈    |
    | 4  | 小火   | 男2  | 吴国2   |
    | 5  | 小男   | 男2  | 吴国1   |
    +----+--------+------+---------+
```

# 修改数据表中数据
