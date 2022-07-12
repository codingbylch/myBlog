---
title: "尚硅谷_React(2) - 两个案例+路由"
date: 2021-08-25T00:00:00+08:00
draft: false
tags: ['React', '尚硅谷', '基础']
categories: ['前端']
---

> 源码：[react_staging.7z](https://www.yuque.com/attachments/yuque/0/2021/7z/714353/1623725678874-9e163b6b-5d52-4f60-92fa-2746657254ce.7z?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2F7z%2F714353%2F1623725678874-9e163b6b-5d52-4f60-92fa-2746657254ce.7z%22%2C%22name%22%3A%22react_staging.7z%22%2C%22size%22%3A179363%2C%22type%22%3A%22%22%2C%22ext%22%3A%227z%22%2C%22source%22%3A%22%22%2C%22status%22%3A%22done%22%2C%22mode%22%3A%22title%22%2C%22download%22%3Atrue%2C%22taskId%22%3A%22u19c90b98-133f-44f6-80d0-f855e90350c%22%2C%22taskType%22%3A%22upload%22%2C%22id%22%3A%22uea3ba6e8%22%2C%22card%22%3A%22file%22%7D)

### 官方脚手架
脚手架：用于帮助程序员快速创建一个基于XXX库的模板项目。
步骤：

1. 安装：`npm i create-react-app -g`
1. 创建项目目录：`create-react-app hello-react`
1. 进入目录：`cd hello-react`
1. 启动项目：`npm start`

脚手架项目结构：
![](https://s2.loli.net/2022/07/12/B9XQYC5xFaIOdVL.png)
功能界面的组件化编码流程：

1. 拆分组件；
1. 实现静态组件：使用组件实现静态页面效果；
1. 实现动态组件：动态显示初始化数据；交互；
### TodoList案例

1. 拆分组件、实现静态组件，注意：className、style的写法
1. 动态初始化列表，如何确定将数据放在哪个组件的state中？

——某个组件使用：放在其自身的state中
——某些组件使用：放在他们共同的父组件state中（官方称此操作为：状态提升）

3. 关于父子之间通信：
   1. 【父组件】给【子组件】传递数据：通过props传递
   1. 【子组件】给【父组件】传递数据：通过props传递，要求父提前给子传递一个函数
4. 注意defaultChecked 和 checked的区别，类似的还有：defaultValue 和 value
4. 状态在哪里，操作状态的方法就在哪里
### 脚手架配置代理
发送网络请求，发生了跨域，如localhost:3000给localhost:5000发请求。
在React中可以通过代理进行解决。这个代理就是中间的服务器
![](https://s2.loli.net/2022/07/12/jLYKrmcz6vWN7OH.png)
两种开启代理的方式：[react脚手架配置代理.md](https://www.yuque.com/attachments/yuque/0/2021/md/714353/1620043472139-6232b632-ecfd-4d8a-868a-4b4e9517c20c.md?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fmd%2F714353%2F1620043472139-6232b632-ecfd-4d8a-868a-4b4e9517c20c.md%22%2C%22name%22%3A%22react%E8%84%9A%E6%89%8B%E6%9E%B6%E9%85%8D%E7%BD%AE%E4%BB%A3%E7%90%86.md%22%2C%22size%22%3A1828%2C%22type%22%3A%22%22%2C%22ext%22%3A%22md%22%2C%22source%22%3A%22%22%2C%22status%22%3A%22done%22%2C%22mode%22%3A%22title%22%2C%22download%22%3Atrue%2C%22uid%22%3A%221620043472006-0%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22percent%22%3A0%2C%22id%22%3A%22Vimou%22%2C%22card%22%3A%22file%22%7D)
法一：package.json：
```javascript
// ... 添加
"proxy": "http://localhost:5000"
```
然后请求函数中的变化为：
```javascript
axios.get("http://localhost:5000/student") --- 原本请求5000端口
// 修改为
axios.get("http://localhost:3000/student") --- 改为请求3000端口，对应上图
```
但不是所有请求都转发给了端口5000，如请求：`"http://localhost:5000/index.html"`，因为本地端口3000下有public/index.html。（因此先会在本地找，找不到就去请求5000端口）

法二：若要请求多个不同的服务器，就不在package.json中配置了。
若想在react最新版脚手架配置，会比较麻烦点。
新建：`src/setupProxy.js`，React会自动查找该文件并执行
```javascript
// 需要使用CommonJS的语法
//react脚手架自带了, 前文的package.json中配置只是简略版
const proxy = require('http-proxy-middleware') 
module.exports = function (app){
  app.use(
  	proxy('api1',{ // 如果请求路径中包含了api1，就会进行转发
    	target: "http://localhost:5000", // 请求转发给谁
      changeOrigin: true, // 控制服务器收到的请求头中Host字段的值
      pathRewrite: {"^/api1": ""} // 再将路径中的api1进行改写为''
    }),
    proxy('api2',{ 
    	target: "http://localhost:5001",
      changeOrigin: true,
      pathRewrite: {"^/api2": ""} 
    }),
  )
}
```
### github搜索案例
#### 设计状态时要考虑全面
例如带有网络请求的组件，要考虑请求失败怎么办。List组件展示的信息的几种可能性：

1. 用户信息列表
1. 初始页面
1. 请求中的loading
1. 错误兜底页面

那就需要一些状态：
```jsx
export default class App extends Component {
    state = {
        users: [], // 用户信息
        isFirst: true, // 是否第一次打开页面
        isLoading: false, // 是否加载中
        err: "", // 存储请求相关错误信息
    };
    updateAppState = (stateObj) => {
        this.setState(stateObj);
    };
    render() {
        const { users } = this.state;
        return (
            <div className="container">
                <Search updateAppState={this.updateAppState} />
                <List {...this.state} />
            </div>
        );
    }
}
```
更新状态的时机：发送请求前；请求成功后；请求失败后；
另外还需要利用三元表达式来写对应状态时所展示的页面。
但这里的各个状态是写在App组件中的，即子传父，父再传子，略繁琐，可利用工具库实现任意组件间的通信。
#### ES6小知识点：解构赋值+重命名
```javascript
let obj = {a:{b:1}}
const {a} = obj; //传统解构赋值
const {a:{b}} = obj; //连续解构赋值
const {a:{b:value}} = obj; //连续解构赋值+重命名
```
#### 消息订阅与发布机制
> 即node中的event Emitter，在京购小程序首页中有类似应用

1. 先订阅，再发布（理解：有一种隔空对话的感觉）
1. 适用于任意组件间通信
1. 要在组件的componentWillUnmount中取消订阅

兄弟(任意)组件间的通信
消息发布与订阅机制，需要利用第三方工具。
先要订阅消息，定好消息名（你想要啥消息），之后再发布消息，才能收的到。
工具库：`[PubSubJS](https://github.com/mroderick/PubSubJS)`，github搜索然后照着安装即可；
```jsx
// 基本使用
// 订阅消息
var token = PubSub.subcribe("My TOPIC",(msg, data)=>{ // 消息名、回调
	console.log(msg, data);
})

// 发布消息
// 发布以后，订阅对应消息的地方就会执行对应的回调函数
PubSub.publish("My TOPIC", "hello world"); 

```
**重点：**谁接东西，谁就在componentDidMount里订阅消息，在componentWillUnmount取消订阅；谁想给别人传东西，谁就发布消息；
#### fetch发送请求（关注分离的设计思想）
发送ajax请求，除了`xhr`，还有`fetch`；xhr不符合“关注分离”的设计；
第一步：联系服务器是否成功；
第二步：联系成功后拿到数据；联系失败
联系服务器是否成功和服务器是否响应你的地址是两码事
```javascript
try {
  const response= await fetch(`/api1/search/users2?q=${keyWord}`)
  const data = await response.json()
  console.log(data);
} catch (error) {
  console.log('联系服务器失败、请求页面不存在等',error);
}
```

### React路由
每一个路径对应着一个组件，这就是一种映射关系。一个路由就是一个映射关系（`key:value`）。其中key为路径，value可能是function或component。
其中后端路由的key:value的value对应为function，前端为component
那如何实现的前端路由呢，即前端路由的原理？浏览器内置的`history`对象；
区分路由和路由器的概念。①点击导航链接引起路径变化，②路径变化被路由器监测到，展示相应组件。
#### 基本使用
步骤：

1. 安装：`npm install --save react-router`
1. 使用：
```jsx
// index.js
import { BrowserRouter } from "react-router-dom";
ReactDOM.render(
  <BrowserRouter> -- 需要包一下，history模式，一个路由器
    <App/>
  </BrowserRouter>,
  document.getElementById('root')
)

// App.jsx
import { Link, Route } from "react-router-dom";
// ...
{/* 原生html中，靠<a>跳转不同的页面 */}
{/* <a className="list-group-item" href="./about.html">About</a>
		<a className="list-group-item active" href="./home.html">Home</a> 
*/}

{/* 在React中靠路由链接实现切换组件--编写路由链接-- 1️⃣ */}
<Link className="list-group-item" to="/about">About</Link>
<Link className="list-group-item" to="/home">Home</Link>

{/* 注册路由 */} -- ②
<Route path="/about" component={About}/> -- 这里的About是路由组件
<Route path="/home" component={Home}/>
```
#### 一般组件和路由组件
一般组件和路由组件最大的区别：

1. 写法不同：

一般组件：`<Demo/>`
 路由组件：`<Route path="/demo" component={Demo}/>`

2. 存放位置不同：一般组件和路由组件放在不同的文件夹；一般组件放在component文件夹下，路由组建放在pages/Home，pages/About这样的形式；
2. 一般组件是写标签时传递什么就接收什么；路由组件会收到路由器传递的props：

![](https://s2.loli.net/2022/07/12/72yJs5pUwxXalLi.png)
```jsx
history:
    go: ƒ go(n)
    goBack: ƒ goBack()
    goForward: ƒ goForward()
    push: ƒ push(path, state)
    replace: ƒ replace(path, state)
location:
    pathname: "/about"
    search: ""
    state: undefined
match:
    params: {}
    path: "/about"
    url: "/about"
```
#### 封装NavLink
需要高亮效果使用`NavLink`标签代替`Link`标签，然后添加`activeClassName`。
```jsx
<NavLink activeClassName="atguigu" className="list-group-item" to="/about">About</NavLink>
```
可以封装一下NavLink让其更好用，封装为MyNavLink：
```jsx
标签体内容，是特殊的标签属性，这样title就可以写在标签体里了
<NavLink className="list-group-item" to="/home">Home</NavLink>
// 相当于
<NavLink className="list-group-item" to="/home" children="Home" />

export default class MyNavLink extends Component {
  render() {
    // console.log(this.props);
    return (
      <NavLink activeClassName="atguigu" className="list-group-item" {...this.props}/>
    )
  }
}
// 使用
<MyNavLink to="/about">About</MyNavLink>

```
#### Switch的使用
Switch组件让路由匹配后就不会再继续匹配，提高了匹配的效率。
```jsx
<Switch> // 引入，包一下就行
  <Route path="/about" component={About}/>
  <Route path="/home" component={Home}/>
  <Route path="/home" component={Test}/>
</Switch>
```
#### 解决样式丢失问题

1. `./`改为`/`
1. `./`改为`%PUBLIC_URL%/`
1. 使用hashrouter
#### 路由的模糊匹配与精确匹配
默认是模糊匹配，严格匹配不要随便开启，需要再开，有些时候开启会导致无法继续匹配二级路由；
```jsx
<MyNavLink to="/home/a/b">Home</MyNavLink>
<Route path="/home" component={Home}/> // 会匹配上
<Route exact path="/home" component={Home}/> -- 不会匹配
```
#### Redirect的使用
称为重定向，感觉像是兜底的人。一般写在所有路由注册的最下方，当所有路由都无法匹配时，跳转到Redirect指定的路由。
```jsx
<Switch>
  <Route path="/about" component={About}/>
  <Route path="/home" component={Home}/>
  <Redirect to="/about"/>
</Switch>
```
#### 嵌套路由（二级路由）
路由的匹配都是按照注册顺序走的，因此注册子路由时要写上父路由的path值。`/home/news`的路径会匹配`/home`和`/home/news`。
#### 向路由组件传递params参数
```jsx
// /Home/Message/index.jsx
{/* 向路由组件传递params参数 */}
<Link to={`/home/message/detail/${msgObj.id}/${msgObj.title}`}>{msgObj.title}</Link>
{/* 声明接收params参数 */}
<Route path="/home/message/detail/:id/:title" component={Detail}/>

// /Home/Message/Detail/index.jsx
// 这样路由组件的props就能收到params参数
const {id, title} = this.props.match.params
```
#### 向路由组件传递search参数
Tips：获取到的search是urlencoded编码字符串，需要借助querystring解析
```jsx
// /Home/Message/index.jsx
{/* 向路由组件传递search参数 */}
<Link to={`/home/message/detail/?id=${msgObj.id}&title=${msgObj.title}`}>{msgObj.title}</Link>
{/* search参数无需声明接收，正常注册路由即可 */}
<Route path="/home/message/detail" component={Detail}/>

// /Home/Message/Detail/index.jsx
// 接收search参数
const {search} = this.props.location // "?id=01&title=消息1"
const {id,title} = qs.parse(search.slice(1)) // react脚手架内置了qs库
// key=value&key=value 这种编码形式是urlencoded
// qs.stringify将对象转为urlencoded编码形式，qs.parse反之
```
#### 向路由组件传递state参数
Tips：刷新后location.state不丢，历史记录清除后刷新会丢，加上`{}`
```jsx
{/* 向路由组件传递state参数 */}
<Link to={{pathname:'/home/message/detail',state:{id:msgObj.id,title:msgObj.title}}}>{msgObj.title}</Link>
{/* state参数无需声明接收，正常注册路由即可 */}
<Route path="/home/message/detail" component={Detail}/>

// 接收state参数
const {id,title} = this.props.location.state || {}
const findResult = DetailData.find((detailObj)=>{
  return detailObj.id === id
}) || {}
```
#### push与replace模式
默认为push模式，添加`replace={true}`开启replace模式，进行栈顶替换。
```jsx
<Link replace={true} className="list-group-item" to="/about">About</Link>
```
#### 编程式路由导航
即不以标签的形式写路由跳转，而是在代码事件中进行路由跳转。
```jsx
pushShow = (id,title)=>{
  //push跳转+携带params参数
  // this.props.history.push(`/home/message/detail/${id}/${title}`)

  //push跳转+携带search参数
  // this.props.history.push(`/home/message/detail?id=${id}&title=${title}`)

  //push跳转+携带state参数
  this.props.history.push(`/home/message/detail`,{id,title})
}

replaceShow = (id,title)=>{
  //replace跳转+携带params参数
  //this.props.history.replace(`/home/message/detail/${id}/${title}`)

  //replace跳转+携带search参数
  // this.props.history.replace(`/home/message/detail?id=${id}&title=${title}`)

  //replace跳转+携带state参数
  this.props.history.replace(`/home/message/detail`,{id,title})
}

back = ()=>{
  this.props.history.goBack() // 后退
}

forward = ()=>{
  this.props.history.goForward() // 前进
}

go = ()=>{
  this.props.history.go(-2) // 后退2
}
```
#### withRouter的使用
用来干嘛的？只有路由组件的props才有接收到history等函数，一般组件没有；为**了让一般组件也能用上history等函数，就需要用到withRouter。**
```jsx
// Header/index.jsx
import {withRouter} from 'react-router-dom'

class Header extends Component{...}
// withRouter的返回值是一个新组件
export default withRouter(Header)
```
#### BrowserRouter与HashRouter区别

1. 底层原理不一样：

BrowserRouter使用的是H5的history API，不兼容IE9及以下版本。
HashRouter使用的是URL的哈希值。

2. path表现形式不一样

BrowserRouter的路径中没有#,例如：localhost:3000/demo/test
HashRouter的路径包含#,例如：localhost:3000/#/demo/test

3. 刷新后对路由state参数的影响

(1).BrowserRouter没有任何影响，因为state保存在history对象中。
(2).HashRouter刷新后会导致路由state参数的丢失！！！

4. 备注：HashRouter可以用于解决一些路径错误相关的问题。

#### antd组件库
流行的开源ReactUI组件库：

1. material-ui（国外）
1. ant-design（国内蚂蚁金服）











