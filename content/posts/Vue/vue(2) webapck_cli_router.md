---
title: "vue(2) webapck_cli_router"
date: 2021-03-25T00:00:00+08:00
draft: false
tags: ['Vue', 'coderwhy', '基础']
categories: ['前端']
---

### 三. webpack
[webpack配置生成器](https://createapp.dev/webpack)
#### 3.1. 什么是webpack
首先理解前端模块化开发，前文已介绍。
**webpack的作用：**在前端开发中，webpak可以帮助我们进行模块化开发、处理模块之间的关系、将模块进行打包方便我们部署到服务器上。上文做的是js的模块化，在webpack中，js/css/图片/json文件等都可以当作模块使用。

**打包的概念：**将各种资源模块进行合并成1个或多个包(Bundle)，打包过程中还可以对资源进行处理，如图片压缩、语法转换等。

- webpack和gulp对比
> grunt/gulp被称为前端自动化任务管理工具，通过配置一系列任务来自动化完成压缩合并、转化语法等，并没有涉及模块化；而模块化管理是webpack的核心，打包、压缩等是附带的功能，更强大。

- webpack依赖环境

首先需要安装nodeJS，nodejs自带npm软件包管理工具

- 安装webpack

需要先全局安装，还需要再局部安装。因为在终端执行的webapck命令，来自全局安装；
当在package.json中定义了script对象时，包含webpack命令，终端使用npm run xxx，则使用的是局部；
`node -v 查看node版本`
`npm install webpack -g 全局安装webapck`
`npm install webpack --save-dev 局部安装webpack到dev`
#### 3.2. webpack的起步

- webpack命令
- webpack配置: webpack.config.js/package.json(scripts)
- 文件解析：
   - dist文件夹：存放打包后的文件
   - src文件夹：存放写的源文件
      - main.js：项目入口文件
   - index.html：浏览器打开展示的html
   - package.json：由npm init生成，npm包管理的文件

**使用模块化语法对js进行导出，可以直接使用吗？webpack有什么作用？** 不可以，因为浏览器不识别其中模块化的代码，需要webpack**对这些js文件进行打包**，随后在index.html中引入<script src="./dist/bundle.js"></script>，才可运行。
另外，项目中写了很多js文件，手动一个个引用非常麻烦，wepback可以将这些js文件进行打包成bundle.js之类的文件，**js引入变得十分方便**。而且js文件之间的依赖错综复杂，webpack可以帮我们**自动处理js依赖关系**。
（依赖指的就是js中引入其它js文件）
手动打包命令：`webpack src/main.js dist/bundle.js`，这里有入口文件、出口文件
可以对webpack进行配置：

1. 在根目录创建文件：webpack.config.js，webpack会自动查找该文件的
```javascript
const path = require('path')
module.exports = {
  entry: './src/main.js', // 入口文件
  output: { // 出口文件配置
    path: path.resolve(__dirname, 'dist'), // 动态获取当前文件路径，是绝对路径
    filename: 'bundle.js'
  }
}
```
**为何要安装局部webpack？**因为一个项目本身就包含webpack版本号，在package.json里，执行`npm i webpack -S-D`安装局部webpack，这样可以避免由于版本号不同而导致的未知错误。

**package.json中script脚本执行顺序：**

1. 先寻找本地node_modules/.bin路径中对应命令；
1. 若1没有，去全局环境变量中寻找；

#### 3.3. webpack的loader
由于webpack本身能力有限（处理引入、依赖、打包）本身只能处理js和json结尾的文件，处理其他文件需要进行能力扩展，**loader就是webpack扩展**。
**loader作用：语法转换（转换器）、不同文件处理**
**loader的使用：**

1. npm安装所需loader
1. 在webpack.config.js下的modules关键字中进行配置

webpack中文官网：[https://webpack.docschina.org/](https://webpack.docschina.org/)，相关loader和配置都有详细介绍。
**
**有哪些常用Loader：**

- **css-loader**（负责加载css文件）、**style-loader**（负责将css样式嵌入到文档中），两者需要一起使用方可正确加载css模块。
- **less-loader、less** （加载、处理less文件）
- **url-loader、file-loader**（加载、处理图片文件）
- **babel-loader**（语言转换）
> 具体步骤，使用css-loader和style-loader：
> 1. 打开Webpack官网，查找相应loader；
> 1. 安装：`npm install css-loader --save-dev`
> 1. 配置：webpack.config.js中配置module关键字，详看官网
> 1. 引入：引入需要使用的css
> 1. 编译：`npm run build`
> 
ps：
> 1. 在webpack 4.0+，终端直接输入webpack进行编译会报错，需要在packeage.json中配置script关键字：
> 
`"dev":"webpack --mode development",`
> `"build":"webpack --mode production",`
> 2. 使用多个loader时，从右向左读取

#### 3.4. webpack中配置Vue
步骤：

1. 安装：`npm install -S vue`
1. 引入：在main.js中引入vue：`import Vue from 'vue'`
1. 创建vue实例：`const myvue = new Vue({......})`
1. 在index.html中添加id：`<div id='app'></div>`，和先前写vue实例一样
1. 在webpack.config.js中添加：
```javascript
  resolve: {
    alias: {
      vue$: "vue/dist/vue.esm.js", // 指定一下vue为runtime-complier，默认为runtime-only
    },
  },
```

6. 编译：`npm run build`，浏览器运行index.html

ps：关于第5点的说明：[https://blog.csdn.net/wxl1555/article/details/83187647](https://blog.csdn.net/wxl1555/article/details/83187647)
ps：注意runtime-only（不让使用template）和runtime-complier（允许使用template）

#### 3.5. el和template有什么区别？
在main.js中的vue实例中添加template关键字，并设置一些样式代码，vue内部会自动将template替换index.html中id为app的标签。
好处：不用再去手动修改index.html，如下图所示：
![](https://s2.loli.net/2022/07/12/8vhM7J4NsrSw5PX.png)
然后，可以再对template进行抽取，最终抽取到App.vue文件的模板中（webpack编译时会将其自动转化），这里识别vue文件需要安装相应的扩展：（这一点详看课程 - Vue终极解决方案 一节）
App.vue：
![](https://s2.loli.net/2022/07/12/baYe3EhNoCscyuB.png)
这里需要安装相应的loader并配置，来处理以.vue结尾的文件：
`npm i vue-loader vue-template-compiler -D`
然后在webpack.config.js中添加rules：
![](https://s2.loli.net/2022/07/12/LeUxpCR1ldOAiIr.png)
PS：Vue-loader在15.*之后的版本都是 vue-loader的使用都是需要伴生 VueLoaderPlugin的，因此还需要安装：
`npm i vue-loader-plugin -D`，并在webpack.config.js中加入：
`plugins: [new VueLoaderPlugin()],`
PS：^和~的区别：
~会更新到中间数字的最新版本，如~2.4.0，就会更新到2.4.x的版本，而不会到2.5.x；
^会更新到第一个数字的最新版本，如^2.4.0，会更新到2.x.x的版本，但不会到3.x.x；
#### 3.5. webpack的plugin
plugin是插件的意思，对webpack现有功能进行扩展。如打包优化，文件压缩、资源管理，注入环境变量等。
loader的区别？loader用于转换，plugin用于对webpack本身功能增强。
使用步骤：

1. 通过npm安装：
1. 在webpack.config.js中的plugins关键字进行配置

有哪些plugin插件，在[webpack](https://webpack.docschina.org/plugins/)可以查询：

1. 添加版权的Plugin：BannerPlugin
1. 打包html：HtmlWebpackPlugin
1. js文件等压缩的插件：uglifyjs-webpack-plugin
#### 3.6. 搭建本地服务器

- webpack-dev-server

新版本好像配置有点点不同，需要再研究一下。webpack4.0+：webpack --mode development，好像没法加--open之类的。
#### 3.7. 配置文件的分离

### 四. Vue CLI脚手架
#### 4.1. 什么是CLI

- 脚手架是什么东西
> vue的脚手架，自动生成项目的相关开发环境和配置，无需再自己手动配置目录结构、webpack的配置等。上文学习了webpack的相关配置，但每个项目不可能都亲自一个个配置，这时候就用到脚手架cli

- **CLI依赖webpack、node、npm**

- 安装CLI3
> 安装cli3：`npm install @vue/cli -g`
> 还能拉取CLI2模块，从而使用cli2的模板：
> `npm i @vue/cli-init -g`
> 然后可以使用vue-cli2的模板：
> `vue init webpack my-project`
> vue-cli使用指南：[https://cli.vuejs.org/zh/guide/](https://cli.vuejs.org/zh/guide/)

#### 4.2. CLI2初始化项目的过程
![](https://s2.loli.net/2022/07/12/G1ti4ZFr2sy5pmn.png)
#### 4.3. CLI2生产的目录结构的解析
![](https://s2.loli.net/2022/07/12/3wRDBFO4aLpESCe.png)
export(导出)/import(导入)

.vue

dist -> distribution(发布)

> webpack ./src/main.js ./dist/bundle.js


开发时依赖

运行时依赖

### 一. Vue CLI

#### 1.1. runtime-compiler和runtime-only的区别
在文件中的区别：（only代码量更少）

- runtime-compiler:使用components属性注册App组件，在template属性是使用App组件。
- runtime-only:直接使用render属性调用h()函数使用App组件。

![](https://s2.loli.net/2022/07/12/MmPzRyNZtpOLfE6.png)

![](https://s2.loli.net/2022/07/12/iW5XCrHkGPemQMl.png)
性能上的区别：（only运行更快）

- runtime-compiler：`template ---> ast ---> render ---> vDom ---> 真实的Dom ---> 页面`
- runtime-only：`render ---> vDom ---> 真实的Dom ---> 页面`

本质区别：

- runtime-only：将template在打包的时候，就已经编译为render函数
- runtime-compiler：在运行的时候才去编译template

参考资料：[vue-cli/Runtime-Compiler和Runtime-Only的区别](https://www.cnblogs.com/jjbw/p/12129771.html)
结论： 发布生产的时候，runtime-only 构建的项目代码体积小运行速度快，**推荐使用runtime-only构建项目**

- ESLint到底是什么?
- template -> ast -> render -> vdom -> 真实DOM
- render: (h) => h, -> createElement

#### 1.2. Vue CLI3

- 如何通过CLI3创建项目
- CLI3的目录结构
- 配置文件: 1.Vue UI 2.隐藏的配置文件 3.自定义vue.config.js

### 二. Vue-Router
[vue-router官网](https://router.vuejs.org/zh/installation.html)
[vue-router相关API](https://router.vuejs.org/zh/api/)
API思维导图：
[Vue-router.xmind](https://www.yuque.com/attachments/yuque/0/2021/xmind/714353/1614848167615-4db42d4c-287d-40cc-b204-ba12c15c3db2.xmind)
#### 2.1. 什么是前端路由

- 后端渲染/后端路由
- 前后端分离
- SPA\前端路由

前端路由的两种模式：

- hash模式：带"#"（锚点），本质上是改变window.location.href
- history模式：H5新增接口，有几种方法可供使用：
   - pushState('/home')
   - replaceState('/home')
   - go(number)
   - back == go(-1)
   - forward == go(1)
#### 2.2. 路由的基本配置

- 安装vue-router

`npm install vue-router`

- Vue.use -> 创建VueRouter对象 -> 挂在到Vue实例上
- 配置映射关系: 1.创建组件 2.配置映射关系 3.使用router-link/router-view

如图所示：
![](https://s2.loli.net/2022/07/12/OEZgxCy61drDYm2.png)
#### 2.3. 细节处理

- 默认路由: redirect，在配置映射关系中加入：

![](https://s2.loli.net/2022/07/12/ZFsifLDmT1JgwSo.png)

- mode: history，选择路由模式
- router-link -> tag/replace/active-class
   - tag: tag可以指定<router-link>之后渲染成什么组件
   - replace: replace不会留下history记录
   - active-class：对应的路由匹配成功时, 会自动给当前元素设置一个router-link-active的class，默认已配置

其他router-link的API：[https://router.vuejs.org/zh/api/#router-link](https://router.vuejs.org/zh/api/#router-link)
#### 路由代码跳转
以上实现了基本的路由，点击路由链接，跳转到相应路由页面。但有时，不一定是点击router-link，而是按钮、自动跳转等情况，所以还有JS中进行路由跳转的方法：
`this.$router.push('/home')`
#### 路由懒加载
路由配置文件router/index.js中，若直接import很多组件，会影响页面加载。路由懒加载，就是需要时才引入，做法很简单：
原理：匹配到对应路由时，才会寻找相应的组件。

```javascript
    {
        path: '/datepiker',
        name: 'datepiker',
        component: () => import('../views/DatePiker.vue') // 路由懒加载，这里是ES6的写法
    },
```
#### 2.4. 动态路由
[https://router.vuejs.org/zh/guide/essentials/dynamic-matching.html#动态路由匹配](https://router.vuejs.org/zh/guide/essentials/dynamic-matching.html#动态路由匹配)

- /user/:id
- this.$route.params.id

其他路由对象属性：[https://router.vuejs.org/zh/api/#路由对象属性](https://router.vuejs.org/zh/api/#路由对象属性)
PS：动态路由也是传递数据的一种方式
PS：动态路由的点击跳转还不太会，疑问：传递为参数时，该如何进行表示？
#### 2.5. 路由参数的传递
传递参数的方式有2种类型：

- params的类型:
   - 配置路由格式: /router/:id
   - 传递的方式: 在path后面跟上对应的值
   - 传递后形成的路径: /router/123, /router/abc
- query的类型:
   - 配置路由格式: /router, 也就是普通配置
   - 传递的方式: 对象中使用query的key作为传递方式
   - 传递后形成的路径: /router?id=123, /router?id=abc

如何使用这些参数，也有2种方式：

- router-link

![](https://s2.loli.net/2022/07/12/TZXaEMUPcHAlzo3.png)

- JS代码

![](https://s2.loli.net/2022/07/12/YRG91oX6hTAwcmN.png)
参数成功传递，在对应组件中如何获取对应参数呢？
`$route.params和$route.query`
#### $route和$router
$route是指当前路由，一般用于获取当前路径、参数等；
$router是指路由器，一般进行路由跳转等；
两者的属性、方法都不同。
#### 2.6. 路由嵌套

- children: []，是一个数组

实现嵌套路由有两个步骤：

1. 创建对应的子组件, 并且在路由映射中配置对应的子路由

![](https://s2.loli.net/2022/07/12/KSelDBWaMEko7OA.png)

2. 在组件内部使用<router-view>标签
#### 2.7. 导航守卫
什么是导航守卫？什么时候使用？
> vue-router提供的导航守卫主要用来监听路由的进入和离开的
> vue-router提供了beforeEach和afterEach的钩子函数, 它们会在路由即将改变前和改变后触发
> 2.5.0+新增全局解析守卫beforeResolve，区别是在导航被确认之前，同时在所有组件内守卫和异步路由组件被解析之后，解析守卫就被调用。调用晚于beforeEach。
> 场景：进入某页面前判断用户是否登录。

导航钩子的三个参数解析：

- **to**： 即将要进入的目标的路由对象.
- **from**： 当前导航即将要离开的路由对象.
- **next**：调用该方法后, 才能进入下一个钩子.
   - next()：进行管道中的下一个钩子，若全执行完，状态未confirmed
   - next(false)：表示不跳转
   - next('/xxxx')：传入path，跳转到相应path
   - next(new Error())：传入一个Error实例
```javascript
// 全局前置导航守卫
router.beforeEach((to, from, next) => {
    window.document.title = to.meta.title || 'default';
    next(); // beforeEach需要调用，afterEach不需要调用
});
```

- 全局导航守卫

`beforeEach`、`beforeResolve` 、`afterEach`（全局后置钩子）

- 路由独享守卫

`beforeEnter`，与全局前置守卫的方法参数是一样的
![](https://s2.loli.net/2022/07/12/To6WcZD7Qayi1LX.png)

- 组件类守卫
   - `beforeRouteEnter`：在渲染该组件的对应路由被 confirm 前调用
   - `beforeRouteUpdate` ：在当前路由改变，但是该组件被复用时调用
   - `beforeRouteLeave`：导航离开该组件的对应路由时调用
```javascript
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```
完整导航解析流程：

1. 导航被触发。
1. 在失活的组件里调用 `beforeRouteLeave` 守卫。
1. 调用全局的 `beforeEach` 守卫。
1. 在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)。
1. 在路由配置里调用 `beforeEnter`。
1. 解析异步路由组件。
1. 在被激活的组件里调用 `beforeRouteEnter`。
1. 调用全局的 `beforeResolve` 守卫 (2.5+)。
1. 导航被确认。
1. 调用全局的 `afterEach` 钩子。
1. 触发 DOM 更新。
1. 调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数，创建好的组件实例会作为回调函数的参数传入。
#### 2.8. Keep-alive
可以使被包含的组件保留状态，或避免重新渲染。下一次点击，不进行重新渲染。
被缓存的组件会调用2个钩子：`activated`和`deactivated`钩子，进入组件时调用`activated`，离开时调用`deactivated`
keep-alive有2个属性，include和exclude
使用：
![](https://s2.loli.net/2022/07/12/bOk267MotpDZqu1.png)
可参考：[https://juejin.cn/post/6844903918313406472](https://juejin.cn/post/6844903918313406472)
#### 2.9. TabBar的封装过程
封装示意图：
![](https://s2.loli.net/2022/07/12/UzGxH5RZIoeWBvS.png)
目录结构：
![](https://s2.loli.net/2022/07/12/SkU7JrQs9IGipmZ.png)
这里的大视图还有组件最好都新建一个文件夹，这样项目大的时候便于管理：
![](https://s2.loli.net/2022/07/12/w8bfhSxdLWBjvpC.png)
各文件代码：
[src.zip](https://www.yuque.com/attachments/yuque/0/2021/zip/714353/1615255777071-df3087c2-bbe3-4cef-88bc-dc240f90db2d.zip?_lake_card=%7B%22uid%22%3A%221615255776977-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fzip%2F714353%2F1615255777071-df3087c2-bbe3-4cef-88bc-dc240f90db2d.zip%22%2C%22name%22%3A%22src.zip%22%2C%22size%22%3A51010%2C%22type%22%3A%22application%2Fx-zip-compressed%22%2C%22ext%22%3A%22zip%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22xPklC%22%2C%22card%22%3A%22file%22%7D)
其中slot插槽、CSS知识使用较多，学习封装组件的思想。

