---
title: "coderwhy_React中的CSS"
date: 2020-07-06T00:00:00+08:00
draft: false
tags: ['React', 'coderwhy', '基础']
categories: ['前端']
---
相比Vue而言，React官方并没有给出在React中统一的样式风格。
以下介绍四种解决方案：

- 方案一：内联样式的写法；
- 方案二：普通的css写法；
- 方案三：css modules；
- 方案四：css in js（styled-components）；
### 方案一：内联样式
```jsx
render() {
  return (
    <div>
      <h2 style={{color: this.state.titleColor, fontSize: "20px"}}>我是App标题</h2>
      <p style={{color: "green", textDecoration: "underline"}}>我是一段文字描述</p>
    </div>
  )
}
```
内联样式的优点:

- 1.内联样式, 样式之间不会有冲突

- 2.可以动态获取当前state中的状态


内联样式的缺点：

- 1.写法上都需要使用驼峰标识

- 2.某些样式没有提示

- 3.大量的样式, 代码混乱

- 4.某些样式无法编写(比如伪类/伪元素)
### 方案二：普通css
```jsx
render() {
  return (
    <div className="app">
      <h2 className="title">我是App的标题</h2>
      <p className="desc">我是App中的一段文字描述</p>
      <Home/>
    </div>
  )
}
```
缺点：

- 组件化开发中我们总是希望组件是一个独立的模块，即便是样式也只是在自己内部生效，不会相互影响；
- 但是普通的css都属于全局的css，样式之间会相互影响；
### 方案三：css modules
.css/.less/.scss 等样式文件都修改成 .module.css/.module.less/.module.scss 等；之后就可以引用并且进行使用了；
使用方式：
![](https://s2.loli.net/2022/07/12/zXAfOm7FaYkL2be.jpg)
优点：
css modules确实解决了局部作用域的问题，也是很多人喜欢在React中使用的一种方案。
缺点：

- 引用的类名，不能使用连接符(.home-title)，在JavaScript中是不识别的；
- 所有的className都必须使用`{style.className}` 的形式来编写；
- 不方便动态来修改某些样式，依然需要使用内联样式的方式；
### 方案四：CSS in JS
所以，目前可以说CSS-in-JS是React编写CSS最为受欢迎的一种解决方案；
目前比较流行的CSS-in-JS的库有哪些呢？

- styled-components
- emotion
- glamorous

目前可以说styled-components依然是社区最流行的CSS-in-JS库。![](https://s2.loli.net/2022/07/12/FqBJT2XHErw1V3Y.jpg)
### classnames
这是一个用于动态添加classnames的一个库。
```jsx
classNames('foo', 'bar'); // => 'foo bar'
classNames('foo', { bar: true }); // => 'foo bar'
classNames({ 'foo-bar': true }); // => 'foo-bar'
classNames({ 'foo-bar': false }); // => ''
classNames({ foo: true }, { bar: true }); // => 'foo bar'
classNames({ foo: true, bar: true }); // => 'foo bar'

// lots of arguments of various types
classNames('foo', { bar: true, duck: false }, 'baz', { quux: true }); // => 'foo bar baz quux'

// other falsy values are just ignored
classNames(null, false, 'bar', undefined, 0, 1, { baz: null }, ''); // => 'bar 1'
```
