---
title: "Node.JS基础(3) - HTTP_Express_Koa"
date: 2020-06-16 14:29:14
draft: false
tags: ['node', '基础']
categories: ['Node']
---

> 参考代码：[04_http模块.zip](https://www.yuque.com/attachments/yuque/0/2021/zip/714353/1623740995324-0451590c-5a70-4bf3-9ab7-a8f180ce6558.zip?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fzip%2F714353%2F1623740995324-0451590c-5a70-4bf3-9ab7-a8f180ce6558.zip%22%2C%22name%22%3A%2204_http%E6%A8%A1%E5%9D%97.zip%22%2C%22size%22%3A6461%2C%22type%22%3A%22application%2Fx-zip-compressed%22%2C%22ext%22%3A%22zip%22%2C%22status%22%3A%22done%22%2C%22taskId%22%3A%22u6a7dd7a1-759f-4f3d-a284-2812c9ec647%22%2C%22taskType%22%3A%22upload%22%2C%22id%22%3A%22X6nby%22%2C%22card%22%3A%22file%22%7D)

## HTTP模块
是node的内置模块，用于处理来自客户端的网络请求。

1. 创建服务：
```javascript
// create_http.js
const http = require('http');

// 创建一个web服务器
const server = http.createServer((req, res) => {
  // request对象中封装了客户端给我们服务器传递过来的所有信息
  console.log(req.url);
  console.log(req.method);
  console.log(req.headers);
  res.end("Hello Server");
  
});

// 启动服务器,并且制定端口号和主机
server.listen(8789, '0.0.0.0', () => {
  console.log("服务器启动成功~");
});

// 终端：node create_http.js，即可查看运行
```
其它详看代码示例。

## express框架
> 代码：[05_express框架.zip](https://www.yuque.com/attachments/yuque/0/2021/zip/714353/1623742593699-001e8ac1-ce21-4164-958e-bfc5f23cca3d.zip?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fzip%2F714353%2F1623742593699-001e8ac1-ce21-4164-958e-bfc5f23cca3d.zip%22%2C%22name%22%3A%2205_express%E6%A1%86%E6%9E%B6.zip%22%2C%22size%22%3A7639%2C%22type%22%3A%22application%2Fx-zip-compressed%22%2C%22ext%22%3A%22zip%22%2C%22status%22%3A%22done%22%2C%22taskId%22%3A%22uac1a2aae-a139-41ce-ada7-9066edcec4f%22%2C%22taskType%22%3A%22upload%22%2C%22id%22%3A%22uc48fcc53%22%2C%22card%22%3A%22file%22%7D)

### 基本使用
为什么需要框架？内置http模块不好用，处理较复杂、混乱，用框架处理更加简洁。

1. 安装：
   1. 官方脚手架：npm i -g express-generator
   1. 自己搭建：npm i express
2. 创建服务：
```javascript
const express = require('express');

// 创建服务器
const app = express();

// /home的get请求处理
app.get("/home", (req, res) => {
  res.end("Hello Home");
});

// /login的post请求处理
app.post("/login", (req, res) => {
  res.end("Hello Login");
});

// 开启监听
app.listen(8000, () => {
  console.log("服务器启动成功~");
})
```
Express是一个路由和中间件的Web框架，本身功能少，本质上是一系列中间件函数的调用。
中间件就是一个回调函数，该函数包含三个参数：resquest对象，response对象，next函数。
中间件中可以做些什么：

- 执行代码；
- 更改resquest、response对象；
- 返回数据；
- 利用next()调用栈中的下一个中间件；

应用中间件：app.use()、app.methods()、router.use()、router.methods()，这里的methods是get或post；
中间件并非都要自己写，express有内置帮助解析的中间件，registry仓库也有；

客户端传递到服务器参数的几种方式，可设置相关中间件进行解析：

- get -- params：http://localhost:8000/login/abc/why
- get -- query：http://localhost:8000/login?username=why&password=123
- post -- application/json
- post -- application/x-www-form-unlencoded
- post -- multipart/form-data

### router路由
为什么使用路由？将各个中间件、逻辑处理函数写在一个文件中将会很复杂，利用路由可将逻辑各自分开，放入单独的文件中。看以下示例代码体会一下：
```javascript
// app.js
const express = require('express');
const userRouter = require('./routers/users');
const productRouter = require('./routers/products');

const app = express();

app.use("/users", userRouter);
app.use("/products", productRouter);

app.listen(8000, () => {
  console.log("路由服务器启动成功~");
});

// ./routers/users.js
const express = require('express');
const router = express.Router();

router.get('/', (req, res, next) => {
  res.json(["why", "kobe", "lilei"]);
});

router.get('/:id', (req, res, next) => {
  res.json(`${req.params.id}用户的信息`);
});

router.post('/', (req, res, next) => {
  res.json("create user success~");
});

module.exports = router;
```
### 静态资源服务器
什么是静态资源？就是npm run build命令生成的index.html及其一些资源文件。
node可以作为部署静态资源的服务器。
```javascript
const express = require('express');

const app = express();

app.use(express.static('./build'));

app.listen(8000, () => {
  console.log("静态服务器启动成功~");
})
```
### 错误处理方式
```javascript
app.use((req, res, next) => {
  next(new Error("USER DOES NOT EXISTS"));
});

app.use((err, req, res, next) => {
  const message = err.message;

  switch (message) {
    case "USER DOES NOT EXISTS":
      res.status(400).json({message})
  }

  res.status(500)
})
```
## koa框架
> 代码：[06_koa框架.zip](https://www.yuque.com/attachments/yuque/0/2021/zip/714353/1623744831541-6459a13d-40b8-4049-a04f-3a455d5269c3.zip?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fzip%2F714353%2F1623744831541-6459a13d-40b8-4049-a04f-3a455d5269c3.zip%22%2C%22name%22%3A%2206_koa%E6%A1%86%E6%9E%B6.zip%22%2C%22size%22%3A69582%2C%22type%22%3A%22application%2Fx-zip-compressed%22%2C%22ext%22%3A%22zip%22%2C%22status%22%3A%22done%22%2C%22taskId%22%3A%22u603c6f3d-ac6e-4288-9238-8deac5a3843%22%2C%22taskType%22%3A%22upload%22%2C%22id%22%3A%22ubb0139e7%22%2C%22card%22%3A%22file%22%7D)

为什么需要Koa框架？相比express更加轻量，并具有更强、更丰富的能力。

1. 创建服务：
```javascript
const Koa = require('koa');

const app = new Koa();

app.use((ctx, next) => {
  ctx.response.body = "Hello World";
});

app.listen(8000, () => {
  console.log("koa初体验服务器启动成功~");
});
```
ctx中就包含了请求、相应对象：ctx.request、ctx.response
koa注册中间件只能通过use方式；
Koa本身不提供路由，我们可选择第三方库：koa-router：npm install koa-router
```javascript
// app.js
const Koa = require('koa');

const userRouter = require('./router/user');

const app = new Koa();

app.use(userRouter.routes());
app.use(userRouter.allowedMethods()); // 可自动帮我们判断某一个method是否支持，状态码200/405/501

app.listen(8000, () => {
  console.log("koa路由服务器启动成功~");
});

// ./router/user.js
const Router = require('koa-router');

const router = new Router({prefix: "/users"});

router.get('/', (ctx, next) => {
  ctx.response.body = "User Lists~";
});

router.put('/', (ctx, next) => {
  ctx.response.body = "put request~";
});

module.exports = router;
```
请求解析：

- ctx.params.id
- ctx.request.query
- 利用koa-bodyparser库解析json格式、x-www-form-urlencoded格式；
- 利用koa-multer库解析form-data格式，还可以实现文件的上传；

响应方式：

- 可设置响应码，ctx.response.status，即ctx.status
- ctx.response.body，即crx.body，设置响应主体

错误处理：
```javascript
const Koa = require('koa');

const app = new Koa();

app.use((ctx, next) => {
  ctx.app.emit('error', new Error("哈哈哈"), ctx);
})

app.on('error', (err, ctx) => {
  console.log(err.message);
  ctx.response.body = "哈哈哈";
})

app.listen(8000, () => {
  console.log("错误处理服务启动成功~");
})
```
静态服务器：需要使用第三方库koa-static
```javascript
const Koa = require('koa');
const static = require('koa-static');

const app = new Koa();

app.use(static('./build'));

app.listen(8000, () => {
  console.log("静态服务器启动成功~");
});
```
## express和koa对比
> 代码：[07_koa对比express.zip](https://www.yuque.com/attachments/yuque/0/2021/zip/714353/1623748725280-e78cebcd-34d2-43b4-90f6-44c69a5a542c.zip?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fzip%2F714353%2F1623748725280-e78cebcd-34d2-43b4-90f6-44c69a5a542c.zip%22%2C%22name%22%3A%2207_koa%E5%AF%B9%E6%AF%94express.zip%22%2C%22size%22%3A1675%2C%22type%22%3A%22application%2Fx-zip-compressed%22%2C%22ext%22%3A%22zip%22%2C%22status%22%3A%22done%22%2C%22taskId%22%3A%22ua3338d2a-65e2-480e-a076-8739af45318%22%2C%22taskType%22%3A%22upload%22%2C%22id%22%3A%22u12fba352%22%2C%22card%22%3A%22file%22%7D)

express是完整、强大的，内置了很多功能；
koa是简洁、自由的，不会对使用其它中间件做限制；
两个框架的核心都是中间件，但其执行机制是不同的，尤其是中间件中包含异步操作。
```javascript
// express异步操作
const middleware1 = async (req, res, next) => {
  req.message = "aaa";
  await next();
  res.end(req.message);
}

const middleware2 = async (req, res, next) => {
  req.message = req.message + 'bbb';
  await next();
}

const middleware3 = async (req, res, next) => {
  const result = await axios.get('http://123.207.32.32:9001/lyric?id=167876');
  req.message = req.message + result.data.lrc.lyric;
  console.log(req.message);
}
// 结果为aaabbb，不正确
```
```javascript
// koa异步操作
const middleware1 = async (ctx, next) => {
  ctx.message = "aaa";
  await next();
  ctx.body = ctx.message;
}

const middleware2 = async (ctx, next) => {
  ctx.message = ctx.message + 'bbb';
  await next();
}

const middleware3 = async (ctx, next) => {
  const result = await axios.get('http://123.207.32.32:9001/lyric?id=167876');
  ctx.message = ctx.message + result.data.lrc.lyric;
}
// 结果为aaabbb歌词信息，正确
```

