---
title: "Node.JS基础(4) - 数据库"
date: 2020-06-16 14:29:14
draft: false
tags: ['node', '基础']
categories: ['Node']
---

> 数据库备份：[coderlch.sql](https://www.yuque.com/attachments/yuque/0/2021/sql/714353/1632554274108-fcefb665-ff97-4369-a064-16fc19500d39.sql?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fsql%2F714353%2F1632554274108-fcefb665-ff97-4369-a064-16fc19500d39.sql%22%2C%22name%22%3A%22coderlch.sql%22%2C%22size%22%3A30853%2C%22type%22%3A%22%22%2C%22ext%22%3A%22sql%22%2C%22status%22%3A%22done%22%2C%22taskId%22%3A%22ude23818f-6b2a-4472-9325-930bdf27761%22%2C%22taskType%22%3A%22upload%22%2C%22id%22%3A%22u3dade9bc%22%2C%22card%22%3A%22file%22%7D)

## 数据库
为什么使用数据库？数据直接存储在文件系统中管理不方便，使用数据库软件能更好地操作数据。

在终端使用命令是操作数据库的一种方式，但很不方便，因此可利用一些图形化软件来操作数据库，如Navicat。

操作数据库就需要有和数据库沟通的代码语言，称作sql语句。常见的关系型数据库的sql语句是相似的，学会一个其它的也很简单。

数据库就是一个软件。
### 常见的数据库

- 关系型数据库：MySQL、Oracle、DB2、SQL Server、Postgre SQL等
   - 关系型数据库通常我们会创建很多个二维数据表；
   - 数据表之间相互关联起来，形成一对一、一对多、多对对等关系；
   - 之后可以利用SQL语句在多张表中查询我们所需的数据；
   - 支持事物，对数据的访问更加的安全；
- 非关系型数据库：MongoDB、Redis、Memcached、HBse等
   - 非关系型数据库的英文其实是Not only SQL，也简称为NoSQL；
   - 相当而已非关系型数据库比较简单一些，存储数据也会更加自由（甚至我们可以直接将一个复杂的json对象直接塞入到数据库中）；
   - NoSQL是基于Key-Value的对应关系，并且查询的过程中不需要经过SQL解析，所以性能更高；
   - NoSQL通常不支持事物，需要在自己的程序中来保证一些原子性的操作；

### 表的约束

- 主键`PRIMARY KEY`：为了区分每一条记录的唯一性。要求该字段不会重复且不为空：
   - 主键是表中唯一的、不会重复的索引；
   - 为NOT NULL
   - 可以是多列索引，称之为联合主键；
   - 建议主键字段和业务无关，尽量不使用业务字段作为主键；
- 唯一`UNIQUE`：唯一的、不会重复的字段，可用UNIQUE进行约束；
- 不能为空`NOT NULL`
- 默认值`DEFAULT`： 某字段没有设置值时给予一个默认值；
- 自动递增`AUTO_INCREMENT`
- 外键约束

## 数据库的操作语句
### 对数据表的操作DDL
```sql
# 查看所有的表
SHOW TABLES;

# 新建表
CREATE TABLE IF NOT EXISTS `students` (
	`name` VARCHAR(10) NOT NULL,
	`age` int,
	`score` int,
	`height` DECIMAL(10,2),
	`birthday` YEAR,
	`phoneNum` VARCHAR(20) UNIQUE
);

# 删除表
DROP TABLE IF EXISTS `moment`;

# 查看表的结构
DESC students;
# 查看创建表的SQL语句
SHOW CREATE TABLE `students`;
-- CREATE TABLE `students` (
--   `name` varchar(10) DEFAULT NULL,
--   `age` int DEFAULT NULL,
--   `score` int DEFAULT NULL
-- ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci

# 完整的创建表的语法
CREATE TABLE IF NOT EXISTS `users`(
	id INT PRIMARY KEY AUTO_INCREMENT,
	name VARCHAR(20) NOT NULL,
	age INT DEFAULT 0,
	phoneNum VARCHAR(20) UNIQUE DEFAULT '',
	createTime TIMESTAMP
);

# 修改表
# 1.修改表的名字
ALTER TABLE `users` RENAME TO `user`;
# 2.添加一个新的列
ALTER TABLE `user` ADD `updateTime` TIMESTAMP;
# 3.修改字段的名称
ALTER TABLE `user` CHANGE `phoneNum` `telPhone` VARCHAR(20);
# 4.修改字段的类型
ALTER TABLE `user` MODIFY `name` VARCHAR(30);
# 5.删除某一个字段
ALTER TABLE `user` DROP `age`;

# 补充
# 根据一个表结构去创建另外一张表
CREATE TABLE `user2` LIKE `user`;
# 根据另外一个表中的所有内容，创建一个新的表
CREATE TABLE `user3` (SELECT * FROM `user`); 
```
### 对数据库的操作DDL
```sql
# 查看所有的数据库
SHOW DATABASES;

# 选择某一个数据库
USE bili;

# 查看当前正在使用的数据库
SELECT DATABASE();

# 创建一个新的数据库
-- CREATE DATABASE douyu;
-- CREATE DATABASE IF NOT EXISTS douyu;
-- CREATE DATABASE IF NOT EXISTS huya DEFAULT CHARACTER SET utf8mb4
-- 				COLLATE utf8mb4_0900_ai_ci;

# 删除数据库
DROP DATABASE IF EXISTS douyu;

# 修改数据库的编码
ALTER DATABASE huya CHARACTER SET = utf8 
				COLLATE = utf8_unicode_ci;
```
### 对数据库的增删改DML
```sql
# DML
# 插入数据
INSERT INTO `user` VALUES (110, 'why', '020-110110', '2020-10-20', '2020-11-11');
INSERT INTO `user` (name, telPhone, createTime, updateTime)
						VALUES ('kobe', '000-1111111', '2020-10-10', '2030-10-20');
						
INSERT INTO `user` (name, telPhone)
						VALUES ('lilei', '000-1111112');

# 需求：createTime和updateTime可以自动设置值
ALTER TABLE `user` MODIFY `createTime` TIMESTAMP DEFAULT CURRENT_TIMESTAMP;
ALTER TABLE `user` MODIFY `updateTime` TIMESTAMP DEFAULT CURRENT_TIMESTAMP					
																			 ON UPDATE CURRENT_TIMESTAMP;

INSERT INTO `user` (name, telPhone)
						VALUES ('hmm', '000-1111212');

INSERT INTO `user` (name, telPhone)
						VALUES ('lucy', '000-1121212');


# 删除数据
# 删除所有的数据
DELETE FROM `user`;
# 删除符合条件的数据
DELETE FROM `user` WHERE id = 110;

# 更新数据
# 更新所有的数据
UPDATE `user` SET name = 'lily', telPhone = '010-110110';
# 更新符合条件的数据
UPDATE `user` SET name = 'lily', telPhone = '010-110110' WHERE id = 115;
```
### 聚合函数 与 GroupBy
顾名思义，聚合的意思是将查询结果看作一组“数组”，该数组经过了某个函数的处理后，返回的结果；
若想将查询结果分成好多个数组，每个数组都可经过函数处理，可以使用Group by；
举例：使用聚合函数求解所有手机的平均价格，再配合group by 可分别求解不同品牌的手机平均价格；
还希望筛选出平均价格在4000以下，并且平均分在7以上的品牌，可使用HAVING
```sql
# 1.聚合函数的使用
# 求所有手机的价格的总和
SELECT SUM(price) totalPrice FROM `products`;
# 求一下华为手机的价格的总和
SELECT SUM(price) FROM `products` WHERE brand = '华为';
# 求华为手机的平均价格
SELECT AVG(price) FROM `products` WHERE brand = '华为';
# 最高手机的价格和最低手机的价格
SELECT MAX(price) FROM `products`;
SELECT MIN(price) FROM `products`;

# 求华为手机的个数
SELECT COUNT(*) FROM `products` WHERE brand = '华为';
SELECT COUNT(*) FROM `products` WHERE brand = '苹果';
SELECT COUNT(url) FROM `products` WHERE brand = '苹果';

SELECT COUNT(price) FROM `products`;
SELECT COUNT(DISTINCT price) FROM `products`;

# 2.GROUP BY的使用
SELECT brand, AVG(price), COUNT(*), AVG(score) FROM `products` GROUP BY brand;


# 3.HAVING的使用，可对结果进行约束后再返回
SELECT brand, AVG(price) avgPrice, COUNT(*), AVG(score) FROM `products` GROUP BY brand HAVING avgPrice > 2000;


# 4.需求：求评分score > 7.5的手机的，平均价格是多少？
# 升级：平均分大于7.5的手机，按照品牌进行分类，求出平均价格。
SELECT brand, AVG(price) FROM `products` WHERE score > 7.5 GROUP BY brand;
```
### 数据的查询语句
```sql
# 创建products的表
CREATE TABLE IF NOT EXISTS `products` (
	id INT PRIMARY KEY AUTO_INCREMENT,
	brand VARCHAR(20),
	title VARCHAR(100) NOT NULL,
	price DOUBLE NOT NULL,
	score DECIMAL(2,1),
	voteCnt INT,
	url VARCHAR(100),
	pid INT
);

# 1.基本查询
# 查询表中所有的字段以及所有的数据
SELECT * FROM `products`;
# 查询指定的字段
SELECT title, price FROM `products`;
# 对字段结果起一个别名
SELECT title as phoneTitle, price as currentPrice FROM `products`;


# 2.where条件
# 2.1. 条件判断语句
# 案例：查询价格小于1000的手机
SELECT title, price FROM `products` WHERE price < 1000;
# 案例二：价格等于999的手机
SELECT * FROM `products` WHERE price = 999;
# 案例三：价格不等于999的手机
SELECT * FROM `products` WHERE price != 999;
SELECT * FROM `products` WHERE price <> 999;
# 案例四：查询品牌是华为的手机
SELECT * FROM `products` WHERE brand = '华为';

# 2.2. 逻辑运算语句
# 案例一：查询1000到2000之间的手机
SELECT * FROM `products` WHERE price > 1000 AND price < 2000;
SELECT * FROM `products` WHERE price > 1000 && price < 2000;
# BETWEEN AND 包含等于
SELECT * FROM `products` WHERE price BETWEEN 1099 AND 2000;

# 案例二：价格在5000以上或者是品牌是华为的手机
SELECT * FROM `products` WHERE price > 5000 || brand = '华为';

# 将某些值设置为NULL
-- UPDATE `products` SET url = NULL WHERE id >= 85 and id <= 88;
# 查询某一个值为NULL
SELECT * FROM `products` WHERE url IS NULL;
-- SELECT * FROM `products` WHERE url = NULL;
-- SELECT * FROM `products` WHERE url IS NOT NULL;

# 2.3.模糊查询
SELECT * FROM `products` WHERE title LIKE '%M%';
SELECT * FROM `products` WHERE title LIKE '%P%';
SELECT * FROM `products` WHERE title LIKE '_P%';


# 3.对结果进行排序
SELECT * FROM `products` WHERE brand = '华为' || brand = '小米' || brand = '苹果';
SELECT * FROM `products` WHERE brand IN ('华为', '小米', '苹果') 
												 ORDER BY price ASC, score DESC;


# 4.分页查询
# LIMIT limit OFFSET offset;
# Limit offset, limit;
SELECT * FROM `products` LIMIT 20 OFFSET 0;
SELECT * FROM `products` LIMIT 20 OFFSET 20;
SELECT * FROM `products` LIMIT 40, 20;
```
### 对象和数组类型
```sql
# 将联合查询到的数据转成对象（一对多）
SELECT 
	products.id id, products.title title, products.price price,
	JSON_OBJECT('id', brand.id, 'name', brand.name, 'website', brand.website) brand
FROM `products`
LEFT JOIN `brand` ON products.brand_id = brand.id;

# 将查询到的多条数据，组织成对象，放入到一个数组中(多对多)
SELECT 
	stu.id, stu.name, stu.age,
	JSON_ARRAYAGG(JSON_OBJECT('id', cs.id, 'name', cs.name, 'price', cs.price))
FROM `students` stu
JOIN `students_select_courses` ssc ON stu.id = ssc.student_id
JOIN `courses` cs ON ssc.course_id = cs.id
GROUP BY stu.id;

SELECT * FROM products WHERE price > 6000;
```
### 多表的设计 - 查询
手机的信息存于product表，品牌的信息存于brand表，两张表之间存在外键关联，即brand_id -- id，此时项查询手机的信息，且信息中包含品牌的信息，涉及到多张表的查询，就是多表查询。
默认获取到的是笛卡尔乘积，即product_length * brand_length，需要根据外键用where进行筛选。

除此之外，多表之间存在多种连接关系：左连接(2)、右连接(2)、内连接(1)、全连接(2)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1626939603644-bce9a02b-9120-48fa-9866-d641562a14e8.png)


