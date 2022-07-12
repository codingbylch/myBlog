---
title: "尚硅谷_React(4) - 扩展"
date: 2021-08-25T00:00:00+08:00
draft: false
tags: ['React', '尚硅谷', '基础']
categories: ['前端']
---

## 1. setState
### setState更新状态的2种写法
```
	(1). setState(stateChange, [callback])------对象式的setState
            1.stateChange为状态改变对象(该对象可以体现出状态的更改)
            2.callback是可选的回调函数, 它在状态更新完毕、界面也更新后(render调用后)才被调用
					
	(2). setState(updater, [callback])------函数式的setState
            1.updater为返回stateChange对象的函数。
            2.updater可以接收到state和props。
            4.callback是可选的回调函数, 它在状态更新、界面也更新后(render调用后)才被调用。
总结:
		1.对象式的setState是函数式的setState的简写方式(语法糖)
		2.使用原则：
				(1).如果新状态不依赖于原状态 ===> 使用对象方式
				(2).如果新状态依赖于原状态 ===> 使用函数方式
				(3).如果需要在setState()执行后获取最新的状态数据, 
					要在第二个callback函数中读取
```
```javascript
// 第二种写法
this.setState((state, props)=>{
	return {count: state.count+1}
})
```

---

## 2. lazyLoad
### 路由组件的lazyLoad
```javascript
	//1.通过React的lazy函数配合import()函数动态加载路由组件 ===> 路由组件代码会被分开打包
	const Login = lazy(()=>import('@/pages/Login'))
	
	//2.通过<Suspense>指定在加载得到路由打包文件前显示一个自定义loading界面
	<Suspense fallback={<h1>loading.....</h1>}>
        <Switch>
            <Route path="/xxx" component={Xxxx}/>
            <Redirect to="/login"/>
        </Switch>
    </Suspense>
```

---

## 3. Hooks

#### 1. React Hook/Hooks是什么?
```
(1). Hook是React 16.8.0版本增加的新特性/新语法
(2). 可以让你在函数组件中使用 state 以及其他的 React 特性
```

#### 2. 三个常用的Hook
```
(1). State Hook: React.useState()
(2). Effect Hook: React.useEffect()
(3). Ref Hook: React.useRef()
```

#### 3. State Hook
```
(1). State Hook让函数组件也可以有state状态, 并进行状态数据的读写操作
(2). 语法: const [xxx, setXxx] = React.useState(initValue)  
(3). useState()说明:
        参数: 第一次初始化指定的值在内部作缓存
        返回值: 包含2个元素的数组, 第1个为内部当前状态值, 第2个为更新状态值的函数
(4). setXxx()2种写法:
        setXxx(newValue): 参数为非函数值, 直接指定新的状态值, 内部用其覆盖原来的状态值
        setXxx(value => newValue): 参数为函数, 接收原本的状态值, 返回新的状态值, 内部用其覆盖原来的状态值
```
```jsx
// 16.8以后有了hooks，从此函数式组件崛起了
import React from "react";
export default function Demo() {
  console.log("demo"); // 1+n
  const [count, setCount] = React.useState(0); // react底层做了处理，再次调用不会进行覆盖

  const add = () => {
    // setCount(count + 1);
    setCount((count) => count + 1); // 第二种写法
  };

  return (
    <div>
      <h2>和为：{count}</h2>
      <button onClick={add}>点击</button>
    </div>
  );
}
```
#### 4. Effect Hook
若第二个参数不设置，则默认监听全体；若为[]，只有页面初始化时被调用；若为[count]，只监听count的变化；
```
(1). Effect Hook 可以让你在函数组件中执行副作用操作(用于模拟类组件中的生命周期钩子)
(2). React中的副作用操作:
        发ajax请求数据获取
        设置订阅 / 启动定时器
        手动更改真实DOM
(3). 语法和说明: 
        useEffect(() => { 
          // 在此可以执行任何带副作用操作
          return () => { // 在组件卸载前执行
            // 在此做一些收尾工作, 比如清除定时器/取消订阅等
          }
        }, [stateValue]) // 如果指定的是[], 回调函数只会在第一次render()后执行
    
(4). 可以把 useEffect Hook 看做如下三个函数的组合
        componentDidMount()
        componentDidUpdate()
    	componentWillUnmount()
```
```jsx
React.useEffect(() => {
    const timer = setInterval(() => {
      setCount((count) => count + 1);
    }, 1000);
    return () => { // 组件被卸载后调用, 模拟componentWillUnmount
      clearInterval(timer);
    };
  }, []);
```
#### 5. Ref Hook

