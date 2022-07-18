---
title: "Node.JS基础(2) - 内置模块、NPM、Buffer、Stream"
date: 2020-06-16 14:29:14
draft: false
tags: ['node', '基础']
categories: ['Node']
---

> 参考代码：[03_learn-node.7z](https://www.yuque.com/attachments/yuque/0/2021/7z/714353/1623825562606-c6f8df45-1cad-48fa-93e8-1de949cb3265.7z?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2F7z%2F714353%2F1623825562606-c6f8df45-1cad-48fa-93e8-1de949cb3265.7z%22%2C%22name%22%3A%2203_learn-node.7z%22%2C%22size%22%3A144689%2C%22type%22%3A%22%22%2C%22ext%22%3A%227z%22%2C%22status%22%3A%22done%22%2C%22taskId%22%3A%22uf862889b-0ba2-4080-8acf-ca6ac861eb9%22%2C%22taskType%22%3A%22upload%22%2C%22id%22%3A%22ub29a5396%22%2C%22card%22%3A%22file%22%7D)

## 常用的内置模块
### 内置模块path
从路径中获取信息：

- dirname：获取文件的父文件夹
- basename：获取文件名
- extname：获取文件扩展名

路径拼接：

- join：
- resolve：某个文件和文件夹拼接
```javascript
const path = require("path");
const basepath = "../User/why";
const filename = "./abc.txt";
const filepath1 = path.join(basepath, filename); // ..\User\why\abc.txt

const basepath2 = "/User/coderwhy";
const filename2 = "/why/abc.txt"; // D:\why\abc.txt
const filename3 = "./why/abc.txt"; // D:\User\coderwhy\why\abc.txt

const result = path.resolve(basepath2, filename3);
```
### fs - 文件系统
Node文件系统的API非常的多：[https://nodejs.org/dist/latest-v14.x/docs/api/fs.html](https://nodejs.org/dist/latest-v14.x/docs/api/fs.html)
学习阶段只需学习常用的即可。这些API大多数提供三种操作方式：

- 方式一：同步操作文件：代码会被阻塞，不会继续执行；
- 方式二：异步回调函数操作文件：代码不会被阻塞，需要传入回调函数，当获取到结果时，回调函数被执行；
- 方式三：异步Promise操作文件：代码不会被阻塞，通过 fs.promises 调用方法操作，会返回一个Promise，可以通过then、catch进行处理；
```javascript
// 1.方式一: 同步读取文件
const state = fs.statSync('../foo.txt');
console.log(state);

console.log('后续代码执行');

// 2.方式二: 异步读取
fs.stat("../foo.txt", (err, state) => {
  if (err) {
    console.log(err);
    return;
  }
  console.log(state);
})
console.log("后续代码执行");

// 3.方式三: Promise方式
fs.promises.stat("../foo.txt").then(state => {
  console.log(state);
}).catch(err => {
  console.log(err);
})
console.log("后续代码执行");
```
文件描述符：

- `fs.open(filename, flag, callback)`

文件的读写：

- `fs.readFile(path[, options], callback)`：读取文件的内容
- `fs.writeFile(file, data[, options], callback)`：写入内容
```javascript
fs.writeFile('../foo.txt', content, {}, err => {
  console.log(err);
})
```
其中`{}`为options，参数有：

- flag：写入的方式
- encoding：字符的编码。一般为`'utf-8'`，不填写会返回Buffer（缓冲区）

其中，`flag`的值可以为：

- w 打开文件写入，默认值；
- w+打开文件进行读写，如果不存在则创建文件；
- r+ 打开文件进行读写，如果不存在那么抛出异常；
- r打开文件读取，读取时的默认值；
- a打开要写入的文件，将流放在文件末尾。如果不存在则创建文件；
- a+打开文件以进行读写，将流放在文件末尾。如果不存在则创建文件

文件夹操作：

- 新建文件夹：`fs.mkdir()`、`fs.mkdirSync()`
```javascript
const fs = require('fs');
const dirname = '../why';
if (!fs.existsSync(dirname)) {
  fs.mkdir(dirname, (err) => {
    console.log(err);
  })
}
```

- 获取文件夹的内容：fs.readdir
```javascript
// 读取文件夹
function readFolders(folder) {
  fs.readdir(folder, {withFileTypes: true} ,(err, files) => {
    files.forEach(file => {
      if (file.isDirectory()) {
        const newFolder = path.resolve(dirname, file.name);
        readFolders(newFolder);
      } else {
        console.log(file.name);
      }
    })
  })
}

readFolders(dirname);
```

- 文件重命名：fs.rename
```javascript
fs.rename('../why', '../coder', err => {
  console.log(err);
})
```

### events - 发出、监听事件
发出事件和监听事件都是通过EventEmitter类来完成的，它们都属于events对象。

- emitter.on(eventName, listener)：监听事件，也可以使用addListener；
- emitter.off(eventName, listener)：移除事件监听，也可以使用removeListener；
- emitter.emit(eventName[, ...args])：发出事件，可以携带一些参数；

常见的EventEmitter实例方法：

- emitter.eventNames()：返回当前 EventEmitter对象注册的事件字符串数组；
- emitter.getMaxListeners()：返回当前 EventEmitter对象的最大监听器数量，可以通过setMaxListeners()来修改，默认是10；
- emitter.listenerCount(事件名称)：返回当前 EventEmitter对象某一个事件名称，监听器的个数；
- emitter.listeners(事件名称)：返回当前 EventEmitter对象某个事件监听器上所有的监听器数组；
```javascript
console.log(bus.eventNames());
console.log(bus.getMaxListeners());
console.log(bus.listenerCount("click"));
console.log(bus.listeners("click"));
```

- emitter.once(eventName, listener)：事件监听一次
- emitter.prependListener()：将监听事件添加到最前面
- emitter.prependOnceListener()：将监听事件添加到最前面，但是只监听一次
- emitter.removeAllListeners([eventName])：移除所有的监听器

代码示例：
```javascript
const EventEmitter = require("events");

const emitter = new EventEmitter(); // 创建events实例对象

function clickHandle(args) {
  // 监听器函数
  console.log("事件监听", args);
}

emitter.once("click", (args) => {
  // 一次性监听
  console.log("监听到事件", args);
});

emitter.on("click", clickHandle); // 监听事件

// b监听事件会被放到前面
emitter.prependListener("click", (args) => {
  console.log("b监听到事件", args);
});

// c监听事件会被放到前面，且只监听一次
emitter.prependOnceListener("click", (args) => {
    console.log("c监听到事件", args);
  })

setTimeout(() => {
  emitter.emit("click", "coderwhy"); // 发射事件
  emitter.emit("click", "coderwhy");
}, 2000);
// emitter.emit("click", "111");
emitter.off("click", clickHandle); // 移除监听

console.log(emitter.eventNames()); // ['click']
console.log(emitter.getMaxListeners()); // 10
console.log(emitter.listenerCount("click")); // 1，监听click事件的个数
console.log(emitter.listeners("click")); // [[Function]]

// 移除emitter上的所有事件监听
emitter.removeAllListeners();
// 移除emitter上的click事件监听
emitter.removeAllListeners("click");
```

## 包管理工具NPM
### 项目配置文件package.json
每个项目都有一个配置文件package.json。这个文件记录着项目的名称、版本号、项目描述、依赖的其它库的信息及版本号。
脚手架可以生成package.json文件，也可以手动创建：`npm init -y`
下面对常见的属性做一些注释：
```json
{
  "name": "learn-npm", // 项目名
  "version": "1.0.0", // 版本号
  "description": "", // 描述信息
  "main": "main.js", // 程序的入口，发布模块的时候用到
  "scripts": { // 配置一些脚本命令
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "", // 作者名，发布时用到
  "license": "ISC", // 开源协议，发布时用到
  "private": "false", // 项目是否私有
  "dependencies": { // 不仅开发环境能使用，生产环境也能使用
  	 "vue": "^2.5.2"
  },
  "devDependencies":{ // 开发时依赖，生产环境不会被打入包内
  	"autoprefixer": "^7.1.2"
  }
}
```
package.lock.json
npm install的原理图：
![](https://cdn.nlark.com/yuque/0/2021/webp/714353/1617106310073-97e16294-87c8-41d0-89bb-01e5956d61d7.webp#height=307&id=J6B7B&originHeight=307&originWidth=1080&originalType=binary&ratio=1&size=0&status=done&style=none&width=1080)

其它npm命令：

- `npm uninstall package`：卸载包
- npm uninstall package --save-dev
- npm uninstall package -D
- `npm rebuild`：强制重新build
- `npm cache clean -f`：清除缓存
### npm和yarn常用命令对比
yarn是由Facebook、Google、Exponent 和 Tilde 联合推出了一个新的 JS 包管理工具。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1617156554863-8f265d64-49f3-49c4-9e60-ba4eadadaa96.png#height=287&id=RY9GX&margin=%5Bobject%20Object%5D&name=image.png&originHeight=383&originWidth=934&originalType=binary&ratio=1&size=95111&status=done&style=none&width=701)
### 补充：cnpm
查看npm镜像地址：`npm config get registry`
设置npm镜像地址：`npm config set registry https://registry.npm.taobao.org`
换回默认镜像地址：`npm config set registry https://registry.npmjs.org`
有些开发者不想更改npm的镜像地址，于是就使用cnpm：
`npm install -g cnpm --registry=https://registry.npm.taobao.org`
`cnpm config get registry # https://r.npm.taobao.org/`
### 补充：npx
npx是npm5.2之后自带的一个命令。npx常用的命令是用来调用项目中的某个模块的指令。
平常在终端输入`webpack -v`显示的是全局的webpack版本，若想找到本项目的webpack，该怎么做？

- 法一：`./node_modules/.bin/webpack --version`
- 法二：修改package.json中的scripts，添加`"webpack": "webpack --version"`，终端`npm run webpack`
- 法三：`npx webpack -v`，原理是npx直接到当前目录的node_modules/.bin目录查找
## 
## Buffer
数据的二进制：计算机上的所有内容最终以二进制形式表示，不同的文件内容有不同的编码方式。
> 参考地址：[几种常见的编码格式](https://blog.csdn.net/maikelsong/article/details/81098456)

不同的文件内容有不同的编码方式，常见的编码方式有：ASCII、ISO-8859-1、GB2312、GBK、UTF-8、UTF-16等等。那为什么需要编码呢？编码是为了让各个国家的语言、符号都能够正确地被计算机识别并显示。如ASCII码总共有128个，用一个字节的低7位表示：
![](https://cdn.nlark.com/yuque/0/2021/gif/714353/1617161926307-838fb60b-f97a-4df4-8d04-2392a317d164.gif#height=8&id=wNXVS&originHeight=8&originWidth=4&originalType=binary&ratio=1&size=0&status=done&style=none&width=4)![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1617161932875-3e51784a-53fe-47a6-9386-9b8620c76dc2.png#height=260&id=Gcn14&margin=%5Bobject%20Object%5D&name=image.png&originHeight=520&originWidth=826&originalType=binary&ratio=1&size=249741&status=done&style=none&width=413)
但ASCII码无法表示中文汉字，于是就需要有支持中文字符的编码方式，就出现了GB2312、GBK、UTF-8、UTF-16等等。这些编码方都支持中文，如何进一步选择呢？这就需要考虑其它因素：存储空间&&编码效率孰轻孰重。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1617169516745-4f78dce7-7309-4abc-904a-107b47431981.png#height=442&id=Ialxz&margin=%5Bobject%20Object%5D&name=image.png&originHeight=734&originWidth=665&originalType=binary&ratio=1&size=361733&status=done&style=none&width=400)
编码示意图
> PS：在Node中通过TCP建立长连接，TCP传输的是字节流，我们需要将数据转成字节再进行传入，并且需要知道传输字节的大小（客户端需要根据大小来判断读取多少内容）

### Buffer和二进制
Buffer是一个存储二进制的数组，数组的每一项可存储8位二进制，即1个字节。
1Byte = 8bit；1KB=1024Byte；1MB=1024KB；
![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1617170559138-33f7f9ca-b5de-439e-8298-8339f00b7f1e.png#height=99&id=Kj7Sm&margin=%5Bobject%20Object%5D&name=image.png&originHeight=170&originWidth=689&originalType=binary&ratio=1&size=60094&status=done&style=none&width=400)
因此，Buffer相当于是字节的数组。那如何将一个字符串放入到Buffer中？
```javascript
const buffer01 = new Buffer('why'); // 已被弃用, 改用Buffer.from()
const buffer01 = Buffer.from('why'); // 编码，默认utf8
console.log(buffer01);
buffer01.toString() // "why", 解码, 默认utf8
const buffer02 = Buffer.alloc(8); // 创建了一个8位长度的Buffer, <Buffer 00 00 00 00 00 00 00 00>

```
### Buffer和文件读取
```javascript
// 1.文本文件读取
const fs = require("fs");
const filename = "./readme.txt";
fs.readFile(filename, (err, buffer) => {
  console.log("buffer", buffer); // buffer <Buffer e5 b0 86 e8 af ...
  console.log(buffer.toString()); // 将读取的某...
});
// 2.图片文件读取和转换，借助sharp库完成

```

## Stream
Stream表示流，想象成水流可以流动。在文件系统中即文件流，可读可写。
上面有fs.readFile、fs.writeFile对文件进行读写，为何还要Stream？

- 直接读写文件的方式，虽然简单，但是无法控制一些细节的操作；
- 比如从什么位置开始读、读到什么位置、一次性读取多少个字节；
- 读到某个位置后，暂停读取，某个时刻恢复读取等等；
- 或者这个文件非常大，比如一个视频文件，一次性全部读取并不合适；

Node中哪些对象基于流实现？有很多，且**所有流都是EventEmitter的实例**：

- http模块的Request和Response对象
- process.stdout对象

流的分类：

- Writable：可以向其写入数据的流（例如 fs.createWriteStream()）。
- Readable：可以从中读取数据的流（例如 fs.createReadStream()）。
- Duplex：同时为Readable和的流Writable（例如 net.Socket）。
- Transform：Duplex可以在写入和读取数据时修改或转换数据的流（例如zlib.createDeflate()）。

**Readable**
```javascript
const fs = require("fs");
const filename = "./readme.txt";
const read = fs.createReadStream(filename, {
  start: 3, // 读取的开始位置，闭区间
  end: 8, // 结束位置，闭区间
  highWaterMark: 4, // 一次读取的字节数
});
read.on("data", (data) => { //通过监听data事件，来读取数据
  console.log(data);
  read.pause() // 暂停读取
  setTimeout(()=>{read.resume()}, 2000) // 2s后继续读取
});

read.on('open',callback) // 监听文件打开
read.on('end',callback) // 监听读取结束
read.on('close',callback) // 监听文件被关闭
```
![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1617172558388-d706a0ca-dd89-438d-bb3b-fcc12783d577.png#height=72&id=ujo5s&margin=%5Bobject%20Object%5D&name=image.png&originHeight=96&originWidth=507&originalType=binary&ratio=1&size=11486&status=done&style=none&width=380)
**Writable**
可以控制写入的位置。
```javascript
const writer = fs.createWriteStream(filename, {
  flags: "a+", // 模式：追加
  start: 8, // 开始位置
});
writer.write("幸会", (err) => {
  console.log("err", err);
});
write.close(); // 写入流需手动关闭
write.end('writeFile end.'); // 
write.on('open',callback) // 监听文件打开
write.on('finish',callback) // 监听文件写入结束
write.on('close',callback) // 监听文件关闭
```
### pipe
```javascript
const fs = require('fs');
const reader = fs.createReadStream('./foo.txt');
const writer = fs.createWriteStream('./bar.txt');

reader.on("data", (data) => {
  console.log(data);
  writer.write(data, (err) => {
    console.log(err);
  });
});

// 相当于
reader.pipe(writer);
writer.on('close', () => {
  console.log("输出流关闭");
})
```
## 事件循环
浏览器的事件循环（EventLoop）是根据HTML5的规范实现的，不同浏览器可能有不同实现。
在Node中是由libuv实现的（libuv是一个多平台的专注于异步IO的库）
JS对文件的操作实际上是由操作系统去完成的，操作系统提供了阻塞式调用和非阻塞式调用。

- 阻塞式：当结果返回才继续执行；
- 非阻塞式：执行了以后，当前线程不会等待，过一段时间去检查结果是否返回即可；如网络请求操作、文件IO操作；

但非阻塞式也有一个问题，就是当未得到结果，则需要定时去轮询。那轮询操作是谁做的？
为避免主线程资源的消耗，主线程不轮询，libuv提供了一个线程池来负责相关操作，并将轮询的结果放到事件循环的某一队列中，事件循环则会将结果通过回调函数提供给JS应用。即：

- 无论是我们的文件IO、数据库、网络IO、定时器、子进程，在完成对应的操作后，都会将对应的结果和回调函数放到事件循环（任务队列）中；
- 事件循环会不断的从任务队列中取出对应的事件（回调函数）来执行；

Node中的事件循环调用顺序：

- 微任务队列：
   - next tick queue：process.nextTick
   - other queue：Promise的then回调、queueMicrotask
- 宏任务队列：
   - timer queue：setTimeout、setInterval
   - poll queue：IO事件
   - check queue：setImmediate
   - close queue：close事件





