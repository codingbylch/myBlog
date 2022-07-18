---
title: "Node.JS实战 coderhub"
date: 2020-06-16 14:29:14
draft: false
tags: ['node', '基础']
categories: ['Node']
---

目的；旨在为了搭建一个分项生活动态的平台，类似一个论坛、博客
项目接口包括： 

- 面向用户的业务接口，给用户提供服务
- 面向内部的后台管理接口，用于管理网站 
   - 用户管理系统
   - 内容管理系统
   - 内容评论管理
   - 内容标签管理
   - 文件管理系统

 
## 项目的搭建 

- 目录结构的划分
- 按照功能模块划分 
   - src 
      - app：存放主逻辑代码，调用路由 
         - index.js：调用路由相关
         - database.js：连接数据库
         - error-handle.js：错误情况相关
         - config.js：调用dotenv库从.env文件获取端口号

 

      - constants：存放常量 
         - error-types.js

 

      - controller：存放控制器代码，即具体的router处理逻辑，被路由调用 
         - user.controller.js

 

      - middleware：存放中间件代码，如验证用户传递参数的逻辑，被路由调用 
         - user.middleware.js

 

      - router：存放路由代码，对用户请求做出响应  
> controller、middleware、service等出现都是为了分担router里的逻辑，如果一股脑全写在router，代码太乱了

            - auth.router.js
            - user.router.js

 

         - service：存放操作数据库相关的逻辑，如插入用户信息、查询用户信息，被middleware和controller调用 
            - user.service.js

 

         - utils：存放一般逻辑的代码，如对密码进行加密 
            - password-handle.js

 

         - main.js：入口文件

 

      - .env：存放与环境相关的代码，如端口号 
         - 利用dotenv库和文件“.env”让端口从文件“.env”进行读取

 

      - 按照业务模块划分

 

- 数据库中直接保存密码是很危险的，密码需要加密后保存

## 用户注册登录逻辑：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1627568308690-bc166086-a378-4a94-b222-c49d92e170f1.png)

## 登录凭证
在用户登录接口中, 登陆成功后, 后端需要返回凭证, 有2种方式可供选择：

- cookie+session
- Token令牌（现在大多用这个）

### 为什么需要登录凭证？
因为HTTP是无状态的协议，所以无法保存用户登录的状态，需要有一个东西能够让用户不用频繁输入账号密码，且能证明用户身份，这就是凭证。

### 关于cookie：
一般在服务器端设置cookie，在Koa种默认支持直接操作cookie，session可以以cookie为载体，这个session就是用户身份凭证。

### session+cookie方式的缺点：

- Cookie会被附加在每个HTTP请求中，所以无形中增加了流量
- Cookie是明文传递的，所以存在安全性的问题
- Cookie的大小限制是4KB
- 对于浏览器外的其他客户端（比如iOS、Android），必须手动的设置cookie和session
- 对于分布式系统和服务器集群中如何可以保证其他系统也可以正确的解析session

### 关于token
用户登录成功后，可给用户颁发一个token，也称为令牌，后续的用户访问接口可借助令牌作为凭证，判断是否有权限访问。

**使用token的两个重要步骤：**

- 生成token：登陆时，给用户颁发token
- 验证token：用户访问接口时，验证token

**生成token**
JWT（JSON WEB TOKEN）是一种认证协议，通过JWT标准可生成符合规范的token。
JWT由三部分组成：

- header
   - alg：采用的加密算法，默认是 HMAC SHA256（HS256），采用同一个密钥进行加密和解密（默认为对称加密）；
   - typ：JWT，固定值，通常都写成JWT即可；
   - 会通过base64Url算法进行编码；
- payload
   - 携带的数据，比如我们可以将用户的id和name放到payload中；
   - 默认也会携带iat（issued at），令牌的签发时间；
   - 我们也可以设置过期时间：exp（expiration time）；
   - 会通过base64Url算法进行编码
- signature
   - 设置一个secretKey，通过将前两个的结果合并后进行HMACSHA256的算法；
   - HMACSHA256(base64Url(header)+.+base64Url(payload), secretKey);
   - 但是如果secretKey暴露是一件非常危险的事情，因为之后就可以模拟颁发token，也可以解密token；

![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1628003170169-dce3e4ae-4c8f-42ed-ae62-9fd05904acc1.png)
在真实开发中，使用库来完成：jsonwebtoken
![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1628427360675-8270a033-ad53-4ad3-8de7-b644aa3571ae.png)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1628427379485-d29ac880-4d2a-496b-9aea-71476f522282.png)

token中使用非对称加密：

- 先用openssl生成一对密钥：
   - git bash中：`> openssl`
   - `> genrsa -out private.key 1024` 生成私钥
   - `> rsa -in private.key -pubout -out public.key` 生成公钥

