---
title: "coderwhy_Hook - 基础与扩展"
date: 2020-07-06T00:00:00+08:00
draft: false
tags: ['React', 'coderwhy', '基础']
categories: ['前端']
---

# 基础Hook
## useState
定义：
`function useState<S>(initialState: S | (() => S)): [S, Dispatch<SetStateAction<S>>];`

`type Dispatch<A> = (value: A) => void;`

`type SetStateAction<S> = S | ((prevState: S) => S);`

```jsx
const [demo, setDemo] = useState(()=>10)
const [demo2, setDemo2] = useState(10)

setDemo((preState)=>{
  return preState + 20;
})

setDemo2(demo2+20)
```
## useEffect
```jsx
import React, { useEffect, useState } from 'react'
export default function MultiEffectHookDemo() {
  const [count, setCount] = useState(0);
  const [isLogin, setIsLogin] = useState(true);

  useEffect(() => {
    console.log("修改DOM", count);
  }, [count]);

  useEffect(() => {
    console.log("订阅事件");
  }, []);

  useEffect(() => {
    console.log("网络请求");
  }, []);

  return (
    <div>
      <h2>MultiEffectHookDemo</h2>
      <h2>{count}</h2>
      <button onClick={e => setCount(count + 1)}>+1</button>
      <h2>{isLogin ? "coderwhy": "未登录"}</h2>
      <button onClick={e => setIsLogin(!isLogin)}>登录/注销</button>
    </div>
  )
}
```

## useContect -- 还是没有redux好用
```jsx
// App.js
import React, { useState, createContext } from 'react';
import ContextHookDemo from './04_useContext使用/useContext的使用';
export const UserContext = createContext();
export const ThemeContext = createContext();
export default function App() {
  return (
    <div>
      <UserContext.Provider value={{name: "why", age: 18}}>
        <ThemeContext.Provider value={{fontSize: "30px", color: "red"}}>
          <ContextHookDemo/>
        </ThemeContext.Provider>
      </UserContext.Provider>
    </div>
  )
}

// ContextHookDemo.js
import React, { useContext } from 'react';
import { UserContext, ThemeContext } from "../App";
export default function ContextHookDemo(props) {
  const user = useContext(UserContext); // 这样组件就可以获取到数据了，无需像props一层层传递
  const theme = useContext(ThemeContext);
  console.log(user, theme);
  return (
    <div>
      <h2>ContextHookDemo</h2>
    </div>
  )
}
```
# 扩展Hook
## useReducer
不是redux的替代品，是useState的一种替代方案。即共享reducer这个函数，而不是对state的共享，每个组件的state都是独立的。
```jsx
import React, { useReducer } from "react";

const reducer = (state, action) => {
  switch (action.type) {
    case "increment":
      return { ...state, counter: state.counter + 1 };
    case "decrement":
      return { ...state, counter: state.counter - 1 };
    default:
      return state;
  }
};

export default function ReducerDemo() {
  //   const [count, setCount] = useState(0);
  const [state, dispatch] = useReducer(reducer, { counter: 0 });
  return (
    <div>
      <h2>当前计数：{state.counter}</h2>
      <button onClick={() => dispatch({ type: "increment" })}>+1</button>
      <button onClick={() => dispatch({ type: "decrement" })}>-1</button>
    </div>
  );
}
```

## useCallback
可以进行性能优化。
使用场景：在将一个组件中的函数，传递给子元素进行回调使用时，使用useCallback对函数进行处理
```jsx
import React, { useState, useCallback, memo } from "react";

const Button = memo((props) => {
  console.log("Button", props.btn);
  return <button onClick={props.increment}>+1</button>;
});

export default function UseCallbackDemo() {
  const [count, setCount] = useState(0);
  const [changes, setChanges] = useState(0);

  const increment_1 = () => { // 每次改变changes的值，increment_1都是新的函数，则对于组件Button来所说...
    console.log("increment_1");
    setCount(count + 1);
  };

  const increment_2 = useCallback(() => {
    console.log("increment_2");
    setCount(count + 1);
  }, [count]);
  return (
    <div>
      数值：{count}
      <Button increment={increment_1} btn={1}></Button>
      <Button increment={increment_2} btn={2}></Button>
      <button
        onClick={() => {
          setChanges(changes + 1);
        }}
      >
        {changes}
      </button>
    </div>
  );
}
```
每次改变changes的值，increment_1都是新的函数，则对于组件Button来所说，所传的props.increment是一个新的值，因此memo对其无效，即每次改变changes的值，传递increment_1的Button组件都会重新渲染；而使用了useCallback的increment_2，由于其值只会因count改变时才会生成新的值，因此改变changes时，increment_2与上次对比并没有得到新的函数（被useCallback保留了），因此传递给Button组件的props.increment没有变化，此时memo就会生效，即不会重新渲染Button组件。

