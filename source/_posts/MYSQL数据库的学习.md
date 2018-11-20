---
title: MYSQL数据库的学习
tags: MYSQL
categories: MYSQL 数据库
archives: JAVA
photos:
  - 'http://phedcy52v.bkt.clouddn.com/top-iamge1.jpg'
date: 2018-10-30 12:17:12
---

##  什么是数据库
数据库（DATABASE）是按照数据结构来组织、存储、和管理数据的仓库。
关系型数据的特点：
1.  数据以表格的形式出现。
2.  每行行各种记录的名称。
3.  每列为记录名称所对象的数据域。
4.  许多行和列组成一张表单。
5.  若干的表单组成DATABASE。

##  MYSQL的安装
详情请参照[该网站](http://www.runoob.com/mysql/mysql-install.html)配置。


##  MYSQL的相关操作

### 创建数据库
`CREATE DATABASE 数据库名` 
例如：`CREATE DATABASE scholl`，创建一个名为**scholl**的数据库。
然后`use 数据库名`就使用该数据库了。

### 删除数据库
`DROP DATABASE 数据库名`
例如：`DROP DATABASE scholl`，删除名为**scholl**的数据库。

### MYSQL的数据类型
MYSQL支持多种类型，大致可以分为三类：数值、日期和字符串。

####  数值类型
这些类型包括严格数值数据类型(INTEGER、SMALLINT、DECIMAL和NUMERIC)，以及近似数值数据类型(FLOAT、REAL和DOUBLE PRECISION)。
![](http://phed7ux2i.bkt.clouddn.com/MySQL%E6%95%B0%E5%80%BC%E7%B1%BB%E5%9E%8B.png)

####  日期和时间类型
表示时间值的日期和时间类型为DATETIME、DATE、TIMESTAMP、TIME和YEAR。
![](http://phed7ux2i.bkt.clouddn.com/MySQL%E6%97%A5%E6%9C%9F.png)

####  字符串类型
字符串类型指CHAR、VARCHAR、BINARY、VARBINARY、BLOB、TEXT、ENUM和SET。
![](http://phed7ux2i.bkt.clouddn.com/MySQL%E5%AD%97%E7%AC%A6%E4%B8%B2.png)

####  创建数据表
创建MySQL数据表需要以下信息
- 表名
- 表字段名
- 定义每个表字段
`CREATE TABLE 表名 (字段名 字段类型)`
例如：`
CREATE TABLE class (
  name VARCHAR,
  id INT
)
`
####  删除数据表
`DROP TABLE 表名`
例如：`DROP TABLE class`删除一个名为**class**的数据表
`TRUNCATE TABLE 表名`删除表内所有数据，保留表结构。
`DELETE FROM 表名 WHERE 条件`删除表内的数据，或者删除某一列。

####  插入数据
MySQL表中使用**INSERT INTO**语句来插入数据。
`INSERT INTO 表名 VALUES （）`
1.  单行插入
`INSERT INTO 表名 VALUES ()`
2.  多行插入
`INSERT INTO 表名 VALUES (),()`

#### 更新数据
MySQL表中使用**UPDATE 表名 SET 数据名称=新值 [WHERE 条件判断]**
`UPDATE class SET grade=60 WHERE name="小凡"`

####  删除数据
MySQL表中使用**DELETE FROM 表名 [WHERE 条件]**
`DELETE FROM class WHERE grade<60`

####  修改表结构
`alter table 表名 rename  新表名`
`alter table 表名 add 新列名 新列的定义`
`alter table 表名 change 旧列名 新列名 新列的定义`

一个例子：
 向学生表插入4行数据
INSERT INTO score (sname,snum,cname,grade) values("王五",3,"英语",59.5),("赵六",4,"语文",99.5),("田七",5,"数学",7),("王八",6,"思想品德",3);
-- 2. 将所有不及格 的成绩在原来基础之上加5分
-- 3.将王五的成绩降到40
-- 4.删除表低于30分的成绩
-- 5.两种方式删除表中所有数据
```
CREATE DATABASE school;
use scholl;

CREATE TABLE score (
  sname VARCHAR(4),
  snum TINYINT,
  cname VARCHAR(4),
  grade float
)

INSERT INTO score (sname,snum,cname,grade) VALUES ("王五",3,"英语",59.5),("赵六",4,"语文",99.5),("田七",5,"数学",7),("王八",6,"思想品德",3);

UPDATE score SET grade = grade + 5 WHERES grade < 60;

UPDATE score SET grade = 40 WHERE sname = "王五";

DELETE FEOM score WHERE grade < 30;

DORP TABLE score  //删除表全部数据和表结构
TRUNCATE TAVLE score  //删除表全部数据，保留表结构
DELETE FROM score //删除表全部数据，表结构不变

```

## 数据完整性
数据库完整性（Database Integrity）是指数据库中数据在逻辑上的一致性、正确性、有效性和相容性。

###  组成
1.  实体完整性
2.  域完整性
3.  参照完整性
4.  自定义完整性

###  主键(PRIMARY KEY)
定义：表中有一列或几列的值能用来唯一的表示表中的每一行。

两种方式添加主键
```
1.
CREATE TABLE class (
  sname VARCHAR(4),
  snum INT PRIMARY KEY,
  sage TINYINT
)

2.
ALTER TABLE class ADD PRIMARY KEY(snum)
```
删除主键
`ALTER TABLE class DROP PRIMARY KEY`

###  外键(FOREIGN KEY)
定义：**字表**对应于**主表**的列，体现了引用完整性、列完整性。主键唯一，外键多个。
添加外键
```
CREATE TABLE class2 (
  sname VARCHAR(4),
  snum INT,
  sage TINYINT,
  PRIMARY KEY (snum),
  FOREIGN KEY (sname) REFERENCES student(class1)
)
ALTER TABLE class2 ADD FOREIGN KEY (sname) REFERENCES student(class1)
```
删除外键
`ALTER TABLE class2 DROP FOREIGN KEY (sname)`

###  自增长(AUTO_INCREMENT)
体现了实体的完整性，常搭配主键一起使用。
```
CREATE TABLE class (
  sname VARCHAR(8),
  snum PRIMARY KEY AUTO_INCREMENT,
  sage TINYINT,
  ssex VARCHAR(2)
)
```

###  唯一约束(unique)
```
CREATE TABLE class (
  sname VARCHAR(8),
  snum PRIMARY KEY UNIQUE,
  sage TINYINT,
  ssex VARCHAR(2)
)
```

###  非空约束(NOT NULL)
```
CREATE TABLE class (
  sname VARCHAR(8),
  snum PRIMARY KEY NOT NULL,
  sage TINYINT,
  ssex VARCHAR(2)
)
```

###  检查约束(SET(类型))
```
CREATE TABLE class (
  sname VARCHAR(8),
  snum PRIMARY KEY,
  sage TINYINT,
  ssex VARCHAR(2) SET("男", "女")
)
```

###  默认值约束(default)
```
CREATE TABLE class (
  sname VARCHAR(8),
  snum PRIMARY KEY AUTO_INCREMENT,
  sage TINYINT DEFAULTE "18",
  ssex VARCHAR(2)
)
```
## 三大范式
1.  确保每列都保持原子性
2.  确保每行都具有唯一性(拥有主键)
3.  确保每列都和主键直接相关，而不是间接相关。

一个实例
```
-- CREATE DATABASE admin;
-- USE admin;

CREATE TABLE user_info (
	user_id INT PRIMARY KEY NOT NULL UNIQUE AUTO_INCREMENT,
	user_nickname VARCHAR(10) UNIQUE,
	user_iphone	VARCHAR(11)	UNIQUE NOT NULL,
	user_email	VARCHAR(40)	UNIQUE NOT NULL,
	user_sex	VARCHAR(8)	NOT NULL,
	user_age	TINYINT NOT NULL,
	user_address	VARCHAR(40) NOT NULL
)

CREATE TABLE user_login_table (
	user_login_id INT PRIMARY KEY NOT NULL UNIQUE AUTO_INCREMENT,
	user_login_userId VARCHAR(20) UNIQUE NOT NULL,
	user_login_username	VARCHAR(20)	UNIQUE NOT NULL,
	user_login_password	VARCHAR(20)	NOT NULL
)

CREATE TABLE product_table (
	product_id INT PRIMARY KEY NOT NULL UNIQUE AUTO_INCREMENT,
	product_name VARCHAR(20) NOT NULL,
	product_num	INT	 NOT NULL,
	product_price	FLOAT	NOT NULL,
	product_description	VARCHAR(100),
	product_createDate	DATE	NOT NULL,
	product_date	DATE
)

CREATE TABLE shoppingcar(
	shoppingcar_id	INT PRIMARY KEY NOT NULL UNIQUE AUTO_INCREMENT,
	shoppingcar_product_name	VARCHAR(20),
	shoppingcar_num	INT,
	shoppingcar_userId	INT,
	FOREIGN KEY(shoppingcar_userId) REFERENCES user_info(user_id)
)

CREATE TABLE hadShopping(
	hadShopping_shoppingcar_id	INT,
	hadShopping_buyTine	DATE,
	hadShopping_address	VARCHAR(40),
-- 	FOREIGN KEY(shoppingcar_userId) REFERENCES user_info(user_id)
	FOREIGN KEY(hadShopping_shoppingcar_id) REFERENCES product_table(product_id)
)
```

##  DQL(数据查询语言)
1.   简单查询
2.  复杂查询
> 1.  聚合函数与分组
> 2.  子查询
> 3.  链接查询

### 简单查询
`SELECT 列名  FROM 表名 [WHERE 条件] [ORDER BY 列名]`
1.  查询所有信息
`SELECT * FROM student`
2.  按条件查询
`SELECT * FROM student Where sex="nv" ORDER BY age;`
3.  重命名AS
`SELECT id AS 学生编号  FROM student`
4.  合并列作为一个新列
`SELECT 工资＋奖金 AS 收入`
5.  限制行数（limit）
`SELECT 列名 FROM 表名 WHERE age=10 LIMIT 起始行，行数;`
6.  不重复查询
`SELECT DISTINCT  列名  FROM  表名`
7.  模糊查询
`SELECT * FROM 表名 WHERE sname like "c_";`

### 复杂查询
####  聚合函数与分组
需求：查询数据表中的最高成绩，最低成绩，平均成绩，总成绩，成绩记录个数
`COUNT, SUM, AVG, MAX, MIN`(行数， 总和， 平均， 最大， 最小)
使用方法：
1.  `select count(列名) from 表名`
2.  `select sum(列名) from 表名`
3.  `select avg(列名) from 表名`
4.  `select max(列名) from 表名`
5.  `select mix(列名) from 表名`
需求：查询每个同学的平均成绩。
`select sname,sum(grade) from 表名 GROUND BY  courseId`

####  多列分组
`select 列名 from 表名 GROUND BY 分组条件 HAVING 聚合条件s`

####  查询顺序
`SELECT * FROM  表名  where 条件  GROUP BY  分组  HAVING 聚合函数过滤  ORDER BY   LIMIT`

### 子查询
定义：可以解决需要结合多张表进行查询的需求。
分类：
1.  查询语句作为另一条查询语句的一个条件的值。
需求：查询平均成绩大于60的学生姓名
```
select sname from student; //从student表中查询sname
select sno from sc groud by sno having avg(score)>60; //
select sname from student where sno in (select sno from sc groud by sno having avg(score)>60);
```

2.  查询语句作为另一条查询语句的一张表。
一般解题步骤：先找到核心需求，确定最终要查的表，将每个条件用单独的一条`select`语句表示出来，将所写出的语句通过表之间的外键联系起来。


### 视图查询
保存查询语句集的展示形式，保存结果集的表

`create view view_student_sc as`

### 链接查询
结合多张表完成更复杂的查询需求
分类：
1.  内连接(INNER JOIN)
`select sno,sname from student`
`select sno from sc groud by sno having avg(score) between 60 and 80`
`select sno,sname from student where sno in (select sno from sc groud by sno having avg(score) between 60 and 80)`
-------------------
`select * from student inner join sc on sc.sno=student.sno groud by sno having avg(score) having between 60 and 80`

2.  外联接（左外，右外）

## 高级特效(包含了DCL)
1.  变量

1.1 用户变量
`SET @变量名=变量值` // 定义变量
`select @变量名`  //输出变量
1.1.1 全局变量
`show global variable`  查看所有的全局变量
在 `my-default.ini`文件里面修改全局变量
1.2 系统变量
1.3 局部变量
```
create procedure pro_if()
begin
declare a tinyint;
select min(age) into a from student;
if a<18 then
select "小孩子";
elseif a>18 and a<60 then
select "大孩子";
else
select "老孩子";
end if;
end

call pro_if;
```
### 存储过程
####  带参数的
1.  输入的形参 和 输出的形参
 

2.  输出的形参



### 触发器
体现自定义完整性，构建更加合理、健壮的数据库，数据检查问题。
### 事务
将多个操作视为一个整体的概念
特性：原子性，一致性，隔离性，持久性。
### 游标