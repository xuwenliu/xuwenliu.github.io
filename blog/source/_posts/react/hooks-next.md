---
title: React-hooks下
index_img: /img/react/hooks.jpg
tags: [react]
category: 技术
abbrlink: f94fb1be
date: 2020-05-11 20:30:00
---

### 1.useRef
> 保存引用值

+ useRef 跟 createRef 类似，都可以用来生成对 DOM 对象的引用。
+ useRef 返回的值传递给组件或者 DOM 的 ref 属性就可以通过 ref.current 值访问组件或真实的 DOM 节点，重点是组件也是可以访问到的，从而可以对 DOM 进行一些操作，比如监听事件等等。
+ useRef 的功能有点像类属性，在组件中记录一些值，并且这些值在稍后可以更改

```js
import React, { useState, useRef } from "react";
import ReactDOM from "react-dom";

import "./styles.css";

function App() {
  let [name, setName] = useState("init");

  let nameRef = useRef();

  //点击按钮把文本框里面的值赋给name
  const submitButton = () => {
    console.log(nameRef.current);
    setName(nameRef.current.value);
  };

  return (
    <div className="App">
      <p>{name}</p>
      <div>
        <input ref={nameRef} type="text" />
        <button type="button" onClick={submitButton}>
          Submit
        </button>
      </div>
    </div>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

```js
import React, { useState, useEffect, useRef } from "react";
import ReactDOM from "react-dom";

function App() {
  const [count, setCount] = useState(0);
  const timer = useRef();

  const changeCount = () => {
    setCount(count + 1);
  };

  useEffect(() => {
    timer.current = setInterval(() => {
      setCount(count => count + 1);
    }, 1000);
  }, []);

  useEffect(() => {
    if (count >= 10) {
      clearInterval(timer.current);
    }
  });

  return (
    <div className="App">
      <button onClick={changeCount}>changeCount:{count}</button>
    </div>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

### 2.useImperativeHandle  
> 透传 Ref 用于让父组件获取子组件内的索引

```js
import React, { useRef, useEffect, useImperativeHandle, forwardRef } from "react";
function ChildInputComponent(props, ref) {
  const inputRef = useRef(null);
  useImperativeHandle(ref, () => inputRef.current);
  return <input type="text" name="child input" ref={inputRef} />;
}
const ChildInput = forwardRef(ChildInputComponent);
function App() {
  const inputRef = useRef(null);
  useEffect(() => {
    inputRef.current.focus();
  }, []);
  return (
    <div>
      <ChildInput ref={inputRef} />
    </div>
  );
}
//通过这种方式，App 组件可以获得子组件的 input 的 DOM 节点
```

### 3.useLayoutEffect  
> 同步执行副作用
+ 大部分情况下，使用 useEffect 就可以帮我们处理组件的副作用，但是如果想要同步调用一些副作用，比如对 DOM 的操作，就需要使用 useLayoutEffect，
+ useLayoutEffect 中的副作用会在 DOM 更新之后同步执行。
+ useLayoutEffect 会在 render，DOM 更新之后同步触发函数，会优于 useEffect 异步触发函数。
+ useLayoutEffect和原来componentDidMount&componentDidUpdate一致，在react完成DOM更新后马上同步调用的代码，会阻塞页面渲染。
而useEffect是会在整个页面渲染完才会调用的代码。
+ 在实际使用时如果想避免页面抖动（在useEffect里修改DOM很有可能出现）的话，可以把需要操作DOM的代码放在useLayoutEffect里
不过useLayoutEffect在服务端渲染时会出现一个warning，要消除的话得用useEffect代替或者推迟渲染时机。

```js
import React, { useLayoutEffect, useState, useEffect } from "react";
import ReactDOM from "react-dom";

function App() {
  const [width, setWidth] = useState(0);
  useLayoutEffect(() => {
    const title = document.querySelector("#title");
    const titleWidth = title.getBoundingClientRect().width;
    console.log("2-useLayoutEffect");
    if (width !== titleWidth) {
      setWidth(titleWidth);
    }
  },[width]);

  useEffect(() => {
    console.log("3-useEffect");
  });

  console.log("1-render");

  return (
    <div>
      <h1 id="title">hello</h1>
      <h2>{width}</h2>
    </div>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

### 4.useReducer 
> 在使用上几乎跟 Redux/React-Redux 一模一样，唯一缺少的就是无法使用 redux 提供的中间件，算是提供一个 mini 的 Redux 版本。
```js
import React, { useReducer } from "react";
const initialState = {
  count: 0
};
function reducer(state, action) {
  switch (action.type) {
    case "add":
      return { count: state.count + action.payload };
    case "reduce":
      return { count: state.count - action.payload };
    default:
      throw new Error();
  }
}
function App() {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({ type: "add", payload: 1 })}>
        +
      </button>
      <button onClick={() => dispatch({ type: "reduce", payload: 1 })}>
        -
      </button>
    </>
  );
}
```