## useMemo
实际目的也是为了性能的优化。
场景：复杂计算的结果、子组件传入一个对象时由于其它变量改变导致子组件重新渲染的问题
![](https://s2.loli.net/2022/07/12/u9iXjcHSB4snLlW.png)
第一种：对在父组件中存在的一些复杂计算，父组件每次重新渲染时，这些计算都会被重新运行并计算，观察该代码被哪些变量所影响，进而用useMemo进行包裹，可避免其它变量的变化引起父组件重新渲染导致额外的计算。
```jsx
import React, {useState, useMemo} from 'react';

function calcNumber(count) {
  console.log("calcNumber重新计算");
  let total = 0;
  for (let i = 1; i <= count; i++) {
    total += i;
  }
  return total;
}

export default function MemoHookDemo01() {
  const [count, setCount] = useState(10);
  const [show, setShow] = useState(true);

  // const total = calcNumber(count);
  const total = useMemo(() => { // 用useMemo包裹一个回调，该回调返回一个值，只有count对其有影响
    return calcNumber(count);
  }, [count]);

  return (
    <div>
      <h2>计算数字的和: {total}</h2>
      <button onClick={e => setCount(count + 1)}>+1</button>
      <button onClick={e => setShow(!show)}>show切换</button>
    </div>
  )
}
```
第二种：父组件每次渲染时，对象会被重新创建，即形成新的对象，传入到子组件中，当父组件重新渲染时会引起子组件的重新渲染。此时子组件即使用memo包裹也无济于事，因为memo只能对传入的props进行浅层比较（如传入的是基本数据类型，则有用），当传入的属性是一个对象时（引用数据类型），该对象传入子组件前，可使用useMemo进行包裹优化。
```jsx
import React, { useState, memo, useMemo } from 'react';

const HYInfo = memo((props) => {
  console.log("HYInfo重新渲染");
  return <h2>名字: {props.info.name} 年龄: {props.info.age}</h2>
});

export default function MemoHookDemo02() {
  console.log("MemoHookDemo02重新渲染");
  const [show, setShow] = useState(true);

  // const info = { name: "why", age: 18 }; // 父组件每次渲染，该对象都不是同一对象
  const info = useMemo(() => {
    return { name: "why", age: 18 };
  }, []); // 使用useMemo进行包裹

  return (
    <div>
      <HYInfo info={info} />
      <button onClick={e => setShow(!show)}>show切换</button>
    </div>
  )
}

```
小总结：
在不使用任何优化时，父组件的任何改变都会引起子组件的重新渲染，其中子组件的重新渲染是可以优化的。若传递给子组件的属性是基本数据类型的，则对子组件使用memo包裹（一般写函数式组件都用上）；若传递给子组件的属性是引用数据类型的，或者是一段复杂的JS计算表达式，则对该变量/该表达式使用useMemo包裹。经过以上处理，当其它变量引起父组件变化时，将不会使子组件重新渲染，从而达到优化性能的目的。
传递给子元素的是一个回调函数时，观察该回调函数存在哪些外部变量的影响，若存在则可使用useCallback对其进行优化。当其它变量发生改变时，该回调函数依旧是之前的回调函数，不是新生成的，由此达到性能优化的目的。

## useRef

1. 类组件中通过createRef对某元素进行绑定，函数式组件中可通过useRef对元素进行绑定。

![](https://s2.loli.net/2022/07/12/o8zbRuPD3dHnZKX.png)
对于子组件是函数式组件，不能直接使用ref，函数式子组件使用React.forwardRef进行包裹后，然后将ref传递进来，与子组件中的某个元素进行绑定：
```jsx
import React, { useRef, forwardRef } from 'react';

const HYInput = forwardRef((props, ref) => { // forwardRef包裹，第二个参数是ref
  return <input ref={ref} type="text"/> // ref对元素的绑定
})

export default function ForwardRefDemo() {
  const inputRef = useRef(); // 这里可以拿到值
  return (
    <div>
      <HYInput ref={inputRef}/>
      <button onClick={e => inputRef.current.focus()}>聚焦</button>
    </div>
  )
}
```

2. 可以通过useRef来保存某个state的上一次的值
```jsx
import React, { useRef, useState, useEffect } from 'react'

export default function RefHookDemo02() {
  const [count, setCount] = useState(0); // count初始是0

  const numRef = useRef(count); // numRef初始是0

  useEffect(() => {
    numRef.current = count; // count变化时执行，此时不会numRef的变化不会引起组件重新渲染
  }, [count])

  return (
    <div>
      <h2>count上一次的值: {numRef.current}</h2>
      <h2>count这一次的值: {count}</h2>
      <button onClick={e => setCount(count + 10)}>+10</button>
    </div>
  )
}
```

## useImperativeHandle
结合forwardRef进行使用。对函数式子组件中的元素进行ref绑定，父组件就可以随意操作该ref，但通常其自由度太高，可使用useImperativeHandle对其进行限制：
```jsx
import React, { useRef, forwardRef, useImperativeHandle } from 'react';

const HYInput = forwardRef((props, ref) => {
  const inputRef = useRef();

  useImperativeHandle(ref, () => ({
    focus: () => { // 只需要用到focus这个方法
      inputRef.current.focus();
    }
  }), [inputRef])

  return <input ref={inputRef} type="text"/>
})

export default function UseImperativeHandleHookDemo() {
  const inputRef = useRef();

  return (
    <div>
      <HYInput ref={inputRef}/>
      <button onClick={e => inputRef.current.focus()}>聚焦</button> // 这里只能用focus的方法
    </div>
  )
}

```

## useLayoutEffect
与useEffect只有一点点区别：
useEffect会在渲染的内容更新到DOM上**之后**执行，不会阻塞DOM的更新；
useLayoutEffect会在选额按的内容更新到DOM上**之前**执行，会阻塞DOM更新；
通常使用useEffect，极少情况使用useLayoutEffect。

## 自定义Hook
本质上是一种函数代码逻辑的抽取，不算是React的新特性。可以将相同的代码逻辑抽取到一个函数之中，这段代码逻辑中包含有hook的相关代码，此时就能**以use+函数名的形式**创建一个函数，之后其它函数式组件就能对其进行使用：（**必须以use开头**）
```jsx
import React, { useEffect } from 'react';

const Home = (props) => {
  useLoggingLife("Home");
  return <h2>Home</h2>
}

const Profile = (props) => {
  useLoggingLife("Profile");
  return <h2>Profile</h2>
}

export default function CustomLifeHookDemo01() {
  useLoggingLife("CustomLifeHookDemo01");
  return (
    <div>
      <h2>CustomLifeHookDemo01</h2>
      <Home/>
      <Profile/>
    </div>
  )
}

function useLoggingLife(name) { // 16.13版本实际测试不以use开头也能运行且正常
  useEffect(() => {
    console.log(`${name}组件被创建出来了`);

    return () => {
      console.log(`${name}组件被销毁掉了`);
    }
  }, []);
}
```
案例二：自定义hook结合useContext的使用：
```jsx
// App.js
export const UserContext = createContext();
export const TokenContext = createContext();
export default function App() {
  return (
    <UserContext.Provider value={{name: "why", age: 18}}>
      <TokenContext.Provider value="fdafdafafa">
        <CustomContextShareHook/>
      </TokenContext.Provider>
    </UserContext.Provider>
  ) 
}

// CustomContextShareHook.js
// 原本的写法
import React, { useContext } from 'react';
import { UserContext, TokenContext } from "../App";

export default function CustomContextShareHook() {
  const user = useContext(UserContext); // 其它地方若要使用，也这样引入，会造成许多冗余代码
  const token = useContext(TokenContext); // 可以对这些用自定义hook进行抽取
  console.log(user, token);

  return (
    <div>
      <h2>CustomContextShareHook</h2>
    </div>
  )
}

// 改写后的写法
import React, { useContext } from 'react';
import useUserContext from '../hooks/user-hook';

export default function CustomContextShareHook() {
  const [user, token] = useUserContext(); // 使用的是自定义hook
  console.log(user, token);

  return (
    <div>
      <h2>CustomContextShareHook</h2>
    </div>
  )
}

// useUserContext.js
import { useContext } from "react";
import { UserContext, TokenContext } from "../App";

function useUserContext() {
  const user = useContext(UserContext);
  const token = useContext(TokenContext);

  return [user, token];
}

export default useUserContext;


```
案例三：利用自定义hook抽取滚动位置的代码逻辑
![](https://s2.loli.net/2022/07/12/OReE9JVI1WLAgx3.png)
抽取后，其它组件中若想获取当前滚动位置，则可以直接引入useScrollPosition这个自定义hook函数。

案例四：利用自定义hook抽取localStorage存储数据的代码逻辑
![](https://s2.loli.net/2022/07/12/SkwQzsiZbYnhCjy.png)
抽取到useLocalStorage.js中：
```jsx
import {useState, useEffect} from 'react';

function useLocalStorage(key) { // 自定义hook
  const [name, setName] = useState(() => {
    const name = JSON.parse(window.localStorage.getItem(key));
    return name;
  });

  useEffect(() => {
    window.localStorage.setItem(key, JSON.stringify(name));
  }, [name]);

  return [name, setName];
}

export default useLocalStorage;
```
之后其它组件想利用localStorage读取、存储数据，则可直接使用该自定义hook。

## Fiber原理
早期的React
hook本质上保存的状态就是保存在fiber上的，
# 一篇讲得很好的文章
> 链接：