```
(1). Ref Hook可以在函数组件中存储/查找组件内的标签或任意其它数据
(2). 语法: const refContainer = useRef()
(3). 作用:保存标签对象,功能与React.createRef()一样
```
```jsx
const myRef = React.useRef();
<input type="text" ref={myRef} />
console.log(myRef.current.value);
```
#### React memo
在先前的类组件中，为了性能优化(不必要的重新渲染)，使用PureComponent来对props进行浅比较，而在shouldComponentUpdate中可进行更细致的比较；
在函数式组件中，为了达到类似效果，可使用React.memo，类似于PureComponent。可传递第二个参数，即一个回调函数，效果类似于shouldComponentUpdate中进行细致比较；
如果props是一个对象，浅比较其实是无效的，需要用到第二个参数进一步进行比较：
```jsx
import React, { memo, } from 'react';

const isEqual = (prevProps, nextProps) => {
  if (prevProps.number !== nextProps.number) {
    return false;
  }
  return true;
}

export default memo((props = {}) => {
  console.log(`--- memo re-render ---`);
  return (
    <div>
      {/* <p>step is : {props.step}</p> */}
      {/* <p>count is : {props.count}</p> */}
      <p>number is : {props.number}</p>
    </div>
  );
}, isEqual);
```
#### useMemo
在某些场景下，只是希望部分component不要进行re-render，而不是整个component不要re-render，这时候就可以用到useMemo：
```jsx
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```
useMemo返回的是一个memoized值，只有当依赖项发生变化时，memoized值才会发生变化。memoized不变，则不会重新渲染。
```jsx
import React, { useMemo } from 'react';

export default (props = {}) => {
    console.log(`--- component re-render ---`);
    return useMemo(() => {
        console.log(`--- useMemo re-render ---`);
        return <div>
            {/* <p>step is : {props.step}</p> */}
            {/* <p>count is : {props.count}</p> */}
            <p>number is : {props.number}</p>
        </div>
    }, [props.number]);
}
```

---

## 4. Fragment
### 使用
```
<Fragment><Fragment>
<></>
```
### 作用
> 可以不用必须有一个真实的DOM根标签了


---