补充知识点：在项目中的任何一个地方的相对路径，都是先对于process.cwd，即在哪个文件夹启动，process.cwd就指向这个文件夹。

## RESTful API
API的意思是，按照别人给的规范调用别人提供的接口/功能，你就可以获得该功能。
REST描述的是在网络中client和server的一种交互的形式，但**不是一种协议，只是一种设计风格**。可以按照REST风格设计接口，可能别人会觉得好用。
[阮一峰的RESTful API文章](https://www.ruanyifeng.com/blog/2014/05/restful_api.html)

## 发布和修改评论
### 发布评论的逻辑：

- 定义路由接口
- 验证当前用户是否登录（token）
- 在对应controller中引用service中对数据库操作的模块，返回数据给用户

### 删除、修改评论的逻辑：

- 定义路由接口
- 验证当前用户是否登录（token）
- 验证当前所登录用户所拥有的权限
- 在对应controller中引用service中对数据库操作的模块，返回数据给用户

### 查询评论的逻辑：

- 可根据评论Id查询某条评论内容
- 可根据offset、limit查询多条评论内容

```javascript
// 发布评论
async create(user_id, content) {
  const statement = "INSERT INTO moment (content, user_id) VALUES (?,?)";
  const result = await connection.execute(statement, [content, user_id]);
  return result;
}

// 查询某条评论
async getSingle(momentId) {
  const statement = `
    SELECT 
    m.id id, m.content content, m.updateAt updateTime,
    JSON_OBJECT('id', u.id, 'name',u.name) userInfo
    FROM moment m LEFT JOIN users u ON m.user_id = u.id WHERE m.id = ?
    `;
  const result = await connection.execute(statement, [momentId]);
  return result;
}
// 查询多条评论
async getMulti(pagesize, offset) {
  const statement = `
    SELECT 
    m.id id, m.content content, m.updateAt updateTime,
    JSON_OBJECT('id', u.id, 'name',u.name) userInfo
    FROM moment m LEFT JOIN users u ON m.user_id = u.id 
    LIMIT ?,?
    `;
  const result = await connection.execute(statement, [pagesize, offset]);
  return result;
}

// 删除某条评论

// 修改某条评论
```
验证权限的中间也很重要。因为很多接口都需要先验证权限再操作。
业务接口系统、后端管理系统，一般是分开部署的，不是一起开发的。

获取评论列表，有两种形式：

1. 动态接口和评论接口是分开的
1. 请求动态接口的时候，就会一起携带评论的列表
   1. sql较为复杂
   1. 内容多则可能比较慢

想实现的效果：
关于某条动态的相关评论，且知道某条评论是哪位用户发表的。
```sql
// 第1种，接口分开写

// 第2种 -- 查询某条动态，包含评论
SELECT 
			m.id id, m.content content, m.updateAt updateTime,
			JSON_OBJECT('id', u.id, 'name',u.name) userInfo,
			JSON_ARRAYAGG(
				JSON_OBJECT('id', c.id, 'content', c.content, 'commentId', c.comment_id,
				'user', JSON_OBJECT('id', cu.id, 'name',cu.name))
			) comments
		FROM moment m 
		LEFT JOIN users u ON m.user_id = u.id 
		LEFT JOIN comment c ON c.moment_id = m.id
		LEFT JOIN users cu ON c.user_id = cu.id
		WHERE m.id = 7
		GROUP BY m.id;

```

小总结：如要查询某条评论的具体信息，简单查询即可；若要增加该条评论是由哪位用户发表的，需使用左连接用户表。（想增加什么信息，就用左连接对应的表）

## 标签接口开发
标签涉及多对多关系。
点赞就是多对多：一个动态可被多个用户点赞，一个用户可以点赞多个动态。

动态表、标签表，之前还有一个关系映射表

创建标签表

多对多的核心就是这张关系表：moment_label表
```sql
CREATE TABLE IF NOT EXISTS `moment_label`(
  moment_id INT NOT NULL,
  label_id INT NOT NULL,
  createAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updateAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY(moment_id, label_id),
  FOREIGN KEY(moment_id) REFERENCES moment(id) ON DELETE CASCADE ON UPDATE CASCADE,
  FOREIGN KEY(label_id) REFERENCES label(id) ON DELETE CASCADE ON UPDATE CASCADE
);
```


上传图片、文件
原理都是差不多的，拿到文件类型，之后告诉用户文件类型就行，浏览器会处理的。

图片上传接口，目的：服务器能保存图片
提供一个接口，让用户可以获取图片
![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1630508567163-8fefbc36-3f96-4902-823a-3095cca95c26.png)

服务器如何将发送过来的图片保存到某个位置？
npm install koa-multer：能够帮助获取发送过来的文件。

78:52
