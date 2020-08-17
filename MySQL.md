MySQL

## 基础操作

所有sql语句 以;结尾

数据库关键字不区分大小写

数据库 xxx 语言

DDL	定义

DML	操作

DQL	查询

DCL	控制



### 查看

```sql
show databases; 		--查看所有数据库

use 数据库名称;			 --使用某个数据库
（如果 表名拥有特殊字符 则需使用 ` `括住表名）

show tables;			--查看数据库中的所有表
desc 表名;			--显示数据库中所有表的信息
```

### 建立

```sql
create database 测试;	   --新建一个数据库
create database if not exists 测试;--新建一个数据库
```

### 删除

```sql
drop database 测试;	   --删除数据库
drop database if exists 测试;	   --删除数据库

ALTER TABLE xueshen2 DROP age2
--删除字段   表名 		字段名
```

### 表的修改、查找与增加

```sql
select id form student;  --在student中查找id数据

ALTER TABLE teacher RENAME AS xueshen2
--修改表名    旧名称			  新名称

ALTER TABLE xueshen2 ADD age INT(5)
--增加一个字段 表名         字段名称  列属性

ALTER TABLE xueshen2 MODIFY age VARCHAR(5)
--修改表的字段 表名			字段名	 变成什么

ALTER TABLE 表名 CHANGE 原字段 新字段 VARCHAR(10)
CHANGE：可以修改名称
MODIFY：可以修改字段与约束条件，但不能改字段名

```

### 注释

```sql
-- 单行注释

/*
多行注释
*/

## 数
```

## 据库的列类型

### 数字变量

tinyint			十分小的数据		 1字节

smallint	 	 较小的数据			2字节

mediumint	中等大小的数据	 3字节

**int 				  标准的整数		     4字节**

 bigint 			较大的数据			 8字节

float				单精度浮点			 4字节

double			双精度浮点			 8字节

decimal		字符串形式的浮点型	（金融计算时使用）



### 字符型

char 			字符串固定大小		0-255

**varchar	可变字符串	 0-65535	常用的变量	String**

tinytext		微型文本				2^8		

**txt				文本串					2^16-1		保存大文本**



### 时间日期

date	YYYY-MM-DD	日期格式

time	HH: mm: ss		时间格式

**datetime	YYYY-MM-DD HH: mm: ss	最常用的时间格式**

**timestamp	时间戳**

year	年份表示



## 数据库的字段属性(重点)

### unsigned

1、无符号的整数

2、该声明不能为负数



### zerofill

1、0填充

2、不足的位数用0来填充

int（3） =	5		显示 00005



### 自增(auto increment) 

1、通常理解为，自动在上一条记录的基础上 + 1（默认）

2、通常用来设计唯一的主键	index 必须是整数类型

3、可以自定义设计主键自增的起始值和步长



### 非空

1、假设设置为 not null ，如果不给他赋值，就会报错

2、NULL 如果不填写值，默认值就是NULL



## 创建数据库表(重点)

目标：创建一个school数据库

创建学生列表（列，字段） 使用sql 创建

学号 int、 登入密码varchar（20）、姓名、性别varchar（2）、出生日期datatime、家庭住址、email

注意点！！！

1、必须使用英文

2、字段、名称 尽量使用``括起来

3、自增(auto increment) 

4、字符串使用，用	' ' 括起来（英文的）

5、所有的语句后面加 , （英文的），最后一个不用加

6、PRIMARY KEY(``)，一般一个表只有一个主键

