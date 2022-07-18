---
title: "Node.JS基础(1) - 全局对象、模块化开发"
date: 2020-06-16 14:29:14
draft: false
tags: ['node', '基础']
categories: ['Node']
---

> 参考资料：
> 1. [Node - 从0基础到实战企业官网](https://juejin.im/post/5c1f8e52f265da6170071e43)
> 1. [廖雪峰node.js](https://www.liaoxuefeng.com/wiki/1022910821149312/1023025235359040)
> 1. [一杯茶的时间，上手 Node.js](https://tuture.co/2019/12/03/892fa12/)
> 1. 代码：[01_learn-node.7z](https://www.yuque.com/attachments/yuque/0/2021/7z/714353/1623825465783-0af9c36b-5201-4aac-b792-645624dec432.7z?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2F7z%2F714353%2F1623825465783-0af9c36b-5201-4aac-b792-645624dec432.7z%22%2C%22name%22%3A%2201_learn-node.7z%22%2C%22size%22%3A53607%2C%22type%22%3A%22%22%2C%22ext%22%3A%227z%22%2C%22status%22%3A%22done%22%2C%22taskId%22%3A%22ud1a13630-98f7-4504-9634-7dbf557d893%22%2C%22taskType%22%3A%22upload%22%2C%22id%22%3A%22u05b18fc3%22%2C%22card%22%3A%22file%22%7D)

Node.js是运行在服务端的javascript环境，与浏览器相比，没有了浏览器安全限制，而且需要实现基本的服务器功能：

1. 读写文件；
1. 处理网络请求；
1. 处理二进制内容；

![](https://cdn.nlark.com/yuque/0/2020/png/714353/1598320295931-e14f8cbf-ed18-4402-a4f8-58528b38a7d7.png#crop=0&crop=0&crop=1&crop=1&height=240&id=YMGuD&originHeight=962&originWidth=2008&originalType=binary&ratio=1&rotation=0&showTitle=false&size=0&status=done&style=none&title=&width=500)
### JavaScript全局对象分类
![](https://cdn.nlark.com/yuque/0/2020/png/714353/1598320472614-2e437470-2573-4267-81ba-ad6e7a0240ce.png#crop=0&crop=0&crop=1&crop=1&height=288&id=Q12fs&originHeight=1034&originWidth=1794&originalType=binary&ratio=1&rotation=0&showTitle=false&size=0&status=done&style=none&title=&width=500)
其中虽然浏览器和Node有共同拥有的全局对象：console、setTimeout等，但其实现方式不同。两者宿主环境不同，底层接口也不同，但对外却有很多相同名称的API，这是因为它们基本遵循ECMAScript。
### Node模块机制
原先的JS编写模式是<script>标签来导入相应模块：
```javascript
<head>
  <script src="fileA.js"></script>
  <script src="fileB.js"></script>
</head>
```
但这样有三个问题：1. 容易产生命名冲突；2. JS文件间不能相互访问；3. 导入的<script>无法被轻易去除或修改；
于是提出模块化规范，其中较流行的是AMD（浏览器）、CommonJS（Node），ES6的模块化机制，其中浏览器逐渐向ES6过渡，而Node一般使用CommonJS规范。因此在进行Node编程时若使用ES6的模块化则有问题或报错，则借助babel将ES6模块化转化成CommonJS。
**我的笔记：**
我们所实现的函数功能，可以以模块的形式进行复用，从而让其它文件引入并进行使用。“以模块的形式”其实就是利用了CommonJS规范。在这个规范下，每个js文件都是一个模块，内部所使用的变量名和函数名都各不冲突（原理是利用了函数闭包）。export.module=xxx导出，在需要使用的地方使用var tools = require("./path")。这样便完成了函数模块的导入导出。（导入、导出原理其实就是保存，然后查找）
为了方便管理，我们可以专门创建一个文件夹，用于存放模块；可以进入存放模块的文件夹下使用终端命令： npm init --yes，来生成一个package.json，这样引入的时候可以直接使用模块文件夹名称，而不是路径。
这里我们创建了node_modules文件夹，然后在里面创建了jsliang-module文件夹，并在该文件夹下创建了模块，并运行了终端命令。那在react/vue工程中，时常看到的就是node_modules文件夹了，在里面就有种类繁多的第三方模块。
路径搜索列表 module.paths搜索顺序：优先搜索当前目录下的node_modules，未找到则逐层向上查找。
### 包与npm
我的笔记：
Node中的模块分为**核心模块、第三方模块**。npm是一种第三方模块的管理工具。包模块的关系如下图。
![](https://cdn.nlark.com/yuque/0/2020/webp/714353/1595992170214-51be159f-3e9e-4645-853e-37ee0262924c.webp#crop=0&crop=0&crop=1&crop=1&height=234&id=XPDwW&originHeight=468&originWidth=822&originalType=binary&ratio=1&rotation=0&showTitle=false&size=0&status=done&style=none&title=&width=411)
很多的npm包可以在[官网](https://www.npmjs.com/)搜索下载！！！
npm scripts 分为两大类：

- **预定义脚本**：例如 test、start、install、publish 等等，直接通过 npm <scriptName> 运行；
- **自定义脚本**：除了以上自带脚本的其他脚本，需要通过 npm run <scriptName> 运行；

关于npm的一些命令：

- `npm -v`：查看 npm 版本。
- `npm list`：查看当前目录下都安装了哪些 npm 包。
- `npm info 模块`：查看该模块的版本及内容。
- `npm i 模块@版本号`：安装该模块的指定版本。
- `i`/`install`：安装。使用 `install` 或者它的简写 `i`，都表明你想要下载这个包。
- `uninstall`：卸载。如果你发现这个模块你已经不使用了，那么可以通过 `uninstall` 卸载它。
- `g`：全局安装。表明这个包将安装到你的计算机中，你可以在计算机任何一个位置使用它。
- `--save`/`-S`：通过该种方式安装的包的名称及版本号会出现在 `package.json` 中的 `dependencies` 中。`dependencies` 是需要发布在生成环境的。例如：`ElementUI` 是部署后还需要的，所以通过 `-S` 形式来安装。
- `--save-dev`/`-D`：通过该种方式安装的包的名称及版本号会出现在 `package.json` 中的 `devDependencies` 中。`devDependencies` 只在开发环境使用。例如：`gulp` 只是用来压缩代码、打包的工具，程序运行时并不需要，所以通过 `-D` 形式来安装。

例子：

- `cnpm i webpack-cli -D`
- `npm install element-ui -S`

在 package.json 的 dependencies 字段中，可以通过以下方式指定版本：

- **精确版本**：例如 `1.0.0`，一定只会安装版本为 `1.0.0` 的依赖
- **锁定主版本和次版本**：可以写成 `1.0`、`1.0.x` 或 `~1.0.0`，那么可能会安装例如 `1.0.8` 的依赖
- **仅锁定主版本**：可以写成 `1`、`1.x` 或 `^1.0.0`（ `npm install` 默认采用的形式），那么可能会安装例如 `1.1.0` 的依赖
- **最新版本**：可以写成 `*` 或 `x`，那么直接安装最新版本（不推荐）

npm 还创建了一个 package-lock.json，这个文件就是用来锁定全部直接依赖和间接依赖的精确版本号，或者说提供了关于 node_modules 目录的精确描述，从而确保在这个项目中开发的所有人都能有完全一致的 npm 依赖。
### node传递参数
正常情况下执行一个node文件，终端命令为：
`node index.js`
但是，在某些情况下执行node程序的过程中，我们可能希望给node传递一些参数：
`node index.js env=development coding`
同时，我们必须在程序中获取到在终端传入的参数，这些参数存在于内置全局对象process的process.argv上：
`console.log(process.argv)`
### node程序输出内容
console.log
console.clear
console.trace
### 常见的全局对象
#### 特殊的全局对象
特点：在模块中任意使用，但在终端命令行中无法使用。
包括：
`__filename`：获取当前文件路径，包括文件名称
`__dirname`：获取当前目录路径，不包括具体文件名
`exports`：
`module`：
`require`：
#### 常见的全局对象
`process`：表示当前的node进程，提供了node进程中相关的信息；
`console`：提供了简单的调试控制台
定时器函数：
`setTimeout`：
`setInterval`：
`setImmediate`：I/O事件后的回调的“立即”执行
`process.nextTick`：添加到下一次tick队列中
对应的取消方法：
`clearTimeout(timeoutObject)`
`clearInterval(intervalObject)`
`clearImmediate(immediateObject)`
`global`：global是一个全局对象，上面的process、console、setTimeout等都有被放到global对象中。类似于浏览器中的全局对象window，但有所不同。
判断node环境还是浏览器环境：
```javascript
if (typeof window == "undefined") {
  console.log("Node.js");
} else {
  console.log("Browser");
}
```
### 模块化开发
#### JavaScript设计缺陷：早期无模块化概念
早期的js语言设计只是为了处理浏览器中简单的逻辑。随着前端越来越复杂，大型工程需要模块化开发，否则会有命名冲突、管理复杂等问题。
#### 立即函数调用表达式
利用IIFE来解决命名冲突的问题，但也存在一些问题：1.不规范，模块名可能会重名；2.代码需要包裹在匿名函数中；
#### CommonJS规范
一种规范，开发node程序过程中方便进行模块化开发。核心变量包括：module.exports、exports、require
**导出成模块：**
exports默认是一个空对象，在node中存在module.exports = exports，导入导出最终是看module.exports这个对象。
```javascript
[[bar.js]]
// 1.使用exports进行导出
let name = 'yummy';
let sums = (a,b) => a + b;
exports.name = name;
exports.sums = sums;
(隐藏存在着module.exports = exports)

// 2.使用module.exports进行导出
module.exports = {name, sums} // 这里用到了es6的语法简写
```
![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1617085058664-42f1e2b5-b263-4fce-b286-f086a39bbeee.png#crop=0&crop=0&crop=1&crop=1&height=208&id=UYWpq&margin=%5Bobject%20Object%5D&name=image.png&originHeight=277&originWidth=428&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19333&status=done&style=none&title=&width=321)
**导入某个模块：**
require帮助我们导入其他模块（自定义模块、系统模块、第三方库模块）
```javascript
[[pop.js]]
const bar = require('./bar.js') // 这里的bar就是module.exports指向的对象
```
> PS：在Node中真正用于导出的其实根本不是exports，而是module.exports；CommonJS中是没有module.exports的概念的，但为了实现模块的导出，Node中使用的是Module的类，每一个模块都是Module的一个实例，也就是module；
> 为何exports也可以导出：因为module对象的exports属性是exports对象的一个引用
> PS：注意 基本数据类型 和 引用类型 的修改。

#### require的查找规则
[node.js文档](https://nodejs.org/dist/latest-v14.x/docs/api/modules.html#modules_all_together)
导入格式如下：`require(X)`

- 情况一：X是一个核心模块，比如path、http
   - 直接返回核心模块，并且停止查找
- 情况二：X是以 ./ 或 ../ 或 /（根目录）开头的
   - 查找目录下面的index文件
   - 1> 查找X/index.js文件
   - 2> 查找X/index.json文件
   - 3> 查找X/index.node文件
   - 1.如果有后缀名，按照后缀名的格式查找对应的文件
   - 2.如果没有后缀名，会按照如下顺序：
   - 1> 直接查找文件X
   - 2> 查找X.js文件
   - 3> 查找X.json文件
   - 4> 查找X.node文件
   - 第一步：将X当做一个文件在对应的目录下查找；
   - 第二步：没有找到对应的文件，将X作为一个目录
   - 如果没有找到，那么报错：not found
- 情况三：直接是一个X（没有路径），并且X不是一个核心模块
   - 比如 /Users/coderwhy/Desktop/Node/TestCode/04_learn_node/05_javascript-module/02_commonjs/main.js中编写 require('why')

![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1617085431856-c8da8369-96ff-4a89-820e-820413711c81.png#crop=0&crop=0&crop=1&crop=1&height=130&id=QmHJQ&margin=%5Bobject%20Object%5D&name=image.png&originHeight=259&originWidth=1080&originalType=binary&ratio=1&rotation=0&showTitle=false&size=235435&status=done&style=none&title=&width=540)

   - 如果上面的路径中都没有找到，那么报错：not found
#### 模块加载顺序
结论一：模块在被第一次引入时，模块中的js代码会被运行一次
结论二：模块被多次引入时，会缓存，最终只加载（运行）一次

- 因为每个模块对象有一个loaded属性，true表示已经加载

结论三：如果有循环引入，那么加载顺序是什么？

- Node采用的是深度优先算法：main -> aaa -> ccc -> ddd -> eee ->bbb
- ![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1617086026329-cd5a6b56-a2b6-4b79-a73b-11078b18b4b1.png#crop=0&crop=0&crop=1&crop=1&height=191&id=LIQjX&margin=%5Bobject%20Object%5D&name=image.png&originHeight=338&originWidth=422&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19560&status=done&style=none&title=&width=239)
#### AMD和CMD规范
CommonJS规范的缺点：
加载模块是同步的，只有模块加载完毕，模块中内容才能运行。这个在服务器中不是问题，因为服务器加载的是本地文件，啪一下很快。但在浏览器中通常不使用CommonJS规范，早期在浏览器使用的是AMD和CMD规范。
目前，绝大部分已不使用AMD/CMD，可供使用的一种是ES6 Module，一种是借助webpack来对CommonJS语法的转换。
暂时不对AMD/CMD语法进行介绍。
#### ES6 Module
ES Module与CommonJS的不同之处：

- 语法不同：使用`import`和`export`关键字；
- 采用**编译期静态类型检测**，并且**动态引用**的方式；
- 自动采用严格模式；

ES Module和CommonJS的区别：

| 区别 | ES Module | CommonJS |
| --- | --- | --- |
| 语法 | import、export | module.exports、exports、require |
| 加载 | 编译时加载（静态解析）、异步（不会阻塞） | 运行时加载、同步（加载完才能继续） |
| 导出 | export导出的是变量本身的引用，不是对象。（js引擎将创建模块环境记录，和变量进行实时绑定，即模块中的值更新，则引入的地方也会实时更新） | module.exports导出的是一个对象（意味着可以将这个对象的引用在其他模块中赋值给其他变量，最终他们指向的都是同一个对象） |

![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1617091643666-6cd93436-4f07-4a59-bbde-c457ba33dd17.png#crop=0&crop=0&crop=1&crop=1&height=248&id=NRxlk&margin=%5Bobject%20Object%5D&name=image.png&originHeight=330&originWidth=724&originalType=binary&ratio=1&rotation=0&showTitle=false&size=33621&status=done&style=none&title=&width=543)
使用介绍：
代码结构如下：
├── index.html
├── main.js
└── modules
    └── foo.js
在index.html中引入两个文件作为模块：
`<script src="./modules/foo.js" type="module"></script>`
`<script src="main.js" type="module"></script>`
会提示[CORS错误](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Modules#故障排除)，这是由于JS模块安全性要求，需要通过服务器来进行测试。使用VScode中的插件`Live Server`，然后在文件中右键选择"Open with Live Server"
语法介绍：
`**export**`
```javascript
// 法1
export const name = 'coderwhy';
export function sayHello(name) {
  console.log("Hello " + name);
}
// 法2  可列在一起
const name = 'coderwhy';
let message = "my name is why";
export {
  name,
  message,
}
// 法3  导出时可起别名
export {
  name as fName,
  message as fMessage,
}
```
`**import**`
```javascript
// 法1  import {标识符列表} from '模块'
import { name, age, message, sayHello } from './modules/foo.js'
// 法2  导入时给标识符起别名
import { name as wName, message as wMessage } from './modules/foo.js'
// 法3  将模块功能放到一个模块功能对象（a module object）上
import * as foo from './modules/foo.js'
console.log(foo.name);
console.log(foo.message);
```
**import和export的结合使用**
在开发和封装一个功能库时，通常我们希望将暴露的所有接口放到一个文件中，这时候就可以将该文件做一个中转。
```javascript
// bar.js
export const sum = function(num1, num2) {
  return num1 + num2;
}

// foo.js 将所有接口暴露在该文件中
export { sum } from './bar.js';

// main.js
export { sum as barSum } from './bar.js';
```
**默认导出 - 每个模块只能有一个**
```javascript
// bar.js
export default const name = 'lch';
// main.js  导出不用括号
import pname from './bar.js'
console.log(pname) // 'lch'
```
由于ES采用编译期静态类型检测，并且动态引用的方式，因此：
```javascript
// ----- 错误，因为ES Module在被JS引擎解析时，就必须知道它的依赖关系
if (true) {
  import sub from './modules/foo.js';
}
// ------- 错误，必须到运行时能确定path的值
const path = './modules/foo.js';
import sub from path;
```
那如何动态加载模块呢？需要使用`import()`函数来实现动态加载。
```javascript
// aaa.js
export function aaa() {
  console.log("aaa被打印");
}
// bbb.js
export function bbb() {
  console.log("bbb被执行");
}
// main.js
let flag = true;
if (flag) {
  import('./modules/aaa.js').then(aaa => {
    aaa.aaa();
  })
} else {
  import('./modules/bbb.js').then(bbb => {
    bbb.bbb();
  })
}
```
**在Node中支持ES Module**
在current版本中：

- 法一：在package.json中配置 type: module
- 法二：文件以 .mjs 结尾，表示使用的是ES Module

在LTS版本中：
可以正常运行的，但是会报一个警告。
### 