```sql
# 1.获取到的是笛卡尔乘积
SELECT * FROM `products`, `brand`;
# 获取到的是笛卡尔乘积进行筛选(与内连接结果一样，但含义不同）
SELECT * FROM `products`, `brand` WHERE products.brand_id = brand.id;

# 2.左连接(开发中最常用)
# 2.1. 查询所有的手机（包括没有品牌信息的手机）以及对应的品牌 null
SELECT * FROM `products` LEFT OUTER JOIN `brand` ON products.brand_id = brand.id;

# 2.2. 查询没有对应品牌数据的手机
SELECT * FROM `products` LEFT JOIN `brand` ON products.brand_id = brand.id WHERE brand.id IS NULL;
-- SELECT * FROM `products` LEFT JOIN `brand` ON products.brand_id = brand.id WHERE brand_id IS NULL;


# 3.右连接
# 3.1. 查询所有的品牌（没有对应的手机数据，品牌也显示）以及对应的手机数据；
SELECT * FROM `products` RIGHT OUTER JOIN `brand` ON products.brand_id = brand.id;

# 3.2. 查询没有对应手机的品牌信息
SELECT * FROM `products` RIGHT JOIN `brand` ON products.brand_id = brand.id WHERE products.brand_id IS NULL;


# 4.内连接
SELECT * FROM `products` JOIN `brand` ON products.brand_id = brand.id;
SELECT * FROM `products` JOIN `brand` ON products.brand_id = brand.id WHERE price = 8699;

# 5.全连接
# mysql是不支持FULL OUTER JOIN
SELECT * FROM `products` FULL OUTER JOIN `brand` ON products.brand_id = brand.id;


(SELECT * FROM `products` LEFT OUTER JOIN `brand` ON products.brand_id = brand.id)
UNION
(SELECT * FROM `products` RIGHT OUTER JOIN `brand` ON products.brand_id = brand.id)

(SELECT * FROM `products` LEFT OUTER JOIN `brand` ON products.brand_id = brand.id WHERE brand.id IS NULL)
UNION
(SELECT * FROM `products` RIGHT OUTER JOIN `brand` ON products.brand_id = brand.id WHERE products.brand_id IS NULL)
```
### 多表的设计 - 外键
```sql
# 1.创建brand的表和插入数据
CREATE TABLE IF NOT EXISTS `brand`(
	id INT PRIMARY KEY AUTO_INCREMENT,
	name VARCHAR(20) NOT NULL,
	website VARCHAR(100),
	phoneRank INT
);

INSERT INTO `brand` (name, website, phoneRank) VALUES ('华为', 'www.huawei.com', 2);
INSERT INTO `brand` (name, website, phoneRank) VALUES ('苹果', 'www.apple.com', 10);
INSERT INTO `brand` (name, website, phoneRank) VALUES ('小米', 'www.mi.com', 5);
INSERT INTO `brand` (name, website, phoneRank) VALUES ('oppo', 'www.oppo.com', 12);

INSERT INTO `brand` (name, website, phoneRank) VALUES ('京东', 'www.jd.com', 8);
INSERT INTO `brand` (name, website, phoneRank) VALUES ('Google', 'www.google.com', 9);


# 2.给brand_id设置引用brand中的id的外键约束
# 添加一个brand_id字段
ALTER TABLE `products` ADD `brand_id` INT;
-- ALTER TABLE `products` DROP `brand_id`;

# 修改brand_id为外键
ALTER TABLE `products` ADD FOREIGN KEY(brand_id) REFERENCES brand(id);

# 设置brand_id的值
UPDATE `products` SET `brand_id` = 1 WHERE `brand` = '华为';
UPDATE `products` SET `brand_id` = 2 WHERE `brand` = '苹果';
UPDATE `products` SET `brand_id` = 3 WHERE `brand` = '小米';
UPDATE `products` SET `brand_id` = 4 WHERE `brand` = 'oppo';

# 3.修改和删除外键引用的id
UPDATE `brand` SET `id` = 100 WHERE `id` = 1;


# 4.修改brand_id关联外键时的action
# 4.1.获取到目前的外键的名称
SHOW CREATE TABLE `products`;


-- CREATE TABLE `products` (
--   `id` int NOT NULL AUTO_INCREMENT,
--   `brand` varchar(20) DEFAULT NULL,
--   `title` varchar(100) NOT NULL,
--   `price` double NOT NULL,
--   `score` decimal(2,1) DEFAULT NULL,
--   `voteCnt` int DEFAULT NULL,
--   `url` varchar(100) DEFAULT NULL,
--   `pid` int DEFAULT NULL,
--   `brand_id` int DEFAULT NULL,
--   PRIMARY KEY (`id`),
--   KEY `brand_id` (`brand_id`),
--   CONSTRAINT `products_ibfk_1` FOREIGN KEY (`brand_id`) REFERENCES `brand` (`id`)
-- ) ENGINE=InnoDB AUTO_INCREMENT=109 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci

# 4.2.根据名称将外键删除掉
ALTER TABLE `products` DROP FOREIGN KEY products_ibfk_1;

# 4.2.重新添加外键约束
ALTER TABLE `products` ADD FOREIGN KEY (brand_id) REFERENCES brand(id)
																									ON UPDATE CASCADE
																									ON DELETE RESTRICT;


UPDATE `brand` SET `id` = 100 WHERE `id` = 1;
```
### 多对多关系 - 设计
有一张student的表，存储学生信息；
有一张course的表，存储课程的信息；
前文所述的手机和品牌是一对一的关系，在生活中也存在着其它形式的关系，如一名学生可选择多门课程，一门课程可被多名学生选择，即存在一对多、多对一、多对多的关系。
针对这种情况，需要新建立一个表来存储多对多的关系，称作关系表，来记录两张表之间的关联。这张关系表与学生表、课程表都有关联，因此其中有student_id、course_id，且这两个id都外键关联到各自的表。
```sql
# 1.基本数据的模拟
CREATE TABLE IF NOT EXISTS students(
	id INT PRIMARY KEY AUTO_INCREMENT,
	name VARCHAR(20) NOT NULL,
	age INT
);

CREATE TABLE IF NOT EXISTS courses(
	id INT PRIMARY KEY AUTO_INCREMENT,
	name VARCHAR(20) NOT NULL,
	price DOUBLE
);

INSERT INTO `students` (name, age) VALUES('why', 18);
INSERT INTO `students` (name, age) VALUES('tom', 22);
INSERT INTO `students` (name, age) VALUES('lilei', 25);
INSERT INTO `students` (name, age) VALUES('lucy', 16);
INSERT INTO `students` (name, age) VALUES('lily', 20);

INSERT INTO `courses` (name, price) VALUES ('英语', 100);
INSERT INTO `courses` (name, price) VALUES ('语文', 666);
INSERT INTO `courses` (name, price) VALUES ('数学', 888);
INSERT INTO `courses` (name, price) VALUES ('历史', 80);
INSERT INTO `courses` (name, price) VALUES ('物理', 888);
INSERT INTO `courses` (name, price) VALUES ('地理', 333);


# 2.建立关系表
CREATE TABLE IF NOT EXISTS `students_select_courses`(
	id INT PRIMARY KEY AUTO_INCREMENT,
	student_id INT NOT NULL,
	course_id INT NOT NULL,
	FOREIGN KEY (student_id) REFERENCES students(id) ON UPDATE CASCADE,
	FOREIGN KEY (course_id) REFERENCES courses(id) ON UPDATE CASCADE
);

# 3.学生选课
# why选择了英文、数学、历史
INSERT INTO `students_select_courses` (student_id, course_id) VALUES (1, 1);
INSERT INTO `students_select_courses` (student_id, course_id) VALUES (1, 3);
INSERT INTO `students_select_courses` (student_id, course_id) VALUES (1, 4);


INSERT INTO `students_select_courses` (student_id, course_id) VALUES (3, 2);
INSERT INTO `students_select_courses` (student_id, course_id) VALUES (3, 4);


INSERT INTO `students_select_courses` (student_id, course_id) VALUES (5, 2);
INSERT INTO `students_select_courses` (student_id, course_id) VALUES (5, 3);
INSERT INTO `students_select_courses` (student_id, course_id) VALUES (5, 4);


# 4.查询的需求
# 4.1. 查询所有有选课的学生，选择了哪些课程
SELECT stu.id id, stu.name stuName, stu.age stuAge, cs.id csId, cs.name csName, cs.price csPrice
FROM `students` stu
JOIN `students_select_courses` ssc ON stu.id = ssc.student_id
JOIN `courses` cs ON ssc.course_id = cs.id;


# 4.2. 查询所有的学生的选课情况
SELECT stu.id id, stu.name stuName, stu.age stuAge, cs.id csId, cs.name csName, cs.price csPrice
FROM `students` stu
LEFT JOIN `students_select_courses` ssc ON stu.id = ssc.student_id
LEFT JOIN `courses` cs ON cs.id = ssc.course_id 

# 4.3. 哪些学生是没有选课
SELECT stu.id id, stu.name stuName, stu.age stuAge, cs.id csId, cs.name csName, cs.price csPrice
FROM `students` stu
LEFT JOIN `students_select_courses` ssc ON stu.id = ssc.student_id
LEFT JOIN `courses` cs ON ssc.course_id = cs.id
WHERE cs.id IS NULL;

# 4.4. 查询哪些课程是没有被选择的
SELECT stu.id id, stu.name stuName, stu.age stuAge, cs.id csId, cs.name csName, cs.price csPrice
FROM `students` stu
RIGHT JOIN `students_select_courses` ssc ON stu.id = ssc.student_id
RIGHT JOIN `courses` cs ON ssc.course_id = cs.id
WHERE stu.id IS NULL;

// or 
select cs.name csName, cs.id csId, stu.name stuName
from `courses` cs
left join `students_select_courses` ssc on cs.id = ssc.course_id
left join `students` stu on stu.id = ssc.student_id
where stu.id is null

# 4.5. 某一个学生选了哪些课程（why）
SELECT stu.id id, stu.name stuName, stu.age stuAge, cs.id csId, cs.name csName, cs.price csPrice
FROM `students` stu
LEFT JOIN `students_select_courses` ssc ON stu.id = ssc.student_id
LEFT JOIN `courses` cs ON ssc.course_id = cs.id
WHERE stu.id = 2;
```
