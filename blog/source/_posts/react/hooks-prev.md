---
title: React-hooks
index_img: /img/react/hooks.jpg
tags: [react]
category: 技术
abbrlink: f94fb1be
date: 2020-05-10 17:10:00
---

### 1.useState
这个就太简单了，不多做介绍

### 2.useEffect

函数组件能保存状态，但是对于异步请求，副作用的操作还是无能为力，所以 React 提供了 useEffect 来帮助开发者处理函数组件的副作用

```js
import React, { useState, useEffect } from "react";
import ReactDOM from "react-dom";
let timer = null;

function App() {
  const [count, setCount] = useState(0);
  useEffect(() => {
      document.title = "componentDidMount" + count;
    },[count]);
    
  useEffect(() => {
    timer = setInterval(() => {
      setCount(prevCount => prevCount + 1);
    }, 1000);
    return () => {
      document.title = "componentWillUnmount";
      clearInterval(timer);
    };
  }, []);
    
  return (
    <div>
      Count: {count}
      <button onClick={() => clearInterval(timer)}>clear</button>
    </div>
  );
}
const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```
  1.第一个参数接收一个函数，可以用来做一些副作用比如异步请求，修改外部参数等行为
  2.第二个参数称之为dependencies，是一个数组，如果数组中的值变化才会触发 执行useEffect 第一个参数中的函数
  3.返回值(如果有)则在组件销毁或者调用函数前调用  用来代替componentWillUnmount
  4.第二个参数：
  + 不传递  代表不监听任何参数变化。每次渲染DOM之后，都会执行useEffect中的函数。
  + 传递空数组[] 这种情况下只有在组件初始化或销毁的时候才会触发，用来代替 componentDidMount 和 componentWillUnmount
  + 传递[count] 理解起来就是一旦 count 值发生改变，则修改 documen.title 值； 用来代替componentDidUpdate

### 3.模拟一个 componentDidUpdate
```js
function useUpdate(fn) {
  // useRef 创建一个引用
  const mounting = useRef(true);
  useEffect(() => {
    if (mounting.current) {
      mounting.current = false;
    } else {
      fn();
    }
  });
}
```

### 4.useContext 
> 跨组件传值（依赖createContext）
useContext用来处理多层级传递数据的方式，在以前组件树种，跨层级祖先组件想要给孙子组件传递数据的时候，除了一层层 props 往下透传之外，我们还可以使用 React Context API 来帮我们做这件事

```js
//1.从react中导入useContext和createContext
import React, { useContext,createContext } from "react";
import ReactDOM from "react-dom";

const { Provider, Consumer } = createContext(null);

//2.调用createContext 接受一个初始化参数
const colorContext = createContext({
  color: "blue"
});

function Bar() {
  //3.把colorContext传递给useContext 返回值即是想要透传的数据了
  //传递给 useContext 的是 context 而不是 consumer
  const colors = useContext(colorContext);
  return (
    <Consumer>
      {color => (
        <>
          <div>{color}</div>
          <h2>我是用useContext传过来的值 {colors.color}</h2>
        </>
      )}
    </Consumer>
  );
}

function Foo() {
  return <Bar />;
}

function App() {
  return (
    <Provider value={"grey"}>
      <Foo />
    </Provider>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

### 5.useCallback
> 常用于记忆事件函数，生成记忆后的事件函数并传递给子组件使用
参考链接：https://juejin.im/post/5dd337985188252a1873730f?utm_source=gold_browser_extension

1.作用是：获得一个记忆后的函数
2.第一个参数接收一个函数，且不会执行第一个参数函数，而是将它返回给你
3.第二个参数传入一个数组，数组中的每一项一旦值或者引用发生改变，useCallback 就会重新返回一个新的记忆函数提供给后面进行渲染。
4.这样只要子组件继承了 PureComponent 或者使用 React.memo 就可以有效避免不必要的 VDOM 渲染。
```js
function App() {
  const memoizedHandleClick = useCallback(() => {
    console.log('Click happened')
  }, []); // 空数组代表无论什么情况下该函数都不会发生改变
  return <SomeComponent onClick={memoizedHandleClick}>Click Me</SomeComponent>;
}
```

### 6.useMemo 
> 记忆计算
参考链接：https://juejin.im/post/5dd337985188252a1873730f?utm_source=gold_browser_extension

useCallback 的功能完全可以由 useMemo 所取代，如果你想通过使用 useMemo 返回一个记忆函数也是完全可以的。
useCallback(fn,inputs) <==> useMemo(()=>fn,inputs);

```js
function App(){
	const memoizedHandleClick = useMemo(() => () => {
    	console.log('click happened');
    },[])// 空数组代表无论什么情况下该函数都不会发生改变
    return <SomeComponent onClick={memoizedHandleClick}>Click Me</SomeComponent>;
}
```
useMemo 会执行第一个函数并且将函数执行结果返回给你
useMemo 更适合经过函数计算得到一个确定的值，比如记忆组件。

```js
import React, { useState, useMemo } from "react";
import ReactDOM from "react-dom";

const Counter = props => {
  return <div>{props.count}</div>;
};

function App() {
  const [count, setCount] = useState(0);

  const changeCount = () => {
    setCount(count + 1);
  };

  //useMemo 返回 经过函数计算得到一个确定的值
  const dobuleCount = useMemo(() => count * 2, [count]);

  //useMemo 返回 一个函数
  const changeCount2 = useMemo(() => () => setCount(count + 1), [count]);

  return (
    <div className="App">
      <Counter count={count} />
      <div>dobuleCount:{dobuleCount}</div>
      <button onClick={changeCount}>changeCount:{count}</button>
      <button onClick={changeCount2}>changeCount2:{count}</button>
    </div>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

```js
import React, { useState, useMemo, memo, useCallback } from "react";
import ReactDOM from "react-dom";

const Counter = memo(props => {
  console.log("counter render");
  return <div onClick={props.onClick}>{props.count}</div>;
});

function App() {
  const [count, setCount] = useState(0);

  const changeCount = () => {
    setCount(count + 1);
  };

  //useMemo 返回 经过函数计算得到一个确定的值
  const dobuleCount = useMemo(() => count * 2, [count === 3]);

  //useMemo 返回 一个函数
  const changeCount2 = useMemo(() => () => setCount(count + 1), [count]);

  //userMemo 返回一个 函数 同时传递给子组件，在子组件里面调用
  // const onClick = useMemo(() => {
  //   return () => {
  //     console.log("click");
  //   };
  // }, []);

  //等价于上面
  // const onClick = useCallback(() => {
  //   console.log("click");
  // }, []);

  const onClick = () => {
    console.log("click");
  };

 

  return (
    <div className="App">
      <Counter count={dobuleCount} onClick={onClick} />
      <div>dobuleCount:{dobuleCount}</div>
      <button onClick={changeCount}>changeCount:{count}</button>
      <button onClick={changeCount2}>changeCount2:{count}</button>
    </div>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);

```

> 总结，当需要通过属性给子组件传递函数（方法）时，这个方法就由useCallback或者useMemo来创建

这个例子，每次点击按钮count都发生改变,在Counter组件中通过memo来优化，只有count改变才渲染。
但是由于我们同时也传递了方法给该组件，就会导致每次count改变 所获取的函数事件句柄重新生成了。
这就导致每次count改变 Counter重新生成事件，要避免就在外面传递函数时，通过useCallback或者useMemo来优化。
```js
const onClick = useCallback(() => {
    console.log("click");
}, []);

const onClick = useMemo(() => {
    return () => {
    console.log("click");
    };
}, []);
```