## 5. Context
### 理解
> 组件实例的一个属性，是一种组件间通信方式, 常用于【祖组件】与【后代组件】间通信
> 某子组件被包裹后，其组件及其后代组件都能收到
> ![](https://s2.loli.net/2022/07/12/bxlFiQqrsYf4Tk5.png)

### 使用
```jsx
1) 创建Context容器对象：
	const MyContext = React.createContext()  
	
2) 渲染子组时，外面包裹xxxContext.Provider, 通过value属性给后代组件传递数据：
	<MyContext.Provider value={数据}>
		后代组件（后代类组件和函数式组件接收方式不太一样，见(3)）
  </MyContext.Provider>
    
3) 后代组件读取数据：

	//第一种方式:仅适用于类组件 
		static contextType = xxxContext  // 声明接收context
	  this.context // 读取context中的value数据
	  
	//第二种方式: 函数组件与类组件都可以
	  <xxxContext.Consumer>
	    {
	      value => ( // value就是context中的value数据
	        要显示的内容
	      )
	    }
	  </xxxContext.Consumer>
```
### 注意
```
在应用开发中一般不用context, 一般都用它的封装react插件
```
### 扩展
Context结合高阶组件，利用高阶组件来传递props，后代组件可以利用props获取来自父组件的值，避免了Context的“污染”：（也是高阶组件的应用之一）
```jsx
// 定义一个高阶组件
function withUser(WrappedComponent) {
  return props => {
    return (
      <UserContext.Consumer>
        {
          user => {
            return <WrappedComponent {...props} {...user}/>
          } 
        }
      </UserContext.Consumer>
    )
  }
}

// 创建Context
const UserContext = createContext({
  nickname: "默认",
  level: -1,
  区域: "中国"
});

class Home extends PureComponent {
  render() {
    return <h2>Home: {`昵称: ${this.props.nickname} 等级: ${this.props.level} 区域: ${this.props.region}`}</h2>
  }
}

const UserHome = withUser(Home);

class App extends PureComponent {
  render() {
    return (
      <div>
        App
        <UserContext.Provider value={{nickname: "why", level: 90, region: "中国"}}>
          <UserHome/>
        </UserContext.Provider>
      </div>
    )
  }
}

export default App;
```

---

## 6. 组件优化
### Component的2个问题
> 
> 1.  只要执行setState(),即使不改变状态数据, 组件也会重新render() ==> 效率低 
> 1.  只当前组件重新render(), 就会自动重新render子组件，纵使子组件没有用到父组件的任何数据 ==> 效率低 

### 效率高的做法
> 只有当组件的state或props数据发生改变时才重新render()

### 原因
> Component中的shouldComponentUpdate()总是返回true

### 解决
```
办法1: 
	重写shouldComponentUpdate()方法
	比较新旧state或props数据, 如果有变化才返回true, 如果没有返回false
办法2:  
	使用PureComponent
	PureComponent重写了shouldComponentUpdate(), 只有state或props数据有变化才返回true
	注意: 
		只是进行state和props数据的浅比较, 如果只是数据对象内部数据变了, 返回false  
		不要直接修改state数据, 而是要产生新数据
项目中一般使用PureComponent来优化
```

---

## 7. render props | 插槽
### 如何向组件内部动态传入带内容的结构(标签)?
```
Vue中: 
	使用slot技术, 也就是通过组件标签体传入结构  <A><B/></A>
React中:
	使用children props: 通过组件标签体传入结构
	使用render props: 通过组件标签属性传入结构,而且可以携带数据，一般用render函数属性
```

### children props
```
<A>
  <B>xxxx</B>
</A>
{this.props.children}
问题: 如果B组件需要A组件内的数据, ==> 做不到
```
```jsx
export default class Parent extends Component {
  render() {
    return (
      <div className="parent">
        <h3>我是Parent组件</h3>
        <A>
          <B/>
        </A>
      </div>
    )
  }
}
class A extends Component {
	render() {
		return (
			<div className="a">
				<h3>我是A组件</h3>
				{this.props.children}
			</div>
		)
	}
}
```
### render props
```
<A render={(data) => <C data={data}></C>}></A>
A组件: {this.props.render(内部state数据)}
C组件: 读取A组件传入的数据显示 {this.props.data}
```
```jsx
export default class Parent extends Component {
	render() {
		return (
			<div className="parent">
				<h3>我是Parent组件</h3>
				<A render={(name)=><C name={name}/>}/> // 这里的render是约定俗成的, peiqi也可以
			</div>
		)
	}
}
class A extends Component {
	state = {name:'tom'}
	render() {
		console.log(this.props);
		const {name} = this.state
		return (
			<div className="a">
				<h3>我是A组件</h3>
				{this.props.render(name)}
			</div>
		)
	}
}
```

---

## 8. 错误边界
#### 理解：
错误边界(Error boundary)：用来捕获后代组件错误，渲染出备用页面，把错误控制在一定范围之内，应该在容易发生错误的组件的父组件写。
只适用于生产环境，在开发环境过会儿还是会报错。
#### 特点：
只能捕获后代组件生命周期产生的错误，不能捕获自己组件产生的错误和其他组件在合成事件、定时器中产生的错误
##### 使用方式：
getDerivedStateFromError配合componentDidCatch
```javascript
// 生命周期函数，一旦后台组件报错，就会触发
static getDerivedStateFromError(error) {
    console.log(error);
    // 在render之前触发
    // 返回新的state
    return {
        hasError: true,
    };
}

componentDidCatch(error, info) { // 如果渲染组件出错，就会被调用
    // 统计页面的错误。发送请求发送到后台去
    console.log(error, info);
}
```

## 9. 组件通信方式总结

#### 组件间的关系：

- 父子组件
- 兄弟组件（非嵌套组件）
- 祖孙组件（跨级组件）

#### 几种通信方式：
```
	1.props：
		(1).children props
		(2).render props
	2.消息订阅-发布：
		pubs-sub、event等等
	3.集中式管理：
		redux、dva等等
	4.conText:
		生产者-消费者模式
```

#### 比较好的搭配方式：
```
	父子组件：props
	兄弟组件：消息订阅-发布、集中式管理
	祖孙组件(跨级组件)：消息订阅-发布、集中式管理、conText(开发用的少，封装插件用的多)
```

## 10. 高阶组件HOC
> 源码：[11_高阶组件的使用.zip](https://www.yuque.com/attachments/yuque/0/2021/zip/714353/1627528129205-ce58fbc6-3b7b-4f94-926d-3d09796fe458.zip?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fzip%2F714353%2F1627528129205-ce58fbc6-3b7b-4f94-926d-3d09796fe458.zip%22%2C%22name%22%3A%2211_%E9%AB%98%E9%98%B6%E7%BB%84%E4%BB%B6%E7%9A%84%E4%BD%BF%E7%94%A8.zip%22%2C%22size%22%3A4395%2C%22type%22%3A%22application%2Fx-zip-compressed%22%2C%22ext%22%3A%22zip%22%2C%22status%22%3A%22done%22%2C%22taskId%22%3A%22u1657e2db-1142-4bf8-a67f-7e86cd054a5%22%2C%22taskType%22%3A%22upload%22%2C%22id%22%3A%22u66dac779%22%2C%22card%22%3A%22file%22%7D)

定义：类似于高阶函数，传入一个组件，返回一个新组件。
```jsx
function enhanceComponent(WrappedComponent) {
  class NewComponent extends PureComponent {
    render() {
      return <WrappedComponent {...this.props}/>
    }
  }

  NewComponent.displayName = "Kobe";
  return NewComponent;
}
```
应用：

1. 增强props
1. 结合Context，由高阶组件接收Context的参数，子组件中用props接收，避免子组件中引入Context相关代码；
1. 可用于登录鉴权操作；
1. 生命周期劫持：如想在组件中的声明周期里加入一些相同操作时，如测速、计算渲染时间，则可以利用高阶组件减少些代码，即部分组件代码复用。（即类似于函数，用函数做些相同的事情。组件也是第一公民咯）
PS：早期React有提供组件之间的代码复用方式mixin，目前已不建议使用

## 11. 组件内容补充
### forwardRef
通过forwardRef，父组件可获得子组件中的某个真实DOM；之前函数式组件在没有forwardRef时是不能接收ref的；

### Portals
某些情况下，我们希望渲染的内容独立于父组件，甚至是独立于当前挂载到的DOM元素中（默认都是挂载到id为root的DOM元素上的）。
```jsx
class Modal extends PureComponent {
  render() {
    return ReactDOM.createPortal(
      this.props.children,
      document.getElementById("modal")
    )
  }
} // 将会被挂载到id位modal的元素上
```
StrictMode
用来突出显示应用程序中潜在问题的工具：

- 与 Fragment 一样，StrictMode 不会渲染任何可见的 UI；
- 它为其后代元素触发额外的检查和警告；
- 严格模式检查仅在开发模式下运行；它们不会影响生产构建；
```jsx
import React, { PureComponent, StrictMode } from 'react';
export default class App extends PureComponent {
  render() {
    return (
      <div>
        <StrictMode> // 将检查Home及其后代组件，不检查Profile
          <Home/>
        </StrictMode>
        <Profile/>
      </div>
    )
  }
}
```
能检测什么：

- 不安全的声明周期、过时的ref API、废弃的findDOMNode方法、检查意外的副作用（严格模式下会故意调用2次constructor，看是否会产生副作用，生产环境中不会生效）、检测过时context API