```sql
CREATE TABLE IF NOT EXISTS `student`(
	`id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
	`name` VARCHAR(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
	`pwd` VARCHAR(20) NOT NULL DEFAULT '123456' COMMENT '密码',
	`sex` VARCHAR(2) NOT NULL DEFAULT '女' COMMENT '性别',
	`address` VARCHAR(100) DEFAULT NULL COMMENT '家庭住址',
	`birthday` DATETIME DEFAULT NULL COMMENT '出生日期'
	`email` VARCHAR(18) DEFAULT NULL COMMENT '邮箱',
	PRIMARY KEY(`id`)
		
)ENGINE=INNODB DEFAULT CHARSET=utf8
```

### 格式

```sql
CREATE TABLE IF NOT EXISTS `表名`(
	`字段名` 列类型 [属性] [引索] [注释],
    `字段名` 列类型 [属性] [引索] [注释],
    `字段名` 列类型 [属性] [引索] [注释],
    `字段名` 列类型 [属性] [引索] [注释],
)[表类型][字符设置][注释]
```

### 查找创建表的步骤

SHOW CREATE TABLE 表名  			展示表的建立过程代码

SHOW CREATE database 库名		展示库的建立过程代码

DESC student 		显示表的结构



## 管理数据库

### 外键

方法一

```sql
CREATE TABLE `grade`(
 `gradeid` INT(10) NOT NULL AUTO_INCREMENT COMMENT '年级id',
 `gradename` VARCHAR(50) NOT NULL COMMENT '年级名称',
 PRIMARY KEY (`gradeid`) 
)ENGINE=INNODB DEFAULT CHARSET=utf8


