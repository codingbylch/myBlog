---
title: "coderwhy_React Router"
date: 2020-07-06T00:00:00+08:00
draft: false
tags: ['React', 'coderwhy', '基础']
categories: ['前端']
---

> 参考资料：[掌握react-router - coderwhy](https://mp.weixin.qq.com/s/uwKGJ5Uy49q6Jgxbyi7XGQ)

## 路由的发展
路由器主要维护的是一个映射表，映射表会决定数据的流向。
web的发展经历了这样的阶段：

- 后端路由阶段；
- 前后端分离阶段；
- 单页面富应用（SPA）；

因此，路由的发展也经历了几个阶段：
**阶段一：后端路由阶段**
早期的HTML页面是由服务器来进行渲染的，服务器渲染完页面然后将页面代码返回给客户端进行展示（服务端渲染）。每个页面就对应这一个URL，当点击一个含有URL的a标签时，就会发送给服务器，服务器进行URL正则匹配，经过一系列处理后，服务端返回HTML页面给客户端。即输入URL ---> 返回HTML页面代码
优点：返回的页面无需再加载其它JS、CSS文件，也有利于SEO优化。
缺点：页面模块由后端人员维护；前端若想开发页面，需要通过PHP、java等语言编写页面代码；html代码、数据、逻辑会混合在一起。
**阶段二：前后端分离**
随着Ajax的出现，有了前后端分离的开发模式。后端提供API返回数据，前端通过Ajax请求数据。
优点：分工明确，责任清晰，后端专注于数据，前端专注于可视化与交互。
缺点：
**阶段三：单页面富应用（SPA）**
整个Web应用只有实际上只有一个页面，当URL发生改变时，并不会从服务器请求新的静态资源；而是通过JS监听URL的改变，根据URL不同渲染新的页面。
前端路由原理
通过JS监听URL的改变，可以使页面不刷新，不刷新就意味着不会从服务器请求新的静态资源。

1. **URL的hash**

hash也就是锚点（#），本质上是改变window.location.href（地址栏上的路径）；可以通过给location.hash给hash赋值来改变href：
```javascript
// 原先的href为https://lchblog.xyz/
// 赋值
window.location.hash = '123'
// 地址会变为https://lchblog.xyz/#123
```
通过**监听hashchange事件**来改变页面内容：
```javascript
// 2.监听hashchange
window.addEventListener("hashchange", () => {
  switch(location.hash) {
    case "#/home":
      routerViewEl.innerHTML = "home";
      break;
    case "#/about":
      routerViewEl.innerHTML = "about";
      break;
    default:
      routerViewEl.innerHTML = "default";
  }
})
```
优点：兼容性好，但地址栏会由#号出现，不够美观。

2. **HTML5的History**

是HTML5新增的接口，有5种方法：back()、forward()、go()、pushState()、replaceState()。
还有一个popState事件：xx.addEventListener('popState', callback)
```html
<div id="app">
  <a href="/home">home</a>
  <a href="/about">about</a>
  <div class="router-view"></div>
</div>

<script>
  // 1.获取router-view
  const routerViewEl = document.querySelector(".router-view");

  // 2.监听所有的a元素
  const aEls = document.getElementsByTagName("a");
  for (let aEl of aEls) {
    aEl.addEventListener("click", (e) => {
      e.preventDefault();
      const href = aEl.getAttribute("href");
      console.log(href);
      history.pushState({}, "", href);
      historyChange();
    })
  }

  // 3.监听popstate和go操作
  window.addEventListener("popstate", historyChange);
  window.addEventListener("go", historyChange);

  // 4.执行设置页面操作
  function historyChange() {
    switch(location.pathname) {
      case "/home":
        routerViewEl.innerHTML = "home";
        break;
      case "/about":
        routerViewEl.innerHTML = "about";
        break;
      default:
        routerViewEl.innerHTML = "default";
    }
  }

</script>
```
目前三大前端框架都有自己的路由实现：vue-router、react-router、ng-router
React Router的版本4开始，路由不再集中在一个包中进行管理了：

- react-router是router的核心部分代码；
- react-router-dom是用于浏览器的；
- react-router-native是用于原生应用的；
## 不使用React router
不使用react router时，如上文所说的，一是利用监听hashChange事件，二是利用HTML5的history这个API。react-router就是基于这两种模式上实现的。
## React-router的API

- BrowserRouter或HashRouter
   - Router中包含了对路径改变的监听，并且会将相应的路径传递给子组件；
   - BrowserRouter使用history模式；
   - HashRouter使用hash模式；
- Link和NavLink：
   - 通常路径的跳转是使用Link组件，最终会被渲染成a元素；
   - NavLink是在Link基础之上增加了一些样式属性；
   - to属性：Link中最重要的属性，用于设置跳转到的路径；
- Route：
   - Route用于路径的匹配；
   - path属性：用于设置匹配到的路径；
   - component属性：设置匹配到路径后，渲染的组件；
   - exact：精准匹配，只有精准匹配到完全一致的路径，才会渲染对应的组件；
## Route标签
### Route的三种渲染方式

1. **component**: 这是最常用也最容易理解的方式，给什么就渲染什么。
```jsx
const Home = () => (
  <div>
    <h2>Home：第一种渲染方法</h2>
  </div>
);
{/*......*/}
<Route path="/" exact component={Home}></Route>
```

2. **render**: render的类型是function，Route会渲染这个function的返回值。
```jsx
// router.js
<Route path="/about" render={()=><div>关于页面</div>} />
<Route path="/demo" render={(props)=><Demo {...props} />} />                          
```
此时就可以在Demo里接收参数：
props：{ history: {…}, location: {…}, match: {…}, staticContext: undefined }
```jsx
// Demo.js
const Demo = (props)=>{
  console.log(props) //可以读到props的信息:
	return(
  	<div>
    	Demo
    </div>
  )
}
```

3. **children**: 这是最特殊的渲染方式。
   - 它同render类似,是一个function。不同的地方在于它会被传入一个match参数来告诉你这个Route的path和location匹配上没有。
   - 第二个特殊的地方在于，即使path没有匹配上，我们也可以将它渲染出来。秘诀就在于前面一点提到的match参数。我们可以根据这个参数来决定在匹配的时候渲染什么，不匹配的时候又渲染什么。
```jsx
// 在匹配时，容器的calss是light，<Home />会被渲染
// 在不匹配时，容器的calss是dark，<About />会被渲染
<Route path='/home' children={( match ) => (
    <div className={match ? 'light' : 'dark'}>
      {match ? <Home/>:<About>}
    </div>
  )}/>
```
### 属性
#### exact
比如有/mine和/mine/dashboard这两个页面，若没有加exact，当访问/mine时，两个路径都会匹配到。
#### strict
更加精准的匹配，生效以后，比如路径为/mine，但输入网址是/mine/，则访问不到。
需要和exact一起使用。
## Link标签
路由访问链接，将会以a标签的形式展示，可以单独写进一个页面，然后在路由页面引入。
### 属性

- to：用于设置跳转到的路径，超级重要；
### NavLink高亮
案例：**路径选中时，对应的a元素变为红色**
#### 属性

- activeStyle：活跃时（匹配时）的样式；
- activeClassName：活跃时添加的class；
- exact：是否精准匹配；
```jsx
import "./Router.css"
...
<NavLink to="/form" activeClassName="hurray">
  Form
</NavLink>
<NavLink to="/form" activeStyle={{color: 'red'}}> // 第二种形式
  Form
</NavLink>
```
## Switch标签
默认情况下，react-router中只要是路径被匹配到的Route对应的组件都会被渲染；
但实际开发中，我们一般只想匹配到找到的第一个路由，这时候就需要使用Switch标签进行包裹。
```html
<Switch>
  <Route exact path="/" component={Home} />
  <Route path="/about" component={About} />
  <Route path="/profile" component={Profile} />
  <Route path="/:userid" component={User} />
  <Route component={NoMatch} />
</Switch>
```
## 动态路由
动态路由指的是路由中的路径是不固定的，如将**Route**中的path属性写成： `/detail/:id` ，那么"/detail/abc"、"/detail/123"都能匹配该Route，这个`/detail/:id`匹配规则，我们称之为动态路由。
### 跳转传参
```jsx
<Route strict exact path="/counter/:id" component={Counter}></Route>
```
然后访问/counter/1001，在this.props.match.params.id就能得到1001。
但如果访问/counter（即没有传参），则无法匹配路由。
这时候需要在id后面添加“？”，这时候访问/counter就能匹配无参数传递的情况了：
```jsx
<Route strict exact path="/counter/:id?" component={Counter}></Route>
```
也可以传多个参数：
```jsx
<Route strict exact path="/counter/:id?/:name?" component={Counter}></Route>
```
?name=xxx&age=20的数据形式如何读取，通过location.search进行获取：
```jsx
// 法1
const nameParams = new URLSearchParams(this.props.location.search);
console.log(nameParams.get("name"));
// 法2
const nameParams = querystring.parse(this.props.location.search);
console.log(nameParams); // {?name:"233"}
```
另一种形式的传参，to属性的另一种形式：
```jsx
<Link
  to={{
    // /tab?sort=name#the-hash
    pathname: "/tab",
      search: "?sort=name",
        hash: "the-hash", 
          state: { // 隐形传递参数
            flag: "flag",
              mine: "mine",
          },
          }}
  >
  TabControl
</Link>
```
### 动态传值
分三步：

1. 在Route上设置允许动态传值：
```jsx
<Route path="/list/:id" component={List} />
```

2. Link上传递值
```jsx
<li><Link to="/list/123">列表</Link> </li>
```

3. 在List组件上接收并显示传递值
```jsx
// List.jsx
console.log(this.props.match)
```
在this.props.match中包含三项：

- path: "/counter/:id?"，显示的就是Route上配置的path
- url: "/counter/060213"，真实的路径
- isExact: true，是否精确匹配
- params: {id: "060213"}，参数对象，就是匹配规则冒号后的id
#### 模拟一个列表数组
```jsx
// 关键代码
{countList.map((item) => {
  return (
    <li key={item.bookId}>
      <Link to={`/counter/${item.bookId}`}>
        {item.bookName},{item.price}
      </Link>
    </li>
  );
})}
```
## Redirect重定向

- 标签式重定向
```jsx
// 访问/form，则会跳转到/counter
<Redirect from="/form" to="/counter"></Redirect>
<Route path="/form" component={Form}></Route>
```
上面的代码路由会匹配第2行，因为按顺序匹配。
登录案例：
```jsx
// App.js
<Switch>
  ...其他Route
  <Route path="/login" component={Login} />
  <Route component={NoMatch} />
</Switch>

// User.js
render() {
  return this.state.isLogin ? (
    <div>
      <h2>User</h2>
      <h2>用户名: coderwhy</h2>
    </div>
  ): <Redirect to="/login"/>
}
```

- 编程式重定向：push和replace
```jsx
// 子组件form.js
<button onClick={this.backClick}>回到home</button>
backClick = () => {
  this.props.history.push("/"); // push是叠加的，上一页面还存在
  this.props.history.replace("/"); // 是替换，上一页面不存在
};
```
## withRouter
组件若是没有被路由直接管理（也就是说不是<Route component={AppXXX}>的形式，即不是通过路由直接跳转额），**则在props中没有包含路由对象**。若想包含路由对象，则需要使用高阶组件withRouter。
```jsx
// 父组件是Form.jsx，子组件是FormItem.jsx
// 父组件有被路由管理，而子组件没有，则在子组件中加入
import { withRouter } from "react-router-dom";
...
export default withRouter(FormItem);
```
## Prompt
在离开页面之前可以做一些事情，比如说提交表单的页面，要离开的时候提示数据未保存之类的是够确认退出的情况。
```jsx
import { Prompt } from "react-router-dom";
<Prompt when={!!name} message="数据未保存，是否要离开" />
<input
  type="text"
  value={name}
  onChange={(e) => this.setState({ name: e.target.value })}
  />
```
## react-router-config
以上的操作都是在Route组件上添加属性完成的，这样显得十分混乱，于是希望将路由配置放入一个集中的地方进行管理。
**使用步骤**：

1. 安装react-router-config
```javascript
npm install react-router-config
```
## 后台动态获取路由进行配置
```jsx
// router.jsx
let routeConfig = [
  { path: "/book", title: "Book页面", exact: true, component: Book },
  { path: "/tab", title: "tab页面", exact: false, component: TabControl },
  {
    path: "/course",
    title: "课程页面",
    exact: false,
    component: Course,
  },
];
...
// 循环Link
<ul>
  {routeConfig.map((item) => {
    return (
      <li>
        <Link key={item.path} to={item.path}>
          {item.title}
        </Link>
      </li>
    );
  })}
</ul>

// 循环Route
<Switch>
  {routeConfig.map((item) => {
    return (
      <Route
        exact={item.exact}
        key={item.path}
        path={item.path}
        component={item.component}
        />
    );
  })}
</Switch>
```
> 值得读的资料：
> 1. [在react项目当中做“导航守卫”](https://www.cnblogs.com/zhuhuoxingguang/p/10906337.html)
















