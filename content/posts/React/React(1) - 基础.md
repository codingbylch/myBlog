---
title: "尚硅谷_React(1) - 基础"
date: 2021-08-25T00:00:00+08:00
draft: false
tags: ['React', '尚硅谷', '基础']
categories: ['前端']
---

> 尚硅谷React，基础源码：[react_basic.zip](https://www.yuque.com/attachments/yuque/0/2021/zip/714353/1620025674632-af3a168e-229f-4818-b484-dcc38b088b92.zip?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fzip%2F714353%2F1620025674632-af3a168e-229f-4818-b484-dcc38b088b92.zip%22%2C%22name%22%3A%22react_basic.zip%22%2C%22size%22%3A1008833%2C%22type%22%3A%22application%2Fx-zip-compressed%22%2C%22ext%22%3A%22zip%22%2C%22source%22%3A%22%22%2C%22status%22%3A%22done%22%2C%22mode%22%3A%22title%22%2C%22download%22%3Atrue%2C%22uid%22%3A%221620025674137-0%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22percent%22%3A0%2C%22id%22%3A%22ntp0q%22%2C%22card%22%3A%22file%22%7D)

React是什么？React是用于构建用户界面的JS库，是一个将数据渲染为HTML视图的开源JS库。
![](https://s2.loli.net/2022/07/12/TUN27q8kgC1GVeW.png)
React特点：

- 组件化模式，声明式编码；
- React Native可进行移动端开发；
- 虚拟DOM+Diffing算法，减少与真实DOM交互；

需掌握的基础知识：

- [x] 判断this指向
- [x] class类
- [x] ES6语法规范
- [x] npm包管理器
- [x] 原型、原型链
- [x] 数组常用方法
- [x] 模块化

为什么要使用JSX语法，而不用原始JS语法写呢？**让编码人员更加简单地创建虚拟DOM：**
![](https://s2.loli.net/2022/07/12/2YFUyCKBXrVQ7Nk.png)
![](https://s2.loli.net/2022/07/12/bEXalAJFHYPf2R3.png)
（babel会将JSX语法转化成React.creatElement的方式，编码人员不用操心）

### 关于虚拟DOM：

1. **本质是Object类型的对象；**
1. **虚拟DOM比较“轻”**，真实DOM比较“重”，因为虚拟DOM是React内部在用，无需真实DOM那么多属性；
1. **虚拟DOM最终会被React转化为真实DOM，呈现在页面上**；
### JSX语法规则

1. 定义虚拟DOM时，不要写引号；
1. 标签中混入**JS表达式**要用花括号`{}`；
1. 样式类名不要用class（避开JS关键字），要使用className；
1. 内联样式，形式为`style={{color:'white',fontSize:'29px'}}`
1. 虚拟DOM必须只有一个根标签；
1. 标签首字母：
   1. 若小写字母开头，则将该标签转为html同名元素，若html中无该标签对应的同名元素，则报错；
   1. 若大写字母开头，react将去渲染对应的组件，若组件没有定义则报错；
7. 一定**注意区分：js语句（代码）与js表达式**
   1. **表达式**：一个表达式会产生一个值，可放在任何一个需要值的地方；

下面这些都是表达式：
                (1) `a`
                (2) `a+b`
                (3) `demo(1)`
                (4) `arr.map()  `
                (5) `function test () {}`

   2. **语句(代码)**：

            下面这些都是语句(代码)：
                    (1) `if(){}`
                    (2) `for(){}`
                    (3) `switch(){case:xxxx}`

### 面向组件编程
React组件有两种形式：函数式组件、类式组件
```jsx
// 1.函数式组件 -- 无状态的使用函数式组件
function MyComponent(){
  return <h2>函数定义的组件, 适用于简单组件的定义</h2>
}
ReactDOM.render(<MyComponent/>,document.getElementById('test'));
// 2.类式组件 -- 有状态的使用类组件
class Mycomponent extends React.Component {
  render() {
    return <h2>我是类定义的组件，适用于复杂组件的定义</h2>;
  }
}
```
### （类的实例）组件的三大属性
state、props、ref
#### state、自定义函数的简写形式
```jsx
class Weather extends React.Component {
  constructor(props) {
    super(props);
    this.state = { // state常规写法
      isHot: true,
      wind: "微风",
    };
    // 解决changeWeather中this指向问题
    this.changeWeather = this.changeWeather.bind(this);
  }
  changeWeather() { 
    console.lob('changeWeather');
  }
  render() {
    const { isHot, wind } = this.state;
    return (
      <h1 onClick={this.changeWeather}>
        今天天气很{isHot ? "炎热" : "凉爽"}, {wind}
      </h1>
    );
  }
}

// state的简写形式
class Weather extends React.Component {
  state = {isHot: true, wind: '微风'};
	changeWeather = () => { // 自定义函数：赋值语句 + 箭头函数 的形式
  	console.lob('changeWeather');
	};
  render() {
    const { isHot, wind } = this.state;
    return (
      <h1 onClick={this.changeWeather}>
        今天天气很{isHot ? "炎热" : "凉爽"}, {wind}
      </h1>
    );
	}
}

```
#### props
父子组件传值使用props；`propTypes`可以对props进行属性限制、指定默认标签属性值。
```jsx
import PropTypes from 'prop-types'
class Person extends React.Component {
  render() {
    const { name, age, sex } = this.props;
    // ...
  }
}
// 加上这个属性，React就能查找这个属性，并进行属性限制
Person.propTypes = {
  // 添加了依赖包以后，16的写法
  name: PropTypes.string.isRequired, // 限制name必传，且为字符串
  sex: PropTypes.string, // 限制为字符串
  age: PropTypes.number, // 限制为数值
  speak: PropTypes.func, // 限制为函数
};
// 指定默认标签属性值
Person.defaultProps = {
  sex: "男",
  age: 18,
};
```
扩展：
父子组件通信可用props。若在同一组件树中且为祖孙关系时，可使用[Context](https://zh-hans.reactjs.org/docs/context.html)进行值的传递，从而避免中间组件传递值时的无效props；也可以将祖父组件本身以对象的方式传递给子组件；也可以使用消息发布-订阅的sdk；

`propTypes`简写形式：
```jsx
class Person extends React.Component {
  render() {
    const { name, age, sex } = this.props;
    // ...
  }
  // static 关键字保证变量添加到类的属性上，而不是类的实例属性上
  // 加上这个属性，React就能查找这个属性，并进行属性限制
  static propTypes = {
    name: PropTypes.string.isRequired, // 限制name必传，且为字符串
    sex: PropTypes.string, // 限制为字符串
    age: PropTypes.number, // 限制为数值
  };

  // 指定默认标签属性值
  static defaultProps = {
    sex: "男",
    age: 18,
  };
}
```
#### ref

1. 字符串形式，不推荐
```jsx
<input ref="input1"/>

showData = () => {
  console.log(this.refs.input1.value);
};
```

2. 回调函数形式 ，常用
```jsx
<input ref={(c)=>{this.input1 = c}}/>

showInfo = ()=>{
	const { input1 } = this; // 使用
}
```

3. createRef形式
```jsx
// 1. React.createRef调用后可以返回一个容器，该容器可以存储被ref所标识的节点
myRef1 = React.createRef();

<input ref={this.myRef1}/>

showData = () => {
  console.log(this.myRef1.current.value);
};
```
#### 函数式组件使用props
由于函数式组件没有this，只有参数可传入，因此类式组件的三大属性，它只有props；
```jsx
function Person(props) { // 使用
  const { name, age, sex } = props;
}
```
### 事件处理
(1).通过onXxx属性指定事件处理函数(注意大小写)
a.React使用的是自定义(合成)事件, 而不是使用的原生DOM事件 —————— 为了更好的兼容性
b.React中的事件是通过事件委托方式处理的(委托给组件最外层的元素) ————————为了的高效
(2).通过event.target得到发生事件的DOM元素对象 ——————————不要过度使用ref
发生事件的元素刚好是你要操作的那个元素，就可以使用这种方式，省略ref
```jsx
showData = (e)=>{
	console.log(e.target.value)
}
// ......
<input onClick={this.showData} />
```
### 收集表单数据
非受控组件： 页面中所有输入类的dom，若是现用现取，就是非受控组件
受控组件：随着输入而维护状态state，受state的控制，即受控组件；优势在于：能省略ref
### 高阶函数、函数柯里化
**高阶函数**：如果一个函数符合下面2个规范中的任何一个，那该函数就是高阶函数。
1.若A函数，接收的参数是一个函数，那么A就可以称之为高阶函数。
2.若A函数，调用的返回值依然是一个函数，那么A就可以称之为高阶函数。
常见的高阶函数有：Promise、setTimeout、arr.map()等等
扩展：React中的高阶组件类似于高阶函数的概念。高阶函数满足入参或返回任一是函数，高阶组件满足入参或返回任一是组件，不过通常高阶组件接受一个组件，返回一个新组件。可以利用高阶组件，在其中对组件加入一些东西，如额外的props

**函数的柯里化**：通过函数调用继续返回函数的方式，实现多次接收参数最后统一处理的函数编码形式。
```jsx
function sum(a){
  return(b)=>{
    return (c)=>{
      return a+b+c
    }
  }
}
```

### 组件的生命周期
即React会在合适的时间点帮你调用的函数。
旧的生命周期：
![](https://s2.loli.net/2022/07/12/NevS4tl7pixPOIX.png)
新的生命周期：
![](https://s2.loli.net/2022/07/12/tEIBbaqjwX7Cfrg.png)
1. 初始化阶段: 由ReactDOM.render()触发---初次渲染
    1.  constructor()
    2.  getDerivedStateFromProps 
    3.  render()
    4.  componentDidMount() =====> 常用
                一般在这个钩子中做一些初始化的事，例如：开启定时器、发送网络请求、订阅消息
2. 更新阶段: 由组件内部this.setSate()或父组件重新render触发
    1.  getDerivedStateFromProps
    2.  shouldComponentUpdate()
    3.  render()
    4.  getSnapshotBeforeUpdate
    5.  componentDidUpdate()
3. 卸载组件: 由ReactDOM.unmountComponentAtNode()触发
    1.  componentWillUnmount()  =====> 常用
                一般在这个钩子中做一些收尾的事，例如：关闭定时器、取消订阅消息

getSnapshotBeforeUpdate，在更新前获取快照的生命周期函数，一个例子：“实时更新的新闻列表，往前插入新闻，滚动条不动”

### React插件
simple react snippets
VS Code ES7 React/Redux/React-Native/JS snippets
之后可以快捷生成react相关骨架代码：

- rcc、rce：快速生成类组件
- rpc、rpce：生成继承于PureComponent的类组件
- rcredux：生成一个与redux连接的类组件


- rfc、rfce：快速生成函数式组件
- rfcredux：生成一个与redux连接的函数式组件
- rmc：加了memo

### 补充：
#### 纯函数
一个函数是否是纯函数需满足两点：

1. 对于确定的输入，一定有确定的输出；
1. 对于外部没有副作用，即不会对外部变量进行修改

写纯函数，可以安心调用，不必关心对传入的内容或依赖其它的外部变量进行修改。
**React中要求，所有的React组件都必须像纯函数一样保护它们的props不被更改。**
reducer也被要求是一个纯函数。




