CREATE TABLE IF NOT EXISTS `student`(
 `id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
 `name` VARCHAR(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
 `pwd` VARCHAR(20) NOT NULL DEFAULT '123456' COMMENT '密码',
 `sex` VARCHAR(2) NOT NULL DEFAULT '女' COMMENT '性别',
 `gradeid` INT(10) NOT NULL COMMENT '学生的年级',
 `address` VARCHAR(100) DEFAULT NULL COMMENT '家庭住址',
 `birthday` DATETIME DEFAULT NULL COMMENT '出生日期',
 `email` VARCHAR(18) DEFAULT NULL COMMENT '邮箱',
 PRIMARY KEY (`id`),
  --重点
 KEY `fk_gradeid` (`gradeid`),
 CONSTRAINT `fk_gradeid` FOREIGN KEY (`gradeid`) REFERENCES `grade`(`gradeid`)

)ENGINE=INNODB DEFAULT CHARSET=utf8
```

方法二

```sql
CREATE TABLE IF NOT EXISTS `student`(
 `id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
 `name` VARCHAR(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
 `pwd` VARCHAR(20) NOT NULL DEFAULT '123456' COMMENT '密码',
 `sex` VARCHAR(2) NOT NULL DEFAULT '女' COMMENT '性别',
 `gradeid` INT(10) NOT NULL COMMENT '学生的年级',
 `address` VARCHAR(100) DEFAULT NULL COMMENT '家庭住址',
 `birthday` DATETIME DEFAULT NULL COMMENT '出生日期',
 `email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',
 PRIMARY KEY (`id`)  
)ENGINE=INNODB DEFAULT CHARSET=utf8

-- 创建表的时候没有外键关系
ALTER TABLE `student`
ADD CONSTRAINT `fk_gradeid` FOREIGN KEY(`gradeid`) REFERENCES `grade` (`gradeid`);
```



以上操作都是物理外键，数据库级别的外键，不建议使用（避免数据库过多照成困扰）

------

最佳实践

1、数据库为单纯的表，只用来存数据 

行（数据）列（字段）

2、若想使用多张表的数据，使用外键，则通过程序实现

### DML语言

数据操纵语言

1、insert

2、update

3、delete

### 添加

单个插入时

```sql
-- 插入语句（添加）
INSERT INTO `表名`(`字段名`) VALUES ('插入值')
```

多个插入时

我们输入的数据必须与字段一一对应

```sql
INSERT INTO `表名`(`字段名`) VALUES ('插入值'),('插入值'),('插入值')
```

![image-20200731152042963](C:\Users\喻嘉兴\AppData\Roaming\Typora\typora-user-images\image-20200731152042963.png)

注意事项：

1、字段与字段之间用 英文的都好 隔开

2、字段是可以省略的，但是后面的值必须要一一对应，不能少

3、可以同时插入多条数据，values后面的值，需要使用时用

(),(),()隔开



### 修改

修改单个数据

```sql
UPDATE `表名` SET `字段`='修改值' WHERE id=1;
--此条代码 给了指定条件，就是id为1号
--若无给出指定条件，则全部数据都会改变
```

修改多个数据

```sql
UPDATE `表名` SET `字段名`='修改值',`字段名`='修改值' WHERE id=1;
--where 为 指定条件
```

操作符会返回 布尔值

| 操作符           | 含义         | 范围        | 结果  |
| ---------------- | ------------ | ----------- | ----- |
| =                | 等于         | 5=6         | flase |
| !=或<>           | 不等于       | 5<>6        | true  |
| between...and... | 在某个范围内 | [x,y]闭区间 |       |
| and              | 和 &&        | 5>1 && 1>2  | flase |
| or               | 或 \|\|      | 5>1 && 1>2  | true  |

通过多个条件定位数据

```sql
UPDATE `表名` SET `字段名`='修改值' WHERE `字段名`='原先值' AND `sex`='原先值';
```



### 删除

```sql
DELETE FROM `表名` WHERE id = 1;
--删除id为1的所有数据
```

清空一个表

```sql
TRUNCATE `表名`;
```



#### TRUNCATE 与 DELETE

相同：

1、都能删除数据，但是不删除表结构

不同

1、TRUNCATE重新设计自增列，计数器归零

2、TRUNCATE不会影响事务



了解即可：**DELETE删除的问题**，重启数据库现象

1、innoDB 	自增列会从1开始 （存在内存中，断电即失）

2、MylSAM	继续从上一个自增量开始	（存在文件中，不会丢失）





## DQL查询数据（重点）

where	先关联再筛选

on		  先筛选再关联

### DQL

(Data Query LANGUAGE:数据查询语言)

1、所有的查询操作都用它 Select

2、简单的，复杂的查询它都能做

3、数据库中最核心的语言，最重要的语言

4、使用频率最高的语言

### 指定查询字段

```sql
SELECT * FROM 表名
* 为表示全部字段sql
```

### 查询指定字段

```sql
SELECT `字段`,`字段`FROM `表名`
```

### 别名

可以给字段起别名，也可以给表起别名

```sql
SELECT `字段` as 通俗名称,`字段` as 通俗名称
FROM `表名`
```

### 函数

```sql
SELECT CONCAT('姓名：',字段名) AS 别名 FROM student
```

![image-20200731235026691](C:\Users\喻嘉兴\AppData\Roaming\Typora\typora-user-images\image-20200731235026691.png)



 

### 去重DISTINCT

去除select查询结果中重复的数据

```sql
SELECT DISTINCT `字段名` FROM 表名
```

DQL 

```sql
SELECT VERSION()	-- 查询系统版本
SELECT 100*3-1 AS 计算结果	-- 计算
SELECT @@auto_increment_increment	-- 查询自增的步长
SELECT `subjectno`+ 1 FROM result	-- 所有学生学号加1
```



### where条件子句

在result中查找分数95到100的学生

方法1、

```sql
SELECT `studentno`,`studentresult`FROM`result`
WHERE `studentresult`>= 95 AND `studentresult`<=100
```

方法二、

between左侧值必须小于右侧值

```sql
SELECT `studentno`,`studentresult`FROM`result`
WHERE `studentresult` BETWEEN 95 AND 100
```





查询学号不为1000的学生的成绩

方法一、

```sql
SELECT `studentno`,`studentresult` FROM `result`
WHERE `studentno` != 1000
```

方法二、

```sql
SELECT `studentno`,`studentresult` FROM `result`
WHERE NOT `studentno` = 1000
```

### 模糊查询（重点）

| 操作符         | 语法                      | 描述                                           |
| -------------- | ------------------------- | ---------------------------------------------- |
| is null        | a is null                 | 如果操作符为null，结果为真                     |
| is not null    | a is not null             | 如果操作符不为null，结果为真                   |
| between        | a between b and c         | 如果啊在b与c之间，结果为真                     |
| like（模糊）   | a like b                  | DQL匹配，如果a匹配b，结果为真                  |
| in（精准查询） | a in （a1，a2，a3......） | 假设a在a1，或者a2...其中的某一个值中，结果为真 |



#### like实例（模糊）

_ 下划线		代表一个字符（可用多个 _ ）

% 百分号	   代表0到任意个字符

```sql
SELECT `subjectno`,`subjectname` FROM `subject`
WHERE `subjectname` LIKE 'C%'
-- 查询名称为 CXXXX 的科目
```

```sql
SELECT `subjectno`,`subjectname` FROM `subject`
WHERE `subjectname` LIKE 'C_'
-- 查询名称为 CX且名称为总长为2位的的科目
```

```sql
SELECT `subjectno`,`subjectname` FROM `subject`
WHERE `subjectname` LIKE '%数%'
-- 查询科目名称中有‘数’这个字的科目（关键词查询）
```



#### in实例（精准）

在`subject`中查找 classhour为110和100的subject

```sql
SELECT `subjectno`,`subjectname`,`classhour` FROM `subject`
WHERE `classhour` IN ('110','100')
```



#### is null实例

```sql
SELECT `subjectno`,`subjectname`,`classhour` FROM `subject`
WHERE `classhour` IS  NULL
--查询classhour为空的
```



#### is not null实例

```sql
SELECT `subjectno`,`subjectname`,`classhour` FROM `subject`
WHERE `classhour` IS NOT NULL
```





## 连表查询join（重点）

思路

1、需求分析，分析查询的字段来自哪些表

2、确定使用哪种连接查询（7种，如下图）

3、确定交叉点（两个表中哪个字段相同）

4、判断条件，A表中的 字段 = B表中的字段 时

![img](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1596273289090&di=a4d4683ba5b455b04a5b3af05ec79727&imgtype=0&src=http%3A%2F%2Fattach.dataguru.cn%2Fattachments%2Fforum%2F201304%2F09%2F210610grkvqb0i0rb3tobc.jpg)



#### 联两表查询

```sql
SELECT s.`studentno`,`studentname`,`studentresult`
FROM`student` AS s
INNER JOIN result AS r
WHERE s.`studentno` = r.`studentno`
```



#### 联三表查询

```sql
SELECT s.`studentno`,`studentname`,`studentresult`,`subjectname`
FROM`student` AS s
RIGHT JOIN result AS r
ON r.`studentno` = s.`studentno`
INNER JOIN `subject` AS sub
ON r.`subjectno` = sub.`subjectno`
```



| 操作符     | 描述                                 |
| ---------- | ------------------------------------ |
| inner join | 如果表中至少有一个匹配，就返回       |
| left join  | 以左表为主表，到右表中补上需要的数据 |
| right join | 以右表为主表，到左表中补上需要的数据 |

### 自连接(了解)

自己的表和自己的表连接，核心：**一张表拆为两张一样的表即可**

父类

| categoryid | categoryName |
| ---------- | ------------ |
| 2          | 信息技术     |
| 3          | 软件开发     |
| 5          | 美术设计     |

子类

| pid  | categoryid | categoryName |
| ---- | ---------- | ------------ |
| 3    | 4          | 数据库       |
| 2    | 8          | 办公信息     |
| 3    | 6          | web开发      |
| 5    | 7          | 美术设计     |



操作：查询父类对应的子类关系

| 父类     | 子类     |
| -------- | -------- |
| 信息技术 | 办公信息 |
| 软件开发 | 数据库   |
| 软件开发 | web开发  |
| 美术设计 | 美术设计 |

```sql
SELECT A.`categoryName` AS '父类',B.`categoryName` AS '子类'
FROM `category`AS A,`category` AS B
WHERE A.`categoryid` = B.`pid`
```



### 查询结果排序与分页

排序

```sql
ORDER BY `studentno` DESC			从大到小
ORDER BY `studentno` ASC			从小到大
```

分页

```sql
LIMIT 3,2
LIMIT 第几页，一页中显示的数量
```



实例

-- 查询 高等数学-2 课程成绩前十的学生，并且分数要大于80的学生信息
-- （学号，姓名，课程名称，分数）

```sql
SELECT s.`studentno`,`studentname`,`subjectname`,`studentresult`
FROM `student` AS s
INNER JOIN `result` AS r
ON s.`studentno` = r.`studentno`
INNER JOIN `subject` AS sub
ON sub.`subjectno` = r.`subjectno`
WHERE sub.`subjectname` = '高等数学-2' AND `studentresult` >= 70
ORDER BY r.`studentresult` DESC
LIMIT 0,10
```

### 子查询

where(这个是可以计算出来的)

本质：**在where语句中嵌套一个查询语句**

where(select * from)





1、查询数据库结构-1 的所有考试结果
（学号，科目编号，成绩），降序排列
方法一：使用连接查询

```sql
SELECT r.`studentno`,sub.`subjectname`,sub.`subjectno`,`studentresult`
FROM `result` AS r
INNER JOIN `subject` AS sub
ON r.`subjectno` = sub.`subjectno`
WHERE `subjectname` = '高等数学-1'
ORDER BY `studentresult` DESC
```

-- 方法二： 使用子查询（）

```sql
SELECT `studentno`,`subjectno`,`studentresult`
FROM `result`
WHERE `subjectno` = (
	SELECT `subjectno` FROM `subject` WHERE `subjectname` = '高等数学-1'
)
```

由里即外的子查询（嵌套）

```sql
SELECT `studentno`,`studentname` FROM `student` WHERE `studentno` IN(
	SELECT `studentno` FROM `result` WHERE `studentresult` >= 10 AND `studentno` = (
		SELECT `subjectno` FROM `subject` WHERE `subjectname` = '高等数学-1'
	)
)
```



### 分组与过滤

查询不同课程的平均分，最高峰，最低分，且平均大于60

```sql
SELECT `subjectname`,AVG(`studentresult`) AS 平均分,MAX(`studentresult`) AS 最高分,MIN(`studentresult`) AS 最低分
FROM `result` AS r
INNER JOIN `subject` AS sub
ON r.`subjectno` = sub.`subjectno`
GROUP BY r.`subjectno`	-- 通过什么字段来分组
HAVING 平均分 >= 60
```

## MySQL函数

### 常用函数

```sql
-- 数学运算

SELECT ABS(-99)		-- 求绝对值
SELECT CEILING(9.4)	-- 向下取整
SELECT FLOOR(9.7)	-- 向上取整
SELECT RAND()		-- 返回一个 0~1 之间的随机数
SELECT SIGN(9)		-- 正数返回1	负数返回-1	0返回0

-- 字符串
SELECT CHAR_LENGTH('naifniewifwig')	-- 求字符串长度
SELECT CONCAT('大','家','好','啊！')	-- 拼接字符串	
SELECT INSERT('我是喻嘉兴',2,1,'叫')	-- 查询，替换 (起始位置，替换长度)
SELECT LOWER('NdnsnkNDnkd')	-- 全部转化为小写
SELECT UPPER('NdnsnkNDnkd')	-- 全部转化为大写
SELECT INSTR('hiunini','n')	-- 返回第一次出现字符的索引
SELECT REPLACE('喻嘉兴说希望大家都很好的','很好的','好好的')
-- 替换指定出现的字符
SELECT SUBSTR('喻嘉兴说希望大家都很好的',1,3) -- 返回指定的子字符串（截取位置，截取长度）
SELECT REVERSE('喻嘉兴说希望大家都很好的')    -- 字符串反转

-- 时间
SELECT CURRENT_DATE()		-- 获取当前日期
SELECT CURDATE()		-- 获取当前日期
SELECT CURRENT_TIME()		-- 获取当前时间
SELECT NOW()			-- 获取当前详细时间
SELECT LOCALTIME()		-- 获取本地时间
SELECT SYSDATE()		-- 获取系统时间 

-- 系统
SELECT SYSTEM_USER()
SELECT USER()
SELECT VERSION()
```




### 聚合函数（常用）

| 函数名称 | 描述   |
| -------- | ------ |
| count    | 计数   |
| sum      | 求和   |
| avg      | 平均再 |
| max      | 最大值 |
| min      | 最小值 |
| ...      | ...    |

```sql
SELECT COUNT(`studentname`) FROM `student`	-- count（指定列） 会忽略所有的 null
SELECT COUNT(*) FROM `student`			-- count（*）	不忽略null
SELECT COUNT(1) FROM `result`			-- count（1）	不忽略null

SELECT SUM(`studentresult`) AS 总分 FROM `result`
SELECT AVG(`studentresult`) AS 平均分 FROM `result`
SELECT MAX(`studentresult`) AS 最大值 FROM `result`
SELECT MIN(`studentresult`) AS 最小值 FROM `result`
```



### 数据库级别的MD5加密（扩展）

主要增强算法复杂程度和不可逆性。

MD5不可逆，具体的值md5是一样的

MD5破解网站的原理，背后有一个字典，md5加密后的值是固定的。

```sql
CREATE TABLE `testmd5`(
	`id` INT(4) NOT NULL,
	`name` VARCHAR(20) NOT NULL,
	`pwd` VARCHAR(50) NOT NULL,
	PRIMARY KEY(`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8

-- 明文密码
INSERT INTO testmd5 VALUES(1,'喻嘉兴','010412'),(2,'喻嘉gf兴','010412fh'),(3,'喻gfd嘉兴','010ef412')

-- 利用md5加密
UPDATE testmd5 SET pwd=MD5(pwd) 

-- 插入的时候加密
INSERT INTO testmd5 VALUES(4,'nof',MD5('010412'))

如何校验：将用户传递进来的密码，进行md5加密，然后对比加密后的值
SELECT * FROM testmd5 WHERE `name` = 'nof' AND pwd=MD5('010412')
```







## 事务

### 什么是事务

要么成功，要么失败

------------------

1、sql执行	A给B转账	A 1000  ----> 200	B 200

2、 sql执行	B收到A的钱	A 800 ----> B 400

----------------------

将一组sql放在一个批次中去执行



事务原则： ACDI原则  原子性	一致性	隔离性	持久性 （脏读，幻读）



**原子性（Atomicity）**

要么成功，要么失败

**一致性（Consistency**

事务数据完整性前后保持一致

**隔离性（Isolation)**

事务的隔离性是多个用户并发访问数据库时，数据库为每一个用户开启的事务，不能被其他事务的操作数据所干扰，多个并发事务之间要相互隔离。

**持久性（Durability）**

持久性是指一个事务一旦被提交，它对数据库中数据的改变就是永久性的



### 执行事务

```sql
-- MySQL 是默认开启事务自动提交的
SET autocommit = 0	-- 事务关闭
SET autocommit = 1	-- 事务开启（默认）

-- 手动处理事务
SET autocommit = 0	-- 关闭自动提交


-- 事务开启
START TRANSACTION -- 标记一个事务的开始，从这个之后的sql都在同一个事务中

INSERT
INSERT

-- 提交：持久化（成功）
COMMIT

-- 回滚：回到原来的样子（失败）
ROLLBACK

-- 事务结束
SET autocommit = 1	-- 开启自动提交


-- 了解
SAVEPOINT 保存点名	-- 设置一个事务的保存点
ROLLBACK TO SAVEPOINT 保存点名	-- 回滚到保存点
RELEASE SAVEPOINT 保存点名	-- 撤销保存点
```



模拟事务：转账

```sql
SET autocommit = 0	-- 关闭自动提交

START TRANSACTION	-- 事务开启

UPDATE `account` SET `money`=`money`-500 WHERE `name`='A'
UPDATE `account` SET `money`=`money`+500 WHERE `name`='B'

COMMIT;		-- 提交事务
ROLLBACK;	-- 回滚，回到原先的数据

SET autocommit = 1;	-- 开启自动提交
```





## 索引

​	MySQL索引的建立对于MySQL的高效运行是很重要的，索引可以大大提高MySQL的检索速度。索引是数据结构

### 索引的分类

主键索引（PRIMARY KEY）

​	唯一的标识，不可重复，只要能有一个列作为主键

唯一索引（UNIQUE KEY）

​	避免出现重复的列，索引可以重复

常规索引（KEY/INDEX）

​	默认的，index，key关键字设置

全文索引（FullText）

​	在特定的数据库引擎才有，MylSAN



```sql
-- 索引的使用
-- 1、在创建表时，给字段增加索引
-- 2、创建完毕后，增加索引

-- 显示所有的索引信息

SHOW INDEX FROM `student`

-- 增加一个全文索引
ALTER TABLE `school`.`student` ADD FULLTEXT INDEX `studentname`(`studentname`)


-- explain 分析sql的执行状况
EXPLAIN SELECT * FROM `student`;	-- 非全文索引

EXPLAIN SELECT * FROM `student` WHERE MATCH(`studentname`) AGAINST('张');
```



### 测试索引

插入一百万条数据

```sql
-- 插入100万数据.
DELIMITER $$
-- 写函数之前必须要写，标志
CREATE FUNCTION mock_data()
RETURNS INT
DETERMINISTIC;
BEGIN
	DECLARE num INT DEFAULT 1000000;
	DECLARE i INT DEFAULT 0;
	
	WHILE i < num DO
	INSERT INTO `app_user`(`name`,`email`,`phone`,`gender`,`password`,`age`);
	VALUES (CONCAT('用户',i),'19224305@qq.com',CONCAT('18',FLOOR(RAND()*((999999999-100000000)+100000000))),FLOOR(RAND()*2),UUID(),FLOOR(RAND()*100));
	SET i = i + 1;
END WHILE;
RETURN i;
DETERMINISTIC;
END;

SELECT mock_data(); -- 执行此函数 生成一百万条数据
```

添加索引

```sql
-- id _表名_字段名
-- CREATE INDEX 索引名 on 表（字段）
CREATE INDEX id_`app_user`_name ON `app_user`(`name`)
-- 为每一个数据的name都增加上索引
```





### 索引原则

1、索引不是越多越好

2、不要对经常变动的数据加索引

3、小数据量时不需要增加索引

4、索引一般加在常用来查询的字段上



> 索引的数据结构 

```sql
-- 我们可以在创建上述索引的时候，为其指定索引类型，分两类
hash类型的索引：查询单条快，范围查询慢
btree类型的索引：b+树，层数越多，数据量指数级增长（我们就用它，因为innodb默认支持它）
 
-- 不同的存储引擎支持的索引类型也不一样
InnoDB 支持事务，支持行级别锁定，支持 B-tree、Full-text 等索引，不支持 Hash 索引；
MyISAM 不支持事务，支持表级别锁定，支持 B-tree、Full-text 等索引，不支持 Hash 索引；
Memory 不支持事务，支持表级别锁定，支持 B-tree、Hash 等索引，不支持 Full-text 索引；
NDB 支持事务，支持行级别锁定，支持 Hash 索引，不支持 B-tree、Full-text 等索引；
Archive 不支持事务，支持表级别锁定，不支持 B-tree、Hash、Full-text 等索引；
```







## 权限管理和备份

### 用户管理

> 使用SQLyog 创建用户，并授予权限演示

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy91SkRBVUtyR0M3SmY3ZGVvbHdRYTQ0clh2aWNJaFhaME5HTDRzWktnOG5pY0JHcllsRUJKaDFWM3ltSjRXekJ4OXpYc0laeVBZRkFESkJ6bjBpYkNtZ2lhdUEvNjQw?x-oss-process=image/format,png)



> 基本命令

```sql
-- 创建用户 
CREATE USER yjx	IDENTIFIED BY '123456'

-- 修改当前用户密码
SET PASSWORD = PASSWORD('123456')

-- 修改指定用户密码
SET PASSWORD FOR yjx = PASSWORD('000000')

-- 重命名
-- RENAME USER 原名称 to 新名称
RENAME USER yjx TO yjx2

-- 授予用户全部的权限
-- 除了给别人授权，都能做
GRANT ALL PRIVILEGES ON *.* TO 用户名

-- 撤消权限
REVOKE 权限列表 ON 表名 FROM 用户名
 -- 撤销所有权限
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 用户名   

-- 查询指定用户权限
SHOW GRANTS FOR yjx2

-- 查询 root用户权限
SHOW GRANTS FOR root@localhost

-- 刷新权限
FLUSH PRIVILEGES

-- 删除用户 
DROP USER 用户名
```



### mysql备份

为什么要备份：

- 保证重要数据不丢失
- 数据转移

MySQL数据库备份的方式

- 直接拷贝物理文件
- 在可视化工具中手动导出
- 使用命令行导出 mysqldump 命令行使用

**mysqldump客户端**

作用 :

- 转储数据库
- 搜集数据库进行备份
- 将数据转移到另一个SQL服务器,不一定是MySQL服务器



**mysqldump的导出**

```sql
-- 导出
1. 导出一张表 -- mysqldump -uroot -p123456 school student >D:/a.sql
　　mysqldump -u用户名 -p密码 库名 表名 > 文件名(D:/a.sql)
2. 导出多张表 -- mysqldump -uroot -p123456 school student result >D:/a.sql
　　mysqldump -u用户名 -p密码 库名 表1 表2 表3 > 文件名(D:/a.sql)
3. 导出所有表 -- mysqldump -uroot -p123456 school >D:/a.sql
　　mysqldump -u用户名 -p密码 库名 > 文件名(D:/a.sql)
4. 导出一个库 -- mysqldump -uroot -p123456 -B school >D:/a.sql
　　mysqldump -u用户名 -p密码 -B 库名 > 文件名(D:/a.sql)
 
可以-w携带备份条件
 
-- 导入
1. 在登录mysql的情况下：-- source D:/a.sql
　　source  备份文件
2. 在不登录的情况下
　　mysql -u用户名 -p密码 库名 < 备份文件
```



## 规范数据库设计

### 为什么需要数据库设计

**当数据库比较复杂时我们需要设计数据库**

**糟糕的数据库设计 :**

- 数据冗余,存储空间浪费
- 数据更新和插入的异常
- 程序性能差

**良好的数据库设计 :**

- 节省数据的存储空间
- 能够保证数据的完整性
- 方便进行数据库应用系统的开发

**软件项目开发周期中数据库设计 :**

- 需求分析阶段: 分析客户的业务和数据处理需求
- 概要设计阶段:设计数据库的E-R模型图 , 确认需求信息的正确和完整.

**设计数据库步骤**

- 收集信息
  - 与该系统有关人员进行交流 , 座谈 , 充分了解用户需求 , 理解数据库需要完成的任务.
- 标识实体[Entity]
- - 标识数据库要管理的关键对象或实体,实体一般是名词
- 标识每个实体需要存储的详细信息[Attribute]
- 标识实体之间的关系[Relationship]

**设计数据库步骤(个人博客)**

- 收集信息，需求分析
  - 用户表（用户登入注销，用户个人信息，写博客，创建分类）
  - 分类表（文章分类，谁创建）
  - 文章表（文章的信息）
  - 友链表（友链信息）
  - 自定义表（系统信息，某个关键的字，或者一些主字段） key：value

- 标识实体（把需求落到每个字段）

### 三大范式

**问题 : 为什么需要数据规范化?**

不合规范的表设计会导致的问题：

- 信息重复
- 更新异常
- 插入异常
  - 无法正确表示信息
- 删除异常
  - 丢失有效信息

> 三大范式

**第一范式 (1st NF)**		

第一范式的目标是确保每列的原子性,如果每列都是不可再分的最小数据单元,则满足第一范式

**第二范式(2nd NF)**

第二范式（2NF）是在第一范式（1NF）的基础上建立起来的，即满足第二范式（2NF）必须先满足第一范式（1NF）。

第二范式要求每个表只描述一件事情

**第三范式(3rd NF)**

如果一个关系满足第二范式,并且除了主键以外的其他列都不传递依赖于主键列,则满足第三范式.

第三范式需要确保数据表中的每一列数据都和主键直接相关，而不能间接相关。

**规范化和性能的关系**

关联查询的表不得超过三张表

- 为满足某种商业目标 , 数据库性能比规范化数据库更重要

- 在数据规范化的同时 , 要综合考虑数据库的性能

- 通过在给定的表中添加额外的字段,以大量减少需要从中搜索信息所需的时间

- 通过在给定的表中插入计算列,以方便查询



