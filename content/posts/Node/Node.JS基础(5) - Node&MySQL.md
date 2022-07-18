---
title: "Node.JS基础(5) - Node&MySQL"
date: 2020-06-16 14:29:14
draft: false
tags: ['node', '基础', mysql]
categories: ['Node']
---

如何在Node代码中对执行SQL语句，可借助以下的库：

- mysql，早期node连接Mysql的库
- mysql2，基于mysql，进行了优化，其中就有：
   - 预编译优化：优化了性能：多次执行语句时，性能更好
   - 防止sql注入：所谓的sql注入就是黑客往sql语句中插入一些条件，使其更容易被执行获取到信息；

如何使用：

- npm i mysql2
- 执行代码，连接数据库：
```json
// connect_mysql.js
const mysql = require('mysql2');

// 1.创建数据库连接
const connection = mysql.createConnection({
  host: 'localhost',
  port: 3306,
  database: 'coderlch',
  user: 'root',
  password: 'root'
});

// 2.执行SQL语句
const statement = `
  SELECT * FROM products WHERE price > 6000;
`
connection.query(statement, (err, results, fields) => {
  console.log(results);
  connection.end();
});


// 预编译语句
const statement = `
SELECT * FROM phone WHERE price > ? AND score > ?;
`;

connection.execute(statement, [3000, 8], (err, results) => {
  console.log(results);
  connection.end();
});
```
连接池的使用
```json
const connections = mysql.createPool({
	host: 'localhost',
  port: 3306,
  database: 'coderlch',
  user: 'root',
  password: 'root',
  connectionLimit: 10 // 连接池数目限制为10
})
```
使用promise方式
```json
connections.promise().excute(statement).then(([res, field])=>{...}) // 
```
ORM：对象关系映射
提供了一个可在编程语言中，使用虚拟对象数据库的效果。
在上面，我们在js代码中使用了sql语句对数据库进行操作。而通过ORM，可以不用编写sql语句，而是通过操作对象的方式从而对数据库进行操作。
如JAVA中的Hibernate、MyBatis，在node中有一个库Sequelize提供这样的功能。
> 其实就是创建了一层 表 - 对象 的映射，从而让我们操作对象

sequelize的使用
利用sequelize做多表之间的一对多的关系，需要将两张表联系起来

